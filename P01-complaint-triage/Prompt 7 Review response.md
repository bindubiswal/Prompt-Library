# P07 · Negative Review Response Draft

**Section:** 03 - Reputation Management
**Workflow step:** Step 1 of 1
**Current version:** v1.1
**Status:** ✅ Tested
**Last updated:** April 2025

---

## 📌 Prompt Text (v1.1 - current)

```
You are a customer experience manager at [STORE_NAME], a mid-size Australian retail chain.

A customer has left a negative review on [PLATFORM] (e.g. Google, ProductReview.com.au).
Draft a professional public response to this review.

Review details:
- Platform: [PLATFORM]
- Star rating: [STAR_RATING] out of 5
- Review text: [REVIEW_TEXT]
- Store/channel mentioned: [STORE_OR_CHANNEL]

Your response must:
1. Thank the customer for their feedback (brief - 1 sentence)
2. Acknowledge the specific issue they raised without being defensive
3. Apologise for the experience
4. State ONE concrete action the business is taking or has taken
5. Invite the customer to contact us directly to resolve (include: [CONTACT_EMAIL])

Tone rules:
- Professional and empathetic - never defensive, sarcastic, or dismissive
- Write as if other customers will read this (they will)
- Do not name specific staff members
- Do not offer compensation publicly
- Do not ask the customer to change or remove their review
- Do not deny or contradict the customer's experience

Maximum length: 120 words.
Do not include: discounts, promotional language, or any claim that cannot be verified from the review text.
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[STORE_NAME]` | Business config | "RetailCo" |
| `[PLATFORM]` | Review monitoring tool | "Google Reviews" |
| `[STAR_RATING]` | Review data | "2" |
| `[REVIEW_TEXT]` | Review monitoring tool | "Staff were rude and I waited 20 minutes to be served..." |
| `[STORE_OR_CHANNEL]` | Review text / metadata | "Doncaster store" |
| `[CONTACT_EMAIL]` | Business config | "customercare@retailco.com.au" |

---

## 🏢 Intended Workflow or Task

- **Trigger:** Review monitoring tool detects a new 1–3 star review on Google, ProductReview.com.au, or Facebook
- **Actor:** Customer experience manager reviews draft and posts publicly (or edits before posting)
- **Timing:** Draft generated within 2 hours of review detection; target: public response posted within 24 hours
- **Next step:** If customer responds to public reply, a direct follow-up is initiated via email (P02 style)

```
Review monitoring tool detects 1–3 star review
        │
        ▼
Alert sent to customer experience manager
        │
        ▼
[P07 RUNS] → Public response draft generated
        │
        ▼
Manager reviews and edits (target: <10 min) → Posts publicly
        │
        └─→ Customer engages → Direct follow-up initiated
```

---

## ❗ Problem Being Solved

The retail chain currently has no consistent process for responding to negative online reviews. Response rates are low - only **35% of negative Google reviews** receive a response - and when responses are written by individual store managers, tone varies widely, with some responses being defensive or dismissive, which compounds the reputational damage.

In Australian retail, online review scores directly influence purchase decisions. A 2024 industry report found that **94% of Australian consumers** read online reviews before visiting a store. Unanswered negative reviews signal to prospective customers that the business does not care about complaints.

**Pain points addressed:**
- Only 35% of negative reviews currently receive a response (65% go unanswered)
- Inconsistent and sometimes damaging response tone from individual store managers
- Slow response time (often 3–5 days when managed manually)
- No structured process ensuring all required response elements are present

---

## ⚡ Automation Potential

**Level: High**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | High - same response structure needed for every negative review |
| Data availability | Review text always available via monitoring tool |
| Human judgment needed | Medium - manager must review before posting publicly; public responses carry reputational risk |
| Integration possibility | Could integrate with Google Business Profile and ProductReview API for direct posting |
| Estimated time saving | ~75% - from 20–30 min per response to 5–10 min review and post |

**Human-in-the-loop role:** Customer experience manager reads and approves every response before it is posted publicly. No response is posted automatically. Manager may edit tone, add context, or redraft entirely if the situation is particularly sensitive.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Response inadvertently contradicts or dismisses the reviewer | High | Explicit tone rules: "Do not deny or contradict the customer's experience"; manager reviews before posting |
| Response offers public compensation creating precedent | High | "Do not offer compensation publicly" instruction; constrained action statement |
| Response references unverifiable claims | Medium | "Do not include any claim that cannot be verified from the review text" constraint |
| Highly sensitive reviews (discrimination, safety) responded to without legal review | High | Flagging rule: reviews containing discrimination, injury, or legal language escalated to manager + legal team before any response is drafted |
| Response reads as templated - reduces authenticity | Medium | Prompt requires specific acknowledgement of the customer's stated issue; not a generic apology template |

**Overall risk rating: MEDIUM** - human review before posting is non-negotiable given public visibility. Sensitive reviews must be escalated before this prompt runs.

---

## 🔄 Version History

### v1.0 - Initial draft
**Date:** 7 April 2025
**Prompt:** `Write a professional response to this negative customer review: [REVIEW_TEXT]. Be apologetic and offer to help.`
**Output:** In one test, the model offered a "full refund or replacement" publicly on Google Reviews - an unauthorised commitment made in a public forum. In another, it addressed the reviewer as "Dear Valued Customer" - generic and impersonal.
**Observed effect:** Draft required complete rewrite in most cases. The unauthorised compensation offer was flagged as a serious governance failure.
**Lesson learned:** Public responses require strict constraints on compensation offers. Generic openers must be prohibited. The response must reference the specific issue raised - not a generic acknowledgement.

---

### v1.1 - Tone rules + compensation prohibition + specific issue reference ✅ Current
**Date:** 11 April 2025
**Change:** Added explicit tone rules (not defensive, not dismissive, don't ask to remove review, don't offer compensation publicly); added requirement to acknowledge specific issue; added contact email invitation; structured 5-element output format; 120-word limit
**Output:** Professional, specific, appropriately empathetic responses in all 15 test cases. No unauthorised offers. Manager review time reduced to under 10 minutes in 13/15 cases.
**Observed effect:** Customer experience manager described drafts as "the right starting point every time." Response rate to negative reviews increased from 35% to a projected 90%+ with automated drafting.
**Lesson learned:** The instruction "write as if other customers will read this" is highly effective - it shifts the model's framing from addressing one unhappy customer to managing brand perception, which is the actual business goal.

---

## 📊 A/B Test Results

**Test date:** 12 April 2025 | **Sample:** 15 negative review scenarios (1–3 stars) | **Evaluator:** Customer experience manager

| Criteria | v1.0 | v1.1 |
|----------|------|------|
| No unauthorised compensation offered | 30% | 100% |
| Specific reference to customer's stated issue | 20% | 100% |
| Tone rated "appropriate for public posting" | 25% | 93% |
| Manager edit time | 20–30 min | <10 min |

---

## 🔗 Related Prompts

- **Triggered by:** Negative review detection (external monitoring tool)
- **Related to:** P06 - Follow-up satisfaction check (if customer left review instead of replying to follow-up)
- **If high-risk review (legal/safety):** P03 - Escalation summary triggered before this prompt runs
- **Library index:** README.md
