# 10. Quality Requirements

The three deciding architecture characteristics (works offline: availability; easy to change: modifiability; cheap to run: cost efficiency) come from [section 1.3](01-introduction-and-goals.md). Privacy is a constraint every design must meet. Trustworthy AI, capacity and availability are also required.

## 10.1 Quality tree

Each quality is refined into concrete concerns, each tested by one or more scenarios in section 10.2.

| Quality | Refined into | Scenarios |
|---|---|---|
| Works offline (availability) | Alarms without internet · entry without a connection · no sensor data lost · companion core offline | Q1–Q4 |
| Easy to change (modifiability) | Changing AI provider · changing prices · adopting a better model | Q5–Q7 |
| Cheap to run (cost efficiency) | AI cost per visitor · AI budgets on peak days | Q8–Q9 |
| Privacy (constraint) | No visitor images or exact locations leave devices · no personal data in AI logs | Q10 |
| Trustworthy AI | Misbehaviour detected quickly · safety answers never generated | Q11–Q12 |
| Capacity (scalability) | Busiest-day arrivals · holiday ticket on-sale | Q13, Q16 |
| Availability | Edge device failover · online services uptime and recovery | Q14–Q15 |
| Timeliness | Reports and dashboards up to date | Q17 |

## 10.2 Quality attribute scenarios

Each scenario states a situation and a measurable response, so it also works as a fitness function: a check that can be run to show the quality still holds. In operation, the targets become service level objectives (SLOs). Targets are rounded and reviewed after year 1.

