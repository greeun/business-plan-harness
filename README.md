# Business Plan Harness

A Claude Code skill that turns a short business idea into a rigorous, investor-grade **business plan** (or a full ~16-deliverable package) using a **Planner → Generator → Evaluator** three-agent GAN harness — adapted from Anthropic's *[Harness Design for Long-Running Application Development](https://www.anthropic.com/engineering/harness-design-long-running-apps)* (Prithvi Rajasekaran, Mar 24 2026).

> **[한국어 README](README.ko.md)**

---

## Why This Exists

Ask any AI to write a business plan and you get the same generic template every time: unsourced market sizes, top-down-only "1% of a huge TAM" sizing, shallow competitor lists, optimistic financials whose totals don't reconcile, and a model that cheerfully rates its own mediocre output as "great."

The root cause — identified by Anthropic in the code domain — is **self-evaluation failure**: generators confidently praise their own work, especially on subjective tasks where there is no compiler error or test failure to contradict them. Business plans are *more* subjective than code, and their financials are *more* prone to wishful thinking.

This skill solves it by **separating generation from evaluation** across distinct agents (each a separate `Agent` call — GAN anti-omission-bias), and by **gating research and financials before synthesis**.

---

## Architecture

Each role is dispatched as a **separate agent** that communicates only through files. The Full tier runs **4 research sprints**, each with a negotiated contract + Evaluator gate, before the plan is integrated.

```
  Planner ──► spec.md + sprint-playbook.md
     │
     ▼
  ┌─────────────── 4-Sprint Loop (S1→S2→S3→S4) ───────────────┐
  │  S1 market·competition·ecosystem   → research-s1.md       │
  │  S2 problem·customer·persona       → research-s2.md       │
  │  S3 business-model·unit-economics·financials → research-s3.md │
  │  S4 plan integration               → business-plan.md     │
  │                                                           │
  │  per sprint:  contract ─► Generator ─► Evaluator          │
  │               PASS ╱╲ FAIL ─► revise (cap 5–15)           │
  └───────────────────────────┬───────────────────────────────┘
                              ▼
        Final integrated Evaluator pass (4 axes + probes + 8 gates)
                              ▼
        business-plan.md  (or docs/ 16-deliverable package + HTML)
```

> **Domain swap from the source code-harness:** in a business plan the axis the model is weakest on is **business-model realism + financial coherence**, so **S3 = financials** (not UX/screens), and the conditional human checkpoint is a **financial-assumption review** (not a wireframe visual-craft review).

### Three Roles

| Role | Input | Output | Rule |
|------|-------|--------|------|
| **Planner** | Business idea + constraints + frozen mode | `spec.md` + `sprint-playbook.md` | Resolve ambiguity; never output generic templates |
| **Generator** | `spec.md` + current sprint contract | sprint deliverable + `generator_report.md` | Must NOT self-grade; negotiate the contract first |
| **Evaluator** | spec + contract + deliverable | `critique.md` with 1–5 scores | Adversarial; must NOT dismiss legitimate issues |

### Four Grading Criteria — *Weight What Claude Lacks*

| Criterion | What It Checks | Weight |
|-----------|---------------|--------|
| **C1 Problem·Market clarity** | who-what-why + concrete persona (situation/pain) | 1× |
| **C2 Differentiation·Moat** | explicit comparison vs ≥1 named competitor + axis + why-hard-to-imitate | **2×** |
| **C3 Feasibility·Unit-economics** | realistic scope/phasing + financials actually reconcile (bottom-up, not top-down-only) | **2×** |
| **C4 Metric·Financial measurability** | every KPI/number has value+method; assumed numbers labeled `ASSUMPTION:` | 1× |

C2/C3 are weighted 2× because Claude fills the C1/C4 section skeleton well by default — what's weak without pressure is a defensible edge and a financial model that holds together. A sprint passes only when **every criterion ≥ 4** (and no 2× axis < 4) with all binary gates clean.

### Eight Binary Gates (§8)

`G-a` value↔problem trace · `G-b` customer specificity · `G-c` differentiation comparison · `G-d` metric/financial measurability + `ASSUMPTION:` labeling · `G-e` scope/phase separation · `G-f` no placeholders · `G-g` infographic-fit · **`G-h` financial coherence** (totals reconcile + money rules single-sourced)

---

## Five Output Modes

Picked at activation (STEP 1, mandatory gate):

| Mode | Name | Gist |
|------|------|------|
| 1 | **Lean business plan** *(default)* | 1-pager / BMC-centric, startable |
| 2 | **Standard business plan** | Full narrative: market → BM → GTM → ops → financials → risk |
| 3 | **Investor IR plan** | Emphasis on market size, 3–5yr model, unit economics, funding ask |
| 4 | **Gov-grant · startup-program form** | Structured to the standard evaluation axes (problem·feasibility·growth·team·budget) |
| 5 | **Full business plan package** | A 5-stage **~16-deliverable** set + a visual HTML dashboard |

