# Challenge: Express 5 Breaking Changes

## Date
2026-03-17

## Problem
After rebuilding with `docker compose down -v`, the app crashed with:
```

Error: Cannot find module 'express'

```

But `node_modules` clearly existed! The modules were there but Node couldn't find them.

## Root Cause
Our `package.json` had been updated to Express 5.2.1, which has breaking changes in how middleware and routes are loaded . The code was written for Express 4 patterns.

## The Breaking Changes That Affected Us

| Express 4 Pattern | Express 5 Behavior | Impact |
|-------------------|-------------------|--------|
| `app.use(middleware)` | Different execution order | Silent failures |
| Route handling | Stricter parameter validation | 404 errors |
| Error middleware | Different signature | Uncaught errors |

## The Fix
```bash
# Downgrade to Express 4
npm install express@4.18.2

# Update package.json manually
```

```json
"express": "^4.18.2"
```

Verification

After downgrade, the module resolution worked and the app started normally.

Why Express 5 Caused Module Resolution Issues

Express 5 has a different internal structure that can confuse Node's module loader when mixed with Express 4-style code . The modules were physically present, but Node's require() couldn't find them because of how Express 5 sets up its exports.

Lessons Learned

1. Never auto-update dependencies in production
2. Pin all versions in package.json
3. Test major version upgrades in staging first
4. Express 4 is supported until October 2026—no rush to upgrade

Current Working Versions (as of March 2026)

```json
{
  "express": "4.18.2",
  "bcryptjs": "2.4.3",
  "jsonwebtoken": "9.0.2",
  "pg": "8.11.3",
  "winston": "3.11.0"
}