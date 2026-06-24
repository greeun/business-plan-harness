# Mode Templates — business-plan-harness

> Document structures (section skeletons) for the 5 deliverable modes. Based on the mode the user picks in STEP 1, the Planner fills the "Structure / sections" part of `spec.md` with this skeleton, and during S4 integration the Generator writes `business-plan.md` (Modes 1–4) or the `docs/` document set (Mode 5) in this structure.
> Non-functional requirements shared by all modes: 0 placeholders/TBDs (G-f); every core value/offering maps to a specific customer problem from the problem definition (G-a value↔problem trace); every metric/financial number includes a measurable value + measurement/derivation method, and every assumed number is sourced or explicitly labeled `ASSUMPTION:` (G-d); MVP/Phase1 vs out-of-scope/later-phase explicitly separated incl. use-of-funds scope (G-e); **every document with structure embeds concept-fit visualization (infographic-first, G-g)**; **financial numbers are internally consistent and money rules are single-sourced (G-h financial coherence)**. (§8 gates G-a–G-h)

> Section labels here are English; the Generator renders headings in the user-selected output language (STEP 1-c; default Korean).

---

## Infographic-first — per-document optimal visual-representation mapping (shared by all modes)

> Content that has structure (hierarchy / flow / relationships / time axis / comparison / state transition / money flow) is delivered not as a text wall but as **the visual representation that best captures that concept**. Three tiers of visualization production (use the highest available): ① **image generation (when possible)** real illustrations (persona webtoon / ecosystem / concept hero / scenario scene) → `docs/assets/`, referenced via md `![](assets/…)` / HTML / ② **inline SVG/CSS infographics** (the primary medium for HTML deliverables, 0 external dependencies) / ③ **ASCII diagrams / matrix tables** (md body, no renderer needed). **Diagrams that depend on external renderers such as Mermaid are not recommended** (self-contained, offline, design control). When a standard diagram does not fit the concept, **invent a topic-specific visualization**.

| Document | Core structure | Optimal visual representation (md: ASCII/matrix · HTML: inline SVG · image when possible) |
|------|-----------|----------------------------------------------------------------|
| 00-business-model | Money/settlement flow / unit economics / revenue sources | **Money-flow diagram (Sankey-style SVG/ASCII)** · unit-economics bar/donut · BMC grid (9 blocks) · LTV:CAC gauge |
| 01-business-plan | Overall structure / value loop / phase scope | Structure mind map · value-loop diagram · MVP↔Phase2 2-axis/quadrant |
| 02-market-analysis | Market size / segments / trends | **TAM/SAM/SOM concentric circles** · market-size bars · segment donut/pie · trend timeline |
| 03-competition-ecosystem | **Ecosystem / value-network** / positioning / competition | **Ecosystem map (stakeholder / value-network diagram)** · positioning 2-axis scatter · competition matrix (✓/△/✗ color chips) · 5-forces diagram |
| 04-customer-persona | Journey / empathy / value fit | Persona journey map · pain-gain matrix · value-proposition canvas · **explainer webtoon panels (image when possible)** |
| 10-value-proposition | Differentiation / moat | Value-proposition canvas (VPC) · moat diagram · before/after comparison |
| 11-product-roadmap | Feature hierarchy / phased roadmap | Feature mind map (P0/P1/P2 branches/colors) · roadmap timeline · MVP↔expansion quadrant |
| 12-pricing-strategy | Tiers / price ladder | Pricing comparison table · price ladder · price-sensitivity curve |
| 20-gtm-strategy | Funnel / channels | **AARRR funnel** · channel-CAC matrix · GTM timeline · acquisition flow |
| 21-operations-plan | Process / supply | Operations process flow · supply/partner map · build-vs-buy matrix |
| 22-team-org | Org / hiring | Org chart · hiring Gantt · capability-gap radar |
| 30-financial-model | Projections / BEP / sensitivity | Revenue/P&L trend line · cash-flow/runway area chart · BEP graph · sensitivity tornado |
| 31-funding-plan | Use-of-funds / cap table | Use-of-funds donut · cap-table stack · milestone-round timeline |
| 32-risk-assessment | Risks / kill-criteria | **Risk heatmap (likelihood×impact)** · assumption-validation matrix · kill-criteria decision tree |
| 33-milestones-kpi | Schedule / KPI | Milestone Gantt · KPI dashboard gauges · OKR tree |

