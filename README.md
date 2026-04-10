# Business Plan Harness

A Claude Code skill that generates rigorous, investor-grade business plans using a **Planner → Generator → Evaluator** three-agent harness — adapted from Anthropic's *[Harness Design for Long-Running Application Development](https://www.anthropic.com/engineering/harness-design-long-running-apps)*.

> **[한국어 README](README.ko.md)**

---

## Why This Exists

Ask any AI to write a business plan and you get the same generic template every time: unsourced market sizes, shallow competitor lists, no executable first steps, and a model that cheerfully rates its own mediocre output as "great."

The root cause — identified by Anthropic in the code domain — is **self-evaluation failure**: generators confidently praise their own work, especially on subjective tasks where there is no compiler error or test failure to contradict them.

Business plans are *more* subjective than code. This skill solves the problem by **separating generation from evaluation** across distinct agent roles that keep each other honest.

---

## Architecture

```
                     ┌─────────────┐
                     │   Planner   │
                     │  spec.md    │
                     └──────┬──────┘
                            │
              ┌─────────────▼─────────────┐
              │       Sprint Loop         │
              │                           │
              │  Generator ──► output     │
              │       │                   │
              │       ▼                   │
              │  Evaluator ──► critique   │
              │       │                   │
              │   PASS ╱╲ FAIL ──► revise │
              │       ╱  ╲                │
              │      ▼    └───────────────│
              │  next sprint              │
              └───────────┬───────────────┘
                          │
                          ▼
                   final_plan.md
```

### Three Roles

| Role | Input | Output | Rule |
|------|-------|--------|------|
| **Planner** | Business topic + constraints | `spec.md` with sprint contracts | Resolve ambiguity; never output generic templates |
| **Generator** | `spec.md` + current sprint contract | Sprint section + `handoff.md` | Must NOT self-grade |
| **Evaluator** | `spec.md` + contract + sprint output | `critique.md` with 1–5 scores | Must NOT dismiss legitimate issues |

### Four Grading Criteria

| Criterion | What It Checks | Weight |
|-----------|---------------|--------|
| **Strategic Coherence** | Problem, solution, customer, model, and GTM reinforce each other | High |
| **Originality & Insight** | Non-obvious judgment about *this specific topic*, not boilerplate | High |
| **Craft & Rigor** | Numbers consistent, claims sourced or labeled `ASSUMPTION:` | Normal |
| **Executability** | A founding team could act on it next week | Normal |

A sprint passes only when **every criterion scores >= 4** with no unresolved blockers.

---

## Sprint Sequence

| # | Sprint | Deliverable |
|---|--------|-------------|
| 1 | Discovery | Problem, customer, pain evidence, comparables |
| 2 | Value & Positioning | Value prop, differentiation, wedge |
| 3 | Business Model | Pricing, unit economics, revenue model |
| 4 | GTM | Acquisition channels, first 100 customers |
| 5 | Operations & Team | Build vs. buy, org, hiring plan |
| 6 | Financials & Milestones | 18-month plan, runway, KPIs |
| 7 | Risk & Kill-Criteria | What would make you stop, and when |
| 8 | Consolidation | Assemble `final_plan.md` from passed sprints |

---

## Output

```
business-plan/
├── spec.md                    # Full specification with sprint contracts
├── plan.md                    # Accumulated sprint sections
├── critique.md                # Evaluator scoring history
├── handoff.md                 # Context-reset handoff at each boundary
├── final_plan.md              # Final assembled plan
└── assumptions_and_risks.md   # Every unverified claim, explicitly listed
```

---

## Key Mechanisms

### Context Anxiety Prevention

Models tend to wrap up work prematurely when context feels full. This skill writes a structured `handoff.md` at each sprint boundary and resets context, keeping late sections (financials, risk) at the same quality as early ones (market analysis).

### Sprint Contracts

Before any drafting begins, Planner and Generator agree on written acceptance criteria. The Evaluator grades against these exact criteria — not vague impressions.

### Assumption Tracking

Every numeric claim must cite a source or carry an `ASSUMPTION:` label. At the end, all assumptions are collected into `assumptions_and_risks.md` — a file that generic AI plans never produce.

---

## Installation

### As a Claude Code Skill

```bash
# Clone to your skills directory
git clone https://github.com/<your-org>/business-plan-harness.git \
  ~/.claude/skills/business-plan-harness
```

### Or symlink from an existing clone

```bash
ln -s /path/to/business-plan-harness ~/.claude/skills/business-plan-harness
```

---

## Usage

Just say any of these in Claude Code:

```
"사업계획 써줘"
"draft a business plan for an AI-powered tutoring platform"
"비즈니스 플랜 작성해줘 — 주제는 반려동물 헬스케어 구독 서비스"
"validate my business idea: vertical SaaS for dental clinics"
```

The skill activates automatically and begins with the Planner role.

---

## Methodology Source

This skill is a faithful adaptation of:

- **Article:** *Harness Design for Long-Running Application Development*
- **Author:** Prithvi Rajasekaran, Anthropic Labs
- **Published:** March 24, 2026

Every principle, failure mode, grading criterion, iteration pattern, and lesson from the article is preserved in the skill's Source Reference section and mapped to the business-plan domain via an explicit Operational Mapping table.

---

## License

MIT
