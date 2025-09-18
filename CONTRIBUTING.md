# Contributing to n8n Helm Chart

Thank you for your interest in contributing to the n8n Helm Chart! This guide will help you get started with development and understand our contribution process.

## Table of Contents

- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Testing](#testing)
- [Making Changes](#making-changes)
- [Submitting Changes](#submitting-changes)
- [Code Style](#code-style)
- [Release Process](#release-process)

## Getting Started

### Prerequisites

- **Kubernetes cluster** (local or remote) - we recommend [kind](https://kind.sigs.k8s.io/) or [minikube](https://minikube.sigs.k8s.io/) for local development
- **Helm 3.8+** - [Installation guide](https://helm.sh/docs/intro/install/)
- **kubectl** - [Installation guide](https://kubernetes.io/docs/tasks/tools/)
- **Docker** (for local testing with external services)

### Fork and Clone

1. Fork the repository on GitHub
2. Clone your fork:
   ```bash
   git clone https://github.com/YOUR-USERNAME/n8n-helm-chart.git
   cd n8n-helm-chart
   ```

## Development Setup

### 1. Set Up External Services

For development, you can use Docker containers for PostgreSQL and Redis:

```bash
# PostgreSQL
docker run -d --name n8n-postgres \
  -e POSTGRES_DB=n8n \
  -e POSTGRES_USER=n8n \
  -e POSTGRES_PASSWORD=n8npassword \
  -p 5432:5432 \
  postgres:15

# Redis
docker run -d --name n8n-redis \
  -p 6379:6379 \
  redis:7-alpine
```

### 2. Create Development Secrets

Create the required secrets for development:

```bash
kubectl create secret generic n8n-core-secrets \
  --from-literal=encryption-key="$(openssl rand -hex 32)" \
  --from-literal=webhook-url="http://localhost:5678"

kubectl create secret generic n8n-db-secret \
  --from-literal=password="n8npassword"
```

### 3. Install the Chart

Use one of the example configurations for development:

```bash
# For basic development
helm install n8n-dev ./charts/n8n -f examples/minimal.yaml

# For testing enterprise features (requires license)
helm install n8n-dev ./charts/n8n -f examples/production-s3.yaml
```

### 4. Access n8n

```bash
# Port forward to access n8n locally
kubectl port-forward service/n8n-dev-main 5678:5678

# Open http://localhost:5678 in your browser
```

## Testing

### Chart Validation

Always validate your changes:

```bash
# Lint the chart
helm lint charts/n8n

# Test template rendering
helm template test-release charts/n8n -f examples/minimal.yaml --dry-run

# Install with dry-run to validate against cluster
helm install test-release charts/n8n -f examples/minimal.yaml --dry-run
```

### Example Validation

Test all examples to ensure they work correctly:

```bash
# Test each example configuration
for example in examples/*.yaml; do
  echo "Testing $example..."
  helm template test charts/n8n -f "$example" --dry-run > /dev/null
  if [ $? -eq 0 ]; then
    echo "✓ $example is valid"
  else
    echo "✗ $example has errors"
  fi
done
```

### Deployment Testing

Test actual deployments with different configurations:

```bash
# Test minimal setup
helm install n8n-test ./charts/n8n -f examples/minimal.yaml
kubectl wait --for=condition=ready pod -l app.kubernetes.io/name=n8n --timeout=300s
helm uninstall n8n-test

# Test production setup (if you have enterprise license)
helm install n8n-test ./charts/n8n -f examples/production-s3.yaml
kubectl wait --for=condition=ready pod -l app.kubernetes.io/name=n8n --timeout=300s
helm uninstall n8n-test
```

## Making Changes

### Chart Development

1. **Template Changes**: Modify files in `charts/n8n/templates/`
2. **Values**: Update `charts/n8n/values.yaml` for new configuration options
3. **Examples**: Add/update examples in `examples/` directory
4. **Documentation**: Update relevant documentation

### Best Practices

- **Follow Helm best practices**: Use proper labels, annotations, and naming conventions
- **Maintain backward compatibility**: Avoid breaking changes in minor releases
- **Use semantic versioning**: Follow the guidelines in CHANGELOG.md
- **Test thoroughly**: Validate all examples and edge cases
- **Document changes**: Update CHANGELOG.md and relevant documentation

### Chart Structure

```
charts/n8n/
├── Chart.yaml          # Chart metadata
├── values.yaml         # Default values
├── templates/
│   ├── deployment.yaml # Main n8n deployment
│   ├── service.yaml    # Kubernetes services
│   ├── secrets.yaml    # Secret templates
│   └── ...
└── tests/              # Helm tests
```

## Submitting Changes

### 1. Create a Branch

```bash
git checkout -b feature/your-feature-name
```

### 2. Make Your Changes

Follow the testing guidelines above and ensure:
- Chart validates successfully
- All examples work correctly
- Documentation is updated
- CHANGELOG.md is updated

### 3. Commit Changes

We use [Conventional Commits](https://www.conventionalcommits.org/) format for automated releases:

```bash
git add .
git commit -m "feat: add new feature description

Detailed description of what this change does and why.

- Specific change 1
- Specific change 2
"
```

#### Commit Types and Release Rules

| Type | Scope | Release | Description |
|------|-------|---------|-------------|
| `feat` | - | minor | New features |
| `fix` | - | patch | Bug fixes |
| `perf` | - | patch | Performance improvements |
| `docs` | - | patch | Documentation changes |
| `refactor` | - | patch | Code refactoring |
| `build` | - | patch | Build system changes |
| `chore` | - | none | General maintenance |
| `chore` | `deps` | patch | Production dependency updates |
| `chore` | `security` | patch | Security updates |
| `chore` | `infra` | patch | Infrastructure changes |
| `chore` | `config` | patch | Configuration changes |
| `chore` | `deps-dev` | none | Development dependency updates |
| `test` | - | none | Test additions/changes |
| `ci` | - | none | CI/CD changes |
| `style` | - | none | Code style changes |

#### Examples

```bash
# Standard commits
git commit -m "feat: add support for custom ingress annotations"
git commit -m "fix: resolve pod startup issues in Redis mode"

# Chore commits that trigger releases
git commit -m "chore(deps): update n8n to v1.15.0"
git commit -m "chore(security): update vulnerable dependencies"
git commit -m "chore(infra): optimize resource requests and limits"
git commit -m "chore(config): update default PostgreSQL version"

# Chore commits that don't trigger releases
git commit -m "chore: update .gitignore"
git commit -m "chore(deps-dev): update helm to v3.13.0"
git commit -m "ci: improve workflow performance"

# Prevent any release with special scope
git commit -m "fix(no-release): temporary hotfix for testing"
```

### 4. Push and Create PR

```bash
git push origin feature/your-feature-name
```

Then create a Pull Request on GitHub with:
- Clear description of changes
- Reference to any related issues
- Testing steps you performed
- Screenshots if UI-related

## Code Style

### YAML Formatting

- Use 2 spaces for indentation
- Keep lines under 120 characters
- Use meaningful names for values
- Add comments for complex configurations

### Values Structure

```yaml
# Good: Nested structure with clear hierarchy
database:
  type: postgres
  useExternal: true
  host: ""
  port: 5432

# Bad: Flat structure
databaseType: postgres
databaseUseExternal: true
databaseHost: ""
databasePort: 5432
```

### Template Conventions

- Use `{{- if }}` for conditionals to avoid extra whitespace
- Include resource labels and annotations
- Use proper indentation for nested YAML
- Add template comments for complex logic

## Release Process

We follow semantic versioning and use automated releases:

1. **Update Chart.yaml** with new version
2. **Update CHANGELOG.md** with release notes
3. **Create and push tag**: `git tag v1.2.3 && git push origin v1.2.3`
4. **GitHub Actions** will automatically create the release

See [CHANGELOG.md](./CHANGELOG.md) for detailed versioning guidelines.

## Getting Help

- **Questions**: Open a GitHub Discussion
- **Bugs**: Create a GitHub Issue with the bug report template
- **Features**: Create a GitHub Issue with the feature request template
- **Documentation**: Improvements are always welcome!

## License

By contributing, you agree that your contributions will be licensed under the same license as the project.

Thank you for contributing to the n8n Helm Chart! 🎉