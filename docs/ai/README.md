# How AI Is Used

AI is used where it clearly helps the estate or its visitors, and only where its results can be checked. Every AI feature starts as simple rules, improves as data builds up, and falls back to those rules if AI is unavailable, too costly or switched off. **The estate keeps running with no AI at all.**

## The four AI uses

| AI use | Brief need | AI methods | People stay in control by | Falls back to |
|---|---|---|---|---|
| [Animal health warnings](animal-health-warnings.md) | Animal health, how much and how well they eat | Time-series anomaly detection against each animal's or group's own normal; LLM summarisation for keeper briefings | Keepers confirm every flag; vet decides treatment; AI never raises "urgent" | Keeper-set rules |
| [Piranha population estimate](piranha-population.md) | Piranha population levels | Object detection (custom-trained vision service), multi-frame count aggregation with a range, reconciliation with an event log | Keepers check unexplained drops and do accurate counts | Keeper counts and event log |
| [Demand forecast and staff planning](demand-forecast-staffing.md) | Where to invest and deploy staff | Seasonal time-series forecasting with calendar, events, booking pace and weather | Managers assign staff and publish; admins approve price suggestions | Calendar and booking-pace rules |
| [Visitor companion](visitor-companion.md) | AI for customers; more visitors and return visits | Retrieval-augmented generation over approved content, safety topic matching inside our own service (no provider call), bounded tool use, guardrails with grounding checks | Visitors confirm actions; safety answers are fixed, never generated; content team approves all content | Offline guide, prepared answers, "ask a member of staff" |

All four sit behind the [AI gateway](ai-gateway-and-fallback.md), which handles provider switching, budgets, rate limits, personal-data removal and guardrails.

## Every AI use at a glance

This table is the estate's **AI register** (AI inventory): every AI use, its owner and how it is checked, reviewed at the monthly AI review ([section 8.5](../08-crosscutting-concepts.md#85-ai-governance)).

| AI use | Method type | Route | Fallback | Check | Owner |
|---|---|---|---|---|---|
| Animal health scoring | Anomaly detection | Batch job runner | Keeper rules | Keeper verdicts, vet diagnoses | Head keeper |
| Food tray estimate (pilot) | Image segmentation | Batch job runner | Keeper-entered leftovers | Weighed leftovers on test trays | Head keeper |
| Visible change check (year 2 pilot) | Image similarity and change detection | Batch job runner | Keeper observations | Keeper and vet verdicts | Head keeper |
| Keeper briefing | Generative AI summary | Request proxy | Plain flag list | Keepers flag mistakes | Head keeper |
| Piranha estimate | Object detection | Batch job runner | Keeper counts, event log | Marked clips, accurate counts | Head keeper |
| Demand forecast | Time-series forecasting | Batch job runner | Calendar and booking-pace rules | Actual visitors vs forecast and baseline | Operations manager |
| Forecast-driven price suggestions (rules on the AI forecast, not AI itself) | Rules on forecast busyness, within admin limits | Batch job runner | Time-based rules | Visits and revenue on similar dates; admin approval | Pricing admin |
| Companion answers | Retrieval-augmented generation | Request proxy | Prepared answers, offline guide | Test set, ratings, sampled reviews | Content team |
| Safety topic matching | Keyword lists and a small matching model | Inside the companion service (no provider) | Treat as safety | Safety test set, daily sample | Safety lead |
| Content translation | Machine translation | Request proxy | Existing approved translations | Human approval before publishing | Content team |
| Feedback insight | Text classification | Request proxy | Ratings only | Weekly staff sample | Marketing |
| Offer text drafts | Generative AI drafts | Request proxy | Standard seasonal messages | Marketing approves every campaign | Marketing |
| Return-visit targeting | Rules, later propensity modelling | Batch job runner | Rule-based segments | Holdout group | Marketing |
| Monthly insight report | Generative AI narrative over statistics | Request proxy | Dashboards only | Every figure traced to data; management review | Management |
| Management insight assistant | Generative AI with fixed read-only tools | Request proxy | Dashboards and monthly report | Figures match dashboards on test questions; untraceable figures blocked | Data owner |

## AI methods catalogue

Which AI method each part uses, in standard terms. Example techniques are illustrative: the managed service or technology provider selects the exact algorithm, validated against the metrics shown.