> Modes 1–4 (single document) use only what fits the relevant sections (e.g. Mode 1 Lean → structure mind map · BMC grid · money-flow · unit-economics chart · MVP quadrant · competition matrix · metric gauges). Mode 5 uses all of the above. In any mode, **a section that has structure is never left as prose/tables only without a diagram (G-g)**.
>
> **Financial-coherence defaults (business common sense, always-on)**: when any section touches the business/financial model, apply sound defaults instead of naive ones — (1) **money rules in one place** — revenue / fees / pricing / unit economics are defined once (Mode 5: `00-business-model`; Modes 1–4: the business-model section) and every downstream section (pricing · financial model · funding) references it, no redefinition/contradiction (G-h); (2) **bottom-up, not just top-down** — don't size revenue only as "1% of a huge TAM"; tie it to a concrete acquisition funnel (channel → reachable users → conversion → ARPU); top-down-only sizing is a finding; (3) **internal consistency** — revenue = price × volume; BEP, runway, and cash-flow reconcile with the unit economics; the funding ask matches the use-of-funds and the runway it buys. A model whose totals don't reconcile, with contradictory money rules or top-down-only sizing, is a G-h defect the Evaluator flags.
>
> **Plain language over jargon (always-on)**: prefer plain wording; the first time an unavoidable finance/business term appears, gloss it in one short clause (e.g. "ARPU(가입자 1명당 평균 매출)", "LTV(고객 1명이 평생 가져다주는 매출)", "BEP(손익분기점)", "런웨이(현재 자금으로 버틸 수 있는 개월 수)"). Visual labels and captions follow the same rule. Calibrate to the broadest stakeholder who must read the doc (심사역·경영·비전문 투자자 as well as 재무).

---

## Mode 1 — Lean business plan [default/recommended]

The lightest, fastest-to-start minimal plan — a 1-pager / BMC-centric plan a founding team can start from. Scoped so the business is startable now, without a full 5-year model.

```
1. Problem definition (who-what-why)
   - for whom / what / why is it a problem (evidence or ASSUMPTION:-labeled)
2. Target customer (1 key persona)
   - 1 person including situation / context / pain points (1 key persona because Lean)
3. Value proposition & differentiation (brief)
   - explicit comparison against at least 1 named competitor/alternative
4. Core offering / solution
   - each offering → problem mapping (offering↔problem traceability table)
5. Business model (BMC-level)
   - revenue model + back-of-envelope unit economics (price · cost · margin per unit)
   - money rules single-sourced here (revenue/fees/pricing) — referenced by §8 below
6. Go-to-market
   - key acquisition channel + how the first customers are reached
7. Phase scope
   - Phase1/MVP included vs out-of-scope/later, + rationale
8. Success metrics & key financials
   - each metric: number + measurement method; key numbers sourced or ASSUMPTION:-labeled
```

S3 light (BMC + back-of-envelope unit economics, no full model) → STEP 7 financial-assumption checkpoint usually skipped.

---

## Mode 2 — Standard business plan

Mode 1 structure + full narrative across market / competition / value prop / business model / GTM / operations / financials / risk.

```
1. Executive summary
2. Problem & market context
3. Target customers & personas (multiple if needed)
4. Value proposition & differentiation
   - explicit comparison vs ≥1 named competitor + comparison axis + moat argument
5. Product/service & roadmap
   - offering → problem mapping (traceability table) + phased roadmap
6. Business model & pricing
   - revenue model + unit economics (price · cost · margin); money rules single-sourced here
7. Go-to-market
   - channels + funnel + first 100 customers
8. Operations & team
9. Financial plan
   - revenue/cost projection + BEP + runway (reconciling with the unit economics, G-h)
10. Phase scope (MVP/Phase1 vs later, explicitly separated)
11. Risks & assumptions
12. Milestones & KPIs (each measurable: number + measurement method)
```

A financial projection may be included → if included, STEP 7 financial-assumption human checkpoint is triggered.

---

## Mode 3 — Investor IR plan

Mode 2 structure + emphasis on market size, financial model (3–5yr), unit economics, and the funding ask / use-of-funds. **S1 (market/competition/ecosystem) + S3 (financials) are expanded.**

