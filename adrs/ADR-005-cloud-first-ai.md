# ADR-005: Run AI in the cloud; no AI servers on site

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-003](ADR-003-local-alarm-path.md), [ADR-006](ADR-006-ai-gateway.md), [ADR-013](ADR-013-managed-ai-services.md)

## Context

Patchy Wi-Fi suggests running AI on site, but on-site AI hardware adds cost, power, cooling and upkeep for a small IT team, and makes switching models harder. The AI uses (health trends, piranha estimate, forecasts, companion) work over hours or days, or on visitors' own connections.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| AI servers on site | Works without internet | Cost, power, cooling, upkeep; hard to update models |
| **Cloud AI by default, with a strict test for on-site AI** | Pay per use; no hardware; easy to switch providers | AI features need connectivity |
| AI models running on phones | No server cost | Heavy for older phones; hard to update and check |

## Decision

AI runs **in the cloud by default**. On site we use only simple rules on the edge devices and AI already built into bought devices (e.g. counting inside people counters). Dedicated AI hardware on site is used **only if all three hold**: people on the ground need the result right now, it must work without connectivity, and it's too heavy for rules, devices or phones. **No current AI use passes this test.**

## Consequences

**Positive**

- Lowest running cost and upkeep
- Providers and models switched centrally
- Urgent needs covered by rules on site (ADR-003)

**Negative**

- AI features pause when the estate is offline → **Mitigation:** Every AI feature has a non-AI fallback (ADR-007); AI results are needed over hours, not seconds

**Risks**

- A future AI use genuinely needs on-site inference: the test is re-applied; the edge devices could host a small model

## How we'll know it was right

No AI use passes the on-site test at annual review, so none needs AI hardware on site; AI outages cause no loss of alarms, entry or animal records.
