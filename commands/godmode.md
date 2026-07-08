---
name: godmode
description: "아이디어→PRD→골→검증→개선을 문서 정의 완성까지 골로 계속 돌리는 루프 오케스트레이터"
argument-hint: "[아이디어 | PRD 경로 | 프로젝트 경로 — 비우면 현재 디렉토리 상태 감지]"
allowed-tools:
  - Read
  - Write
  - Glob
  - Grep
  - Bash
  - WebSearch
  - WebFetch
---

<!-- first-run setup: idempotent, non-blocking, self-skips after first run -->
**Step 0 — run once at the very start, before anything else:** run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" ask`. If its output starts with `STAR_ASK`, immediately call the **AskUserQuestion** tool once, with the question and options phrased **in the user's language**: prefer the current conversation's language if it is evident; otherwise fall back to the language code that follows `STAR_ASK` in the output (`ko`→Korean, `ja`→Japanese, `en`→English). Never default to Korean blindly.
- header: a short localized "GitHub Star" label
- question: ask whether they'd like to give this plugin (and the gptaku-plugins marketplace) a GitHub ⭐ to support it — note it is optional and every feature works either way
- options: exactly two — (1) yes, star it → then run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" star yes`; (2) no thanks → then run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" star no`

If the output is empty, just continue silently. (AskUserQuestion must NOT be in frontmatter allowed-tools.) Do not narrate beyond the question itself.

# /godmode Command

**문서로 정의된 완성(DoD)에 도달할 때까지 파이프라인을 골로서 계속 돌린다.**
DoD = VALIDATION 전체 통과 **AND** blocking 가정 0. 이 조건을 충족하기 전에는 루프가 끝나지 않는다.

입력: `$ARGUMENTS`

## 4대 원칙

1. **루프의 엔진은 /goal, 문서가 DoD다.** 개선 회차(검증→리뷰→개선→재검증)는 골 내부의 반복 단계다. 골 밖 수동 턴제 루프가 아니다.
2. **오케스트레이션만, 재구현 금지.** 하위 플러그인(show-me-the-prd, goaljaby)과 리뷰어(셀프리뷰/insane-review/codex/gjc)는 **있는 그대로 호출**한다. 어댑터·출력 계약 강제 없음. 전달은 파일 경로 + 프롬프트 컨텍스트뿐.
3. **승인 게이트 존중.** goaljaby Step 9(사람 검토) 등 하위 게이트를 절대 우회하지 않는다. 개선 적용도 요약을 보이고 진행한다.
4. **질문 경계 = 골 시작 (shared/questioning-policy.md §1·§2·§6):** **골 시작 전에는 물어볼 결정이 남아 있으면 모두 묻는다** — 카드는 콜당 ≤4문항으로 배치하되 콜 수 제한 없음. blocking 가정·방향 결정이 미해소인 채로 골을 발사하지 않는다(사실은 §6b대로 조사, 결정만 질문). **골 시작 후에는 질문 0** — 기본값+가정 원장 기록 후 계속하고, 사다리 3단은 질문을 띄우는 게 아니라 골을 멈추고 `gate_pending`으로 파킹한다(질문은 사용자가 돌아와 재진입할 때 제시).

## Turn 0: 상태 감지 (자동 — 유저에게 묻지 않음)

$ARGUMENTS와 현재 디렉토리를 다음 순서로 감지해 진입 스테이지를 정한다:

1. `LOOP.md` 존재 → **스테이지 C (resume)**
2. `VALIDATION.md`(또는 goaljaby 산출물 4종) 존재 → **스테이지 C (DoD 판정부터)**
3. `PRD/` 폴더(01_PRD.md) 존재 → **스테이지 B (골 세팅)**
4. 아무것도 없음(또는 $ARGUMENTS가 아이디어 텍스트) → **스테이지 A (PRD부터)**

의존 감지 (Glob/Bash, 유저에게 묻지 않음):
- `$HOME/.claude/plugins/cache/*/show-me-the-prd` / `*/goaljaby` / `*/insane-review`
- `command -v codex`, `command -v gjc`
감지 결과는 스테이지 라우팅과 리뷰어 선택지 구성에만 쓴다. 상세: Read `${CLAUDE_PLUGIN_ROOT}/skills/godmode/references/reviewer-menu.md`

## 스테이지 A: 아이디어 → PRD

- show-me-the-prd 설치됨 → `/show-me-the-prd {아이디어}`로 위임한다 (v0.6+ 선초안 흐름이 가정 원장까지 만들어준다). 완료 후 스테이지 B로.
- 미설치 → 설치 안내 한 줄 + 폴백: 기존 PRD 경로를 받거나, questioning-policy §6 선초안 방식으로 이 세션에서 직접 최소 PRD(목표/수용 기준/가정 원장)를 작성한다.

## 스테이지 B: PRD → 루프형 골 세팅

1. **루프 설정 질문 (골 전 — 물어볼 건 여기서 전부):**
   - 리뷰어: 감지된 것만 노출 — 셀프리뷰 (추천, 매 회차·비용 제로) / insane-review (깊은 회차, GPT Pro 구독 필요) / codex / gjc. 딥 리뷰어 선택 시 투입 시점(마일스톤 or 사다리 2단)도 옵션에 포함.
   - 주기 재드라이브 옵트인: 없음 (추천) / 세션 내 /loop
   - **PRD의 blocking 가정 전부** + 골 도중 결정이 필요해질 게 뻔한 항목 — 콜당 ≤4문항으로 배치해 **해소될 때까지** 묻는다. 골 시작 후에는 물을 수 없으므로 여기가 마지막 질문 기회다.
2. **`LOOP.md` 생성** — 포맷: Read `${CLAUDE_PLUGIN_ROOT}/skills/godmode/SKILL.md` §LOOP.md. 골 정의·DoD·리뷰어 설정·회차 0 기록.
3. **goaljaby 호출** — `/goaljaby {PRD 경로}`. 호출 시 다음을 **프롬프트 컨텍스트로** 전달한다 (goaljaby 문서 5종에 새겨지도록):
   - PRD `## 가정 원장` → VALIDATION 검증 항목으로 반영 요청
   - 골 종료 조건(= DoD: VALIDATION 전체 통과 + blocking 가정 0) → goal-command·VALIDATION에 명시 요청
   - 리커버리 사다리 + 멈춤 원인 매핑 + 개선 회차 재진입 규칙 → RECOVERY에 반영 요청. 원문: Read `${CLAUDE_PLUGIN_ROOT}/skills/godmode/references/recovery-ladder.md`
