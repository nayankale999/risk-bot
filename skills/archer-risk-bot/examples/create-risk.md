# Example: Create a Risk for the Loan Process (Archer)

## User Request
```
Create a Risk for the Loan process
```

## Bot Response - Gathering Information
```
I'll help you create a new Risk in Archer. I need a few details:

1. **Risk Name**: What should this risk be called?
2. **Description**: Brief description of the risk
3. **Risk Category**: (Operational, Credit, Compliance, Market, Strategic, Liquidity)
4. **Business Unit**: Which business unit owns this?
5. **Risk Owner**: Who is the risk owner?

Optional fields:
- Inherent Likelihood
- Inherent Impact
```

## User Provides Details
```
Name it "Loan Default Risk", description is "Risk of borrower defaults impacting loan portfolio quality and profitability", Category is Credit, Business Unit is Retail Banking, Owner is John Smith
```

## Bot Response - Confirmation Request
```
**Pending Action: CREATE Risk**

| Field | Value |
|-------|-------|
| Risk Name | Loan Default Risk |
| Risk Description | Risk of borrower defaults impacting loan portfolio quality and profitability |
| Risk Category | Credit Risk |
| Business Unit | Retail Banking |
| Risk Owner | John Smith |
| Risk Status | Draft |

Please confirm by responding "yes" or "confirm" to proceed.
```

## User Confirms
```
yes
```

## Bot Response - Success
```
Risk created successfully.

| Field | Value |
|-------|-------|
| Content ID | 184630 |
| Risk Name | Loan Default Risk |
| Risk Category | Credit Risk |
| Business Unit | Retail Banking |
| Risk Owner | John Smith |
| Risk Status | Draft |
| Created | 2026-02-02 14:32:00 UTC |

The risk is now in Draft status. You can update it to Active when ready for formal tracking.
```

## API Calls Made

### 1. Authenticate (if session expired)
```http
POST /platformapi/core/security/login
Content-Type: application/json

{
  "InstanceName": "Production",
  "Username": "api_user",
  "UserDomain": "",
  "Password": "****"
}
```

### Response
```json
{
  "SessionToken": "C204E18D0ED58E288533F39C455A36E8"
}
```

### 2. Resolve Cross-References

**Get Business Unit Content ID:**
```http
POST /platformapi/core/content/fieldcontent
Content-Type: application/json
Authorization: Archer session-id="C204E18D0ED58E288533F39C455A36E8"

{
  "FieldIds": [1001],
  "ContentIds": []
}
```

**Get Risk Owner Content ID:**
```http
POST /platformapi/core/content/fieldcontent
Content-Type: application/json
Authorization: Archer session-id="C204E18D0ED58E288533F39C455A36E8"

{
  "FieldIds": [2001],
  "ContentIds": []
}
```

### 3. Create Risk Record
```http
POST /platformapi/core/content
Content-Type: application/json
Authorization: Archer session-id="C204E18D0ED58E288533F39C455A36E8"

{
  "Content": {
    "LevelId": 100,
    "FieldContents": {
      "1501": {
        "Type": 1,
        "Value": "Loan Default Risk"
      },
      "1502": {
        "Type": 1,
        "Value": "Risk of borrower defaults impacting loan portfolio quality and profitability"
      },
      "1503": {
        "Type": 4,
        "Value": {
          "ValuesListIds": [5002]
        }
      },
      "1504": {
        "Type": 9,
        "Value": {
          "ContentIds": [50100]
        }
      },
      "1505": {
        "Type": 9,
        "Value": {
          "ContentIds": [60234]
        }
      },
      "1510": {
        "Type": 4,
        "Value": {
          "ValuesListIds": [7001]
        }
      }
    }
  }
}
```

### Response
```json
{
  "RequestedObject": {
    "Id": 184630
  },
  "IsSuccessful": true,
  "ValidationMessages": []
}
```

## Field ID Reference (Example)

| Field | Field ID | Type |
|-------|----------|------|
| Risk Name | 1501 | 1 (Text) |
| Risk Description | 1502 | 1 (Text) |
| Risk Category | 1503 | 4 (Values List) |
| Business Unit | 1504 | 9 (Cross-Reference) |
| Risk Owner | 1505 | 9 (Cross-Reference) |
| Risk Status | 1510 | 4 (Values List) |

## Values List Reference (Example)

| Value | Value ID |
|-------|----------|
| Credit Risk | 5002 |
| Draft Status | 7001 |
| Retail Banking (Content ID) | 50100 |
| John Smith (Content ID) | 60234 |
