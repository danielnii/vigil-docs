# Challenge: Docker Volume Init.sql Directory Issue

## Date
2026-03-06

## Problem
When running `docker compose up`, PostgreSQL failed to initialize with error:
```
psql:/docker-entrypoint-initdb.d/init.sql: error: could not read from input file: Is a directory
```

## Root Cause
Docker automatically creates missing host paths as **directories**, not files. Our volume mount was:
```yaml
volumes:
  - ./init.sql:/docker-entrypoint-initdb.d/init.sql
```
Since `init.sql` didn't exist on the host, Docker created it as a directory.

## The Debugging Journey
```bash
# What we saw
ls -la
# drwxr-xr-x 2 root root 4096 Mar 6 10:00 init.sql

# What we expected
# -rw-r--r-- 1 root root 2048 Mar 6 10:00 init.sql
```

## Solution
1. Delete the auto-created directory:
   ```bash
   rm -rf init.sql
   ```
2. Create the actual file from your seed data:
   ```bash
   cp seed.sql init.sql
   ```
3. Ensure it contains both schema and seed data
4. Rebuild:
   ```bash
   docker compose down -v
   docker compose up -d
   ```

## Prevention
Always create files **before** mounting them as volumes in Docker:
```bash
touch init.sql
docker compose up
```

## Lesson Learned
> "Never let Docker create your volume mount paths. Create them yourself first."
```