# P05 · Order Issue Investigation Summary

**Section:** 02 — Resolution Management
**Workflow step:** Step 1 of 2
**Current version:** v1.1
**Status:** ✅ Tested
**Last updated:** April 2025

---

## 📌 Prompt Text (v1.1 — current)

```
You are a customer service investigator at a mid-size Australian retail chain.

A customer has reported an issue with their order. Using ONLY the information provided below,
prepare a structured investigation summary for the handling agent.

Do not add any conclusions, assumptions, or recommendations not supported by the data provided.

Order and complaint data:
- Order number: [ORDER_NUMBER]
- Order date: [ORDER_DATE]
- Items ordered: [ITEMS_ORDERED]
- Delivery address: [DELIVERY_ADDRESS]
- Shipping carrier: [CARRIER]
- Tracking status: [TRACKING_STATUS]
- Last tracking event: [LAST_TRACKING_EVENT]
- Customer complaint: [COMPLAINT_TEXT]
- Previous contacts on this order: [PREVIOUS_CONTACTS]

Your summary must include these sections:

1. ORDER STATUS: What the current status of the order is based on the data above (2 sentences max).
2. ISSUE IDENTIFIED: What specific problem is evident from the data (1–2 sentences).
3. DISCREPANCY CHECK: Does anything in the tracking/order data contradict the customer's complaint? State Yes or No and explain briefly.
4. RECOMMENDED NEXT STEP: One specific action the agent should take to move this forward (1 sentence).
5. INFORMATION GAPS: List any data that is missing and would be needed for full resolution.

Maximum length: 180 words. Plain text only. No bullet points within sections.
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[ORDER_NUMBER]` | CRM order record | "#ORD-20250389" |
| `[ORDER_DATE]` | CRM record | "18 March 2025" |
| `[ITEMS_ORDERED]` | CRM order record | "1x Merino wool jumper, navy, size M" |
| `[DELIVERY_ADDRESS]` | CRM record | "14 Smith St, Richmond VIC 3121" |
| `[CARRIER]` | CRM shipping record | "Australia Post" |
| `[TRACKING_STATUS]` | Carrier API | "In transit — delayed" |
| `[LAST_TRACKING_EVENT]` | Carrier API | "Departed Melbourne facility, 22 March 2025" |
| `[COMPLAINT_TEXT]` | Customer complaint | "My order hasn't arrived and no one is helping me." |
| `[PREVIOUS_CONTACTS]` | CRM contact history | "None" |

---

## 🏢 Intended Workflow or Task

- **Trigger:** Agent opens a delivery or order-related complaint ticket
- **Actor:** Agent uses summary to understand the situation before contacting customer or carrier
- **Timing:** Runs immediately when agent opens ticket; replaces manual data gathering across CRM and carrier portal
- **Next step:** Agent uses summary to draft response (P02) or make refund decision (P04)

```
Delivery/order complaint received → Routed to delivery team (via P01)
        │
        ▼
Agent opens ticket → [P05 RUNS]
        │
        ▼
Investigation summary generated (replaces 10–15 min manual data gathering)
        │
        ├─→ Issue clear → Agent drafts response via P02
        └─→ Refund/return needed → P04 triggered
```

---

## ❗ Problem Being Solved

When a delivery complaint is received, agents currently spend **10–15 minutes manually cross-referencing** the CRM order record, the shipping carrier portal, and the customer's contact history before they can even begin drafting a response. This fragmented process leads to incomplete information, inconsistent investigation depth, and slow resolution times.

In a team handling 200+ complaints per day, this represents **30–50 hours of investigation time per day** spent on data gathering rather than resolution. Agents with less experience often miss key discrepancies (e.g. tracking showing "delivered" while customer reports non-receipt) that are critical to resolution.

**Pain points addressed:**
- 10–15 min manual data gathering per complaint (30–50 hrs/day across team)
- Inconsistent investigation depth between experienced and inexperienced agents
- Missed discrepancies between order data and customer claims
- No structured documentation of what was investigated and what gaps remain

---

## ⚡ Automation Potential

**Level: High**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | High — same data points investigated for every delivery/order complaint |
| Data availability | All inputs available via CRM + carrier API integration |
| Human judgment needed | Medium — agent must interpret summary and decide next action |
| Integration possibility | Could pull order + tracking data automatically via CRM/carrier API, removing need for manual input |
| Estimated time saving | ~75% — from 10–15 min manual investigation to 2–3 min review of structured summary |

**Human-in-the-loop role:** Agent reads and acts on the summary. All decisions about resolution (refund, resend, carrier escalation) are made by the agent. The summary organises information — it does not decide outcomes.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model draws conclusions from incomplete data | High | Grounding constraint: "Do not add conclusions not supported by data provided"; INFORMATION GAPS section surfaces missing data explicitly |
| Tracking data is stale or inaccurate at time of prompt | Medium | Agent instructed to verify tracking status directly with carrier before committing to resolution |
| Agent treats summary as definitive without checking source data | Medium | Summary framed as "investigation starting point" in agent training; DISCREPANCY CHECK section encourages critical review |
| CRM + carrier data integration failure causes incomplete inputs | Low | Validation step checks all placeholders are populated before prompt runs; incomplete inputs trigger manual process |

**Overall risk rating: MEDIUM** — suitable for automation as an investigation starting point. Agent must verify key facts before communicating resolution to customer.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 5 April 2025
**Prompt:** `Summarise the following order issue for a customer service agent: [ORDER_DETAILS] [COMPLAINT_TEXT]`
**Output:** Unstructured paragraph that mixed order facts with assumptions. In one test, model concluded "the parcel is likely lost" based only on a 3-day delay — a conclusion not supported by the tracking data provided.
**Observed effect:** Agents found the unstructured output hard to act on. Fabricated conclusions created risk of incorrect commitments to customers.
**Lesson learned:** Structured output sections are essential for operational use. Grounding constraints must explicitly prohibit assumptions. An INFORMATION GAPS section is needed to surface what the AI doesn't know.

---

### v1.1 — Structured sections + grounding constraint + DISCREPANCY CHECK ✅ Current
**Date:** 10 April 2025
**Change:** Added five structured output sections; added grounding constraint ("do not add conclusions not supported by data"); added DISCREPANCY CHECK section; added INFORMATION GAPS section; added 180-word limit
**Output:** Clean, structured summaries in all 20 test cases. Zero fabricated conclusions. Discrepancy check correctly identified mismatch between tracking and customer claim in 4/5 relevant cases.
**Observed effect:** Agent investigation time reduced from 10–15 min to 2–3 min. Agents reported higher confidence in understanding the issue before contacting customers.
**Lesson learned:** The DISCREPANCY CHECK section is unexpectedly valuable — it surfaces conflicts in the data that agents might miss when rushing through manual investigation. INFORMATION GAPS section prevents agents from assuming they have the full picture.

---

## 📊 A/B Test Results

**Test date:** 11 April 2025 | **Sample:** 20 delivery/order complaint scenarios | **Evaluator:** Senior customer service agent

| Criteria | v1.0 | v1.1 |
|----------|------|------|
| Completeness score (all required elements) | 2/5 | 5/5 |
| Zero fabricated conclusions | 35% | 100% |
| Discrepancy correctly identified | N/A | 80% |
| Agent investigation time | 10–15 min | 2–3 min |

---

## 🔗 Related Prompts

- **Next in chain:** P04 — Refund / return decision recommendation
- **Feeds into:** P02 — Customer response draft
- **Library index:** README.md
