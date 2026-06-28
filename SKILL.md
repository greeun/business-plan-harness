---
name: business-plan-harness
description: 3-agent GAN harness (Planner→Generator→Evaluator) turning a short business idea or requirements into an investor-ready / startup-ready business plan or full deliverable package. User picks 1 of 5 modes (Lean plan / Standard plan / Investor IR / Gov-grant·startup-program form / Full package); 4 research sprints (market·competition·ecosystem → problem·customer·persona → business-model·unit-economics·financials → integration) with per-sprint contracts, quality gates, adversarial Evaluator + rubric. Mode 5 emits ~16 deliverables (business-model·market·competition·persona·value-prop·pricing·GTM·operations·team·financial-model·funding·risk·milestones). Infographic-first: best-fit visual per deliverable (BMC·TAM-SAM-SOM·ecosystem map·unit-economics·competition matrix·funnel·Gantt·cap table·risk heatmap·journey) as ASCII in `.md` + inline SVG/CSS in offline HTML. Also covers nonprofit / 비영리·공익단체 plans (arts orgs, foundations, social enterprises) — "investor/ROI" read as "funder·donor·grant / 자립도·사회적성과". Use when asked — Korean: 사업계획, 사업 계획서, 사업계획서 작성, 사업 기획, 창업 계획, 창업 기획서, 투자유치, IR 자료, 사업 아이템 검증, 비즈니스 플랜, 비즈니스 모델, 사업 전략, GTM 전략, 정부지원사업, 창업지원사업, 사업 패키지, 비영리 사업계획, 공익법인 사업계획, 후원 유치 계획, 예술단체 사업계획, 사회적기업 사업계획, 재정 자립 계획; English: business plan, startup plan, venture plan, investor deck plan, GTM strategy, business model canvas, validate my business idea, draft a business plan, full business plan package, nonprofit business plan, fundraising plan, grant proposal plan, arts organization plan. Default Korean output.
---

# Business Plan Harness

A GAN-style 3-agent harness that takes a short business idea (1–4 sentences) and produces a **business plan (사업계획서)** a founding team or an investor can act on immediately. Planner → Generator → Evaluator are each dispatched as separate `Agent` calls (GAN anti-omission-bias). **Full tier**: it runs 4 research sprints with per-sprint contract negotiation + quality gates, and runs per-sprint + a final integrated Evaluator pass.

> Skill-authoring language is English. The **generated deliverable** is written in the user-selected output language (STEP 1-c; default Korean). Korean trigger phrases are intentional — the skill must still activate on Korean requests.

The methodology is a direct port of Anthropic's "Harness Design for Long-Running Application Development" (Prithvi Rajasekaran, Mar 24 2026). The full source inventory + the code-harness→business-plan operational mapping live in `references/source-article.md` (read it to apply the methodology faithfully).

## Core design intent (why Full tier)

A business plan requires **research-phase decomposition + per-phase quality gating**. Stacking a plan on top of weak market research / problem definition / financial assumptions makes differentiation (C2) and feasibility·unit-economics soundness (C3) collapse structurally. So S4 synthesizes the plan **only after** the Evaluator passes the S1–S3 research through gates. This gating is not for context relief — it is an orthogonal quality mechanism. (See the model-guidance section — Opus 4.8 has effectively no context anxiety, yet Full was chosen deliberately.)

## Reference files (references/)

