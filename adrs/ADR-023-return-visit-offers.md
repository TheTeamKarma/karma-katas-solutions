# ADR-023: Return-visit offers: rules first, prediction later, measured against a holdout group

**Status:** Accepted · **Date:** 2026-09-15 · **Related:** [ADR-008](ADR-008-ai-autonomy-levels.md), [ADR-015](ADR-015-validating-ai-results.md), [ADR-017](ADR-017-privacy-by-design.md), [ADR-019](ADR-019-pricing-and-capacity.md)

## Context

The brief names return visits as a business challenge: "we want to get more returning visitors too, but aren't sure how". The estate has no visit history at opening, a family audience, and GDPR-standard consent rules. Offers cost money through discounts, so an offer that goes to people who would have come back anyway loses revenue.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| The same seasonal messages to everyone | Simple; no data needed | Weak effect; discounts wasted on people who would return anyway |
| Prediction of who will return from day one | Targeted | No history to learn from; can't be validated at launch |
| **Rule-based segments first, propensity modelling after a season, every campaign measured against a holdout group** | Works from opening day; improves with data; proves whether offers work | A share of consenting visitors never gets an offer; needs a season of history |
| Buy a marketing platform with built-in AI targeting | Quick to start | Visitor data leaves our platform; its models can't be checked against our holdout group |

## Decision

| Aspect | Decision |
|---|---|
| Year 1 | **Rule-based segments**: family ticket buyers → school holiday events; repeat day visitors → annual pass upgrade with today's ticket credited; pass holders → what's new |
| Later | **Propensity modelling** (supervised classification) run nightly with managed machine learning once a season of visit history exists; **uplift modelling** considered once enough campaigns have run, so offers go to people they actually change |
| Offer text | Generative AI drafts; **marketing approves every campaign** ([ADR-008](ADR-008-ai-autonomy-levels.md)) |
| Measurement | A random **holdout group** (control group) of about **10%** of consenting visitors gets no offers; return and upgrade rates are compared to measure uplift. A segment's offers stop if they show no uplift over two campaigns, each with a minimum sample agreed with marketing |
| Consent and privacy | Consenting visitors only; no gender, age or children's data used for targeting; unsubscribe at any time ([ADR-017](ADR-017-privacy-by-design.md)) |
| Fallback | Rule-based segments and standard seasonal messages |

## Consequences

**Positive**

- Useful from opening day, without waiting for data
- Every campaign shows whether it brought visitors back, not only who responded
- Discounts stop where they add nothing

**Negative**

- The holdout group misses offers → **Mitigation:** Kept small (about 10%) and reshuffled each season
- Propensity scores reward people who would return anyway → **Mitigation:** Success judged on uplift against the holdout group, not on response rate

**Risks**

- Small segments make uplift hard to measure: minimum campaign sizes; segments merged when too small

## How we'll know it was right

Return-visit targets in [section 10.3](../docs/10-quality-requirements.md#103-ai-quality-targets) met against the holdout group; no campaign sent without marketing approval; no targeting on data the visitor didn't consent to.
