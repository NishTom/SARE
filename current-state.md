# SARE — Current State of Truth

**Status:** Authoritative. Where any other document in this project
conflicts with this file, this file wins.
**Last updated:** 2026-07-23
**Owner:** Nishant Tomar

> Every other document here was written at a different point in SARE's
> evolution. Read this first. Anything not reconciled against this file
> should be treated as historical.

---

## 1. Identity

**SARE — Security Autonomy Reasoning Engine.**

Not "Sentinel Autonomous Response Engine." That expansion appears in
`overview.md`, `sare-core-engine.md`, `market-strategy.md`, the HLD, and
the Concept Note. All five are stale on this point.

---

## 2. What SARE is

A vendor-agnostic governance and delegation layer that sits **above**
security AI tools (Microsoft Security Copilot, CrowdStrike Charlotte AI,
SOAR platforms). It decides which actions an AI agent may take
autonomously, which require human approval, and records every decision
against a versioned policy so it can be replayed later.

**What SARE is not:**
- Not a detection engine. It does not write or run detections.
- Not an autonomous agent. It does not act freely.
- Not a SOAR replacement. It governs orchestration; it is not one.
- Not a chatbot.

**Research contribution:** decision replay under policy version —
re-running historical decisions against amended policy to measure what
would have changed. This is the publishable claim.

---

## 3. Build status — state this honestly, everywhere

| | |
|---|---|
| Architecture | Specified across 11 markdown files and one HLD |
| Working software | **None** |
| Demo flows | Documented walkthroughs, not executing systems |
| Code | No repository exists |
| The one implementation artefact | A KQL query with `ConfidenceScore = 0.72` hard-coded as a literal, alongside synthetic context and a hard-coded account and IP. Nothing is computed. |
| Diagrams | Seven placeholders in the HLD; one conceptual PNG exists |

**TRL: 2–3.** Technology concept formulated, moving toward analytical
proof of concept. **Not 4–5.** TRL 4 requires component validation in a
laboratory environment; nothing here has been validated because nothing
here runs.

The `SARE_Chehri_Briefing_v3.docx` claim of "TRL 4–5 with two working
demonstration flows completed in applied environment" is **not
supportable** and must not be repeated in any future proposal,
briefing, or conversation. Correct it in the next contact with
Dr. Chehri rather than letting it stand.

Do not present `SARE_AI_Agent___Decision_Context_Generation_Logic.jpg`
to anyone technical. Separately: verify `185.199.110.153` before it
appears anywhere — it falls in GitHub's Pages range.

---

## 4. Agent architecture — five advisory agents

Current design, matching `SARE_Conceptual_Research_Architecture.png`:

| Agent | Produces |
|---|---|
| Context | Case framing and environmental context |
| Evidence | Enrichment and corroboration |
| Risk | Risk assessment input |
| Policy | Applicable policy evaluation |
| Explanation | Human-readable rationale |

**All five are advisory.** No agent has execution authority. No agent
assigns a final risk score. Deterministic scoring remains the source of
truth for routing.

Superseded: the four advisory agents in `agent-architecture.md`, and the
four operational agents in HLD §7 including an executing Response Agent.
The HLD's "LLM Reasoner" framing — where the Risk Agent owns
`risk_score` and `confidence_score` — is **not** current.

---

## 5. Autonomy posture

**Human-in-the-loop is the default.** Autonomy is granted per action
class by policy, never assumed.

The Concept Note position is current: SARE does not act freely; a
Phase 1 pilot runs with no automated actions.

The HLD position is **superseded** — "autonomous-first Tier-1 SOC
decision-and-response orchestrator," "replaces repetitive Tier-1
workflows," and the "Aggressive Autonomy" flow are all obsolete.

Where the same scenario resolves differently between HLD §8.1/§8.2 and
`demo-flow-1`/`demo-flow-2`, **the demo flows are current.** Account
suspension and mail deletion are approval-gated.

Where `approval-model.md` and HLD §7.3 disagree on action
classification, **`approval-model.md` is current.** Blocking a single
IP or URL is Conditional — approval required unless pre-approved.

---

## 6. Scoring model

**Confidence is continuous, 0.0–1.0.** Not High/Medium/Low. The
autonomy gating table requires a continuous value to threshold against
action risk; the categorical form in `sare-case-model.md` and both demo
flows cannot support it.

**Risk is continuous, 0–100.**

HLD §7.2's example weights (+25, +15, +25, +20, +20) sum to 105 against
a 0–100 scale with no normalisation rule. This is a defect, not a
design.

> **[DECIDE]** Confidence thresholds — the boundary values at which an
> action class becomes autonomous, conditional, or mandatory-approval.
> No document in this project contains a number. Until these exist the
> gating table cannot be implemented or tested, and this is the first
> thing an incoming architect will ask for.

> **[DECIDE]** Risk score normalisation rule.

---

## 7. Case model extension — the research fix

**This is the most consequential change in this document.**

