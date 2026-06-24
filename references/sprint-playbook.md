# Sprint Playbook — business-plan-harness (Full tier)

> REQUIRED Full-tier file. Defines the plan for the 4 fixed research sprints, each sprint's deliverable file, the mode-adapted scope, and the observable checks.
> In STEP 3, the Planner reflects the user-selected mode and outputs an instance of this playbook as `sprint-playbook.md` (filling in the structure below + the mode-adapted scope). At the start of each sprint the Generator reads this playbook and proposes a `sprint_contract.md`; the Evaluator approves/amends the contract against this playbook's observable bars and evaluates the sprint deliverable.

## Why 4 sprints (Full-tier justification)

A business plan requires **research-phase decomposition + per-phase quality gating**. If you stack a plan on top of weak market research / problem definition / financial assumptions, differentiation (C2) and feasibility·unit-economics soundness (C3) collapse structurally. So **only after** the Evaluator passes the S1–S3 research deliverables through gates does S4 synthesize the plan on top of them. This gating is not for context relief — it is an orthogonal quality mechanism (a stronger model does not invalidate this assumption — an ungated LLM still drifts to optimistic, non-reconciling financials; see the SKILL.md model guidance).

## 4 fixed sprints

In every mode the 4 sprints **always** run. Each sprint's `sprint_contract.md` scope adapts to the selected mode.

### S1 — research / market·competition·ecosystem research → `research-s1.md`

- **Purpose**: market context + identify at least 1 real competitor/alternative + benchmark + **ecosystem·participant analysis**. Provides the basis for differentiation (C2).
- **Feeds**: C2, §8 gates G-c (explicit differentiation comparison), G-g (ecosystem map).
- **Observable checks (bars to include in the contract)**:
  - At least 1 **real (named)** competitor/alternative is identified.
  - For each competitor, a comparison axis (what it does / does not do) is recorded.
  - Market/customer context is described.
  - **The full set of ecosystem participants** (supply/demand side, platforms·intermediaries, complements·substitutes, regulators·institutions, payment·infrastructure·channel·data partners) is identified along with their roles·incentives·value exchange·dependencies.
  - An **ecosystem map (stakeholder / value network diagram)** is visualized (infographic-first, G-g — no text wall,
    no dependence on external renderers such as Mermaid).
- **Mode-adapted scope**:
  - Mode 3 (Investor IR): expanded — market sizing (TAM/SAM/SOM basis)·segments·trends·regulation·revenue model·ecosystem analysis included.
  - Modes 1/2/4: minimal — "at least 1 explicit competitive comparison" + context + ecosystem participants·map.
- **Evaluator sprint pass**: C2 scoring + G-c/G-g gates + explicit differentiation comparison probe + ecosystem/visualization fitness probe.

### S2 — problem·customer·persona research → `research-s2.md`

- **Purpose**: who-what-why problem definition + concrete persona (situation·pain points). Basis for problem-definition clarity (C1).
- **Feeds**: C1, §8 gates G-a (the problem list that is the starting point for tracing), G-b (customer specificity).
- **Observable checks**:
  - The problem is sharply defined in who/what/why form.
  - At least 1 persona is concrete down to situation·context·pain points (including frequency/consequence) — abstract demographics alone fail.
- **Mode-adapted scope**:
  - Mode 1 (Lean): lean with 1 core persona.
  - Modes 2/3/4: multiple personas as needed, but the 1 core persona must be concrete.
- **Evaluator sprint pass**: C1 scoring + G-a (problem list exists)/G-b gates + customer specificity probe.

### S3 — business-model·unit-economics·financial hypotheses research → `research-s3.md`

> **Domain swap (vs the source code-harness)**: in a business plan, S3 is the **financial / business-model research** — the axis Claude is weakest on for a business plan — **NOT UX/screens**.