4. goaljaby Step 9 승인 게이트는 그대로 — 승인되면 goaljaby가 `/goal`을 발사하고 골이 시작된다. LOOP.md status를 `active`로.
- goaljaby 미설치 → 설치 안내 + 폴백: recovery-ladder.md와 PRD로 이 세션에서 goal-command 초안을 직접 작성해 사용자 승인 후 `/goal` 발사.

## 스테이지 C: 루프 진행 / 재개 (골 내부 반복)

**매 회차 (골 자율 주행 안에서):**
1. **검증**: VALIDATION.md 항목 실행 + LOOP.md의 잔여 blocking 가정 대조 → 실패/미확정 목록
2. **DoD 판정**: 전체 통과 + blocking 가정 0 → **완료** (LOOP.md status `done`, 회차 요약 보고, 잔여 non-blocking 가정 목록 노출)
3. 미달 → **리커버리 사다리 단계 판정** (LOOP.md 회차 기록 기준):
   - 1단 (단발 실패): 같은 접근으로 수정 → 재검증
   - 2단 (같은 항목 2회 연속): 접근 변경 — 설정된 딥 리뷰어 투입 또는 다른 각도 셀프리뷰. 실패 목록+가정 원장+관련 코드를 컨텍스트로 리뷰어를 **있는 그대로** 호출
   - 3단 (3회 연속 or 구조적 갭): LOOP.md에 `gate_pending`+질문 내용 기록하고 **골을 멈춘다** — 골 도중 질문을 띄우지 않는다. 방향 질문은 사용자가 돌아와 `/godmode`로 재진입할 때 제시
4. **개선 적용**: 리뷰 결과에서 태스크를 추려 적용, PROGRESS.md·LOOP.md 갱신 → 재검증(1번으로)

**드리븐 규칙 (조기 종료 금지):** DoD 미달 상태에서 턴/골을 끝내려는 자신을 발견하면 — 완료 선언 전에 반드시 VALIDATION 통과 증거를 확인하고, 증거가 없으면 사다리 단계로 재진입한다. 모호하면 질문 대신 기본값+가정 원장 기록 후 계속. 포기 대신 사다리 2단. 정당한 멈춤은 세 가지뿐: DoD 충족 / `gate_pending` / 사용자 명시 중단.

**Resume:** LOOP.md가 있으면 마지막 회차 블록에서 이어간다. `gate_pending`이면 그 질문부터.

## 완료 보고

DoD 충족 시: 총 회차 수 / 회차별 판정 추이 / 사용된 리뷰어 / 남은 non-blocking 가정 목록 / 다음 제안(릴리즈·리팩터 등) 을 간결히 보고한다.
