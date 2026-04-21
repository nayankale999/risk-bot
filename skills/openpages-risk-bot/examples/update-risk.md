# Example: Update Climate Risk for Trading Desk

## User Request
```
Update the climate risk for the Trading desk
```

## Bot Response - Search Results
```
Let me search for climate-related risks in the Trading Desk...

**Search Results: Risks matching "climate" in Trading Desk**

| Risk ID | Name | Severity | Status |
|---------|------|----------|--------|
| RSK-2025-00089 | Climate Transition Risk | High | Active |
| RSK-2025-00102 | Physical Climate Risk | Medium | Draft |

Which risk would you like to update, and what changes should I make?
```

## User Specifies Changes
```
Update RSK-2025-00089, change severity to Critical and add note about increased regulatory scrutiny
```

## Bot Response - Current State and Confirmation
```
Let me fetch the current details of RSK-2025-00089...

**Current State: RSK-2025-00089**

| Field | Value |
|-------|-------|
| Name | Climate Transition Risk |
| Description | Risk of financial losses from transitioning to low-carbon economy |
| Risk Category | Strategic Risk |
| Business Entity | Trading Desk |
| Owner | Risk Management Team |
| Severity | High |
| Status | Active |

**Pending Action: UPDATE Risk RSK-2025-00089**

| Field | Current | New |
|-------|---------|-----|
| Severity | High | Critical |
| Description | Risk of financial losses from transitioning to low-carbon economy | Risk of financial losses from transitioning to low-carbon economy. Note: Increased regulatory scrutiny from 2026 climate disclosure requirements. |

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
| Risk ID | RSK-2025-00089 |
| Name | Climate Transition Risk |
| Severity | Critical |
| Status | Active |
| Modified | 2026-02-02 14:35:22 UTC |
| Modified By | Current User |

The severity has been escalated to Critical and the description updated with the regulatory note.
```

## API Calls Made

### 1. Search for Risks
```http
GET /opgrc/api/v2/query?statement=SELECT * FROM [SOXRisk] WHERE [Name] LIKE '%climate%' AND [Resource ID:Business Entity] = '{{trading_desk_id}}'
Authorization: Bearer {{access_token}}
```

### 2. Get Risk Details
```http
GET /opgrc/api/v2/contents/RSK-2025-00089
Authorization: Bearer {{access_token}}
```

### 3. Lock Risk for Editing
```http
POST /opgrc/api/v2/contents/RSK-2025-00089/lock
Authorization: Bearer {{access_token}}
```

### 4. Update Risk
```http
PUT /opgrc/api/v2/contents/RSK-2025-00089
Content-Type: application/json
Authorization: Bearer {{access_token}}

{
  "fields": {
    "OPSS-Risk:Severity": "Critical",
    "Description": "Risk of financial losses from transitioning to low-carbon economy. Note: Increased regulatory scrutiny from 2026 climate disclosure requirements."
  }
}
```

### 5. Unlock Risk
```http
DELETE /opgrc/api/v2/contents/RSK-2025-00089/lock
Authorization: Bearer {{access_token}}
```

### Response
```json
{
  "id": "RSK-2025-00089",
  "typeDefinitionId": "SOXRisk",
  "name": "Climate Transition Risk",
  "modifiedDate": "2026-02-02T14:35:22Z",
  "modifiedBy": "current_user",
  "fields": {
    "OPSS-Risk:Severity": "Critical",
    "Description": "Risk of financial losses from transitioning to low-carbon economy. Note: Increased regulatory scrutiny from 2026 climate disclosure requirements."
  }
}
```