| AI use | Task type | Method family | Learning type | Example techniques | Evaluation metrics |
|---|---|---|---|---|---|
| Animal health scoring | Anomaly detection | Time-series anomaly detection per animal or group; later multivariate | Unsupervised | Seasonal decomposition with robust z-scores; isolation forest; feature attribution for main signals | Recall on vet-confirmed illnesses; precision of flags; recall on synthetic anomaly injection |
| Illness risk (later) | Risk prediction | Classification | Supervised | Gradient-boosted trees | Precision, recall |
| Food tray estimate (pilot) | Estimating food left over from photos | Image segmentation | Supervised (managed vision service) | Segmentation model fine-tuned on labelled tray photos | Absolute percentage error against weighed leftovers |
| Visible change check (year 2 pilot) | Spotting visible change in an animal | Image similarity and change detection | Pre-trained image embeddings, no generation | Embedding distance between photos of the same animal over time | Precision of "watch" flags against keeper and vet verdicts |
| Keeper briefing | Summarisation | LLM summarisation, grounded in flag data | Pre-trained generative model | Prompted LLM with structured input | Keeper-flagged errors; groundedness |
| Piranha estimate | Counting from images | Object detection, then statistical aggregation and calibration | Supervised (transfer learning on labelled frames) | YOLO-family or managed AutoML detector; robust median with confidence interval | Detection precision and recall; counting error (absolute percentage error); low-confidence rate |
| Demand forecast | Forecasting | Probabilistic time-series forecasting with exogenous variables; pickup model; hierarchical reconciliation | Supervised (learned from history) | Managed AutoML ensembles of exponential smoothing, ARIMA, gradient-boosted trees | MAPE vs rules baseline (rolling-origin back-testing); prediction interval coverage |
| Staff assignment (later) | Scheduling | Constraint programming (operations research) | Not machine learning | Open-source constraint solver | Coverage targets met; manager changes to plans |
| Forecast-driven price suggestions | Pricing | Rules on forecast busyness | Not AI | Price bands within admin limits | Visits and revenue on similar dates |
| Companion answers | Question answering | Retrieval-augmented generation with hybrid retrieval | Pre-trained generative and embedding models | Keyword plus semantic search over embeddings; source citations; LLM routing | Correctness on test set; groundedness (LLM-as-judge checked against people); helpful ratings |
| Safety topic routing | Intent classification | Keyword rules plus embedding similarity, fail-safe threshold | Pre-trained embedding model (no generation) | Multilingual sentence embeddings | Recall on safety test set (target 100%) |
| Companion tools | Tool use | LLM function calling with read-only tools | Pre-trained generative model | Structured tool schemas | Tool-call correctness; red-team results |
| Day plan | Planning | LLM structured output, then constraint-based route scheduling | Pre-trained generative model; not machine learning for the route | JSON schema output; constraint solver | Plan validity; queue time of plan followers |
| Content translation | Translation | Neural machine translation | Pre-trained | Managed translation service | Human approval rate |
| Feedback insight | Text classification | Zero-shot classification and sentiment analysis with an LLM | Pre-trained generative model | Prompted classification into fixed themes | Agreement with weekly staff sample |
| Return-visit targeting | Propensity | Rules first, then propensity modelling | Supervised (later) | Gradient-boosted classifier | Uplift vs control group (holdout) |
| Monthly insight report | Report writing | Data-to-text generation over statistics | Pre-trained generative model | Prompted LLM with citation check | Untraceable figures (target zero); management review |
| Management insight assistant | Question answering over figures | LLM tool calling over a semantic layer (restricted agent) | Pre-trained generative model | Fixed read-only tools, e.g. via the Model Context Protocol | Figures matching dashboards on test questions; blocked answers |
| People counting | Counting people | On-device computer vision in bought counters | Pre-trained (vendor) | Built into devices; numbers only leave the device | Count accuracy against manual counts at installation |
| Faulty sensor detection | Outlier detection | Rules plus statistical outlier detection | Not machine learning | Stuck, silent or impossible-value rules; z-scores | Faulty sensors found before keepers notice |

## Where AI is deliberately not used

See also [ADR-016](../../adrs/ADR-016-where-generative-ai-is-not-used.md).

