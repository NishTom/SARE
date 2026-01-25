# Architecture Overview

SARE (Sentinel Autonomous Response Engine) is a governed, modular automation layer that sits alongside Microsoft Sentinel to improve triage speed and response consistency without compromising safety or auditability.

## High-Level Components
- **Signal Intake**: Sentinel incidents/alerts and enrichment signals (threat intel, asset context)
- **Case Model**: Normalized object representing an investigation (entities, evidence, risk, decision state)
- **Decision Pipeline**: Deterministic steps that score, explain, and route cases
- **Agent Architecture**: AI assistant modules that draft summaries, recommend actions, and generate evidence packs
- **Governance**: Guardrails, approvals, logging, and human-in-the-loop enforcement

## Operating Principles
- Deterministic logic before automation
- Human approval for high-impact actions
- Every recommendation is explainable and logged
