# Example: Create an Issue Linked to a Control

## User Request
```
Create an issue for the password reset control CTL-2345 about the missing MFA requirement
```

## Bot Response - Gathering Information
```
I'll create an Issue linked to control CTL-2345 (Password Reset Control). Let me gather a few details:

1. **Issue Name**: What should this issue be called?
2. **Description**: Detailed description of the issue
3. **Priority**: (Critical/High/Medium/Low)
4. **Owner**: Who is responsible for resolving this?
5. **Due Date**: Target resolution date (optional)

Or I can suggest defaults based on your description.
```

## User Provides Details
```
Name it "MFA Not Enforced on Password Reset", priority High, assign to Sarah Johnson, due in 30 days
```

## Bot Response - Confirmation Request
```
**Pending Action: CREATE Issue**

| Field | Value |
|-------|-------|
| Name | MFA Not Enforced on Password Reset |
| Description | Missing MFA requirement on password reset control |
| Priority | High |
| Owner | Sarah Johnson |
| Due Date | 2026-03-04 |
| Status | Open |
| Linked Control | CTL-2345 (Password Reset Control) |

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
| Issue ID | ISS-2026-00034 |
| Name | MFA Not Enforced on Password Reset |
| Priority | High |
| Owner | Sarah Johnson |
| Due Date | 2026-03-04 |
| Status | Open |
| Linked Control | CTL-2345 |
| Created | 2026-02-02 15:12:33 UTC |

The issue has been linked to control CTL-2345 and assigned to Sarah Johnson.
```

## API Calls Made

### 1. Query Control to Get Resource ID
```http
GET /opgrc/api/v2/query?statement=SELECT * FROM [SOXControl] WHERE [Control ID] = 'CTL-2345'
Authorization: Bearer {{access_token}}
```

### 2. Resolve User Reference
```http
GET /opgrc/api/v2/query?statement=SELECT * FROM [User] WHERE [Name] LIKE 'Sarah Johnson%'
Authorization: Bearer {{access_token}}
```

### 3. Create Issue
```http
POST /opgrc/api/v2/contents
Content-Type: application/json
Authorization: Bearer {{access_token}}

{
  "typeDefinitionId": "SOXIssue",
  "name": "MFA Not Enforced on Password Reset",
  "description": "Missing MFA requirement on password reset control",
  "primaryParentId": "{{control_resource_id}}",
  "fields": {
    "OPSS-Issue:Priority": "High",
    "OPSS-Issue:Status": "Open",
    "OPSS-Issue:Due Date": "2026-03-04",
    "Resource ID:Issue Owner": "{{sarah_johnson_user_id}}"
  }
}
```

### 4. Create Association (if not using primaryParentId)
```http
POST /opgrc/api/v2/contents/{{issue_resource_id}}/associations/parents
Content-Type: application/json
Authorization: Bearer {{access_token}}

{
  "associations": [
    {
      "id": "{{control_resource_id}}"
    }
  ]
}
```

### Response
```json
{
  "id": "{{issue_resource_id}}",
  "typeDefinitionId": "SOXIssue",
  "name": "MFA Not Enforced on Password Reset",
  "description": "Missing MFA requirement on password reset control",
  "createdDate": "2026-02-02T15:12:33Z",
  "createdBy": "current_user",
  "fields": {
    "Issue ID": "ISS-2026-00034",
    "OPSS-Issue:Priority": "High",
    "OPSS-Issue:Status": "Open",
    "OPSS-Issue:Due Date": "2026-03-04"
  }
}
```
