# P06 · Follow-Up Satisfaction Check Email

**Section:** 03 - Post-Resolution
**Workflow step:** Step 1 of 1
**Current version:** v1.1
**Status:** Tested
**Last updated:** April 2025

---

## 📌 Prompt Text (v1.1 - current)

```
You are a customer experience specialist at [STORE_NAME], a mid-size Australian retail chain.

A customer complaint has been resolved. Draft a short follow-up email to check on the customer's
satisfaction with how their complaint was handled.

Resolution details:
- Customer name: [CUSTOMER_NAME]
- Complaint category: [CATEGORY]
- Resolution provided: [RESOLUTION_PROVIDED]
- Resolution date: [RESOLUTION_DATE]
- Handling agent: [AGENT_NAME]

Your email must include:
1. A warm, genuine opening that references their specific issue (not generic)
2. Confirmation that their complaint has been resolved
3. A single direct question asking if they are satisfied with the outcome
4. An invitation to contact us if they are not fully satisfied
5. A professional sign-off from the customer service team

Tone: Warm, human, brief. This is not a marketing email. Do not include promotions, discounts,
survey links, or any call-to-action beyond the satisfaction question.
Do not use the customer's full name more than once.
Maximum length: 100 words (email body only, excluding subject line).

Also provide:
SUBJECT LINE: [Write a subject line that references their specific issue - not generic]
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[STORE_NAME]` | Business config | "RetailCo" |
| `[CUSTOMER_NAME]` | CRM record | "Priya" |
| `[CATEGORY]` | P01 output | "Delivery" |
| `[RESOLUTION_PROVIDED]` | CRM resolution log | "Full refund processed on 10 April 2025" |
| `[RESOLUTION_DATE]` | CRM record | "10 April 2025" |
| `[AGENT_NAME]` | CRM ticket | "James" |

---

## 🏢 Intended Workflow or Task

- **Trigger:** CRM marks complaint ticket as "Resolved"; automated pipeline queues this prompt 48 hours after resolution
- **Actor:** System sends the email automatically after a 30-second agent spot-check
- **Timing:** 48 hours after complaint resolution - enough time for resolution to take effect, soon enough to be relevant
- **Next step:** If customer replies negatively, ticket is reopened and routed to senior agent; if no reply, ticket is closed after 5 business days

```
Complaint resolved → CRM status = "Resolved"
        │
        ▼
48-hour delay (automated timer)
        │
        ▼
[P06 RUNS] → Follow-up email drafted
        │
        ▼
Agent spot-check (30 seconds) → Email sent automatically
        │
        ├─→ Customer replies satisfied → Ticket closed
        └─→ Customer replies unsatisfied → Ticket reopened → Senior agent assigned
```

---

## ❗ Problem Being Solved

The retail chain currently sends no structured follow-up after complaint resolution. Customer satisfaction with complaint handling is therefore unmeasured, and customers with unresolved dissatisfaction have no low-friction pathway to re-engage - leading to **negative online reviews and social media complaints** that could have been intercepted.

Research in retail customer experience indicates that customers who receive a follow-up after complaint resolution have significantly higher retention rates than those who do not. The current gap means the business is losing recoverable customers to competitor brands.

**Pain points addressed:**
- No post-resolution follow-up currently exists (zero touchpoint after complaint closure)
- Dissatisfied customers have no low-friction re-engagement option (leading to public reviews)
- Customer satisfaction with complaint handling is unmeasured (no data for team performance review)
- Agents spend time drafting individual follow-up emails when they remember to send them

---

## ⚡ Automation Potential

**Level: Very High**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | Very high - same follow-up process for every resolved complaint |
| Data availability | All inputs (name, category, resolution) available in CRM at resolution |
| Human judgment needed | Very low - 30-second spot-check only; tone is consistent and low-risk |
| Integration possibility | Fully integratable with CRM automated pipeline; no manual trigger needed |
| Estimated time saving | ~90% - from occasional manual drafting (5–10 min) to automated 30-second check |

**Human-in-the-loop role:** Agent performs a 30-second visual check before email sends. This exists to catch any obvious errors (wrong customer name, resolution detail mismatch) rather than to evaluate content - content is consistently appropriate at this risk level.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Follow-up sent for a complaint that was not fully resolved | Medium | CRM automation triggers on "Resolved" status only; agent who resolves sets this status manually |
| Customer name placeholder not filled (email sends with "[CUSTOMER_NAME]") | Medium | Validation step checks all placeholders populated before prompt runs; blank fields halt the pipeline |
| Follow-up perceived as marketing rather than genuine care | Low | Prompt explicitly prohibits promotions, discounts, and survey links; 100-word limit keeps it brief and human |
| Customer replies to follow-up with new complaint - goes unnoticed | Low | Reply monitoring: any reply to follow-up email automatically reopens ticket in CRM |

**Overall risk rating: LOW** - suitable for near-full automation with 30-second agent spot-check. Risk profile is the lowest in the library due to low stakes and non-binding content.

---

## 🔄 Version History

### v1.0 - Initial draft
**Date:** 6 April 2025
**Prompt:** `Write a follow-up email to [CUSTOMER_NAME] whose complaint about [CATEGORY] was resolved on [RESOLUTION_DATE].`
**Output:** Generic email with phrases like "We hope everything is going well!" and "As a valued customer..." One test output included a 10% discount offer not requested or authorised.
**Observed effect:** Emails felt impersonal and marketing-like. The discount offer created a significant governance issue - agents cannot offer compensation via automated email.
**Lesson learned:** Must explicitly prohibit marketing language and unauthorised offers. Must require the email to reference the specific complaint issue to feel genuine rather than templated.

---

### v1.1 - Tone constraints + no promotions + specific reference to issue ✅ Current
**Date:** 10 April 2025
**Change:** Added explicit prohibition on promotions, discounts, and survey links; added requirement to reference specific complaint issue; added 100-word limit; added subject line requirement; restructured 5-element output format
**Output:** Warm, specific, brief emails in all 15 test cases. No marketing language. No unauthorised offers. Subject lines correctly referenced specific issues (e.g. "Following up on your recent delivery concern").
**Observed effect:** Test emails rated by customer experience manager as "exactly the right tone." Estimated time saving of 90% vs occasional manual drafting.
**Lesson learned:** The 100-word limit is essential - it forces brevity that makes the email feel human, not corporate. Explicitly prohibiting marketing elements is more effective than saying "keep it genuine."

---

## 📊 A/B Test Results

**Test date:** 11 April 2025 | **Sample:** 15 resolved complaint scenarios | **Evaluator:** Customer experience manager

| Criteria | v1.0 | v1.1 |
|----------|------|------|
| No unauthorised offers or promotions | 70% | 100% |
| Specific reference to customer's complaint | 20% | 100% |
| Tone rated "appropriate" by evaluator | 40% | 100% |
| Within 100-word limit | N/A | 100% |

---

## 🔗 Related Prompts

- **Triggered after:** P02 - Customer response draft (complaint resolved)
- **If negative reply received:** Reopens ticket → P02 re-runs for senior agent
- **Feeds into:** P07 - Negative review response (if customer posts negative review instead of replying)
- **Library index:** README.md
