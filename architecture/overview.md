# SARE Architecture Overview

This document provides a high-level overview of the SARE (Sentinel Autonomous Response Engine) architecture and explains how the major components fit together.

SARE is designed as a **governed, AI-assisted decision layer** that augments Microsoft Sentinel by improving triage consistency, response safety, and analyst efficiency.

## Architectural Goals

- Reduce mean time to triage without increasing operational risk
- Enforce governance and human-in-the-loop controls by design
- Produce explainable, auditable decisions
- Remain aligned with Microsoft Sentinel rather than replacing it

## High-Level Architecture

At a high level, SARE consists of the following logical layers:

1. **Signal Intake**
   - Sentinel incidents and alerts
   - Automation rule triggers
   - Optional external signals (future)

2. **Core Engine**
   - Case normalization
   - Evidence assembly
   - Deterministic risk evaluation
   - Policy and guardrail enforcement

3. **Decision Pipeline**
   - Structured multi-stage triage
   - Scoring, confidence checks, and routing
   - Approval gating and execution coordination

4. **Agent Layer**
   - Assistive AI agents for summarization, interpretation, and documentation
   - Strictly bounded and non-authoritative
   - Always subordinate to deterministic logic

5. **Execution & Feedback**
   - Sentinel incident updates
   - Playbook execution (where allowed)
   - Audit logging and outcome capture

## Component Responsibilities

| Component | Responsibility |
|---------|----------------|
| Core Engine | Orchestrates case processing and policy enforcement |
| Decision Pipeline | Determines routing and response outcomes |
| Case Model | Provides a normalized investigation structure |
| Agent Architecture | Assists analysts with bounded AI capabilities |
| Governance Layer | Enforces approvals, guardrails, and auditability |

## Key Architecture Documents

This overview is supported by the following detailed documents:

- **sare-core-engine.md** — Core responsibilities and Sentinel integration
- **decision-pipeline.md** — Deterministic triage and routing logic
- **sare-case-model.md** — Normalized case representation
- **agent-architecture.md** — Safe and governed AI agent design

## Governance by Design

Governance is embedded throughout the architecture:
- Deterministic logic controls routing
- Guardrails apply before and after agent involvement
- Human approval is mandatory for high-impact actions
- Every decision and action is logged

SARE treats automation as a privilege, not a default.

## Assumptions and Constraints

- SARE operates within enterprise RBAC and change management controls
- Sentinel remains the system of record for incidents
- Production execution uses approved queries and playbooks only
- AI agents never execute actions directly

## Non-Goals

SARE explicitly does not:
- Fully automate all incident response
- Replace SIEM or SOAR platforms
- Introduce black-box decisioning
- Bypass analyst oversight

## Evolution Path

SARE is designed to evolve safely:
- Capstone version: documentation-first, demo-driven
- MVP version: selective automation with approvals
- Future versions: broader agent specialization under governance

This architecture ensures SARE can scale responsibly while maintaining trust, control, and auditability.
