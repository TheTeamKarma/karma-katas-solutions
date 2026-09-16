# ADR-007: Every AI feature falls back to a non-AI version

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-006](ADR-006-ai-gateway.md), [ADR-008](ADR-008-ai-autonomy-levels.md)

## Context

AI can be unavailable, too costly, or wrong. The estate has almost no history on day one, so AI can't be relied on from the start. The brief asks how AI misbehaviour in production is handled.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| AI features stop when AI is unavailable | Simple | Estate operations depend on AI providers |
| A second AI model or provider as the only fallback | Keeps AI quality during a single provider outage | Fails when both providers or the whole AI layer fail; no baseline to prove AI adds value; higher cost |
| **Each feature starts as rules, improves with AI, and falls back to its rules** | Works from day one; resilient; a baseline to compare AI against | Two versions of each feature to maintain |

## Decision

Every AI feature **degrades gracefully** (graceful degradation): it has a **day-one version without AI**, which is also its **fallback**, and an **off switch** for staff. The fallback for every AI use is listed in the [AI register](../docs/ai/README.md#every-ai-use-at-a-glance), for example: animal health warnings → keeper-set rules; companion → prepared answers and offline guide; food tray estimates → keeper-entered leftovers; insight assistant → dashboards. Fallback is triggered by provider failure, budget hard stop, failed quality checks, or staff switching the feature off. Where possible the rules version runs alongside the AI as a baseline. **The estate keeps running with no AI at all.**

## Consequences

**Positive**

- Useful from opening day despite no history
- Provider outages and price rises never stop operations
- AI must keep beating its baseline to stay switched on

**Negative**

- Rules versions need maintaining → **Mitigation:** They are simple and double as test baselines

**Risks**

- Staff forget how to work without AI: fallback drills twice a year

## How we'll know it was right

Every AI feature has a tested fallback; switching a feature off causes no loss of core operations; each AI feature beats its baseline on its production signal.
