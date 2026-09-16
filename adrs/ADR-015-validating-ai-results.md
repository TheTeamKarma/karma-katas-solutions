# ADR-015: Validating AI results before release and in production

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-006](ADR-006-ai-gateway.md), [ADR-007](ADR-007-graceful-degradation.md), [ADR-008](ADR-008-ai-autonomy-levels.md)

## Context

The brief asks how we confirm AI is working, noting that generative AI is non-deterministic, and how we'll know if AI starts misbehaving in production. Our AI features make predictions about animal health, fish numbers, visitor demand and visitor questions, where errors cost animal welfare, money or visitor safety.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Test once before launch | Simple | Misses drift, provider changes and seasonal shifts |
| Rely on provider quality claims | No effort | Benchmarks don't reflect our data or tasks |
| **Ground truth per feature + release gate + shadow period + production signals + rollback** | Evidence-based; catches drift; every change checked | Needs keepers, managers and the content team to record verdicts |

## Decision

Every AI feature is validated the same way, following common MLOps and LLMOps practice:

| Step | What happens |
|---|---|
| **Ground truth** | Real outcomes recorded by people: keeper verdicts and vet diagnoses (health), accurate fish counts and population events (piranhas), actual visitors (forecast), ratings and content-team reviews (companion) |
| **Test set before release** | Each feature has a fixed test set with pass marks, run automatically by an **evaluation harness**; pass marks are on each AI use page, headline targets in [section 10.3](../docs/10-quality-requirements.md#103-ai-quality-targets) |
| **Shadow period** (shadow deployment) | New features run without affecting people (2–12 weeks, set per feature), compared with what people did |
| **Release gate** (evaluation gate) | Any change of model, provider, prompt, threshold or content index re-runs the test set; it must be no worse than the current version |
| **Non-determinism** | Generative AI test questions are each run 5 times and pass marks apply across all runs; model versions are pinned; any provider model update goes through the release gate |
| **Gradual rollout** (canary release) | Changes take a small share of traffic first where possible (companion about 5%) |
| **Production signals** | Each feature has signals, thresholds and an owner, including data drift and model drift, e.g. share of flags keepers dismiss, forecast error, estimate error against counts, unhelpful ratings, unsupported answers (hallucinations), missed safety topics |
| **Detection speed** | Hard failures (errors, malformed output, failed daily sample checks, any safety miss) within 1 day; quality drift within each feature's monitoring window, from 1 week (companion) to 3 months (piranha accurate counts, with monthly keeper clip counts as the earlier signal) |
| **Response** | Signal crosses its threshold → owner reviews; serious cases (any missed safety topic, unsupported answers above 5%) → immediate rollback or fallback to rules |
| **Checking the checkers** | Automated judging of companion answers must agree with the content team on at least **90%** of a monthly sample, otherwise the judge is fixed and the release gate re-run; keepers see the value of their verdicts |

## Consequences

**Positive**

- A concrete answer to the validation criterion, measurable per feature
- Drift and silent provider changes are caught
- Rules baselines show whether AI actually adds value

**Negative**

- Staff effort to record verdicts and counts → **Mitigation:** Built into existing routines: two taps per flag, counts at tank cleaning
- Rare events (illnesses) make accuracy slow to measure → **Mitigation:** Injected test cases plus monthly review of every missed case

**Risks**

- Verdicts recorded carelessly make the metrics meaningless: head keeper and content team review samples

## How we'll know it was right

Every AI feature has a test set, a passed release gate, production signals with owners, and a tested rollback; hard failures detected within 1 day and quality drift within each feature's monitoring window.