- **Purpose**: revenue model + pricing hypothesis + cost structure + unit economics (CAC/LTV) + BEP + runway + key financial assumptions — each number sourced or `ASSUMPTION:`-labeled. The **money rules originate here (single source).** Basis for feasibility·unit-economics soundness (C3) and market-attractiveness/financial-upside (C4).
- **Feeds**: C3/C4, §8 gates G-d (metric & financial measurability), G-h (financial coherence).
- **Observable checks**:
  - The revenue model is defined (how money is made, from whom, on what cadence).
  - Unit economics are shown — CAC (고객 1명 확보 비용) and LTV (고객 1명이 평생 가져다주는 매출), each with a stated basis.
  - BEP (손익분기점) and runway (지금 자금으로 버틸 수 있는 개월 수) are computed and **internally consistent** with the unit economics.
  - Market sizing is **bottom-up** — tied to an acquisition funnel (channel → reachable users → conversion → ARPU), not top-down-only ("1% of a huge TAM" alone fails).
  - **Every number is sourced or explicitly labeled `ASSUMPTION:`** — an unlabeled invented number is a defect.
  - A **money-flow diagram (Sankey/value-flow)** + a **unit-economics chart** are visualized (infographic-first, G-g — no text wall, no external-renderer dependence).
- **Mode-adapted scope**:
  - Mode 1 (Lean): light — BMC (business model canvas) + back-of-envelope unit economics, **no full 5-year model**.
  - Mode 2 (Standard): standard — revenue model + unit economics + cost structure + a simple multi-period view.
  - Modes 3/5 (IR / Full package): **heaviest** — 3–5yr financial model + funding ask / use-of-funds.
- **Evaluator sprint pass**: C3 + C4 scoring + G-d/G-h gates + financial-coherence probe + market-sizing (bottom-up vs top-down-only) probe. (If a financial-assumption block exists → the STEP 7 human checkpoint may trigger.)

### Mode 5 (full business-plan package) sprint adaptation

Mode 5 is not a single document but a set of ~16 deliverables. The 4 sprints run as is but with expanded scope:
- **S1**: competition matrix + **ecosystem analysis·ecosystem map** organized into `02-market-analysis.md` / `03-competition-ecosystem.md` (expanded).
- **S2**: personas into `04-customer-persona.md` (multiple allowed). The problem list is the anchor for the value proposition·plan.
- **S3**: full **financial model + unit economics** done heavily → the **money single-source** `00-business-model.md` + research foundation for `30-financial-model.md`. A financial-model block existing → triggers the STEP 7 financial-assumption checkpoint.
- **S4**: expanded **per deliverable-group** (integration writing is not a single document but multiple documents per group). Recommended group split (each group = 1 sprint_contract + 1 Evaluator pass):
  - S4-g1 discovery·strategy: **00-business-model (LOCK the single source of money rules FIRST)**, 01-business-plan, 02-market-analysis, 03-competition-ecosystem, 04-customer-persona
  - S4-g2 definition·edge: 10-value-proposition, 11-product-roadmap, 12-pricing-strategy
  - S4-g3 GTM·ops·org: 20-gtm-strategy, 21-operations-plan, 22-team-org
  - S4-g4 finance·funding·verify: 30-financial-model (references 00), 31-funding-plan, 32-risk-assessment, 33-milestones-kpi
  - S4-g0 image assets (when possible, infographic-first tier ①): if image-generation capability exists, produce persona webtoons·ecosystem illustrations·concepts etc. into `docs/assets/` and reference them from the relevant md/HTML. If no capability, fall back to inline SVG/ASCII (skippable).
  - S4-g6 HTML output (STEP 1-b choice; md always preserved, after g1~g4 PASS + STEP 7 clearance): **both**[default] = each `.md`→readable `NN-name.html` (ASCII preserved + inline SVG infographics + generated images) + hub `index.html` (`references/html-doc-template.md`, reflecting INDEX.md) + diagram synthesis `overview.html` (`html-visual-template.md`) / **dashboard only** = `index.html` diagram synthesis (`html-visual-template.md`) / **per-document HTML only** = overview omitted / **none** = skip. No new content generation at all (only conversion·visualization of existing deliverables), 0 dependence on external renderers (Mermaid not recommended).
  - S4-g7 guide: `INDEX.md` — generated **last, after all documents (g1~g6) are complete**. 3-tier importance grouping of all deliverables (T1 essential / T2 conditional / T3 supporting) + work order (dependency-based) + per-role bundles (Founder/Investor/Marketing-Sales/Operations) + minimal starting set (Lean). Follows the "INDEX.md generation rules" in `references/mode-templates.md`. Judge the actual tiers per domain (e.g., for a capital-heavy venture, 30·31·00 as T1). No new content generation — classification·sorting only.
  - **Order enforcement**: g1 (especially the 00-business-model money rules) must be locked first so g4 (financial-model·funding) can stack consistently on top of it. Money rules are defined in only one place (00). Always last in the order g6 HTML → g7 INDEX.

