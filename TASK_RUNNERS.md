# Task Runner Support in n8n Helm Chart

This document describes the task runner support implementation in the n8n Helm chart.

## Overview

Task runners are a mechanism to execute user-provided JavaScript and Python code in the Code node in a secure and performant way. This Helm chart implements task runners using **external mode with sidecar containers**.

## Architecture

When task runners are enabled, the chart deploys:

1. **n8n Main/Worker Pods** with:
   - Primary n8n container (acts as task broker)
   - Sidecar task runner container (`n8nio/runners` image)

2. **Task Runner Secret** containing the authentication token

3. **Environment Configuration** via ConfigMap

### Communication Flow

```
┌─────────────────────────────────────────┐
│           Pod (Main/Worker)             │
│                                         │
│  ┌──────────────┐    ┌──────────────┐  │
│  │ n8n          │◄──►│ task-runner  │  │
│  │ (Broker)     │    │ (Sidecar)    │  │
│  │ Port: 5679   │    │              │  │
│  └──────────────┘    └──────────────┘  │
│         │                    │          │
└─────────┼────────────────────┼──────────┘
          │                    │
          ▼                    ▼
     Task Request         Task Execution
     Task Response        (JS/Python Code)
```

## Configuration

### Enabling Task Runners

To enable task runners, set `taskRunners.enabled=true` in your values file:

```yaml
taskRunners:
  enabled: true

  # Image configuration
  image:
    repository: n8nio/runners
    # Leave tag empty to automatically use the same version as n8n (recommended)
    tag: ""
    # Or set explicitly if needed (not recommended unless you have a specific reason)
    # tag: "1.111.0"
    pullPolicy: IfNotPresent

  # Authentication token (required)
  authToken:
    # Option 1: Use existing secret (recommended for production)
    existingSecret: "my-task-runner-secret"
    existingSecretKey: "N8N_RUNNERS_AUTH_TOKEN"

    # Option 2: Provide token directly (not recommended for production)
    # Generate with: openssl rand -base64 32
    value: ""

  # Broker configuration
  broker:
    listenAddress: "0.0.0.0"  # Allow sidecar connections
    port: 5679

  # Launcher settings
  launcher:
    logLevel: info  # debug, info, warn, error
    autoShutdownTimeout: 15  # Seconds of inactivity before shutdown (0=disabled)

  # Enable Python support (currently in beta)
  nativePythonRunner: true

  # Resource limits for task runner sidecar
  resources:
    requests:
      cpu: 100m
      memory: 256Mi
    limits:
      cpu: 500m
      memory: 512Mi
```

### Custom Package Configuration

To allowlist additional JavaScript or Python packages for use in the Code node:

1. Create a custom launcher configuration file (`n8n-task-runners.json`):

```json
{
  "task-runners": [
    {
      "runner-type": "javascript",
      "env-overrides": {
        "NODE_FUNCTION_ALLOW_BUILTIN": "crypto,fs",
        "NODE_FUNCTION_ALLOW_EXTERNAL": "moment,lodash,axios"
      }
    },
    {
      "runner-type": "python",
      "env-overrides": {
        "PYTHONPATH": "/opt/runners/task-runner-python",
        "N8N_RUNNERS_STDLIB_ALLOW": "json,os,sys",
        "N8N_RUNNERS_EXTERNAL_ALLOW": "numpy,pandas,requests"
      }
    }
  ]
}
```

2. Create a ConfigMap with this configuration:

```bash
kubectl create configmap n8n-task-runner-config \
  --from-file=n8n-task-runners.json=./n8n-task-runners.json \
  -n <namespace>
```

3. Enable custom config in your values:

```yaml
taskRunners:
  enabled: true
  customConfig:
    enabled: true
    configMapName: "n8n-task-runner-config"
    configMapKey: "n8n-task-runners.json"
```

## Security Considerations

### Authentication Token

The authentication token is used to secure communication between the n8n broker and task runners.