| File | Role |
|------|------|
| `references/source-article.md` | Full source-article inventory (Rajasekaran 2026) + code-harness→business-plan operational mapping. The methodology's source of truth. |
| `references/planner-prompt.md` | Planner prompt — handle mode selection + write spec.md/sprint-playbook.md |
| `references/generator-prompt.md` | Generator prompt — per-sprint contract negotiation + production + self-verify |
| `references/evaluator-prompt.md` | Evaluator prompt — per-sprint + final pass, adversarial probes, verdict logic |
| `references/rubric.md` | 4 criteria (C1–C4), weights (C2/C3 2×), justification, verdict logic |
| `references/evaluator-calibration.md` | few-shot 1/3/5 anchors (C1–C4) — scoring calibration |
| `references/sprint-playbook.md` | 4-sprint plan, deliverables, mode-adapted scope, observable checks |
| `references/mode-templates.md` | document-structure skeletons for the 5 output modes (incl. Mode 5 full-package 16 deliverables) |
| `references/html-doc-template.md` | render each deliverable `.md` as readable/shareable/reviewable HTML + hub `index.html`. ASCII diagrams converted to inline-SVG graphics + per-section infographics + generated images. Included in the STEP 1-b default ("Both") |
| `references/html-visual-template.md` | render the final deliverables as a self-contained, diagram-rich HTML dashboard (BMC·money flow·unit-economics·ecosystem map·competition matrix·funnel·cap table·Gantt·risk heatmap + topic-specific invented visualizations), all inline SVG/CSS + generated images where available (Mermaid discouraged). The `overview.html` of the default "Both" / the `index.html` of "Dashboard only" |

## 5 output modes

| Mode | Name | Gist |
|------|------|------|
| 1 | Lean business plan [default/recommended] | Minimal 1-pager / BMC-centric plan. Light S3 financials, no full 5-year model. |
| 2 | Standard business plan | + full narrative: market / competition / value prop / business model / GTM / operations / financials / risk. |
| 3 | Investor IR plan | + emphasis on market size, financial model (3–5yr), unit economics, funding ask / use-of-funds, team. |
| 4 | Gov-grant · startup-program form | Structured to the standard evaluation axes (problem·feasibility·growth potential·team·budget plan). Form-fit. |
| 5 | Full business plan package | A 5-stage ~16-deliverable set (discovery·strategy·definition·GTM/ops·finance/funding) + a **final visual HTML dashboard (`index.html`)**. After S1–S3 research, S4 expands into multi-document generation (g1~g6). Not a single business-plan.md but a numbered document set under `docs/` + diagram-rich visualization HTML. |

For detailed section structures see `references/mode-templates.md`. Mode 5's deliverable list and document skeletons are defined there too.

> **Nonprofit / 비영리·공익단체**: the same 5 modes, 16 deliverables, gates, and sprints apply unchanged — only the interpretation shifts (goal = 재정 자립도·사회적성과 not ROI; BEP = 수입⊇지출; `31-funding` = 공적지원·메세나·지정기부금·사회적기업, no equity; Mode 3 IR = 후원·지원사업 유치; Mode 4 = 공모 신청서). See mode-templates.md "Nonprofit / public-interest variant" for the full slot-by-slot mapping. The §8 gates (esp. G-d ASSUMPTION labels, G-h financial coherence) matter *more* here — nonprofit financials are benchmark-thin and drift optimistic.

## 4 research sprints (fixed, scope variable)

| Sprint | Name | Deliverable | Feeds | Mode adaptation |
|--------|------|-------------|-------|-----------------|
| S1 | research / market·competition·ecosystem | `research-s1.md` | C2, G-c/G-g | ecosystem+participant description·ecosystem map always included; Mode 3 expanded (TAM/SAM/SOM·segments·trends·regulation); otherwise at least 1 named competitor |
| S2 | problem·customer·persona | `research-s2.md` | C1, G-a/G-b | Mode 1 lean (1 persona); others may have multiple |
| S3 | business-model·unit-economics·financial hypotheses | `research-s3.md` | C3/C4, G-d/G-h | Mode 1 light (BMC + back-of-envelope unit economics); Mode 3/5 heaviest (3–5yr model, CAC/LTV, BEP, funding) |
| S4 | plan integration | `business-plan.md` | C3, G-a/G-d/G-e/G-g/G-h | integrate S1–S3 into the chosen mode's structure (no new research) |

