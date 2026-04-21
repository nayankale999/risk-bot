# Example: Create an Issue Linked to a Control (Archer)

## User Request
```
Create an issue for the password reset control CTL-2345 about the missing MFA requirement
```

## Bot Response - Gathering Information
```
I'll create an Issue in Archer linked to control CTL-2345 (Password Reset Control). Let me gather a few details:

1. **Issue Title**: What should this issue be called?
2. **Description**: Detailed description of the issue
3. **Priority**: (Critical/High/Medium/Low)
4. **Owner**: Who is responsible for resolving this?
5. **Due Date**: Target resolution date (optional)

Or I can suggest defaults based on your description.
```

## User Provides Details
```
Title is "MFA Not Enforced on Password Reset", priority High, assign to Sarah Johnson, due in 30 days
```

## Bot Response - Confirmation Request
```
**Pending Action: CREATE Issue**

| Field | Value |
|-------|-------|
| Issue Title | MFA Not Enforced on Password Reset |
| Issue Description | Missing MFA requirement on password reset control |
| Issue Priority | High |
| Issue Owner | Sarah Johnson |
| Due Date | 2026-03-04 |
| Issue Status | Open |
| Related Control | CTL-2345 (Password Reset Control) |

Please confirm by responding "yes" or "confirm" to proceed.
```

## User Confirms
```
yes
```

## Bot Response - Success
```
Issue created successfully.

| Field | Value |
|-------|-------|
| Content ID | 195670 |
| Issue Title | MFA Not Enforced on Password Reset |
| Issue Priority | High |
| Issue Owner | Sarah Johnson |
| Due Date | 2026-03-04 |
| Issue Status | Open |
| Related Control | CTL-2345 (Content ID: 175890) |
| Created | 2026-02-02 15:12:33 UTC |

The issue has been linked to control CTL-2345 and assigned to Sarah Johnson.
```

## API Calls Made

### 1. Resolve Control Reference
```http
POST /platformapi/core/content/fieldcontent?$filter=ControlID eq 'CTL-2345'
Content-Type: application/json
Authorization: Archer session-id="{{SessionToken}}"

{
  "FieldIds": [2501],
  "ContentIds": []
}
```

### Response
```json
{
  "RequestedObject": [
    {
      "Id": 175890,
      "FieldContents": {
        "2501": {
          "Type": 1,
          "Value": "CTL-2345"
        }
      }
    }
  ],
  "IsSuccessful": true
}
```

### 2. Resolve Owner Reference
```http
POST /platformapi/core/content/fieldcontent?$filter=contains(Name,'Sarah Johnson')
Content-Type: application/json
Authorization: Archer session-id="{{SessionToken}}"

{
  "FieldIds": [3001],
  "ContentIds": []
}
```

### Response
```json
{
  "RequestedObject": [
    {
      "Id": 70234,
      "FieldContents": {
        "3001": {
          "Type": 1,
          "Value": "Sarah Johnson"
        }
      }
    }
  ],
  "IsSuccessful": true
}
```

### 3. Create Issue
```http
POST /platformapi/core/content
Content-Type: application/json
Authorization: Archer session-id="{{SessionToken}}"

{
  "Content": {
    "LevelId": 300,
    "FieldContents": {
      "3501": {
        "Type": 1,
        "Value": "MFA Not Enforced on Password Reset"
      },
      "3502": {
        "Type": 1,
        "Value": "Missing MFA requirement on password reset control"
      },
      "3503": {
        "Type": 4,
        "Value": {
          "ValuesListIds": [10002]
        }
      },
      "3504": {
        "Type": 9,
        "Value": {
          "ContentIds": [70234]
        }
      },
      "3505": {
        "Type": 3,
        "Value": "2026-03-04T00:00:00"
      },
      "3506": {
        "Type": 4,
        "Value": {
          "ValuesListIds": [11001]
        }
      },
      "3510": {
        "Type": 9,
        "Value": {
          "ContentIds": [175890]
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
    "Id": 195670
  },
  "IsSuccessful": true,
  "ValidationMessages": []
}
```

## Field ID Reference (Example)

| Field | Field ID | Type |
|-------|----------|------|
| Issue Title | 3501 | 1 (Text) |
| Issue Description | 3502 | 1 (Text) |
| Issue Priority | 3503 | 4 (Values List) |
| Issue Owner | 3504 | 9 (Cross-Reference) |
| Due Date | 3505 | 3 (Date) |
| Issue Status | 3506 | 4 (Values List) |
| Issue Source | 3507 | 4 (Values List) |
| Remediation Plan | 3508 | 1 (Text) |
| Related Controls | 3510 | 9 (Cross-Reference) |
| Related Risks | 3511 | 9 (Cross-Reference) |

## Values List Reference (Example)

| Issue Priority | Value ID |
|----------------|----------|
| Critical | 10001 |
| High | 10002 |
| Medium | 10003 |
| Low | 10004 |

| Issue Status | Value ID |
|--------------|----------|
| Open | 11001 |
| In Progress | 11002 |
| Pending Validation | 11003 |
| Closed | 11004 |

## Date Format Note

Archer expects dates in ISO 8601 format: `YYYY-MM-DDTHH:MM:SS`

For date-only fields, use: `2026-03-04T00:00:00`
