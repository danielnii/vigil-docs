# Challenge: Postman Test Automation with Collection Variables

## Date
2026-03-10

## Problem
Running the 10-step lifecycle test in Postman Runner would fail because:
1. Variables didn't persist between requests
2. Test order got mixed up
3. Duplicate vehicle registration errors

## The Solution: Collection Variables with Delay

### Key Scripts

**Pre-request Script** (clears old data):
```javascript
// Clear previous test data to start fresh
pm.collectionVariables.unset("test_vehicle_id");
pm.collectionVariables.unset("john_shift_id");
pm.collectionVariables.unset("peter_shift_id");
console.log("🧹 Starting fresh test run - cleared old variables");
```

Tests Script (saves data):

```javascript
pm.test("Vehicle created successfully", function() {
    pm.response.to.have.status(201);
    var jsonData = pm.response.json();
    
    pm.collectionVariables.set("test_vehicle_id", jsonData.id);
    console.log("🚗 New vehicle created with ID: " + jsonData.id);
});
```

Runner Settings:

· Delay: 500ms between requests
· Keep variable values: ✅
· Run in order: ✅

The Duplicate Registration Problem

```javascript
// BEFORE: Hardcoded registration (FAILED on second run)
"registration_number": "KEE 001A"

// AFTER: Dynamic timestamp (WORKS every time)
"registration_number": "KEE {{$timestamp}}"
```

The Complete 10-Step Test Sequence

1. Admin creates vehicle → saves test_vehicle_id
2. John logs in → saves token
3. John starts shift → saves john_shift_id
4. John ends shift → verifies completed
5. Peter logs in → saves token
6. Peter starts shift → saves peter_shift_id
7. Peter ends shift → verifies FLAGGED
8. Admin logs in
9. Check vehicle analytics → verifies data
10. Check alerts → verifies alert created

Lessons Learned

1. Collection variables > Environment variables for test runs
2. Dynamic data prevents conflicts
3. Delay between requests prevents race conditions
4. Console logs make debugging possible

The Final Working Collection

```json
{
  "info": {
    "name": "Vigil Full Lifecycle Test",
    "schema": "https://schema.getpostman.com/..."
  },
  "item": [...],
  "variable": [
    {
      "key": "base_url",
      "value": "http://localhost:8000"
    }
  ]
}