# 리뷰어 메뉴 — 감지·선택·호출

원칙: **있는 그대로 호출, 녹이지 않는다.** 어댑터 레이어·출력 형식 계약 없음. 컨텍스트(검증 실패
목록 + 가정 원장 + 관련 코드 경로)를 프롬프트에 얹어 호출하고, 응답을 그대로 읽어 태스크를 추린다.

## 감지 (Turn 0, 유저에게 묻지 않음)

```bash
ls $HOME/.claude/plugins/cache/*/insane-review 2>/dev/null   # insane-review 설치 여부
command -v codex >/dev/null 2>&1 && codex --version          # Codex CLI
command -v gjc   >/dev/null 2>&1 && gjc --version            # gajae-code CLI
```

감지된 것만 루프 설정 카드의 선택지로 노출한다. 아무것도 없으면 카드에서 리뷰어 문항 생략(셀프리뷰 고정).

## 선택지 구성 (루프 설정 카드 — 골 시작 전 1회 확정)

| 옵션 | 설명(카드 description에 쓸 내용) | 투입 방식 |
|---|---|---|
| 셀프리뷰 (추천) | 현 세션 Claude가 즉시 검토. 비용 제로 — 미세 루프는 싸야 자주 돈다 | 매 회차 기본 |
| insane-review | GPT-5.5 Pro 웹 교차진단 (ChatGPT Pro 구독 필요). 같은 폴더 리뷰가 한 ChatGPT 프로젝트에 쌓여 회차 간 맥락 누적. 느림 | 사다리 2단 또는 마일스톤 (카드에서 선택) |
| codex | Codex CLI 교차검토 (OpenAI 구독) | 사다리 2단 또는 마일스톤 |
| gjc | gajae-code 멀티모델 교차검토 | 사다리 2단 또는 마일스톤 |

선택 결과는 LOOP.md `reviewer:` 줄에 기록 — 자율 주행 중에는 카드 UI가 불가하므로 여기 기록이 유일한 설정원이다.

## 호출 방법 (있는 그대로)

- **셀프리뷰**: 현 세션에서 검토 턴 수행. 관점을 바꿔 재검토할 때는 실패 항목별 반박 시도(왜 이 수정이 틀렸을 수 있는가)로.
- **insane-review**: `/insane-review` 스킬 흐름 그대로 (repomix 패킹 → ChatGPT Pro → 회수). 정밀 진단이므로 풀 코드 — `--compress` 금지, `--force-answer-after` 금지 (스킬 자체 규칙).
- **codex**: `codex exec "{컨텍스트+질문}"` 비대화 모드.
- **gjc**: `gjc --print "{컨텍스트+질문}"` (프롬프트는 --print 뒤 positional).

CLI 두 종은 응답이 텍스트로 돌아온다 — 파싱하지 말고 읽어서 개선 태스크를 추린다.
호출 실패(미로그인·타임아웃 등) 시: LOOP.md에 기록하고 셀프리뷰로 폴백, 루프는 계속.
