# ADR-016: Where generative AI is deliberately not used

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-003](ADR-003-local-alarm-path.md), [ADR-008](ADR-008-ai-autonomy-levels.md), [ADR-012](ADR-012-visitor-companion.md)

## Context

Generative AI is flexible but non-deterministic, can be confidently wrong, and costs money per call. Several estate tasks are safety-critical, money-related, numerical, or need to work offline.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Use generative AI wherever possible | One technology; flexible | Unpredictable for numbers, safety and money; high running cost |
| Generative AI as the interface, calling statistics and forecasting tools for every number | One conversational way to reach everything | Adds cost and non-determinism around deterministic results; still needs the underlying methods; kept only where people ask questions ([ADR-021](ADR-021-management-insight-assistant.md)) |
| **Choose per task: rules, statistics, machine learning, vision or generative AI** | Right tool per task; predictable where it matters; lower cost | Several techniques to manage (behind one gateway) |

## Decision

Generative AI is used **only for language**: companion answers from approved content, keeper briefings, insight report narratives, management insight assistant answers ([ADR-021](ADR-021-management-insight-assistant.md)), feedback themes and translation drafts. It is **not used** for:

| Task | Used instead | Method family | Why not generative AI |
|---|---|---|---|
| Urgent alarms | Rules on edge devices | Deterministic rules | Must be instant, predictable and offline |
| Safety answers | Fixed approved answers | Intent classification (embeddings) routing to fixed text | One wrong answer about a venomous animal is unacceptable |
| Health warnings | Anomaly detection on readings | Unsupervised time-series anomaly detection | Numbers over time; needs repeatable, checkable results |
| Fish counting | Object detection | Supervised computer vision (object detection) | Visual counting task |
| Visitor forecasts | Time-series forecasting | Probabilistic time-series forecasting | Poor at numerical forecasting; not repeatable |
| Prices | Rules, forecast-based suggestions approved by admins | Rule-based | Money; must be explainable and fair |
| Day plan route | Constraint scheduling | Constraint programming | Order and timings must be correct |
| Live crowd levels | Counts and arithmetic | Arithmetic on counts | No model needed |
| Popularity analysis | Descriptive statistics on counts, waits, feedback and repeat visits | Descriptive statistics | Needs exact, repeatable numbers; generative AI only writes the report narrative |
| Figures in manager answers | Fixed read-only tools returning checked figures | LLM tool calling over a semantic layer | A wrong figure looks as convincing as a right one; generative AI only picks tools and writes the words |

Rough counterfactual: forecasting and health scoring by generative AI would mean thousands of language model calls every night with less accurate, unrepeatable results; managed forecasting and anomaly detection cost about €5K–15K a year for these tasks.

## Consequences

**Positive**

- Predictable behaviour where safety, money and numbers matter
- Lower running cost
- Generative AI risks confined to language tasks with guardrails

**Negative**

- More than one AI technique to run → **Mitigation:** All behind the AI gateway and the same validation (ADR-006, ADR-015)

**Risks**

- Pressure to add chat-style features everywhere: new generative AI uses must pass this ADR's criteria

## How we'll know it was right

In architecture reviews, no generative AI call sits in an alarm, price or safety-answer path, and no figure shown to people is produced by generative AI.
