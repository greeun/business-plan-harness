# Business Plan Harness (사업계획 하니스)

Anthropic의 *[Harness Design for Long-Running Application Development](https://www.anthropic.com/engineering/harness-design-long-running-apps)* 아티클에서 제시한 **Planner → Generator → Evaluator** 3-에이전트 하니스 패턴을 사업계획서 작성 도메인에 적용한 Claude Code 스킬입니다.

> **[English README](README.md)**

---

## 왜 만들었나

AI에게 "사업계획서 써줘"라고 하면 항상 같은 결과가 나옵니다:

- 어떤 주제든 동일한 템플릿
- 출처 없는 시장 규모 숫자
- 표면적 경쟁사 나열
- 실행 가능한 첫 번째 행동 없음
- **자기가 쓴 글을 자기가 "좋다"고 평가**

근본 원인은 Anthropic이 코드 도메인에서 밝혀낸 **자기 평가(self-evaluation)의 한계**와 동일합니다. 사업계획서는 코드보다 더 주관적이어서 이 문제가 더 심각합니다.

이 스킬은 **생성과 평가를 분리**하여 해결합니다.

---

## 아키텍처

```
                     ┌─────────────┐
                     │   Planner   │
                     │  spec.md    │
                     └──────┬──────┘
                            │
              ┌─────────────▼─────────────┐
              │       Sprint Loop         │
              │                           │
              │  Generator ──► 작성       │
              │       │                   │
              │       ▼                   │
              │  Evaluator ──► 평가       │
              │       │                   │
              │   통과 ╱╲ 실패 ──► 수정   │
              │       ╱  ╲                │
              │      ▼    └───────────────│
              │  다음 Sprint              │
              └───────────┬───────────────┘
                          │
                          ▼
                   final_plan.md
```

### 세 가지 역할

| 역할 | 입력 | 출력 | 규칙 |
|------|------|------|------|
| **Planner** | 사업 주제 + 제약조건 | `spec.md` (Sprint 계약 포함) | 모호함을 명시적으로 해소; 제네릭 템플릿 금지 |
| **Generator** | `spec.md` + 현재 Sprint 계약 | Sprint 섹션 + `handoff.md` | 자기 평가 금지 |
| **Evaluator** | `spec.md` + 계약 + Sprint 결과물 | `critique.md` (1~5점 채점) | 발견한 문제를 축소하거나 무시 금지 |

### 4대 평가 기준

| 기준 | 체크 항목 | 가중치 |
|------|----------|--------|
| **전략적 일관성** | 문제, 솔루션, 고객, 모델, GTM이 서로 강화하는가 | 높음 |
| **독창성 & 통찰** | 이 주제에 대한 비자명한 판단인가, 상투어구가 아닌가 | 높음 |
| **정밀성 & 엄밀성** | 숫자가 일관되고, 출처가 있거나 `ASSUMPTION:` 표기되는가 | 보통 |
| **실행 가능성** | 창업팀이 다음 주에 행동할 수 있는 수준인가 | 보통 |

모든 기준이 **4점 이상**이고 미해결 블로커가 없어야 Sprint가 통과됩니다.

---

## Sprint 시퀀스

| # | Sprint | 산출물 |
|---|--------|--------|
| 1 | Discovery | 문제, 고객, 고통 증거, 비교 대상 |
| 2 | Value & Positioning | 가치 제안, 차별화, 진입 쐐기 |
| 3 | Business Model | 가격, 단위경제학, 수익 모델 |
| 4 | GTM | 획득 채널, 첫 100명 고객 |
| 5 | Operations & Team | 빌드 vs 바이, 조직, 채용 |
| 6 | Financials & Milestones | 18개월 계획, 런웨이, KPI |
| 7 | Risk & Kill-Criteria | 중단 기준과 시점 |
| 8 | Consolidation | 통과한 Sprint를 `final_plan.md`로 조립 |

---

## 산출물

```
business-plan/
├── spec.md                    # 전체 사양서 + Sprint 계약
├── plan.md                    # Sprint별 섹션 누적
├── critique.md                # Evaluator 평가 이력
├── handoff.md                 # Sprint 경계 인수인계 문서
├── final_plan.md              # 최종 조립본
└── assumptions_and_risks.md   # 검증되지 않은 가정 전체 목록
```

---

## 핵심 메커니즘

### Context Anxiety 방지

모델은 문맥이 차오르면 뒤쪽 섹션을 성급하게 마무리합니다. 이 스킬은 각 Sprint 경계에서 `handoff.md`를 작성하고 context를 리셋하여, 재무/리스크 섹션의 품질을 시장 분석 섹션과 동등하게 유지합니다.

### Sprint 계약

작성 시작 전에 Planner와 Generator가 수용 기준에 합의합니다. Evaluator는 이 기준에 대해 채점합니다 — 막연한 인상 평가가 아닙니다.

### 가정 추적

모든 숫자는 출처를 밝히거나 `ASSUMPTION:` 라벨을 달아야 합니다. 최종 단계에서 모든 가정이 `assumptions_and_risks.md`로 수집됩니다 — 일반 AI 사업계획서에는 없는 파일입니다.

---

## 설치

### Claude Code 스킬로 설치

```bash
# skills 디렉토리에 클론
git clone https://github.com/<your-org>/business-plan-harness.git \
  ~/.claude/skills/business-plan-harness
```

### 또는 기존 클론에서 심볼릭 링크

```bash
ln -s /path/to/business-plan-harness ~/.claude/skills/business-plan-harness
```

---

## 사용법

Claude Code에서 아래처럼 말하면 자동 발동됩니다:

```
"사업계획 써줘"
"비즈니스 플랜 작성해줘 — 주제는 반려동물 헬스케어 구독 서비스"
"사업 아이템 검증해줘: AI 기반 과외 플랫폼"
"창업 기획서 만들어줘"
"draft a business plan for vertical SaaS for dental clinics"
```

---

## 방법론 출처

이 스킬은 아래 아티클의 충실한 적용입니다:

- **아티클:** *Harness Design for Long-Running Application Development*
- **저자:** Prithvi Rajasekaran, Anthropic Labs
- **발행일:** 2026년 3월 24일

원문의 모든 원칙, 실패 모드, 평가 기준, 반복 패턴, 교훈이 SKILL.md의 Source Reference 섹션에 보존되어 있으며, Operational Mapping 표를 통해 사업계획 도메인으로 매핑됩니다.

---

## 라이선스

MIT
