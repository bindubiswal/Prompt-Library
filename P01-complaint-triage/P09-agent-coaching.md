# P09 · Agent Coaching Feedback Note

**Section:** 04 — Team Performance
**Workflow step:** Step 1 of 1
**Current version:** v1.1
**Status:** ✅ Tested
**Last updated:** April 2025

---

## 📌 Prompt Text (v1.1 — current)

```
You are a customer service team leader at a mid-size Australian retail chain.

Using ONLY the performance data provided below, draft a coaching feedback note for a
customer service agent. This note will be shared with the agent directly.

Do not infer character traits, motivations, or personal qualities not evidenced in the data.
Do not make assumptions about why performance gaps occurred.
This is a coaching note — it must be constructive, specific, and forward-looking.

Agent performance data (this week):
- Agent name: [AGENT_NAME]
- Tickets resolved: [TICKETS_RESOLVED]
- Average resolution time: [AVG_RESOLUTION_TIME]
- Team average resolution time: [TEAM_AVG]
- Customer satisfaction score (CSAT): [CSAT_SCORE] out of 5
- Team average CSAT: [TEAM_CSAT]
- Escalation rate (complaints passed to manager): [ESCALATION_RATE]
- Team average escalation rate: [TEAM_ESCALATION_RATE]
- Compliance rate (response sent within SLA): [COMPLIANCE_RATE]
- Notable positive feedback received: [POSITIVE_FEEDBACK]
- Notable area of concern from QA review: [QA_CONCERN]

Your coaching note must include:

1. OPENING: One positive observation grounded in the data above.
2. PERFORMANCE SUMMARY: 2–3 sentences summarising this week's metrics vs team average. State numbers.
3. AREA FOR DEVELOPMENT: Identify ONE specific area where performance is below team average. Be specific — reference the actual metric.
4. SUGGESTED ACTION: ONE concrete, actionable suggestion the agent can try next week (based only on the data — do not invent causes or remedies not supported by the data).
5. CLOSING: A brief, encouraging close that reinforces team support.

After drafting, review your note and confirm:
- Does it contain any inference about the agent's personality or motivation? If yes, remove it.
- Is every claim backed by a specific number from the data? If not, revise.

Tone: Supportive, direct, professional. This is not a performance review — it is a coaching conversation starter.
Maximum length: 200 words.
```

**Placeholders to fill:**

| Placeholder | Source | Example |
|-------------|--------|---------|
| `[AGENT_NAME]` | CRM performance report | "Marcus" |
| `[TICKETS_RESOLVED]` | CRM export | "47" |
| `[AVG_RESOLUTION_TIME]` | CRM export | "31 hours" |
| `[TEAM_AVG]` | CRM export | "26 hours" |
| `[CSAT_SCORE]` | CRM export | "3.8" |
| `[TEAM_CSAT]` | CRM export | "4.3" |
| `[ESCALATION_RATE]` | CRM export | "22%" |
| `[TEAM_ESCALATION_RATE]` | CRM export | "12%" |
| `[COMPLIANCE_RATE]` | CRM export | "91%" |
| `[POSITIVE_FEEDBACK]` | QA review notes | "Customer praised Marcus's patience on a complex refund case" |
| `[QA_CONCERN]` | QA review notes | "Two responses sent without confirming order details first" |

---

## 🏢 Intended Workflow or Task

- **Trigger:** Weekly CRM performance report generated (Monday); team leader reviews agent metrics
- **Actor:** Team leader reads draft coaching note, personalises if needed, then shares with agent in 1:1 meeting or via message
- **Timing:** Generated after weekly P08 trend report; used in Monday or Tuesday team check-ins
- **Next step:** Agent receives note; team leader follows up on suggested action the following week

```
P08 (Weekly trend report) complete
        │
        ▼
Team leader reviews individual agent metrics from CRM
        │
        ▼
[P09 RUNS] for each agent whose metrics fall below team average
        │
        ▼
Team leader reviews draft (target: <5 min) → Shares with agent in 1:1
        │
        └─→ Follow-up check next Monday
```

---

## ❗ Problem Being Solved

Team leaders currently spend **30–45 minutes per agent** preparing individual performance feedback notes — when they prepare them at all. In practice, informal feedback is given verbally and inconsistently, with no written record. This means performance coaching is reactive rather than structured, and agents receive different quality and frequency of feedback depending on their team leader.

For a team of 10 agents, structured weekly coaching notes represent **5–7 hours of team leader time** — an investment that is frequently deprioritised when complaint volumes are high.

