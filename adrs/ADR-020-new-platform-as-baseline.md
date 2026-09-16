# ADR-020: The new non-AI platform is the baseline the AI additions must fit

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-001](ADR-001-service-based-architecture.md), [ADR-007](ADR-007-graceful-degradation.md)

## Context

The brief asks whether the architectural characteristics of the additions match the existing architecture, but also asks for a new comprehensive architecture. Apart from patchy Wi-Fi, the brief describes no existing systems. We assume the estate has almost no digital systems today: a basic box office, the rides' own safety systems and paper keeper routines ([section 2.5](../docs/02-constraints.md)).

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Preserve and extend current systems | Less change | Almost nothing digital to build on |
| Treat all AI as part of one design with no baseline | Simpler to describe | No way to show the AI additions fit anything |
| **Keep existing systems to a minimum; make the new non-AI platform the baseline** | Honest about what exists; gives the "additions match the existing architecture" criterion a clear answer | Assumes little existing technology |

## Decision

Only what we must connect to or can't replace is kept: the **rides' safety systems** (read-only status), **existing Wi-Fi** (reused where it reaches) and **keeper routines** (supported by new tools). The current box office is replaced by the new ticketing. The **new non-AI platform** (tickets, entry, records, devices, data hub) is the **baseline**: it works on its own, and each AI feature is added on top of it. Every AI use document states how it matches the baseline's qualities (works offline, easy to change, cheap to run), and ADR-007 ensures the baseline keeps running when AI is off.

## Consequences

**Positive**

- A clear, checkable answer to "do the additions match the existing architecture?"
- AI can be added, changed or removed without redesign

**Negative**

- Little reuse of current systems → **Mitigation:** Little exists; replacing the box office is part of the ticketing decision (ADR-004)

**Risks**

- More existing systems found than assumed: integration points added through events and APIs

## How we'll know it was right

Switching off any AI feature keeps core operations (tickets, entry, alarms, animal records) meeting their quality attribute scenarios. The feature's own measure falls back to its rules baseline, and that difference is how we show what the AI adds. Each AI use document shows its fit with the three architecture characteristics.
