# 12. Glossary

| Term | Meaning |
|---|---|
| Accurate count | A piranha count taken with a counting app during planned tank maintenance; used to check the nightly estimate |
| Active-passive failover | One copy runs while an identical standby takes over automatically if it fails |
| ADR (architecture decision record) | A short document recording one significant decision, its options and consequences |
| AI gateway | The single layer every AI call passes through: provider routing and failover, budgets, rate limits, personal-data removal, guardrails, versioning |
| AI register | The list of every AI use with its method, fallback, checks and owner |
| Annual pass | A reusable pass for an individual or a family, valid for a year |
| Anomaly detection | Finding readings that differ from what is normal for that animal, group or sensor |
| API gateway | The single entry point for app requests: routing, authentication and rate limits |
| arc42 | A standard template for architecture documentation, used for this document |
| Architecture characteristics | The qualities an architecture must deliver, such as availability or modifiability; the kata's judging criteria use this term. Also called quality attributes |
| Audit trail | Write-once records of who did what, when and why, kept for regulators, finance and AI accountability |
| AutoML | A managed service that selects, trains and tunes models automatically on our data |
| Availability | The share of time a service works when needed; here it includes working offline during outages |
| Bounded context | A business area with its own model, language and data, such as Ticketing or Animal care; each of our seven domain services covers one |
| C4 model | A way of drawing software architecture at four levels of detail: system context, container (deployable parts), component (parts inside one container) and code |
| Calibration | Adjusting a model's output so it matches real outcomes, e.g. correcting fish counts for hidden fish |
| Canary release | Sending a small share of traffic (e.g. 5%) to a new version first, and watching it before switching fully |
| Change detection | Comparing images of the same subject over time to spot what has changed |
| Check today | The highest urgency an AI feature can raise: a person should look at it today |
| Checked figure | A figure with one agreed definition, calculated in the data hub and shown on dashboards; the only kind of figure the management insight assistant may use |
| Circuit breaker | Stops calls to a failing service for a while so it can recover; our AI gateway then uses the backup provider |
| Confidence interval / prediction interval | A range that, at a stated confidence level (e.g. 80%), is expected to contain the true value (confidence) or a future observation (prediction) |
| Constraint programming | Finding a plan that satisfies fixed rules (opening times, crew minimums); an operations research method, not machine learning |
| Control group | A group that gets no change, so the effect of the change can be measured; our holdout group is one |
| Cost efficiency | Delivering the required qualities at the lowest running cost |
| Data drift | Input data changing over time (e.g. with the seasons), which can make a model less accurate |
| Data hub | Our store of event history and checked figures, in open formats on object storage, used for reporting, forecasting and AI |
| Data minimisation | The GDPR principle of collecting and keeping only the personal data a purpose needs |
| Data protection impact assessment (DPIA) | A GDPR assessment of privacy risks, required before processing such as cameras in public areas or location data |
| Data protection officer (DPO) | The person responsible for data protection compliance, internal or contracted |
| Data-to-text generation | Generative AI writing narrative from structured figures, with each statement tied to its figure |
| Database per service | Each service keeps its own database; other services get its data only through events or its API |
| Day-one version | The simple rules-based version of an AI feature, used at launch and as its fallback |
| Design day | The busy day that on-site capacity is sized for, rather than the single busiest day; here the lower end of the 2–3× average range, with capacity added in steps |
| Distributed tracing | Following one request across several services using a shared trace ID |
| Domain event | A record that something happened in the business, such as "ticket sold", published for other services to use |
| Dwell time | How long visitors stay in a zone |
| Edge device | A small industrial computer on site that runs the message broker, alarm rules and data buffer |
| Embedding | A list of numbers representing the meaning of a text, so similar meanings can be found by similarity |
| Estate performance reporting | Dashboards, daily summary, monthly insight report and quarterly and yearly business review showing whether the estate and the platform are on track |
| EU AI Act | The EU law regulating AI systems by risk level, including transparency duties for chatbots from 2 August 2026 |
| Evaluation harness | The automated setup that runs a feature's test set against a model, prompt or provider and compares the results with pass marks |
| Event log (population) | Keeper records of piranha births, deaths, removals and fish found outside the tank |
| Eventual consistency | Copies of data in different places may differ briefly, then agree once updates arrive, e.g. gate entries after an outage |
| Exact-match cache | Reuses a stored answer only when a question is identical; never for answers with live facts or personal data |
| Exogenous variable (covariate) | An outside input to a forecast, such as weather or holidays |
| Fallback | What a feature does when AI is unavailable, too costly, failing its checks or switched off |
| Family ticket | A one-day ticket at a bundle price for 2 adults and 1 child, or 2 adults and 2 children |
| Feature attribution | Showing which inputs contributed most to a model's result |
| FinOps | Managing cloud and AI running costs actively, with budgets, alerts and cost per feature |
| Fitness function | A measurable check that an architecture decision or quality still holds; each ADR ends with one |
| Flag manager | The part of Animal care that applies thresholds, known causes and urgency to scores before a flag reaches keepers |
| Footfall | The number of visitors passing through or into a zone |
| GDPR | EU data protection law; the design meets its standard, which also meets UK GDPR |
| Graceful degradation | Keeping core functions working with reduced features when a part fails, e.g. rules when AI is unavailable |
| Ground truth | Real outcomes recorded by people, used to measure whether AI is right |
| Grounding check (groundedness) | A check that a generated answer is supported by the approved content it was given |
| Guardrails | Checks on what goes into and comes out of an AI model |
| Hallucination | A generated answer that sounds right but isn't supported by facts or sources |
| Hierarchical forecasting | Forecasting a total (visitors a day) and splitting it consistently into parts (hours and zones) |
| Holdout group | A random share of consenting visitors who get no offer, so the effect of offers can be measured; a control group |
| Horizontal autoscaling | Adding or removing copies of a service automatically as load changes |
| Hot standby / warm standby | A backup that is running and taking a share of traffic (hot), or connected and tested regularly but used only on failure (warm) |
| Human-in-the-loop | A person reviews or confirms an AI output before it takes effect |
| Hybrid retrieval | Finding relevant content by combining keyword search and semantic search |
| Image segmentation | Computer vision that labels which parts of an image belong to what, e.g. food versus tray |
| Integration event | A domain event published for other services to use, in a stable, versioned format |
| Intelligence pipeline | The path from sensing to people deciding: sense, capture, understand, predict, explain, check, decide, learn; urgent alarms skip it |
| Intent classification | Deciding what a question is about, e.g. whether it is a safety topic |
| ISO/IEC 25010 | The international standard list of software quality characteristics |
| ISO/IEC 42001 | The international standard for managing AI responsibly within an organisation |
| Keeper verdict | A keeper's decision on whether an AI health flag was a real concern |
| LLM (large language model) | A generative AI model that reads and writes text |
| LLM function calling (tool use) | A language model choosing a defined tool and its inputs, which our code then runs |
| LLM routing | Sending each request to a cheaper or stronger model depending on how hard it looks; a model cascade instead tries the cheaper model first and escalates |
| LLM-as-judge | Using a language model to grade answers against a test set; checked regularly against human reviewers |
| LoRaWAN | A long-range, low-power radio network for battery sensors; covers kilometres; low data rates |
| Managed AI service | A cloud service that provides forecasting, anomaly detection or vision without us building models |
| Management insight assistant | A plain-language question and quick-report tool for managers that answers only from checked figures returned by fixed read-only tools |
| MAPE (mean absolute percentage error) | The average forecast error as a percentage of actual values |
| Message bus | The managed cloud service that carries integration events between services (publish-subscribe) |
| MLOps / LLMOps | Practices for releasing, monitoring and updating machine learning and language model features safely |
| Model drift | A model's accuracy falling over time as the world changes |
| Modifiability | How easily the architecture can be changed, e.g. switching an AI provider |
| MQTT | A lightweight publish-subscribe messaging protocol for devices; the brief budgets for MQTT-capable devices |
| MQTT ingestion | How device messages enter the platform: MQTT broker on the edge devices → encrypted MQTT → cloud MQTT bridge → message bus |
| Mutual TLS | Encrypted connections where both sides prove their identity with certificates |
| Network segmentation | Splitting a network into separate parts with rules on what may pass between them |
| Neural machine translation | Translation by a neural network model |
| NIST AI Risk Management Framework | A US framework for identifying and managing AI risks |
| Object detection | Computer vision that finds and outlines objects, such as fish, in an image |
| Offline-first | Designed to work without a connection, syncing when one is available |
| p95 latency | The response time that 95% of requests are faster than |
| PCI DSS | The security standard for handling card data; kept with the payment provider |
| Pickup model (booking pace) | A revenue-management method that forecasts demand from how fast bookings for a date build up compared with the usual pattern |
| PII redaction | Removing personally identifiable information (names, emails, phone numbers) before data is sent on |
| Precision | The share of flags or detections that are correct |
| Prediction interval coverage | The share of actual values that fall inside the forecast range |
| Prompt injection | An attempt to trick an AI model with instructions hidden in a question or content |
| Propensity modelling | Predicting how likely someone is to do something, such as return or upgrade to an annual pass |
| Pseudonymisation | Replacing identity with an ID that can't be linked back without separate data; still personal data under GDPR |
| Publish-subscribe | Services publish events to a message bus and any interested service subscribes, without knowing each other |
| Quality attribute scenario | A situation and a measurable response that shows a quality is achieved |
| Quality tree | A breakdown of the architecture's qualities into concrete concerns, each tested by quality attribute scenarios |
| Rate limit | A cap on how many AI requests a feature or visitor can make per minute |
| Recall | The share of real cases that were flagged or detected |
| Recovery point objective (RPO) | The most data that may be lost in a failure; zero for our sensor readings in outages up to 72 hours |
| Recovery time objective (RTO) | The longest a service may be down before it is restored |
| Red-team testing | Deliberately trying to make an AI feature misbehave, e.g. with prompt injection or harmful requests |
| Release gate (evaluation gate) | The rule that any AI change must pass its feature's test set before going live |
| Retrieval-augmented generation (RAG) | Generative AI that first looks up approved content, then answers only from it |
| Role-based access control (RBAC) | Access granted by job role, with only the rights each role needs (least privilege) |
| Rolling-origin back-testing | Testing a forecast by repeatedly predicting past periods as if they were the future |
| Scalability | Handling growth, such as 15,000 visitors a day, without redesign |
| Semantic layer | One agreed definition of each business figure, used by dashboards, reports and the insight assistant alike |
| Semantic search | Finding content by meaning using embeddings, not only matching words |
| Sentiment analysis | Classifying text as positive, neutral or negative |
| Service level objective (SLO) | A measurable target for a service in operation, e.g. alarms within 60 seconds |
| Service-based architecture | An architecture style (Richards and Ford) with a small number of coarse, independently deployable domain services; our variant gives each service its own database |
| Shadow period (shadow deployment) | A trial where a new AI feature runs and is measured without affecting people |
| Staffing ratio (coverage target) | Operations' rule for how many staff a zone or ride needs, e.g. staff per 500 visitors; a setting, not learned by AI |
| Store-and-forward | Keeping data locally while the connection is down and sending it when it returns |
| Structured output | A language model answering in a fixed format, such as JSON, that code can check |
| Supervised / unsupervised learning | Learning from labelled examples (supervised) or finding patterns without labels (unsupervised) |
| Synthetic anomaly injection | Adding realistic fake problems to copies of real data to test whether they are detected |
| Technology provider | The company contracted to build, operate and hand over the platform |
| Test set | A fixed set of cases with known correct results, used to check an AI feature |
| Time-series forecasting | Predicting future values, such as visitor numbers, from past values and inputs like weather |
| Timed arrival slot | An arrival time window printed on a ticket, with a limit per slot, used only on forecast busiest days |
| Token budget | A spending limit for AI model calls, measured in tokens or money, per feature and period |
| Transfer learning | Adapting a model pre-trained on general data to a specific task with a small labelled set |
| Uplift | The extra effect of an action, measured against a control group |
| Uplift modelling | Predicting how much an offer changes a person's behaviour, rather than how likely they are to act anyway |
| Vendor lock-in | Being unable to change supplier without high cost or effort |
| Visitor companion | The AI assistant in the visitor app that plans the day and answers questions from approved content |
| Visitor mix | The share of visitor types on a day, e.g. families, adults, concessions |
| Watch | The lowest flag level: noted in the keeper's list for the next round; below "check today" |
| Zero-shot classification | Sorting text into categories with a pre-trained model and no task-specific training |
| Zone | An area of the estate used for anonymous crowd counts and location sharing |
