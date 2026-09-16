# ADR-017: Privacy by design: anonymous sensing and minimal personal data

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-006](ADR-006-ai-gateway.md), [ADR-012](ADR-012-visitor-companion.md)

## Context

The estate counts crowds, serves families with children, runs a companion that sees questions and location, and sends data to external AI providers. The region isn't stated, so the design meets GDPR standard (which also meets UK GDPR).

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Camera video analysed in the cloud for crowds | Rich data | Images of visitors including children leave the estate; bandwidth |
| Tracking phones by Wi-Fi or Bluetooth signals | Cheap | Device IDs are personal data; unreliable |
| **Counting inside devices, minimal data, consent, personal data removed before AI** | Low privacy risk; lower bandwidth; simpler compliance | Less detailed movement data |

## Decision

| Principle | Decision |
|---|---|
| Crowd sensing | People counters and crowd cameras count **inside the device** and send numbers only; images of visitors never leave the device |
| Location | Known on the phone; shared only as a **zone**, only with consent |
| Minimal data (data minimisation) | Ticket type instead of age; gender optional and used **only in anonymous reports**, never in offers or pricing |
| Prices | Never based on personal characteristics |
| AI providers | Personal details removed by the AI gateway (PII redaction); providers contracted not to train on our data; hosting region set by us |
| Companion | Conversations stay on the phone; server logs with personal data removed, kept 30 days |
| Children | No child accounts; no profiling |
| Payments | Card data only with the payment provider |
| Consent | Offers, marketing and location sharing are opt-in |
| Impact assessment | A data protection impact assessment (DPIA) before launch; a data protection officer (DPO) role owns it and data-protection requests |

## Consequences

**Positive**

- Low risk for a family audience
- Meets GDPR standard in any likely region
- Less data to secure and store

**Negative**

- Less detailed visitor movement data for popularity insight → **Mitigation:** Zone counts, time spent, waits and feedback are enough for investment decisions
- Personal data removal from free text is best effort → **Mitigation:** Provider contracts are the firm control; sampled log checks

**Risks**

- New features quietly collect more data: DPIA updated for every new data field

## How we'll know it was right

Sampled audits find no visitor images or exact locations leaving devices, and no personal data in AI logs; data-protection requests answered within legal deadlines.
