# Reusable Workflow Usage

The shared workflows in this repository are intentionally small. Project repositories keep only thin caller workflows and their own business-specific configuration.

## Android

```yaml
name: Android CI

on:
  pull_request:
  push:
    branches: [main]

jobs:
  android:
    uses: Assembles-J/.github/.github/workflows/android-build.yml@main
    with:
      java-version: '21'
      gradle-task: assembleDebug
```

## Docker build and push

```yaml
name: Docker

on:
  push:
    branches: [main]

jobs:
  docker:
    uses: Assembles-J/.github/.github/workflows/docker-build.yml@main
    with:
      image: your-registry/your-image
      tag: latest
      push: true
    secrets:
      registry-username: ${{ secrets.COMMON_DOCKER_USERNAME }}
      registry-token: ${{ secrets.COMMON_DOCKER_TOKEN }}
```

## Server deploy

```yaml
name: Deploy

on:
  workflow_dispatch:

jobs:
  deploy:
    uses: Assembles-J/.github/.github/workflows/server-deploy.yml@main
    with:
      command: cd /opt/app && docker compose pull && docker compose up -d
    secrets:
      server-host: ${{ secrets.COMMON_SERVER_HOST }}
      server-user: ${{ secrets.COMMON_SERVER_USER }}
      ssh-private-key: ${{ secrets.COMMON_SSH_PRIVATE_KEY }}
```

## Project migration rule

Do not rewrite a project's CI merely to make it look uniform. During migration:

1. Preserve the existing build/deploy behavior.
2. Replace only duplicated generic steps with a reusable workflow.
3. Keep project-specific steps in the project repository.
4. Verify the migrated workflow before removing the old workflow.