```
1. Executive summary (the ask in one line)
2. Problem
3. Market analysis
   - TAM/SAM/SOM with basis (bottom-up), segments, trends, regulation
4. Competitive analysis & ecosystem
   - named-competitor matrix + positioning + **ecosystem / value-network map** + moat
5. Solution / product
6. Business model & unit economics
   - revenue model + CAC/LTV + pricing; money rules single-sourced here
7. Traction / validation
   - if any; else clearly ASSUMPTION:-labeled hypotheses
8. Go-to-market
9. Team
10. Financial model
   - 3–5yr P&L + cash flow + BEP + sensitivity (reconciling, G-h)
11. Funding ask & use-of-funds
   - amount + the runway it buys + milestones it unlocks + cap table + exit
12. Risks & milestones/KPIs (measurable)
```

S1 + S3 expanded (market size / 3–5yr model / unit economics / funding). STEP 7 financial-assumption human checkpoint is triggered.

---

## Mode 4 — Gov-grant · startup-program form

Structured to the standard evaluation axes (problem · feasibility · growth potential · team · budget plan). Modeled on a typical Korean 창업지원사업 / 정부지원 application — section labels map directly to the evaluation rubric and to the §8 gates.

```
1. 창업 아이템 개요 (Item summary)
   - 문제인식 배경 · 창업 동기 · 아이템 한 줄 정의 (→ G-a)
2. 문제인식 (Problem)
   - market / customer problem + evidence (사용자·시장 근거; ASSUMPTION:-labeled where unverified) (→ G-a / G-b)
3. 실현가능성 (Feasibility)
   - solution · differentiation vs named competitors · development/realization plan · current status (개발현황/보유역량) (→ 차별화 = G-c)
4. 성장전략 (Growth)
   - business model & revenue (수익모델) + market entry & scale-up (GTM) + funding/투자 plan + 사업비 집행계획 (budget / use-of-funds) (→ 사업비/financials = G-d / G-e / G-h)
5. 팀 구성 (Team)
   - 대표 · 팀원 역량 + R&R + 외부 협력 (멘토/파트너/자문)
+ 성과지표 (measurable KPIs — number + measurement method) (→ G-d)
+ 추진일정 (milestones / Gantt) (→ G-e phase separation)
```

> Section→gate map: 문제인식 → G-a/G-b · 차별화(실현가능성) → G-c · 사업비/financials/성과지표 → G-d/G-e/G-h. Each axis is written to satisfy its gate(s).

STEP 7 financial-assumption human checkpoint is triggered if a budget / 사업비 / financial block is included (almost always, since 사업비 집행계획 is a form requirement).

---

## Mode 5 — Full business plan package

Generates the **entire set of business-plan deliverables** for a startup / fundraising / program submission in one go. Not a single document but numbered multiple documents in a `docs/` directory. After sharing S1–S3 research, S4 expands into **per-deliverable-group generation**.

> The same non-functional requirements shared by all deliverables apply (§8 gates G-a–G-h, including infographic-first G-g and financial-coherence G-h). **Single-source-of-truth rule**: money rules (revenue / fees / pricing / unit economics) are finalized in one place, `00-business-model.md`, and downstream documents (`12-pricing-strategy`, `30-financial-model`, `31-funding-plan`) reference it as-is (no duplicate definition / contradiction G-h). All documents must be mutually traceable (problem↔customer↔value↔offering↔financials consistent).

### Deliverable list (5 stages, ~16 types)

