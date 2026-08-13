# 서브에이전트 위임 프롬프트 템플릿

큰 기능 영역(대략 코드 1000줄 이상, 파일 5개 이상)을 조사할 때 이 템플릿을 채워서 `Agent` 툴(subagent_type: Explore, run_in_background: false)로 보낸다. 결과가 바로 필요하므로 foreground로 돌린다. 논리적으로 겹치지 않는 2~3그룹으로 쪼개서 한 메시지에 병렬로 보낸다.

```
Read these files in the repo at <경로> (<제품/기능 한 줄 설명>):

1. <파일 경로> (<줄 수> lines)
2. <파일 경로> (<줄 수> lines)
...
(파일이 500줄 넘으면: "skim structure first, then focus on interactive/state logic")

I am building a manual QA functional test-case (TC) spreadsheet — ONLY hand-testable
UI behaviors, explicitly EXCLUDING performance/load/stress/concurrency testing
(separate future workstream, out of scope here). I need STRUCTURED FACTS I can turn
into TC rows myself — do not write TC rows yourself, just report facts.

For each distinct interactive behavior you find, report:
- A short name for the scenario
- Precondition
- The user action that triggers it
- The exact observable feedback (exact button/label text, exact message text,
  disabled states, loading states, toasts, redirects)
- File:line reference

Specifically look for and report on:
- <이 기능에 맞는 체크리스트, 예: 필터링/정렬/검색/선택/벌크액션/페이지네이션/
  행별액션/빈상태/생성진입점/드래그앤드롭/뷰토글 등>

Also check: does any of these files have direct or nearby automated test coverage
(*.test.ts / *.spec.ts importing them, or e2e specs mentioning this area)? Report
yes/no per file with basis (import path match, not just filename guess).

Report as a structured list grouped by feature area. Keep it factual and compact —
under <1600~1800> words. This is read-only analysis, do not modify any files.
```

## 체크리스트 항목 고르는 법

기능 유형별로 물어볼 것이 다르다. 목록/테이블류라면:

- 정렬(정렬 가능한 컬럼? 기본 정렬?)
- 필터(차원이 몇 개? 멀티선택? 해제는 어떻게?)
- 검색(디바운스? 매칭 필드? 빈 결과 문구?)
- 선택(단일/전체선택, 페이지 한정인지 전체인지)
- 벌크 액션(삭제 외에 뭐가 더 있는지, 확인 플로우, 로딩/성공/실패 피드백)
- 페이지네이션/무한스크롤
- 행별 액션(호버 메뉴, disabled 조건)
- 빈 상태(완전 빈 상태 vs 필터로 인한 0건)
- 생성 진입점(버튼 → 모달인지 페이지 이동인지)

폼/모달류라면: 필드별 유효성, 에러 문구, 버튼 disabled 조건, 로딩 상태, 성공/실패 시 동작.

승인/워크플로우류라면: 상태 전이 목록, 각 상태에서 가능한 액션, 되돌리기/취소 가능 여부, 잠금 조건(어떤 상태에서 편집 불가한지).

## 재검증 모드 (0.3.0류 버전업 이후 diff 기반 재조사)

최초 저작 템플릿(위)은 "이 영역을 처음부터 통째로 조사해서 보고하라"는 전제다. 커버리지 맵 기준으로 특정 탭의 커버 경로에 변경이 있었다고 확인된 뒤 재조사할 때는 이 전제가 안 맞는다 — 안 바뀐 부분까지 매번 풀분량(1600~1800단어)으로 재보고하면 낭비다. 대신:

1. 서브에이전트를 부르기 **전에** 메인 에이전트가 해당 탭의 기존 TC 행을 `sheets_get_values`로 읽어온다.
2. `git log <기준커밋>..<새커밋> --stat -- <커버 경로>`로 실제 변경된 파일만 추린다.
3. 아래 템플릿으로 프롬프트를 구성한다 — 핵심 차이는 "기존 사실을 먼저 주고 델타만 보고하라"는 지시와, 훨씬 낮은 응답 상한:

```
Read these changed files in the repo at <경로> (<제품/기능 한 줄 설명>). These files
changed between commit <기준커밋> and <새커밋> — a prior TC audit already covered this
area as of <기준커밋>, so I only need what's DIFFERENT now.

Changed files:
1. <파일 경로>
2. <파일 경로>
...

Here is the existing TC sheet content for this area (from the prior audit) as ground
truth — treat every row as still true unless the current code contradicts it:

<기존 TC 행들을 압축된 표 형태로 붙여넣기 — # / 테스트 케이스 / 기대 피드백 컬럼만 있어도 충분>

For each existing row, report ONLY if something changed:
- "STILL TRUE" rows do not need to be listed individually — just confirm the batch
  is unaffected if nothing changed in the files that back it
- "CHANGED" — old expected feedback vs new exact text/condition, file:line
- "REMOVED" — behavior no longer exists in the code
- "NEW" — a new interactive behavior not covered by any existing row (report with the
  same structure as first-pass authoring: scenario/precondition/action/exact feedback)

Also re-confirm automated test coverage status only if it plausibly changed (new
test files added near these paths) — don't re-grep the whole repo if nothing suggests
it would differ from the prior finding.

Keep it factual and compact — under 500 words unless the diff is unusually large.
This is read-only analysis, do not modify any files.
```

이 방식이면 안 바뀐 탭은 아예 서브에이전트를 안 부르고 건너뛰고, 바뀐 탭도 "전체 재조사"가 아니라 "델타 확인"만 하게 되어 토큰 소모가 크게 줄어든다.
