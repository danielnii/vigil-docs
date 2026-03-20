# Challenge: PostgreSQL GROUP BY Error in Fleet Analytics

## Date
2026-03-07

## Problem
Fleet analytics endpoint returned:
```
column "vp.revenue" must appear in the GROUP BY clause or be used in an aggregate function
```

## The Original (Broken) Query
```sql
(
    SELECT json_agg(vp) 
    FROM vehicle_performance vp 
    WHERE vp.revenue > 0
    ORDER BY vp.revenue DESC 
    LIMIT 3
) as top_performers
```

## Root Cause
In a subquery, `ORDER BY` requires the column to be in `GROUP BY` when using aggregation.

## Solution
Move `ORDER BY` inside `json_agg`:
```sql
(
    SELECT json_agg(vp ORDER BY vp.revenue DESC)
    FROM vehicle_performance vp 
    WHERE vp.revenue > 0
) as top_performers
```

Then limit in JavaScript:
```javascript
if (fleetData.top_performers && Array.isArray(fleetData.top_performers)) {
    fleetData.top_performers = fleetData.top_performers.slice(0, 3);
}
```

## Alternative Solution
Use two separate queries with `Promise.all`:
```javascript
const statsQuery = `SELECT ... FROM vehicles ...`;
const performersQuery = `SELECT ... ORDER BY revenue DESC LIMIT 3`;
const [statsResult, performersResult] = await Promise.all([
    db.query(statsQuery),
    db.query(performersQuery)
]);
```

## Lessons Learned
- When using `json_agg`, put `ORDER BY` inside, not outside
- Sometimes splitting a complex query into two is better
- `Promise.all` runs queries in parallel for better performance
```