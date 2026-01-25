# Demo Flow 1 — Risky Sign-in Investigation

This demo illustrates how SARE processes a risky sign-in scenario detected by Microsoft Sentinel, from signal intake through decision routing and governed response.

The objective of this demo is to demonstrate:
- Deterministic triage
- Evidence-backed decisioning
- Safe use of AI assistance
- Governance and human-in-the-loop enforcement

---

## Scenario Overview

**Detection Type:** Risky Sign-in  
**Source:** Microsoft Sentinel (Identity Protection / SignInLogs)  
**Primary Entity:** User account  
**Risk Indicator Examples:**
- Impossible travel
- Sign-in from unfamiliar location
- Elevated sign-in risk level
- Repeated authentication attempts

Sentinel generates an incident based on a detection rule identifying a high-risk authentication event.

---

## Step 0 — Signal Intake (Sentinel)

SARE is triggered via a Sentinel automation rule when the incident is created.

### Input Artifacts
- Sentinel Incident ID
- Alert metadata (rule name, severity, timestamps)
- Initial entity set (user, IP address, location)

### Outcome
- Incident accepted for processing
- Case key generated
- Initial case object created

(Architecture reference: **Core Engine – Intake & Normalization**)

---

## Step 1 — Case Normalization

SARE normalizes the incoming incident into its internal case model.

### Actions
- Resolve user identifier (UPN, display name, object ID)
- Normalize IP address and geolocation
- Associate related entities (user ↔ IP ↔ device if available)

### Output
- Stable CaseId
- Normalized entity list
- Timeline initialized

(Architecture reference: **Case Model**)

---

## Step 2 — Evidence Collection

SARE executes bounded enrichment queries to gather context.

### Example Evidence Sources
- SignInLogs (risk level, authentication method, location)
- Identity posture (MFA status, privileged role membership)
- Historical sign-in behavior (recent successful logins)
- Threat intelligence reputation for source IP (optional)

### Constraints
- Time-bounded queries
- Approved query templates only
- Evidence provenance recorded

### Output
- Evidence list with source references
- Identified evidence gaps (if any)

(Architecture reference: **Decision Pipeline – Stage 2**)

---

## Step 3 — Deterministic Risk Evaluation

SARE evaluates risk using deterministic scoring logic.

### Risk Factors (examples)
- Sign-in risk level = High
- Location anomaly detected
- Privileged role associated with account

### Mitigating Factors (examples)
- MFA challenge succeeded
- No prior malicious activity for IP

### Output
- Risk score
- Impact score
- Confidence level

(Architecture reference: **Decision Pipeline – Stage 3**)

---

## Step 4 — Agent Assistance (Bounded)

AI agents are invoked only after deterministic evaluation completes.

### Agent Contributions
- Draft analyst summary of the sign-in event
- Highlight key evidence and anomalies
- Suggest possible response actions (non-executing)

### Constraints
- No scoring authority
- No action execution
- Outputs validated before use

(Architecture reference: **Agent Architecture**)

---

## Step 5 — Policy & Guardrail Enforcement

SARE validates recommended actions against governance rules.

### Guardrail Checks
- User account impact
- Privileged role implications
- Approval requirements
- Allowed response actions

### Outcome
- High-impact actions gated for approval
- Low-impact actions permitted

(Architecture reference: **Governance / Guardrails**)

---

## Step 6 — Decision & Routing

Based on risk, confidence, and policy:

### Routing Outcome (example)
- Route to analyst review
- Recommend temporary account suspension (approval required)
- Allow safe actions (tag incident, notify SOC)

### Output
- Decision record
- Action plan with approval requirements
- Analyst-ready evidence pack

(Architecture reference: **Decision Pipeline – Stage 6**)

---

## Step 7 — Execution & Feedback

### Actions
- Sentinel incident updated with:
  - SARE decision summary
  - Tags (RiskySignIn, ApprovalRequired)
  - Recommended next steps
- Optional playbook execution after approval

### Feedback Loop
- Analyst outcome recorded
- Decision effectiveness logged for future tuning

(Architecture reference: **Execution & Feedback Loop**)

---

## Result Summary

This demo demonstrates that:
- SARE does not auto-disable accounts blindly
- Decisions are evidence-backed and explainable
- AI assists without bypassing controls
- Human approval is enforced for high-risk actions

---

## Screenshot Placeholders

The following screenshots can be added to support the demo:
- Sentinel incident overview
- Evidence enrichment results
- Decision summary comment
- Approval request view

Screenshots should be placed under:
`demo/screenshots/`

---

## Key Takeaway

SARE transforms a raw risky sign-in signal into a governed, auditable, and explainable response workflow — improving analyst efficiency while preserving trust and control.
