# ADR-002: Estate edge: LoRaWAN, MQTT, two edge devices and store-and-forward

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-003](ADR-003-local-alarm-path.md), [ADR-001](ADR-001-service-based-architecture.md)

## Context

The brief states three technical constraints: Wi-Fi coverage in the park is patchy; estate data must reach the cloud; there is a budget for MQTT-capable devices. About 550 devices by year 3 are spread across historic grounds where cabling is hard, and animal sensors must work all year.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Extend Wi-Fi for devices | High bandwidth; devices speak MQTT | Mains power and many access points across heritage grounds; costly |
| **LoRaWAN for battery sensors, MQTT for wired or Wi-Fi devices, via two edge devices** | Kilometre range; years on battery; few gateways; alarms and buffering on site | Low bandwidth (no video); LoRaWAN needs a network server to publish to MQTT |
| Mobile-network IoT only | Wide coverage; MQTT directly | Depends on operator coverage (unknown); fee per device; no local alarm path |
| Devices straight to the cloud | Nothing on site | Alarms and data fail when the internet does |

## Decision

**Battery sensors, counters and alarm buttons** in the grounds use **LoRaWAN**, heard by several overlapping gateways. **Wired or Wi-Fi MQTT devices** are used in and near buildings (aquarium, animal houses, animal cameras, ride stations). Everything arrives at **two small industrial edge devices** in different buildings (active-passive failover, backup power, managed remotely), which run the **MQTT broker**, the LoRaWAN network server, local alarm rules, a **store-and-forward buffer** (at least 72 hours) and gate sync. The site connects to the cloud over **fibre with automatic 4G/5G backup**, or low-orbit satellite if a site survey finds weak mobile signal. After an outage, alarms are sent first, then latest readings, then backlog. **All sensor readings** are sent to the cloud; cameras send counts and clips, never continuous video.

**Example technologies** (illustrative, see [section 7.5](../docs/07-deployment.md#75-example-technologies)): ChirpStack as the LoRaWAN network server, Eclipse Mosquitto or EMQX as the broker, industrial IoT gateways as edge devices, a managed cloud MQTT service as the bridge.

## Consequences

**Positive**

- Meets all three technical constraints in the brief
- No single edge device or gateway failure stops alarms; the radio base station is an accepted single point, with SMS from the edge devices as an independent path
- Battery sensors avoid cabling across heritage grounds
- Full reading history for AI training, validation and welfare records

**Negative**

- Two edge devices to maintain on site → **Mitigation:** Fanless, remotely managed, updated from the cloud
- LoRaWAN can't carry images → **Mitigation:** Cameras use the wired or Wi-Fi network and send only counts and short clips

**Risks**

- Radio gaps in dense or walled areas: radio survey before installation; mobile-network IoT for spots out of reach

## How we'll know it was right

No sensor readings lost in outages up to 72 hours; every enclosure heard by at least two gateways; edge failover tested monthly.
