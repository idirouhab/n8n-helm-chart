# n8n Helm Chart

> ⚠️ This is a **community-maintained** Helm chart and is not officially supported by the n8n team.

## Quick Start

New to this chart? Start with our examples:

```bash
# 1. Create required secrets
./examples/create-secrets.sh

# 2. Choose and customize an example
cp examples/production-aws.yaml my-values.yaml

# 3. Deploy
helm install n8n ./charts/n8n-app -f my-values.yaml
```

[See all available examples](./examples/)

## Overview

This Helm chart deploys [n8n](https://n8n.io), a workflow automation platform, on Kubernetes.

### Deployment Modes

This chart **only supports queue mode** with external PostgreSQL and Redis dependencies for better scalability and production readiness.

| Mode | Description | When to Choose |
|------|-------------|----------------|
| **Queue Mode** | Separate main (web/API) and worker pods | Default mode - suitable for all workloads, provides scalability and resilience |
| **Multi-Main Queue Mode** | Multiple main pods + multiple workers | High-availability production setups with large webhook/API traffic and multiple concurrent editors/users |
| **Webhook Processor Mode** | Dedicated webhook processing pods + main + workers | Ultra high-throughput webhook scenarios requiring massive parallel webhook processing |

Note: This chart enforces external PostgreSQL and Redis for all deployments to ensure production-ready configurations.

## Prerequisites

### Required
- Kubernetes cluster v1.25+
- Helm 3.x
- PostgreSQL database (external, required)
- Redis instance (external, required)
- Kubernetes secrets (see [Secrets Management](#secrets-management))

### Optional
- Ingress controller (for external access)  
- Cert-manager (for TLS)

## Webhook Processors Important Notice

If using webhook processors with `disableProductionWebhooksOnMainProcess: true`, you **MUST** configure your load balancer/ingress to route traffic correctly:

```
/webhook/*      → webhook processor service (production webhooks)
/webhook-test/* → main service             (test webhooks)
/*              → main service             (UI, API, everything else)
```

**Without proper routing, production webhooks will fail!** See [`examples/webhook-processors-guide.md`](./examples/webhook-processors-guide.md) for complete setup instructions.

## Secrets Management

The chart requires two secrets to be created before installation:

### Required Secrets

**Database Password**:
```bash
kubectl create secret generic n8n-db-password \
  --from-literal=password="your-postgres-password"
```

**n8n Configuration**:
```bash
kubectl create secret generic n8n-core-secrets \
  --from-literal=N8N_ENCRYPTION_KEY="$(openssl rand -base64 32)" \
  --from-literal=N8N_HOST="n8n.example.com" \
  --from-literal=N8N_PORT="5678" \
  --from-literal=N8N_PROTOCOL="https"
```

### Quick Setup Script

Use the provided script to create all required secrets:

```bash
# Edit the script with your values
nano scripts/create-secrets.sh

# Run it
./scripts/create-secrets.sh
```

> **Important**: Save the generated `N8N_ENCRYPTION_KEY` securely - you'll need it for backups!

## Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/idirouhab/n8n-helm-chart.git
cd n8n-helm-chart
```

### 2. Create Required Secrets

**Before installing the chart**, create the required secrets:

```bash
# Method 1: Use the provided script (recommended)
./scripts/create-secrets.sh

# Method 2: Create manually
kubectl create secret generic n8n-db-password --from-literal=password="your-db-password"
kubectl create secret generic n8n-core-secrets \
  --from-literal=N8N_ENCRYPTION_KEY="$(openssl rand -base64 32)" \
  --from-literal=N8N_HOST="n8n.example.com" \
  --from-literal=N8N_PORT="5678" \
  --from-literal=N8N_PROTOCOL="https"
```

### 3. Install the Chart

#### Basic Queue Mode (Default)

```bash
helm install n8n ./charts/n8n-app \
  --set database.host=postgres.example.com \
  --set database.passwordSecret.name=n8n-db-password \
  --set redis.host=redis.example.com \
  --set secretRefs.existingSecret=n8n-core-secrets \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=n8n.example.com
```

#### Custom Namespace Installation

```bash
# Create secrets in custom namespace
./scripts/create-secrets.sh n8n-production

# Install in custom namespace
helm install n8n ./charts/n8n-app \
  --namespace n8n-production \
  --create-namespace \
  --set database.host=postgres.example.com \
  --set database.passwordSecret.name=n8n-db-password \
  --set redis.host=redis.example.com \
  --set secretRefs.existingSecret=n8n-core-secrets \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=n8n.example.com
```

#### Development Environment

Use the provided development configuration:

```bash
helm install n8n ./charts/n8n-app -f values-dev.yaml
```

#### High Availability (Multi-Main Mode)

```bash
helm install n8n ./charts/n8n-app \
  --set multiMain.enabled=true \
  --set multiMain.replicas=3 \
  --set queueMode.workerReplicaCount=4 \
  --set database.host=postgres.example.com \
  --set database.passwordSecret.name=n8n-db-password \
  --set redis.host=redis.example.com \
  --set secretRefs.existingSecret=n8n-core-secrets \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=n8n.example.com
```

#### Ultra High-Throughput (Webhook Processor Mode)

```bash
helm install n8n ./charts/n8n-app \
  --set webhookProcessor.enabled=true \
  --set webhookProcessor.replicaCount=5 \
  --set queueMode.workerReplicaCount=10 \
  --set hpa.webhookProcessor.enabled=true \
  --set hpa.webhookProcessor.maxReplicas=100 \
  --set database.host=postgres.example.com \
  --set database.passwordSecret.name=n8n-db-password \
  --set redis.host=redis.example.com \
  --set secretRefs.existingSecret=n8n-core-secrets \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=n8n.example.com
```

## New Features

### Enterprise Features
- **License Management**: Support for n8n Enterprise licenses via secrets or direct configuration
- **Multi-Main Setup**: Advanced leader election and coordination for multiple main instances

### Storage & Scalability  
- **S3 External Storage**: Full S3 integration for binary data with configurable modes
- **Advanced Redis Configuration**: Comprehensive Redis setup with cluster support, authentication, and fine-tuned worker settings
- **Webhook Configuration**: Enhanced webhook support with custom URLs, timeouts, and testing endpoints

### Execution Management
- **Execution Control**: Configurable timeouts, data retention, and concurrency limits
- **Data Pruning**: Automatic cleanup with configurable retention policies
- **Worker Readiness**: Custom health checks for worker instances

### Configuration Files
- **Generic Values**: Clean `values.yaml` for easy customization
- **Development Config**: Pre-configured `values-dev.yaml` for development environments

## Configuration


### Core Settings

| Parameter          | Description                                             | Default     |
| ------------------ | ------------------------------------------------------- | ----------- |
| `replicaCount`     | Number of main pods (when multiMain.enabled=false)     | `1`         |
| `image.repository` | n8n image repository                                    | `n8nio/n8n` |
| `image.tag`        | n8n image version                                       | `1.110.1`   |
| `nameOverride`     | Override chart name                                     | `""`        |
| `fullnameOverride` | Override full deployment name                           | `""`        |

### Queue Mode (Always Enabled)

| Parameter                      | Description                                 | Default |
| ------------------------------ | ------------------------------------------- | ------- |
| `queueMode.workerReplicaCount` | Number of worker pods                       | `2`     |

### Multi-Main Mode

| Parameter            | Description                                    | Default |
| -------------------- | ---------------------------------------------- | ------- |
| `multiMain.enabled`  | Enable multiple main pods (optional)          | `false` |
| `multiMain.replicas` | Number of main pods when enabled               | `2`     |

### Webhook Processor Mode

| Parameter            | Description                                    | Default |
| -------------------- | ---------------------------------------------- | ------- |
| `webhookProcessor.enabled`  | Enable dedicated webhook processing pods | `false` |
| `webhookProcessor.replicaCount` | Number of webhook processor pods | `2`     |
| `webhookProcessor.disableProductionWebhooksOnMainProcess` | Disable webhook processing in main pods | `true`     |

### Database Configuration (Required)

| Parameter                      | Description                    | Default                                    |
| ------------------------------ | ------------------------------ | ------------------------------------------ |
| `database.useExternal`         | Use external PostgreSQL (always true) | `true`                             |
| `database.type`                | Database type                  | `postgres`                                 |
| `database.host`                | PostgreSQL host (required)     | `""`                                       |
| `database.port`                | PostgreSQL port                | `5432`                                     |
| `database.database`            | Database name                  | `n8n`                                      |
| `database.user`                | Database user                  | `n8n`                                      |
| `database.passwordSecret.name` | K8s secret containing password | `"n8n-db-password"`                        |
| `database.passwordSecret.key`  | Key in secret for password     | `password`                                 |

### Redis Configuration (Required)

| Parameter                   | Description                    | Default                                    |
| --------------------------- | ------------------------------ | ------------------------------------------ |
| `redis.enabled`             | Enable Redis (always true)    | `true`                                     |
| `redis.useExternal`         | Use external Redis (always true) | `true`                                  |
| `redis.host`                | Redis host (required)          | `""`                                       |
| `redis.port`                | Redis port                     | `6379`                                     |
| `redis.passwordSecret.name` | K8s secret containing password | `null`                                     |
| `redis.passwordSecret.key`  | Key in secret for password     | `password`                                 |

### S3 Storage Configuration

| Parameter                          | Description                           | Default      |
| ---------------------------------- | ------------------------------------- | ------------ |
| `s3.enabled`                       | Enable S3 external storage           | `false`      |
| `s3.bucket.name`                   | S3 bucket name                        | `""`         |
| `s3.bucket.region`                 | S3 region                             | `""`         |
| `s3.bucket.host`                   | S3 endpoint (for S3-compatible)       | `""`         |
| `s3.auth.autoDetect`               | Use AWS credential provider chain (requires `serviceAccount.awsRoleArn`) | `false`      |
| `s3.auth.accessKeyId`              | S3 access key ID                      | `""`         |
| `s3.auth.secretAccessKeySecret.name` | K8s secret containing secret key    | `""`         |
| `s3.auth.secretAccessKeySecret.key`  | Key in secret for secret key        | `""`         |
| `s3.storage.mode`                  | Binary data storage mode              | `filesystem` |
| `s3.storage.availableModes`        | Available storage modes               | `filesystem` |

#### S3 Secrets Setup

```bash
# Create S3 credentials secret
kubectl create secret generic s3-credentials \
  --from-literal=aws-access-key-id=AKIAIOSFODNN7EXAMPLE \
  --from-literal=aws-secret-access-key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

# Use in values:
s3:
  enabled: true
  bucket:
    name: "your-bucket"
    region: "us-east-1"
  auth:
    accessKeyId: "AKIAIOSFODNN7EXAMPLE"
    secretAccessKeySecret:
      name: "s3-credentials"
      key: "aws-secret-access-key"
```

#### AWS IRSA Setup (Recommended for EKS)

For AWS EKS clusters, use IAM Roles for Service Accounts (IRSA) instead of access keys:

```bash
# 1. Create IAM policy for S3 access
aws iam create-policy \
  --policy-name n8n-s3-policy \
  --policy-document '{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "s3:GetObject",
          "s3:PutObject",
          "s3:DeleteObject"
        ],
        "Resource": "arn:aws:s3:::your-bucket/*"
      }
    ]
  }'

