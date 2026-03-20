# Decision Record 005: React Query over Redux

## Date
2026-03-08

## Context
We needed to manage server state (vehicles, shifts, alerts) efficiently without excessive boilerplate.

## Decision
We chose React Query + Context API over Redux:
- **React Query**: Server state (caching, background updates, mutations)
- **Context API**: Auth state (user, role, token)
- **useState/useReducer**: UI state (forms, modals)

## Comparison
| Feature | Redux | React Query + Context |
|---------|-------|----------------------|
| Boilerplate | 50+ lines per feature | 10-15 lines |
| API Caching | Manual | Automatic |
| Loading states | Manual flags | Built-in |
| Bundle size | ~15KB | ~8KB |

## Consequences
- Faster development with less code
- Automatic background refetching
- Simplified error handling
- Easy cache invalidation after mutations
