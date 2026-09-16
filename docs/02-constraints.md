# 2. Constraints

Constraints are fixed conditions the architecture must work within. They come from the brief, from the estate's situation, or from law.

## 2.1 Technical constraints (from the brief)

| Constraint | Brief wording | Consequence for the architecture |
|---|---|---|
| Patchy Wi-Fi | "Wifi coverage on the park is patchy" | Alarms, gates, box office, keeper tools and the companion's core must work without a connection |
| Estate data must reach the cloud | "You can make use of cloud services, but you need to have some ways of getting information from the estate to the cloud" | A designed site-to-cloud path: on-site message broker, store-and-forward buffering, fibre with backup link |
| MQTT-capable devices | "Assume there is a budget for MQTT-capable hardware devices which can be installed throughout the park" | All device data reaches the platform as MQTT; long-range battery sensors are converted to MQTT on site |

## 2.2 Organisational constraints

| Constraint | Consequence |
|---|---|
| The estate is losing money | Running cost is a deciding quality; managed services; AI budgets with hard stops |
| Small in-house IT team (assumption) | Few, coarse services; remotely managed edge devices; clear alerts. The technology provider builds and operates the platform |
| Almost no historical data (no idea what's popular; no animal records in digital form) | Every AI feature starts with a simple rules-based version and improves as data builds up |
| Historic 18th-century grounds (assumption) | Cabling across the grounds is hard; wireless devices in the grounds |

## 2.3 Legal and regulatory constraints

The brief names no country. The design meets the strictest common standard, so it holds in the EU and the UK.

| Area | Constraint | Consequence |
|---|---|---|
| Personal data | GDPR-standard data protection (also meets UK GDPR) | Collect only what's needed (data minimisation); consent for location and offers; personal data removed before AI calls; retention limits; hosting region is a setting; a data protection impact assessment (DPIA) before launch; a data protection officer (DPO) role |
| Card payments | Card data security standard (PCI DSS) | Card data handled only by the payment provider; never passes through the platform |
| Pricing | Price differences by sex are generally unlawful for goods and services in the EU and UK | Prices depend on time, demand and ticket type only; never on personal characteristics |
| AI transparency | People must be told when they interact with an AI system (EU AI Act transparency duties, Article 50, applying from 2 August 2026) | The companion and the management insight assistant are clearly labelled as AI and show their sources |
| Animal welfare | Zoo licensing requires welfare and veterinary records | Care records, flags, keeper verdicts and diagnoses kept as an audit trail |
| Ride safety | Inspection and maintenance records required | Ride status and maintenance records kept; safety decisions stay with people |

## 2.4 Conventions

| Convention | Used for |
|---|---|
| arc42 | Structure of this document |
| C4-style diagrams (system context, container and component levels) plus supporting views (capability map, intelligence pipeline, runtime and deployment), written in Mermaid | All diagrams: text-based, versioned, rendered on GitHub |
| Architecture decision records (ADRs) | Significant decisions, 1–2 pages each, in [adrs/](../adrs/); each ends with a fitness function, a measurable check that the decision still holds |
| Providers named by role | External providers are described by what they do, not by vendor, so each stays replaceable; example products are listed separately in [section 7.5](07-deployment.md#75-example-technologies) |

## 2.5 Key assumptions

The brief leaves these open. Each is stated so the design can be checked if it proves wrong.

| Assumption | If wrong |
|---|---|
| Estate buildings have internet (fibre possible) | Satellite or mobile link as the main connection; offline modes carry more |
| Mobile data coverage in the grounds is partial | The companion relies more on its downloaded guide |
| The brief's 15,000 visitors a day is an average, not a peak | Peak sizing rises accordingly |
| The busiest days bring 2–3× the average visitors | On-site parts (entry scanners, entrance Wi-Fi, box office) start at a design day at the lower end and expand in modular steps; operations measures (pre-booked timed arrival slots, temporary extra lanes) cover the busiest days |
| Keepers record observations reliably | Animal AI can't be validated; care routines need adjusting |
| Many animals live in groups and can't all be identified individually | Monitoring per group or enclosure rather than per animal |
| About 300 open days a year; sensors run all 365 days | Cost estimates shift; design unchanged |
| Today the estate has only a basic box office, paper keeper records and patchy Wi-Fi (the brief states only the Wi-Fi) | More existing systems would be connected through events and APIs ([ADR-020](../adrs/ADR-020-new-platform-as-baseline.md)) |
| The rides have their own safety systems, which passed inspection (the brief says the rides passed inspection) | The platform only reads ride status and never controls rides; if no status output exists, ride status is entered by operators |