# 2. Create IAM role with OIDC trust relationship
aws iam create-role \
  --role-name n8n-s3-role \
  --assume-role-policy-document '{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Principal": {
          "Federated": "arn:aws:iam::ACCOUNT-ID:oidc-provider/oidc.eks.REGION.amazonaws.com/id/CLUSTER-ID"
        },
        "Action": "sts:AssumeRoleWithWebIdentity",
        "Condition": {
          "StringEquals": {
            "oidc.eks.REGION.amazonaws.com/id/CLUSTER-ID:sub": "system:serviceaccount:NAMESPACE:n8n",
            "oidc.eks.REGION.amazonaws.com/id/CLUSTER-ID:aud": "sts.amazonaws.com"
          }
        }
      }
    ]
  }'

# 3. Attach policy to role
aws iam attach-role-policy \
  --role-arn arn:aws:iam::ACCOUNT-ID:role/n8n-s3-role \
  --policy-arn arn:aws:iam::ACCOUNT-ID:policy/n8n-s3-policy

# 4. Use in Helm values:
s3:
  enabled: true
  bucket:
    name: "your-bucket"
    region: "us-east-1"
  auth:
    autoDetect: true  # <-- Enable auto-detection
serviceAccount:
  awsRoleArn: "arn:aws:iam::ACCOUNT-ID:role/n8n-s3-role"  # <-- IAM role ARN