The 4 sprints **always** run; only each sprint's scope adapts to the mode. **Note the domain swap vs the source code-harness: S3 = business-model·financials (the axis Claude is weak on for a business plan), not UX/screens.** Details in `references/sprint-playbook.md`.

---

## Activation Flow (orchestrator)

An 8-step flow. STEP 1 = output-mode selection (mandatory gate). The Full-tier 4-sprint loop (STEP 4) has per-sprint contract negotiation + evaluation.

### STEP 1 — mode selection + HTML form + output language (mandatory gate)
On activation, ask the user for and receive **three** things together:

**(1-a) Output mode** — (1) Lean business plan [default/recommended], (2) Standard business plan, (3) Investor IR plan, (4) Gov-grant·startup-program form, (5) Full business plan package (5-stage ~16 deliverables). If unanswered/ambiguous, **propose** (1) Lean but do not generate before confirmation.

**(1-b) HTML output form** (md always preserved; infographic-first, so a diagram dashboard is included by default; `references/html-doc-template.md`):
- **Both (per-document HTML + diagram-rich dashboard)** [default/recommended] — render each `.md` as readable/shareable/reviewable html (body ASCII diagrams converted to inline-SVG graphics + per-section inline-SVG infographics·generated images shown) + hub `index.html` (links by importance·order·role, reflecting `INDEX.md`) + diagram-rich dashboard `overview.html` (`references/html-visual-template.md`).
- **Visual dashboard only** — `index.html` = diagram-rich dashboard only (`references/html-visual-template.md`).
- **Per-document HTML only** — each `.md` → readable html (ASCII diagrams·inline SVG·generated images) + hub, dashboard omitted.
- **None (md only)** — skip HTML. (But the md body's infographics/diagrams [ASCII·matrices·generated images where available] are always embedded — G-g.)

**(1-c) Deliverable output language** — the language the generated deliverable is written in: **Korean [default]** or **English**. (The skill's own instructions are English; this controls only the produced document's prose/section headings.) If unanswered, default to Korean.

Freeze the chosen mode, HTML form, and output language into `spec.md` / `sprint-playbook.md`. **Output-location convention: all deliverables are written under the project's `business-plan/`** — Mode 5 → `business-plan/docs/` (numbered 16 `.md` + optional HTML + `INDEX.md`), Modes 1–4 → `business-plan/` (`business-plan.md` + optional HTML). Harness intermediate files (`spec.md`·`sprint_contract.md`·`research-s*.md`·`critique.md`·`handoff.md`) live in the same working tree. **Mode 5 is multi-deliverable**, so STEP 4 S4 expands per deliverable-group and the STEP 7 financial-assumption checkpoint almost always fires (financial model + funding included).

### STEP 2 — intake
Receive the 1–4 sentence business idea/requirements + any known constraints (geography, budget, target customer, founder background, timeline). If too vague, ask back about only 1–2 essentials and proceed (no over-questioning). If still vague, the Planner forms the single sharpest hypothesis and records it as an assumption in spec/playbook.

### STEP 3 — dispatch Planner
Run the Planner once via an `Agent` call on `references/planner-prompt.md`. It writes `spec.md` with the frozen mode's section structure and **writes the 4-sprint plan + each sprint's mode-adapted scope into `sprint-playbook.md`**. Returns: `SPEC_READY: spec.md` + `PLAYBOOK_READY: sprint-playbook.md`.

### STEP 4 — 4-sprint loop (S1→S2→S3→S4, each sprint cap 5–15)
For each sprint in order:

- **4a — contract negotiation**: The Generator (`Agent` call, `references/generator-prompt.md`) proposes that sprint's `sprint_contract.md` (scope + observable checks + output file/format). Dispatch the Evaluator (`Agent` call, `references/evaluator-prompt.md`, Mode A1) to **approve/amend** the contract. If the checks are weak, the Evaluator strengthens them. **No build before approval (safety gate).**
- **4b — sprint build**: Dispatch the Generator via an `Agent` call. It produces that sprint's deliverable (`research-s1.md`/`-s2.md`/`-s3.md`, or S4's `business-plan.md`) and `generator_report.md`, self-verifying before handoff. Returns: `READY_FOR_QA`, or `HANDOFF_NEEDED` (→ 4d), or `DEADLOCK` (→ STEP 8).
- **4c — sprint evaluation**: Dispatch the Evaluator (`Agent` call, Mode A2) to produce `critique.md` against that sprint's observable checks + sprint-relevant probes/gates. On `PASS`, advance to the next sprint; on `FAIL` and round < cap, retry the same sprint (applying the Generator's Strategic Decision: REFINE/PIVOT); on `FAIL` and round = cap, transparently record the unresolved issues and get the user's judgment on whether to advance.
- **4d — context handoff**: On `HANDOFF_NEEDED`, resume the sprint with a fresh Generator session reading only `handoff.md`. **No compaction** — compaction preserves the anxiety state, so it is forbidden as a recovery path (reset ≠ compaction).

