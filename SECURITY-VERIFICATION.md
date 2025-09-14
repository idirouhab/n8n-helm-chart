# Security Verification Guide

This document explains how to verify the integrity and authenticity of n8n Helm chart releases.

## Chart Signing with Cosign

All releases are signed using [Cosign](https://github.com/sigstore/cosign) with keyless signing via GitHub OIDC.

### Prerequisites

Install Cosign:

```bash
# macOS
brew install cosign

# Linux
wget "https://github.com/sigstore/cosign/releases/latest/download/cosign-linux-amd64"
sudo mv cosign-linux-amd64 /usr/local/bin/cosign
sudo chmod +x /usr/local/bin/cosign

# Windows
winget install sigstore.cosign
```

### Verifying Chart Signatures

#### Verify OCI Chart

```bash
# Verify the latest version
cosign verify oci://registry-1.docker.io/idirouhab/n8n-app:latest \
  --certificate-identity-regexp="https://github.com/idirouhab/n8n-helm-chart" \
  --certificate-oidc-issuer="https://token.actions.githubusercontent.com"

# Verify a specific version
cosign verify oci://registry-1.docker.io/idirouhab/n8n-app:0.9.2 \
  --certificate-identity-regexp="https://github.com/idirouhab/n8n-helm-chart" \
  --certificate-oidc-issuer="https://token.actions.githubusercontent.com"
```

#### Verify Downloaded Chart

```bash
# Download chart
helm pull oci://registry-1.docker.io/idirouhab/n8n-app --version 0.9.2

# Verify the downloaded .tgz file
cosign verify-blob n8n-app-0.9.2.tgz \
  --certificate-identity-regexp="https://github.com/idirouhab/n8n-helm-chart" \
  --certificate-oidc-issuer="https://token.actions.githubusercontent.com" \
  --signature <(cosign download signature oci://registry-1.docker.io/idirouhab/n8n-app:0.9.2)
```

### Understanding the Verification Output

Successful verification will show:

```json
{
  "critical": {
    "identity": {
      "docker-reference": "registry-1.docker.io/idirouhab/n8n-app"
    },
    "image": {
      "docker-manifest-digest": "sha256:..."
    },
    "type": "cosign container image signature"
  },
  "optional": {
    "Bundle": {
      "SignedEntryTimestamp": "...",
      "Payload": {
        "body": "...",
        "integratedTime": ...,
        "logIndex": ...,
        "logID": "..."
      }
    },
    "Issuer": "https://token.actions.githubusercontent.com",
    "Subject": "https://github.com/idirouhab/n8n-helm-chart/.github/workflows/release-please.yml@refs/heads/main"
  }
}
```

This confirms:
- ✅ The chart was built from this repository
- ✅ The chart was signed during the official release process
- ✅ The signature is timestamped in the public transparency log
- ✅ The chart hasn't been tampered with

## SLSA Provenance

Each release includes SLSA Level 3 provenance attestation that provides:

- **Build integrity**: Proof the chart was built from the declared source
- **Build isolation**: Proof the build ran in an isolated environment
- **Reproducible builds**: Information to reproduce the exact build

### Verifying Provenance

```bash
# Download provenance from GitHub release
curl -L "https://github.com/idirouhab/n8n-helm-chart/releases/download/v0.9.2/n8n-helm-chart.intoto.jsonl" \
  -o provenance.jsonl

# Verify with slsa-verifier
slsa-verifier verify-artifact n8n-app-0.9.2.tgz \
  --provenance-path provenance.jsonl \
  --source-uri github.com/idirouhab/n8n-helm-chart
```

## Security Best Practices

### For End Users

1. **Always verify signatures** before installing charts
2. **Use specific versions** instead of `latest` in production
3. **Pin chart versions** in your GitOps configurations
4. **Monitor for updates** via Renovate or similar tools

### Example Secure Installation

```bash
# 1. Verify the chart
cosign verify oci://registry-1.docker.io/idirouhab/n8n-app:0.9.2 \
  --certificate-identity-regexp="https://github.com/idirouhab/n8n-helm-chart" \
  --certificate-oidc-issuer="https://token.actions.githubusercontent.com"

# 2. Install only if verification succeeds
if [ $? -eq 0 ]; then
  helm install n8n oci://registry-1.docker.io/idirouhab/n8n-app --version 0.9.2 \
    --set database.host=postgres.example.com \
    --set redis.host=redis.example.com
else
  echo "❌ Chart verification failed - aborting installation"
  exit 1
fi
```

## Incident Response

If you suspect a compromised chart:

1. **Stop using the chart** immediately
2. **Report to security team**: promptndplay@gmail.com
3. **Check recent releases** for security advisories
4. **Verify all installed charts** in your cluster

## Additional Resources

- [Cosign Documentation](https://docs.sigstore.dev/cosign/overview/)
- [SLSA Framework](https://slsa.dev/)
- [Supply Chain Security Best Practices](https://www.cisa.gov/resources-tools/resources/software-supply-chain-security)
- [Sigstore Public Transparency Log](https://search.sigstore.dev/)