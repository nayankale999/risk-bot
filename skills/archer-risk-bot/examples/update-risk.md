# Example: Update Climate Risk for Trading Desk (Archer)

## User Request
```
Update the climate risk for the Trading desk
```

## Bot Response - Search Results
```
Let me search for climate-related risks in the Trading Desk...

**Search Results: Risks matching "climate" in Trading Desk**

| Content ID | Risk Name | Inherent Rating | Status |
|------------|-----------|-----------------|--------|
| 182456 | Climate Transition Risk | High | Active |
| 183102 | Physical Climate Risk | Medium | Draft |

Which risk would you like to update, and what changes should I make?
```

## User Specifies Changes
```
Update 182456, change inherent likelihood to Very High and add note about regulatory pressure
```

## Bot Response - Current State and Confirmation
```
Let me fetch the current details of Content ID 182456...

**Current State: Climate Transition Risk (Content ID: 182456)**

| Field | Value |
|-------|-------|
| Risk Name | Climate Transition Risk |
| Risk Description | Risk of financial losses from transitioning to low-carbon economy |
| Risk Category | Strategic Risk |
| Business Unit | Trading Desk |
| Risk Owner | Risk Management Team |
| Inherent Likelihood | High |
| Inherent Impact | High |
| Inherent Risk Rating | High |
| Risk Status | Active |

**Pending Action: UPDATE Risk (Content ID: 182456)**

| Field | Current | New |
|-------|---------|-----|
| Inherent Likelihood | High | Very High |
| Risk Description | Risk of financial losses from transitioning to low-carbon economy | Risk of financial losses from transitioning to low-carbon economy. Note: Increased regulatory pressure from 2026 climate disclosure requirements. |

Please confirm by responding "yes" or "confirm" to proceed.
```

## User Confirms
```
confirm
```

## Bot Response - Success
```
Risk updated successfully.

| Field | Value |
|-------|-------|
| Content ID | 182456 |
| Risk Name | Climate Transition Risk |
| Inherent Likelihood | Very High |
| Risk Status | Active |
| Modified | 2026-02-02 14:35:22 UTC |

Note: The Inherent Risk Rating may be recalculated based on the updated likelihood.
```

## API Calls Made

### 1. Search for Risks
```http
POST /platformapi/core/content/fieldcontent?$filter=contains(RiskName,'climate')&$top=10
Content-Type: application/json
Authorization: Archer session-id="{{SessionToken}}"

{
  "FieldIds": [1501, 1503, 1504, 1506, 1510],
  "ContentIds": []
}
```

### 2. Get Risk Details
```http
POST /platformapi/core/content/contentid?id=182456
Authorization: Archer session-id="{{SessionToken}}"
X-Http-Method-Override: GET
```

### Response
```json
{
  "RequestedObject": {
    "Id": 182456,
    "LevelId": 100,
    "FieldContents": {
      "1501": {
        "Type": 1,
        "Value": "Climate Transition Risk"
      },
      "1502": {
        "Type": 1,
        "Value": "Risk of financial losses from transitioning to low-carbon economy"
      },
      "1506": {
        "Type": 4,
        "Value": {
          "ValuesListIds": [8002]
        }
      }
    }
  },
  "IsSuccessful": true
}
```

### 3. Update Risk
```http
PUT /platformapi/core/content
Content-Type: application/json
Authorization: Archer session-id="{{SessionToken}}"

{
  "Content": {
    "Id": 182456,
    "LevelId": 100,
    "FieldContents": {
      "1506": {
        "Type": 4,
        "Value": {
          "ValuesListIds": [8001]
        }
      },
      "1502": {
        "Type": 1,
        "Value": "Risk of financial losses from transitioning to low-carbon economy. Note: Increased regulatory pressure from 2026 climate disclosure requirements."
      }
    }
  }
}
```

### Response
```json
{
  "RequestedObject": {
    "Id": 182456
  },
  "IsSuccessful": true,
  "ValidationMessages": []
}
```

## Important Notes

1. **Archer HTTP 400 Error**: If you try to update a record with the same values it already has, Archer returns HTTP 400. Always verify the new values are different.

2. **Record Locking**: Archer REST API can update records even when locked in the UI. Use with caution.

3. **Matrix Fields**: The Inherent Risk Rating (Type 16) is typically calculated automatically based on Likelihood and Impact. You cannot directly update calculated matrix fields.

4. **Values List IDs**:
   - Very High Likelihood = 8001
   - High Likelihood = 8002
   - Medium Likelihood = 8003
   - Low Likelihood = 8004
   - Very Low Likelihood = 8005
