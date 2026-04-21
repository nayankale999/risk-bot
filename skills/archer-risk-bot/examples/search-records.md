# Example: Search for Records (Archer)

## Example 1: Search Risks by Keyword

### User Request
```
Find all risks related to cyber security
```

### Bot Response
```
**Search Results: Risks matching "cyber"**

| Content ID | Risk Name | Risk Category | Inherent Rating | Status |
|------------|-----------|---------------|-----------------|--------|
| 185234 | Cyber Attack Risk | Technology | Critical | Active |
| 185267 | Cyber Fraud Risk | Operational | High | Active |
| 185301 | Third-Party Cyber Exposure | Third Party | High | Active |
| 184189 | Ransomware Attack Risk | Technology | Critical | Active |
| 184145 | Data Breach Risk | Technology | High | Active |

Showing 5 of 12 results. Would you like to see more or filter by specific criteria?
```

### API Call
```http
POST /platformapi/core/content/fieldcontent?$filter=contains(RiskName,'cyber') or contains(RiskDescription,'cyber')&$top=10&$orderby=InherentRiskRating desc
Content-Type: application/json
Authorization: Archer session-id="{{SessionToken}}"

{
  "FieldIds": [1501, 1503, 1506, 1510],
  "ContentIds": []
}
```

---

## Example 2: Search by Business Unit

### User Request
```
Show me all active risks for the Trading desk
```

### Bot Response
```
**Active Risks: Trading Desk**

| Content ID | Risk Name | Category | Inherent Rating | Owner |
|------------|-----------|----------|-----------------|-------|
| 182456 | Climate Transition Risk | Strategic | Critical | Risk Management |
| 182078 | Market Volatility Risk | Market | High | Trading Risk Team |
| 182056 | Counterparty Default Risk | Credit | High | Credit Risk Team |
| 182045 | Model Risk - Pricing | Model | Medium | Quant Team |
| 182034 | Regulatory Reporting Risk | Compliance | Medium | Compliance Team |

Showing 5 of 8 active risks. Would you like details on any specific risk?
```

### API Call
```http
POST /platformapi/core/content/referencefield/referencefieldid?id={{business_unit_field_id}}&$filter=BusinessUnit eq {{trading_desk_content_id}} and RiskStatus eq 'Active'&$top=10
Content-Type: application/json
Authorization: Archer session-id="{{SessionToken}}"

{
  "FieldIds": [1501, 1503, 1504, 1505, 1506, 1510],
  "ContentIds": []
}
```

---

## Example 3: Search Controls by Effectiveness

### User Request
```
List all controls that need improvement
```

### Bot Response
```
**Controls with Operating Effectiveness: Needs Improvement**

| Content ID | Control ID | Control Name | Business Unit | Owner |
|------------|------------|--------------|---------------|-------|
| 175890 | CTL-2345 | Password Reset Control | IT | Sarah Johnson |
| 176234 | CTL-2401 | Access Review Process | Compliance | John Smith |
| 176456 | CTL-2456 | Data Backup Verification | IT | IT Operations |
| 176567 | CTL-2478 | Vendor Assessment | Third Party | Procurement |

Found 4 controls with "Needs Improvement" effectiveness rating.

Would you like to:
- View details on any specific control
- Create issues for these controls
- See related risks
```

### API Call
```http
POST /platformapi/core/content/fieldcontent?$filter=OperatingEffectiveness eq {{needs_improvement_value_id}}&$top=20
Content-Type: application/json
Authorization: Archer session-id="{{SessionToken}}"

{
  "FieldIds": [2501, 2502, 2504, 2505, 2508],
  "ContentIds": []
}
```

---

## Example 4: Search Issues by Status and Due Date

### User Request
```
Show me all open issues due in the next 30 days
```

### Bot Response
```
**Open Issues Due Within 30 Days**

| Content ID | Issue Title | Priority | Owner | Due Date | Days Left |
|------------|-------------|----------|-------|----------|-----------|
| 195670 | MFA Not Enforced | High | Sarah Johnson | 2026-03-04 | 30 |
| 195234 | Access Policy Gap | Critical | John Smith | 2026-02-15 | 13 |
| 195345 | Incomplete Audit Trail | Medium | IT Security | 2026-02-20 | 18 |
| 195456 | Vendor SLA Breach | High | Procurement | 2026-02-25 | 23 |

Found 4 open issues due within 30 days.

**Attention**: 1 issue is Critical priority (Access Policy Gap).
```

### API Call
```http
POST /platformapi/core/content/fieldcontent?$filter=IssueStatus eq {{open_status_value_id}} and DueDate le '2026-03-04T00:00:00'&$orderby=DueDate asc
Content-Type: application/json
Authorization: Archer session-id="{{SessionToken}}"

{
  "FieldIds": [3501, 3503, 3504, 3505, 3506],
  "ContentIds": []
}
```

---

## Example 5: Complex Multi-Criteria Search

### User Request
```
Find high severity operational risks in Retail Banking that are not closed
```

### Bot Response
```
**High Severity Operational Risks: Retail Banking (Active/Draft)**

| Content ID | Risk Name | Inherent Rating | Status | Owner |
|------------|-----------|-----------------|--------|-------|
| 183156 | Branch Fraud Risk | High | Active | Ops Risk Team |
| 183167 | System Outage Risk | High | Active | IT Operations |
| 183178 | Process Error Risk | High | Draft | Quality Team |

Found 3 matching risks.

Note: Risk 183178 is still in Draft status. Would you like to activate it?
```

### API Call
```http
POST /platformapi/core/content/fieldcontent?$filter=InherentRiskRating eq {{high_rating_value_id}} and RiskCategory eq {{operational_risk_value_id}} and BusinessUnit eq {{retail_banking_content_id}} and RiskStatus ne {{closed_status_value_id}}
Content-Type: application/json
Authorization: Archer session-id="{{SessionToken}}"

{
  "FieldIds": [1501, 1505, 1506, 1510],
  "ContentIds": []
}
```

---

## Query Parameter Reference

### Supported OData Operations

| Parameter | Description | Example |
|-----------|-------------|---------|
| `$filter` | Filter results | `$filter=Status eq 'Active'` |
| `$select` | Select specific fields | `$select=RiskName,Status` |
| `$top` | Limit records returned | `$top=10` |
| `$skip` | Skip records (pagination) | `$skip=10` |
| `$orderby` | Sort results | `$orderby=DueDate desc` |

### Filter Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `eq` | Equals | `Status eq 'Active'` |
| `ne` | Not equals | `Status ne 'Closed'` |
| `gt` | Greater than | `DueDate gt '2026-02-01'` |
| `ge` | Greater than or equal | `Rating ge 3` |
| `lt` | Less than | `DueDate lt '2026-03-01'` |
| `le` | Less than or equal | `Rating le 5` |
| `contains` | Contains substring | `contains(Name,'cyber')` |
| `and` | Logical AND | `Status eq 'Open' and Priority eq 'High'` |
| `or` | Logical OR | `Priority eq 'Critical' or Priority eq 'High'` |

### Pagination Example

**First page (records 1-10):**
```
$top=10&$skip=0
```

**Second page (records 11-20):**
```
$top=10&$skip=10
```

**Third page (records 21-30):**
```
$top=10&$skip=20
```

---

## Notes

1. **Search does NOT require confirmation** - Only mutations need explicit approval
2. **Default limit is 10 records** - Use `$top` to request more
3. **Archer returns max 1000 records per request** - Use pagination for large result sets
4. **Field IDs are instance-specific** - Configure in config.json
5. **Cross-reference searches** require knowing the Content IDs of related records
