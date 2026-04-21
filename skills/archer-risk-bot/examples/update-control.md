# Example: Update Password Reset Control (Archer)

## User Request
```
Update the Password reset control CTL-2345
```

## Bot Response - Search and Current State
```
Let me search for control CTL-2345...

**Current State: CTL-2345 (Content ID: 175890)**

| Field | Value |
|-------|-------|
| Control ID | CTL-2345 |
| Control Name | Password Reset Control |
| Control Description | Ensures secure password reset procedures via identity verification |
| Control Type | Preventive |
| Control Owner | IT Security Team |
| Control Frequency | On-demand |
| Automation Level | Semi-Automated |
| Design Effectiveness | Adequate |
| Operating Effectiveness | Effective |
| Control Status | Active |
| Related Risks | 2 linked risks |

What changes would you like to make?
```

## User Specifies Changes
```
Change operating effectiveness to Needs Improvement and owner to Sarah Johnson
```

## Bot Response - Confirmation Request
```
**Pending Action: UPDATE Control (Content ID: 175890)**

| Field | Current | New |
|-------|---------|-----|
| Operating Effectiveness | Effective | Needs Improvement |
| Control Owner | IT Security Team | Sarah Johnson |

Please confirm by responding "yes" or "confirm" to proceed.
```

## User Confirms
```
yes
```

## Bot Response - Success
```
Control updated successfully.

| Field | Value |
|-------|-------|
| Content ID | 175890 |
| Control ID | CTL-2345 |
| Control Name | Password Reset Control |
| Operating Effectiveness | Needs Improvement |
| Control Owner | Sarah Johnson |
| Modified | 2026-02-02 14:38:45 UTC |

Note: Since effectiveness changed to "Needs Improvement", you may want to create an Issue to track remediation efforts.
```

## API Calls Made

### 1. Search for Control by Control ID
```http
POST /platformapi/core/content/fieldcontent?$filter=ControlID eq 'CTL-2345'
Content-Type: application/json
Authorization: Archer session-id="{{SessionToken}}"

{
  "FieldIds": [2501, 2502, 2503, 2504, 2505, 2506, 2507, 2508, 2509, 2510],
  "ContentIds": []
}
```

### 2. Get Full Control Record
```http
POST /platformapi/core/content/contentid?id=175890
Authorization: Archer session-id="{{SessionToken}}"
X-Http-Method-Override: GET
```

### Response
```json
{
  "RequestedObject": {
    "Id": 175890,
    "LevelId": 200,
    "FieldContents": {
      "2501": {
        "Type": 1,
        "Value": "CTL-2345"
      },
      "2502": {
        "Type": 1,
        "Value": "Password Reset Control"
      },
      "2503": {
        "Type": 1,
        "Value": "Ensures secure password reset procedures via identity verification"
      },
      "2508": {
        "Type": 4,
        "Value": {
          "ValuesListIds": [9001]
        }
      },
      "2505": {
        "Type": 9,
        "Value": {
          "ContentIds": [70100]
        }
      }
    }
  },
  "IsSuccessful": true
}
```

### 3. Resolve New Owner Reference
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

### 4. Update Control
```http
PUT /platformapi/core/content
Content-Type: application/json
Authorization: Archer session-id="{{SessionToken}}"

{
  "Content": {
    "Id": 175890,
    "LevelId": 200,
    "FieldContents": {
      "2508": {
        "Type": 4,
        "Value": {
          "ValuesListIds": [9003]
        }
      },
      "2505": {
        "Type": 9,
        "Value": {
          "ContentIds": [70234]
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
    "Id": 175890
  },
  "IsSuccessful": true,
  "ValidationMessages": []
}
```

## Field ID Reference (Example)

| Field | Field ID | Type |
|-------|----------|------|
| Control ID | 2501 | 1 (Text) |
| Control Name | 2502 | 1 (Text) |
| Control Description | 2503 | 1 (Text) |
| Control Type | 2504 | 4 (Values List) |
| Control Owner | 2505 | 9 (Cross-Reference) |
| Control Frequency | 2506 | 4 (Values List) |
| Automation Level | 2507 | 4 (Values List) |
| Operating Effectiveness | 2508 | 4 (Values List) |
| Design Effectiveness | 2509 | 4 (Values List) |
| Control Status | 2510 | 4 (Values List) |

## Values List Reference (Example)

| Operating Effectiveness | Value ID |
|------------------------|----------|
| Effective | 9001 |
| Ineffective | 9002 |
| Needs Improvement | 9003 |
| Not Tested | 9004 |
