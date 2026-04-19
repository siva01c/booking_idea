# Environment and Secrets Contract

This document defines mandatory configuration rules for any booking-system implementation generated from this repository.

## Core Principles

1. No hardcoded secrets in code, tests, examples, docker files, or docs.
2. All sensitive values must be injected through environment variables or a dedicated secrets manager.
3. Configuration must fail fast on startup if required sensitive values are missing.
4. Production and non-production secrets must be isolated.

## Configuration Source Precedence

Highest priority first:

1. Runtime environment variables
2. Secret manager mounted/injected values
3. Non-sensitive config files (YAML/JSON)
4. Safe defaults (non-sensitive only)

## Sensitive vs Non-Sensitive

### Sensitive (MUST be secret)

- JWT signing keys
- OAuth client secrets
- SMTP credentials
- Database passwords/connection secrets
- Encryption keys
- API keys and webhook signing secrets

### Non-sensitive (MAY be plain config)

- Public site name
- Feature flags with no security implications
- Display texts and localization defaults
- Non-secret service hostnames

## Required Environment Variables (Template)

> Exact names can vary by language stack; categories are mandatory.

### Authentication

- `AUTH_JWT_SECRET` (required, secret)
- `AUTH_JWT_EXPIRES_IN` (required)
- `AUTH_BCRYPT_ROUNDS` (required)

### OAuth

- `OAUTH_GOOGLE_CLIENT_ID` (required)
- `OAUTH_GOOGLE_CLIENT_SECRET` (required, secret if backend flow requires it)

### Database

- `DB_URI` (required, secret)
- `DB_NAME` (required)

### Cache/Session

- `REDIS_URI` (required if redis enabled)
- `REDIS_PASSWORD` (required if redis password is enabled, secret)

### Email

- `SMTP_HOST` (required)
- `SMTP_PORT` (required)
- `SMTP_USER` (required, secret)
- `SMTP_PASSWORD` (required, secret)
- `SMTP_FROM` (required)

### Security

- `RATE_LIMIT_WINDOW_MS` (required)
- `RATE_LIMIT_MAX` (required)
- `CORS_ALLOWED_ORIGINS` (required)

### Runtime

- `NODE_ENV` or equivalent runtime environment marker (required)
- `APP_BASE_URL` (required)
- `API_BASE_URL` (required)

## Hardcoding Prohibition Rules

The following are forbidden:

- API keys or credentials committed to git.
- Tokens in tests that represent real secrets.
- Credentials in sample docker-compose files.
- Passwords in markdown except obvious placeholders.

Allowed:

- Placeholder values like `CHANGEME` or `<REQUIRED_SECRET>` in examples.

## Secret Rotation Requirements

- Rotation cadence: at least every 90 days for high-impact secrets.
- Immediate rotation after exposure suspicion.
- Emergency runbook must exist and be tested.

## CI/CD Enforcement

CI pipeline must include:

1. Secret scanning on every push/PR.
2. Policy check that required env vars are declared.
3. Startup validation test proving fail-fast behavior.
4. Config drift check for production deployment manifests.

## Acceptance Checklist

- [ ] No hardcoded secrets in repository.
- [ ] Required env vars documented and validated.
- [ ] Secret rotation policy documented.
- [ ] CI security/config checks enabled.