```

**Environment Variables Set:**
When S3 is configured, the following environment variables are automatically set:
- `N8N_EXTERNAL_STORAGE_S3_HOST` (if host is specified)
- `N8N_EXTERNAL_STORAGE_S3_BUCKET_NAME`
- `N8N_EXTERNAL_STORAGE_S3_BUCKET_REGION`
- `N8N_EXTERNAL_STORAGE_S3_ACCESS_KEY`
- `N8N_EXTERNAL_STORAGE_S3_ACCESS_SECRET`
- `AWS_ACCESS_KEY_ID` (for broader AWS SDK compatibility)
- `AWS_SECRET_ACCESS_KEY` (for broader AWS SDK compatibility)
- `N8N_DEFAULT_BINARY_DATA_MODE`
- `N8N_AVAILABLE_BINARY_DATA_MODES`

### Service Account Configuration

| Parameter                          | Description                           | Default      |
| ---------------------------------- | ------------------------------------- | ------------ |
| `serviceAccount.create`            | Create service account                | `true`       |
| `serviceAccount.name`              | Service account name                  | `n8n`        |
| `serviceAccount.awsRoleArn`        | AWS IAM Role ARN for IRSA             | `""`         |
| `serviceAccount.annotations`       | Additional service account annotations | `{}`         |
| `rbac.create`                      | Create RBAC resources                 | `true`       |

The service account is configured with minimal required permissions:
- **ConfigMaps**: `get`, `list` - For reading timezone and configuration
- **Secrets**: `get`, `list` - For reading database passwords and encryption keys

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
| `hpa.main.enabled` | Enable HPA for main pods (UI/API) | `false` |
| `hpa.main.minReplicas` | Main minimum replicas | `1` |
| `hpa.main.maxReplicas` | Main maximum replicas | `5` |
| `hpa.main.targetCPUUtilizationPercentage` | Main target CPU % | `80` |
| `hpa.worker.enabled` | Enable HPA for worker pods (execution) | `false` |
| `hpa.worker.minReplicas` | Worker minimum replicas | `2` |
| `hpa.worker.maxReplicas` | Worker maximum replicas | `20` |
| `hpa.worker.targetCPUUtilizationPercentage` | Worker target CPU % | `70` |
| `hpa.webhookProcessor.enabled` | Enable HPA for webhook processor pods | `false` |
| `hpa.webhookProcessor.minReplicas` | Webhook processor minimum replicas | `2` |
| `hpa.webhookProcessor.maxReplicas` | Webhook processor maximum replicas | `50` |
| `hpa.webhookProcessor.targetCPUUtilizationPercentage` | Webhook processor target CPU % | `60` |

## Advanced Configuration

### Environment Variables

The chart supports all n8n environment variables through the `config.extraEnv` section in values.yaml. This provides a flexible way to configure any n8n setting.

#### Basic Example

```yaml
# values.yaml
config:
  extraEnv:
    - name: N8N_EDITOR_BASE_URL
      value: "https://n8n.example.com"
    - name: HTTP_PROXY
      value: "http://proxy.example.com:8080"
    - name: N8N_DISABLE_UI
      value: "false"