### S4 — plan integration writing → `business-plan.md` (final deliverable; Mode 5 is the per-group multiple documents above)

- **Purpose**: integrate the S1–S3 research into the selected mode structure (`mode-templates.md`). **No new research, integration + scope/priority/metrics finalization only.** (Mode 5 expands into multiple documents under `docs/`.)
- **Feeds**: C3, §8 gates G-a/G-d/G-e/G-g/G-h (and the placeholder slot for all of G-f).
- **Observable checks / Definition of Done**:
  - All sections of the selected mode are filled with real content (0 placeholders — G-f).
  - Every core value/offering is mapped to a specific customer problem (value↔problem tracing table — G-a).
  - MVP·Phase1 / out-of-scope·later-phase are explicitly separated (G-e), **including use-of-funds scope**, and priorities are justified.
  - Every KPI/financial number has a measurable value + measurement/derivation method (tool/period/criterion) attached (G-d).
  - The differentiation section explicitly compares against at least 1 real competitor + comparison axis + moat argument (G-c, leveraging the S1 basis).
  - Financials reconcile and money rules are single-sourced — revenue = price × volume; BEP·runway·cash-flow consistent with unit economics; funding ask matches use-of-funds + runway; money rules defined once and only referenced elsewhere (G-h).
  - Each document/section that has structure embeds a concept-fit visualization (diagram·infographic) (G-g — 0 text walls, 0 empty diagrams,
    0 dependence on external renderers such as Mermaid; generated images when possible).
  - Cross-section consistency is cross-checked.
- **Evaluator pass**: this sprint pass + **in the STEP 5 final integration pass, all 4 axes + all probes + all 8 §8 gates**.

## Sprint loop (common to each sprint)

```
1) Generator: propose sprint_contract.md (deliverable + observable checks + output file/format, mode-adapted)
2) Evaluator: approve or amend the contract (strengthen if the checks are weaker than the spec/playbook bars) — no building before approval
3) Generator: write the sprint deliverable → self-verify (all contract checks) → generator_report.md → READY_FOR_QA
4) Evaluator: evaluate the deliverable against the contract checks + sprint-relevant probes/gates → critique.md → PASS/FAIL
5) PASS → next sprint / FAIL & round<cap → retry the same sprint (Strategic Decision) / FAIL & round=cap → transparently report unresolved issues + user judgment
```

## Iteration cap (per sprint)

- **5–15 rounds per sprint, default start ~8** (a range, not a single value).
- Picking the low end (5) may miss a late-round breakthrough — "iteration 10 leap" warning (SKILL.md). The cap applies per sprint; the whole run = the sum of the 4 sprints.

## File flow (Full file set)

`spec.md`·`sprint-playbook.md` (Planner→) / `sprint_contract.md` (Generator⇄Evaluator, per sprint) / `research-s1.md`·`research-s2.md`·`research-s3.md` (sprint deliverables, read by subsequent sprints) / `business-plan.md` (S4 final, or `docs/` 16 docs for Mode 5) / `generator_report.md`·`critique.md` (per sprint + final) / `handoff.md` (under context pressure) / `assumptions_and_risks.md` (final, every unverified/`ASSUMPTION:`-labeled claim).
