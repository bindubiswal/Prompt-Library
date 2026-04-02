# Prompt Library — Customer Service Workflow Automation

**Assessment 1 | Generative AI for Business — BUS4005**
**Business Field:** Retail — Customer Service
**Organisation Type:** Mid-size retail chain (online + in-store)
**Model tested on:** Claude Sonnet 4
**Total prompts:** 10 (fully documented)

## What this library does

This prompt library automates the end-to-end customer service workflow for a mid-size Australian retail chain handling 200+ complaints per day across online and in-store channels. It covers four operational functions: complaint handling, resolution management, post-resolution follow-up, and team performance. Each prompt is documented with full workflow context, automation potential, risk assessment, and version history demonstrating iterative improvement.

## Library summary — all 10 prompts

| ID | Prompt Name | Workflow Function | Automation Level | Risk Level | Status |
|----|-------------|-------------------|-----------------|------------|--------|
| P01 | Complaint triage & classification | Complaint handling | Very High | Medium | Tested |
| P02 | Customer response draft | Complaint handling | High | Medium | Tested |
| P03 | Escalation summary for management | Complaint handling | Medium | High | Tested |
| P04 | Refund / return decision recommendation | Resolution management | Medium | High | Tested |
| P05 | Order issue investigation summary | Resolution management | High | Medium | Tested |
| P06 | Follow-up satisfaction check email | Post-resolution | Very High | Low | Tested |
| P07 | Negative review response draft | Reputation management | High | Medium | Tested |
| P08 | Weekly complaint trend report | Reporting & insights | Very High | Low | Tested |
| P09 | Agent coaching feedback note | Team performance | Medium | High | Tested |
| P10 | FAQ auto-response for common queries | Self-service deflection | Very High | Low | Tested |

**Automation levels:** Very High / High / Medium / Low
**Risk levels:** High (always needs human review) / Medium (spot-check recommended) / Low (can automate with audit)

## Prompt chaining map

Many prompts are put together to work in sequence. Outputs from one prompt is fed directly into the next, creating an auditable pipeline from complaint receipt to customer issue resolution.

```

CHAIN 1 — Complaint handling
P01 (Complaint triage) → P02 (Customer response draft) → P03 (Escalation summary)

CHAIN 2 — Resolution management
P05 (Order investigation summary) → P04 (Refund decision recommendation)

CHAIN 3 — Post-resolution & reputation
P02 (Response sent) → P06 (Follow-up satisfaction check) → P07 (Review response if negative)

CHAIN 4 — Reporting & performance
P01 data → P08 (Weekly trend report)
P02 data → P09 (Agent coaching note)
```

---

## Prompting strategies used across the library

| Strategy | Prompts Used | Why Chosen |
|----------|-------------|------------|
| RACE framework (Role–Action–Context–Evaluation) | P02, P03, P07, P09 | Ensures consistent, professional tone aligned to retail brand voice |
| Constrained output (JSON, fixed categories, word limits) | P01, P08, P10 | Enables machine-readable outputs for CRM/API integration |
| Grounding constraints ("using only the information provided") | P03, P04, P05 | Prevents hallucination in legal, financial, and factual contexts |
| Prompt decomposition (chaining) | All chains above | Reduces error rate vs single mega-prompts handling the whole workflow |
| Self-critique step | P03, P09 | Improves accuracy in high-stakes outputs (escalation, coaching notes) |
| Structured output sections | P02, P04, P06, P07 | Reduces agent editing time by ensuring no required element is missed |

## Iteration evidence summary

Full version histories are documented in each prompt's folder. The table below summarises key iterations.

| Prompt | Versions | Key Problem Fixed | Measurable Improvement |
|--------|----------|-------------------|----------------------|
| P01 | v1.0 → v1.2 | Added constrained category list + JSON output | CRM parse rate: 0% → 100% |
| P02 | v1.0 → v1.1 | Added RACE role + tone constraint + word limit | Edit time: 18 min → 3 min |
| P03 | v1.0 → v1.1 | Added grounding constraint + self-critique step | Factual accuracy: 70% → 94% |
| P04 | v1.0 → v1.2 | Added policy boundary + human-override note | Manager override rate: 40% → 12% |
| P05 | v1.0 → v1.1 | Added structured output sections + data grounding | Completeness score: 3/5 → 5/5 |
| P06–P10 | v1.0 → v1.1 | Various — see individual prompt folders | Documented per prompt |

---

## Folder structure

```
BUS4005-Prompt-Library/
│
├── README.md                          ← You are here (library index)
│
├── P01-complaint-triage/
│   ├── P01-v1.0.md                   ← First rough draft
│   ├── P01-v1.1.md                   ← After adding JSON + categories
│   └── P01-v1.2.md                   ← Final tested version
│
├── P02-response-draft/
│   ├── P02-v1.0.md
│   └── P02-v1.1.md
│
├── P03-escalation-summary/
│   ├── P03-v1.0.md
│   └── P03-v1.1.md
│
├── P04-refund-decision/
│   ├── P04-v1.0.md
│   ├── P04-v1.1.md
│   └── P04-v1.2.md
│
├── P05-order-investigation/
│   ├── P05-v1.0.md
│   └── P05-v1.1.md
│
├── P06-followup-satisfaction/
│   ├── P06-v1.0.md
│   └── P06-v1.1.md
│
├── P07-review-response/
│   ├── P07-v1.0.md
│   └── P07-v1.1.md
│
├── P08-weekly-trend-report/
│   ├── P08-v1.0.md
│   └── P08-v1.1.md
│
├── P09-agent-coaching/
│   ├── P09-v1.0.md
│   └── P09-v1.1.md
│
└── P10-faq-auto-response/
    ├── P10-v1.0.md
    └── P10-v1.1.md
```

---

## References

- Anthropic (2025). *Prompt Engineering Overview.* https://docs.claude.ai
- Kartaca (2026). *Standardizing Enterprise Intelligence with a Corporate Prompt Library.*
- MIT Sloan (2025). *Prompt Engineering is So 2024 — Try These Prompt Templates Instead.*
- Microsoft (2025). *Get Started with Prompt Library — Copilot Studio.*
- VE3 Global (2025). *10 Key Elements of a Prompt Library for Enterprise Tasks.*
