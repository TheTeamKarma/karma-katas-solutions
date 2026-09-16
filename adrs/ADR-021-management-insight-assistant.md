# ADR-021: Management insight assistant: fixed tools over checked figures

**Status:** Accepted · **Date:** 2026-09-15 · **Related:** [ADR-006](ADR-006-ai-gateway.md), [ADR-008](ADR-008-ai-autonomy-levels.md), [ADR-015](ADR-015-validating-ai-results.md), [ADR-016](ADR-016-where-generative-ai-is-not-used.md)

## Context

Managers need answers that cut across areas: "Which zones got busier after the new feeding show, and did we staff them enough?" Today that means opening several dashboards and combining figures by hand. Generative AI can take a plain-language question and write a short answer or report, but a wrong figure looks as convincing as a right one, and [ADR-016](ADR-016-where-generative-ai-is-not-used.md) keeps generative AI out of numerical decisions.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Dashboards and the monthly report only | No new risk; already built | Cross-area questions stay slow; managers wait for the monthly report |
| Language model writes its own database queries | Answers almost any question | Wrong joins and filters give wrong figures that look right; hard to test; needs broad data access |
| **Fixed read-only tools that return checked figures; the model picks tools and writes the answer** | Every figure comes from the same definitions as the dashboards; testable; access limited by role | Only answers what the tools cover; tools need upkeep |
| AI chat built into a bought reporting product | Quick to switch on | Tied to that product; bypasses our gateway, budgets and test sets |

## Decision

A **management insight assistant** answers managers' questions and builds quick reports using **fixed, read-only tools**. Each tool returns a **checked figure** from the data hub's semantic layer: the same figure, with the same definition, that the dashboards show (for example "visitors per zone and hour", "annual pass renewals", "flags confirmed by keepers").

| Rule | Detail |
|---|---|
| Areas covered | Visitors and popularity · sales, passes and prices · forecasts and staffing · return-visit offers · animal care summaries · AI quality and cost |
| Figures | Only from tools; every figure in an answer links to its source. An answer with an untraceable figure is blocked, as for the monthly report. The check proves where a figure came from, not that the right figure was chosen, so each answer also shows the tool inputs used (for example week and zone), and the test set includes ambiguous and paraphrased questions |
| Unanswerable questions | "The assistant can't answer that yet", with a link to the nearest dashboard; logged as a request for a new tool |
| Access | Tools check the manager's role: animal records for animal care managers and the vet, revenue for finance and management |
| What it may do | Read and explain only; never change prices, publish, send offers or assign staff ([ADR-008](ADR-008-ai-autonomy-levels.md)) |
| Personal data | Tools return totals only, never individual visitors |
| Route | Through the AI gateway's request proxy, with its own budget ([ADR-006](ADR-006-ai-gateway.md)) |
| Roll-out | Year 1 pilot with sales, crowds and forecasts; animal care and offer tools added as those features go live |

**Example technologies** (illustrative, see [section 7.5](../docs/07-deployment.md#75-example-technologies)): figure definitions in the data hub's semantic layer; tools exposed to the model through standard tool calling (for example the Model Context Protocol); any capable generative AI model behind the gateway.

## Consequences

**Positive**

- Cross-area answers in seconds, from the same figures managers already trust
- Accurate numbers without letting generative AI calculate them
- Requests for new tools show what managers actually need

**Negative**

- Limited to the tools built → **Mitigation:** Start with the most-asked questions; add tools from the request log
- Tools must change when figure definitions change → **Mitigation:** Tools read the data hub's definitions, so one change updates dashboards and assistant together

**Risks**

- Managers trust a well-written answer that misreads a correct figure or used the wrong inputs (week, zone): tool inputs shown with every answer; sources shown next to each figure; weekly sampled review by the data owner

## How we'll know it was right

Figure-match, refusal and adoption targets in [section 10.3](../docs/10-quality-requirements.md#103-ai-quality-targets) met; no untraceable figure released.
