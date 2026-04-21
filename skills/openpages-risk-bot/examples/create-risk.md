# Example: Create a Risk for the Loan Process

## User Request
```
Create a Risk for the Loan process
```

## Bot Response - Gathering Information
```
I'll help you create a new Risk for the Loan process. I need a few details:

1. **Risk Name**: What should this risk be called?
2. **Description**: Brief description of the risk
3. **Risk Category**: (Operational, Credit, Compliance, Market, Strategic, Liquidity)
4. **Business Entity**: Which business unit owns this?
5. **Owner**: Who is the risk owner?

Optional fields:
- Severity (Critical/High/Medium/Low)
- Likelihood
- Impact
```

## User Provides Details
```
Name it "Loan Default Risk", description is "Risk of borrower defaults impacting loan portfolio quality and profitability", Category is Credit, Business Entity is Retail Banking, Owner is John Smith, Severity High
```

## Bot Response - Confirmation Request
```
**Pending Action: CREATE Risk**

| Field | Value |
|-------|-------|
| Name | Loan Default Risk |
| Description | Risk of borrower defaults impacting loan portfolio quality and profitability |
| Risk Category | Credit Risk |
| Business Entity | Retail Banking |
| Owner | John Smith |
| Severity | High |
| Status | Draft |

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
| Risk ID | RSK-2026-00147 |
| Name | Loan Default Risk |
| Risk Category | Credit Risk |
| Business Entity | Retail Banking |
| Owner | John Smith |
| Severity | High |
| Status | Draft |
| Created | 2026-02-02 14:32:00 UTC |

The risk is now in Draft status. You can update it to Active when ready for formal tracking.
```

## API Calls Made

### 1. Create Risk
```http
POST /opgrc/api/v2/contents
Content-Type: application/json
Authorization: Bearer {{access_token}}

{
  "typeDefinitionId": "SOXRisk",
  "name": "Loan Default Risk",
  "description": "Risk of borrower defaults impacting loan portfolio quality and profitability",
  "primaryParentId": "{{retail_banking_entity_id}}",
  "fields": {
    "Resource ID:Risk Category": "{{credit_risk_category_id}}",
    "Resource ID:Risk Owner": "{{john_smith_user_id}}",
    "OPSS-Risk:Severity": "High",
    "OPSS-Risk:Status": "Draft"
  }
}
```

### Response
```json
{
  "id": "RSK-2026-00147",
  "typeDefinitionId": "SOXRisk",
  "name": "Loan Default Risk",
  "description": "Risk of borrower defaults impacting loan portfolio quality and profitability",
  "createdDate": "2026-02-02T14:32:00Z",
  "createdBy": "current_user",
  "fields": {
    "OPSS-Risk:Severity": "High",
    "OPSS-Risk:Status": "Draft"
  }
}
```
