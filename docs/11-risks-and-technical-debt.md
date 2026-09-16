# 11. Risks and Technical Debt

## 11.1 Risks

**Scale.** Likelihood: Low = unlikely within three years · Medium = possible · High = expected. Impact: Low = minor inconvenience · Medium = a feature degraded or a target missed · High = harm to animal welfare, safety, legal standing or major revenue.

| ID | Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|---|
| R1 | Mobile signal at estate buildings is weak, so the 4G/5G backup link is unreliable | Medium | Medium | Site survey; low-orbit satellite backup instead ([ADR-002](../adrs/ADR-002-estate-edge-and-connectivity.md)) | IT team |
| R2 | LoRaWAN radio gaps in walled or dense areas | Medium | Medium | Radio survey before installation; overlapping gateways; mobile-network IoT for spots out of reach | Technology provider |
| R3 | Keepers don't record observations reliably, so animal AI can't be validated | Medium | High | Two-tap form in rounds; show keepers the value of their verdicts; head keeper samples records | Head keeper |
| R4 | Alert fatigue from animal health warnings | Medium | High | Flag precision target and weekly cap ([section 10.3](10-quality-requirements.md#103-ai-quality-targets)); shadow period; noise monitoring ([ADR-009](../adrs/ADR-009-animal-health-warnings.md)) | Head keeper |
| R5 | A safety question reaches generative AI | Low | High | Fail-safe intent routing; sampled audits; any miss fixed immediately ([ADR-012](../adrs/ADR-012-visitor-companion.md)) | Safety lead |
| R6 | Companion answer not supported by approved content | Medium | Medium | Grounding check; sources shown; rollback above 5% unsupported answers ([ADR-015](../adrs/ADR-015-validating-ai-results.md)) | Content lead |
| R7 | AI provider shuts down, degrades or raises prices (vendor lock-in) | Medium | Medium | AI gateway with backups, budgets and fallbacks ([ADR-006](../adrs/ADR-006-ai-gateway.md), [ADR-007](../adrs/ADR-007-graceful-degradation.md)) | Technology provider |
| R8 | Managers or keepers don't trust AI outputs | Medium | Medium | Shadow periods; every output shows its reasons, data and range | Feature owners |
| R9 | Visitor numbers grow faster than planned | Low | Medium | On-site hardware expanded early on measured triggers; cloud scales automatically | Operations manager |
| R10 | Ticketing build effort larger than expected | Medium | Medium | Small core; payments bought; phased rollout ([ADR-004](../adrs/ADR-004-ticketing-build-payments-buy.md)) | Technology provider |
| R11 | A manager acts on an assistant answer that misreads a correct figure | Low | Medium | Sources shown next to every figure; untraceable figures blocked; weekly sampled review ([ADR-021](../adrs/ADR-021-management-insight-assistant.md)) | Data owner |
| R12 | Personal data slips into AI calls through free text | Low | Medium | Removal by the AI gateway; provider contracts; sampled log checks ([ADR-017](../adrs/ADR-017-privacy-by-design.md)) | Data protection officer |
| R13 | Dependence on a single technology provider to build and run the platform | Medium | High | Architecture documented in arc42 and ADRs; open standards (MQTT, LoRaWAN); containers; data owned by the estate; handover terms in the contract | Management |
| R14 | Heritage restrictions delay device installation | Medium | Medium | Battery sensors needing no cabling; early engagement with heritage advisers | Management |
| R15 | Cyberattack on estate devices, edge devices or networks | Medium | High | Network segmentation; per-device certificates (mutual TLS) for IP devices and per-device keys for LoRaWAN sensors; no inbound access from the internet; remote patching; security testing before opening | IT team with technology provider |
| R16 | Drifting, stuck or faulty sensors feed wrong readings | Medium | Medium | Faulty-sensor rules and statistical outlier checks; calibration schedule; "not enough data" shown instead of silent scoring | IT team, head keeper |
| R17 | Content team can't keep approved content and translations up to date | Medium | Medium | Unanswered questions as the to-do list; an owner per content area; the companion says "don't know" rather than guess | Content lead |
| R18 | The data protection impact assessment (DPIA) restricts counters, cameras or location sharing | Low | Medium | Counting inside devices; zone-only, consent-based location; popularity insight still works from counts, waits and feedback | Data protection officer |
| R19 | Regulation changes, e.g. an AI use classified as high-risk under the EU AI Act | Low | Medium | Yearly assessment; human-in-the-loop, audit trail and off switches already in place | Data protection officer |
| R20 | Forecasts are weak in year 1 with little history | High | Medium | Rules baseline runs alongside; AI used only when it beats the baseline; manual event uplifts; on-call lists for wide ranges | Operations manager |
| R21 | Photo-based estimates are unreliable for some species | Medium | Low | Pilot species only; shadow period; feeds existing signals or raises "watch" only; off per species ([ADR-022](../adrs/ADR-022-phone-photos-in-animal-care.md)) | Head keeper |

## 11.2 Technical debt and deferred work

Accepted shortcuts and deliberate deferrals, each with a trigger to revisit.

| Item | Why accepted now | Revisit when |
|---|---|---|
| Managers assign named staff manually | Numbers first; managers must trust them | Managers ask for assignment suggestions |
| Behaviour analysis from fixed camera clips deferred | High cost, uncertain benefit ([ADR-014](../adrs/ADR-014-camera-behaviour-analysis-deferred.md)); phone photo features staged instead ([ADR-022](../adrs/ADR-022-phone-photos-in-animal-care.md)) | Year 1 review of missed illnesses |
| Predictive maintenance for rides | Needs extra sensors on heritage rides | Years 2–3, with ride maintenance records |
| Year 1 forecasts rely on manual event uplifts (e.g. Christmas) | No history yet | Replaced by real history from year 2 |
| "Happy animals": comparing visitor crowding at an enclosure with the animals' activity | Needs a year of crowd and activity data | Year 2, reviewed with keepers |
| Voice notes for keepers | Tap form is more reliable | Keepers request hands-free recording |
| Tracking individual fish across video | Fragile in murky water | Estimate error stays above 15% |
| Services may grow large | Coarse services are simpler to run | A service's load or change rate demands splitting |
| Public review sites not integrated | In-app feedback first | Feedback insight is established |

## 11.3 Running cost of AI (year 3, rough)

| Feature | Assumption | Yearly range |
|---|---|---|
| Visitor companion | 30% of 15,000 daily visitors ask about 5 questions; €0.002–0.01 per answer; about 300 open days | €15K–70K |
| Forecasting, anomaly detection, keeper briefings, insight reports | Managed services, nightly batch jobs | €5K–20K |
| Piranha estimate | About 150 frames a night; occasional retraining | Under €2K |
| Management insight assistant | About 30 users, 1,000 questions a month | Under €1K |
| Other AI uses | Content translation, feedback insight, return-visit propensity, food tray photo estimates | €1K–3K |
| **Total** | About 4.5 million visitors a year (15,000 × 300 days) | **€20K–95K a year, about €0.004–0.021 per visitor** |

**If a provider doubles its price:** the companion reaches its budget sooner and serves prepared answers for the rest of the period, simple questions move to cheaper models, and traffic can switch to the backup provider after its test set passes. Operations are unaffected.

Hardware (sensors, cameras, edge devices, gateways), platform build and hosting are not estimated here: they depend on procurement and the technology provider's rates. [Section 7.4](07-deployment.md) gives the volumes they depend on. The build is kept small: coarse services, bought payments, managed cloud services, no scheduling licence.

## 11.4 Open questions for the estate

| Question | Affects |
|---|---|
| Is fibre available at the estate buildings, and how strong is mobile signal? | Internet link and backup choice |
| Are there heritage restrictions on mounting devices and gateways? | Device placement, installation time |
| How many piranhas, in how many tanks? | Camera count, estimate accuracy |
| Does the estate already have a staff radio system? | Alarm path integration |
| Is there any safety licence limit on visitor numbers? | Capacity setting (off by default) |
| Which budget and reporting cycle does finance use (monthly, quarterly, financial year)? | Budget import and business review timing ([section 5.7](05-building-blocks.md#57-estate-performance-reporting)) |
