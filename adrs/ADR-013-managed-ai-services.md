# ADR-013: Use managed AI services; don't build models

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-005](ADR-005-cloud-first-ai.md), [ADR-006](ADR-006-ai-gateway.md)

## Context

Managed AI products also come and go: Amazon Forecast closed to new customers in July 2024, and Azure AI Anomaly Detector retires on 1 October 2026. Any design that depends on one managed product needs owned data and a portable backup.

The estate isn't a technology company, has a small IT team and must control costs. Forecasting, anomaly detection, generative AI and vision are available as managed services and vendor products.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Build and train our own models | Full control | Specialist skills, cost, slow; little data at first |
| **Managed services, pre-trained models and vendor products; configure and validate them** | Fast; cost-effective; improves as services improve | Dependence on providers |
| Buy complete AI solutions per use | Quick | Many vendors; data locked in; hard to switch |

## Decision

Use **managed AI services** that learn from our data (forecasting, anomaly detection), **pre-trained generative AI models**, and **vendor devices** with built-in AI (people counters). The technology provider selects, configures, integrates, **validates and monitors** them; the IT team runs them day to day. Backups for machine learning features are **standard open-source methods** (e.g. anomaly detection and forecasting libraries) run by the technology provider on our own data. Vision services for piranhas and food trays are taught with a few hundred labelled images each, which is configuration on our data rather than research. All data used for AI (history, marked frames, test sets, prompts) stays in our data hub, so any service can be replaced.

**Example technologies** (illustrative, see [section 7.5](../docs/07-deployment.md#75-example-technologies)): Amazon SageMaker Canvas or BigQuery ML for forecasting and anomaly detection, with StatsForecast and PyOD as backups; Amazon Rekognition Custom Labels or Google AutoML object detection for piranhas, with a YOLO-family detector as backup.

## Consequences

**Positive**

- Realistic for the estate's size and budget
- Features improve as providers improve
- Our data and test sets keep us independent

**Negative**

- Less control over model internals → **Mitigation:** Validation, monitoring and release gates on every feature
- Provider dependence → **Mitigation:** AI gateway, fallbacks and owned data (ADR-006, ADR-007)

**Risks**

- A managed service changes behaviour silently: production signals and sampled reviews catch drift

## How we'll know it was right

No research or hand-built models needed for the four AI uses; each managed service could be replaced by another service or a standard open-source method, run by the technology provider on data in our data hub.
