# Challenge: CORS Preflight Hell & Path-to-Regexp Error

## Date
2026-03-15

## Problem
After deploying, the browser console showed:
```

Access to XMLHttpRequest at 'https://api.vigil247.app/api/auth/login'
from origin 'https://www.vigil247.app' has been blocked by CORS policy

```

And the backend logs showed:
```

PathError [TypeError]: Missing parameter name at index 1: *

```

## Root Cause
Two separate issues:
1. **Express Route Parsing**: `app.options('*', ...)` was being interpreted as a route parameter, not a wildcard
2. **CORS Headers**: Preflight OPTIONS requests weren't getting proper headers

## The Debugging Journey
```javascript
// What we TRIED first (BROKEN):
app.options('*', cors(corsOptions));  // path-to-regexp crashed!

// What we tried second (STILL BROKEN):
app.options('*', (req, res) => {
  // manual headers
});  // Same error!

// What actually worked (MIDDLEWARE approach):
app.use((req, res, next) => {
  if (req.method === 'OPTIONS') {
    const origin = req.headers.origin;
    if (allowedOrigins.includes(origin)) {
      res.header('Access-Control-Allow-Origin', origin);
      res.header('Access-Control-Allow-Methods', 'GET,POST,PUT,PATCH,DELETE,OPTIONS');
      res.header('Access-Control-Allow-Headers', 'Content-Type,Authorization');
      res.header('Access-Control-Allow-Credentials', 'true');
    }
    return res.sendStatus(200);
  }
  next();
});
```

Why It Worked

Middleware runs before Express's route parser, so the wildcard * never triggers the path-to-regexp error.

Lessons Learned

· NEVER use route handlers for OPTIONS requests
· ALWAYS use middleware for preflight
· path-to-regexp in Express v5+ is strict about wildcards

Prevention

Add this pattern to every new Express app:

```javascript
// Handle OPTIONS at middleware level, not route level
app.use((req, res, next) => {
  if (req.method === 'OPTIONS') {
    // Set CORS headers
    return res.sendStatus(200);
  }
  next();
});