```

#### Supported Environment Variables

**Proxy Configuration**
- `HTTP_PROXY` - Proxy URL for unencrypted HTTP requests
- `HTTPS_PROXY` - Proxy URL for encrypted HTTPS requests  
- `ALL_PROXY` - Fallback proxy URL for all requests
- `NO_PROXY` - Comma-separated list of hosts to bypass proxy

> **Note**: The proxy-from-env package used by n8n gives precedence to lowercase versions (e.g., `http_proxy` over `HTTP_PROXY`)

**Core Configuration**
- `N8N_EDITOR_BASE_URL` - Public URL where users access the editor
- `N8N_CONFIG_FILES` - Path to JSON configuration file
- `N8N_USER_FOLDER` - Path for n8n user data folder (default: `/home/node/.n8n`)
- `N8N_PATH` - Path where n8n is deployed (default: `/`)

**Feature Toggles**
- `N8N_DISABLE_UI` - Set to `true` to disable the n8n UI
- `N8N_PREVIEW_MODE` - Set to `true` to run in preview mode
- `N8N_TEMPLATES_ENABLED` - Enable workflow templates (`true`/`false`)
- `N8N_TEMPLATES_HOST` - Custom workflow templates API host
- `N8N_PERSONALIZATION_ENABLED` - Show personalization questions (default: `true`)
- `N8N_HIRING_BANNER_ENABLED` - Show hiring banner in console (default: `true`)

**Network Configuration**
- `N8N_HOST` - Host name n8n runs on (default: `localhost`)
- `N8N_PORT` - HTTP port n8n runs on (default: `5678`)
- `N8N_LISTEN_ADDRESS` - IP address to listen on (default: `::`)
- `N8N_PROTOCOL` - Protocol: `http` or `https` (default: `http`)
- `N8N_SSL_KEY` - SSL private key for HTTPS
- `N8N_SSL_CERT` - SSL certificate for HTTPS
- `N8N_PROXY_HOPS` - Number of reverse-proxies n8n is behind (default: `0`)

**Telemetry & Notifications**
- `N8N_DIAGNOSTICS_ENABLED` - Share anonymous telemetry (default: `true`)
- `N8N_DIAGNOSTICS_CONFIG_FRONTEND` - Frontend telemetry configuration
- `N8N_DIAGNOSTICS_CONFIG_BACKEND` - Backend telemetry configuration
- `N8N_VERSION_NOTIFICATIONS_ENABLED` - Show version notifications (default: `true`)
- `N8N_VERSION_NOTIFICATIONS_ENDPOINT` - Version check endpoint
- `N8N_VERSION_NOTIFICATIONS_INFO_URL` - Update information URL

**API Configuration**
- `N8N_PUBLIC_API_DISABLED` - Disable public API (default: `false`)
- `N8N_PUBLIC_API_ENDPOINT` - Public API path (default: `api`)
- `N8N_PUBLIC_API_SWAGGERUI_DISABLED` - Disable Swagger UI (default: `false`)
- `N8N_PUSH_BACKEND` - Backend push method: `websocket` or `sse` (default: `websocket`)

**System Configuration**
- `N8N_GRACEFUL_SHUTDOWN_TIMEOUT` - Shutdown timeout in seconds (default: `30`)
- `N8N_ENCRYPTION_KEY` - Custom encryption key for credentials

**Development Settings**
- `N8N_DEV_RELOAD` - Auto-reload on code changes (default: `false`)
- `N8N_REINSTALL_MISSING_PACKAGES` - Auto-reinstall missing packages (default: `false`)
- `N8N_TUNNEL_SUBDOMAIN` - Custom subdomain for n8n tunnel

**Frontend Build (Advanced)**
- `VUE_APP_URL_BASE_API` - Frontend API base URL for manual builds

#### Complete Configuration Example

```yaml
# values.yaml
config:
  extraEnv:
    # Proxy configuration
    - name: HTTP_PROXY
      value: "http://proxy.example.com:8080"
    - name: HTTPS_PROXY
      value: "https://secure-proxy.example.com:8443"
    - name: NO_PROXY
      value: "localhost,127.0.0.1,.example.com"
    
    # Core settings
    - name: N8N_EDITOR_BASE_URL
      value: "https://n8n.example.com"
    - name: N8N_USER_FOLDER
      value: "/home/node/.n8n"
    - name: N8N_PATH
      value: "/"
    
    # Feature toggles
    - name: N8N_TEMPLATES_ENABLED
      value: "true"
    - name: N8N_PERSONALIZATION_ENABLED
      value: "true"
    - name: N8N_HIRING_BANNER_ENABLED
      value: "false"
    
    # Network configuration
    - name: N8N_LISTEN_ADDRESS
      value: "::"
    - name: N8N_PROXY_HOPS
      value: "1"
    
    # API settings
    - name: N8N_PUBLIC_API_DISABLED
      value: "false"
    - name: N8N_PUBLIC_API_ENDPOINT
      value: "api"
    - name: N8N_PUSH_BACKEND
      value: "websocket"
    
    # System settings
    - name: N8N_GRACEFUL_SHUTDOWN_TIMEOUT
      value: "30"
    
    # Development (only for dev environments)
    - name: N8N_DEV_RELOAD
      value: "false"