The current `sare-case-model.md` provides RiskScore, ImpactScore,
ConfidenceLevel and a governance block. The four-dimension trust model
(the project's central research contribution) requires four independent
values, and the current model collapses or omits three of them.

Required fields:

| Dimension | Field | Notes |
|---|---|---|
| Technical confidence | `confidence_score` | Continuous 0.0–1.0 |
| Action risk | `action_risk` | **New.** Distinct from asset impact. Risk of *taking the action*, not risk of the incident. |
| Policy permission | `policy_outcome` | **New.** First-class value: permitted / conditional / denied, with the `policy_version` that produced it. |
| Human trust | operator telemetry | **New.** See below. |

Operator interaction telemetry — none of this is currently captured,
and without it the trust-calibration dataset cannot exist:

- `time_to_decision` — presentation to operator action
- `operator_action` — approved / rejected / modified / escalated
- `override_flag` and `override_reason` (free text)
- `questions_asked` — which explanation elements the operator requested
- `explanation_version` — which explanation was shown

**Without these fields there is no dataset, and without a dataset there
is no research project.** This is the gap that makes the measurement
plan writable.

---

## 8. Logging and retention — research path

Two provisions in `audit-logging.md` actively block the research and
are **superseded for any case in the research corpus**:

- **Retention.** 90–180 days is insufficient. A study measuring trust
  change over time must retain for the full project duration plus the
  publication window. Set research-corpus retention accordingly.
- **Agent output.** Logging a "summary hash (not raw text if
  restricted)" makes explanation-usefulness analysis impossible.
  Agent output in the research path is stored **raw**.

This creates two logging tiers — operational and research — with
different retention and redaction rules. That distinction does not
currently exist in any document and needs to be designed.

> **[DECIDE]** How research-corpus cases are consented, de-identified,
> and separated from operational data. This is also the REB question.

---

## 9. Scope

**Out of scope, current:**
- Multi-tenant MSSP packaging (HLD §3.2). `pricing-model.md` offers
  MSSP licensing — that file is commercialisation-pathway material,
  not current scope.
- Custom SOC UI portal as a replacement for the operations console.

> **[DECIDE]** Platform positioning. `market-strategy.md` sells
> "Sentinel-native" as *the* differentiator. The Chehri briefing sells
> "not vendor-locked." Both are public-facing and mutually undermining.
> Pick one. Recommended framing: platform-neutral by design, Sentinel
> as the first reference implementation — but that has to be a decision,
> not a hedge.

> **[DECIDE]** Where Talk to Data lives. HLD non-goal says no custom UI;
> the conceptual architecture makes the Operator Trust Interface a
> distinct layer. Also confirm which Talk to Data is current — the
> NL-to-KQL product in `Talk-to-Data-Business-Case.docx` is a different
> system from the decision-record interrogation surface in the IDEaS
> material, and they share a name.

> **[DECIDE]** LLM hosting and model. The HLD names the component but
> makes no decision. Azure AI Foundry appears only in the Concept Note;
> GPT-4o only in the Talk to Data business case.

> **[DECIDE]** Policy ruleset format. The policy ruleset is the central
> object of the entire system and is defined in no document — no schema,
> no DSL, no example, no versioning mechanism beyond a `policy_version`
> field. Decision replay is impossible to implement without it.

> **[DECIDE]** UAV / IoE / public-safety generalisation. Asserted in
> four documents with zero supporting examples. Either write one
> concrete non-cyber decision example, or remove the claim everywhere.

---

## 10. Document status

| Document | Status |
|---|---|
| `decision-pipeline.md` | **Current.** Most reusable file in the project. |
| `guardrails.md` | Current in intent; unimplemented. |
| `sare-core-engine.md` | Current, Sentinel-coupled. Stale name. |
| `approval-model.md` | Current on action classification. |
| `demo-flow-1` / `demo-flow-2` | Current on scenario resolution. |
| `sare-case-model.md` | **Superseded by §7 of this document.** |
| `agent-architecture.md` | **Superseded by §4.** |
| `SARE_HLD_Enterprise_v1_1.docx` | **Historical — re-version as v0.** Oldest and most aggressive document; contradicts current posture on autonomy, agents, and action classification. |
| `SARE-Enterprise-Concept-Note.docx` | Current on autonomy posture. §8.2/§8.4 baseline metrics unsupported — remove or source. |
| `SARE_Chehri_Briefing_v3.docx` | **Historical.** IDEaS vehicle, not pursued. TRL claim incorrect. |
| Deep-dive §§1, 3, 4, 5, 7, 8, 10 | **Current and valuable.** Vehicle-independent. §4 trust model is the core intellectual asset. |
| Deep-dive, all IDEaS-specific sections | Historical. |
| `market-strategy.md`, `go-to-market.md`, `pricing-model.md` | Historical as product strategy; retain as commercialisation-pathway evidence. Add a header saying so. |
| `Talk-to-Data-Business-Case.docx` | Different system. Rename or separate. |
| `v1-capstone.md`, `v2-mvp.md`, `future.md` | Empty. Delete. |
| `README.md` | Naming convention for captures that don't exist. |

---

## 11. External dependencies — not solvable by writing

| | Status |
|---|---|
| Academic supervisor confirmed | **Unknown.** Dr. Chehri redirected to Mitacs; no record he offered to supervise. |
| Named intern | **None.** |
| IP ownership — company or individual | **Undocumented.** |
| Partner cash capacity | **Unbudgeted.** |

## 12. Status log

- **2026-06-17** — IDEaS call not submitted. Dr. Chehri was not
  supportive of the IDEaS vehicle and recommended Mitacs Accelerate
  Defence and Security instead. All IDEaS-targeted framing is
  historical from this date.
- **2026-07-23** — Mitacs Accelerate D&S (deadline 2026-08-17) not
  pursued this cycle. No named intern, no confirmed supervisor, no
  research design, no budget; submission is non-revisable. Target a
  future cycle with a named student and a working prototype.
