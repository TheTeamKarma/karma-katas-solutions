# ADR-004: Build the ticketing core, buy payments

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-001](ADR-001-service-based-architecture.md)

## Context

The brief requires ticket sales "including family passes". The estate needs one-day tickets, family tickets (2+1, 2+2), annual passes, time-based pricing set by admins, forecast-driven price suggestions, entry that works offline, and sales data for forecasting. Every €0.10 per-order fee from a bought ticketing platform costs about €60K a year today and €180K at year 3 (assuming about 300 open days and 2.5 people per order).

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Buy a complete ticketing platform | Fastest to launch; mature | Fees per order; limits on pricing rules, data access and offline gate checks |
| Buy an API-first platform, build around it | Some flexibility | Two systems to keep consistent; still fees and offline limits |
| **Build the ticketing core; buy payments** | Owns products, prices, sales data and offline gate checks | Build and run effort |
| Build payments too | Full control | Card-data security burden; no benefit |

## Decision

Build the **ticketing core**: product catalogue (one-day tickets, family tickets 2+1 and 2+2, annual passes for individuals and families), **pricing rules as data** (weekends, peak hours, holidays), orders and refunds, **signed tickets** that gates can check offline, and a cancelled-ticket list. Prices are locked per order and never depend on personal characteristics. **Card payments are bought** from a payment provider; card details never pass through the platform. Payment terminals use the estate network with their own mobile connection as backup.

**Example technologies** (illustrative, see [section 7.5](../docs/07-deployment.md#75-example-technologies)): Stripe or Adyen for online and terminal payments; ticketing as our own service on managed PostgreSQL.

## Consequences

**Positive**

- Sales and pass data feed forecasting, staff planning and return-visit offers
- Gates work offline; admins change prices without code
- No per-order platform fees as visitors triple

**Negative**

- Build and run effort, which may approach the saved fees → **Mitigation:** Accepted for data ownership and offline control; the core is kept small
- Peak on-sale load → **Mitigation:** Cloud scaling and load testing before holiday on-sales

**Risks**

- Screenshot reuse during outages: entries shared between lanes; duplicates flagged afterwards

## How we'll know it was right

Ticket check under 1 second offline; price changes live within minutes without code; no lost or duplicated sales after outages.
