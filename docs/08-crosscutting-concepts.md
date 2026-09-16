# 8. Cross-cutting Concepts

Concepts that apply across all building blocks.

## 8.1 Working offline

| Principle | How |
|---|---|
| Offline-first only where it matters | Alarms, gates, box office, keeper app and the companion's core work without a connection. Dashboards, forecasts and AI answers may pause |
| Decide urgent things on site | Simple alarm rules on the edge devices, never AI |
| Keep everything | All sensor readings buffered on site (at least 72 hours) and sent to the cloud when the link returns: alarms first, then latest readings, then backlog. Recovery point objective (RPO) for sensor readings: zero data lost in outages up to 72 hours |
| Signed, self-contained tickets | Gates check signatures and validity dates locally; cancelled tickets and entries shared between lanes by the edge devices |
| Sync, then reconcile (eventual consistency) | Offline sales, entries and keeper records upload later; duplicates flagged for staff review |
| Honest about staleness | Apps show when data was last updated (e.g. "waits updated 20 min ago") |

Runtime examples: [6.2 offline gate entry](06-runtime.md), [6.3 alarm with internet down](06-runtime.md).

## 8.2 Alerting and escalation

One shared alerting service handles every alert ([ADR-003](../adrs/ADR-003-local-alarm-path.md)): animal alarms, health warnings, crowding, faulty sensors, AI provider failures.

| Urgency | Examples | Delivery | Escalation |
|---|---|---|---|
| **Urgent** | Water quality, temperature, alarm buttons, unusual crowding | Staff radio and pagers from the edge devices (no internet needed); SMS from the edge devices' own mobile modems | Next person after 5 minutes without acknowledgement |
| **Check today** | Animal health warning, unexplained piranha drop | Keeper app and morning briefing | Head keeper if unreviewed after 24 hours |
| **Routine** | Faulty non-animal sensor, drifting forecast | Daily summary or ticket to IT or the technology provider | Weekly review |

AI can raise "check today" at most. Only rules and people raise "urgent".

## 8.3 Personal data and privacy

Designed to GDPR standard ([section 2.3](02-constraints.md), [ADR-017](../adrs/ADR-017-privacy-by-design.md)).

| Principle | How |
|---|---|
| Collect only what's needed (data minimisation) | Zone instead of exact location; ticket type instead of age; gender optional and used only in anonymous reports |
| Anonymous by design | People counters and crowd cameras send numbers only; images of visitors never leave the device |
| Consent | Location sharing, offers and marketing are opt-in |
| Separate identity from behaviour | Visit data uses pseudonymous IDs (pseudonymisation), kept apart from accounts; still treated as personal data under GDPR. Crowd counts carry no IDs at all |
| Protect data sent to AI | Personal details removed by the AI gateway (PII redaction); providers contracted not to train on our data; hosting region set by us |
| Keep conversations on the phone | Companion conversations stored on the device; server logs with personal data removed, kept 30 days |
| Children | No child accounts, no profiling; family tickets hold only ticket types |
| Card data | Handled only by the payment provider |
| Impact assessment | A data protection impact assessment (DPIA) completed before launch, covering people counters and crowd cameras, zone location sharing, the companion and the family audience; updated when a feature adds new data |
| Accountability | A data protection officer (DPO) role, internal or contracted, owns DPIAs, audits and data-protection requests |

## 8.4 Security