```
docs/
─ A. Discovery / strategy (why does this business exist)
  00-business-model.md         revenue sources / fees / settlement triggers / unit economics (SINGLE SOURCE of money rules)
  01-business-plan.md          master plan (problem / customer / value / market / BM / financials / execution summary)
  02-market-analysis.md        TAM/SAM/SOM, segments, trends, regulation (organized from S1)
  03-competition-ecosystem.md  competitor matrix + industry ecosystem / value-chain map
  04-customer-persona.md       personas, JTBD, pain, buying journey (based on S2)
─ B. Definition / edge (what makes it win)
  10-value-proposition.md      value prop, differentiation, moat, positioning / VPC
  11-product-roadmap.md        product/service definition, core features, phased roadmap
  12-pricing-strategy.md       pricing, packaging, tiers (references 00)
─ C. GTM / ops / org (how it sells & runs)
  20-gtm-strategy.md           GTM, channels, sales/marketing funnel, first 100 customers
  21-operations-plan.md        operations, core processes, supply/partners, build-vs-buy
  22-team-org.md               team, org, hiring plan, R&R, advisors
─ D. Finance / funding / verify
  30-financial-model.md        3–5yr P&L, cash flow, BEP, sensitivity (references 00)
  31-funding-plan.md           funding ask, use-of-funds, cap table, exit
  32-risk-assessment.md        risks, assumptions, kill-criteria, mitigations
  33-milestones-kpi.md         milestones, roadmap, KPIs/OKRs, success metrics
─ E. Guide + HTML (STEP 1-b choice; md always preserved)
  INDEX.md                     importance grouping + work order + per-role bundles + minimal start set across all deliverables (generated LAST, after all documents are written).
  index.html                   [if per-doc HTML + hub chosen] hub — importance / order / per-role links (reflecting INDEX.md). `references/html-doc-template.md`.
  NN-name.html                 [if per-doc HTML + hub chosen] each `.md`'s read/share/review HTML (original .md preserved). `references/html-doc-template.md`.
  overview.html                [if "both" chosen] aggregate visual dashboard (BMC / money flow / TAM-SAM-SOM / ecosystem map / funnel / cap table / Gantt / risk heatmap / KPI gauges). `references/html-visual-template.md`.
  (or) index.html              [if "visual dashboard only" chosen] index.html = aggregate visual dashboard.
```

### INDEX.md generation rule (last, after all documents are written — g7)

After making all 16 types, generate `INDEX.md` so the reader knows "what to read first / who / why". To avoid becoming a document for documents' sake, **judge actual necessity per mode/domain** and group accordingly. Include these 4 blocks:

1. **3 importance tiers** (place each document in one + one-line rationale):
   - **T1 essential** — the pitch-critical core: `00-business-model` · `01-business-plan` · `02-market-analysis` · `10-value-proposition` · `30-financial-model` · `33-milestones-kpi`.
   - **T2 conditionally essential** — essential depending on the case: `31-funding-plan` (when fundraising) · `12-pricing-strategy` (when usage/tier-priced) · `20-gtm-strategy` · `32-risk-assessment`. (If essential for this case, promote to T1 in the notation.)
   - **T3 supporting** — for alignment / persuasion / depth: `03-competition-ecosystem` · `04-customer-persona` · `11-product-roadmap` · `21-operations-plan` · `22-team-org`.
   - The judgment is not fixed. E.g.: for a **fundraising** case promote `31-funding-plan` to T1; for an **internal / bootstrapped** case drop `31-funding-plan` or move it to T3; for a **two-sided marketplace** case promote `03-competition-ecosystem` (ecosystem map) to T1. INDEX records the actual tiers for this business.

2. **Work order** (dependency-based, numbered): ① `00` (lock the money rules single-source first) → ② `01` · `02` · `03` · `04` (why / who) → ③ `10` · `11` · `12` (edge / what) → ④ `20` · `21` · `22` (how to sell / run) → ⑤ `30` · `31` · `32` · `33` (finance / funding / verify, referencing `00`) → ⑥ `index.html` (visual aggregate). If dependencies change, adjust the order.

3. **Per-role bundles** (no single person reads all 16 types):
   - Founder / 대표: `01` · `00` · `30` · `33`
   - Investor / 심사역: `00` · `02` · `10` · `30` · `31` · `22`
   - Marketing–Sales: `04` · `20` · `12`
   - Operations / COO: `21` · `22` · `33`

4. **Minimal start set** (Lean — enough to pitch with just these): `01` · `00` · `02` · `10` · `30`. + add `31-funding-plan` if fundraising. State that "the full set is for the full raise / program submission / kickoff, and this set is enough to start pitching".

Attach a one-line purpose next to each document name, plus a T1/T2/T3 badge. INDEX.md must not generate new content — it only classifies/sorts existing deliverables.

### Skeleton of each deliverable (gist)

> Each document embeds the optimal visualization from the "infographic-first mapping" above into the body (G-g). md = ASCII diagrams / matrices, HTML = inline SVG, generated image when possible. Mermaid not recommended. Money rules are single-sourced in `00` and only referenced downstream (G-h).