**Pain points addressed:**
- 30–45 min per agent to prepare feedback (5–7 hrs/week total for team of 10)
- Inconsistent feedback quality across team leaders
- No written record of coaching conversations for HR purposes
- Feedback often given verbally without specific data references — hard for agents to act on

---

## ⚡ Automation Potential

**Level: Medium**

| Dimension | Assessment |
|-----------|------------|
| Repetitiveness | High — same structure needed for every agent every week |
| Data availability | All inputs available in weekly CRM performance report |
| Human judgment needed | High — team leader must review carefully; feedback affects employee relationships and wellbeing |
| Integration possibility | Could integrate with CRM performance module to auto-populate agent data |
| Estimated time saving | ~70% — from 30–45 min per agent to 5–10 min review and personalisation |

**Human-in-the-loop role:** Team leader reads every coaching note before sharing with an agent. Team leader may personalise tone, add context not captured in metrics, or adjust wording for the specific agent relationship. No coaching note is shared with an agent without team leader review and approval.

---

## ⚠️ Risks and Limitations

| Risk | Level | Mitigation |
|------|-------|------------|
| Model infers personality traits or motivations from performance data (e.g. "Marcus seems disengaged") | High | Explicit constraint: "Do not infer character traits or motivations not evidenced in the data"; self-critique step checks for this before output finalised |
| Coaching note used as substitute for real conversation | High | Note explicitly framed as "coaching conversation starter" in prompt; team leader training reinforces that note is a tool, not a replacement for dialogue |
| Negative framing damages agent morale or psychological safety | High | Tone constraint: "Supportive, direct, professional"; mandatory positive opening; forward-looking suggested action only |
| Performance data contains errors (CRM misattribution) | Medium | Team leader verifies metrics before running prompt; CRM data validation process in place |
| Privacy — agent performance data processed through AI | Medium | Enterprise API deployment; no PII logging; performance data anonymised where possible |

**Overall risk rating: HIGH** — human review before sharing with agents is mandatory. This prompt operates in a sensitive employment context where errors carry significant wellbeing and legal risk.

---

## 🔄 Version History

### v1.0 — Initial draft
**Date:** 9 April 2025
**Prompt:** `Write a coaching note for [AGENT_NAME] based on this week's performance data: [PERFORMANCE_DATA]. Be constructive.`
**Output:** Note included: "It seems like Marcus may be struggling to prioritise his workload" (personality inference not in data) and "Perhaps he needs more motivation" (assumption about cause not supported by metrics). One note described an agent as "inconsistent" — a character judgement, not a data observation.
**Observed effect:** Team leader immediately flagged the inferences as inappropriate and potentially discriminatory. Draft could not be shared with agents without significant rewriting.
**Lesson learned:** Employment-context feedback requires extremely strong constraints against personality inference. A self-critique step is essential to catch inferences before they reach a manager.

---

### v1.1 — Anti-inference constraint + self-critique + structured format ✅ Current
**Date:** 13 April 2025
**Change:** Added explicit constraint against personality/motivation inference; added self-critique step (two verification questions); added mandatory positive opening; restructured 5-element format; removed open-ended "be constructive" instruction in favour of specific output requirements; 200-word limit added
**Output:** In 12 test cases, zero personality inferences detected. All metric references backed by specific numbers. Self-critique step caught two instances of implicit judgement language and removed them.
**Observed effect:** Team leader review time reduced from 30–45 min to 5–10 min. Team leader described drafts as "a solid starting point that I can personalise without worrying about what to remove."
**Lesson learned:** The self-critique step is the most important element of this prompt — without it, the model consistently slips into characterising agents rather than describing data. In HR-adjacent contexts, this step is non-negotiable.

---

## 📊 A/B Test Results

**Test date:** 14 April 2025 | **Sample:** 12 agent performance scenarios | **Evaluator:** Team leader + HR representative

| Criteria | v1.0 | v1.1 |
|----------|------|------|
| Zero personality/motivation inferences | 0% | 100% |
| All claims backed by specific metrics | 25% | 100% |
| Tone rated "appropriate for sharing with agent" | 17% | 92% |
| Team leader preparation time | 30–45 min | 5–10 min |

---

## 🔗 Related Prompts

- **Data source:** P08 — Weekly complaint trend report (performance metrics)
- **Context:** P02 — Customer response draft (QA review of agent responses informs coaching)
- **Library index:** README.md
