# ADR-022: Phone photos in animal care: staged vision features

**Status:** Accepted · **Date:** 2026-09-15 · **Related:** [ADR-009](ADR-009-animal-health-warnings.md), [ADR-013](ADR-013-managed-ai-services.md), [ADR-014](ADR-014-camera-behaviour-analysis-deferred.md), [ADR-015](ADR-015-validating-ai-results.md)

## Context

The brief asks how much and how well animals eat. Feed scales don't fit every enclosure, so some leftovers are estimated by eye. Keepers also notice visible changes (skin, wounds, body condition) that numbers miss. Fixed cameras for behaviour analysis are deferred because of cost and labelling effort ([ADR-014](ADR-014-camera-behaviour-analysis-deferred.md)). Keepers already carry phones with the keeper app, so photos cost almost nothing to collect.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| No photos; keeper estimates and observations only | Simplest | Leftovers guessed where scales don't fit; visible changes depend on memory |
| Photos as records only | Cheap; builds history | No help estimating or spotting change |
| **Staged: photo records first, then food tray estimates, then a visible change check for pilot species** | Uses phones already carried; each stage proven before the next; builds its own labelled data | Keeper effort to take photos; accuracy varies by species |
| Generative AI describing animal photos | No labelling | Can be confidently wrong; unsafe for welfare decisions |
| Fixed cameras with behaviour analysis now | Continuous signals | High cost and labelling effort (ADR-014) |

## Decision

Introduce photo features **in stages**, each switched on only after its checks pass, and each switchable off per species or enclosure:

| Stage | When | What | AI method (learning type) | Human role and limit | Pass mark before release |
|---|---|---|---|---|---|
| 0 · Photo records | Day one | Keepers attach photos to feeding records and observations | None | Photos become labelled history for later stages | — |
| 1 · Food tray estimate | Year 1, pilot enclosures where feed scales don't fit | Before and after tray photos give the share of food left over, feeding the existing intake signal | **Image segmentation** (supervised computer vision, managed vision service) | Keeper can correct the estimate; **no new flag type**, only an input to health scoring; shadow period first | Within **±10%** of weighed leftovers on at least **8 of 10** test trays |
| 2 · Visible change check | Year 2, a few pilot species that are easy to photograph (e.g. large reptiles, primates) | A new photo compared with the same animal's earlier photos | **Image similarity and change detection** (pre-trained image embeddings, compared over time) | Raises **"watch"** at most; keeper confirms; vet decides | At least **1 in 3** "watch" flags confirmed on shadow data (precision); expanded to more species only if met |

Fixed-camera behaviour analysis stays deferred ([ADR-014](ADR-014-camera-behaviour-analysis-deferred.md)). Generative AI never judges health from images. Photos are framed on animals and trays, not visitors or staff, kept for a set period, and processed through the AI gateway with its budgets.

## Consequences

**Positive**

- Answers "how much they eat" where scales can't be installed
- An innovative use of AI with almost no new hardware
- Every stage can be switched off without losing anything keepers do today

**Negative**

- Keepers take extra photos → **Mitigation:** Two taps in the feeding form; only in pilot enclosures
- Accuracy varies by species, lighting and food type → **Mitigation:** Pilot species first; shadow period; re-test after changes in food or trays

**Risks**

- Estimates trusted over keeper judgement: estimates shown as editable, and stage 2 raises "watch" only

## How we'll know it was right

Stage 1 and stage 2 targets in [section 10.3](../docs/10-quality-requirements.md#103-ai-quality-targets) met before each expands; cost under **€2K a year**; switching any stage off leaves health warnings working on their other signals.
