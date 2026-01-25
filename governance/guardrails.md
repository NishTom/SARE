# Guardrails

SARE guardrails ensure actions are safe, explainable, and compliant.

## Key Guardrails
1. **Action Gating**
   - No disruptive action without explicit approval (unless pre-approved low-risk)
2. **Evidence-First**
   - Every recommendation includes evidence pointers and reasoning
3. **Least Privilege**
   - Minimal permissions for playbooks and connectors
4. **Policy Constraints**
   - Block actions outside defined scope (tenants, entities, time windows)
5. **Auditability**
   - Log inputs, decision steps, outputs, and approvals

## Enforcement
- Decision pipeline validates guardrails before any action request is sent to automation.
