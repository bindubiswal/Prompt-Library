# P10 · FAQ Auto-Response for Common Queries

**Section:** 04 — Self-Service Deflection
**Workflow step:** Step 1 of 1
**Current version:** v1.1
**Status:** ✅ Tested
**Last updated:** April 2025

---

## 📌 Prompt Text (v1.1 — current)

```
You are a customer service assistant for [STORE_NAME], a mid-size Australian retail chain.

A customer has submitted a query via the website contact form. Determine whether this query
can be answered using the FAQ answers provided below.

Customer query: [CUSTOMER_QUERY]

FAQ knowledge base (use ONLY these answers — do not answer from general knowledge):

Q: What is your return policy?
A: We accept returns within 30 days of purchase with proof of purchase. Items must be in original condition. Faulty items can be returned at any time under Australian Consumer Law.

Q: How long does delivery take?
A: Standard delivery takes 3–7 business days. Express delivery takes 1–2 business days. Rural and remote areas may take longer.

Q: How do I track my order?
A: You will receive a tracking number via email once your order is dispatched. Visit [CARRIER_TRACKING_URL] and enter your tracking number to check your order status.

Q: Can I change or cancel my order?
A: Orders can be changed or cancelled within 2 hours of placement. After this window, the order cannot be modified. Contact us at [CONTACT_EMAIL] immediately if you need to cancel.

Q: Do you offer price matching?
A: We do not currently offer price matching.

Q: How do I use a gift card?
A: Enter your gift card code at checkout in the "Gift card or discount code" field. Gift cards do not expire and cannot be exchanged for cash.

Respond in this format:

CAN_ANSWER: [Yes / No / Partial]
RESPONSE: [If Yes or Partial — write the customer-facing response in 80 words or less, warm and helpful tone. If No — write exactly: "Thank you for your query. A member of our team will be in touch within 1 business day."]
ESCALATE: [Yes / No — Yes if query requires human follow-up or is not covered by FAQ]
ESCALATION_REASON: [If ESCALATE = Yes — one sentence explaining why]
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[STORE_NAME]` | Business config | "RetailCo" |
| `[CUSTOMER_QUERY]` | Website contact form submission | "Hi, I want to know if I can return shoes I bought last week that don't fit." |
| `[CARRIER_TRACKING_URL]` | Business config | "auspost.com.au/parcels-mail/track" |
| `[CONTACT_EMAIL]` | Business config | "customercare@retailco.com.au" |

---

## 🏢 Intended Workflow or Task

- **Trigger:** Customer submits query via website contact form
- **Actor:** Automated pipeline handles FAQ queries; human agent handles escalated queries
- **Timing:** Response generated within seconds of form submission; FAQ responses sent automatically; escalations routed to agent queue
- **Next step:** If ESCALATE = No — response sent automatically to customer. If ESCALATE = Yes — query routed to P01 (triage) and treated as a complaint or complex enquiry

```
Customer submits website contact form query
        │
        ▼
[P10 RUNS] → CAN_ANSWER assessed
        │
        ├─→ CAN_ANSWER = Yes/Partial → ESCALATE = No → Auto-response sent to customer
        └─→ CAN_ANSWER = No → ESCALATE = Yes → Routed to agent queue → P01 triage
```

---

## ❗ Problem Being Solved

Approximately **40% of all inbound customer service contacts** are simple FAQ queries — return policy questions, delivery timeframes, order tracking, and cancellation requests — that do not require human agent involvement. At 200+ contacts per day, this represents **80+ contacts per day** consuming agent time on questions with known, documented answers.

Each FAQ query handled by an agent takes an average of 8–12 minutes including reading, researching, and responding. Automating FAQ responses would free approximately **10–16 hours of agent time per day** for complex complaints and escalations that genuinely require human judgement.

**Pain points addressed:**
- 40% of contacts are simple FAQ queries consuming skilled agent time (80+ per day)
- Inconsistent answers to the same question across different agents
- Customer wait time for simple queries (currently same queue as complex complaints)
- No 24/7 response capability for out-of-hours queries

---

