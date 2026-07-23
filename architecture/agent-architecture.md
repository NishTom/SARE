# Agent Architecture

> **HISTORICAL.** Superseded by current-state.md §4. Retained
> for decision history. Do not treat as current.

This document defines how AI agents are used within SARE, what responsibilities they hold, and the constraints under which they operate.

SARE uses **assistive, bounded agents** — not autonomous actors — to ensure safety, explainability, and governance.

## Design Principles

1. **Assist, not replace**
   - Agents support analysts and decision pipelines
   - Final authority remains with deterministic logic or humans

2. **Bounded capability**
   - Agents operate within explicitly defined scopes
   - No free-form execution or direct system control

3. **Explainability first**
   - Every agent output must be traceable to evidence
   - No opaque or ungrounded recommendations

4. **Governance by default**
   - Agent outputs are inputs to the decision pipeline
   - They cannot bypass guardrails or approvals

## Agent Types

### 1. Case Summarization Agent

Purpose:
- Convert raw incident data into a concise, analyst-ready narrative

Inputs:
- Normalized case object
- Evidence list
- Timeline events

Outputs:
- Structured summary (what happened, who/what involved, why it matters)
- Key evidence references

Constraints:
- No new facts may be introduced
- Must reference evidence IDs

---

### 2. Evidence Interpretation Agent

Purpose:
- Assist in interpreting enrichment data and patterns

Inputs:
- KQL results
- Threat intelligence lookups
- Identity and asset context

Outputs:
- Observed patterns and anomalies
- Highlighted risk indicators

Constraints:
- Cannot assign final risk scores
- Cannot override deterministic scoring

---

### 3. Recommendation Drafting Agent

Purpose:
- Propose response actions based on decision outcomes

Inputs:
- Decision pipeline output
- Allowed action set
- Policy context

Outputs:
- Ranked action recommendations
- Rationale for each recommendation

Constraints:
- Only propose actions explicitly permitted by policy
- Must include justification and expected impact

---

### 4. Evidence Pack Generator

Purpose:
- Produce structured documentation for analysts, approvals, or audits

Inputs:
- Case object
- Decision record
- Timeline

Outputs:
- Analyst handoff summary
- Approval justification packet
- Audit-ready evidence bundle

Constraints:
- No modification of source evidence
- No summarization without references

## Agent Invocation Model

Agents are invoked:
- After deterministic stages complete
- Only with validated, bounded inputs
- Only for specific, defined tasks

Agents never:
- Trigger playbooks
- Modify incidents directly
- Make approval decisions
- Execute actions

## Human-in-the-Loop Enforcement

Human review is mandatory when:
- Actions affect user access
- Actions impact production systems
- Confidence score is medium or low
- Policy explicitly requires approval

Agents may assist by:
- Drafting explanations
- Highlighting risks
- Suggesting safe next steps

## Failure and Safety Handling

If an agent:
- Produces ungrounded output
- Exceeds scope
- Fails validation checks

Then:
- Output is discarded
- Case is routed to analyst review
- Failure is logged for analysis

## Why This Architecture Is Safe

- Deterministic logic controls flow
- Agents are advisory, not authoritative
- Guardrails apply before and after agent output
- Every step is auditable

This ensures SARE delivers AI-assisted speed **without sacrificing control or trust**.
