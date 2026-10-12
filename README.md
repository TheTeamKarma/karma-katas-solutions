# Von Digitalis Estates · AI-Assisted Architecture

**O'Reilly Architectural Katas 2026: AI-Assisted Software Architecture**
Team: **Team Karma** · [Team](#team) · 🎥 [Semifinal video (4:49)](https://drive.google.com/file/d/1aBddhjvx3vmDCiz5m6D2zsWcLSxVRT7x/view?usp=sharing)

> **In one sentence:** a new estate platform that runs without AI, with AI uses that must each beat a simple rules version, keep people in charge, and keep working when the Wi-Fi, the cloud or an AI provider fails.

## The problem

The newly appointed 72nd Countess Von Digitalis has inherited a sprawling, unprofitable estate: **40 historic 18th-century rides** and **200+ exotic and poisonous animals** in 55 enclosures, now opening to the public. The estate gets **5,000 visitors a day** and needs **15,000 within three years**. It needs ticketing with family passes, insight into what's popular, careful animal monitoring (including the jumping piranhas' population), and growth in visitors and profit. Wi-Fi across the park is patchy, and there is a budget for MQTT-capable devices.

## A day at the estate

Before the gates open, the head keeper's briefing flags a monitor lizard that has eaten less for three days, and last night's piranha estimate agrees with the log of births and deaths. Operations already knows today's expected visitors and how many staff each zone needs. A family downloads the guide on the entrance Wi-Fi; deep in the grounds with no signal, the app still shows their plan and map. Where signal returns, the companion suggests the piranha feeding before the carousel queue builds, and answers "is the scorpion dangerous?" with a fixed, approved answer in Spanish. At midday the main AI provider fails: the companion switches to its backup within seconds, and if that failed too it would give prepared answers. When water oxygen drops in the aquarium, the alarm reaches the keeper's pager in under a minute, without internet or AI. In the afternoon the operations manager asks the insight assistant which zones got busier after the new feeding show; every figure in the answer links to its dashboard. As the family leaves, they're offered an annual pass with today's ticket credited.

## How AI solves the estate's problems

