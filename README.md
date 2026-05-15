[![Publish Docker image](../../actions/workflows/docker.yml/badge.svg)](../../actions/workflows/docker.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-purple.svg)](https://opensource.org/licenses/MIT)

# :rocket: GitHub Actions Publish Docker Image

GitHub Actions builds and publishes Docker images with multi-platform support and multiple registry authentication.

This workflow automates Docker image builds using Buildx, supports caching, SBOM generation, provenance attestation, and publishing to multiple registries (Docker Hub, GHCR, and private registries).

# :package: Features

- :whale: Build and push Docker images automatically
- :flying_saucer: Multi-platform support (linux/amd64, linux/arm64, etc.)
- :closed_lock_with_key: Multiple registry authentication (Docker Hub, GHCR, private registries)
- :bookmark: Automatic tagging (branch, git tag, SHA, semantic versioning, latest)
- :zap: GitHub Actions cache support for faster builds
- :page_facing_up: SBOM (Software Bill of Materials) generation for dependency tracking
- :shield: Provenance attestation for supply chain security
- :recycle: Concurrency control to avoid parallel build conflicts

# :wrench: Configuration

## :gear: Variables

| Variable                 | Description                | Example                 |
| ------------------------ | -------------------------- | ----------------------- |
| DOCKER_IMAGES            | Docker image name(s)       | myorg/myapp             |
| DOCKER_USERNAME          | Docker Hub username        | myuser                  |
| DOCKER_PLATFORMS         | Target platforms for build | linux/amd64,linux/arm64 |
| USE_GITHUB_REGISTRY      | Enable GHCR publishing     | true / false            |
| DOCKER_REGISTRY          | Custom registry URL        | myregistry.com          |
| DOCKER_REGISTRY_USERNAME | Custom registry username   | myuser                  |
| BUILDX_CONFIG            | BuildKit configuration     |                         |

## :key: Secrets

| Secret                | Description                      |
| --------------------- | -------------------------------- |
| DOCKER_TOKEN          | Docker Hub access token          |
| DOCKER_REGISTRY_TOKEN | Private registry token           |

## :closed_lock_with_key: Registry Authentication

### :whale: Docker Hub

Used when:

- DOCKER_USERNAME is set
- DOCKER_TOKEN secret is provided

### :octocat: GitHub Container Registry (GHCR)

Automatically generate a Docker image: `ghcr.io/repository_owner/repository_name`

Enabled when:

- USE_GITHUB_REGISTRY = true

### :space_invader: Private registry

Used when:

- DOCKER_REGISTRY is set
- DOCKER_REGISTRY_USERNAME is set
- DOCKER_REGISTRY_TOKEN secret is provided

## :flying_saucer: Multi-platform builds

Enable multi-platform builds by setting the DOCKER_PLATFORMS environment variable.

This variable defines the list of target platforms used during the build process (e.g. linux/amd64, linux/arm64, linux/riscv64, linux/ppc64le, linux/s390x, etc.).

Example:

```bash
DOCKER_PLATFORMS=linux/amd64,linux/arm64
```

## :hammer: BuildKit configuration (optional)

Configure Docker BuildKit via BUILDX_CONFIG to customize registry access and build behavior.

Example:
This allows you to define registry settings such as enabling plain HTTP or marking a registry as insecure (useful for private or local registries without valid TLS certificates).

```ini
[registry."myregistry.com:5000"]
  http = true
  insecure = true
```

# :rocket: Workflow Triggers

This workflow runs when:

- push to main or master
- push of any git tag
- manual trigger via GitHub Actions UI

# :bookmark: Automatic Tagging

The workflow generates docker images tags automatically:

- sha-abcdefg (commit hash)
- branch name (main, develop, etc.)
- git tag (v1.0.0)
- latest (only on default branch)
- semantic versioning:
  - 1.2.3
  - 1.2

# :test_tube: Example Setup

## :whale: Docker Hub only

### :gear: Variables & Secret

```dotenv
DOCKER_IMAGES=myorg/myapp
DOCKER_USERNAME=myuser
DOCKER_TOKEN=xxxxxx
```

### :bar_chart: Output

```text
docker.io/myorg/myapp
```

## :whale: Docker Hub + :octocat: GitHub Registry + :space_invader: Private registry

### :gear: Variables & Secret

```dotenv
DOCKER_IMAGES=
  myorg/myapp
  myregistry.com/myapp
DOCKER_USERNAME=myuser
DOCKER_TOKEN=xxxxxx
USE_GITHUB_REGISTRY=true
DOCKER_REGISTRY=myregistry.com
DOCKER_REGISTRY_USERNAME=myuser
DOCKER_REGISTRY_TOKEN=xxxxxx
```

### :bar_chart: Output

```text
docker.io/myorg/myapp
myregistry.com/myapp
ghcr.io/repository_owner/repository_name
```

# :hearts: Show your support

Give a :star: star if this project helped you !

# :paperclip: Contribute

Your contribution is always welcome!
Please read [contribution guideline](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

<!--
## Acknowledgments
- [contributors](url)
- [contributors](url)
-->

# :closed_book: License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
