---
name: godmode
description: >
  Goal-driven pipeline loop orchestrator — routes idea→PRD(show-me-the-prd)→goal setup(goaljaby)→/goal
  execution, then keeps iterating verify→review→improve cycles INSIDE the goal until the documented
  Definition of Done (all VALIDATION items pass AND zero blocking assumptions) is met. Never stops early:
  recovery ladder (retry→change approach with deep reviewer→re-plan gate) plus driven guard re-enter the
  loop whenever it stalls. Reviewers are selectable and invoked as-is: self-review (default) /
  insane-review / codex / gjc. Korean triggers: "갓모드", "루프 돌려줘", "완성될 때까지 돌려",
  "아이디어부터 완성까지", "파이프라인 돌려줘", "PRD부터 골까지". English triggers: "godmode",
  "run the loop", "loop until done", "idea to done pipeline".
---

# godmode — 문서 정의 완성까지 골로 도는 파이프라인 루프

## 핵심 개념

- **DoD (완성 정의)** = VALIDATION 전체 통과 **AND** blocking 가정 0. 문서가 정의하고, 루프가 도달한다.
- **엔진 = /goal.** 개선 회차는 골 내부의 반복 단계다. godmode는 골을 세팅하고(goaljaby 경유),
  멈추면 다시 걸고(드리븐), 회차를 기록한다(LOOP.md).
- **오케스트레이션만.** 하위 플러그인·리뷰어를 있는 그대로 호출한다. 전달은 경로+프롬프트 컨텍스트.
- **한방 완성이 아니라 미세 루프 반복 고도화.** 각 단계는 "시작하기에 충분한" 수준으로 빠르게
  통과시키고, 루프가 품질을 조인다. (근거: gjc deep-interview 분석 + Draft-First A/B 검증)

실행 절차는 `commands/godmode.md`가 지시서다. 이 문서는 규칙 상세와 LOOP.md 포맷을 정의한다.

## 스테이지 계약

Read `references/stage-contracts.md` — 스테이지별 입출력(경로·전달 컨텍스트 문구).

## 리뷰어

Read `references/reviewer-menu.md` — 감지·선택지 구성·호출 방법. 원칙: **있는 그대로 호출, 녹이지 않음.**
선택은 골 시작 전 루프 설정 카드에서 확정(자율 주행 중 카드 UI 불가). 골 내 기본 = 셀프리뷰.

## 리커버리 사다리 + 드리븐 가드

Read `references/recovery-ladder.md` — goaljaby에 전달해 RECOVERY.md에 새겨지는 원문.
요지: 1단 재시도 → 2단 접근 변경(딥 리뷰어) → 3단 재계획(사용자 게이트 `gate_pending`).
멈춤 원인 매핑: 같은 에러 반복→2단 강제 / 도구·환경 실패→기록 후 우회 / 모호→기본값+가정 원장
기록 후 계속(질문 금지) / 완료 착각→검증 증거 요구. 정당한 멈춤 = DoD 충족 · gate_pending · 사용자 중단.

## LOOP.md 포맷 (프로젝트 루트, 저널 단일 파일)

```markdown
# godmode 저널: {골 이름}

- status: active | gate_pending | done | stopped
- dod: VALIDATION 전체 통과 + blocking 가정 0
- prd: {PRD 경로} / validation: {VALIDATION.md 경로}
- reviewer: self | insane-review | codex | gjc ({투입 시점: 사다리2단 | 마일스톤})
- redrive: none | loop
- blocking_assumptions_remaining: [번호 목록 — PRD 가정 원장 참조]

## 회차 {n} — {ISO 날짜}
- 검증: {통과 x/y} / 실패 항목: {목록}
- 사다리 단계: {1|2|3} / 리뷰어: {이번 회차 사용}
- 적용 태스크: {요약}
- 판정: {계속 | gate_pending: 질문 | done}
```

규칙: 회차마다 블록 1개 append. `.gjc`식 상태 디렉토리·상태 머신 금지 — 이 파일 하나가 resume 지점이다.
`gate_pending`일 때는 질문 내용을 판정 줄에 남겨 재개 시 그 질문부터 시작한다.

## 질문 경계 = 골 시작 (questioning-policy §1·§2·§6 상속)

- **골 시작 전: 물어볼 결정이 남아 있으면 모두 묻는다.** 콜당 ≤4문항 배칭, 콜 수 제한 없음.
  blocking 가정 미해소 상태로 골을 발사하지 않는다. 사실(§6b)은 조사, 결정만 질문.
- **골 시작 후: 질문 0.** 기본값 채택 + 가정 원장 기록 후 계속. 사다리 3단도 질문을 띄우지 않고
  골을 멈춰 `gate_pending`+질문 내용을 LOOP.md에 기록 — 질문은 사용자 재진입 시 제시한다.

## 실패 모드 (하지 말 것)

- DoD 미달인데 완료 선언 — 검증 증거 없이 done 금지 (드리븐 가드가 잡는 1순위)
- 같은 접근 3회 반복 — 2회 실패면 접근을 바꿔야 한다 (사다리 2단)
- 하위 플러그인 게이트 우회 — goaljaby Step 9는 반드시 사람이 통과시킨다
- 리뷰어 출력 파싱/계약 강제 — 프롬프트 컨텍스트로 주고받는다
- 자율 주행 중 사용자 호출 남발 — 질문은 gate_pending에서만
