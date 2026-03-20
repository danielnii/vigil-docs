# Decision Record 001: Flat MVC Pattern

## Date
2026-03-01

## Context
We needed to build a fleet management MVP in 7 days. Speed and direct debugging were priorities over abstraction.

## Decision
We chose a "flattened" MVC pattern with:
- No service layer
- No DTOs
- Direct SQL queries in controllers
- Hardcoded business logic (REVENUE_PER_KM = 2.5)

## Consequences
**Positive:**
- Faster development (no abstraction layers to navigate)
- Easier debugging (logic is where you expect it)
- Direct visibility into database queries

**Negative:**
- Less reusable code
- Harder to unit test in isolation
- More duplication across controllers

## When to Revisit
If the app grows to 50+ endpoints, consider adding a service layer.
