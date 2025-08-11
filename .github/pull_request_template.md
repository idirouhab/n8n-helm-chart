## Summary

Fixes #(issue)

---

## Changes

- [ ] Feature
- [ ] Bug fix
- [ ] Documentation update
- [ ] Other (specify):

### What’s changed?

1. 
2. 
3. 

---

## How to test

Example:

```bash
helm lint ./charts/n8n-app
helm template n8n ./charts/n8n-app -f values-dev.yaml > rendered.yaml
k3d cluster create test --port 8080:80@loadbalancer
kubectl create namespace n8n-test
helm upgrade --install n8n ./charts/n8n-app \
  -n n8n-test \
  -f values-dev.yaml \
  --set database.host=postgres-postgresql.n8n-test.svc.cluster.local \
  --set redis.host=redis-master.n8n-test.svc.cluster.local
kubectl get pods -n n8n-test
````

---

## PR Checklist

* [ ] Chart lints without errors: `helm lint ./charts/n8n-app`
* [ ] Templates render without errors: `helm template ...`
* [ ] Tested locally in k3d/kind/minikube
* [ ] New values documented in `README.md`
* [ ] Example in `values-dev.yaml` updated if needed
* [ ] Chart version bumped in `charts/n8n-app/Chart.yaml`
* [ ] No hardcoded namespaces, hosts, or secrets
* [ ] Follows [Conventional Commits](https://www.conventionalcommits.org/)

---

## Screenshots / Logs (optional)