| Area | Approach |
|---|---|
| Staff identity | Estate's single sign-on; role-based access control (RBAC) per job (keeper, vet, operations, admin) with least privilege; keeper app stays signed in offline for a shift |
| Visitor identity | Guest checkout for one-day tickets; accounts for annual passes and offers |
| Device identity | IP devices and the edge-to-cloud link use per-device certificates (mutual TLS); LoRaWAN sensors use per-device keys (LoRaWAN security); edge devices only accept known devices |
| Network segmentation | Separate device, staff, payment and visitor networks ([Diagram 7.3](07-deployment.md)) |
| Encryption | All traffic encrypted, including MQTT from site to cloud |
| Secrets | Provider keys held only by the AI gateway and services, in a secrets vault, rotated regularly |
| AI-specific threats | Prompt-injection filtering; companion and management assistant tools read-only; assistant tools check the manager's role; safety topics never reach AI providers |
| Audit | Every consequential action recorded in a write-once audit trail ([section 8.9](#89-audit-trail)) |

## 8.5 AI governance

| Concern | Approach | Details |
|---|---|---|
| What AI may do alone | Automatic only when safe and reversible · a person confirms money, prices, animal care and publishing (human-in-the-loop) · never AI for safety systems, alarms, medical decisions or excluding visitors | [ADR-008](../adrs/ADR-008-ai-autonomy-levels.md) |
| Fallback | Every AI feature has a non-AI version and an off switch | [ADR-007](../adrs/ADR-007-graceful-degradation.md) |
| Release gate (evaluation gate) | Any model, prompt, provider or threshold change passes the feature's test set, run automatically by the evaluation harness, before going live | [ADR-015](../adrs/ADR-015-validating-ai-results.md) |
| Right technique per task | Generative AI only for language tasks | [ADR-016](../adrs/ADR-016-where-generative-ai-is-not-used.md) |
| Monitoring | Each AI use defines production signals, thresholds and an owner, including data drift and model drift | Each [AI use document](ai/README.md) |
| Ground truth | Keeper verdicts, vet diagnoses, accurate fish counts, actual visitor numbers, visitor ratings | Each AI use document |
| Versioning | Prompts, models and routing settings versioned; rollback in minutes | [AI gateway](ai/ai-gateway-and-fallback.md) |
| Transparency | Companion labelled as AI with sources; flags show their reasons; forecasts show ranges | Each AI use document |
| Cost (FinOps) | Monthly token budget per feature, alerts at 80%, hard stop at 100% (animal care: alert only, no automatic stop) | [AI gateway](ai/ai-gateway-and-fallback.md) |
| Who decides | Monthly AI review (head keeper, operations manager, content lead, safety lead, technology provider) checks signals, fallbacks and costs. A new AI use needs an owner, a test set, a fallback, a cost budget and a pass against ADR-016 | [ADR-016](../adrs/ADR-016-where-generative-ai-is-not-used.md) |
| AI register (AI inventory) | One table lists every AI use with its method, route, fallback, check and owner | [Every AI use at a glance](ai/README.md#every-ai-use-at-a-glance) |
| Regulation | EU AI Act transparency duties (Article 50) apply from 2 August 2026: the companion and the insight assistant say they are AI. None of our AI uses appears to fall in the Act's high-risk categories; the assessment is reviewed yearly | [ADR-017](../adrs/ADR-017-privacy-by-design.md) |
| Frameworks | Governance follows the structure of ISO/IEC 42001 (AI management systems) and the NIST AI Risk Management Framework, used as references, not claimed as certification | This section |

## 8.6 Data management

| Data | Stored in | Kept |
|---|---|---|
| Raw device readings and events | Data hub (object storage) | Full history; older data compacted if cost requires |
| Curated history (visits, sales, care records) | Data hub | Full history, for forecasting and audits |
| Welfare records (readings, flags, verdicts, diagnoses, population events) | Animal care and data hub | As required by zoo licensing |
| Visitor accounts and consent | Loyalty and offers | Until the visitor deletes the account, or after inactivity |
| Animal camera clips and marked frames | Data hub | Clips for a limited period; marked frames kept for retraining |
| AI prompts and answers | AI gateway logs | 30 days, personal data removed |
| Metrics (cost, quality, performance) | Monitoring | 13 months |

Data we own (history, marked frames, test sets, prompts) stays in our data hub, so any AI provider can be replaced.

## 8.7 Resilience: failure modes

| Failure | Alarms | Gates | Box office payments | Keeper app | Companion | Crowd levels | Sensor data |
|---|---|---|---|---|---|---|---|
| A sensor loses contact | Faulty-sensor alert; urgent for animal sensors | — | — | Shows "no data" | — | Gap for that zone | Gap flagged |
| One edge device fails | The other takes over | Unaffected | Unaffected | Unaffected | Unaffected | Unaffected | Unaffected |
| Estate internet down (fibre and backup) | **Work locally** (radio, pagers) | Work offline | Terminals switch to own mobile connection | Works offline, syncs later | Core works offline; no AI answers | Local only; cloud dashboards pause | Buffered on site (72 h) |
| Cloud outage | Work locally | Work offline | Unaffected | Works offline | Core offline; no AI | Pause | Buffered |
| AI provider down or budget used up | Unaffected | Unaffected | Unaffected | Briefing becomes a plain list | Backup provider, then prepared answers | Forecast uses rules | Unaffected |
| Power cut in a zone | Battery sensors continue; edge devices on backup power | Gates on backup power | Terminals on battery | Unaffected | Unaffected | Mains-powered counters stop | Battery sensors continue |
| Payment provider down | — | — | Cash; online sales pause | — | Ticket purchase pauses | — | — |
| Mobile network down | Radio unaffected; SMS backup lost | Unaffected | Unaffected: terminals use the estate network; their mobile backup is lost | Uses estate Wi-Fi where available | Visitor Wi-Fi at key spots | Unaffected | Unaffected |

## 8.8 Observability

| What | How |
|---|---|
| Services and devices | Health checks, metrics and alerts; device health from the edge devices |
| Requests across services | One trace ID from app to companion to AI gateway to provider (distributed tracing) |
| AI quality and cost | Per-feature signals and budgets (section 8.5) |
| On-site kit | Edge devices managed and updated remotely; spare edge device and test sensors on site to test alarm rules before release |

## 8.9 Audit trail

One audit trail records who did what, when, from where and why, for regulators (zoo licensing, ride safety, data protection), finance and AI accountability.

| What is recorded | Examples | Kept |
|---|---|---|
| Money and prices | Price rule changes and approvals, refunds, box office corrections | As accounting law requires |
| Animal welfare | Flags, keeper verdicts, diagnoses, treatments, population events, alarms, acknowledgements and escalations | As zoo licensing requires |
| Ride safety | Ride status changes, incidents, maintenance records | As ride safety inspection requires |
| AI decisions | Feature, model and prompt version, sources or figures used, output and confidence, fallback used, human approver or verdict | With the business record it affected; raw prompts and answers 30 days, personal data removed |
| Personal data | Access to visitor accounts, exports, deletions, consent changes | As GDPR accountability requires |
| Settings and access | Role changes, AI feature switches, thresholds, alarm rule versions pushed to edge devices, admin sign-ins | At least 2 years |

| Aspect | Approach |
|---|---|
| Record content | Who (person or service), what, when (synchronised clocks), where (app, device, site or cloud), values before and after, reason or approval |
| Tamper protection | Append-only, write-once storage; no role, including admins, can edit or delete records; integrity checked automatically |
| Offline | Edge devices and apps record audit events locally and upload them with the backlog, keeping original timestamps |
| Who can read it | A read-only auditor role; reading the audit trail is itself audited |
| Exports | Filtered exports for regulators and data-protection requests |
| Not in the audit trail | Card data (payment provider) and raw sensor readings (data hub) |
