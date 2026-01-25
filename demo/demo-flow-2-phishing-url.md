# Demo Flow 2 — Phishing URL Investigation

This demo illustrates how SARE processes a suspected phishing URL detected by Microsoft Sentinel, from initial alert through enrichment, decisioning, and governed response.

The objective of this demo is to demonstrate:
- Consistent handling of phishing indicators
- Evidence-based classification of URLs
- Safe response recommendations
- Governance and analyst control over remediation actions

---

## Scenario Overview

**Detection Type:** Phishing URL / Malicious Link  
**Source:** Microsoft Sentinel (Email security / URL intelligence)  
**Primary Entity:** URL  
**Secondary Entities:** User mailbox, sender domain, IP address

### Example Indicators
- URL observed in phishing email
- Domain newly registered or low reputation
- URL resolves to known phishing infrastructure
- Multiple users received the same message

Sentinel generates an incident when a URL meets phishing risk criteria.

---

## Step 0 — Signal Intake (Sentinel)

SARE is triggered via a Sentinel automation rule when the phishing incident is created.

### Input Artifacts
- Sentinel Incident ID
- Detection rule metadata
- URL entity and related entities (user, mailbox, sender)

### Outcome
- Incident accepted for processing
- Case key generated
- Initial case object created

(Architecture reference: **Core Engine – Intake & Normalization**)

---

## Step 1 — Case Normalization

SARE normalizes the phishing signal into the internal case model.

### Actions
- Normalize URL (scheme, punycode, redirects if known)
- Resolve affected users and mailboxes
- Correlate sender domain and IP addresses

### Output
- Stable CaseId
- Normalized URL and entity relationships
- Timeline initialized

(Architecture reference: **Case Model**)

---

## Step 2 — Evidence Collection

SARE gathers context using bounded enrichment queries.

### Example Evidence Sources
- URL reputation (threat intelligence provider)
- Domain age and registration details
- Email delivery logs (who received and clicked)
- Historical incidents involving the same URL or domain

### Constraints
- Approved enrichment sources only
- Time-bounded searches
- Evidence provenance recorded

### Output
- Evidence list with confidence indicators
- Evidence gaps identified (if any)

(Architecture reference: **Decision Pipeline – Stage 2**)

---

## Step 3 — Deterministic Risk Evaluation

SARE evaluates phishing risk using deterministic logic.

### Risk Factors (examples)
- URL flagged as phishing by TI
- Domain age < 30 days
- Multiple recipients impacted

### Mitigating Factors (examples)
- No clicks observed
- URL blocked by email gateway

### Output
- Risk score
- Impact score
- Confidence level

(Architecture reference: **Decision Pipeline – Stage 3**)

---

## Step 4 — Agent Assistance (Bounded)

AI agents assist after deterministic scoring completes.

### Agent Contributions
- Draft phishing incident summary
- Highlight affected users and scope
- Suggest response actions (non-executing)

### Constraints
- No authority to change scores
- No execution capability
- Outputs validated before use

(Architecture reference: **Agent Architecture**)

---

## Step 5 — Policy & Guardrail Enforcement

SARE validates recommended actions against governance rules.

### Guardrail Checks
- Email quarantine and purge permissions
- User notification requirements
- Approval required for mailbox-wide actions

### Outcome
- High-impact actions gated for approval
- Informational actions permitted

(Architecture reference: **Governance / Guardrails**)

---

## Step 6 — Decision & Routing

Based on evaluation results:

### Routing Outcome (example)
- Route to analyst review
- Recommend URL blocking and email purge (approval required)
- Allow safe actions (tag incident, notify users)

### Output
- Decision record
- Action plan with approval requirements
- Evidence pack for analyst review

(Architecture reference: **Decision Pipeline – Stage 6**)

---

## Step 7 — Execution & Feedback

### Actions
- Sentinel incident updated with:
  - Phishing classification
  - Impacted users
  - Recommended remediation steps
- Optional playbook execution after approval

### Feedback Loop
- Analyst confirms phishing outcome
- Effectiveness metrics recorded
- Data used to tune future detections

(Architecture reference: **Execution & Feedback Loop**)

---

## Result Summary

This demo demonstrates that:
- Phishing URLs are handled consistently and safely
- Evidence drives classification, not assumptions
- AI assists without triggering destructive actions
- Governance and approvals prevent overreach

---

## Screenshot Placeholders

Recommended screenshots:
- Phishing incident overview
- URL reputation enrichment
- Affected users list
- SARE decision summary
- Approval-gated remediation request

Screenshots should be stored under:
`demo/screenshots/`

---

## Key Takeaway

SARE converts phishing alerts into structured, explainable, and governed response workflows — reducing analyst workload while maintaining security control.
