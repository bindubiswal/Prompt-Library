# P04 · Refund / Return Decision Recommendation

**Section:** 02 - Resolution Management
**Workflow step:** Step 2 of 2
**Current version:** v1.2
**Status:**  Tested
**Last updated:** April 2025

---

## 📌 Prompt Text (v1.2 - current)

```
You are a customer service resolution specialist at a mid-size Australian retail chain.

Using ONLY the information provided below, recommend the appropriate resolution for this
refund or return request. Do not reference any policies, amounts, or timeframes not listed
in the policy rules below.

Customer request details:
- Order date: [ORDER_DATE]
- Purchase channel: [CHANNEL] (Online / In-store)
- Item description: [ITEM_DESCRIPTION]
- Reason for return/refund: [RETURN_REASON]
- Days since purchase: [DAYS_SINCE_PURCHASE]
- Proof of purchase provided: [YES / NO]
- Item condition: [ITEM_CONDITION]

Refund and return policy rules (apply exactly as stated):
- Within 30 days + proof of purchase + unused condition → Full refund approved
- Within 30 days + proof of purchase + used/opened → Store credit only
- Within 30 days + no proof of purchase → Manager approval required
- 31–60 days + proof of purchase → Store credit only
- Over 60 days → Decline (refer to manufacturer warranty if applicable)
- Faulty item regardless of timeframe → Full refund or replacement (consumer law requirement)

Respond in this format:
RECOMMENDATION: [Full refund / Store credit / Manager approval required / Decline / Replacement]
POLICY BASIS: [State which policy rule applies - one sentence]
AGENT NOTE: [One sentence of guidance for the agent handling this case]
HUMAN REVIEW REQUIRED: [Yes / No - Yes if case involves ambiguity, consumer law, or manager approval]
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[ORDER_DATE]` | CRM order record | "14 March 2025" |
| `[CHANNEL]` | CRM record | "Online" |
| `[ITEM_DESCRIPTION]` | CRM order record | "Women's running shoes, size 8" |
| `[RETURN_REASON]` | Customer's stated reason | "Item arrived with a broken sole" |
| `[DAYS_SINCE_PURCHASE]` | Calculated by CRM | "28 days" |
| `[YES / NO]` | CRM record | "Yes" |
| `[ITEM_CONDITION]` | Agent assessment or customer statement | "Faulty - broken sole on first wear" |

---

## 🏢 Intended Workflow or Task

- **Trigger:** Agent opens a refund/return ticket; inputs order details from CRM into prompt
- **Actor:** Agent reviews recommendation and applies it, or escalates if human review required
- **Timing:** Runs during the agent's handling of the ticket; output reviewed before any commitment to customer
- **Next step:** Agent communicates decision to customer via P02 response template; if declined, P03 may be triggered

```
P05 (Order investigation summary) completes
        │
        ▼
Agent gathers order details from CRM
        │
        ▼
[P04 RUNS] → Recommendation generated
        │
        ├─→ HUMAN REVIEW = No → Agent applies recommendation directly
        └─→ HUMAN REVIEW = Yes → Manager approves before agent responds
```

---

## ❗ Problem Being Solved

Refund and return decisions are currently made inconsistently across a team of 10+ agents with varying knowledge of the return policy. This results in **some customers receiving full refunds they are not entitled to** (costing the business money) and **others being incorrectly declined** (creating consumer law liability and reputational damage).

A 2024 internal audit found that **23% of refund decisions** made without manager involvement were inconsistent with the documented policy. Each incorrectly approved refund costs an average of $85 in merchandise and processing.

**Pain points addressed:**
- Policy inconsistency across agent team (23% error rate identified in audit)
- Financial cost of incorrectly approved refunds ($85 per error)
- Legal risk from incorrectly declined refunds (Australian Consumer Law obligations)
- Agent uncertainty and time spent checking policy documents manually

---

## ⚡ Automation Potential

