# Evaluator Calibration — business-plan-harness

> Few-shot scoring calibration anchors. Before scoring, the Evaluator reads this file and aligns each criterion's 1/3/5 score to these anchors. This aligns scoring with the judgment of a demanding VC partner / startup-program judge and reduces score drift between rounds.
> The anchors are concrete examples from the Korean business-plan domain. **Do not change** — apply them as-is, only add to them.
> Operational tuning (read the critique log, add missing patterns as counterexamples) follows the SKILL.md "Evaluator tuning workflow (a)–(d)" procedure. Update this file when adding a new counterexample there.

---

## C1 problem·market-definition clarity (문제·시장정의 명확성)

- **1/5**: "An app for busy people. People find time management hard." ("바쁜 사람들을 위한 앱. 사람들이 시간 관리를 어려워한다.") — who/why is abstract, no persona, situation, or market boundary.
- **3/5**: "A small-cafe owner can't see daily sales, inventory, and labor cost in one place, so ordering and shift decisions are guesswork." ("소규모 카페 사장이 일별 매출·재고·인건비를 한 곳에서 못 봐서 발주와 스케줄을 감으로 결정한다.") — target and problem are clear, but the frequency/context/cost of the pain is shallow.
- **5/5**: "An owner-operator of 1–2 cafes doing 3,000만~5,000만원 monthly revenue (e.g., Park, age 41, runs the counter herself) reconciles POS sales, supplier invoices, and part-timer hours by hand every closing, ~40 min/day; she over-orders or runs out about 6 times a month, costing roughly 80만원/month in waste and lost sales." ("월매출 3,000만~5,000만원 규모 1~2개 점포를 직접 운영하는 사장(예: 만 41세 박OO, 본인이 카운터를 봄)이 매일 마감마다 POS 매출·거래처 세금계산서·알바 근무시간을 손으로 대사하는 데 하루 약 40분을 쓰고, 월 6회꼴로 과발주·품절을 내며 약 월 80만원의 폐기·기회손실을 본다.") — sharp down to persona, situation, frequency, and quantified consequence.

## C2 differentiation·originality·moat (차별화·독창성·해자)

- **1/5**: "More convenient and cheaper than existing services." ("기존 서비스보다 더 편리하고 저렴합니다.") — no competitor named, no differentiation axis, no moat (table-stakes generality).
- **3/5**: "Compared to 캐시노트, we auto-link supplier invoices." ("캐시노트 대비 거래처 세금계산서를 자동 연동한다.") — names one competitor but the comparison axis is single and there's no argument about why a competitor couldn't copy it.
- **5/5**: "Against 캐시노트 (sales-aggregation focus, no inventory/ordering layer), 도소매 ERP like 이카운트 (full-featured but too heavy/expensive for a 1-person cafe), and 엑셀 (manual, no real-time link), the core differentiator is auto-merging POS + supplier e-invoice + labor data into a one-tap daily order recommendation. The moat is a proprietary supplier–SKU matching dataset built from early users' purchase history (network effect: more users → denser supplier coverage → better recommendations), plus a ~6-month head start on the e-invoice parsing pipeline." ("캐시노트(매출 집계 중심, 재고·발주 레이어 없음)·이카운트 같은 도소매 ERP(기능은 많으나 1인 카페엔 과중·고가)·엑셀(수기, 실시간 연동 없음) 대비, POS+거래처 전자세금계산서+근무데이터를 자동 병합해 일일 발주 추천을 원탭으로 주는 것이 핵심 차별점이다. 해자는 초기 사용자 구매 이력에서 쌓이는 거래처-SKU 매칭 독자 데이터셋(네트워크 효과: 사용자↑ → 거래처 커버리지 조밀↑ → 추천 정확도↑)과 전자세금계산서 파싱 파이프라인의 약 6개월 선점이다.") — multiple named competitors, comparison axes, and an explicit defensible-moat argument.

## C3 feasibility·unit-economics soundness (실행가능성·유닛이코노믹스 건전성)