---

## Output

**Modes 1–4** → `business-plan/business-plan.md` (+ optional HTML).

**Mode 5** → a `docs/` package of 16 numbered deliverables, single-sourced and cross-traceable:

```
docs/
─ Discovery / strategy   00-business-model · 01-business-plan · 02-market-analysis ·
                         03-competition-ecosystem · 04-customer-persona
─ Definition / edge      10-value-proposition · 11-product-roadmap · 12-pricing-strategy
─ GTM / ops / org        20-gtm-strategy · 21-operations-plan · 22-team-org
─ Finance / funding      30-financial-model · 31-funding-plan · 32-risk-assessment ·
                         33-milestones-kpi
─ Guide + HTML           INDEX.md (tiers/order/role bundles/minimal set) ·
                         index.html · NN-name.html · overview.html
```

> **Single source of truth:** money rules (revenue/fees/pricing/unit economics) are defined once in `00-business-model` and only *referenced* downstream — never redefined (enforced by gate G-h).

Harness intermediate files (`spec.md`, `sprint-playbook.md`, `sprint_contract.md`, `research-s*.md`, `critique.md`, `handoff.md`, `assumptions_and_risks.md`) live in the same working tree.

### Infographic-first

Every structured section is delivered as the **visual that best holds the concept** — BMC, money-flow (Sankey), unit-economics chart, TAM/SAM/SOM concentric, ecosystem map, competition matrix, AARRR funnel, cap table, risk heatmap, milestone Gantt — as ASCII in `.md` and self-contained **inline SVG/CSS** in HTML (no Mermaid, no CDN, works offline). A financial doc with numbers in tables only and no chart fails G-g.

---

## Skill Structure

```
business-plan-harness/
├── SKILL.md                       # Orchestrator: 8-step flow, gates, model guidance
└── references/
    ├── source-article.md          # Full source inventory + code→business mapping
    ├── planner-prompt.md          # Planner agent prompt
    ├── generator-prompt.md        # Generator agent prompt (per-sprint)
    ├── evaluator-prompt.md        # Evaluator agent prompt (sprint + final pass)
    ├── rubric.md                  # 4 criteria, weights, verdict logic
    ├── evaluator-calibration.md   # Few-shot 1/3/5 scoring anchors
    ├── sprint-playbook.md         # 4-sprint plan, mode-adapted scope
    ├── mode-templates.md          # 5 mode skeletons + Mode 5 16-deliverable set
    ├── html-doc-template.md       # Per-document readable HTML + hub
    └── html-visual-template.md    # Diagram-rich dashboard
```

---

## Key Mechanisms

- **GAN role separation** — Generator and Evaluator are distinct agents, structurally countering self-evaluation bias (especially optimistic financials).
- **Sprint contracts** — before any drafting, Generator and Evaluator agree in writing on the observable checks; the Evaluator grades against those, not vague impressions.
- **Research/financial gating** — S4 synthesizes the plan only after S1–S3 (market, customer, financials) pass their gates. Stacking a plan on weak research collapses differentiation and feasibility.
- **Evidence or `ASSUMPTION:`** — every number is sourced or explicitly labeled; all are collected into `assumptions_and_risks.md` (a file generic AI plans never produce).
- **Financial coherence (G-h)** — revenue = price × volume; BEP/runway/cash-flow reconcile with unit economics; the funding ask matches use-of-funds + the runway it buys.
- **Financial-assumption human checkpoint** — when a financial model is present, the founder reviews the *input assumptions* (market capture, conversion, CAC, runway) before final PASS; the math is still scored automatically.
- **Context anxiety prevention** — under context pressure the recovery path is a structured `handoff.md` + a fresh agent, never compaction (which preserves the anxiety state).

---

## Installation

```bash
# Symlink into your skills directory
ln -s /path/to/business-plan-harness ~/.claude/skills/business-plan-harness
```

## Usage

Just say any of these in Claude Code — the skill activates automatically and asks for the mode first:

```
"사업계획서 써줘"
"draft a full business plan package for an AI-powered tutoring platform"
"투자유치용 IR 자료 만들어줘 — 반려동물 헬스케어 구독 서비스"
"validate my business idea: vertical SaaS for dental clinics"
```

---

## Methodology Source

A faithful adaptation of *Harness Design for Long-Running Application Development* (Prithvi Rajasekaran, Anthropic Labs, Mar 24 2026). Every principle, failure mode, grading criterion, iteration pattern, and lesson is preserved in `references/source-article.md` and mapped to the business-plan domain via an explicit Operational Mapping table.

## License

MIT
