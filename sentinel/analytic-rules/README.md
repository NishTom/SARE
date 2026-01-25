# Sentinel Analytic Rules — SARE Integration

This folder describes how Microsoft Sentinel analytic rules feed signals into SARE.

SARE does not replace detection logic. Instead, it consumes Sentinel incidents created by existing or custom analytic rules.

---

## Supported Detection Categories

### Identity-Based Detections
Examples:
- Risky sign-in
- Impossible travel
- MFA fatigue
- Privileged account misuse

Tables commonly used:
- SignInLogs
- AADNonInteractiveUserSignInLogs
- IdentityInfo

---

### Email and Phishing Detections
Examples:
- Phishing URL detected
- Suspicious sender domain
- Malicious attachment

Tables commonly used:
- EmailEvents
- EmailUrlInfo
- CloudAppEvents

---

## Integration Pattern

1. Analytic rule triggers and creates an incident
2. Sentinel automation rule fires on incident creation
3. Automation triggers SARE intake workflow
4. SARE enriches, scores, and routes the case

---

## Rule Design Guidance

- Prefer incident-based rules (not alert-only)
- Include clear entity mappings (User, IP, URL)
- Avoid overly broad detections
- Ensure severity reflects potential impact

---

## Example Mapping

| Analytic Rule | SARE Demo |
|-------------|-----------|
| Risky Sign-in Rule | Demo Flow 1 |
| Phishing URL Rule | Demo Flow 2 |

---

## Future Enhancements

- Versioned analytic rule templates
- Rule metadata annotations for SARE routing
- Automated rule-to-decision profile mapping
