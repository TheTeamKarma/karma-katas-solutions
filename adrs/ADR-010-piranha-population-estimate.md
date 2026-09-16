# ADR-010: Piranha population: estimate with a range, reconciled with counts and events

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-007](ADR-007-graceful-degradation.md), [ADR-013](ADR-013-managed-ai-services.md)

## Context

The brief asks to "check population levels" of the jumping piranhas. Fish move fast, overlap, hide and live in water that can be murky, so exact counting from video is unrealistic. Losses can come from deaths, fish eating each other, or fish jumping out.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Keeper counts only | Accurate at the time | Monthly at best; too slow to catch losses |
| **Nightly estimate from sampled frames, reconciled with accurate counts and an event log** | Daily trend with a range; checkable | A few hundred marked frames needed to set up |
| Tracking each fish across video | Potentially more accurate | Fragile in murky water; costlier |
| Counting passage between tank sections | Accurate | Needs a tank rebuild; resident fish swim back and forth |

## Decision

A camera facing into the display tank, away from the viewing glass, records **three short clips a day**. Each night, poor frames are dropped and a **custom-trained object detection service** (configured with a few hundred marked frames from our own tank) counts fish in sampled frames, combined into an **estimate with a range**. The estimate is **reconciled** with the expected count (last accurate count plus births, minus deaths and removals). **Accurate counts** are taken with a counting app during planned tank maintenance (every 1–3 months), plus monthly keeper counts from marked clips. A drop below the range on **two nights in a row** with no logged cause raises "check today". No estimate is published on low-confidence nights.

## Consequences

**Positive**

- Daily visibility of population levels, as the brief asks
- Honest about uncertainty through ranges and confidence
- Training data and events are ours; the service can be replaced

**Negative**

- Fish hiding behind decor cause undercounting → **Mitigation:** Correction from past accurate counts
- Very small young fish not detected → **Mitigation:** Tracked through the event log

**Risks**

- Accuracy drops after tank changes: re-mark and re-test before flags resume

## How we'll know it was right

Estimate error and drop-detection targets in [section 10.3](../docs/10-quality-requirements.md#103-ai-quality-targets) met, checked against every accurate count and the monthly keeper clip counts.