- **00 business-model**: revenue-sources table / **money-flow diagram (Sankey-style)** / take-rate or pricing justification / **unit economics** (example calculation + bar/donut, LTV:CAC gauge) / settlement trigger sequence (event→who→how much→when) / **BMC 9-block grid** / assumptions / risks. (**SINGLE SOURCE of all money rules.**)
- **01 business-plan**: master plan threading the whole story — problem / customer / value / market / business model (link to 00) / financials summary (link to 30) / execution summary (**structure mind map** + **value loop** + MVP↔Phase2 quadrant).
- **02 market-analysis**: TAM/SAM/SOM with bottom-up basis (**TAM/SAM/SOM concentric circles** + market-size bars) + segments (**segment donut**) + trends (**trend timeline**) + regulation/policy context + market gap/opportunity.
- **03 competition-ecosystem**: **ecosystem analysis (all participants / roles / incentives / value exchange / dependencies) + ecosystem map (stakeholder / value-network diagram)** + named-competitor matrix (✓/△/✗) + positioning quadrant + **5-forces** + moat/whitespace argument.
- **04 customer-persona**: key personas (situation / pain / JTBD / frequency / outcome) + **persona journey map** + **pain-gain matrix** + value-proposition fit + (when possible) **explainer webtoon panel image**.
- **10 value-proposition**: value-prop statement + **value-proposition canvas (VPC)** + differentiation vs named competitors + **moat diagram** + before/after comparison + positioning.
- **11 product-roadmap**: product/service definition + core features (P0/P1/P2, **feature mind map**) + **phased roadmap timeline** + MVP↔expansion quadrant + scope/out-of-scope.
- **12 pricing-strategy**: pricing tiers + packaging (**pricing comparison table** + **price ladder**) + price-sensitivity rationale (**curve**) — **references 00 money rules, no redefinition** (G-h).
- **20 gtm-strategy**: GTM motion + channels (**channel-CAC matrix**) + sales/marketing funnel (**AARRR funnel**) + first-100-customers plan + **GTM timeline** + acquisition flow.
- **21 operations-plan**: core processes (**operations process flow**) + supply/partners (**supply/partner map**) + build-vs-buy decisions (**build-vs-buy matrix**) + key dependencies/SLAs.
- **22 team-org**: team & roles (**org chart**) + capability vs gap (**capability-gap radar**) + hiring plan (**hiring Gantt**) + R&R + advisors/external collaboration.
- **30 financial-model**: 3–5yr P&L + cash flow + BEP + sensitivity (**revenue/P&L trend line** + **cash-flow/runway area chart** + **BEP graph** + **sensitivity tornado**) — **references 00 unit economics; totals reconcile** (G-h).
- **31 funding-plan**: funding ask (amount + the runway it buys + milestones unlocked) + **use-of-funds donut** + **cap-table stack** + **milestone-round timeline** + exit scenarios — references 00/30.
- **32 risk-assessment**: risk register + **risk heatmap (likelihood × impact)** + key assumptions (**assumption-validation matrix**) + **kill-criteria decision tree** + mitigations.
- **33 milestones-kpi**: milestones / roadmap (**milestone Gantt**) + KPIs/OKRs (**KPI dashboard gauges** + **OKR tree**) + success metrics (each measurable: number + method, G-d).

S1 is expanded (market/competition/ecosystem → 02 · 03), S3 is heaviest (business-model/unit-economics/3–5yr financials/funding → 00 · 30 · 31), and S4 expands into the 16 types above. Includes 30-financial-model + 31-funding-plan → **STEP 7 financial-assumption human checkpoint almost always triggered.**

---

## Per-mode sprint-scope adaptation summary

> **Domain swap vs the source code-harness: S3 = business-model · unit-economics · financials (the axis the model is weak on for a business plan), not UX/screens. STEP 7 = financial-assumption checkpoint (not a wireframe visual-craft checkpoint).**

