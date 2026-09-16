# 6. Runtime View

Key scenarios, step by step. Platform scenarios are shown here; each AI use has its own runtime view.

| Diagram | Scenario | Where |
|---|---|---|
| 6.1 | Buying a family ticket online | Below |
| 6.2 | Entry at the gate with the internet down | Below |
| 6.3 | Urgent animal alarm with the internet down | Below |
| 6.4 | Animal health warning, from feeding record to vet diagnosis | [Animal health warnings](ai/animal-health-warnings.md) |
| 6.5 | Nightly piranha estimate to keeper check | [Piranha population](ai/piranha-population.md) |
| 6.6 | Nightly forecast to published staff shifts | [Demand forecast and staffing](ai/demand-forecast-staffing.md) |
| 6.7 | A family's question, a safety question, and offline use | [Visitor companion](ai/visitor-companion.md) |
| 6.8 | Primary AI provider fails during a companion question | [AI gateway and fallback](ai/ai-gateway-and-fallback.md) |

*Key for all runtime views: numbered steps in time order; solid arrows are requests or messages; dashed arrows are replies; "alt" boxes show alternative paths; grey notes show conditions.*

## 6.1 Buying a family ticket online

**Diagram 6.1 · Runtime view: buying a family ticket online**

```mermaid
sequenceDiagram
  autonumber
  actor V as Visitor
  participant A as Visitor app or web
  participant T as Ticketing
  participant P as Payment provider
  participant E as Message bus and data hub
  participant G as Entry, gates via edge devices
  V->>A: Choose Saturday, family ticket 2 adults and 2 children
  A->>T: Get price for date and ticket type
  T->>T: Apply pricing rules for weekend, lock price for this order
  T-->>A: Price with any fair-use limit checks
  V->>A: Pay
  A->>T: Start payment for this order
  T->>P: Create payment session
  P-->>A: Provider's secure card form
  V->>P: Card details entered on the provider's form
  P->>T: Payment confirmed notification
  T->>T: Issue signed ticket with validity dates
  T-->>A: Ticket saved in the app and phone wallet, works offline
  T->>E: Sale integration event for forecasts and insight
  T->>G: Updated ticket list and cancelled tickets, synced over HTTPS
```

Card details go straight to the payment provider's secure form and never pass through the platform. The signed ticket works at the gate without a connection.

## 6.2 Entry at the gate with the internet down

**Diagram 6.2 · Runtime view: gate entry while the estate internet link is down**

```mermaid
sequenceDiagram
  autonumber
  actor V as Visitor
  participant GL as Gate lane
  participant ED as Edge devices
  participant T as Ticketing in the cloud
  Note over GL,T: Estate internet link is down
  V->>GL: Show signed ticket or annual pass
  GL->>GL: Check signature and validity dates without a connection
  GL->>ED: Check cancelled list and entries recorded at other lanes
  ED-->>GL: Status of this ticket
  alt Valid, not cancelled, not used today
    GL-->>V: Gate opens within 1 second
    GL->>ED: Record entry, shared with all lanes on site
  else Cancelled or already used at another lane
    GL-->>V: Refused, staff check
  end
  Note over ED,T: Link returns
  ED->>T: Upload entries and box office sales
  T->>T: Reconcile and flag duplicates for review
```

Accepted risk: during an outage, a screenshot of the same ticket could be used at two lanes before their records meet. Lanes share entries through the edge devices, which makes this rare, and duplicates are flagged when the link returns (the lanes and the cloud are eventually consistent).

## 6.3 Urgent animal alarm with the internet down

**Diagram 6.3 · Runtime view: urgent water-quality alarm while the estate internet link is down**

```mermaid
sequenceDiagram
  autonumber
  participant S as Water sensor, aquarium
  participant ED as Edge device, active
  participant R as Staff radio and pagers
  actor K as Keeper on duty
  actor H as Head keeper
  participant C as Cloud platform
  Note over ED,C: Estate internet link is down
  S->>ED: Dissolved oxygen below safe limit, wired MQTT
  ED->>ED: Local alarm rule matched, urgent
  ED->>R: Alarm to keeper on duty
  R->>K: Pager alert with enclosure and reading
  alt Acknowledged within 5 minutes
    K->>R: Acknowledge
    R->>ED: Acknowledgement recorded
  else No acknowledgement
    ED->>R: Escalate to head keeper
    R->>H: Pager alert
    ED->>H: SMS from edge device mobile modem, if signal
  end
  K->>K: Fix aeration, record action in keeper app offline
  ED->>ED: Buffer readings and alarm history
  Note over ED,C: Link returns
  ED->>C: Alarm first, then latest readings, then backlog
  C->>C: Alarm and actions added to welfare records
```

The whole alarm path runs on site: sensor, edge device, staff radio, with SMS from the edge device's own mobile modem. The aquarium sensor is wired; battery sensors in the grounds reach the edge devices the same way through LoRaWAN gateways. Target: keeper alerted within **60 seconds**. The cloud receives the alarm and actions afterwards for the welfare record.
