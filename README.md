# build-docker

[![GitHub Release](https://img.shields.io/github/v/release/meysam81/build-docker)](https://github.com/meysam81/build-docker/releases/latest)
[![Renovate](https://img.shields.io/badge/renovate-enabled-brightgreen.svg)](https://developer.mend.io/github/meysam81/build-docker)
[![GitHub last commit](https://img.shields.io/github/last-commit/meysam81/build-docker)](https://github.com/meysam81/build-docker/commits/main)
[![License](https://img.shields.io/github/license/meysam81/build-docker)](https://github.com/meysam81/build-docker/blob/main/LICENSE)
[![GitHub issues](https://img.shields.io/github/issues/meysam81/build-docker)](https://github.com/meysam81/build-docker/issues)
[![GitHub pull requests](https://img.shields.io/github/issues-pr/meysam81/build-docker)](https://github.com/meysam81/build-docker/pulls)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Features](#features)
- [Usage](#usage)
- [Inputs](#inputs)
- [Outputs](#outputs)
- [Example](#example)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

A GitHub composite action that simplifies Docker image building, pushing, and scanning. It combines Docker build, push, metadata handling, Scout scanning, and Kubescape security analysis into a single action.

## Features

- Multi-platform builds (amd64/arm64)
- GitHub Container Registry integration
- Docker Hub support
- Docker Scout vulnerability scanning
- Kubescape security analysis
- GitHub cache optimization
- SARIF report generation

## Usage

```yaml
- uses: meysam81/build-docker@e1a24e3e00108b76f3d174ed4b723881f97c1b91 # v1.7.0
  with:
    image-name: ghcr.io/${{ github.repository }} # e.g. ghcr.io/octocat/hello-world
```

Pin to a full commit SHA with the version in a trailing comment, as above;
Renovate and Dependabot both keep that form current. `@v1` also works but the
tag moves with every release, so what runs can change under you.

## Inputs

| Name                     | Description                                                                                      | Required | Default                    |
| ------------------------ | ------------------------------------------------------------------------------------------------ | -------- | -------------------------- |
| `image-name`             | Image name (including registry path); several lines are allowed, the first is signed and scanned | Yes      | -                          |
| `github-token`           | GitHub token for authentication                                                                  | Yes      | `${{ github.token }}`      |
| `build-args`             | Docker build arguments                                                                           | No       | `""`                       |
| `cache-from`             | Build cache source                                                                               | No       | `type=gha`                 |
| `cache-to`               | Build cache destination                                                                          | No       | `type=gha,mode=max`        |
| `checkout`               | Check out the repository first; `false` when an earlier step already did                         | No       | `true`                     |
| `context`                | Build context path                                                                               | No       | `.`                        |
| `cosign`                 | Sign the image with Cosign (installs the CLI)                                                    | No       | `false`                    |
| `dockerfile`             | Dockerfile path; empty uses `Dockerfile` in the context                                          | No       | `""`                       |
| `dockerhub-username`     | Docker Hub username (required when `scout-cves` or `scout-compare` is enabled)                   | No       | `""`                       |
| `dockerhub-password`     | Docker Hub password (required when `scout-cves` or `scout-compare` is enabled)                   | No       | `""`                       |
| `image-extra-tags`       | Additional image tags                                                                            | No       | `""`                       |
| `image-target`           | Multi-stage build target                                                                         | No       | `""`                       |
| `kubescape`              | Enable Kubescape scanning                                                                        | No       | `false`                    |
| `kubescape-upload-sarif` | Upload Kubescape SARIF results to GitHub Security (requires `security-events: write`)            | No       | `false`                    |
| `metadata-tags`          | Tag rules passed to `docker/metadata-action`                                                     | No       | `""`                       |
| `persist-credentials`    | Keep the token in the checkout's git config; `false` unless a later step pushes                  | No       | `true`                     |
| `platforms`              | Build platforms                                                                                  | No       | `linux/amd64,linux/arm64`  |
| `pre-build-commands`     | Bash commands run before the build, as written (only pass trusted text)                          | No       | `""`                       |
| `provenance`             | Provenance attestation for the build, e.g. `mode=max`; empty keeps the default                   | No       | `""`                       |
| `pull`                   | Always pull the base image                                                                       | No       | `false`                    |
| `push`                   | Push to registry (requires `packages: write`)                                                    | No       | `true`                     |
| `ref`                    | Git ref to check out; empty checks out the event's default ref                                   | No       | `""`                       |
| `registry-username`      | Registry username Kubescape uses to pull the image                                               | No       | `""`                       |
| `registry-password`      | Registry password Kubescape uses to pull the image                                               | No       | `""`                       |
| `repository`             | Repository to check out                                                                          | No       | `${{ github.repository }}` |
| `runner`                 | Declared for compatibility; the job's `runs-on` decides where this runs                          | No       | `ubuntu-latest`            |
| `sbom`                   | Attach an SBOM attestation to the image                                                          | No       | `false`                    |
| `scout-comment-pr`       | Add Scout results as PR comment (only in pull requests)                                          | No       | `false`                    |
| `scout-compare`          | Enable Docker Scout comparison                                                                   | No       | `false`                    |
| `scout-cves`             | Enable Docker Scout CVE scanning                                                                 | No       | `false`                    |
| `submodules`             | Submodules to check out (`true`, `recursive`); empty skips them                                  | No       | `""`                       |

Every action this one uses is pinned to a commit SHA, and the QEMU (`tonistiigi/binfmt`) and BuildKit (`moby/buildkit`) helper images are pinned by digest; Renovate moves the pins.

## Outputs

| Name         | Description                                   |
| ------------ | --------------------------------------------- |
| `digest`     | Image content-addressable digest              |
| `image-name` | The first `image-name` line, trimmed (signed) |
| `imageid`    | Built image ID                                |
| `metadata`   | Build result metadata                         |

## Example

```yaml
name: ci

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: meysam81/build-docker@e1a24e3e00108b76f3d174ed4b723881f97c1b91 # v1.7.0
        with:
          image-name: ghcr.io/${{ github.repository }}
          image-extra-tags: |
            ghcr.io/${{ github.repository }}:${{ github.run_id }}
          kubescape: true
          kubescape-upload-sarif: true
```
