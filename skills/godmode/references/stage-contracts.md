# 스테이지 계약 — 입출력과 전달 컨텍스트

원칙: 형식 계약이 아니라 **파일 경로 + 프롬프트 컨텍스트** 전달(느슨한 결합). 하위 플러그인 무수정.

## A. 아이디어 → PRD (show-me-the-prd)

| | 내용 |
|---|---|
| 입력 | 아이디어 텍스트 ($ARGUMENTS) |
| 호출 | `/show-me-the-prd {아이디어}` — v0.6+ 선초안 흐름 그대로 |
| 출력 | `PRD/01_PRD.md`(말미 `## 가정 원장` 포함) ~ `04_PROJECT_SPEC.md`, `README.md` |
| 폴백 | 미설치 시: 설치 안내 + 기존 PRD 경로 접수 또는 questioning-policy §6 방식으로 최소 PRD(목표/수용 기준/가정 원장) 직접 작성 |

## B. PRD → 루프형 골 세팅 (goaljaby)

| | 내용 |
|---|---|
| 입력 | PRD 디렉토리 경로 + LOOP.md(리뷰어 설정) |
| 호출 | `/goaljaby {PRD 경로}` + 아래 컨텍스트를 호출 프롬프트에 포함 |
| 전달 컨텍스트 | ⑴ "PRD의 `## 가정 원장` blocking 항목을 VALIDATION 검증 항목에 반영하라" ⑵ "골 종료 조건은 VALIDATION 전체 통과 + blocking 가정 0 — goal-command와 VALIDATION에 명시하라" ⑶ recovery-ladder.md 원문 — "RECOVERY에 이 사다리·멈춤 매핑·재진입 규칙을 반영하라" |
| 게이트 | goaljaby Step 9 (사람 검토) — 우회 금지. 승인 시 goaljaby가 `/goal` 발사 |
| 출력 | VALIDATION/RECOVERY/PLAN/PROGRESS/goal-command.md + 골 시작 |
| 폴백 | 미설치 시: recovery-ladder.md + PRD로 goal-command 초안을 직접 작성, 사용자 승인 후 `/goal` |

## C. 골 내부 개선 회차 (godmode 자체 규칙)

| | 내용 |
|---|---|
| 검증 | VALIDATION.md 항목 실행 + LOOP.md의 blocking 가정 대조 → 실패/미확정 목록 |
| 리뷰어 호출 | 실패 목록 + 가정 원장 + 관련 코드 경로를 컨텍스트로, 설정된 리뷰어를 있는 그대로 (reviewer-menu.md) |
| 개선 적용 | 리뷰 결과에서 태스크 추출(요약 표시) → 적용 → PROGRESS.md·LOOP.md 갱신 |
| 판정 | DoD 충족→done / 미달→사다리 단계로 재진입 / 구조적 갭→gate_pending |

## D. 완료

| | 내용 |
|---|---|
| 조건 | VALIDATION 전체 통과 + blocking 가정 0 |
| 출력 | LOOP.md status done + 보고(총 회차/판정 추이/사용 리뷰어/잔여 non-blocking 가정/다음 제안) |
