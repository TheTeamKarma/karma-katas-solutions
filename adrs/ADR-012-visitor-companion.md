# ADR-012: Visitor companion: approved content, fixed safety answers, offline core

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-006](ADR-006-ai-gateway.md), [ADR-007](ADR-007-graceful-degradation.md), [ADR-008](ADR-008-ai-autonomy-levels.md)

## Context

The brief asks for AI that helps customers and grows visitor numbers. Visitors include families and tourists in many languages; the collection includes venomous animals; Wi-Fi in the park is patchy. A generative AI assistant that invents an answer about opening times, prices or a dangerous animal would harm visitors and trust.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| General chatbot | Answers anything | Invents answers; unsafe; hard to check |
| **Retrieval-augmented answers from approved content, fixed safety answers, bounded tools, offline guide** | Grounded, checkable, safe; useful without signal | Content team maintains approved content |
| AI model running on the phone | Works offline | Heavy for older phones; hard to update and check |
| Staff live chat only | Human quality | Doesn't scale to thousands of visitors |

## Decision

The companion answers **only from approved estate content** plus live facts (waits, feeding times, the visitor's tickets), using **retrieval-augmented generation with sources shown**. A safety check runs first, inside the companion service without calling any AI provider (keyword lists plus a small pre-trained matching model): **safety topics** (dangerous animals, bites and stings, first aid, lost children, emergencies) get **fixed, approved, pre-translated answers, never generated**; when unsure, a question is treated as safety. When nothing relevant is found, it says so and points to staff. Tools are read-only; actions (add to plan, go to checkout) are **confirmed by the visitor** in normal app screens. **Offline**, the app offers a downloaded guide with search, map, saved plan and last known waits. Languages are an admin-chosen set, with people checking safety translations. Conversations stay on the phone; server logs have personal data removed and kept 30 days. Cost is limited by a daily cap inside the monthly budget and about 30 questions per visitor per day. The companion is clearly labelled as an AI assistant.

## Consequences

**Positive**

- Helpful in the visitor's language, even without signal
- Safety answers can't be wrong through generation
- Every answer traceable to approved content

**Negative**

- Content gaps mean more "I don't know" answers → **Mitigation:** Unanswered questions become the content team's to-do list
- Largest AI running cost, growing with visitors → **Mitigation:** Caps, cheaper models for simple questions, exact-match cache

**Risks**

- A safety question slips through as normal: fail-safe routing, sampled audits, any miss fixed immediately

## How we'll know it was right

Safety routing, answer quality and helpfulness targets in [section 10.3](../docs/10-quality-requirements.md#103-ai-quality-targets) met.
