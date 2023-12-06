# Docker Build Workflow

This GitHub Actions workflow is designed to build and push Docker images. It is a reusable workflow that can be called from other workflows in your GitHub repository.

## Inputs

| Input | Required | Type | Default | Description |
|-------|----------|------|---------|-------------|
| `tag` | Yes | string | - | The tag for the Docker image. |
| `push` | Yes | boolean | - | A boolean indicating whether to push the image to Docker registries. |
| `context` | No | string | '.' | The context path for Docker build. |
| `build_args` | No | string | - | Any build arguments to pass to Docker build command. |
| `platforms` | No | string | 'linux/amd64,linux/arm64,linux/arm/v7' | The platforms for which the Docker image should be built. |
| `file` | No | string | 'Dockerfile' | The path to the Dockerfile. |

## Secrets

- `DOCKER_USERNAME` (required): The username for DockerHub.
- `DOCKER_PASSWORD` (required): The password for DockerHub.
- `GH_TOKEN` (required): The GitHub token for authentication with GitHub Container Registry.

## Jobs

- `build`: This job runs on the latest version of Ubuntu. It checks out the code, sets up Docker Buildx, logs in to DockerHub and GitHub Container Registry, and then builds and pushes the Docker images.

## Usage

To use this workflow in your own repository, you can call it from another workflow file with the `workflow_call` event. Here's an example:

```yaml
name: My Workflow

on:
  push:
    branches:
      - main

jobs:
  call-workflow:
    uses: jvrck/workflows/.github/workflows/docker-build.yml@main
    secrets:
      DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
      DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
      GH_TOKEN: ${{ secrets.GH_TOKEN }}
    with:
      tag: my-image:latest
      push: true