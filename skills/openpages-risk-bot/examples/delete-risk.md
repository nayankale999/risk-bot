# Example: Delete a Risk

## User Request
```
Delete risk RSK-2024-00312
```

## Bot Response - Fetch and Confirm
```
Let me fetch the details of RSK-2024-00312...

**Current State: RSK-2024-00312**

| Field | Value |
|-------|-------|
| Risk ID | RSK-2024-00312 |
| Name | Legacy System Migration Risk |
| Description | Risk associated with migrating legacy trading systems |
| Risk Category | Technology Risk |
| Business Entity | Trading Desk |
| Owner | IT Migration Team |
| Severity | Medium |
| Status | Closed |
| Associated Controls | 2 |
| Associated Issues | 0 |

**WARNING: This risk has 2 associated controls that will be orphaned.**

**Pending Action: DELETE Risk RSK-2024-00312**

This action will:
- Remove the risk from OpenPages
- Orphan 2 associated controls (they will remain but lose their risk linkage)

Please confirm by responding "yes" or "confirm" to proceed.
```

## User Confirms
```
confirm
```

## Bot Response - Success
```
Risk deleted successfully.

| Field | Value |
|-------|-------|
| Risk ID | RSK-2024-00312 |
| Name | Legacy System Migration Risk |
| Deleted | 2026-02-02 15:45:12 UTC |
| Deleted By | Current User |

Note: The 2 previously associated controls are now orphaned. You may want to reassign them to another risk or archive them.
```

## Alternative: User Cancels

## User Response
```
no, wait - what controls are associated?
```

## Bot Response - Show Associations
```
Here are the controls associated with RSK-2024-00312:

| Control ID | Name | Status | Effectiveness |
|------------|------|--------|---------------|
| CTL-2024-00156 | Legacy Data Validation | Active | Effective |
| CTL-2024-00157 | Migration Rollback Procedure | Active | Effective |

Would you like to:
1. Proceed with deleting the risk (controls will be orphaned)
2. Reassign the controls to another risk first
3. Cancel the deletion

Please let me know how you'd like to proceed.
```

## API Calls Made

### 1. Get Risk Details
```http
GET /opgrc/api/v2/contents/RSK-2024-00312
Authorization: Bearer {{access_token}}
```

### 2. Get Associated Controls
```http
GET /opgrc/api/v2/contents/RSK-2024-00312/associations/children?typeDefinitionId=SOXControl
Authorization: Bearer {{access_token}}
```

### 3. Get Associated Issues
```http
GET /opgrc/api/v2/contents/RSK-2024-00312/associations/children?typeDefinitionId=SOXIssue
Authorization: Bearer {{access_token}}
```

### 4. Delete Risk (after confirmation)
```http
DELETE /opgrc/api/v2/contents/RSK-2024-00312
Authorization: Bearer {{access_token}}
```

### Response
```json
{
  "status": "deleted",
  "id": "RSK-2024-00312",
  "deletedDate": "2026-02-02T15:45:12Z",
  "deletedBy": "current_user"
}
```

## Important Notes

1. **Soft Delete**: OpenPages typically performs soft deletes. Administrators can restore deleted objects if needed.

2. **Cascade Behavior**: Deleting a risk does NOT automatically delete associated controls or issues. They become orphaned.

3. **Workflow Restrictions**: Some risks may be protected by workflow rules and cannot be deleted until they reach a specific status (e.g., "Closed" or "Archived").

4. **Audit Trail**: All deletions are logged in the OpenPages audit trail for compliance purposes.
