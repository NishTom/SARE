# Sentinel Playbooks — SARE Execution Model

This folder documents how SARE integrates with Microsoft Sentinel playbooks (Logic Apps) to execute response actions.

SARE separates:
- **Decisioning** (core engine + deterministic pipeline)
from
- **Execution** (Sentinel playbooks)

This separation enables strong governance and least privilege.

---

## Principles

- Playbooks do not decide *what* to do — they execute *approved* actions
- High-impact actions require explicit approval context
- All actions must be auditable and reversible where possible

---

## Execution Patterns

### Pattern A — Safe, No-Approval Actions
Used for low-risk automation.

Examples:
- Add tags to incident
- Add a decision summary comment
- Notify SOC channel
- Create an approval request record

Approval:
- Not required

---

### Pattern B — Approval-Gated Actions (Recommended)
Used for any action that changes access or removes data.

Examples:
- Disable user account
- Quarantine or purge email
- Block URL/IP in security controls

Approval:
- Required

Enforcement:
- Playbook validates an approval signal before execution
- If approval is missing/invalid, playbook exits safely

---

## Approval Context (How SARE Passes Approval)

Typical methods:
- Incident tag + approval ID stored in incident comment or custom field
- External approval store (ticketing system / database) referenced by case ID
- Manual step in Sentinel incident workflow prior to playbook run

Minimum fields to validate:
- CaseId
- ApprovedAction
- Approver identity
- Approval timestamp
- Expiry window (optional)

---

## Guardrails Applied in Playbooks

Playbooks must enforce:
- Scope: single entity vs bulk actions
- Rate limits: avoid repeated disable/quarantine loops
- Identity permissions: least privilege managed identity
- Logging: execution results written back to incident and audit store

---

## Playbook Naming Convention

Use consistent names to match action intent:

- `SARE-Notify-SOC`
- `SARE-Add-Decision-Summary`
- `SARE-Request-Approval`
- `SARE-Disable-User-Approved`
- `SARE-Quarantine-Email-Approved`

---

## Future Enhancements

- Versioned playbook templates
- Standardized approval token format
- Automated drift detection (playbook changes vs approved baseline)
