# ADR-018: Visitor channels: web and native apps, offline core, Wi-Fi at key spots

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-002](ADR-002-estate-edge-and-connectivity.md), [ADR-012](ADR-012-visitor-companion.md)

## Context

Visitors book at home and use the park where Wi-Fi is patchy and mobile coverage is unknown. Some tourists pay roaming charges. The companion and tickets must still work in the grounds.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Web app only | Cheapest; no install | Weak offline support in the grounds |
| **Web app plus native apps, with an offline core** | Best offline behaviour; wallet tickets; push notifications | Two app platforms to maintain |
| Estate-wide visitor Wi-Fi | Always online | Costly across heritage grounds; hard at 9,000 people; burden on small IT team |
| **Visitor Wi-Fi guaranteed at key spots, used elsewhere where it reaches** | Everyone can download content at the entrance; cheap | Gaps between spots |

## Decision

Offer a **web app and native mobile apps**, plus the on-site box office, all on one ticketing system. Anything visitors use in the grounds works **offline-first**: tickets in the app and phone wallet, downloaded guide, map and saved day plan. Apps go **online first** when a connection exists (visitor's mobile data or estate Wi-Fi) for AI answers and live waits. **Visitor Wi-Fi is guaranteed at key spots** (entrance, cafés, main squares), used elsewhere where coverage happens to reach, and always kept **separate** from device, staff and payment networks. Visitors are prompted to download the guide at home or at the entrance.

## Consequences

**Positive**

- Tickets and guide always work in the grounds
- Visitors without mobile data still get the content
- Visitor devices can never reach estate systems

**Negative**

- Two native app platforms to build and release → **Mitigation:** Content, settings and AI prompts update without app releases
- No AI answers where there is no signal → **Mitigation:** Offline guide covers core needs; answers return when connected

**Risks**

- Few visitors download the guide: prompts at booking and on entrance Wi-Fi

## How we'll know it was right

100% of guide, map and plan available offline after download; the guide downloads in under 1 minute on entrance Wi-Fi during the design-day morning peak (Q13).
