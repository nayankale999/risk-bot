# Example: Delete a Risk Record (Archer)

## User Request
```
Delete risk 182100
```

## Bot Response - Fetch and Confirm
```
Let me fetch the details of Content ID 182100...

**Current State: Content ID 182100**

| Field | Value |
|-------|-------|
| Content ID | 182100 |
| Risk Name | Legacy System Migration Risk |
| Risk Description | Risk associated with migrating legacy trading systems |
| Risk Category | Technology Risk |
| Business Unit | Trading Desk |
| Risk Owner | IT Migration Team |
| Inherent Risk Rating | Medium |
| Risk Status | Closed |
| Related Controls | 2 linked |
| Related Issues | 0 linked |

**WARNING: This risk has 2 related controls that reference it.**

**Pending Action: DELETE Risk (Content ID: 182100)**

This action will:
- Permanently remove the risk record from Archer
- Break cross-reference links from the 2 related controls

This action cannot be undone through the API.

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
| Content ID | 182100 |
| Risk Name | Legacy System Migration Risk |
| Deleted | 2026-02-02 15:45:12 UTC |

Note: The 2 previously related controls may now have broken cross-references. You may want to update them.
```

---

## Alternative Flow: User Requests More Info

## User Response
```
wait - what controls are linked to this risk?
```

## Bot Response - Show Related Records
```
Let me fetch the related controls for risk 182100...

**Related Controls for Risk 182100 (Legacy System Migration Risk)**

| Content ID | Control ID | Control Name | Status |
|------------|------------|--------------|--------|
| 176234 | CTL-2024-0156 | Legacy Data Validation | Active |
| 176235 | CTL-2024-0157 | Migration Rollback Procedure | Active |

Would you like to:
1. Proceed with deleting the risk (control references will be broken)
2. First remove the control-risk associations, then delete the risk
3. Cancel the deletion

Please let me know how you'd like to proceed.
```

## User Decides to Proceed
```
proceed with deletion
```

## Bot Response
```
**Pending Action: DELETE Risk (Content ID: 182100)**

Confirming deletion of "Legacy System Migration Risk" which will break links from:
- CTL-2024-0156 (Legacy Data Validation)
- CTL-2024-0157 (Migration Rollback Procedure)

Please confirm by responding "yes" or "confirm" to proceed.
```

---

## API Calls Made

### 1. Get Risk Details
```http
POST /platformapi/core/content/contentid?id=182100
Authorization: Archer session-id="{{SessionToken}}"
X-Http-Method-Override: GET
```

### Response
```json
{
  "RequestedObject": {
    "Id": 182100,
    "LevelId": 100,
    "FieldContents": {
      "1501": {
        "Type": 1,
        "Value": "Legacy System Migration Risk"
      },
      "1502": {
        "Type": 1,
        "Value": "Risk associated with migrating legacy trading systems"
      },
      "1503": {
        "Type": 4,
        "Value": {
          "ValuesListIds": [5007]
        }
      },
      "1510": {
        "Type": 4,
        "Value": {
          "ValuesListIds": [7003]
        }
      }
    }
  },
  "IsSuccessful": true
}
```

### 2. Get Related Controls (via Cross-Reference)
```http
POST /platformapi/core/content/fieldcontent
Content-Type: application/json
Authorization: Archer session-id="{{SessionToken}}"

{
  "FieldIds": [2511],
  "ContentIds": []
}
```

Then filter for controls where Related Risks contains 182100.

### 3. Delete Risk
```http
DELETE /platformapi/core/content/182100
Content-Type: application/json
Authorization: Archer session-id="{{SessionToken}}"
```

### Response
```json
{
  "IsSuccessful": true,
  "ValidationMessages": [
    {
      "Message": "Record 182100 has been deleted."
    }
  ]
}
```

---

## Important Notes

### 1. Deletion is Permanent
Unlike some systems, Archer's REST API deletion is typically permanent. There's no "soft delete" via API - the record is removed from the database.

### 2. Cross-Reference Handling
When you delete a record that is referenced by other records (via Cross-Reference fields):
- The referencing records are NOT automatically updated
- They may display errors or broken links in the UI
- Best practice: Clean up cross-references before deletion

### 3. Workflow Restrictions
Some Archer configurations may:
- Prevent deletion of records in certain statuses
- Require records to go through a workflow before deletion
- Restrict deletion to certain user roles

### 4. Audit Trail
Even though the record is deleted, Archer typically maintains an audit trail of the deletion event, including:
- Who deleted the record
- When it was deleted
- What the record contained (depending on configuration)

### 5. Error Handling

**403 Forbidden**: User lacks delete permission or record is protected
**404 Not Found**: Record doesn't exist or already deleted
**409 Conflict**: Record is in a state that prevents deletion
