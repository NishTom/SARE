# Decision Pipeline

This document defines the deterministic decision pipeline SARE uses to triage cases and decide routing and response actions.

## Pipeline Objectives

- Create consistent triage outcomes from similar inputs
- Maintain explainability and auditability
- Enforce governance before any automation
- Reduce analyst time by producing a structured evidence pack

## Pipeline Stages

### Stage 0 — Intake and Validation
Inputs:
- Incident/alert payload
- Entities list
- Trigger metadata (rule name, severity, time range)

Checks:
- payload completeness
- supported entity types
- scope (tenant/workspace)
- duplication (already processed case key)

Outputs:
- accepted case key
- initial case object

### Stage 1 — Normalization and Correlation
Actions:
- normalize identifiers (UPN/email/name)
- resolve entity relationships (user↔device↔IP↔URL)
- correlate to recent related incidents (optional)

Outputs:
- normalized entities list
- correlation summary (if any)

### Stage 2 — Evidence Collection
Actions:
- run bounded enrichment queries (KQL)
- fetch contextual attributes (role tier, asset criticality)
- add TI reputation lookups (optional)

Constraints:
- bounded time window
- query templates only (no freeform queries in production mode)
- provenance required for each evidence item

Outputs:
- evidence list (with sources)
- missing evidence list (gaps)

### Stage 3 — Risk Scoring (Deterministic)
Actions:
- compute risk score from weighted factors
- compute impact score (asset tier, privileged role)
- compute likelihood score (signal strength, correlation)

Example factors:
- privileged identity involvement
- known-bad indicator match
- impossible travel / risky sign-in flags
- suspicious URL domain reputation
- repeated failures / brute force patterns

Outputs:
- numeric scores
- factor breakdown (explainability)

### Stage 4 — Confidence and Quality Gates
Actions:
- evaluate evidence completeness
- check data freshness
- check false positive indicators

Outputs:
- confidence level (high/med/low)
- recommended next evidence collection (if low)

### Stage 5 — Policy & Guardrail Enforcement
Actions:
- map proposed actions to policy requirements
- enforce approval model (human-in-loop)
- validate blast radius (single user vs group vs tenant-wide)
- validate playbook permissions and scopes

Outputs:
- allowed action set
- blocked action set (with reasons)
- approval requirement

### Stage 6 — Decision and Routing
Routing outcomes:
- Auto-Resolve: only for strictly defined false positives
- Triage: analyst review needed
- Approval Queue: high impact action recommended
- Execute (low-risk): permitted by policy (e.g., tag, comment, notify)
- Escalate: critical incidents requiring immediate senior review

Outputs:
- final decision record
- routing instructions

### Stage 7 — Execution and Feedback Loop
Actions:
- update Sentinel incident (severity, tags, owner, comments)
- trigger playbooks if allowed
- generate evidence pack for analyst handoff
- store audit log entry

Feedback:
- record final analyst outcome (true/false positive)
- capture action effectiveness signals
- adjust weights and rules via controlled change process

## Decision Record (Logical)

Each pipeline run should emit:
- case key
- timestamps (intake, decision, execution)
- inputs (rule, entities, key fields)
- evidence references (queries, TI hits)
- scores (risk, impact, confidence)
- policy checks (pass/fail)
- routing outcome
- actions requested/executed
- approvals (who/when) if applicable

## Explainability Requirements

Every decision must be explainable via:
- factor breakdown
- evidence pointers
- policy mapping for blocked/allowed actions
- short analyst-ready narrative

## Failure Handling

If the pipeline fails at any stage:
- do not execute actions
- route to analyst review
- log failure reason and missing data
- optionally retry evidence stage once (bounded)

## Governance Notes

- Deterministic scoring is the “source of truth” for routing.
- AI outputs must never bypass guardrails.
- Production mode uses approved templates and bounded queries only.
