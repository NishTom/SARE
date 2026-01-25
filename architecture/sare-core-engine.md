# SARE Core Engine

This document defines the core engine of SARE (Sentinel Autonomous Response Engine): what it is responsible for, how it operates, and how it integrates with Microsoft Sentinel.

## Purpose

SARE Core Engine is the orchestration and decision backbone that converts a security signal (incident/alert) into:
- a normalized case object
- an evidence-backed assessment
- a recommended action plan
- a governed execution path (human approval or automated playbook)

The core engine is designed to be deterministic-first, with AI used to assist summarization, enrichment interpretation, and recommendation drafting.

## Inputs

Primary inputs:
- Sentinel incident trigger payload (incident metadata, entities, alert linkages)
- Sentinel alert details (if applicable)
- Entity context (user, host, IP, URL, mailbox, device identifiers)

Optional enrichment inputs:
- Identity posture (MFA state, PIM context, sign-in risk, impossible travel indicators)
- Asset criticality (tiering, ownership, business impact)
- Threat intelligence (reputation, known-bad indicators, campaign mapping)
- Historical context (similar incidents, prior outcomes, suppressed false positives)

## Outputs

Core outputs produced per case:
- Case object (normalized): entities, evidence pointers, timestamps, risk signals
- Decision record: score, explanation, policy checks, confidence, routing outcome
- Action plan: recommended response steps (with guardrail gating)
- Evidence pack: analyst-ready narrative and supporting data references

## Core Responsibilities

### 1. Case Normalization
Convert platform-specific payloads into a stable internal model:
- Deduplicate and resolve entities
- Normalize identifiers (UPN vs display name, hostnames, IP formats)
- Create stable case keys for correlation and timeline stitching

### 2. Evidence Assembly
Build an evidence graph from:
- Sentinel incident/alert content
- KQL query results (enrichment queries)
- External enrichment providers (if configured)

Evidence assembly must:
- preserve provenance
- be time-bounded
- be reproducible

### 3. Risk Evaluation
Produce a structured risk view:
- risk factors
- mitigating factors
- contextual weighting (asset tier, user role, scope)

### 4. Policy & Guardrail Validation
Before any response recommendation is allowed:
- validate scope constraints
- validate approval requirements
- validate least privilege constraints
- validate safety constraints

### 5. Routing and Execution Coordination
Based on decision outcome:
- Auto-close (strict false positives only)
- Analyst triage
- Approval queue
- Execute low-risk actions
- Execute high-risk actions after approval

## Integration with Microsoft Sentinel

SARE aligns with Sentinel through:
- Incident-triggered workflows
- KQL-based enrichment
- Sentinel playbooks for response
- Centralized audit logging

## Non-Goals

The SARE Core Engine does not:
- replace Sentinel
- bypass RBAC
- execute irreversible actions without approval
- hide logic inside opaque AI outputs

## Success Criteria

A successful run produces:
- a normalized case
- reproducible evidence
- explainable decisions
- guardrail-compliant actions
- predictable routing
