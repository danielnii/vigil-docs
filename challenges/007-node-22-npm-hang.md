# Challenge: Node 22 + npm 10 Docker Hang

## Date
2026-03-17

## Problem
Docker build would hang forever at `RUN npm ci --omit=dev`:
```

npm error Exit handler never called!

```

## Symptoms
- Build would run for 7+ minutes then fail
- No error message, just timeout
- Works fine on local machine, fails in Docker
- Different Node versions behave differently

## Root Cause
Node 22.x + npm 10.x has a known bug with Docker's networking layer when both IPv4 and IPv6 are enabled . The `--no-network-family-autoselection` flag wasn't enough—the bug is deeper.

## The Debugging Journey

### Attempt 1: Network Flags (FAILED)
```dockerfile
ENV NODE_OPTIONS="--no-network-family-autoselection"
```

Still hung.

Attempt 2: npm Config Tweaks (FAILED)

```dockerfile
RUN npm config set fetch-retry-mintimeout 20000 && \
    npm config set fetch-retry-maxtimeout 120000
```

Still hung.

Attempt 3: The Nuclear Option (WORKED)

```dockerfile
# Switch from Node 22 to Node 20 LTS
FROM node:20-slim
```

Why Node 20 Worked

Node 20.x uses npm 9.x, which doesn't have the Docker networking bug. The code is identical, the dependencies are the same—only the Node/npm version changed.

The Clean Build Process That Finally Worked

```bash
# Complete cleanup
docker compose down -v
docker system prune -a -f
rm -rf node_modules package-lock.json

# Update package.json to Express 4 (just in case)
npm install express@4.18.2

# Rebuild with Node 20
docker compose build --no-cache
docker compose up -d
```

Lessons Learned

1. Node 22 in Docker is risky (as of March 2026)
2. Node 20 LTS is the safe choice for production
3. When npm hangs, check the Node version first
4. package-lock.json must exist for npm ci to work

Prevention

Pin your Node version in Dockerfile:

```dockerfile
FROM node:20-slim  # Not node:latest, not node:22