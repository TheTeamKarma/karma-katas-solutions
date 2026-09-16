# ADR-008: Clear limits on what AI may do on its own

**Status:** Accepted · **Date:** 2026-09-14 · **Related:** [ADR-007](ADR-007-graceful-degradation.md), [ADR-012](ADR-012-visitor-companion.md)

## Context

AI features touch money (prices, tickets), animal welfare, visitors including children, and published content. The companion can use tools. Unclear limits risk harm, legal problems and loss of trust.

## Options considered

*Chosen option in bold.*

| Option | Pros | Cons |
|---|---|---|
| Decide case by case | Flexible | Inconsistent; hard to audit |
| Autonomy by confidence score: automatic above a threshold, a person below | Fewer approvals; scales with volume | Confidence scores are poorly calibrated for rare cases; errors on money and welfare are hard to reverse |
| **Three levels: automatic, a person confirms, never AI** | Clear and auditable; reused across features | Some tasks slower because a person confirms |

## Decision

| Level | Rule | Examples |
|---|---|---|
| **Automatic** | Safe, reversible, low impact, within set limits | Update a day plan · reminders · keeper briefing · faulty-sensor tickets · draft reports · read-only answers to manager questions from checked figures · offers to consenting visitors within an approved campaign |
| **A person confirms** (human-in-the-loop) | Money, prices, animal care, anything published | Buying or refunding tickets · applying price suggestions · changing care · publishing content or translations · launching campaigns · assigning staff |
| **Never AI** | Safety and people's rights | Operating rides or safety systems · silencing alarms · medical decisions · generated safety answers · excluding visitors |

The companion may **look things up** and **prepare** actions; the visitor confirms in normal app screens. AI can raise at most a "check today" flag, never "urgent".

## Consequences

**Positive**

- Consistent, explainable limits that judges and regulators can check
- People stay accountable for money, welfare and safety

**Negative**

- Approvals add work for staff → **Mitigation:** Only for money, care and publishing; everything else automatic

**Risks**

- Approval becomes rubber-stamping: approval and rejection rates monitored

## How we'll know it was right

No AI action outside its level found in audits; every price, care and publishing change has a recorded approver.
