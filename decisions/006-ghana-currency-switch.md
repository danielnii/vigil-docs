# Decision Record 006: Ghana Currency Switch (KES → GHS)

## Date
2026-03-09

## Context
We initially built the app assuming Kenyan Shillings (KES), but Addo operates in Ghana. The revenue calculations needed to reflect Ghana Cedis (GHS).

## Decision
Changed all currency displays and hardcoded values from KES to GHS:

```javascript
// BEFORE
const REVENUE_PER_KM = 2.5; // KES
flaggedReason = `... @ KES ${REVENUE_PER_KM}/km`;

// AFTER
const REVENUE_PER_KM = 2.5; // GHS
flaggedReason = `... @ GHS ${REVENUE_PER_KM}/km`;
```

Why 2.5 GHS per km?

Based on Ghana taxi economics:

· Average fare: 8-10 GHS/km
· Minus fuel: 1.5-2 GHS/km
· Minus driver commission: 20-30%
· Minus maintenance: 0.5 GHS/km
· Minimum expected revenue: 2.5 GHS/km

Future Enhancement

Phase 2 will make this configurable:

```javascript
const fleetSettings = {
    "addo_ghana": {
        currency: "GHS",
        revenue_per_km: 2.5,
        fuel_cost_per_km: 1.2
    },
    "addo_kenya": {
        currency: "KES", 
        revenue_per_km: 3.5,
        fuel_cost_per_km: 1.5
    }
};
```

Consequences

· ✅ Correct currency for target market
· ✅ No code changes needed (just display strings)
· ❌ Hardcoded value still needs Phase 2 config

```