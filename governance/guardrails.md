# Guardrails

This document defines the guardrails enforced by SARE to prevent unsafe, unauthorized, or unintended actions during security investigations and response.

Guardrails ensure that automation operates **within explicit safety, policy, and scope boundaries**.

---

## Objectives

- Prevent accidental or malicious misuse of automation
- Enforce least privilege and scope control
- Ensure predictable and reversible behavior
- Protect business-critical systems and users

---

## Guardrail Categories

### 1. Scope Guardrails

Ensure actions are executed only within allowed scope.

Enforced constraints:
- Tenant and workspace boundaries
- Entity type restrictions (user, device, mailbox, URL)
- Environment restrictions (prod vs non-prod)
- Time-bound execution windows

Example:
- A playbook scoped to a single user cannot act on groups or tenants.

---

### 2. Action Guardrails

Control which actions may be executed and under what conditions.

Rules:
- Only explicitly approved actions are executable
- High-impact actions require approval tokens
- Actions must be reversible where possible

Blocked examples:
- Bulk account disablement
- Tenant-wide configuration changes
- Destructive actions without rollback

---

### 3. Identity & Permission Guardrails

Ensure automation identities operate with least privilege.

Controls:
- Managed identities with scoped permissions
- No shared credentials
- Separation between decision logic and execution identity
- Permission validation before execution

If permissions exceed policy:
- Action is blocked
- Incident routed for review
- Violation logged

---

### 4. Evidence Guardrails

Ensure decisions are evidence-backed.

Requirements:
- Minimum evidence thresholds before action recommendation
- Evidence freshness checks
- Provenance validation

If evidence is insufficient:
- Automation is halted
- Analyst review required

---

### 5. Rate and Frequency Guardrails

Prevent automation loops or excessive actions.

Limits:
- Max actions per case
- Max actions per entity per time window
- Cool-down periods between similar actions

Violations result in throttling or blocking.

---

### 6. AI Output Guardrails

Ensure AI assistance remains safe and bounded.

Rules:
- AI outputs are advisory only
- No execution authority
- No policy overrides
- Outputs must reference evidence

Invalid AI outputs are discarded and logged.

---

## Guardrail Enforcement Points

Guardrails are enforced:
- Before decision finalization
- Before approval routing
- Before execution
- After execution (post-checks)

Failure at any enforcement point blocks execution.

---

## Guardrail Violations

When a guardrail is violated:
- Action is blocked
- Violation reason recorded
- Incident routed to analyst
- Optional escalation triggered

Repeated violations may trigger alerts or disable automation paths.

---

## Configuration and Change Control

- Guardrails are centrally defined
- Changes require approval and versioning
- Policy changes are audited
- Rollback supported

---

## Key Takeaway

Guardrails ensure SARE automation remains **safe, predictable, and controllable**, enabling trust in AI-assisted security operations.
