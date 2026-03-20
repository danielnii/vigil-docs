# Challenge: Vercel Deployment White Screen

## Date
2026-03-11

## Problem
After deploying to Vercel, the app showed a white screen. Console showed:
```
Failed to load resource: the server responded with a status of 404
```
React never mounted to `#root`.

## Root Cause
The `index.html` contained a manual React Refresh script pointing to `localhost`:
```html
<script type="module">
  import RefreshRuntime from 'http://localhost:5174/@react-refresh'
  RefreshRuntime.injectIntoGlobalHook(window)
  window.$RefreshReg$ = () => {}
  window.$RefreshSig$ = () => (type) => type
  window.__vite_plugin_react_preamble_installed__ = true
</script>
```

This script only works in development. In production, it failed and prevented React from mounting.

## Solution
1. Remove the manual React Refresh script entirely
2. Keep only essential HTML:
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vigil.jpeg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vigil · The Digital Eye</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```
3. Add `vercel.json` for React Router:
```json
{
  "rewrites": [{ "source": "/(.*)", "destination": "/" }]
}
```

## Prevention
- Test production build locally before deploying:
  ```bash
  npm run build
  npm run preview
  ```
- Never commit `index.html` with development-only scripts

## Lesson Learned
> "Never include development-only scripts in production builds. Let Vite handle this automatically."
```