
# Docker Log Rotation

## Configuration in docker-compose.yml

```yaml
services:
  api:
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
    volumes:
      - ./logs:/usr/src/app/logs
```

## Why This Matters

- Prevents disk overflow
- Automatically rotates logs
- Keeps last 3 files (30MB total)
- Winston handles application logs, Docker handles container logs

## Check Log Disk Usage

```bash
docker system df
du -sh /var/lib/docker/containers/*
```

## Viewing Logs

```bash
# Live tail
tail -f logs/vigil-*.log

# Search for errors
grep -r "ERROR" logs/

# View today's logs
cat logs/vigil-$(date +%Y-%m-%d).log

# Docker logs still work
docker logs vigil_api
```

## Log Rotation Schedule

· Daily: vigil-2026-03-17.log
· After 20MB: New file
· After 14 days: Auto-deleted
· Old logs: Compressed to .gz

```

