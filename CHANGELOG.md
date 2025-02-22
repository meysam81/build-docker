# Changelog

## [1.3.0](https://github.com/meysam81/build-docker/compare/v1.2.0...v1.3.0) (2025-02-22)


### Features

* accept dockerfile as arg ([37723f0](https://github.com/meysam81/build-docker/commit/37723f0fd37de980c89a00d0a70b58253113b784))
* accept submodules as arg ([478ff53](https://github.com/meysam81/build-docker/commit/478ff53a61a1c6c3c761e10565c51fdd86510546))
* checkout custom repositories ([33b0db1](https://github.com/meysam81/build-docker/commit/33b0db15967343a22b242246b8473cf9a38e3464))

## [1.2.0](https://github.com/meysam81/build-docker/compare/v1.1.0...v1.2.0) (2024-12-08)


### Features

* allow to run pre-build commands for extra flexbility ([aebb3d4](https://github.com/meysam81/build-docker/commit/aebb3d43d48adf1c29b1697bab0fca4d998ededd))

## [1.1.0](https://github.com/meysam81/build-docker/compare/v1.0.0...v1.1.0) (2024-11-30)


### Features

* allow signing the image with cosign CLI ([36922ca](https://github.com/meysam81/build-docker/commit/36922ca196fa8014520df9332eda1f3834efcf54))


### Bug Fixes

* capture multiline image name ([91abcf8](https://github.com/meysam81/build-docker/commit/91abcf81b1042c26554372d8949fbbc42c2a40ea))
* capture the first image name if multiple are provided ([b8dd95d](https://github.com/meysam81/build-docker/commit/b8dd95ddf77a75b2316af236f73bb31633d67e85))
* pass the image name before the digest ([b115270](https://github.com/meysam81/build-docker/commit/b11527060bc36512925f661b766c59d639d851c6))
* set the env var for GH CLI ([2b99b05](https://github.com/meysam81/build-docker/commit/2b99b055a34815b11b0ba565c0e514bba7455c4f))
* use the official kubescape otherwise ([a6c4c66](https://github.com/meysam81/build-docker/commit/a6c4c66f42e3afdbc86ec0b935fa1ecc6bdec1fa))

## 1.0.0 (2024-11-07)


### Bug Fixes

* ignore upload sarif errors ([bc5def1](https://github.com/meysam81/build-docker/commit/bc5def19024203a7a6bc17138495df4ae4f5cdf4))
* provide registry credential for kubescape ([24ceebc](https://github.com/meysam81/build-docker/commit/24ceebcec860cfe36ee77d9cccf285b4f58db014))