**Best Practices:**
- Use a random, secure token (at least 32 characters)
- Generate with: `openssl rand -base64 32`
- Store in an external secret management system (not in values.yaml)
- Use `existingSecret` parameter to reference existing secrets

### Package Allowlisting

By default, only a minimal set of packages are allowed in the Code node. To use additional packages:
1. Add them to your custom `n8n-task-runners.json`
2. Explicitly allowlist them in the appropriate section
3. For external packages, ensure they're installed in a custom runners image

### Network Isolation

Task runners communicate with the broker over localhost (within the same pod), providing network isolation from other services.

## Deployment Examples

### Minimal Configuration

```yaml
taskRunners:
  enabled: true
  authToken:
    value: "your-secret-token-here"  # Replace with secure token
```

### Production Configuration

```yaml
taskRunners:
  enabled: true

  image:
    repository: n8nio/runners
    # Tag automatically matches n8n version when empty
    tag: ""
    pullPolicy: IfNotPresent

  authToken:
    existingSecret: "n8n-task-runner-secret"
    existingSecretKey: "token"

  broker:
    listenAddress: "0.0.0.0"
    port: 5679

  launcher:
    logLevel: warn
    autoShutdownTimeout: 30

  nativePythonRunner: true

  customConfig:
    enabled: true
    configMapName: "n8n-task-runner-config"
    configMapKey: "n8n-task-runners.json"

  resources:
    requests:
      cpu: 200m
      memory: 512Mi
    limits:
      cpu: 1
      memory: 1Gi
```

## Verification

After deploying with task runners enabled, verify the setup:

### 1. Check Pod Structure

```bash
kubectl get pods -n <namespace>
kubectl describe pod <n8n-main-pod-name> -n <namespace>
```

You should see two containers in each pod:
- `n8n-main` or `n8n-worker`
- `task-runner`

### 2. Check Logs

```bash
# Check n8n broker logs
kubectl logs <pod-name> -c n8n-main -n <namespace>

# Check task runner logs
kubectl logs <pod-name> -c task-runner -n <namespace>
```

Look for successful WebSocket connections between broker and runners.

### 3. Test Code Node

1. Log into n8n UI
2. Create a workflow with a Code node
3. Add JavaScript or Python code
4. Execute the workflow
5. Verify the code executes successfully

Example JavaScript test:
```javascript
return [
  {
    json: {
      message: "Task runner is working!",
      timestamp: new Date().toISOString()
    }
  }
];
```

Example Python test:
```python
return [
  {
    "json": {
      "message": "Python runner is working!",
      "version": "3.x"
    }
  }
]
```

## Troubleshooting

### Task Runner Not Starting

**Symptoms:** Task runner container crashes or fails to start

**Possible Causes:**
- Image version mismatch between n8n and runners
- Invalid authentication token
- Resource constraints

**Solutions:**
1. Ensure `taskRunners.image.tag` matches `image.tag`
2. Verify auth token is correctly set
3. Check pod events: `kubectl describe pod <pod-name>`
4. Increase resource limits

### Authentication Failures

**Symptoms:** Logs show "Authentication failed" or similar errors

**Solutions:**
1. Verify the same auth token is used by both broker and runners
2. Check secret exists: `kubectl get secret <secret-name>`
3. Verify secret key name matches configuration

### Code Execution Failures

**Symptoms:** Code node fails with "Module not found" or similar errors

**Solutions:**
1. Check if the package is allowlisted in launcher config
2. Verify custom config ConfigMap is mounted correctly
3. For external packages, ensure they're installed in runners image

### Connection Issues

**Symptoms:** Task runner cannot connect to broker

**Solutions:**
1. Verify broker is listening on correct port (5679)
2. Check `N8N_RUNNERS_BROKER_LISTEN_ADDRESS` is set to `0.0.0.0`
3. Verify task runner is using correct URI: `http://localhost:5679`

