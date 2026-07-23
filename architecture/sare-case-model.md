# SARE Case Model

> **HISTORICAL.** Superseded by current-state.md §7. Retained
> for decision history. Do not treat as current.

This document defines the normalized “case model” used by SARE to represent an investigation consistently across signals and tooling.

## Why a Case Model

Security platforms represent data differently (incidents vs alerts vs entities). A normalized case model provides:
- a stable structure to drive the decision pipeline
- reproducible evidence assembly
- clean handoffs to analysts, approvals, and playbooks
- consistent audit logs and reporting

## Core Case Object (Logical Structure)

A case is composed of:

### Case Metadata
- CaseId (stable key)
- Source (Sentinel incident, alert, custom intake)
- RuleName / DetectionName
- Workspace / Tenant scope identifiers
- CreatedTime / UpdatedTime
- CurrentState (New, Enriched, Routed, WaitingApproval, Executed, Closed)

### Entities
A set of normalized entities, each with:
- EntityType (User, Host, IP, URL, Mailbox, App, File, Process)
- PrimaryId (UPN, hostname, IP, URL)
- AlternateIds (display name, object ID, device ID)
- RoleContext (privileged role, tier classification)
- Ownership / criticality (if known)

### Timeline
Ordered events or evidence points:
- timestamped evidence items
- key state transitions
- analyst actions and approvals
- execution actions

### Evidence
Evidence items must include:
- EvidenceId
- SourceType (KQL, TI provider, directory lookup, Sentinel payload)
- SourceReference (query name + parameters, API endpoint, incident link)
- ObservedValue (normalized)
- Confidence (for the evidence item itself)
- Notes (short description)

### Scores
- RiskScore (0–100)
- ImpactScore (0–100)
- ConfidenceLevel (High/Med/Low)
- ScoreBreakdown (factors and weights)

### Decisions
- DecisionId
- RoutingOutcome
- RecommendedActions (ranked list)
- BlockedActions (with guardrail reasons)
- ApprovalRequired (true/false)
- ExplanationSummary (analyst-ready text)

### Governance
- PolicyVersion / GuardrailProfile
- Approvals (who, when, decision)
- AuditLogPointers (where logs are stored)
- DataRetentionClass (how long to retain)

## Case Key Strategy

A stable CaseId should be derived from:
- incident id + workspace id (preferred), or
- (rule + primary entity + time bucket) for non-incident sources

This prevents duplication and supports correlation.

## Entity Normalization Rules

Examples:
- If entity is provided as display name, attempt to resolve to UPN/objectId
- Hostnames normalized to lowercase, strip domain suffix where needed
- IP addresses normalized to canonical form
- URLs normalized (scheme, punycode, trailing slash handling)

Normalization output should preserve original values for audit.

## Evidence Provenance

Every evidence item must be reproducible:
- KQL evidence includes the query template name, time window, and parameters
- TI evidence includes provider name, lookup key, and timestamp
- directory evidence includes object IDs and retrieval timestamp

## Minimal Case Requirements

A case is considered “processable” if it has:
- a CaseId
- at least one primary entity
- a detection name
- a time window

If these are missing, the pipeline must route to manual triage.

## Extensibility

The case model is intentionally extensible to support:
- specialized cases (phishing, risky sign-ins, insider risk, data exfil)
- evidence packs tailored to different teams (SOC vs legal vs compliance)
- future agent modules that add domain-specific evidence