| ID | Quality | Scenario | Target | Achieved by |
|---|---|---|---|---|
| Q1 | Works offline | A water reading goes out of range while the estate internet is down | Keeper's radio or pager alerted within **60 seconds** of the reading (urgent sensors report every minute, and immediately when a limit is crossed) | [ADR-003](../adrs/ADR-003-local-alarm-path.md) · [Diagram 6.3](06-runtime.md) |
| Q2 | Works offline | A visitor scans a ticket or annual pass with no connection | Accepted or refused within **1 second** | [ADR-004](../adrs/ADR-004-ticketing-build-payments-buy.md) · [Diagram 6.2](06-runtime.md) |
| Q3 | Works offline | The estate internet is down for a weekend | **No sensor readings lost** for outages up to 72 hours | [ADR-002](../adrs/ADR-002-estate-edge-and-connectivity.md) |
| Q4 | Works offline | A visitor with no signal opens the companion | Guide, map, times, day plan, **fixed safety answers and help points** available (**100%** after download) | [ADR-018](../adrs/ADR-018-visitor-channels-and-connectivity.md) |
| Q5 | Easy to change | The main AI provider shuts down, or doubles its price | Shutdown: companion on the backup within **seconds**, nightly features by their **next run**; price rise: moved to the backup within **1 day** after its test set passes; no app release either way | [ADR-006](../adrs/ADR-006-ai-gateway.md) · [Diagram 6.8](ai/ai-gateway-and-fallback.md) |
| Q6 | Easy to change | An admin changes a weekend price | Live within **minutes**, no code change | [ADR-019](../adrs/ADR-019-pricing-and-capacity.md) |
| Q7 | Easy to change | A better AI model becomes available | Released only after the feature's test set passes; rollback within **minutes** | [ADR-015](../adrs/ADR-015-validating-ai-results.md) |
| Q8 | Cheap to run | AI running cost at year 3 | About **€0.02 per visitor** or less on average (estimate €0.004–0.021) | [Section 11.3](11-risks-and-technical-debt.md) |
| Q9 | Cheap to run | A peak weekend drives heavy companion use | Every AI feature except animal care hard-stops at its monthly budget (the companion falls back to prepared answers); animal care alerts finance and the head keeper instead | [ADR-006](../adrs/ADR-006-ai-gateway.md) |
| Q10 | Privacy | A data-protection audit samples devices and AI logs | Monthly sampled audits find **no** visitor images or exact locations leaving devices and **no** personal data in AI logs; any finding fixed within **7 days** | [ADR-017](../adrs/ADR-017-privacy-by-design.md) |
| Q11 | Trustworthy AI | An AI feature starts misbehaving in production | **Hard failures** (errors, missing or malformed output, failed daily sample checks, any safety miss) detected within **1 day**; **quality drift** detected within each feature's monitoring window (1 week to 3 months, depending on the feature); either way the feature can fall back to its rules version | [ADR-015](../adrs/ADR-015-validating-ai-results.md) · [ADR-007](../adrs/ADR-007-graceful-degradation.md) |
| Q12 | Trustworthy AI | A visitor asks about a venomous animal, in any supported language | **100%** of the safety test set routed to fixed answers, never generated; production misses found by daily sampling and fixed within **1 day** | [ADR-012](../adrs/ADR-012-visitor-companion.md) |
| Q13 | Capacity (scalability) | A design day at year 3 (30,000 visitors, 2× average), or a busier day of up to 45,000 | Up to **6,000 arrivals an hour** in the morning peak with every ticket check under **1 second**; on busier days timed arrival slots keep arrivals at or below that rate; the guide downloads in under **1 minute** on entrance Wi-Fi; cloud services scale without manual action | [Section 7.4](07-deployment.md) · [ADR-019](../adrs/ADR-019-pricing-and-capacity.md) |
| Q14 | Availability | One edge device fails during opening hours | The other takes over with **no lost alarms** | [ADR-002](../adrs/ADR-002-estate-edge-and-connectivity.md) |
| Q15 | Availability | A cloud component fails during booking hours | Online ticket sales and visitor apps available **99.9%** of booking hours; cloud services restored within **1 hour** (recovery time objective, RTO); gates, alarms and the keeper app unaffected because they work offline | [Section 7.1](07-deployment.md) · [Section 8.7](08-crosscutting-concepts.md) |
| Q16 | Capacity (scalability) | Tickets for Christmas dates go on sale at year 3 (assumption: about 20,000 tickets on the first day, 2.5 people per order, a quarter of orders in the first 10 minutes) | **2,000 orders in 10 minutes** with checkout under **3 seconds** (p95) and no oversold time slots; load tested before every holiday on-sale | [ADR-004](../adrs/ADR-004-ticketing-build-payments-buy.md) |
| Q17 | Timeliness | Management opens reports before the gates open | Daily summary ready by **07:00**; live dashboards no more than **5 minutes** behind | [Section 5.7](05-building-blocks.md#57-estate-performance-reporting) |

## 10.3 AI quality targets

The **single source** for the headline targets of each AI use: the README, the ADRs and the AI use pages link here. Release pass marks and monitoring thresholds stay on each AI use page, and owners and fallbacks in the [AI register](ai/README.md#every-ai-use-at-a-glance). Targets are rounded and reviewed after year 1.

| AI use | Headline targets |
|---|---|
| [Animal health warnings](ai/animal-health-warnings.md) | **70%** of vet-confirmed illnesses flagged before a keeper recorded a concern (recall) · at least **1 in 3** flags confirmed as a real concern (precision) · about **6–10** flags a week across the estate, capped at **15** · flags reviewed within **4 hours** in the working day |
| Food tray photos and visible change check ([ADR-022](../adrs/ADR-022-phone-photos-in-animal-care.md)) | Tray estimates within **±10%** of weighed leftovers on at least **8 of 10** test trays · at least **1 in 3** "watch" flags confirmed before a stage expands |
| [Piranha population](ai/piranha-population.md) | Daily estimate within **±10%** of the latest accurate count · unexplained drop of **10% or more** flagged within **2 days** · accurate count every **1–3 months** and keeper clip counts monthly · losses of 1–3 fish caught through the event log and keeper checks, not the camera |
| [Demand forecast and staffing](ai/demand-forecast-staffing.md) | Day-ahead forecast within **15%** on average after 6 months (**25%** at 4 weeks ahead) · hours with queues above target down **30%** against year 1 · staff hours per 1,000 visitors down **10%** |
| [Visitor companion](ai/visitor-companion.md) | **40%** of app users use the companion · **80%** of rated answers helpful · **100%** of the safety test set routed to fixed answers, production misses fixed within **1 day** · **95%** of answers backed by approved content · queue time of visitors following a day plan **15%** lower |
| [Return-visit offers](05-building-blocks.md#55-growth-and-return-visits) ([ADR-023](../adrs/ADR-023-return-visit-offers.md)) | Return rate of offer recipients at least **10%** higher than the holdout group · annual pass holders **15%** of visits by year 3 |
| [Management insight assistant](05-building-blocks.md#56-management-insight-assistant) | At least **95%** of test figures match dashboards · **no** untraceable figure released · **100%** of out-of-scope and wrong-role questions refused · **70%** of pilot managers using it weekly after three months |