- **1/5**: "If we take 1% of the market within a year, revenue is 50억." ("1년 안에 시장 1% 점유하면 매출 50억.") — top-down-only sizing, no acquisition funnel, no unit economics, over-scoped and not startable.
- **3/5**: "MVP is the daily sales/inventory dashboard; add order recommendation later. We'll grow through cafe-owner communities." ("MVP는 일별 매출·재고 대시보드, 발주 추천은 이후 추가. 카페 사장 커뮤니티로 확산한다.") — MVP scope is narrowed, but out-of-scope is vague and no unit economics (CAC/LTV/BEP) are shown.
- **5/5**: "Bottom-up: 카페 사장 커뮤니티·인스타 광고 채널로 월 도달 20,000 → 무료체험 전환 4%(800) → 유료 전환 25%(200 신규 유료/월). 가격 월 29,000원, ARPU(가입자 1명당 평균 매출) 29,000원, 평균 유지 18개월 → LTV 약 52만원. CAC(고객 1명 확보 비용) 약 9만원 → LTV/CAC ≈ 5.8. 고정비 월 2,500만원 기준 BEP(손익분기점) 유료 약 860명 → 약 9개월 차 도달 전망. 시드 3억으로 약 14개월 런웨이(현 자금 버티는 개월 수) 확보. Out-of-scope(명시 제외): 프랜차이즈 본사 연동·결제 정산 대행·해외는 Phase 2로 분리(고객 검증 후)." ("Bottom-up: 카페 사장 커뮤니티·인스타 광고로 월 도달 20,000 → 무료체험 전환 4%(800) → 유료 전환 25%(월 신규 유료 200). 월 29,000원, ARPU 29,000원, 평균 유지 18개월 → LTV 약 52만원. CAC 약 9만원 → LTV/CAC ≈ 5.8. 고정비 월 2,500만원 → BEP 유료 약 860명, 약 9개월 차 도달. 시드 3억 → 약 14개월 런웨이. Out-of-scope: 프랜차이즈 본사 연동·정산 대행·해외 = Phase 2 분리.") — bottom-up funnel (채널→도달→전환→ARPU), reconciling unit economics (CAC/LTV·BEP·runway consistent), and explicit phase separation.

## C4 metric·financial measurability·coherence (지표·재무 측정가능성·정합성)

- **1/5**: "Increase revenue / raise satisfaction." ("매출을 늘린다 / 만족도를 높인다.") — no number, no measurement method, no decision criterion.
- **3/5**: "Reach 200 paying users in 6 months." ("6개월 내 유료 사용자 200명 달성.") — has a number but the measurement method/period/source is incomplete, and the input it rests on (e.g. conversion rate) is unlabeled.
- **5/5**: "Within 6 months of launch: weekly active rate (W4) 35%+ measured via 앰플리튜드 코호트, decision criterion = below 25% triggers an onboarding rework. Paid conversion target 3% (ASSUMPTION: 동종 B2B SaaS 무료→유료 전환 벤치마크 2~5% 중앙값), tracked in 스티비/믹스패널, reviewed monthly; 유료 200명·BEP 860명 진척률을 월간 대시보드로 점검." ("출시 6개월 내 주간 활성률(W4) 35% 이상(앰플리튜드 코호트 측정), 판단 기준 = 25% 미만이면 온보딩 재설계. 유료 전환 목표 3%(ASSUMPTION: 동종 B2B SaaS 무료→유료 전환 벤치마크 2~5% 중앙값), 믹스패널로 추적·월간 점검, 유료 200명·BEP 860명 진척률을 월간 대시보드로 확인.") — number + measurement tool + period + decision criterion, AND each assumed input is labeled `ASSUMPTION:` with its basis.

---

## Usage rules

- When scoring each criterion, **directly compare** the relevant part of the deliverable against the 1/3/5 anchors above. As in: "Is this persona sharper than the C1 3/5 anchor — quantified to frequency/cost like the 5/5 anchor?" / "Does C3 give a bottom-up funnel and reconciling unit economics like the 5/5 anchor, or only top-down sizing like the 1/5 anchor?"
- For in-between values (2/4), decide by which adjacent anchor it is closer to.
- If every axis looks ≥4, consider PASS only after applying rubric.md "Calibration Checkpoint" (VC-partner / competitor / customer lenses).
- When you find a new failure pattern (e.g., overrating a financial model that looks measurable but whose totals don't reconcile, or accepting a TAM with no SAM/SOM bottom-up basis), add a concrete counterexample to this file per SKILL.md tuning step (c) — and confirm the catch on rerun (step d).
- The anchors above are from the Korean business-plan domain and **must not be changed, only added to.**
