# SARE Sentinel KQL Library

This folder contains KQL used by SARE for enrichment, evidence collection, and reporting.

## Design Rules
- Queries must be time-bounded (no unbounded searches)
- Queries should be reusable via parameters (time range, entity)
- Prefer projecting only fields needed for decisioning and audit
- Output should be stable and parseable

## Files
- risky-signin-enrichment.kql — Enrich risky sign-in incidents with identity + sign-in context
- phishing-url-enrichment.kql — Enrich phishing URL incidents with URL/email context

## Usage Pattern (SARE)
1. Intake incident entities (UPN, IP, URL, etc.)
2. Run enrichment queries with bounded time windows
3. Store results as evidence with provenance
4. Feed deterministic scoring and governance checks
