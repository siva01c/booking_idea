# Docker Runtime

## Source of Truth

Primary compose file:

- [docker-compose.yml](/home/siva01/projects/booking/cazua/docker-compose.yml)

Additional Docker assets:

- `docker/docker-compose.yml`
- `docker/docker-compose.production.yml`
- `frontend/Dockerfile.dev`
- `frontend/Dockerfile.prod`
- `backend/Dockerfile.dev`
- `backend/Dockerfile.prod`
- `docker/nginx/Dockerfile.dev`
- `docker/nginx/Dockerfile.prod`

## Main Compose Services

## `backend`

Purpose:

- runs the Express API in production mode

Important characteristics:

- built from `./backend`
- depends on `mongodb` health
- joins external network `nginx-proxy`
- mounts `./backend/config` read-only into `/app/config`
- mounts logs into `/app/logs`
- exposes health endpoint at `/api/health`

Environment concerns:

- JWT and bcrypt settings
- admin email bootstrap
- Google OAuth client ID
- MongoDB URL
- Redis URL and password
- SMTP credentials
- site identity
- logging and rate limiting

## `frontend-builder`

Purpose:

- builds the frontend once and copies static files into a Docker volume

Characteristics:

- built from `./frontend`
- receives Vite build arguments
- writes generated frontend to `frontend_dist`

This separates building from serving.

## `frontend`

Purpose:

- serves the built SPA via `nginx:1.27-alpine`

Characteristics:

- depends on `frontend-builder`
- mounts `frontend_dist` as read-only web root
- mounts custom `frontend/nginx.conf`
- participates in reverse-proxy and TLS environment conventions

## `redirect`

Purpose:

- optional hostname redirect service

Characteristics:

- enabled through Docker profile `redirect`
- uses `docker/nginx/redirect.conf`

## `mongodb`

Purpose:

- stores users, bookings, and token blacklist/session data

Characteristics:

- `mongo:7.0`
- auth enabled
- persistent bind-mounted data volume
- initialization scripts from `docker/mongo-init`

## `redis`

Purpose:

- intended for cache/session support

Characteristics:

- `redis:7-alpine`
- password protected
- LRU memory policy
- persistent bind-mounted data volume

Important note:

The compose setup provisions Redis, but the currently mounted backend `index.ts` does not wire Redis into the active request path. The backend currently uses an in-memory cache middleware and Mongo token services.

## Networks and Volumes

External network:

- `nginx-proxy`

Volumes:

- `frontend_dist`
- `mongodb_data`
- `mongodb_config`
- `redis_data`

Persistent bind paths default to:

- `./data/mongodb`
- `./data/redis`

## Runtime Flow

1. MongoDB becomes healthy.
2. Backend starts and connects to MongoDB.
3. Frontend builder compiles the SPA and copies files into `frontend_dist`.
4. Frontend Nginx serves the generated files.
5. Optional reverse proxy and TLS components route public traffic.

## Docker Assumptions for Rebuild

- frontend and backend should not be run directly on the host in the intended deployment model
- configuration should enter containers through environment variables plus mounted YAML files
- frontend build artifacts should be immutable static assets
- backend should remain stateless apart from logs
- MongoDB and Redis should own persistence concerns

## Configuration Files That Matter

- `.env`
- `.env.example`
- `.env.local.example`
- `docker/.env`
- `docker/.env.production`
- backend config YAML files under `backend/config/`

## Rebuild Guidance

If rebuilding, preserve these Docker-level concerns:

- separate build and serve stages for the frontend
- health checks for backend and MongoDB
- named persistent volumes or bind mounts for stateful services
- externally attachable reverse-proxy network
- read-only mounted configuration for backend YAML
