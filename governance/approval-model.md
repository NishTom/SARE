# Approval Model

This document defines the approval model used by SARE to govern high-impact security response actions.

The approval model ensures that automation improves speed and consistency **without removing human accountability**.

---

## Objectives

- Prevent unsafe or irreversible actions
- Enforce human-in-the-loop decision making where required
- Align with enterprise change control and RBAC policies
- Provide a clear audit trail for every approved action

---

## Approval Philosophy

SARE treats **automation as a privilege, not a default**.

Actions are classified based on:
- Impact scope
- Reversibility
- User or system disruption
- Business risk

Only explicitly approved action classes may execute without human approval.

---

## Action Classification

### 1. No-Approval Actions (Informational)

Characteristics:
- No user or system impact
- Fully reversible
- Low risk

Examples:
- Add incident tags
- Add comments to incidents
- Notify SOC via email or chat
- Update incident severity (within bounds)

Approval Requirement:
- ❌ Not required

---

### 2. Conditional Approval Actions (Medium Impact)

Characteristics:
- Limited scope
- Reversible with effort
- Moderate operational impact

Examples:
- Disable a single user account temporarily
- Quarantine an email for a single mailbox
- Block a single IP or URL

Approval Requirement:
- ✅ Required unless explicitly pre-approved by policy

---

### 3. Mandatory Approval Actions (High Impact)

Characteristics:
- Broad scope or irreversible impact
- Affects multiple users or systems
- Potential business disruption

Examples:
- Disable privileged accounts
- Bulk email purge
- Tenant-wide blocking rules
- Identity access changes affecting production systems

Approval Requirement:
- 🔒 Always required

---

## Approval Workflow

### Step 1 — Recommendation

- SARE generates an action recommendation
- Recommendation includes:
  - Evidence summary
  - Risk and impact assessment
  - Justification for the action
  - Expected outcome

### Step 2 — Approval Request

- Action is routed to an approval queue
- Approver is selected based on:
  - Action type
  - Entity ownership
  - RBAC and escalation rules

### Step 3 — Decision

Approver may:
- Approve
- Reject
- Request more evidence

All decisions are logged.

### Step 4 — Execution

- Approved actions are executed via controlled playbooks
- Execution context is recorded
- Failures are captured and surfaced

---

## Human Roles

### Analyst
- Reviews evidence and recommendations
- Initiates approval requests
- Executes approved actions

### Approver
- Confirms business and security impact
- Authorizes high-impact actions
- Ensures policy compliance

### System (SARE)
- Enforces approval gates
- Prevents unauthorized execution
- Maintains audit logs

---

## Enforcement Mechanisms

- Approval checks embedded in the decision pipeline
- Playbooks validate approval tokens before execution
- Actions blocked if approval context is missing or invalid

---

## Audit and Traceability

Each approval record includes:
- Case ID
- Action requested
- Evidence references
- Approver identity
- Timestamp
- Decision outcome

Approval records are immutable and retained according to policy.

---

## Failure Handling

If approval fails or times out:
- Action is not executed
- Incident remains in review state
- Analyst is notified
- Case is escalated if required

---

## Key Takeaway

The SARE approval model ensures that **speed never overrides safety** — enabling automation that is trusted, governed, and auditable.
