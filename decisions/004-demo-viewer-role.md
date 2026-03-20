# Decision Record 004: Demo Viewer Role

## Date
2026-03-09

## Context
We needed a way for recruiters and beta testers to explore the full admin dashboard without being able to modify data.

## Decision
We added a `viewer` role that:
- Can access all admin routes (same as admin)
- Sees all data (vehicles, alerts, analytics)
- Has ALL action buttons disabled/grayed out
- Shows a blue "Demo Mode" banner
- Cannot POST, PUT, PATCH, or DELETE (enforced in backend)

## Benefits
- Recruiters see full functionality
- Zero risk of data corruption
- No complex reset logic needed
- Professional "enterprise demo" feel
