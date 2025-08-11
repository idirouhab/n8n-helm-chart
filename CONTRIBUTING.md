# Contributing to the n8n Helm Chart

Thanks for taking the time to contribute! This guide will help you get started.


## Repository structure

```

charts/
n8n-app/           # Main Helm chart
templates/       # YAML templates
values.yaml      # Default chart values
values-dev.yaml      # Dev/test values

````

---

## Local development

### 1. Prerequisites

You’ll need:

- [Helm 3.14+](https://helm.sh/docs/intro/install/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- A local cluster:
  - **k3d**: `brew install k3d`
  - or **kind**: `brew install kind`
- Optional: [`kubeconform`](https://github.com/yannh/kubeconform) for schema validation

---

### 2. Setup

Clone the repo:

```bash
git clone https://github.com/idirouhab/n8n-helm-chart.git
cd n8n-helm-chart
````

Create a local k3d cluster:

```bash
k3d cluster create n8n-test -p "8080:80@loadbalancer"
kubectl create namespace n8n-test
```

Install dependencies (if any subcharts are used):

```bash
helm dependency update ./charts/n8n-app
```

---

### 3. Lint & render

Before pushing any change:

```bash
# Lint chart
helm lint ./charts/n8n-app

# Render manifests to check output
helm template n8n ./charts/n8n-app -f values-dev.yaml > rendered.yaml

# Optional: validate with kubeconform
kubeconform -strict -summary rendered.yaml
```

---

### 4. Test in a local cluster

```bash
helm upgrade --install n8n ./charts/n8n-app \
  -n n8n-test \
  -f values-dev.yaml \
  --create-namespace \
  --set database.host=postgres-postgresql.n8n-test.svc.cluster.local \
  --set redis.host=redis-master.n8n-test.svc.cluster.local

kubectl get pods -n n8n-test
```

Port-forward the main service:

```bash
kubectl port-forward svc/n8n-n8n-app-main 5678:5678 -n n8n-test
open http://localhost:5678
```

---

## Guidelines for changes

1. **Nil-safety**: wrap all optional `.Values` references in `if`/`and` guards to avoid `nil pointer` errors.
2. **Defaults**: any new value should have a sensible default in `values.yaml`.
3. **Docs**: if you add/change a value, update `README.md` and add an example in `values-dev.yaml`.
4. **Chart version bump**: update `version:` in `charts/n8n-app/Chart.yaml` whenever the chart behavior changes.
5. **Consistency**: follow existing indentation (2 spaces in YAML).

---

## Git workflow

* Fork the repo and create a branch from `main`.
* Use [Conventional Commits](https://www.conventionalcommits.org/):

    * `feat: add ingress host templating`
    * `fix(worker): set encryption key secret`
* Open a pull request (PR) to `main`.

---

## PR checklist

Before you mark your PR as ready for review:

* [ ] Chart lints without errors: `helm lint ./charts/n8n-app`
* [ ] Templates render without errors: `helm template ...`
* [ ] New values documented in `README.md`
* [ ] Chart version bumped in `Chart.yaml`
* [ ] If applicable, tested locally in k3d/kind
* [ ] No hardcoded namespaces, hosts, or secrets

---

## Reporting bugs

Open a [bug report](../../issues/new?template=bug_report.md) and include:

* Chart version
* Kubernetes version
* `values.yaml` or overrides used
* Install/upgrade command
* Relevant logs or `kubectl describe` output

---

## License

By contributing, you agree that your contributions will be licensed under the [MIT License](LICENSE).

```