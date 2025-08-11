# n8n Helm Chart

> ⚠️ This is a **community-maintained** Helm chart and is not officially supported by the n8n team.

## Overview

This Helm chart deploys [n8n](https://n8n.io), a workflow automation platform, on Kubernetes.

### Deployment Modes


| Mode | Description | When to Choose |
|------|-------------|----------------|
| **Single Instance** | One pod running all n8n processes | Development, testing, or very small workloads |
| **Queue Mode** | Separate main (web/API) and worker pods | Production workloads needing scalability, resilience for job execution, or separation of concerns |
| **Multi-Main Queue Mode** | Multiple main pods + multiple workers (requires queue mode) | High-availability production setups with large webhook/API traffic and multiple concurrent editors/users |



> ⚠️ **Important**: Multi-main mode (multiple main pods) is ONLY supported when queue mode is enabled. Never scale main pods >1 in regular mode.

## Prerequisites

### Required
- Kubernetes cluster v1.25+
- Helm 3.x
- PostgreSQL database (external recommended for production)

### Required for Queue/Multi-Main Mode
- Redis instance (external recommended for production)
- Proper `N8N_ENCRYPTION_KEY` configuration

### Optional
- Ingress controller (for external access)
- Cert-manager (for TLS)

## Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/idirouhab/n8n-helm-chart.git
cd n8n-helm-chart
```

### 2. Install the Chart

#### Development (Single Instance)

```bash
helm install n8n ./charts/n8n-app \
  --set database.useExternal=false \
  --set redis.useExternal=false
```

#### Production (Queue Mode with External Dependencies)

```bash
helm install n8n ./charts/n8n-app \
  --set queueMode.enabled=true \
  --set queueMode.workerReplicaCount=2 \
  --set database.useExternal=true \
  --set database.host=postgres.example.com \
  --set database.passwordSecret.name=postgres-secret \
  --set redis.useExternal=true \
  --set redis.host=redis.example.com \
  --set redis.passwordSecret.name=redis-secret \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=n8n.example.com
```

#### High Availability (Multi-Main + Queue Mode)

```bash
helm install n8n ./charts/n8n-app \
  --set queueMode.enabled=true \
  --set multiMain.enabled=true \
  --set multiMain.replicas=3 \
  --set queueMode.workerReplicaCount=4 \
  --set database.useExternal=true \
  --set database.host=postgres.example.com \
  --set redis.useExternal=true \
  --set redis.host=redis.example.com \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=n8n.example.com
```

## Configuration


### Core Settings

| Parameter          | Description                                             | Default     |
| ------------------ | ------------------------------------------------------- | ----------- |
| `replicaCount`     | Number of main pods (ignored if multiMain.enabled=true) | `1`         |
| `image.repository` | n8n image repository                                    | `n8nio/n8n` |
| `image.tag`        | n8n image version                                       | `latest`    |
| `nameOverride`     | Override chart name                                     | `""`        |
| `fullnameOverride` | Override full deployment name                           | `""`        |

### Queue Mode

| Parameter                      | Description                                 | Default |
| ------------------------------ | ------------------------------------------- | ------- |
| `queueMode.enabled`            | Enable queue mode (required for multi-main) | `false` |
| `queueMode.workerReplicaCount` | Number of worker pods                       | `2`     |

### Multi-Main Mode

| Parameter            | Description                                    | Default |
| -------------------- | ---------------------------------------------- | ------- |
| `multiMain.enabled`  | Enable multiple main pods (requires queueMode) | `false` |
| `multiMain.replicas` | Number of main pods when enabled               | `2`     |

### Database Configuration

| Parameter                      | Description                    | Default                                    |
| ------------------------------ | ------------------------------ | ------------------------------------------ |
| `database.useExternal`         | Use external PostgreSQL        | `true`                                     |
| `database.type`                | Database type                  | `postgresdb`                               |
| `database.host`                | PostgreSQL host                | `""` *(must be set if `useExternal=true`)* |
| `database.port`                | PostgreSQL port                | `5432`                                     |
| `database.database`            | Database name                  | `n8n`                                      |
| `database.user`                | Database user                  | `n8n`                                      |
| `database.passwordSecret.name` | K8s secret containing password | `""`                                       |
| `database.passwordSecret.key`  | Key in secret for password     | `password`                                 |

### Redis Configuration

| Parameter                   | Description                    | Default                                    |
| --------------------------- | ------------------------------ | ------------------------------------------ |
| `redis.useExternal`         | Use external Redis             | `true`                                     |
| `redis.host`                | Redis host                     | `""` *(must be set if `useExternal=true`)* |
| `redis.port`                | Redis port                     | `6379`                                     |
| `redis.passwordSecret.name` | K8s secret containing password | `""`                                       |
| `redis.passwordSecret.key`  | Key in secret for password     | `password`                                 |


### Session Affinity (for Multi-Main)

Choose ONE of the following approaches:

#### Option 1: Ingress-Level (Recommended)

```yaml
ingress:
  annotations:
    nginx.ingress.kubernetes.io/affinity: "cookie"
    nginx.ingress.kubernetes.io/session-cookie-name: "n8n-session"
    nginx.ingress.kubernetes.io/session-cookie-max-age: "86400"
```

#### Option 2: Service-Level

```yaml
service:
  sessionAffinity:
    enabled: true
    type: ClientIP
    timeoutSeconds: 10800
```

### Persistence

| Parameter | Description | Default |
|-----------|-------------|---------|
| `persistence.enabled` | Enable persistent volume | `true` |
| `persistence.size` | Volume size | `10Gi` |
| `persistence.storageClass` | Storage class | `""` |

### Autoscaling

| Parameter | Description | Default |
|-----------|-------------|---------|
| `hpa.enabled` | Enable HPA for main pods | `false` |
| `hpa.minReplicas` | Minimum replicas | `2` |
| `hpa.maxReplicas` | Maximum replicas | `10` |
| `hpa.targetCPUUtilizationPercentage` | Target CPU % | `70` |

## Advanced Configuration

### Custom Environment Variables

```yaml
# values.yaml
env:
  - name: N8N_ENCRYPTION_KEY
    valueFrom:
      secretKeyRef:
        name: n8n-secrets
        key: encryption-key
  - name: WEBHOOK_URL
    value: "https://n8n.example.com"
  - name: N8N_PROTOCOL
    value: "https"
  - name: N8N_HOST
    value: "n8n.example.com"
```

### Resource Limits

```yaml
resources:
  main:
    requests:
      memory: "512Mi"
      cpu: "500m"
    limits:
      memory: "2Gi"
      cpu: "2000m"
  worker:
    requests:
      memory: "256Mi"
      cpu: "250m"
    limits:
      memory: "1Gi"
      cpu: "1000m"
```

### Network Policies

```yaml
networkPolicy:
  enabled: true
  ingress:
    - from:
      - namespaceSelector:
          matchLabels:
            name: ingress-nginx
```

## Deployment Patterns

### Pattern 1: Development

- Single instance mode
- In-cluster PostgreSQL and Redis
- No ingress (port-forward for access)
- Minimal resources

### Pattern 2: Production - Basic

- Queue mode enabled
- 1 main pod, 2-3 worker pods
- External PostgreSQL and Redis
- Ingress with TLS
- Persistence enabled

### Pattern 3: Production - High Availability

- Multi-main mode with queue
- 2-3 main pods, 4-6 worker pods
- External PostgreSQL with connection pooling
- External Redis cluster
- Ingress with session affinity
- HPA enabled
- PodDisruptionBudget configured

## Operations

### Verify Deployment

```bash
# Check pods
kubectl get pods -l app.kubernetes.io/name=n8n

# Check services
kubectl get svc -l app.kubernetes.io/name=n8n

# View logs
kubectl logs -f deployment/n8n-main
kubectl logs -f deployment/n8n-worker
```

### Access n8n

#### With Ingress
Navigate to your configured host (e.g., https://n8n.example.com)

#### Without Ingress (Port Forward)
```bash
kubectl port-forward svc/n8n 5678:5678
# Access at http://localhost:5678
```

### Scaling

```bash
# Scale workers
kubectl scale deployment n8n-worker --replicas=6

# Scale main pods (only in multi-main mode)
kubectl scale deployment n8n-main --replicas=3
```

### Upgrade

```bash
# Update chart values
helm upgrade n8n ./charts/n8n-app \
  --reuse-values \
  --set image.tag=new-version
```

### Backup

```bash
# Backup database
kubectl exec -it postgres-pod -- pg_dump n8n > n8n-backup.sql

# Backup persistent volume
kubectl cp n8n-pod:/home/node/.n8n ./n8n-backup
```

## Troubleshooting

### Common Issues

#### Main pods crash-looping
- Check `N8N_ENCRYPTION_KEY` is set and consistent across all pods
- Verify database connectivity
- Check Redis connectivity (if queue mode enabled)

#### Webhooks not working
- Ensure `WEBHOOK_URL` is set to public URL
- Verify ingress configuration
- Check that all main pods can receive webhook requests

#### Session issues in multi-main mode
- Enable session affinity (see Session Affinity section)
- Verify sticky session configuration at ingress

#### Workers not processing jobs
- Check Redis connectivity
- Verify queue mode is enabled
- Check worker logs for errors

### Debug Commands

```bash
# Get pod details
kubectl describe pod n8n-main-xxxxx

# Check environment variables
kubectl exec n8n-main-xxxxx -- env | grep N8N

# Test database connection
kubectl exec n8n-main-xxxxx -- nc -zv postgres-host 5432

# Test Redis connection
kubectl exec n8n-main-xxxxx -- nc -zv redis-host 6379
```

## Security Considerations

1. **Always use external secrets** for sensitive data
2. **Enable NetworkPolicies** in production
3. **Use TLS** for ingress
4. **Rotate encryption keys** regularly
5. **Implement RBAC** for service accounts
6. **Use PodSecurityPolicies** or Pod Security Standards
7. **Enable audit logging** for compliance

## Uninstall

```bash
# Remove the release
helm uninstall n8n

# Clean up PVCs (data will be lost!)
kubectl delete pvc -l app.kubernetes.io/name=n8n
```

## Support

- **Chart Issues**: [GitHub Issues](https://github.com/idirouhab/n8n-helm-chart/issues)
- **n8n Documentation**: [https://docs.n8n.io](https://docs.n8n.io)
- **n8n Community**: [https://community.n8n.io](https://community.n8n.io)

## License

This Helm chart is provided under the Apache 2.0 License. n8n itself is distributed under the [Sustainable Use License](https://github.com/n8n-io/n8n/blob/master/LICENSE.md).

## Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request

For major changes, please open an issue first to discuss the proposed changes.