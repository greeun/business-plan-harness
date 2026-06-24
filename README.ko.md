# 사업계획 하네스 (Business Plan Harness)

짧은 사업 아이디어를 투자급 **사업계획서**(또는 ~16종 산출물 풀패키지)로 만드는 Claude Code 스킬. **Planner → Generator → Evaluator** 3-에이전트 GAN 하네스. Anthropic의 *[Harness Design for Long-Running Application Development](https://www.anthropic.com/engineering/harness-design-long-running-apps)* (Prithvi Rajasekaran, 2026.3.24) 방법론 이식.

> **[English README](README.md)**

---

## 왜 필요한가

AI에 사업계획서를 시키면 매번 같은 템플릿이 나온다 — 출처 없는 시장규모, "거대 TAM의 1%" 식 톱다운 추정, 얄팍한 경쟁사 목록, 합이 안 맞는 낙관적 재무, 그리고 자기 산출물을 "훌륭함"으로 자평하는 모델.

근본 원인(Anthropic이 코드 도메인에서 규명)은 **자기평가 실패** — 생성자는 컴파일 에러나 테스트 실패처럼 반박해줄 게 없는 주관적 과제에서 자기 작업을 자신 있게 칭찬한다. 사업계획서는 코드보다 더 주관적이고, 재무는 더 희망회로를 탄다.

해법: **생성과 평가를 별도 에이전트로 분리**(각각 별도 `Agent` 호출 — GAN 누락편향 차단)하고, **리서치·재무를 통합 전에 게이트로 검증**.

---

## 구조

각 역할은 **별도 에이전트**로 디스패치되며 파일로만 소통. Full tier는 **4개 리서치 스프린트**를 돌리고, 각 스프린트마다 계약 협상 + Evaluator 게이트를 거친 뒤 계획을 통합한다.

```
  Planner ──► spec.md + sprint-playbook.md
     │
     ▼
  ┌─────────────── 4-스프린트 루프 (S1→S2→S3→S4) ───────────────┐
  │  S1 시장·경쟁·생태계              → research-s1.md          │
  │  S2 문제·고객·페르소나            → research-s2.md          │
  │  S3 비즈니스모델·유닛이코노믹스·재무 → research-s3.md         │
  │  S4 계획 통합 작성                → business-plan.md        │
  │                                                            │
  │  스프린트별:  계약 ─► Generator ─► Evaluator                │
  │              PASS ╱╲ FAIL ─► 수정 (cap 5–15)               │
  └────────────────────────────┬───────────────────────────────┘
                               ▼
        최종 통합 Evaluator 패스 (4축 + 프로브 + 8게이트)
                               ▼
        business-plan.md  (또는 docs/ 16종 패키지 + HTML)
```

> **원본 코드-하네스 대비 도메인 치환:** 사업계획서에서 모델이 가장 약한 축은 **비즈니스모델 현실성 + 재무 정합성**. 그래서 **S3 = 재무**(UX/화면 아님), 조건부 휴먼 체크포인트도 **재무 가정 검토**(와이어프레임 시각 검토 아님).

### 3개 역할

| 역할 | 입력 | 출력 | 규칙 |
|------|------|------|------|
| **Planner** | 사업 아이디어 + 제약 + 고정 모드 | `spec.md` + `sprint-playbook.md` | 모호성 해소; 범용 템플릿 금지 |
| **Generator** | `spec.md` + 현재 스프린트 계약 | 스프린트 산출물 + `generator_report.md` | 자기채점 금지; 계약 먼저 협상 |
| **Evaluator** | spec + 계약 + 산출물 | `critique.md` (1–5점) | 적대적; 정당한 이슈 묵살 금지 |

### 4개 평가축 — *Claude가 약한 곳에 가중*

| 평가축 | 검증 내용 | 가중 |
|--------|-----------|------|
| **C1 문제·시장 정의 명확성** | who-what-why + 구체 페르소나(상황/페인) | 1× |
| **C2 차별화·해자** | 실명 경쟁사 ≥1 대비 명시 비교 + 비교축 + 모방난이도 | **2×** |
| **C3 실행가능성·유닛이코노믹스** | 현실적 범위/단계 + 재무가 실제로 맞아떨어짐(톱다운 아닌 바텀업) | **2×** |
| **C4 지표·재무 측정가능성** | 모든 KPI/수치에 값+측정법; 가정 수치는 `ASSUMPTION:` 라벨 | 1× |

C2/C3에 2× 가중하는 이유: Claude는 C1/C4 섹션 골격은 기본적으로 잘 채운다 — 압력 없이 약한 건 **방어 가능한 차별점**과 **합이 맞는 재무모델**. 스프린트는 **모든 축 ≥ 4**(2×축은 4 미만 불가) + 전 게이트 통과 시에만 PASS.

### 8개 바이너리 게이트 (§8)

`G-a` 가치↔문제 추적 · `G-b` 고객 구체성 · `G-c` 차별화 비교 · `G-d` 지표·재무 측정성 + `ASSUMPTION:` 라벨 · `G-e` 범위·단계 분리 · `G-f` placeholder 0 · `G-g` 인포그래픽 적합 · **`G-h` 재무 정합성**(합 일치 + 머니룰 단일출처)

---

## 5개 출력 모드

활성화 시 STEP 1(필수 게이트)에서 선택:

| 모드 | 이름 | 핵심 |
|------|------|------|
| 1 | **Lean 사업계획** *(기본)* | 1-pager / BMC 중심, 바로 시작 가능 |
| 2 | **표준 사업계획서** | 서술형 풀: 시장 → BM → GTM → 운영 → 재무 → 리스크 |
| 3 | **투자유치용 IR** | 시장규모·3~5년 모델·유닛이코노믹스·투자요청 강조 |
| 4 | **정부지원·창업지원 양식형** | 표준 평가항목 구조(문제·실현가능성·성장성·팀·사업비) |
| 5 | **Full 패키지** | 5단계 **~16종 산출물** + 시각화 HTML 대시보드 |

---

## 산출물

**모드 1–4** → `business-plan/business-plan.md` (+ 선택적 HTML).

**모드 5** → 단일출처·상호추적 16종 번호 문서 패키지(`docs/`):

```
docs/
─ 진단·전략토대   00-business-model · 01-business-plan · 02-market-analysis ·
                 03-competition-ecosystem · 04-customer-persona
─ 정의·엣지       10-value-proposition · 11-product-roadmap · 12-pricing-strategy
─ GTM·운영·조직   20-gtm-strategy · 21-operations-plan · 22-team-org
─ 재무·투자       30-financial-model · 31-funding-plan · 32-risk-assessment ·
                 33-milestones-kpi
─ 가이드+HTML     INDEX.md (중요도/순서/역할별/최소세트) ·
                 index.html · NN-name.html · overview.html
```

> **단일 출처(Single source of truth):** 돈 규칙(매출/수수료/가격/유닛이코노믹스)은 `00-business-model`에서만 정의하고 하위 문서는 *참조만* — 재정의 금지(게이트 G-h가 강제).

하네스 중간 파일(`spec.md`·`sprint-playbook.md`·`sprint_contract.md`·`research-s*.md`·`critique.md`·`handoff.md`·`assumptions_and_risks.md`)은 같은 작업 트리에 위치.

### 인포그래픽 우선

구조를 가진 모든 섹션은 **개념을 가장 잘 담는 시각물**로 전달 — BMC, 머니플로우(Sankey), 유닛이코노믹스 차트, TAM/SAM/SOM 동심원, 생태계맵, 경쟁 매트릭스, AARRR 퍼널, 캡테이블, 리스크 히트맵, 마일스톤 간트 — `.md`엔 ASCII, HTML엔 자체완결 **인라인 SVG/CSS**(Mermaid·CDN 없음, 오프라인 동작). 숫자를 표로만 두고 차트 없는 재무 문서는 G-g 실패.

---

## 스킬 구조

```
business-plan-harness/
├── SKILL.md                       # 오케스트레이터: 8단계 흐름, 게이트, 모델 가이드
└── references/
    ├── source-article.md          # 원문 전체 인벤토리 + 코드→사업 매핑
    ├── planner-prompt.md          # Planner 에이전트 프롬프트
    ├── generator-prompt.md        # Generator 에이전트 프롬프트(스프린트별)
    ├── evaluator-prompt.md        # Evaluator 에이전트 프롬프트(스프린트+최종)
    ├── rubric.md                  # 4축, 가중, 판정 로직
    ├── evaluator-calibration.md   # few-shot 1/3/5 채점 앵커
    ├── sprint-playbook.md         # 4스프린트 계획, 모드별 스코프
    ├── mode-templates.md          # 5개 모드 골격 + 모드5 16종 산출물
    ├── html-doc-template.md       # 문서별 가독 HTML + 허브
    └── html-visual-template.md    # 다이어그램 대시보드
```

---

## 핵심 메커니즘

- **GAN 역할 분리** — Generator·Evaluator를 별도 에이전트로 분리해 자기평가 편향(특히 낙관적 재무)을 구조적으로 차단.
- **스프린트 계약** — 작성 전, Generator·Evaluator가 관찰가능한 체크를 문서로 합의. Evaluator는 그 체크로 채점(막연한 인상 아님).
- **리서치·재무 게이팅** — S4는 S1–S3(시장·고객·재무)가 게이트를 통과한 뒤에만 통합. 약한 리서치 위에 쌓으면 차별화·실행가능성이 붕괴.
- **출처 또는 `ASSUMPTION:`** — 모든 수치는 출처 표기 또는 명시 라벨. 최종에 `assumptions_and_risks.md`로 집계(범용 AI 계획서엔 없는 파일).
- **재무 정합성(G-h)** — 매출 = 가격 × 수량; BEP·런웨이·현금흐름이 유닛이코노믹스와 일치; 투자요청액 = use-of-funds + 그게 사주는 런웨이.
- **재무 가정 휴먼 체크포인트** — 재무모델 포함 시, 창업자가 *입력 가정*(시장 점유·전환·CAC·런웨이)을 최종 PASS 전 검토; 계산 자체는 자동 채점.
- **컨텍스트 불안 방지** — 컨텍스트 압박 시 복구 경로는 구조화된 `handoff.md` + 새 에이전트, 컴팩션 금지(불안 상태를 보존하므로).

---

## 설치

```bash
# 스킬 디렉토리에 심링크
ln -s /path/to/business-plan-harness ~/.claude/skills/business-plan-harness
```

## 사용

Claude Code에서 아래처럼 말하면 자동 활성화되고 먼저 모드를 묻는다:

```
"사업계획서 써줘"
"AI 튜터링 플랫폼 사업계획서 풀패키지로 만들어줘"
"투자유치용 IR 자료 만들어줘 — 반려동물 헬스케어 구독 서비스"
"내 사업 아이디어 검증해줘: 치과 클리닉용 버티컬 SaaS"
```

---

## 방법론 출처

*Harness Design for Long-Running Application Development* (Prithvi Rajasekaran, Anthropic Labs, 2026.3.24)의 충실한 이식. 모든 원칙·실패모드·평가기준·반복패턴·교훈을 `references/source-article.md`에 보존하고, 명시적 Operational Mapping 표로 사업계획 도메인에 매핑.

## 라이선스

MIT
