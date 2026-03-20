# Decision Record 003: Docker Security Architecture

## Date
2026-03-05

## Context
We needed to deploy to production while hiding the database from the public internet.

## Decision
We chose a Docker Compose setup with:
- Database container: No ports exposed to host
- API container: Binds to localhost only (127.0.0.1:8000)
- Internal Docker network for container communication
- Health checks for both services

## Security Benefits
- Database port 5432 is COMPLETELY invisible (not even on host)
- API only accessible via localhost
- No direct container access from outside
- Data persists via volumes, not exposed