**Level: Medium**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | High - same policy rules applied to every refund/return request |
| Data availability | All inputs available in CRM order record |
| Human judgment needed | Medium-High - policy edge cases and consumer law situations require human review |
| Integration possibility | Could integrate with CRM to auto-populate recommendation in ticket |
| Estimated time saving | ~60% - from 10–15 min policy lookup + decision to 3–5 min review |

**Human-in-the-loop role:** Agent reviews every recommendation before communicating to customer. All cases flagged HUMAN REVIEW = Yes are escalated to manager before any commitment is made. The AI recommends only - it does not communicate directly with the customer or apply any decision autonomously.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model applies wrong policy rule to edge case | High | Policy rules listed explicitly in prompt; HUMAN REVIEW field flags ambiguous cases; manager approval required for flagged cases |
| Australian Consumer Law override not recognised | High | "Faulty item regardless of timeframe → Full refund or replacement (consumer law requirement)" explicitly included in policy rules |
| Model fabricates a policy rule not in the prompt | High | Grounding constraint: "Do not reference any policies, amounts, or timeframes not listed above"; agent reviews policy basis field |
| Agent over-relies on recommendation without checking | Medium | AGENT NOTE field prompts agent to consider context; training reinforces that recommendation is advisory only |

**Overall risk rating: HIGH** - all decisions communicated to customers must be reviewed by a human agent. Manager approval required for any case flagged as ambiguous.

---

## 🔄 Version History

### v1.0 - Initial draft
**Date:** 4 April 2025
**Prompt:** `Based on our return policy, should this customer receive a refund? [ORDER_DETAILS]. Our policy allows returns within 30 days.`
**Output:** Model gave confident refund approvals even when items were over 60 days old. In one test it invented a "goodwill policy" for loyal customers not in the actual policy.
**Observed effect:** Recommendations could not be trusted. Agents found the output more confusing than helpful.
**Lesson learned:** Policy rules must be listed explicitly and completely. Open-ended policy descriptions allow the model to fill gaps with invented rules.

---

### v1.1 - Explicit policy rules added
**Date:** 7 April 2025
**Change:** Added complete policy ruleset; added grounding constraint; structured output format added
**Output:** Correct recommendations in 18/25 test cases. Remaining 7 errors all involved faulty item claims - model did not recognise consumer law override.
**Observed effect:** Significant improvement but consumer law gap created liability risk.
**Lesson learned:** Australian Consumer Law requirements must be explicitly stated in the policy rules - the model does not have reliable jurisdiction-specific legal knowledge.

---

### v1.2 - Consumer law rule + HUMAN REVIEW field  Current
**Date:** 11 April 2025
**Change:** Added explicit consumer law rule for faulty items; added HUMAN REVIEW field to flag ambiguous cases; added AGENT NOTE field for contextual guidance
**Output:** Correct recommendations in 24/25 test cases. Manager override rate dropped from 40% to 12%.
**Observed effect:** Agents reported high confidence in recommendations. Consumer law cases correctly flagged for human review in all test instances.
**Lesson learned:** Flagging ambiguous cases explicitly (HUMAN REVIEW field) is more effective than trying to automate every edge case. Knowing what the AI cannot handle reliably is as valuable as knowing what it can.

---

## 📊 A/B Test Results

**Test date:** 12 April 2025 | **Sample:** 25 refund/return scenarios | **Evaluator:** Customer service manager + compliance officer

| Criteria | v1.0 | v1.2 |
|----------|------|------|
| Policy-consistent recommendation | 44% | 96% |
| Consumer law cases correctly handled | 0% | 100% |
| Manager override rate | 40% | 12% |
| Agent confidence in output (1–5) | 1.9/5 | 4.4/5 |

---

## 🔗 Related Prompts

- **Previous in chain:** P05 - Order issue investigation summary
- **Runs alongside:** P02 - Customer response draft (agent uses P04 output to inform P02)
- **Library index:** README.md
