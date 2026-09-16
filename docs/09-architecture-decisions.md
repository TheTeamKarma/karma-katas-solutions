# 9. Architecture Decisions

All significant decisions are recorded as ADRs in [adrs/](../adrs/README.md). Each gives the context, options considered, the decision, consequences with mitigations, and a fitness function: a measurable check of whether it was right.

## Platform

| ADR | Decision |
|---|---|
| [ADR-001](../adrs/ADR-001-service-based-architecture.md) | Service-based architecture with event-driven integration |
| [ADR-002](../adrs/ADR-002-estate-edge-and-connectivity.md) | Estate edge: LoRaWAN, MQTT, two edge devices and store-and-forward |
| [ADR-003](../adrs/ADR-003-local-alarm-path.md) | Alerting with urgency levels; urgent alarms never depend on the internet |
| [ADR-004](../adrs/ADR-004-ticketing-build-payments-buy.md) | Build the ticketing core, buy payments |
| [ADR-018](../adrs/ADR-018-visitor-channels-and-connectivity.md) | Web and native apps, offline core, visitor Wi-Fi at key spots |
| [ADR-019](../adrs/ADR-019-pricing-and-capacity.md) | Pricing as data, forecast-driven price suggestions approved by admins, timed arrival slots only on the busiest days |
| [ADR-020](../adrs/ADR-020-new-platform-as-baseline.md) | The new non-AI platform is the baseline the AI additions must fit |

## AI approach

| ADR | Decision |
|---|---|
| [ADR-005](../adrs/ADR-005-cloud-first-ai.md) | Run AI in the cloud; no AI servers on site |
| [ADR-006](../adrs/ADR-006-ai-gateway.md) | One AI gateway for all AI calls |
| [ADR-007](../adrs/ADR-007-graceful-degradation.md) | Every AI feature falls back to a non-AI version |
| [ADR-008](../adrs/ADR-008-ai-autonomy-levels.md) | Clear limits on what AI may do on its own |
| [ADR-013](../adrs/ADR-013-managed-ai-services.md) | Use managed AI services; don't build models |
| [ADR-015](../adrs/ADR-015-validating-ai-results.md) | Validating AI results before release and in production |
| [ADR-016](../adrs/ADR-016-where-generative-ai-is-not-used.md) | Where generative AI is deliberately not used |
| [ADR-017](../adrs/ADR-017-privacy-by-design.md) | Privacy by design: anonymous sensing and minimal personal data |

## AI uses

| ADR | Decision |
|---|---|
| [ADR-009](../adrs/ADR-009-animal-health-warnings.md) | Animal health warnings: staged anomaly detection with keeper confirmation |
| [ADR-010](../adrs/ADR-010-piranha-population-estimate.md) | Piranha population: estimate with a range, reconciled with counts and events |
| [ADR-011](../adrs/ADR-011-demand-forecast-staff-planning.md) | Demand forecast driving staff planning |
| [ADR-012](../adrs/ADR-012-visitor-companion.md) | Visitor companion: approved content, fixed safety answers, offline core |
| [ADR-014](../adrs/ADR-014-camera-behaviour-analysis-deferred.md) | Defer behaviour analysis from camera clips |
| [ADR-021](../adrs/ADR-021-management-insight-assistant.md) | Management insight assistant: fixed tools over checked figures |
| [ADR-022](../adrs/ADR-022-phone-photos-in-animal-care.md) | Phone photos in animal care: staged vision features |
| [ADR-023](../adrs/ADR-023-return-visit-offers.md) | Return-visit offers: rules first, prediction later, measured against a holdout group |
