# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.13.0](https://github.com/idirouhab/n8n-helm-chart/compare/v0.12.0...v0.13.0) (2025-09-14)


### Features

* Update chart version on PR ([3b2732a](https://github.com/idirouhab/n8n-helm-chart/commit/3b2732a111752239b62fe1d078dc283b4b485148))

## [0.12.0](https://github.com/idirouhab/n8n-helm-chart/compare/v0.11.0...v0.12.0) (2025-09-14)


### Features

* add n8n Helm chart with queue mode support and production examples ([07c7ab3](https://github.com/idirouhab/n8n-helm-chart/commit/07c7ab3ea82bda6c7895038a2bd2654ed08221a7))
* add open source community files ([19f2cab](https://github.com/idirouhab/n8n-helm-chart/commit/19f2cab6ad4b7dc498f163404ca6bd8923f5b302))
* add PostgreSQL SSL and external volume mounting support ([3982814](https://github.com/idirouhab/n8n-helm-chart/commit/3982814811e091f731c94591f0bc9d263d4d4add))
* add pre-commit validation hook and fix schema compliance ([cba2dfb](https://github.com/idirouhab/n8n-helm-chart/commit/cba2dfbc31e99465f27a2b56cfd5dc85607ad559))
* add worker concurrency configuration ([7a585b0](https://github.com/idirouhab/n8n-helm-chart/commit/7a585b0b7077d1cd7dfa84b6a680681915816d93))
* Automatically update Chat version on release ([54c3158](https://github.com/idirouhab/n8n-helm-chart/commit/54c315835d12745916b71d35fe8823bb6f1bfe08))
* enhance ci ([7a16ce9](https://github.com/idirouhab/n8n-helm-chart/commit/7a16ce91aad06385fe78f10d325c74e129e49609))
* tag last release as latest ([910c629](https://github.com/idirouhab/n8n-helm-chart/commit/910c629fd5f970a12cfee4ec769c9475befc588e))
* Update README.md with example to install helm chart ([91b11bb](https://github.com/idirouhab/n8n-helm-chart/commit/91b11bb040647749aeb9beac877ab4f459074071))


### Bug Fixes

* add enable queue mode ([af13055](https://github.com/idirouhab/n8n-helm-chart/commit/af13055cd925162907e49267ed3a454ae34a6361))
* add missing SSL environment variables to pod deployments ([d36be68](https://github.com/idirouhab/n8n-helm-chart/commit/d36be687dd82160a0b15df0d7474355f165b2b73))
* Add permissions to push security reports ([d0752d1](https://github.com/idirouhab/n8n-helm-chart/commit/d0752d15a69a733f667b4e77746550a4084c57d2))
* Create releae ([549ad0f](https://github.com/idirouhab/n8n-helm-chart/commit/549ad0f206ed1f20c1178f6be796872d62d05246))
* improve webhook URL configuration and clean up environment variables ([dccbee7](https://github.com/idirouhab/n8n-helm-chart/commit/dccbee7a83e87ded43039851a919f988f44566cc))
* remove hyphen normalization and use ORAS v1.0.0 for setup-oras action ([09da8ed](https://github.com/idirouhab/n8n-helm-chart/commit/09da8ed3b07a2155b1fed1039149a9ac44d35af7))
* Update release file ([2f05fb2](https://github.com/idirouhab/n8n-helm-chart/commit/2f05fb2c5e6aa862d1e28185dcd5525361d06e0e))
* Use oras 1.0.0 ([f13974a](https://github.com/idirouhab/n8n-helm-chart/commit/f13974ab5dbd4f39fbedc693c7453b839ff986b3))
* Use oras 1.2.2 ([8471956](https://github.com/idirouhab/n8n-helm-chart/commit/8471956febf96462759cc26b47d541daa1475d30))
* Use proper value for executions timeout ([7ddb581](https://github.com/idirouhab/n8n-helm-chart/commit/7ddb5813f62370159616640d54a3a37c3d39be18))


### Documentation

* add Redis TLS configuration guidance for AWS ElastiCache ([bc04445](https://github.com/idirouhab/n8n-helm-chart/commit/bc04445caa721c9b0919c680d030ea1a9bdc0e25))


### Miscellaneous Chores

* Add extension to License file ([e34bcac](https://github.com/idirouhab/n8n-helm-chart/commit/e34bcacdf94672ff25e59a94321c6d1df68a8778))
* fix format header in LICENSE.md ([ccf3d6a](https://github.com/idirouhab/n8n-helm-chart/commit/ccf3d6ab6dcd4456b70e9803ecfc742b0f05ff43))
* **main:** release 0.10.0 ([#22](https://github.com/idirouhab/n8n-helm-chart/issues/22)) ([1615e45](https://github.com/idirouhab/n8n-helm-chart/commit/1615e453b9d21a4c2b8f86878634a8c214a59073))
* **main:** release 0.11.0 ([#25](https://github.com/idirouhab/n8n-helm-chart/issues/25)) ([f7577c8](https://github.com/idirouhab/n8n-helm-chart/commit/f7577c8ec6b21ca8b335680aba8e3e57dd834985))
* **main:** release 0.2.0 ([#3](https://github.com/idirouhab/n8n-helm-chart/issues/3)) ([01d3535](https://github.com/idirouhab/n8n-helm-chart/commit/01d3535e42a652c66a02746046a6f352bc6a9ef8))
* **main:** release 0.3.0 ([#4](https://github.com/idirouhab/n8n-helm-chart/issues/4)) ([f365f44](https://github.com/idirouhab/n8n-helm-chart/commit/f365f44c67425716b732ee3e0a2e2c132e5911eb))
* **main:** release 0.3.1 ([#5](https://github.com/idirouhab/n8n-helm-chart/issues/5)) ([a51a3cc](https://github.com/idirouhab/n8n-helm-chart/commit/a51a3cc02fcf5c371ab3f202ba829d118ed71611))
* **main:** release 0.4.0 ([#6](https://github.com/idirouhab/n8n-helm-chart/issues/6)) ([5b35003](https://github.com/idirouhab/n8n-helm-chart/commit/5b350033a3a8c3b4adb2fc4fce478421b63a2cb7))
* **main:** release 0.5.0 ([#7](https://github.com/idirouhab/n8n-helm-chart/issues/7)) ([7f52b00](https://github.com/idirouhab/n8n-helm-chart/commit/7f52b00aead40fc102818f6495179175c3929f43))
* **main:** release 0.6.0 ([#8](https://github.com/idirouhab/n8n-helm-chart/issues/8)) ([19ce9cf](https://github.com/idirouhab/n8n-helm-chart/commit/19ce9cf7a6fd744ffe8090a33dba257533a2800a))
* **main:** release 0.6.1 ([#9](https://github.com/idirouhab/n8n-helm-chart/issues/9)) ([13fed78](https://github.com/idirouhab/n8n-helm-chart/commit/13fed782e04c0820d6bd2192a7f15aa2839e26df))
* **main:** release 0.6.2 ([#10](https://github.com/idirouhab/n8n-helm-chart/issues/10)) ([f0578cc](https://github.com/idirouhab/n8n-helm-chart/commit/f0578cc41e6904ffc04841f66fe6a640c30c89e0))
* **main:** release 0.6.3 ([#11](https://github.com/idirouhab/n8n-helm-chart/issues/11)) ([616ccb0](https://github.com/idirouhab/n8n-helm-chart/commit/616ccb02f4aa41283f89de279670fcb5c4ed3421))
* **main:** release 0.6.4 ([#12](https://github.com/idirouhab/n8n-helm-chart/issues/12)) ([cb05838](https://github.com/idirouhab/n8n-helm-chart/commit/cb05838900b69d3dbd353a82664509f66af2efac))
* **main:** release 0.6.5 ([#13](https://github.com/idirouhab/n8n-helm-chart/issues/13)) ([dd11637](https://github.com/idirouhab/n8n-helm-chart/commit/dd116377d99b5290571ffc1dce22407e7501c3a4))
* **main:** release 0.7.0 ([#14](https://github.com/idirouhab/n8n-helm-chart/issues/14)) ([ccd89d0](https://github.com/idirouhab/n8n-helm-chart/commit/ccd89d0ce5a542c2480cbcc1af84ad50273db555))
* **main:** release 0.7.1 ([#16](https://github.com/idirouhab/n8n-helm-chart/issues/16)) ([c0304b8](https://github.com/idirouhab/n8n-helm-chart/commit/c0304b82c6053eba408ad0c1e0b5950d078e044d))
* **main:** release 0.8.0 ([#17](https://github.com/idirouhab/n8n-helm-chart/issues/17)) ([3b6f67e](https://github.com/idirouhab/n8n-helm-chart/commit/3b6f67eaa24448bced5a0d13061474f17416e07c))
* **main:** release 0.9.0 ([#18](https://github.com/idirouhab/n8n-helm-chart/issues/18)) ([fb19aaf](https://github.com/idirouhab/n8n-helm-chart/commit/fb19aaf73ec2429e36d63fdf4f1717b3c3e6ccac))
* **main:** release 0.9.1 ([#19](https://github.com/idirouhab/n8n-helm-chart/issues/19)) ([ff9ecab](https://github.com/idirouhab/n8n-helm-chart/commit/ff9ecab4b5f20477e42032b054a52b9e57948523))
* **main:** release 0.9.2 ([#20](https://github.com/idirouhab/n8n-helm-chart/issues/20)) ([5d9b8da](https://github.com/idirouhab/n8n-helm-chart/commit/5d9b8da43ff2744d98eb0c013bfcfa8dbc1d9e51))
* **main:** release 0.9.3 ([#21](https://github.com/idirouhab/n8n-helm-chart/issues/21)) ([e7dfdeb](https://github.com/idirouhab/n8n-helm-chart/commit/e7dfdeb24783733ae1d95ba720252632da99cadf))
* Rename LICENSE to LICENSE.md ([e34bcac](https://github.com/idirouhab/n8n-helm-chart/commit/e34bcacdf94672ff25e59a94321c6d1df68a8778))


### Continuous Integration

* add Docker Hub OCI publishing workflow integrated with release-please ([2180e38](https://github.com/idirouhab/n8n-helm-chart/commit/2180e38782fa9c31e9a6d6639fa073d79e98b421))
* add Docker Hub OCI registry publishing workflow ([4264475](https://github.com/idirouhab/n8n-helm-chart/commit/426447541c033cc3f532cae5d3f33e5ff5aba233))
* integrate Docker Hub OCI publishing into release-please workflow ([0955501](https://github.com/idirouhab/n8n-helm-chart/commit/095550176247cb781b25f89e7deb70c3452166ba))

## [0.11.0](https://github.com/idirouhab/n8n-helm-chart/compare/v0.10.0...v0.11.0) (2025-09-14)


### Features

* Update README.md with example to install helm chart ([91b11bb](https://github.com/idirouhab/n8n-helm-chart/commit/91b11bb040647749aeb9beac877ab4f459074071))


### Bug Fixes

* Update release file ([2f05fb2](https://github.com/idirouhab/n8n-helm-chart/commit/2f05fb2c5e6aa862d1e28185dcd5525361d06e0e))

## [0.10.0](https://github.com/idirouhab/n8n-helm-chart/compare/v0.9.3...v0.10.0) (2025-09-13)


### Features

* add open source community files ([19f2cab](https://github.com/idirouhab/n8n-helm-chart/commit/19f2cab6ad4b7dc498f163404ca6bd8923f5b302))

## [0.9.3](https://github.com/idirouhab/n8n-helm-chart/compare/v0.9.2...v0.9.3) (2025-09-13)


### Bug Fixes

* remove hyphen normalization and use ORAS v1.0.0 for setup-oras action ([09da8ed](https://github.com/idirouhab/n8n-helm-chart/commit/09da8ed3b07a2155b1fed1039149a9ac44d35af7))

## [0.9.2](https://github.com/idirouhab/n8n-helm-chart/compare/v0.9.1...v0.9.2) (2025-09-13)


### Bug Fixes

* Use oras 1.0.0 ([f13974a](https://github.com/idirouhab/n8n-helm-chart/commit/f13974ab5dbd4f39fbedc693c7453b839ff986b3))

## [0.9.1](https://github.com/idirouhab/n8n-helm-chart/compare/v0.9.0...v0.9.1) (2025-09-13)


### Bug Fixes

* Use oras 1.2.2 ([8471956](https://github.com/idirouhab/n8n-helm-chart/commit/8471956febf96462759cc26b47d541daa1475d30))

## [0.9.0](https://github.com/idirouhab/n8n-helm-chart/compare/v0.8.0...v0.9.0) (2025-09-13)


### Features

* add n8n Helm chart with queue mode support and production examples ([07c7ab3](https://github.com/idirouhab/n8n-helm-chart/commit/07c7ab3ea82bda6c7895038a2bd2654ed08221a7))
* add PostgreSQL SSL and external volume mounting support ([3982814](https://github.com/idirouhab/n8n-helm-chart/commit/3982814811e091f731c94591f0bc9d263d4d4add))
* add pre-commit validation hook and fix schema compliance ([cba2dfb](https://github.com/idirouhab/n8n-helm-chart/commit/cba2dfbc31e99465f27a2b56cfd5dc85607ad559))
* add worker concurrency configuration ([7a585b0](https://github.com/idirouhab/n8n-helm-chart/commit/7a585b0b7077d1cd7dfa84b6a680681915816d93))
* Automatically update Chat version on release ([54c3158](https://github.com/idirouhab/n8n-helm-chart/commit/54c315835d12745916b71d35fe8823bb6f1bfe08))
* enhance ci ([7a16ce9](https://github.com/idirouhab/n8n-helm-chart/commit/7a16ce91aad06385fe78f10d325c74e129e49609))
* tag last release as latest ([910c629](https://github.com/idirouhab/n8n-helm-chart/commit/910c629fd5f970a12cfee4ec769c9475befc588e))


### Bug Fixes

* add enable queue mode ([af13055](https://github.com/idirouhab/n8n-helm-chart/commit/af13055cd925162907e49267ed3a454ae34a6361))
* add missing SSL environment variables to pod deployments ([d36be68](https://github.com/idirouhab/n8n-helm-chart/commit/d36be687dd82160a0b15df0d7474355f165b2b73))
* Add permissions to push security reports ([d0752d1](https://github.com/idirouhab/n8n-helm-chart/commit/d0752d15a69a733f667b4e77746550a4084c57d2))
* improve webhook URL configuration and clean up environment variables ([dccbee7](https://github.com/idirouhab/n8n-helm-chart/commit/dccbee7a83e87ded43039851a919f988f44566cc))
* Use proper value for executions timeout ([7ddb581](https://github.com/idirouhab/n8n-helm-chart/commit/7ddb5813f62370159616640d54a3a37c3d39be18))


### Documentation

* add Redis TLS configuration guidance for AWS ElastiCache ([bc04445](https://github.com/idirouhab/n8n-helm-chart/commit/bc04445caa721c9b0919c680d030ea1a9bdc0e25))


### Miscellaneous Chores

* Add extension to License file ([e34bcac](https://github.com/idirouhab/n8n-helm-chart/commit/e34bcacdf94672ff25e59a94321c6d1df68a8778))
* fix format header in LICENSE.md ([ccf3d6a](https://github.com/idirouhab/n8n-helm-chart/commit/ccf3d6ab6dcd4456b70e9803ecfc742b0f05ff43))
* **main:** release 0.2.0 ([#3](https://github.com/idirouhab/n8n-helm-chart/issues/3)) ([01d3535](https://github.com/idirouhab/n8n-helm-chart/commit/01d3535e42a652c66a02746046a6f352bc6a9ef8))
* **main:** release 0.3.0 ([#4](https://github.com/idirouhab/n8n-helm-chart/issues/4)) ([f365f44](https://github.com/idirouhab/n8n-helm-chart/commit/f365f44c67425716b732ee3e0a2e2c132e5911eb))
* **main:** release 0.3.1 ([#5](https://github.com/idirouhab/n8n-helm-chart/issues/5)) ([a51a3cc](https://github.com/idirouhab/n8n-helm-chart/commit/a51a3cc02fcf5c371ab3f202ba829d118ed71611))
* **main:** release 0.4.0 ([#6](https://github.com/idirouhab/n8n-helm-chart/issues/6)) ([5b35003](https://github.com/idirouhab/n8n-helm-chart/commit/5b350033a3a8c3b4adb2fc4fce478421b63a2cb7))
* **main:** release 0.5.0 ([#7](https://github.com/idirouhab/n8n-helm-chart/issues/7)) ([7f52b00](https://github.com/idirouhab/n8n-helm-chart/commit/7f52b00aead40fc102818f6495179175c3929f43))
* **main:** release 0.6.0 ([#8](https://github.com/idirouhab/n8n-helm-chart/issues/8)) ([19ce9cf](https://github.com/idirouhab/n8n-helm-chart/commit/19ce9cf7a6fd744ffe8090a33dba257533a2800a))
* **main:** release 0.6.1 ([#9](https://github.com/idirouhab/n8n-helm-chart/issues/9)) ([13fed78](https://github.com/idirouhab/n8n-helm-chart/commit/13fed782e04c0820d6bd2192a7f15aa2839e26df))
* **main:** release 0.6.2 ([#10](https://github.com/idirouhab/n8n-helm-chart/issues/10)) ([f0578cc](https://github.com/idirouhab/n8n-helm-chart/commit/f0578cc41e6904ffc04841f66fe6a640c30c89e0))
* **main:** release 0.6.3 ([#11](https://github.com/idirouhab/n8n-helm-chart/issues/11)) ([616ccb0](https://github.com/idirouhab/n8n-helm-chart/commit/616ccb02f4aa41283f89de279670fcb5c4ed3421))
* **main:** release 0.6.4 ([#12](https://github.com/idirouhab/n8n-helm-chart/issues/12)) ([cb05838](https://github.com/idirouhab/n8n-helm-chart/commit/cb05838900b69d3dbd353a82664509f66af2efac))
* **main:** release 0.6.5 ([#13](https://github.com/idirouhab/n8n-helm-chart/issues/13)) ([dd11637](https://github.com/idirouhab/n8n-helm-chart/commit/dd116377d99b5290571ffc1dce22407e7501c3a4))
* **main:** release 0.7.0 ([#14](https://github.com/idirouhab/n8n-helm-chart/issues/14)) ([ccd89d0](https://github.com/idirouhab/n8n-helm-chart/commit/ccd89d0ce5a542c2480cbcc1af84ad50273db555))
* **main:** release 0.7.1 ([#16](https://github.com/idirouhab/n8n-helm-chart/issues/16)) ([c0304b8](https://github.com/idirouhab/n8n-helm-chart/commit/c0304b82c6053eba408ad0c1e0b5950d078e044d))
* **main:** release 0.8.0 ([#17](https://github.com/idirouhab/n8n-helm-chart/issues/17)) ([3b6f67e](https://github.com/idirouhab/n8n-helm-chart/commit/3b6f67eaa24448bced5a0d13061474f17416e07c))
* Rename LICENSE to LICENSE.md ([e34bcac](https://github.com/idirouhab/n8n-helm-chart/commit/e34bcacdf94672ff25e59a94321c6d1df68a8778))


### Continuous Integration

* add Docker Hub OCI publishing workflow integrated with release-please ([2180e38](https://github.com/idirouhab/n8n-helm-chart/commit/2180e38782fa9c31e9a6d6639fa073d79e98b421))
* add Docker Hub OCI registry publishing workflow ([4264475](https://github.com/idirouhab/n8n-helm-chart/commit/426447541c033cc3f532cae5d3f33e5ff5aba233))
* integrate Docker Hub OCI publishing into release-please workflow ([0955501](https://github.com/idirouhab/n8n-helm-chart/commit/095550176247cb781b25f89e7deb70c3452166ba))

## [0.8.0](https://github.com/idirouhab/n8n-helm-chart/compare/v0.7.1...v0.8.0) (2025-09-13)


### Features

* tag last release as latest ([910c629](https://github.com/idirouhab/n8n-helm-chart/commit/910c629fd5f970a12cfee4ec769c9475befc588e))

## [0.7.1](https://github.com/idirouhab/n8n-helm-chart/compare/v0.7.0...v0.7.1) (2025-09-12)


### Bug Fixes

* improve webhook URL configuration and clean up environment variables ([dccbee7](https://github.com/idirouhab/n8n-helm-chart/commit/dccbee7a83e87ded43039851a919f988f44566cc))

## [0.7.0](https://github.com/idirouhab/n8n-helm-chart/compare/v0.6.5...v0.7.0) (2025-09-11)


### Features

* Automatically update Chat version on release ([54c3158](https://github.com/idirouhab/n8n-helm-chart/commit/54c315835d12745916b71d35fe8823bb6f1bfe08))

## [0.6.5](https://github.com/idirouhab/n8n-helm-chart/compare/v0.6.4...v0.6.5) (2025-09-11)


### Continuous Integration

* integrate Docker Hub OCI publishing into release-please workflow ([0955501](https://github.com/idirouhab/n8n-helm-chart/commit/095550176247cb781b25f89e7deb70c3452166ba))

## [0.6.4](https://github.com/idirouhab/n8n-helm-chart/compare/v0.6.3...v0.6.4) (2025-09-11)


### Continuous Integration

* add Docker Hub OCI publishing workflow integrated with release-please ([2180e38](https://github.com/idirouhab/n8n-helm-chart/commit/2180e38782fa9c31e9a6d6639fa073d79e98b421))

## [0.6.3](https://github.com/idirouhab/n8n-helm-chart/compare/v0.6.2...v0.6.3) (2025-09-11)


### Continuous Integration

* add Docker Hub OCI registry publishing workflow ([4264475](https://github.com/idirouhab/n8n-helm-chart/commit/426447541c033cc3f532cae5d3f33e5ff5aba233))

## [0.6.2](https://github.com/idirouhab/n8n-helm-chart/compare/v0.6.1...v0.6.2) (2025-09-11)


### Documentation

* add Redis TLS configuration guidance for AWS ElastiCache ([bc04445](https://github.com/idirouhab/n8n-helm-chart/commit/bc04445caa721c9b0919c680d030ea1a9bdc0e25))

## [0.6.1](https://github.com/idirouhab/n8n-helm-chart/compare/v0.6.0...v0.6.1) (2025-09-11)


### Bug Fixes

* add enable queue mode ([af13055](https://github.com/idirouhab/n8n-helm-chart/commit/af13055cd925162907e49267ed3a454ae34a6361))
* add missing SSL environment variables to pod deployments ([d36be68](https://github.com/idirouhab/n8n-helm-chart/commit/d36be687dd82160a0b15df0d7474355f165b2b73))

## [0.6.0](https://github.com/idirouhab/n8n-helm-chart/compare/v0.5.0...v0.6.0) (2025-09-11)


### Features

* add PostgreSQL SSL and external volume mounting support ([3982814](https://github.com/idirouhab/n8n-helm-chart/commit/3982814811e091f731c94591f0bc9d263d4d4add))

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
