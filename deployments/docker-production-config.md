
## Docker Production Configuration

## Key Differences from Development

Feature Development Production
Node version 20 (or 22) 20 LTS
Dependencies All (including dev) Only production
Volumes Code mounted Code baked in
User root non-root (nodeuser)

## Dockerfile (Production)

```dockerfile
FROM node:20-slim
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN useradd -m -u 1001 nodeuser && chown -R nodeuser:nodeuser /usr/src/app
USER nodeuser
CMD ["node", "server.js"]
```

## Docker Compose (Production)

```yaml
services:
  api:
    build: .
    restart: unless-stopped
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
```

