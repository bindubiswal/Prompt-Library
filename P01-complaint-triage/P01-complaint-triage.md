# P01 · Complaint Triage and Classification

**Section:** 01 — Complaint Handling
**Workflow step:** Step 1 of 3
**Current version:** v1.2
**Status:** ✅ Tested
**Last updated:** April 2025

---

## 📌 Prompt Text (v1.2 — current)

```
Classify the following customer complaint.

Choose EXACTLY ONE category from this list:
- Refund
- Delivery
- Product quality
- Staff conduct
- Website / App
- Other

Rate urgency as one of: Low / Medium / High
Use "High" if the complaint involves: personal injury, discrimination, significant financial loss,
or threats of legal action.

Complaint text: [COMPLAINT_TEXT]

Respond in JSON only. Do not add any explanation, greeting, or text outside the JSON.

{
  "category": "[chosen category]",
  "urgency": "[Low / Medium / High]",
  "one_line_reason": "[One sentence — why this category and urgency]"
}
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[COMPLAINT_TEXT]` | Raw complaint from email / web form | "I ordered online 3 weeks ago and my parcel still hasn't arrived. Nobody is replying to my emails." |

---

## 🏢 Intended Workflow or Task

- **Trigger:** New complaint received in customer service inbox or web form
- **Integration:** JSON output is parsed by CRM system to route ticket to correct team queue automatically
- **Timing:** Runs within seconds of complaint receipt (automated pipeline)
- **Next step:** P02 (Customer response draft) is queued for the assigned team agent

```
Complaint arrives → [P01 RUNS] → JSON parsed by CRM
                              → Ticket routed to: Refund team / Delivery team / etc.
                              → If urgency = High: manager alert triggered within 1 hour
                              → P02 queued for assigned agent
```

---

## ❗ Problem Being Solved

Manual triage by a team leader takes **3–5 minutes per ticket**.

At 200 complaints/day: **10–17 hours of triage time per day eliminated.**

Beyond time, manual triage introduces inconsistency — different leaders route similar complaints differently, causing backlogs in some queues and idle capacity in others. There is also no structured audit trail of how complaints were classified, creating compliance risk.

**Pain points addressed:**
- Excessive triage labour cost (10–17 hrs/day at team leader pay rate)
- Routing inconsistency causing uneven team workloads
- Delayed escalation of high-risk complaints (legal, injury, discrimination)
- No auditable classification record for compliance purposes

---

## ⚡ Automation Potential

**Level: Very High**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | Extremely high — identical classification task performed on every complaint |
| Data availability | Input is the complaint text itself — always available at point of receipt |
| Human judgment needed | Very low for routine cases; edge cases caught by "Other" category |
| Integration possibility | JSON output integrates directly with CRM routing rules via API |
| Estimated time saving | ~95% — from 3–5 min manual to near-instant automated |
| Scalability | Scales linearly with complaint volume at near-zero marginal cost |

**Human-in-the-loop role:** Weekly audit of classification accuracy by team leader (random sample of 30 tickets). Any "Other" category reviewed daily by senior agent. High-urgency flags reviewed by manager within 1 hour of receipt.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Complaint spans two categories (e.g. delivery AND refund) | Medium | "Other" catch-all category retained; agent reviews all "Other" tickets daily |
| Urgency misclassified — serious complaint marked Low | High | Explicit urgency criteria in prompt ("personal injury, discrimination, legal action"); manager reviews all High flags within 1 hour |
| JSON format breaks if model adds extra text | Medium | "Do not add any explanation outside the JSON" constraint; API-side JSON validation before CRM ingestion |
| Sensitive complaint content processed through external LLM | Medium | Enterprise API deployment with data processing agreement; PII anonymised before processing; no LLM logging of complaint content |
| Bias — complaint tone affecting urgency rating | Medium | Urgency criteria are explicit and criteria-based (injury, legal, financial loss), not tone-based; weekly audit monitors for patterns |

**Overall risk rating: MEDIUM** — suitable for full automation with weekly accuracy audit and daily "Other" review.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 1 April 2025
**Prompt:** `Classify this customer complaint into a category and rate its urgency: [COMPLAINT_TEXT]`
**Output:** Prose response — "This complaint appears to be about a delivery issue and seems moderately urgent."
**Observed effect:** Could not be parsed by CRM system. Required full manual re-entry. No time saving achieved.
**Lesson learned:** For integration with systems, output format must be machine-readable. Free-form prose defeats the automation purpose entirely.

---

### v1.1 — JSON output enforced
**Date:** 5 April 2025
**Change:** Added JSON output requirement; categories still undefined — model chose its own
**Output:** JSON returned but categories varied: "shipping", "logistics", "late delivery" all used for same complaint type
**Observed effect:** CRM routing broke — categories did not match routing rules. Tickets fell into unrouted queue.
**Lesson learned:** Must provide a fixed, constrained category list. The model cannot define its own taxonomy if output needs to match CRM routing rules.

---

### v1.2 — Constrained category list + urgency criteria ✅ Current
**Date:** 10 April 2025
**Change:** Added explicit fixed category list; added specific urgency criteria; added "Other" catch-all; added one_line_reason field for audit trail
**Output:** Consistent JSON with categories matching CRM routing rules in 47/50 test cases
**Observed effect:** Misclassification rate: 6%. CRM parse rate: 100%. Weekly audit introduced to catch remaining errors.
**Lesson learned:** Constrained output categories + explicit criteria = reliable enough for automation. "Other" is essential — never force the model into a category that doesn't fit.

---

## 📊 A/B Test Results

**Test date:** 12 April 2025 | **Sample:** 50 real complaints (anonymised) | **Evaluator:** CX team lead

| Criteria | v1.0 | v1.2 |
|----------|------|------|
| Machine-parseable output | 0% | 100% |
| Category accuracy | N/A | 94% |
| Urgency accuracy | N/A | 92% |
| Integration with CRM | Failed | Successful |

---

## 🔗 Related Prompts

- **Next in chain:** P02 — Customer response draft
- **If High urgency:** P03 — Escalation summary for management
- **Library index:** README.md
