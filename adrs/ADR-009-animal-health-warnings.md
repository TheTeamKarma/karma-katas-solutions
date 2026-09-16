# ADR-009: Animal health warnings: staged anomaly detection with keeper confirmation

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-007](ADR-007-graceful-degradation.md), [ADR-008](ADR-008-ai-autonomy-levels.md), [ADR-013](ADR-013-managed-ai-services.md)

## Context

The brief asks to track animal health and how much and how well animals eat; sick animals are costly. There is almost no digital history, many animals live in groups, and both a missed illness and a flood of false alerts cause harm.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Keeper rules only | Simple, explainable | Misses gradual changes |
| **Staged: keeper rules → each animal's own normal → combined-signal model** | Works from day one; improves with data | Needs good keeper records and a shadow period |
| Illness prediction trained on past cases from day one | Potentially precise | No labelled cases yet |
| Generative AI judging health | Flexible | Can be confidently wrong; hard to validate; unsafe |

## Decision

Detect changes in **nightly batches** in three stages: keeper-set rules (day one) → **time-series anomaly detection** against each animal's or group's own normal (after 4–8 weeks) → **multivariate anomaly detection** combining intake, weight, activity and environment (after about a season), using a managed machine learning service. Signals are monitored per animal where identifiable, otherwise per group or enclosure. Readings are collected for **4–8 weeks before opening**. AI raises at most **"check today"**; **keepers confirm** every flag (human-in-the-loop) and the vet decides treatment. Known causes (breeding, moulting, treatment, weather) downgrade flags. Generative AI only writes the keeper briefing. Flag noise is limited by a flag precision target and a weekly cap across the estate ([section 10.3](../docs/10-quality-requirements.md#103-ai-quality-targets)).

## Consequences

**Positive**

- Earlier intervention reduces illness cost
- Keeper verdicts create the ground truth to validate the AI
- Safe: AI never diagnoses or changes care

**Negative**

- Group-housed animals hide individual illness → **Mitigation:** Keeper observation stays primary for groups
- Depends on reliable keeper records → **Mitigation:** Two-tap form built into keeper rounds

**Risks**

- Alert fatigue: flag quality target, workload cap and noise monitoring; thresholds tuned in an 8–12 week shadow period

## How we'll know it was right

Recall on vet-confirmed illnesses, flag precision and the weekly cap meet their targets in [section 10.3](../docs/10-quality-requirements.md#103-ai-quality-targets); every missed case reviewed monthly.
