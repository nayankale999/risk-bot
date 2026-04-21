# Example: Update Password Reset Control

## User Request
```
Update the Password reset control CTL-2345
```

## Bot Response - Current State
```
Let me fetch control CTL-2345...

**Current State: CTL-2345**

| Field | Value |
|-------|-------|
| Control ID | CTL-2345 |
| Name | Password Reset Control |
| Description | Ensures secure password reset procedures via identity verification |
| Control Type | Preventive |
| Owner | IT Security Team |
| Operating Effectiveness | Effective |
| Design Effectiveness | Adequate |
| Frequency | On-demand |
| Automation | Semi-Automated |
| Status | Active |

What changes would you like to make to this control?
```

## User Specifies Changes
```
Change effectiveness to Needs Improvement and reassign owner to Sarah Johnson
```

## Bot Response - Confirmation Request
```
**Pending Action: UPDATE Control CTL-2345**

| Field | Current | New |
|-------|---------|-----|
| Operating Effectiveness | Effective | Needs Improvement |
| Owner | IT Security Team | Sarah Johnson |

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
| Control ID | CTL-2345 |
| Name | Password Reset Control |
| Operating Effectiveness | Needs Improvement |
| Owner | Sarah Johnson |
| Modified | 2026-02-02 14:38:45 UTC |
| Modified By | Current User |

Note: Since effectiveness changed to "Needs Improvement", you may want to create an Issue to track remediation efforts.
```

## API Calls Made

### 1. Query Control by ID
```http
GET /opgrc/api/v2/query?statement=SELECT * FROM [SOXControl] WHERE [Control ID] = 'CTL-2345'
Authorization: Bearer {{access_token}}
```

### 2. Get Full Control Details
```http
GET /opgrc/api/v2/contents/{{control_resource_id}}
Authorization: Bearer {{access_token}}
```

### 3. Resolve User Reference
```http
GET /opgrc/api/v2/query?statement=SELECT * FROM [User] WHERE [Name] LIKE 'Sarah Johnson%'
Authorization: Bearer {{access_token}}
```

### 4. Lock Control
```http
POST /opgrc/api/v2/contents/{{control_resource_id}}/lock
Authorization: Bearer {{access_token}}
```

### 5. Update Control
```http
PUT /opgrc/api/v2/contents/{{control_resource_id}}
Content-Type: application/json
Authorization: Bearer {{access_token}}

{
  "fields": {
    "OPSS-Ctrl:Operating Effectiveness": "Needs Improvement",
    "Resource ID:Control Owner": "{{sarah_johnson_user_id}}"
  }
}
```

### 6. Unlock Control
```http
DELETE /opgrc/api/v2/contents/{{control_resource_id}}/lock
Authorization: Bearer {{access_token}}
```

### Response
```json
{
  "id": "{{control_resource_id}}",
  "typeDefinitionId": "SOXControl",
  "name": "Password Reset Control",
  "modifiedDate": "2026-02-02T14:38:45Z",
  "modifiedBy": "current_user",
  "fields": {
    "Control ID": "CTL-2345",
    "OPSS-Ctrl:Operating Effectiveness": "Needs Improvement",
    "Resource ID:Control Owner": "{{sarah_johnson_user_id}}"
  }
}
```

## Follow-up Suggestion

When a control's effectiveness is downgraded, the bot may suggest:

```
Would you like me to create an Issue to track the remediation of this control?
This would help ensure the effectiveness gap is addressed.
```
