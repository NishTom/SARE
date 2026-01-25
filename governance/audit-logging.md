# Audit Logging

This document defines how SARE records, stores, and protects audit logs to ensure traceability, accountability, and compliance.

Audit logging is a first-class requirement in SARE and applies to **every decision, recommendation, approval, and execution**.

---

## Objectives

- Provide end-to-end traceability for security decisions
- Support investigations, audits, and compliance reviews
- Prevent tampering or unauthorized modification
- Enable post-incident analysis and tuning

---

## What Is Logged

SARE records audit events at each critical stage of the workflow.

### 1. Intake Events
- Sentinel incident ID
- Detection rule name
- Trigger timestamp
- Initial entity set
- CaseId generated

---

### 2. Evidence Collection Events
- Enrichment query identifiers
- Time windows used
- Evidence source type (KQL, TI provider, directory lookup)
- Result counts and summaries
- Missing or failed evidence lookups

---

### 3. Decision Events
- Risk score and factor breakdown
- Impact and confidence levels
- Routing outcome
- Policy and guardrail checks (pass/fail)

---

### 4. Agent Assistance Events
- Agent type invoked
- Input references
- Output summary hash (not raw text if restricted)
- Validation results

---

### 5. Approval Events
- Action requested
- Approver identity
- Approval decision
- Timestamp
- Justification (if provided)

---

### 6. Execution Events
- Action executed
- Playbook or automation identifier
- Execution result (success/failure)
- Error details (if applicable)

---

### 7. Closure Events
- Final analyst disposition
- Incident closure reason
- Post-incident notes
- Effectiveness signals

---

## Log Structure

Each audit event includes:
- CaseId
- EventType
- Timestamp (UTC)
- Actor (system, analyst, approver)
- Source component
- Correlation identifiers

Logs are structured and machine-readable.

---

## Storage and Retention

Audit logs are:
- Written to centralized logging (e.g., Log Analytics or ADX)
- Immutable once written
- Retained according to enterprise policy

Retention examples:
- Standard cases: 90–180 days
- High-impact actions: 1–3 years
- Compliance-driven cases: per regulatory requirement

---

## Access Controls

- Read-only access for auditors
- Restricted write access (system only)
- Separation of duties enforced
- Access attempts logged

---

## Tamper Protection

- Append-only logging
- No in-place updates
- Integrity checks on log ingestion
- Alerts on unexpected modifications

---

## Failure Handling

If audit logging fails:
- Actions are blocked
- Incident routed to analyst review
- Failure event logged locally and retried

No execution proceeds without audit coverage.

---

## Audit Review and Reporting

Audit logs support:
- Timeline reconstruction
- Approval verification
- Compliance reporting
- Root cause analysis
- Continuous improvement

---

## Key Takeaway

SARE audit logging ensures that **every automated or assisted action can be reconstructed, explained, and defended** — supporting trust, governance, and compliance.
