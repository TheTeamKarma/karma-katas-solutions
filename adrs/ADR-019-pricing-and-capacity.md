# ADR-019: Pricing as data, forecast-driven price suggestions approved by admins, timed arrival slots only on the busiest days

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-004](ADR-004-ticketing-build-payments-buy.md), [ADR-008](ADR-008-ai-autonomy-levels.md), [ADR-011](ADR-011-demand-forecast-staff-planning.md)

## Context

The estate must become more profitable and spread visits away from peak days. The brief gives no visitor capacity limit, but gates and entrance Wi-Fi are sized for a design day, and the busiest days bring up to 3× average visitors. Price differences by sex are generally unlawful in the EU and UK, and dynamic pricing can feel unfair to visitors.

## Options considered

*Chosen options in bold: one for pricing, one for capacity.*

| Option | Pros | Cons |
|---|---|---|
| Fixed prices | Simple, predictable | No way to spread demand or capture peak value |
| Fully automatic dynamic pricing | Maximum revenue | Fairness and trust risk; hard to explain |
| **Time-based pricing rules as data, plus forecast-driven price suggestions (rules on the AI forecast) an admin approves** | Predictable for visitors; flexible for the estate; explainable | Admin effort to review suggestions |
| Hard daily visitor caps | Crowd control | Lost sales; brief gives no limit |
| **Timed arrival slots with per-slot limits, switched on only for forecast busiest days** | Keeps morning arrivals within what the gates can handle; no lost sales, demand spread through the day | Visitors pick an arrival slot on those days |
| No limits at all | Simplest | Gates and entrance Wi-Fi overwhelmed above the design day |

## Decision

Prices follow **time-based rules held as data** (weekends, peak hours, school holidays), changed by admins without code. **Forecast-driven price suggestions** (rules on the AI forecast, not AI themselves) are produced for dates weeks ahead, within admin-set minimum and maximum prices; an **admin approves** before any change. Prices are shown before purchase, **locked on each order**, audited when changed, and **never based on personal characteristics**. There is **no daily visitor cap**. On days forecast to exceed the on-site design day, **timed arrival slots** switch on: each ticket carries an arrival slot, and each slot is limited so arrivals stay within what the gates and entrance Wi-Fi can handle (about 6,000 an hour). Demand is spread through the day rather than turned away. Other days have no slot limits, only fair-use limits (e.g. maximum tickets per order). A daily capacity setting also exists, switched off by default, in case safety licensing or animal welfare ever requires one.

## Consequences

**Positive**

- Spreads visits to quieter days; supports profitability
- Fair and explainable to visitors
- Lawful in any likely region

**Negative**

- Suggestions need admin time → **Mitigation:** Weekly review; suggestions only for dates ahead
- Timed slots add a booking step on the busiest days → **Mitigation:** Quieter slots highlighted at purchase; pass holders pick a slot in the app; staff planning and on-call lists cover the rest

**Risks**

- Price rises on busy days feel unfair: limits on price range; prices published in advance

## How we'll know it was right

Visits on quieter days rise and peak-day queues fall; morning arrivals stay within slot limits on the busiest days (Q13); admin approves or rejects every suggestion; no pricing complaints upheld.