```

#### Using Secrets for Sensitive Data

```yaml
config:
  extraEnv:
    - name: N8N_EDITOR_BASE_URL
      value: "https://n8n.example.com"
    - name: CUSTOM_SECRET
      valueFrom:
        secretKeyRef:
          name: n8n-secrets
          key: custom-key
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

- Queue mode (default)
- 1 main pod, 1-2 worker pods
- External PostgreSQL and Redis (lightweight instances)
- Port-forward or ingress for access
- Minimal resources

### Pattern 2: Production - Standard

- Queue mode (default)
- 1 main pod, 2-4 worker pods
- External PostgreSQL and Redis with high availability
- Ingress with TLS
- Persistence enabled
- Resource limits configured

### Pattern 3: Production - High Availability

- Multi-main mode with queue
- 2-3 main pods, 4-6 worker pods
- External PostgreSQL with connection pooling
- External Redis cluster
- Ingress with session affinity
- HPA enabled
- PodDisruptionBudget configured

### Pattern 4: Ultra High-Throughput Webhooks

- Dedicated webhook processors + queue mode
- 1-2 main pods (UI only), 5-20 webhook processors, 10-50 workers
- Webhook processors handle all webhook traffic
- Load balancer routes `/webhook/*` to webhook processors
- All other traffic routes to main pods
- Aggressive HPA scaling for webhook processors
- External PostgreSQL and Redis clusters

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

#### Pod crash-looping
- Ensure secrets are created before deployment
- Check database and Redis connectivity
- Verify `N8N_ENCRYPTION_KEY` is set correctly

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
# Check pods status
kubectl get pods -l app.kubernetes.io/name=n8n

# View pod logs
kubectl logs -f deployment/n8n-main

# Check secrets exist
kubectl get secrets | grep n8n
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