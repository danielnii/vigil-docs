# Decision Record 002: Odometer Triangulation

## Date
2026-03-02

## Context
Drivers were potentially "shadow driving" (pocketing cash). We needed a way to verify revenue based on actual distance driven.

## Decision
We implemented "Cumulative Odometer Triangulation":
- Vehicles have a `base_odometer` (Day 0 reading)
- `current_odometer` updates after every shift
- Expected revenue = distance × REVENUE_PER_KM (2.5 GHS)
- Shifts with revenue < expected are automatically flagged

## Algorithm
```

distance = end_odometer - start_odometer
expectedRevenue = distance × 2.5
if (reportedRevenue < expectedRevenue) {
createAlert('REVENUE_DISCREPANCY')
flagShift()
}

```

## Consequences
- ✅ Catches revenue theft automatically
- ✅ Creates audit trail via alerts
- ❌ REVENUE_PER_KM is hardcoded (configurable in Phase 2)
