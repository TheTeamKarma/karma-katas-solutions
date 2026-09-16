# ADR-014: Defer behaviour analysis from camera clips

**Status:** Accepted (decision to defer) · **Date:** 2026-09-14 · **Related:** [ADR-009](ADR-009-animal-health-warnings.md), [ADR-022](ADR-022-phone-photos-in-animal-care.md)

## Context

Animal behaviour from video (pacing, lethargy) could improve health warnings. It needs cameras in many enclosures, clip storage, and people labelling hundreds of clips for every species so a model learns what abnormal looks like. Intake, weight, activity and keeper observations already catch most health changes.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Build it now | Richer health signals | High cost and labelling effort; little data; hard to validate |
| **Defer; use activity data built into sensors and cameras, and keeper observations** | Low cost; health warnings still work | Misses some behavioural signs |
| Generative AI describing clips | No labelling | Unreliable for welfare decisions |

## Decision

**Defer** behaviour analysis from camera clips. Health warnings use intake, weight, **activity levels already provided by motion sensors or cameras**, and keeper observations. Photos taken by keepers on their phones are a separate, staged decision ([ADR-022](ADR-022-phone-photos-in-animal-care.md)); only fixed-camera behaviour analysis is deferred here. Revisit after year 1, when keeper verdicts and vet diagnoses show which illnesses were missed and whether behaviour would have caught them.

## Consequences

**Positive**

- Avoids a large cost with uncertain benefit
- Keeps effort on features that serve the brief directly

**Negative**

- Some behaviour-only signs of illness may be missed → **Mitigation:** Keepers observe behaviour on every round; missed cases reviewed monthly

**Risks**

- Adding cameras later costs more: mounting points and network capacity planned in enclosures now

## How we'll know it was right

The year 1 review of missed illnesses shows whether behaviour analysis would have flagged them; the decision is revisited with that evidence.