## Upgrading

When upgrading n8n versions:

1. Update only `image.tag` - the task runner tag will automatically match
2. Review release notes for any task runner changes
3. Test in a non-production environment first

Example:
```yaml
image:
  tag: "1.111.0"

taskRunners:
  enabled: true
  image:
    # tag is empty - automatically uses 1.111.0 from image.tag
    tag: ""
```

**Note:** The task runner image tag automatically defaults to the same version as the main n8n image. This ensures version compatibility and simplifies upgrades. Only set `taskRunners.image.tag` explicitly if you have a specific reason to use a different version (not recommended).

## Performance Tuning

### Resource Allocation

Adjust based on your workload:

```yaml
taskRunners:
  resources:
    # For light workloads
    requests: {cpu: 100m, memory: 256Mi}
    limits: {cpu: 500m, memory: 512Mi}

    # For heavy workloads
    # requests: {cpu: 500m, memory: 1Gi}
    # limits: {cpu: 2, memory: 2Gi}
```

### Auto-Shutdown

Control runner lifecycle to save resources:

```yaml
taskRunners:
  launcher:
    # Shutdown after 15s of inactivity (default)
    autoShutdownTimeout: 15

    # Disable auto-shutdown for always-on runners
    # autoShutdownTimeout: 0
```

### Concurrency

Worker concurrency affects task distribution:

```yaml
queueMode:
  workerReplicaCount: 2
  workerConcurrency: 10  # Tasks per worker
```

## Implementation Details

### Files Modified

- `values.yaml` - Added `taskRunners` configuration section
- `configmap.yaml` - Added task runner environment variables
- `_configmap-env.tpl` - Added helper functions for task runner env vars
- `deployment-main.yaml` - Added sidecar container and broker env vars
- `deployment-worker.yaml` - Added sidecar container and broker env vars
- `values.schema.json` - Added schema validation for task runners
- `Chart.yaml` - Bumped version to 1.3.0

### Files Added

- `templates/secret-task-runners.yaml` - Secret for auth token

### Key Implementation Features

1. **Automatic Version Matching**: The task runner image tag automatically defaults to the same version as the main n8n image. This is achieved using Helm's `default` function:
   ```yaml
   image: "{{ .Values.taskRunners.image.repository }}:{{ default .Values.image.tag .Values.taskRunners.image.tag }}"
   ```
   This ensures version compatibility and simplifies upgrades.

2. **Sidecar Pattern**: Task runners run as sidecar containers in the same pod as the main/worker n8n containers, enabling localhost communication and network isolation.

3. **Conditional Deployment**: All task runner resources are conditionally deployed based on the `taskRunners.enabled` flag, ensuring backward compatibility.

### Environment Variables Set

**On n8n containers (broker):**
- `N8N_RUNNERS_ENABLED=true`
- `N8N_RUNNERS_MODE=external`
- `N8N_RUNNERS_BROKER_LISTEN_ADDRESS=0.0.0.0`
- `N8N_RUNNERS_AUTH_TOKEN=<token>`
- `N8N_NATIVE_PYTHON_RUNNER=true`

**On task-runner containers:**
- `N8N_RUNNERS_TASK_BROKER_URI=http://localhost:5679`
- `N8N_RUNNERS_AUTH_TOKEN=<token>`
- `N8N_RUNNERS_AUTO_SHUTDOWN_TIMEOUT=15`
- `N8N_RUNNERS_LAUNCHER_LOG_LEVEL=info`

## References

- [n8n Task Runners Documentation](https://docs.n8n.io/hosting/configuration/task-runners/)
- [n8n Runners Docker Image](https://hub.docker.com/r/n8nio/runners)
- [n8n Code Node Documentation](https://docs.n8n.io/code-examples/expressions/)

## Support

For issues or questions:
1. Check the troubleshooting section above
2. Review n8n documentation
3. Open an issue on the chart repository
4. Join the n8n community forums
