# Example: Search for Risks

## Example 1: Search by Keyword

### User Request
```
Find all risks related to cyber security
```

### Bot Response
```
**Search Results: Risks matching "cyber"**

| Risk ID | Name | Business Entity | Severity | Status |
|---------|------|-----------------|----------|--------|
| RSK-2025-00234 | Cyber Attack Risk | Information Technology | Critical | Active |
| RSK-2025-00267 | Cyber Fraud Risk | Retail Banking | High | Active |
| RSK-2025-00301 | Third-Party Cyber Exposure | Operations | High | Active |
| RSK-2024-00189 | Ransomware Attack Risk | Information Technology | Critical | Active |
| RSK-2024-00145 | Data Breach Risk | Information Technology | High | Active |

Showing 5 of 12 results. Would you like to see more or filter by specific criteria?
```

---

## Example 2: Search by Business Entity

### User Request
```
Show me all active risks for the Trading desk
```

### Bot Response
```
**Active Risks: Trading Desk**

| Risk ID | Name | Category | Severity | Owner |
|---------|------|----------|----------|-------|
| RSK-2025-00089 | Climate Transition Risk | Strategic | Critical | Risk Management |
| RSK-2025-00078 | Market Volatility Risk | Market | High | Trading Risk Team |
| RSK-2025-00056 | Counterparty Default Risk | Credit | High | Credit Risk Team |
| RSK-2025-00045 | Model Risk - Pricing | Model | Medium | Quant Team |
| RSK-2025-00034 | Regulatory Reporting Risk | Compliance | Medium | Compliance Team |

Showing 5 of 8 active risks. Would you like details on any specific risk?
```

---

## Example 3: Search by Severity

### User Request
```
List all critical risks
```

### Bot Response
```
**Critical Severity Risks**

| Risk ID | Name | Business Entity | Category | Status |
|---------|------|-----------------|----------|--------|
| RSK-2025-00234 | Cyber Attack Risk | IT | Technology | Active |
| RSK-2025-00089 | Climate Transition Risk | Trading | Strategic | Active |
| RSK-2024-00189 | Ransomware Attack Risk | IT | Technology | Active |
| RSK-2025-00198 | AML Compliance Breach | Compliance | Compliance | Active |
| RSK-2025-00212 | Liquidity Crisis Risk | Treasury | Liquidity | Active |

Showing 5 of 15 critical risks across the organization.

Would you like to:
- Filter by business entity
- See risks with overdue reviews
- View associated controls for any risk
```

---

## Example 4: Complex Search

### User Request
```
Find high severity operational risks in Retail Banking that are not closed
```

### Bot Response
```
**High Severity Operational Risks: Retail Banking (Active/Draft)**

| Risk ID | Name | Status | Owner | Last Review |
|---------|------|--------|-------|-------------|
| RSK-2025-00156 | Branch Fraud Risk | Active | Ops Risk Team | 2026-01-15 |
| RSK-2025-00167 | System Outage Risk | Active | IT Operations | 2026-01-20 |
| RSK-2025-00178 | Process Error Risk | Draft | Quality Team | 2026-01-10 |

Found 3 matching risks.

RSK-2025-00178 is still in Draft status. Would you like to activate it?
```

---

## API Calls for Search Operations

### Simple Keyword Search
```http
GET /opgrc/api/v2/query?statement=SELECT * FROM [SOXRisk] WHERE [Name] LIKE '%cyber%' OR [Description] LIKE '%cyber%'
Authorization: Bearer {{access_token}}
```

### Search by Business Entity
```http
GET /opgrc/api/v2/query?statement=SELECT * FROM [SOXRisk] WHERE [Resource ID:Business Entity] = '{{trading_desk_id}}' AND [OPSS-Risk:Status] = 'Active'
Authorization: Bearer {{access_token}}
```

### Search by Severity
```http
GET /opgrc/api/v2/query?statement=SELECT * FROM [SOXRisk] WHERE [OPSS-Risk:Severity] = 'Critical'
Authorization: Bearer {{access_token}}
```

### Complex Search
```http
GET /opgrc/api/v2/query?statement=SELECT * FROM [SOXRisk] WHERE [OPSS-Risk:Severity] = 'High' AND [Resource ID:Risk Category] = '{{operational_risk_id}}' AND [Resource ID:Business Entity] = '{{retail_banking_id}}' AND [OPSS-Risk:Status] != 'Closed'
Authorization: Bearer {{access_token}}
```

## Notes

- Search operations do NOT require confirmation
- Results are paginated (default 10 items)
- Use `LIKE` with `%` for partial matching
- Combine conditions with `AND`/`OR`
- Field names must match OpenPages schema exactly
