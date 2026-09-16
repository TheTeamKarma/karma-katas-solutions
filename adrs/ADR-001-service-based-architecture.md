# ADR-001: Service-based architecture with event-driven integration

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-002](ADR-002-estate-edge-and-connectivity.md), [ADR-006](ADR-006-ai-gateway.md)

## Context

The platform covers ticketing, entry, visitor apps, park operations, animal care, data, AI and on-site devices. Areas change at different speeds (pricing and AI change often; gate checks rarely), the estate has a small IT team, running cost must stay low, and load is modest: 15,000 visitors a day at year 3.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Modular monolith | Cheapest and simplest to run | One deployable; on-site and AI parts don't fit; areas can't change independently |
| **Service-based: about ten coarse services, events between them** | Areas deploy independently; few things to run; fits a small IT team | Some services are larger; a busy part scales with its whole service |
| Microservices: 30+ small services, each with its own database | Maximum independence and fine-grained scaling | Heavy to run and monitor; costly; the load doesn't need it |

## Decision

Use a **service-based architecture**: about ten coarse services: **seven domain services**, each a bounded context in domain-driven design terms (ticketing, entry, visitor experience, loyalty, park operations, animal care, data and insight), and **three platform services** (AI gateway, alerting, identity). Services publish **integration events** (domain events shared with other services, in stable, versioned formats) through a managed message bus (publish-subscribe) and call each other through APIs. This is the **service-based architecture** style described by Mark Richards and Neal Ford (*Fundamentals of Software Architecture*). In their description services often share one database; **our variant gives each service its own database** (database per service), and services share data only through domain events and APIs. The data hub keeps a history of all events for reporting and AI. Each service is designed so it can be split into smaller services later if load or change proves it necessary (the companion and ticket on-sales are the likely first candidates).

## Consequences

**Positive**

- Each business area can change and deploy on its own, supporting "easy to change"
- About ten deployables suit a small IT team and "cheap to run"
- Events let data flow to forecasting and insight without tight coupling

**Negative**

- No queries across services' databases → **Mitigation:** Reports, forecasts and the insight assistant read from the data hub, which receives every domain event
- Services share some data through events, which can arrive late (eventual consistency) → **Mitigation:** Services work from their own copies; consumers handle late and duplicate events safely
- A busy part (e.g. the companion on a peak day) scales with its whole service → **Mitigation:** Split that service when measurements show it's needed

**Risks**

- Boundaries blur and services grow into a monolith: service ownership and event contracts reviewed each quarter

## How we'll know it was right

Changes to one area (e.g. a pricing rule or AI prompt) release without redeploying others; fewer than about 15 deployables at year 3; cloud running cost within budget.
