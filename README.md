# Assembles-J Engineering

This repository stores shared GitHub configuration and reusable CI/CD workflows for repositories under the `Assembles-J` organization.

## Goals

- Keep CI/CD simple and reusable.
- Keep shared infrastructure secrets at organization level.
- Keep project-specific secrets inside each repository or environment.
- Avoid copying large workflow files between projects.

## Secret layers

| Layer | Naming | Examples |
| --- | --- | --- |
| Organization shared | `COMMON_*` | `COMMON_DOCKER_USERNAME`, `COMMON_DOCKER_TOKEN`, `COMMON_SERVER_HOST` |
| Repository project | `<PROJECT>_*` | `GROUPIM_JWT_SECRET`, `THIRD_HAND_AI_KEY`, `EV_BOOK_API_KEY` |
| Environment | Same semantic name, scoped by environment | production/staging database credentials |

See [`docs/SECRET_MANAGEMENT.md`](docs/SECRET_MANAGEMENT.md).

## Reusable workflows

- `android-build.yml` - Gradle/Android build baseline.
- `docker-build.yml` - Docker image build and optional push.
- `server-deploy.yml` - Minimal SSH deployment runner.

Repositories should call these workflows instead of copying their implementation.
