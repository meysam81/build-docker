# build-docker

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [build-docker](#build-docker)
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
- uses: meysam81/build-docker@v1
  with:
    image-name: ghcr.io/${{ github.repository }} # e.g. ghrc.io/octocat/hello-world
```

## Inputs

| Name                     | Description                          | Required | Default                   |
| ------------------------ | ------------------------------------ | -------- | ------------------------- |
| `image-name`             | Image name (including registry path) | Yes      | -                         |
| `github-token`           | GitHub token for authentication      | Yes      | `${{ github.token }}`     |
| `build-args`             | Docker build arguments               | No       | `""`                      |
| `context`                | Build context path                   | No       | `.`                       |
| `dockerhub-username`     | Docker Hub username                  | No       | `""`                      |
| `dockerhub-password`     | Docker Hub password                  | No       | `""`                      |
| `image-extra-tags`       | Additional image tags                | No       | `""`                      |
| `image-target`           | Multi-stage build target             | No       | `""`                      |
| `platforms`              | Build platforms                      | No       | `linux/amd64,linux/arm64` |
| `push`                   | Push to registry                     | No       | `true`                    |
| `ref`                    | Git ref to checkout                  | No       | `${{ github.ref }}`       |
| `runner`                 | GitHub runner to use                 | No       | `ubuntu-latest`           |
| `scout-compare`          | Enable Docker Scout comparison       | No       | `false`                   |
| `scout-comment-pr`       | Add Scout results as PR comment      | No       | `false`                   |
| `scout-cves`             | Enable Docker Scout CVE scanning     | No       | `false`                   |
| `kubescape`              | Enable Kubescape scanning            | No       | `false`                   |
| `kubescape-upload-sarif` | Upload Kubescape SARIF results       | No       | `false`                   |

## Outputs

| Name       | Description                      |
| ---------- | -------------------------------- |
| `digest`   | Image content-addressable digest |
| `imageid`  | Built image ID                   |
| `metadata` | Build result metadata            |

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
      - uses: meysam81/build-docker@v1
        with:
          image-name: ghcr.io/${{ github.repository }}
          kubescape: true
          kubescape-upload-sarif: true
```
