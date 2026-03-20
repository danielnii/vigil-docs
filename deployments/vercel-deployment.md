
# Vercel Deployment Guide

## Step-by-Step

### 1. Push to GitHub

```bash
git push origin main
```

### 2. Connect to Vercel

- Go to vercel.com
- Import your frontend repo
- Add environment variable: VITE_API_URL=https://api.vigil247.app/api

### 3. Add vercel.json for Routing

```json
{
  "rewrites": [{ "source": "/(.*)", "destination": "/" }]
}
```

### 4. Deploy

Vercel auto-deploys on every push to main.
