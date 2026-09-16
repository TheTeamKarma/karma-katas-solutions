# ADR-003: Alerting with urgency levels; urgent alarms never depend on the internet

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-002](ADR-002-estate-edge-and-connectivity.md), [ADR-007](ADR-007-graceful-degradation.md)

## Context

Animals can die within hours if water oxygen or temperature goes wrong. Keepers work in the grounds where Wi-Fi is patchy, and the estate internet link can fail. App notifications need the internet, so they can't be the only alarm path.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| App push notifications from the cloud | Simple; rich content | Fails when the internet or the keeper's signal is down |
| **Rules on the edge devices alerting staff radios and pagers; SMS backup; app when online** | Works with no internet; staff radios are common in parks and zoos | A radio or pager system to integrate |
| Local sirens and lights at enclosures only | No network needed | Keepers may be far away; no acknowledgement or escalation |

## Decision

Urgent conditions (e.g. dissolved oxygen, temperature, alarm buttons, unusual crowding) are detected by **simple rules on the edge devices**, never AI. Alarms go to the **staff radio and pager system** on site, with **SMS sent by each edge device's own mobile modem** as backup (needs mobile signal, not internet). A firewall rule lets the edge devices reach the radio base station, for alarm messages only. App notifications are sent as well when a connection exists. Unacknowledged alarms **escalate** to the next person after 5 minutes. Alarms and actions are uploaded to the cloud when the link returns.

One **shared alerting service** handles every alert with three urgency levels:

| Urgency | Examples | Delivery | Escalation |
|---|---|---|---|
| **Urgent** | Water quality, temperature, alarm buttons, unusual crowding | Staff radio and pagers from the edge devices; SMS from their mobile modems | Next person after 5 minutes |
| **Check today** | Animal health warning, unexplained piranha drop | Keeper app, morning briefing | Head keeper after 24 hours unreviewed |
| **Routine** | Faulty non-animal sensor, drifting forecast | Daily summary or ticket | Weekly review |

Only rules and people raise "urgent"; AI raises at most "check today".

## Consequences

**Positive**

- Alarms work during internet, cloud and AI outages
- Predictable, testable rules for safety-critical alerts
- Escalation ensures someone responds

**Negative**

- Alarm rules live on site as well as in the cloud → **Mitigation:** Rules managed centrally and pushed to the edge devices with versioning
- Radio integration effort → **Mitigation:** Standard interfaces of common staff radio systems

**Risks**

- Rules set too sensitively cause alarm fatigue: thresholds reviewed with the head keeper after the first month

## How we'll know it was right

Keeper alerted within 60 seconds with the internet down, tested monthly with the on-site test kit; every urgent alarm acknowledged or escalated.