| Area | Why not |
|---|---|
| Urgent alarms (water quality, temperature, alarm buttons) | Must be instant, predictable and work offline: simple rules on site |
| Live crowd levels and waits | Counting and arithmetic are accurate enough |
| Safety answers (venomous animals, first aid, lost children, emergencies) | One wrong generated answer is unacceptable |
| Setting prices, completing payments or refunds | Money: rules on the AI forecast may only suggest; a person approves |
| Diagnosis, feeding or medication changes | Animal care decisions stay with keepers and the vet |
| Assigning named staff | Labour rules and fairness: managers decide |
| Operating rides or silencing alarms | Safety systems stay independent of AI |

## Also covered, with lighter treatment

| Feature | Where |
|---|---|
| Keeper briefing, faulty sensor detection | [Animal health warnings](animal-health-warnings.md) |
| Forecast-driven price suggestions (rules on the AI forecast) | [Demand forecast and staff planning](demand-forecast-staffing.md) |
| Return-visit offers | [Growth and return visits](../05-building-blocks.md#55-growth-and-return-visits) · [ADR-023](../../adrs/ADR-023-return-visit-offers.md) |
| Feedback insight, content translation | [Visitor companion](visitor-companion.md) |
| Crowd patterns and investment insight | [Building blocks](../05-building-blocks.md) |
| Food tray photos and visible change check (staged) | [Animal health warnings](animal-health-warnings.md) · [ADR-022](../../adrs/ADR-022-phone-photos-in-animal-care.md) |
| Management insight assistant: questions and quick reports across visitors, sales, forecasts, offers, animal care and AI cost | [Building blocks 5.6](../05-building-blocks.md#56-management-insight-assistant) · [ADR-021](../../adrs/ADR-021-management-insight-assistant.md) |

| Deferred | Why |
|---|---|
| Behaviour analysis from fixed camera clips | High cost: cameras in many enclosures, storage, and labelled clips for every species ([ADR-014](../../adrs/ADR-014-camera-behaviour-analysis-deferred.md)); keepers' phone photos are staged instead ([ADR-022](../../adrs/ADR-022-phone-photos-in-animal-care.md)) |
| Voice notes to records | A tap-based keeper form captures the same observations more reliably |
| Predictive maintenance for rides | Needs extra sensors on heritage rides; planned for years 2–3 |

## How the AI additions fit the baseline, including tensions

The new non-AI platform is the baseline ([ADR-020](../../adrs/ADR-020-new-platform-as-baseline.md)). Most AI additions follow its traits; where they don't, the tension is stated with its mitigation.

| Baseline trait | Health warnings | Piranha estimate | Forecast and staffing | Companion | Tension and mitigation |
|---|---|---|---|---|---|
| Works offline on site | Readings buffered; scoring waits | Clips buffered | Planning days ahead | Offline core only | **Companion AI answers need signal** → offline guide, entrance Wi-Fi |
| Coarse services, events | Uses reading and verdict events | Uses clip and count events | Uses sale and entry events | Calls the gateway synchronously | **The AI gateway is a synchronous dependency and one more thing to run** → two instances; fallback to prepared answers |
| Managed cloud services | Managed ML | Custom vision service | Managed ML | Generative AI provider | **Each adds an external provider dependency** → owned data, backups, rules fallback |
| Cost caps | Alert, no stop | Hard stop | Hard stop | Daily and monthly caps | **Animal care deliberately breaks the hard-stop rule** → finance and head keeper alerted instead |
| Privacy | No visitor data | Camera away from visitors | Aggregates only | Visitor questions leave the phone | **Companion sends questions to a provider** → personal data removed, contracts, logs kept 30 days |
| Small team can run it | Nightly batch | Nightly batch | Nightly batch | Content team must maintain approved content | **Content upkeep is new work** → unanswered questions become the content team's to-do list |

## How the judging criteria on AI are answered

| Criterion | Where to look |
|---|---|
| Innovative use of AI | The four AI uses above, each with a targeted diagram (Diagrams 5.4–5.7) and runtime scenario (Diagrams 6.4–6.7) |
| Dealing with uncertainty in AI | [AI gateway and fallback](ai-gateway-and-fallback.md): the brief's three questions answered directly; each AI use's "Provider changes and price rises" section |
| Validation and verification of AI results | [ADR-015](../../adrs/ADR-015-validating-ai-results.md) · each AI use's "Checks before release" and "Monitoring in production" sections |
| AI additions match the architecture | [ADR-020](../../adrs/ADR-020-new-platform-as-baseline.md) · the tensions table above · each AI use's "Fit with the platform" section |
