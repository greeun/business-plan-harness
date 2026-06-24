# Rubric — business-plan-harness

> The evaluation rubric. Scores a business-plan (사업계획서) deliverable on 4 axes.
> The Evaluator uses this rubric in the per-sprint pass (only the relevant axes) and the final integrated pass (all 4 axes).
> For scoring calibration (the few-shot 1/3/5 anchors) see `evaluator-calibration.md`.

## Design principle — "Weight What Claude Lacks"

By default Claude fills structure, skeleton, and technical correctness well. What is weak without pressure is **differentiation, feasibility/financial soundness, and domain taste**. So those weak axes get 2× weight. The axes where Claude fills the section skeleton well by default are left at 1×, but §8 binary gates block surface satisfaction (the section exists but the content is empty, abstract, or non-reconciling).

Style Magnet Warning: write criteria as **quality**, not as a **reference name**. Brand/name drops like "like Toss", "YC-grade", "McKinsey-grade", "north-star-metric level" converge the Generator onto one direction, so they are banned. State the quality bar in plain words (e.g., "compared explicitly against at least 1 named competitor on a stated axis").

## The 4 evaluation criteria (C1–C4)

| # | Criterion (Korean) | Weight | What is measured | Justification for Claude's default weakness |
|---|--------------------|--------|------------------|---------------------------------------------|
| **C1** | **problem · market-definition clarity** (문제·시장정의 명확성) | **1×** | Is the who / what / why problem defined sharply, with a concrete persona (situation·pain points), not abstract demographics? Is the market it sits in defined? | Claude fills a structured problem/market section well by default (adequate by default) → 1×. But the customer-specificity gate (G-b) prevents passing with abstract demographics only. |
| **C2** | **differentiation / originality / moat** (차별화·독창성·해자) | **2×** | Is there an explicit differentiator vs ≥1 **named** competitor on a stated comparison axis, plus a why-hard-to-imitate (moat) argument? | Without pressure Claude drifts to table-stakes generalities like "more convenient / cheaper than existing options". An originality/specificity/defensibility weak axis → 2× confirmed. |
| **C3** | **feasibility & unit-economics soundness** (실행가능성·유닛이코노믹스 건전성) | **2×** | Is the scope realistic with clear phasing, and do the business model & financials actually reconcile (revenue = price × volume; BEP/runway consistent; bottom-up, not top-down-only)? | Without pressure Claude drifts to optimistic, non-reconciling, over-scoped financials and top-down-only sizing. An actionability/financial-soundness weak axis → 2× confirmed. |
| **C4** | **metric & financial measurability** (지표·재무 측정가능성·정합성) | **1×** | Does every KPI/financial number have a value + measurement/derivation method, and is every assumed number sourced or labeled `ASSUMPTION:`? | Claude generates the KPI/finance section itself by default (structure adequate) → 1×. But the measurability gate (G-d) FAILs non-measurable metrics ("improve satisfaction") and unlabeled invented numbers. |

**2× weights confirmed**: the 2× axes = **C2 differentiation/originality/moat + C3 feasibility & unit-economics soundness** (the axes Claude is weak on for a business plan). C1/C4 are left at 1× because Claude fills the section skeleton by default, but §8's binary gates (G-a~G-h) block surface satisfaction.

## Score meaning (1–5)

| Score | Meaning |
|-------|---------|
| 5 | Exceeds expectations — would impress a demanding VC partner / startup-program judge |
| 4 | Meets expectations — solid practical quality |
| 3 | Acceptable but noticeably weak — needs reinforcement |
| 2 | Below expectations — meaningful gaps |
| 1 | Unacceptable — fundamental problems |

## Verdict Logic

```
All criteria ≥ 4 AND adversarial probes clean AND all §8 gates pass → PASS
Any 2×-weighted criterion (C2/C3) < 4                              → FAIL  (this is the core rule)
Any 1×-weighted criterion (C1/C4) < 3                              → FAIL
Any Definition-of-Done item or §8 gate unverified/failed          → FAIL
```

## Calibration Checkpoint

After scoring, if every axis is ≥4, apply the following second-pass lenses just before passing and add findings to the critique:

1. **Domain-expert lens**: "What would a demanding VC partner / startup-program judge catch?"
2. **Competitor lens**: "Where would a named competitor point out this plan as a weakness?"
3. **Customer lens**: "Would the target customer actually pay for this — is the value worth the price to them?"
4. **Infographic lens**: "Is content that has structure (hierarchy / flow / relationship / timeline / comparison / money flow / state transition) conveyed as a diagram/infographic, or is it a text wall? Does each per-document HTML visualize each major section's key content (several inline `<svg>`), not just one token diagram, with existing ASCII converted to SVG graphics?" — structure left as prose/table (or raw ASCII) docks C1 (clarity) and C3 (readability/actionability). The presence, fit, and per-section coverage of visualization is separately enforced by the §8 gate **G-g (infographic fit, binary)** (empty diagram / external-renderer dependence such as Mermaid / a per-doc HTML with zero-or-one `<svg>` = FAIL).
5. **Plain-language lens (쉬운 설명)**: "Read as 심사역 / 경영 / 비전문 투자자, not just a finance expert — is there unexplained jargon or an undefined acronym? Is the first use of each finance/business term glossed in one clause (ARPU·LTV·BEP·런웨이, etc.)?" Unexplained jargon docks C1 (clarity). Enforced as adversarial probe + a Definition-of-Done item.

In the per-sprint pass, score only the axes tied to that sprint:

| Sprint | Scored axis(es) | Notes |
|--------|-----------------|-------|
| S1 (market·competition·ecosystem) | **C2** | + G-c / G-g |
| S2 (problem·customer·persona) | **C1** | + G-a / G-b |
| S3 (business-model·unit-economics·financials) | **C3 + C4** | business-model / financial coherence — **this is the domain swap: S3 is financials, NOT UX/screens.** + G-d / G-h |
| S4 / final | **all 4 axes** | + all adversarial probes + all §8 gates on the integrated `business-plan.md` |

The infographic/financial gates **G-g (infographic fit)** and **G-h (financial coherence / single-source money rules)** are binary and apply alongside the rubric — a passing rubric score does not override a failed G-g or G-h. For the full sprint→axis mapping and the §8 gate definitions see `evaluator-prompt.md` and `sprint-playbook.md`.
