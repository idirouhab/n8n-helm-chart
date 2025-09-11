# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.5.0](https://github.com/idirouhab/n8n-helm-chart/compare/v0.4.0...v0.5.0) (2025-09-11)


### Features

* add pre-commit validation hook and fix schema compliance ([cba2dfb](https://github.com/idirouhab/n8n-helm-chart/commit/cba2dfbc31e99465f27a2b56cfd5dc85607ad559))

## [0.4.0](https://github.com/idirouhab/n8n-helm-chart/compare/v0.3.1...v0.4.0) (2025-09-11)


### Features

* add worker concurrency configuration ([7a585b0](https://github.com/idirouhab/n8n-helm-chart/commit/7a585b0b7077d1cd7dfa84b6a680681915816d93))

## [0.3.1](https://github.com/idirouhab/n8n-helm-chart/compare/v0.3.0...v0.3.1) (2025-09-11)


### Miscellaneous Chores

* Add extension to License file ([e34bcac](https://github.com/idirouhab/n8n-helm-chart/commit/e34bcacdf94672ff25e59a94321c6d1df68a8778))
* fix format header in LICENSE.md ([ccf3d6a](https://github.com/idirouhab/n8n-helm-chart/commit/ccf3d6ab6dcd4456b70e9803ecfc742b0f05ff43))
* Rename LICENSE to LICENSE.md ([e34bcac](https://github.com/idirouhab/n8n-helm-chart/commit/e34bcacdf94672ff25e59a94321c6d1df68a8778))

## [0.3.0](https://github.com/idirouhab/n8n-helm-chart/compare/v0.2.0...v0.3.0) (2025-09-11)


### Features

* add n8n Helm chart with queue mode support and production examples ([07c7ab3](https://github.com/idirouhab/n8n-helm-chart/commit/07c7ab3ea82bda6c7895038a2bd2654ed08221a7))
* enhance ci ([7a16ce9](https://github.com/idirouhab/n8n-helm-chart/commit/7a16ce91aad06385fe78f10d325c74e129e49609))


### Bug Fixes

* Add permissions to push security reports ([d0752d1](https://github.com/idirouhab/n8n-helm-chart/commit/d0752d15a69a733f667b4e77746550a4084c57d2))
* Use proper value for executions timeout ([7ddb581](https://github.com/idirouhab/n8n-helm-chart/commit/7ddb5813f62370159616640d54a3a37c3d39be18))


### Miscellaneous Chores

* **main:** release 0.2.0 ([#3](https://github.com/idirouhab/n8n-helm-chart/issues/3)) ([01d3535](https://github.com/idirouhab/n8n-helm-chart/commit/01d3535e42a652c66a02746046a6f352bc6a9ef8))

## [0.2.0](https://github.com/idirouhab/n8n-helm-chart/compare/v0.1.0...v0.2.0) (2025-09-11)


### Features

* add n8n Helm chart with queue mode support and production examples ([07c7ab3](https://github.com/idirouhab/n8n-helm-chart/commit/07c7ab3ea82bda6c7895038a2bd2654ed08221a7))
* enhance ci ([7a16ce9](https://github.com/idirouhab/n8n-helm-chart/commit/7a16ce91aad06385fe78f10d325c74e129e49609))


### Bug Fixes

* Add permissions to push security reports ([d0752d1](https://github.com/idirouhab/n8n-helm-chart/commit/d0752d15a69a733f667b4e77746550a4084c57d2))
* Use proper value for executions timeout ([7ddb581](https://github.com/idirouhab/n8n-helm-chart/commit/7ddb5813f62370159616640d54a3a37c3d39be18))

## [Unreleased]

### Added
- Comprehensive webhook processor support with dedicated routing
- Production-ready examples with proper license configuration
- S3 storage configuration with IRSA and access key options
- Multi-main setup with high availability configuration
- Horizontal Pod Autoscaling (HPA) for all components
- Session affinity support for WebSocket connections
- Comprehensive documentation and setup guides

### Changed
- Reorganized examples by license requirements (Community vs Enterprise)
- Renamed examples for clarity (production-s3.yaml, multi-main-queue.yaml)
- Consolidated redundant documentation into single README
- Updated .gitignore to prevent development artifacts

### Removed
- Development-specific values files from repository
- Redundant S3 examples (consolidated into production-s3.yaml)
- IDE configuration files and development artifacts
- Duplicate documentation files

## [0.2.0] - Previous Release

### Added
- Initial Helm chart implementation
- Queue mode support with worker pods
- External PostgreSQL and Redis support
- Basic S3 storage integration
- Service account configuration
- Ingress support

### Security
- External-only database configuration to prevent insecure defaults
- Proper secret management patterns

## [0.1.0] - Initial Release

### Added
- Basic n8n Helm chart structure
- Core deployment templates
- Basic configuration options

---

## Release Guidelines

### Versioning Strategy

This project follows [Semantic Versioning](https://semver.org/):

- **MAJOR** version for incompatible API changes
- **MINOR** version for backwards-compatible functionality additions  
- **PATCH** version for backwards-compatible bug fixes

### Release Types

#### Major Releases (X.0.0)
- Breaking changes to chart values structure
- Removal of deprecated features
- Major architectural changes
- Kubernetes version requirement bumps

#### Minor Releases (0.X.0)
- New features and enhancements
- New configuration options
- Additional deployment modes
- New examples and documentation

#### Patch Releases (0.0.X)
- Bug fixes
- Security patches
- Documentation improvements
- Template fixes without breaking changes

### Release Process

1. **Update Chart.yaml** with new version
2. **Update CHANGELOG.md** with release notes
3. **Tag release** following `vX.Y.Z` format
4. **Create GitHub release** with changelog excerpt
5. **Package and publish** Helm chart (if using chart repository)

### Breaking Changes

When introducing breaking changes:

1. **Document** the change in CHANGELOG under `### Changed` or `### Removed`
2. **Provide migration guide** in the release notes
3. **Bump major version** according to semver
4. **Consider deprecation period** for major changes when possible

### Changelog Categories

- **Added** for new features
- **Changed** for changes in existing functionality
- **Deprecated** for soon-to-be removed features
- **Removed** for now removed features
- **Fixed** for any bug fixes
- **Security** for vulnerability fixes