We built a **new estate platform that works without AI**, then added **four main AI uses**, each answering a need in the brief, plus lighter uses listed in the [AI register](docs/ai/README.md#every-ai-use-at-a-glance). Every one has a rules-based fallback and is validated against real outcomes recorded by people.

| The estate's problem | What AI does | How we keep it trustworthy |
|---|---|---|
| "Looking after the animals is costly, even more so if they get sick" | **[Animal health warnings](docs/ai/animal-health-warnings.md)** compare each animal's or group's intake, weight and activity with its own normal, and ask keepers to check changes early | Keepers confirm every flag (human-in-the-loop); AI never diagnoses or raises urgent alarms; thresholds are raised if too few flags turn out to be real concerns |
| "Check population levels" of the jumping piranhas | **[Piranha population estimate](docs/ai/piranha-population.md)** counts fish in three short daily clips, analysed nightly, and gives an estimate with a range | Reconciled with accurate counts during tank maintenance, monthly keeper counts from marked clips, and a log of births, deaths and removals |
| "Difficult to know where to invest & deploy staff" | **[Demand forecast and staff planning](docs/ai/demand-forecast-staffing.md)** forecasts visitors 4 weeks ahead (3 months on demand) from seasons, events, bookings and weather, and turns it into required staff per zone and hour | Managers assign and publish; the rules forecast runs alongside every night and AI must beat it |
| "We want to get more returning visitors too, but aren't sure how" | **[Growth and return visits](docs/05-building-blocks.md#55-growth-and-return-visits)**: ticket, pass and feedback data drive annual pass upgrades, what's-new messages and quieter-day offers; rules first, prediction of who will return later | Consenting visitors only; marketing approves every campaign; success measured against a holdout group that gets no offer ([ADR-023](adrs/ADR-023-return-visit-offers.md)) |
| "We have no real idea of what parts of the estates are most popular" | **[Popularity insight](docs/05-building-blocks.md#54-popularity-insight)** combines anonymous zone counts, waits, feedback and repeat visits into dashboards and a monthly report | Statistics, not AI, for the numbers; every figure in the report traced to its data; management reviews it |
| Managers need answers across areas, fast | **[Management insight assistant](docs/05-building-blocks.md#56-management-insight-assistant)** answers plain-language questions and builds quick reports across visitors, sales, forecasts, offers and animal care | Figures come only from fixed read-only tools that return the dashboards' own figures; untraceable figures blocked; read-only ([ADR-021](adrs/ADR-021-management-insight-assistant.md)) |
| Grow visitor numbers | **[Visitor companion](docs/ai/visitor-companion.md)** suggests what to do next and answers questions in their language from approved estate content, even with patchy Wi-Fi | Safety answers are fixed, never generated; answers show sources; core works offline |

Underneath, **one [AI gateway](docs/ai/ai-gateway-and-fallback.md)** handles provider switching, budgets and privacy, so a better model, a price rise or a provider shutting down never stops the estate. Urgent alarms, prices, safety answers and live crowd levels deliberately **don't use generative AI** ([ADR-016](adrs/ADR-016-where-generative-ai-is-not-used.md)).

**How management sees the estate is on track:** live dashboards, a daily summary, a monthly insight report, and a quarterly and yearly business review of visitors, revenue and costs against budget and the path to 15,000 a day, each with a panel showing whether the platform and its AI are working ([estate performance reporting](docs/05-building-blocks.md#57-estate-performance-reporting)).

**Architecture style and scale:** a **service-based architecture** (seven domain services, each a bounded context, plus three platform services) with **event-driven integration** (integration events, publish-subscribe) through a message bus, and an **edge tier on site** for alarms and buffering ([ADR-001](adrs/ADR-001-service-based-architecture.md), [ADR-002](adrs/ADR-002-estate-edge-and-connectivity.md)). 15,000 visitors a day (30,000–45,000 on the busiest days) is a light load for cloud services, which scale automatically; the real limits are physical, gates and entrance Wi-Fi at about 6,000 arrivals an hour (timed arrival slots on the busiest days), plus companion AI cost, and each is sized or capped ([sizing](docs/07-deployment.md#74-sizing-and-growth), [Q13](docs/10-quality-requirements.md), [AI cost](docs/11-risks-and-technical-debt.md#113-running-cost-of-ai-year-3-rough)).

**What sets this design apart**

- **AI has to earn its place.** Every AI feature runs next to a simple rules version; the AI must beat it to stay switched on, and the rules take over if it fails (graceful degradation) ([ADR-007](adrs/ADR-007-graceful-degradation.md)).
- **The piranha numbers must add up.** The nightly estimate is reconciled with a log of births, deaths and removals ([ADR-010](adrs/ADR-010-piranha-population-estimate.md)).
- **Safety before any model.** Safety questions are recognised inside our own service and answered with fixed approved text; they never reach an AI provider ([ADR-012](adrs/ADR-012-visitor-companion.md)).
- **AI never raises the alarm.** AI can flag "check today" at most; urgent alarms come from rules on site that work without internet ([ADR-003](adrs/ADR-003-local-alarm-path.md), [ADR-008](adrs/ADR-008-ai-autonomy-levels.md)).
- **Offers prove themselves.** Return-visit offers are measured against a holdout group that gets none.
- **Phones before new cameras.** Keepers' own photos first build a record, then estimate food left over where scales don't fit, then spot visible changes in pilot species, each stage proven before the next ([ADR-022](adrs/ADR-022-phone-photos-in-animal-care.md)).
- **AI explains figures, never makes them up.** The management insight assistant can only use figures returned by fixed tools; each answer shows where every figure came from and the tool inputs used (week, zone), so a manager can see whether the right question was answered ([ADR-021](adrs/ADR-021-management-insight-assistant.md)).

## Start here

| Time | Read |
|---|---|
| **5 minutes** | This page · [How AI is used](docs/ai/README.md) · [AI quality targets at a glance](docs/10-quality-requirements.md#103-ai-quality-targets) |
| **20 minutes** | [Introduction and goals](docs/01-introduction-and-goals.md) · [Context](docs/03-context-and-scope.md) · [Solution strategy](docs/04-solution-strategy.md) · one AI use of your choice |
| **Full** | The arc42 sections below, the AI uses and the [23 ADRs](adrs/README.md) |

## Architecture documentation

The AI uses have their own pages. The rest follows **arc42**, a standard template for architecture documents.

| Section | Contents |
|---|---|
| [How AI is used](docs/ai/README.md) | The four AI uses, the AI register, the AI methods catalogue and the AI gateway |
| [1. Introduction and goals](docs/01-introduction-and-goals.md) | Needs, quality goals, stakeholders |
| [2. Constraints](docs/02-constraints.md) | Technical, organisational, legal; assumptions |
| [3. Context and scope](docs/03-context-and-scope.md) | Business and technical context; external providers |
| [4. Solution strategy](docs/04-solution-strategy.md) | Fundamental decisions; where AI is and isn't used; the intelligence pipeline |
| [5. Building blocks](docs/05-building-blocks.md) | Services and their parts; popularity insight; growth; management insight assistant; estate performance reporting |
| [6. Runtime](docs/06-runtime.md) | Key scenarios step by step |
| [7. Deployment](docs/07-deployment.md) | Cloud, estate site, networks, sizing, example technologies |
| [8. Cross-cutting concepts](docs/08-crosscutting-concepts.md) | Offline, alerting, privacy, security, AI governance, failure modes |
| [9. Architecture decisions](docs/09-architecture-decisions.md) | Index of ADRs |
| [10. Quality requirements](docs/10-quality-requirements.md) | Quality tree, scenarios, AI quality targets |
| [11. Risks and technical debt](docs/11-risks-and-technical-debt.md) | Risks, deferred work, AI running cost |
| [12. Glossary](docs/12-glossary.md) | Terms |

## Diagrams

| Diagram | View | Where |
|---|---|---|
| 1.1 | Capability map | [Section 1](docs/01-introduction-and-goals.md) |
| 3.1 · 3.2 | Business context · technical context | [Section 3](docs/03-context-and-scope.md) |
| 4.1 | Intelligence pipeline | [Section 4](docs/04-solution-strategy.md#45-the-intelligence-pipeline) |
| 5.1 | Building blocks, level 1 | [Section 5](docs/05-building-blocks.md) |
| 5.2 | AI gateway, level 2 | [AI gateway](docs/ai/ai-gateway-and-fallback.md) |
| 5.3 | Popularity insight | [Section 5](docs/05-building-blocks.md#54-popularity-insight) |
| 5.8 | Growth and return visits | [Section 5](docs/05-building-blocks.md#55-growth-and-return-visits) |
| 5.9 | Management insight assistant | [Section 5](docs/05-building-blocks.md#56-management-insight-assistant) |
| 5.4 · 5.5 · 5.6 · 5.7 | Targeted AI views: health warnings · piranha · forecast and staffing · companion | [AI uses](docs/ai/README.md) |
| 6.1 · 6.2 · 6.3 | Runtime: family ticket purchase · offline gate entry · alarm with internet down | [Section 6](docs/06-runtime.md) |
| 6.4 · 6.5 · 6.6 · 6.7 · 6.8 | Runtime: each AI use · AI provider failure | [AI uses](docs/ai/README.md) |
| 7.1 · 7.2 · 7.3 | Deployment: overview · estate site · network separation | [Section 7](docs/07-deployment.md) |

All diagrams are written in Mermaid and render directly on GitHub.

## Where to find evidence for each judging criterion

For judges scoring one criterion at a time: where this entry answers it.

| Criterion | Where it's answered |
|---|---|
| Innovative use of AI | [Four AI uses](docs/ai/README.md), each with a targeted diagram and runtime scenario · [where generative AI is deliberately not used](adrs/ADR-016-where-generative-ai-is-not-used.md) |
| Suitability given the constraints | [Constraints](docs/02-constraints.md) · [estate edge and connectivity](adrs/ADR-002-estate-edge-and-connectivity.md) · [alarms without internet](adrs/ADR-003-local-alarm-path.md) · [failure modes](docs/08-crosscutting-concepts.md) |
| Appropriate levels of detail | arc42 structure · C4-style diagrams at context, container and component levels · [23 ADRs](adrs/README.md) · [example technologies for every part](docs/07-deployment.md#75-example-technologies) |
| Dealing with uncertainty in AI | [AI gateway and fallback](docs/ai/ai-gateway-and-fallback.md) · [ADR-006](adrs/ADR-006-ai-gateway.md) · [ADR-007](adrs/ADR-007-graceful-degradation.md) · [ADR-013](adrs/ADR-013-managed-ai-services.md) |
| Additions match the existing architecture | [ADR-020](adrs/ADR-020-new-platform-as-baseline.md) · "Fit with the platform" in each AI use |
| Validation and verification of AI results | [ADR-015](adrs/ADR-015-validating-ai-results.md) · "Checks before release" and "Monitoring in production" in each AI use · [AI quality targets at a glance](docs/10-quality-requirements.md#103-ai-quality-targets) · [quality attribute scenarios](docs/10-quality-requirements.md) |

## Team

| Member | Role in this entry |
|---|---|
| **Neeraj** | Lead Architect |
| **Raveendra Chakrakodi** | AI Assurance Lead |
| **Bhanupriya Purohit** | Platform & Data Architect |
| **Aniket Purohit** | Visitor Experience Engineer |

## About this entry

[How we used AI to produce this entry](how-we-used-ai.md)
