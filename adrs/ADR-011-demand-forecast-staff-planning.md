# ADR-011: Demand forecast driving staff planning

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-007](ADR-007-graceful-degradation.md), [ADR-008](ADR-008-ai-autonomy-levels.md), [ADR-013](ADR-013-managed-ai-services.md)

## Context

The client doesn't know where to invest or deploy staff. Visitors are expected to grow from 5,000 to 15,000 a day, with seasonal peaks such as Christmas and school holidays, and there is little history at first.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Calendar averages only | Simple | Ignores bookings and weather; weak for peaks |
| **Industry-standard staged forecasting: seasonal and events → booking pace → weather** | Used by hotels and attractions; explainable; improves with data | Management enters event uplifts in year 1 |
| Custom deep learning | Potentially precise | We don't build models; too little data |
| Buy staff scheduling software | Rosters and labour rules built in | Another vendor and licence; staff data outside our platform |

## Decision

Forecast **nightly for the next 4 weeks** (daily visitors and visitor mix; hourly by zone for the first 7 days) and **on demand up to 3 months ahead**. Method: a **seasonal forecast** with holidays and estate events (management enters expected uplifts such as Christmas in year 1), adjusted by **booking pace** for the coming weeks and by **weather** for the coming days; rules on day one, managed time-series forecasting after about 3 months. Our own **staff planning module** turns forecasts and operations' coverage targets into **required staff per zone and hour**; **managers assign named staff and publish**. Assignment suggestions using an open-source constraint solver may follow once managers trust the numbers. The rules baseline runs alongside the AI every night.

## Consequences

**Positive**

- Staff placed where visitors will be; on-call lists cover uncertain days
- No scheduling licence; staff data stays in our platform
- The forecast also feeds price suggestions and the companion's quieter-time tips

**Negative**

- Managers still assign staff manually at first → **Mitigation:** Required numbers remove most of the planning effort
- Year 1 events rely on manual uplifts → **Mitigation:** Replaced by real history from year 2

**Risks**

- Managers don't trust the plan: shadow period with forecasts shown beside their own plans

## How we'll know it was right

Forecast error, queue and staff-hour targets in [section 10.3](../docs/10-quality-requirements.md#103-ai-quality-targets) met, with the AI forecast beating the rules baseline.
