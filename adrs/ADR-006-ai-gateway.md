# ADR-006: One AI gateway for all AI calls

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-005](ADR-005-cloud-first-ai.md), [ADR-007](ADR-007-graceful-degradation.md), [ADR-013](ADR-013-managed-ai-services.md)

## Context

The brief asks how we deal with the best models changing, providers changing prices, and providers shutting down. Our AI features use generative AI, managed machine learning, custom vision and translation. Without a central layer, each feature would need code changes to switch providers and would handle costs and personal data separately, locking the estate into each provider (vendor lock-in).

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Features call providers directly | Simplest start | Switching needs code changes everywhere; no central cost or privacy control |
| A cloud provider's own AI gateway | Managed | Ties the AI layer to one cloud |
| **Open-source AI gateway in our cloud, configured with our policies** | Portable; mature routing and failover; our rules | A component to run and keep updated |
| Build a gateway from scratch | Full control | Effort with no advantage |

## Decision

Every AI call passes through **one AI gateway**: an open-source gateway running in our cloud, with a request proxy for generative AI and translation calls and a batch job runner for machine learning and vision jobs. Both routes follow the same policies:

| Policy | Rule |
|---|---|
| Failover | Automatic on errors; hot standby for the companion, warm standby for nightly features |
| Switching for quality or price | Manual, after the feature's test set passes |
| Cost (FinOps) | Monthly token budget per feature: alert at 80%, hard stop at 100% (animal care alerts the head keeper and finance instead) |
| Privacy and safety | Personal data removed (PII redaction); input and output checks |
| Change control | Versioned prompts, models and routing; rollback in minutes |

Supporting controls (rate limits, key vault, caching, tracing) are described in [AI gateway and fallback](../docs/ai/ai-gateway-and-fallback.md).

**Example technologies** (illustrative, see [section 7.5](../docs/07-deployment.md#75-example-technologies)): LiteLLM or Portkey as the gateway; Langfuse for tracing; a cloud key vault for provider keys.

## Consequences

**Positive**

- Answers all three uncertainty questions in the brief
- One place for cost, privacy and quality controls
- Rollback of any AI change in minutes

**Negative**

- Adds a component and a little delay → **Mitigation:** Runs as at least two instances close to services; response time monitored
- Backup models answer differently → **Mitigation:** Separate prompt versions per provider; test set must pass on both

**Risks**

- The chosen open-source project loses support: it sits behind our own interface and can be replaced

## How we'll know it was right

Companion on its backup within seconds of a provider outage, other features by their next run; price or quality switches within 1 day after tests; no app release; every feature except animal care stays within its budget (animal care alerts instead); personal data found in sampled logs fixed immediately.