### STEP 5 — final integrated Evaluator pass
After S4, dispatch the Evaluator (Mode B) via an `Agent` call to run **the 4-axis rubric + all adversarial probes + all §8 gates** on the integrated `business-plan.md`, producing `critique.md`. Returns: `CRITIQUE_READY: critique.md`.

### STEP 6 — verdict branching
- `PASS` (all ≥4, 2×(C2/C3) ≥4, probes clean, all §8 pass) → STEP 7.
- `FAIL` → if the gap is attributable to a specific sprint deliverable, return to that sprint (STEP 4, within cap); if it's an integration-stage gap, retry S4. If the overall cap is reached, transparently report the current best version + unresolved blocking issues.

### STEP 7 — conditional financial-assumption human-checkpoint (most relevant right after S3/S4)
Check whether `research-s3.md` / `business-plan.md` / `30-financial-model.md` contains a **financial model / funding-ask / key-assumption block** (market size, conversion rate, price, CAC/LTV, runway, use-of-funds).
- **Not present (Mode 1 generally, light financials)** → **skip** the checkpoint, fully automatic, proceed to STEP 8.
- **Present (Modes 3/5, and Mode 2/4 when a model is included)** → **ask the user to review·approve the key financial assumptions** (the input numbers the projections rest on), and do not confirm the final PASS before approval. The plan's text/logic PASSes normally; the assumption block is marked `HUMAN_CHECKPOINT_REQUIRED` and surfaced to the user.

> Rationale for this gate: an LLM can read every word of a text deliverable and judge its internal consistency (the Evaluator enforces G-h financial-coherence on the math). But the **input assumptions** a financial model rests on — realistic market capture, achievable conversion/CAC, defensible price, the founder's actual runway and risk appetite — depend on the founder's real domain knowledge and risk preference, which the LLM cannot confirm alone. So a human checkpoint is inserted only for the assumption inputs (the math/consistency is still scored automatically by G-h). "The Evaluator will handle it" is not allowed for the input assumptions — approving them is the founder's call. (This is the business-plan analog of the source harness's wireframe visual-craft human checkpoint.)

