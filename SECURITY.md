# Security Policy

## Reporting Security Issues

The n8n Helm Chart community takes security issues seriously. If you discover a security vulnerability in this Helm chart, please report it responsibly.

### How to Report

**Please do NOT report security vulnerabilities through public GitHub issues.**

Instead, please send an email to: promptndplay@gmail.com

Include the following information in your report:
- Description of the vulnerability
- Steps to reproduce the issue
- Potential impact
- Suggested fix (if any)

### What to Expect

- **Acknowledgment**: We will acknowledge receipt of your report within 48 hours
- **Investigation**: We will investigate and validate the reported issue
- **Timeline**: We aim to provide an initial response within 7 days
- **Resolution**: Critical issues will be prioritized for immediate fixes

### Disclosure Policy

- We request that you give us reasonable time to investigate and fix the issue before public disclosure
- We will coordinate with you on the timing of any public disclosure
- We will credit you for responsible disclosure (unless you prefer to remain anonymous)

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 0.9.x   | :white_check_mark: |
| < 0.9   | :x:                |

## Security Considerations

This Helm chart includes several security features and considerations:

### Secrets Management
- All sensitive data must be stored in Kubernetes secrets
- Database passwords and encryption keys are never exposed in plain text
- The chart supports external secret management systems

### Network Security
- NetworkPolicy support for restricting pod-to-pod communication
- TLS configuration for ingress controllers
- Secure Redis connections with TLS support

### Pod Security
- Non-root containers by default
- Read-only root filesystem where possible
- Resource limits and requests configured
- Security contexts applied to containers

### RBAC
- Minimal required permissions for service accounts
- No cluster-level permissions granted by default
- Principle of least privilege enforced

### Best Practices
- Regular security updates through automated releases
- Container image security scanning
- Dependency vulnerability monitoring

## Additional Security Resources

- [n8n Security Documentation](https://docs.n8n.io/security/)
- [Kubernetes Security Best Practices](https://kubernetes.io/docs/concepts/security/)
- [Helm Security Considerations](https://helm.sh/docs/topics/securing_installation/)

## Disclaimer

This Helm chart is community-maintained and independent of n8n. For security issues related to n8n itself (not this chart), please refer to the [official n8n security policy](https://github.com/n8n-io/n8n/security/policy).