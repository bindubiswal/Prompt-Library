# P02 · Customer Response Draft

**Section:** 01 — Complaint Handling
**Workflow step:** Step 2 of 3
**Current version:** v1.1
**Status:** ✅ Tested
**Last updated:** April 2025

---

## 📌 Prompt Text (v1.1 — current)

```
You are a customer service representative for [STORE_NAME], a mid-size Australian retail chain.

Draft a professional, empathetic response to the following customer complaint.

Complaint category: [CATEGORY]
Complaint text: [COMPLAINT_TEXT]

Your response must include these sections in this order:
1. Acknowledgement — Recognise the customer's frustration without admitting liability
2. Apology — A genuine but measured apology for their experience
3. Action — What the business will do to resolve this (use only the options below)
4. Timeline — When the customer can expect resolution or follow-up
5. Closing — A warm, professional sign-off

Approved resolution options:
- Refund complaints: "We will process a full review of your order and contact you within 2 business days."
- Delivery complaints: "We are investigating your shipment and will provide an update within 24 hours."
- Product quality complaints: "We will arrange a replacement or store credit upon return of the item."
- Staff conduct complaints: "We are taking your feedback seriously and will review this matter internally."
- Website / App complaints: "We have logged this issue with our technical team for investigation."
- Other: "A member of our senior customer service team will contact you within 1 business day."

Tone: Professional, warm, empathetic. Do not be defensive. Do not offer discounts or compensation not listed above.
Maximum length: 150 words.
Do not include: staff names, order numbers, pricing details, or any information not provided to you.
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[STORE_NAME]` | Business configuration | "RetailCo" |
| `[CATEGORY]` | Output from P01 (complaint triage) | "Delivery" |
| `[COMPLAINT_TEXT]` | Original complaint text | "My order hasn't arrived after 3 weeks..." |

---

## 🏢 Intended Workflow or Task

- **Trigger:** P01 completes triage and routes ticket to correct team queue
- **Actor:** Assigned team agent reviews the draft and sends (or edits before sending)
- **Timing:** Draft generated automatically when agent opens the ticket; human reviews before sending
- **Next step:** Agent sends response to customer; if High urgency, P03 (Escalation summary) also runs in parallel

```
P01 output (category + urgency)
        │
        ▼
[P02 RUNS] → Response draft generated
        │
        ▼
Agent reviews draft (target: under 2 minutes review time)
        │
        ├─→ Approved → Sent to customer
        └─→ Edited → Sent to customer
                │
                └─→ If High urgency → P03 also triggered
```

---

## ❗ Problem Being Solved

Drafting individual customer complaint responses currently takes customer service agents **15–20 minutes per complaint** when done from scratch. At 200 complaints per day across a team of 10 agents, this represents significant labour cost and inconsistent tone and quality across responses.

Inconsistent responses also create legal risk — agents may inadvertently admit liability, offer unauthorised compensation, or include information that contradicts the company's formal position on a complaint.

**Pain points addressed:**
- High time cost of drafting responses from scratch (15–20 min per complaint)
- Tone and quality inconsistency across a large team
- Legal risk from unauthorised admissions or compensation offers
- No structured format ensuring all required response elements are present

---

## ⚡ Automation Potential

**Level: High**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | High — same response structure required for every complaint |
| Data availability | Inputs (category from P01 + original complaint text) always available |
| Human judgment needed | Medium — agent must review for tone, accuracy, and context before sending |
| Integration possibility | Could integrate with CRM to auto-populate draft in agent's reply window |
| Estimated time saving | ~80% — from 15–20 min drafting to 2–3 min review and send |

**Human-in-the-loop role:** Agent reads and approves every draft before it is sent. No response goes to a customer without human sign-off. Agents may edit tone, add specific order details, or adjust wording where context requires it.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model generates response that admits liability | High | Explicit prompt instruction: "Acknowledge without admitting liability"; approved resolution options listed; agent reviews before send |
| Model offers compensation not authorised by business | High | Constrained resolution options listed in prompt; "Do not offer discounts or compensation not listed above" instruction |
| Response tone too cold or too casual for the situation | Medium | RACE role framing establishes professional-empathetic tone; agent review catches tone issues |
| Model includes information not provided (hallucination) | Medium | "Do not include any information not provided to you" constraint; agent reviews for fabricated details |
| Word limit exceeded — response too long for email format | Low | Maximum 150 word limit enforced in prompt |

**Overall risk rating: MEDIUM** — human review before send is non-negotiable. Suitable for draft automation with mandatory agent approval gate.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 2 April 2025
**Prompt:** `Write a customer service response to this complaint: [COMPLAINT_TEXT]. Be professional and empathetic.`
**Output:** Generic response with inconsistent structure. In one test, the model offered a full refund without being asked. In another, it used a defensive tone ("we are not responsible for...").
**Observed effect:** Responses required near-complete rewrites. Agents said draft was "more work than writing from scratch." Liability risk identified.
**Lesson learned:** Must constrain resolution options explicitly. Must define tone with specific guidance. Must provide structured output format so all required elements are present.

---

### v1.1 — RACE framework + constrained resolution options ✅ Current
**Date:** 8 April 2025
**Change:** Added role framing (RACE), structured output sections (5 required elements), approved resolution options list, explicit constraints on compensation and PII, 150-word limit
**Output:** Consistent, professional responses with correct structure in all 20 test cases. No unauthorised compensation offers. No liability admissions.
**Observed effect:** Agent review time reduced from 15–20 min to 2–3 min. Agents reported drafts were "send-ready with minor tweaks."
**Lesson learned:** Providing approved resolution options eliminates hallucinated compensation. Structured output sections ensure no required element is missed. Role framing stabilises tone across different complaint types.

---

## 📊 A/B Test Results

**Test date:** 9 April 2025 | **Sample:** 20 complaint responses | **Evaluator:** Customer service manager

| Criteria | v1.0 | v1.1 |
|----------|------|------|
| Correct structure (all 5 elements present) | 30% | 100% |
| No unauthorised compensation offered | 60% | 100% |
| Professional tone rating (1–5) | 2.8/5 | 4.6/5 |
| Agent review time | 15–20 min | 2–3 min |

---

## 🔗 Related Prompts

- **Previous in chain:** P01 — Complaint triage and classification
- **Next in chain:** P03 — Escalation summary (if High urgency)
- **Post-resolution:** P06 — Follow-up satisfaction check email
- **Library index:** README.md