| Mode | S1 (market / competition / ecosystem) | S2 (problem / customer) | S3 (business model / financials) | S4 (integration) | STEP 7 financial-assumption checkpoint |
|------|----------------|----------------|-------------------|-----------|-----------------------------|
| 1 Lean business plan | at least 1 named competitor + ecosystem participants/map | 1 key persona | light — BMC + back-of-envelope unit economics, no full model | Mode 1 structure | usually skipped |
| 2 Standard business plan | at least 1 named competitor + ecosystem map | personas (multiple possible) | standard — revenue/cost projection + BEP + runway | Mode 2 structure | triggered if a financial projection is included |
| 3 Investor IR plan | **expanded** — TAM/SAM/SOM (bottom-up) / segments / trends / regulation + ecosystem analysis/map | personas | **heaviest** — 3–5yr P&L / CAC·LTV / BEP / sensitivity / funding ask | Mode 3 structure | triggered |
| 4 Gov-grant · startup-program form | at least 1 named competitor + ecosystem map | key persona + problem evidence | 수익모델 + 사업비 집행계획 (budget/use-of-funds) | Mode 4 (form) structure | triggered if a budget/financial block is included (almost always) |
| 5 Full business plan package | **expanded** (TAM/SAM/SOM + competitor matrix + ecosystem analysis/map → 02 · 03 docs) | multiple personas (→ 04 doc) | **heaviest** (00 unit economics + 30 3–5yr model + 31 funding → 00 · 30 · 31 docs) | **multiple documents ~16 types** (S4 expands per deliverable group) | almost always triggered (financial model + funding included) |

---

## Nonprofit / public-interest variant (비영리·공익단체 변형)

> When the venture is a **nonprofit, arts/culture org, foundation, social enterprise, or grant-funded body** (no equity to sell), the harness runs unchanged — only the *interpretation* of a few slots shifts. Keep the same 16 deliverables, gates, and 4 sprints. Validated on a real run (a nonprofit ballet company's economic-revitalization plan).

| Slot | For-profit default | Nonprofit reading |
|------|--------------------|-------------------|
| Goal frame | profit / ROI | **revenue diversification → deficit reduction → 재정 자립도(self-sufficiency) → mission continuity**. Never "ROI." |
| BEP (G-h) | revenue > cost (profit) | **수입 ⊇ 지출** (break-even / deficit→0); "BEP" = the year the income mix covers spending |
| 00 money rules (수입원) | revenue streams | **4대 수입원: 공연/판매수입 · 공적지원(공모·보조금) · 후원·기부(개인 정기 + 기업 메세나/CSR) · 교육·부대사업**. Single-source still in 00. |
| C3 unit economics | unit profit, CAC/LTV | **per-event funding-gap** (제작비 − 티켓수입 = 갭 + 충당원 명시) + donor LTV:CAC + **자립도 비중 전환**. A structurally loss-making event funded by grants/donations is normal, not a defect — the gap's *stable funding* is the model. |
| Mode 3 "Investor IR" | equity raise / cap table | **후원·지원사업 유치 IR** (후원자·지원기관 설득). `31` cap-table/exit → **use-of-funds + 자립 로드맵 + 매칭 의지**. |
| Mode 4 "Gov-grant form" | startup-program app | maps natively to **공모 신청서** (아르코·문화재단·문체부 평가항목: 문제·실현가능성·성장성·팀·사업비). |
| `31-funding-plan` | VC/loan/equity | **공적지원 캘린더 · 기업 메세나(1+1 매칭) · 개인 정기후원 · 지정기부금(공익법인 세액공제) · 사회적기업 전환 · 크라우드펀딩**. No equity. |
| `22-team-org` gap | sales/BD hire | **Development(모금·후원개발) 전담** — usually the biggest org gap; justify by self-funding (한 명이 인건비 이상 모금). |
| Differentiation moat (C2/G-c) | tech/network/brand | **사회적 가치(취약계층·공익 임팩트)** + 전달망 + 지역거점 — and these unlock 지정기부금·메세나·사회적기업 자격 (자금조달 자산). |
| Ecosystem (S1/03) | suppliers/channels | + **후원개인 · 기업메세나 · 공적지원기관 · 위탁 지자체 · 산하법인 · 언론** as value-network participants. |
| STEP 7 checkpoint | revenue/cost assumptions | **연 예산 규모 · 수입 구성비 · 편당 제작비 · 현 후원 현황 · 공익법인(지정기부금) 등록 여부 · 런웨이** — confirm with the org (these depend on real books, not benchmarks). |

Visualization adds: **수입믹스 전환 누적막대(자립도 ↑)** · **공모 신청 캘린더 타임라인** · **메세나 매칭 레버리지(1+1)** · **후원자 0→N 펀넬**. Drop equity-only visuals (cap table) unless a for-profit subsidiary exists.

Rule of thumb: if the body cannot issue equity, read "investor" as "funder/donor/grantmaker," "ROI" as "자립도 + 사회적 성과," and keep everything else. The §8 gates (esp. G-d ASSUMPTION labels, G-h coherence) matter *more* here because nonprofit financials are benchmark-thin and easily optimistic.