### STEP 7.5 — HTML render (branches on the STEP 1-b choice; md always preserved)
Render the PASSed final deliverable into HTML in the form chosen at STEP 1-b. **All HTML generates no new content — only faithfully convert/visualize the existing `.md` deliverables** (money rules cite the single source `00-business-model`). Readable HTML preserves the md's ASCII diagrams (monospace), promotes md data into **inline SVG/CSS infographics**, and displays generated image assets (`assets/`). **No external-renderer dependency (Mermaid discouraged) — every visualization is self-contained.** If a financial-assumption block is included, render after the STEP 7 human checkpoint clears.
- **Both** [default]: per `references/html-doc-template.md`, each `.md` → readable `NN-name.html` + hub `index.html` (nav from `INDEX.md`'s importance·order·role) **+ diagram-rich dashboard `overview.html`** (`references/html-visual-template.md`).
- **Visual dashboard only**: `index.html` = diagram dashboard (`references/html-visual-template.md`). Fill the visualization catalog (BMC·money flow·unit-economics·TAM-SAM-SOM·ecosystem map·competition matrix·positioning·persona journey·value loop·AARRR funnel·GTM timeline·org chart·financial trend·cap table·use-of-funds·risk heatmap·milestone Gantt·KPI gauges, plus topic-specific invented visualizations) with real data, all inline SVG/CSS + generated images where available.
- **Per-document HTML only**: the "Both" above minus `overview.html`.
- **None**: skip HTML (md only — but the md body's infographics/diagrams are always embedded).

### STEP 7.6 — importance-grouping·order guide (Mode 5, after all documents written)
In Mode 5, after all deliverables (16 + `index.html`) are complete, **generate `INDEX.md` last** (`references/mode-templates.md` "INDEX.md generation rules", sprint-playbook S4-g7). Include: ① 3-tier importance (T1 essential / T2 conditional / T3 supplementary, judged per domain) ② work order (dependency-based) ③ per-role bundles (Founder/Investor/Marketing-Sales/Operations) ④ minimal start set (Lean 5). Don't make documentation theater — only classify·sort by actual need, no new content.

### STEP 8 — deliver/finish
Deliver the final output (Mode 5 = `docs/` 16 `.md` + `INDEX.md` + STEP 1-b HTML). The original `.md` is always preserved. On `DEADLOCK`, summarize both sides' positions and request the user's judgment. Always also produce `assumptions_and_risks.md` — every unverified claim / `ASSUMPTION:`-labeled number the plan depends on, collected in one place.

### User-confirmation gate summary
(a) no generation before the STEP 1 mode is confirmed; (b) no build before the STEP 4a contract is approved; (c) no final PASS before user approval when STEP 7 financial-assumption block is present; (d) transparent reporting of unresolved issues when the cap is reached.

### Safety gates (domain)
mode-confirmation gate (STEP 1), per-sprint contract-approval gate (STEP 4a), conditional financial-assumption human-checkpoint (STEP 7), §8 binary verification gates (STEP 4c/STEP 5).

---

## Iteration cap & iteration wisdom

- **iteration cap = 5–15 per sprint, default start ~8** — this is a **range, not a single value.** The cap applies per sprint (S1–S4), and the whole run is the sum of the 4 sprints.
- **Low-end (5) warning**: cutting a sprint too early (e.g. 3 rounds) can miss a **late-round breakthrough**. In the source, the best result came at iteration 10 — "Dutch Art Museum leap at iteration 10". Choosing a low cap risks cutting off such late-iteration breakthroughs (e.g. a pivot to a sharper wedge / a more defensible revenue model).
- **Wall-clock tolerance**: don't rush artificially. A good plan takes time — don't cut rounds just because progress feels slow.
- **An intermediate iteration may beat the final**: the Evaluator leaves an **Iteration Quality Note** in `critique.md`, calling out strengths a prior round had. Preserve those in the next round so they aren't lost.

---

## Principles

- **reset ≠ compaction**: under context pressure the recovery path is `handoff.md` + a fresh session, not compaction. Compaction preserves the anxiety state, so it is **forbidden** as a recovery path. Context anxiety is rare on Opus 4.8, but follow this rule when it occurs.
- **Every component encodes an assumption**: each component of this harness encodes an assumption about "something the model can't do alone." Here the sprint structure's assumption is "research/financial quality must be gated before synthesis." On model upgrade, validate assumptions **one at a time** — remove one component at a time and measure (radical all-at-once removal failed; methodical one-at-a-time succeeded).
- **GAN separation**: Generator and Evaluator are split into separate `Agent`s precisely to structurally counter self-evaluation bias (an LLM's tendency to confidently praise mediocre work — especially optimistic financials).
- **Simplicity first** (Anthropic, "Building Effective Agents"): "find the simplest solution possible, and only increase complexity when needed." This skill choosing Full is not a violation of simplicity — it's for the orthogonal need of research/financial gating, and it simplifies once that need disappears.
- **Evidence or ASSUMPTION**: every numeric claim (market size, growth rate, conversion, CAC, LTV, price, cost) is either sourced or explicitly labeled `ASSUMPTION:`. An unlabeled invented number is a defect, not a fact. This is a first-class quality bar (probe + G-d), not optional polish.
- **Infographic-first**: content with structure (hierarchy·flow·relationship·timeline·comparison·state·money flow) is conveyed first through **the visual that best holds the concept**, not a prose/table wall — BMC·mind map·money-flow (Sankey)·unit-economics chart·TAM/SAM/SOM concentric·ecosystem map·competition matrix·positioning scatter·funnel·journey·Gantt·cap-table stack·risk heatmap·KPI gauge, and when no standard diagram fits, **invent a topic-specific visualization** (gate G-g). Diagrams are the primary medium, not decoration. **3-tier visualization production (use the highest available)**:
  1. **Image generation (when possible)** — persona webtoon panels·ecosystem illustrations·concept heroes·scenario scenes. Save to `docs/assets/`, reference from md (`![](assets/…)`)·HTML. Fall back to tier 2 if unavailable.
  2. **Inline SVG/CSS infographics** — the primary visual medium for HTML output (readable + dashboard). Hand-author data-driven charts·matrices·flows·ecosystem maps·money flows·Gantt·cap tables in SVG/CSS. **Zero external dependencies** (Mermaid discouraged — self-contained·offline·design control first).
  3. **ASCII diagrams + matrix tables** — portable visualization in the md body (no renderer needed). The md always carries this tier or above.
  - **Every document, not just the dashboard — comprehensively**: the infographic-first bar applies to **every deliverable** and to **every per-document readable HTML**, not only `overview.html`. Each `NN-name.html` visualizes **every major section/concept** of that document — typically **several** best-fit **inline SVG/CSS infographics** per page. Existing ASCII diagrams in the `.md` are **CONVERTED to inline-SVG graphics** in the HTML (raw ASCII kept only inside a collapsed `<details>`). A faithful text/ASCII-only conversion with zero inline `<svg>` fails the bar.
- **Plain language over jargon (쉬운 설명 우선)**: prefer plain, everyday wording over jargon. When a finance/business term is unavoidable, gloss it in one short clause the first time (e.g. "ARPU(가입자 1명당 평균 매출)", "LTV(고객 1명이 평생 가져다주는 매출)", "BEP(손익분기점 — 버는 돈과 쓰는 돈이 같아지는 시점)", "런웨이(지금 자금으로 버틸 수 있는 개월 수)"). Calibrate to the broadest stakeholder who must read the doc (심사역·경영·비전문 투자자 as well as 재무). Visual labels, captions, and infographics follow the same rule. This is an explicit quality bar (adversarial probe + Definition-of-Done item), not optional polish.
- **Financial-coherence defaults (business common sense)**: when building the business model / financial model, apply sound defaults instead of naive ones:
  - **Money rules in one place.** Revenue / fees / pricing / unit economics are defined once in `00-business-model` (Mode 5) or the business-model section (Modes 1–4); downstream documents (pricing, financial model, funding) reference it — no redefinition or contradiction (G-h).
  - **Bottom-up, not just top-down.** Don't size the opportunity only as "1% of a huge TAM"; tie revenue to a concrete acquisition funnel (channel → reachable users → conversion → ARPU). Top-down-only sizing is a finding.
  - **Internal consistency.** Revenue = price × volume; BEP, runway, and cash-flow are arithmetically consistent with the unit economics; the funding ask matches the use-of-funds and the runway it buys. A model whose totals don't reconcile is a G-h defect.
  These are first-class **findings the Evaluator flags** — a plan with contradictory money rules, top-down-only sizing, or non-reconciling totals is downgraded.
- **Ecosystem lens**: analysis does not stop at a single customer / single competitor but describes **the whole ecosystem (value network) the business sits in and all its participants** — supply/demand side, platforms·intermediaries, complement·substitute providers, regulators·institutions, payment·infrastructure·channel·data partners. Identify each participant's role·incentive·value exchange·dependency and visualize it as an **ecosystem map (stakeholder / value-network diagram)** (S1·02-market·03-competition-ecosystem). This is the foundation for the differentiation (C2)·moat·platform two-sidedness argument.

---

## V1 vs V2 guidance (per model)

This skill is **Full tier (the V1 line)**. Recommended tier by model class:

| Model class | Context anxiety | Recommended tier | Notes |
|-------------|----------------|-----------------|-------|
| **Sonnet 4.5** | Strong — wraps up prematurely | Full (V1) | Small sprints, aggressive Evaluator, firm context resets |
| **Opus 4.5** | Largely eliminated | Simplified (V2) | Multi-hour coherent sessions; sprint decomposition droppable |
| **Opus 4.6** | Eliminated; improved planning, long-context, debugging | Simplified or Single-session | 2+ hour builds sustainable; re-examine every component, drop what's not load-bearing |
| **Opus 4.8** | Eliminated (1M context) | Simplified by default — BUT this skill chooses Full deliberately | Full chosen NOT for context relief but for research-phase decomposition + per-sprint quality gating (S1–S3 research/financials gated before S4 synthesis). Stress-test on upgrade (G-1/G-2): can the model self-organize the 4 research phases + self-gate quality without sprint contracts? If yes, drop sprints one at a time. |

### Opus 4.8: Full vs Simplified justification (G-1/G-2)
Opus 4.8 (1M context) has effectively eliminated context anxiety — from a context standpoint the sprint structure is redundant, so the source recommends **Simplified** by default. But here **Full was chosen deliberately.** Reason: this sprint structure is not a context-relief device but an **orthogonal mechanism** — research-phase decomposition + per-phase quality gating (S4 synthesizes the plan on top of S1–S3 research/financials only after the Evaluator passes it through gates). A strong model does not lose this gating's benefit (assumption: "research/financial quality must be gated before synthesis" — model strength does not invalidate it; an LLM left ungated still drifts to optimistic, non-reconciling financials).

**Upgrade stress-test (G-1/G-2, one at a time)**: "Can the model self-organize the 4 research phases and self-gate quality (incl. financial coherence) without explicit sprint contracts?" If YES, remove the sprint structure **one sprint/contract at a time** and re-measure — don't remove it all at once (radical removal failed; methodical one-at-a-time succeeded). The harness space doesn't shrink, it shifts — re-examine what's load-bearing on every upgrade.

---

## Evaluator tuning workflow

An untuned Evaluator is too lenient. Treat the first runs as drafts and calibrate via this **operating loop** (G-4/P-3). Don't settle for a one-sentence acknowledgment — actually run (a)–(d):

- **(a) Read critique logs** — read completed runs' `critique.md` side by side with the actual deliverables. For each rubric score, ask: "Would a demanding VC partner / startup-program judge give the same score?"
- **(b) Identify divergence patterns** — pinpoint the divergence patterns. Typical: lenient scoring (accepting generic/shallow output as adequate), accepting top-down-only market sizing, passing optimistic non-reconciling financials, passing non-measurable KPIs, passing abstract customers on C1, accepting "more convenient/cheaper" as differentiation on C2.
- **(c) Update prompt or calibration with counter-examples** — add **concrete counter-examples** targeting that failure mode to `references/evaluator-prompt.md` or `references/evaluator-calibration.md` (e.g. "a TAM cited with no SAM/SOM bottom-up basis is 2/5, not 5/5"). Few-shot calibration reduces score drift.
- **(d) Rerun on the same input; confirm the catch** — rerun on the same input and confirm the Evaluator now catches the prior miss. If it does, lock that calibration.

Skipping any of (a)–(d) is not tuning. Iterate until the Evaluator's verdicts correlate with a careful human-expert pass and every blocking issue it raises is reproducible (expect several cycles).

---

## §8 domain verification gates (binary)

All 8 binary gates must pass before the final PASS (Generator self-checks before S4 handoff; Evaluator verifies at STEP 4c/STEP 5). Details in `references/sprint-playbook.md` / `references/evaluator-prompt.md`.

| Gate | Check | PASS / FAIL |
|------|-------|-------------|
| G-a value↔problem trace | every core value/offering maps to a specific customer problem | every offering linked to ≥1 problem / any unmapped offering |
| G-b customer specificity | ≥1 persona includes situation·pain points (frequency/consequence) | situation·context·pain points stated / abstract demographics only |
| G-c explicit differentiation comparison | explicit comparison vs ≥1 real competitor + comparison axis + moat argument | competitor name + axis + why-hard-to-imitate present / unspecified or "it's better"-style |
| G-d metric & financial measurability | every KPI/financial number has a value + measurement/derivation method, and every assumed number is sourced or labeled `ASSUMPTION:` | each metric has number·method·period AND every number sourced or `ASSUMPTION:`-labeled / any non-measurable metric or unlabeled invented number |
| G-e scope / phase separation | MVP·Phase1 / out-of-scope·later-phase explicitly separated (incl. use-of-funds scope) | separation present / no separation or ambiguous |
| G-f no placeholders | placeholder/TBD/empty cells = 0 | scan finds 0 / any |
| G-g infographic fit | a document with structure (hierarchy·flow·relationship·timeline·comparison·money flow) expresses that structure as a diagram/infographic. **+ every per-document readable HTML visualizes each major section — typically several inline-SVG/CSS infographics per page (not just one, not just the dashboard).** | each structured document embeds ≥1 concept-appropriate visualization (image/inline SVG/ASCII/matrix) AND each `NN-name.html` carries a visualization for each major section (several inline `<svg>`) / structure left as a text wall, a per-doc HTML with zero inline `<svg>` (ASCII-in-`<pre>` only) or a single token diagram while other major sections stay text walls, any empty diagram, or dependence on an external renderer such as Mermaid |
| G-h financial coherence | financial numbers are internally consistent and money rules are single-sourced | revenue = price × volume; BEP·runway·cash-flow reconcile with unit economics; funding ask matches use-of-funds + runway; money rules defined once in 00/business-model and only referenced elsewhere / any contradiction, non-reconciling total, or duplicated/conflicting money rule |

---

## File handoffs (file-based communication, full set)

Roles communicate only through files. Active files:
`spec.md`·`sprint-playbook.md` (Planner→) / `sprint_contract.md` (Generator⇄Evaluator, per sprint) / `research-s1.md`·`research-s2.md`·`research-s3.md` (sprint deliverables, read by later sprints) / `business-plan.md` (S4 final, or `docs/` 16 docs for Mode 5) / `generator_report.md`·`critique.md` (per sprint + final) / `handoff.md` (Generator → fresh Generator, under context pressure) / `assumptions_and_risks.md` (final, every unverified/ASSUMPTION-labeled claim).

Each role is dispatched as a fresh `Agent` call reading only its handoff files. The Planner runs once; the Generator and Evaluator alternate within each sprint + across the 4 sprints.

---

## Out-of-scope
Code debugging ("fix this React component bug"), content writing ("write a blog post"), and pure service/dev specs (PRD·IA·ERD·API·wireframes — that is `service-planning-harness`) are not business-plan production, so this skill does not activate for them. Activate only when matching the description's triggers (business plan / startup plan / investor IR / BMC / business-model validation, etc.).