## ⚡ Automation Potential

**Level: Very High**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | Very high — same FAQ questions received repeatedly every day |
| Data availability | Customer query available at submission; FAQ answers are static and pre-defined |
| Human judgment needed | Very low for in-scope queries; ESCALATE field routes edge cases to agents |
| Integration possibility | Fully integratable with website contact form and CRM email system |
| Estimated time saving | ~90% — 80+ FAQ queries per day resolved without agent involvement |

**Human-in-the-loop role:** Agents monitor escalation queue for queries flagged ESCALATE = Yes. A weekly sample of auto-sent FAQ responses is audited for accuracy and tone. FAQ knowledge base is reviewed and updated monthly by the customer service manager.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model answers from general knowledge rather than FAQ (hallucination) | High | Grounding constraint: "Use ONLY these answers — do not answer from general knowledge"; CAN_ANSWER = No routes unanswered queries to humans |
| FAQ knowledge base becomes outdated (e.g. policy changes) | Medium | Monthly review process; FAQ updates require manager sign-off before prompt is updated |
| Customer query is ambiguous — partially answered incorrectly | Medium | CAN_ANSWER = Partial option flags incomplete answers; ESCALATE = Yes routes ambiguous cases to human |
| Auto-response sent for a query that is actually a complaint | Medium | Escalation logic: any query containing words associated with dissatisfaction (e.g. "still hasn't arrived", "damaged", "wrong item") flagged for human review |
| 24/7 auto-responses sent without oversight during out-of-hours | Low | Daily audit of all auto-responses sent; escalations queue monitored next business day |

**Overall risk rating: LOW** — strictly constrained to documented FAQ answers; all out-of-scope queries escalated to humans. Monthly knowledge base review maintains accuracy.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 10 April 2025
**Prompt:** `Answer this customer query about our retail store: [CUSTOMER_QUERY]. Be helpful and professional.`
**Output:** In one test, the model answered a return policy question by stating "most retailers allow 60-day returns" — drawing on general knowledge rather than the actual 30-day policy. In another, it provided a tracking URL for a different carrier entirely.
**Observed effect:** Incorrect information sent to customers. One test response contradicted the actual return policy, creating a potential obligation the business did not intend to make.
**Lesson learned:** For customer-facing auto-responses, the model must be constrained to a documented knowledge base. Open-ended "be helpful" instructions allow the model to draw on general knowledge that may contradict actual policy.

---

### v1.1 — Grounding constraint + FAQ knowledge base + ESCALATE field ✅ Current
**Date:** 13 April 2025
**Change:** Added explicit FAQ knowledge base in prompt; added grounding constraint ("use ONLY these answers"); added CAN_ANSWER / ESCALATE / ESCALATION_REASON structured output; added 80-word response limit; removed open-ended instruction
**Output:** In 20 test cases, zero instances of answers drawn from outside the FAQ knowledge base. ESCALATE correctly triggered in all 7 out-of-scope queries. CAN_ANSWER = Partial correctly used in 3 ambiguous cases.
**Observed effect:** Agents reported 90% reduction in simple FAQ handling time during testing period. All auto-sent responses matched documented policy exactly.
**Lesson learned:** The CAN_ANSWER = Partial option is essential — without it, the model attempts to fully answer queries it can only partially address, producing half-correct responses. Naming this explicitly gives the model a safe middle option between full answer and full escalation.

---

## 📊 A/B Test Results

**Test date:** 14 April 2025 | **Sample:** 20 customer queries (mix of FAQ and non-FAQ) | **Evaluator:** Customer service manager

| Criteria | v1.0 | v1.1 |
|----------|------|------|
| Answer matches documented policy | 40% | 100% |
| Out-of-scope queries correctly escalated | N/A | 100% |
| Zero answers from general knowledge | 10% | 100% |
| Agent FAQ handling time reduction | 0% | ~90% |

---

## 🔗 Related Prompts

- **If ESCALATE = Yes:** P01 — Complaint triage and classification
- **For complaint follow-up:** P02 — Customer response draft
- **Library index:** README.md
