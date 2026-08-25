# Secret Management

## Principles

1. Shared infrastructure credentials belong at organization scope.
2. Business/project credentials belong at repository scope.
3. Production/staging credentials should be isolated with GitHub Environments when the same repository deploys to multiple environments.
4. Never commit real secrets, `.env` files, signing material, tokens, or private keys.
5. Prefer the smallest secret scope that still avoids duplication.

## Naming convention

### Organization secrets

Use the `COMMON_` prefix for credentials intentionally shared by multiple repositories.

Examples:

- `COMMON_DOCKER_USERNAME`
- `COMMON_DOCKER_TOKEN`
- `COMMON_SERVER_HOST`
- `COMMON_SERVER_USER`
- `COMMON_SSH_PRIVATE_KEY`

Do not place application passwords such as PostgreSQL, Redis, JWT, API-provider keys, or Android signing passwords here unless they are intentionally shared.

### Repository secrets

Prefix project-specific secrets with a stable project identifier.

Examples:

- Group-IM: `GROUPIM_*`
- Third-Hand: `THIRD_HAND_*`
- EV-Charge-Book: `EV_BOOK_*`

Examples:

- `GROUPIM_POSTGRES_PASSWORD`
- `GROUPIM_REDIS_PASSWORD`
- `GROUPIM_JWT_SECRET`
- `THIRD_HAND_AI_KEY`
- `THIRD_HAND_DATABASE_PASSWORD`
- `EV_BOOK_API_KEY`

### Environment secrets

Use GitHub Environments for values that differ by deployment target, such as `staging` and `production`.

Typical environment-scoped values:

- server host
- database URL/password
- deployment path
- production-only third-party credentials

## Recommended ownership

| Secret type | Scope |
| --- | --- |
| Docker registry account shared across projects | Organization |
| Shared deployment SSH identity | Organization |
| Project database password | Repository or Environment |
| Project JWT secret | Repository or Environment |
| Third-party API key used by one project | Repository or Environment |
| Android signing key/password | Repository or Environment |

## GitHub Actions usage

Organization and repository secrets are referenced the same way inside a workflow:

```yaml
${{ secrets.COMMON_DOCKER_USERNAME }}
${{ secrets.GROUPIM_JWT_SECRET }}
```

Reusable workflows do not automatically receive every caller secret. Prefer explicit secret contracts in reusable workflows, or use `secrets: inherit` only when the caller and called workflow are in the same trusted organization and broad inheritance is intentional.

## Repository files

Repositories may commit an `.env.example` containing variable names and safe placeholders:

```dotenv
POSTGRES_PASSWORD=
REDIS_PASSWORD=
JWT_SECRET=
```

Real `.env` files must remain ignored.

## Initial migration order

1. Create shared `COMMON_*` organization secrets.
2. Migrate one mature repository first (Group-IM is the recommended pilot).
3. Verify CI/build/deploy behavior.
4. Migrate Third-Hand.
5. Migrate EV-Charge-Book.
6. Only add new shared abstractions after two or more projects actually need them.
