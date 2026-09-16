# Architecture Decision Records

Each record gives the status, context, options considered, decision, consequences, and how we'll know it was right (a fitness function: a measurable check that the decision still holds).

| ADR | Decision | Type |
|---|---|---|
| [ADR-001](ADR-001-service-based-architecture.md) | Service-based architecture with event-driven integration | Platform |
| [ADR-002](ADR-002-estate-edge-and-connectivity.md) | Estate edge: LoRaWAN, MQTT, two edge devices and store-and-forward | Platform |
| [ADR-003](ADR-003-local-alarm-path.md) | Alerting with urgency levels; urgent alarms never depend on the internet | Platform |
| [ADR-004](ADR-004-ticketing-build-payments-buy.md) | Build the ticketing core, buy payments | Platform |
| [ADR-005](ADR-005-cloud-first-ai.md) | Run AI in the cloud; no AI servers on site | AI |
| [ADR-006](ADR-006-ai-gateway.md) | One AI gateway for all AI calls | AI |
| [ADR-007](ADR-007-graceful-degradation.md) | Every AI feature falls back to a non-AI version | AI |
| [ADR-008](ADR-008-ai-autonomy-levels.md) | Clear limits on what AI may do on its own | AI |
| [ADR-009](ADR-009-animal-health-warnings.md) | Animal health warnings: staged anomaly detection with keeper confirmation | AI |
| [ADR-010](ADR-010-piranha-population-estimate.md) | Piranha population: estimate with a range, reconciled with counts and events | AI |
| [ADR-011](ADR-011-demand-forecast-staff-planning.md) | Demand forecast driving staff planning | AI |
| [ADR-012](ADR-012-visitor-companion.md) | Visitor companion: approved content, fixed safety answers, offline core | AI |
| [ADR-013](ADR-013-managed-ai-services.md) | Use managed AI services; don't build models | AI |
| [ADR-014](ADR-014-camera-behaviour-analysis-deferred.md) | Defer behaviour analysis from camera clips | AI |
| [ADR-015](ADR-015-validating-ai-results.md) | Validating AI results before release and in production | AI |
| [ADR-016](ADR-016-where-generative-ai-is-not-used.md) | Where generative AI is deliberately not used | AI |
| [ADR-017](ADR-017-privacy-by-design.md) | Privacy by design: anonymous sensing and minimal personal data | Privacy |
| [ADR-018](ADR-018-visitor-channels-and-connectivity.md) | Visitor channels: web and native apps, offline core, Wi-Fi at key spots | Platform |
| [ADR-019](ADR-019-pricing-and-capacity.md) | Pricing as data, forecast-driven price suggestions approved by admins, timed arrival slots only on the busiest days | Platform |
| [ADR-020](ADR-020-new-platform-as-baseline.md) | The new non-AI platform is the baseline the AI additions must fit | Platform |
| [ADR-021](ADR-021-management-insight-assistant.md) | Management insight assistant: fixed tools over checked figures | AI |
| [ADR-022](ADR-022-phone-photos-in-animal-care.md) | Phone photos in animal care: staged vision features | AI |
| [ADR-023](ADR-023-return-visit-offers.md) | Return-visit offers: rules first, prediction later, measured against a holdout group | AI |
