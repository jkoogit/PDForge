# Codex 작업 절차

## 문서 ID

- 정책 ID: `POL-CODEX-001`
- 관련 작업 ID: `TASK-MVP-001`

## 목적

이 문서는 Codex를 사용해 PDForge 작업을 진행할 때 브랜치, Issue, PR, 문서 기록을 어떤 순서로 관리할지 정리합니다.

## 참조 문서

- [GitHub 운영 프로세스](02-github-운영-프로세스.md)
- [Issue와 PR 작성 가이드](03-issue-pr-작성-가이드.md)
- [OpenAI Help: Using Codex with your ChatGPT plan](https://help.openai.com/en/articles/11369540-using-codex-with-your-chatgpt-plan.pdf)
- [OpenAI Help: Enterprise admin getting started guide for Codex](https://help.openai.com/en/articles/11390924-placeholder)

## Codex 작업 시작 기준

Codex는 연결된 GitHub 저장소를 격리된 샌드박스 환경에서 받아 작업합니다. 공식 도움말 기준으로 Codex는 작업 결과를 검토하거나 병합하거나 로컬로 내려받을 수 있는 형태로 만들 수 있고, GitHub에 연결된 사용자는 작업 결과 PR을 push할 수 있습니다.

PDForge에서는 다음 기준을 사용합니다.

1. Codex 작업은 가능하면 `dev` 브랜치를 기준으로 시작합니다.
2. 실수로 `main` 브랜치에서 채팅이나 작업을 시작했다면 바로 구현하지 않고 기준 브랜치를 `dev`로 바꿀 수 있는지 확인합니다.
3. 이미 `main` 기준으로 변경이 만들어졌다면 그대로 `main`에 PR하지 않고, 변경 내용을 `dev` 기준 작업 브랜치로 옮깁니다.
4. Codex가 PR 제목과 본문을 작성할 때는 한글로 작성합니다.
5. PR 본문 첫 줄에는 `작업브랜치명: 브랜치명`을 표시합니다.
6. PR 대상 브랜치는 항상 `dev`입니다.
7. `stg`, `main`으로 직접 PR하거나 자동 머지하지 않습니다.

## 작업 브랜치명 규칙

브랜치명은 작업 내용을 사람이 바로 이해할 수 있게 작성합니다.

```text
날짜_에이전트_작업내용
```

예시:

```text
20260530_Codex_문서정책정리
20260530_Copilot_MVP요건보완
20260530_Gemini_PDF뷰어설계
20260530_Claude_회고문서정리
```

### 에이전트 구분값

| 에이전트 | 표기 |
| --- | --- |
| Codex | `Codex` |
| Gemini | `Gemini` |
| Copilot | `Copilot` |
| Claude | `Claude` |

## Issue 문서 관리 규칙

Issue 문서는 `docs/07-이슈`에 관리합니다.

파일명 형식:

```text
[이슈번호](작업ID-순번)_배경_작업내용.md
```

아직 GitHub Issue 번호가 없으면 이슈번호 자리는 비워두고 작업 ID와 순번을 먼저 기록합니다.

예시:

```text
[](TASK-MVP-001-1)_AI주도개발준비_문서정책정리.md
[12](TASK-MVP-001-1)_AI주도개발준비_문서정책정리.md
```

## PR 문서 관리 규칙

PR 문서는 `docs/08-PR`에 관리합니다.

파일명 형식:

```text
[PR번호](이슈번호)_에이전트_작업내용.md
```

아직 PR 번호나 Issue 번호가 없으면 해당 위치를 빈칸으로 유지합니다.

예시:

```text
[]()_Codex_문서정책정리.md
[5](12)_Codex_문서정책정리.md
```

## Codex 작업 절차

| 순서 | 단계 | 담당 | 산출물 | 비고 |
| --- | --- | --- | --- | --- |
| 1 | 작업 ID 확인 | 사람 | 일정 문서의 작업 ID | 예: `TASK-MVP-001` |
| 2 | Issue 작성 | 사람 또는 에이전트 | GitHub Issue, `docs/07-이슈` 문서 | Issue 번호가 없으면 빈칸 유지 |
| 3 | Codex 작업 시작 | 사람 | Codex 작업 채팅 | 기준 브랜치는 `dev` 권장 |
| 4 | 작업 브랜치 생성 | Codex | `날짜_에이전트_작업내용` 브랜치 | 샌드박스/원격 작업 브랜치 |
| 5 | 문서 또는 코드 수정 | Codex | 변경 파일 | 문서 작업은 테스트/린터 미실행 |
| 6 | PR 내용 작성 | Codex | 한글 PR 제목/본문, `docs/08-PR` 문서 | 첫 줄에 작업브랜치명, 관련 Issue 포함 |
| 7 | PR 생성 | 사람 또는 Codex | 대상 `dev` PR | `stg`, `main` 금지 |
| 8 | 검토와 수정 | 사람 + 에이전트 | 리뷰 반영 커밋 | 필요 시 후속 PR |
| 9 | 병합 | 사람 | `dev` 반영 | 보호 브랜치 정책 준수 |
| 10 | 회고 작성 | 사람 또는 에이전트 | `docs/05-회고` | 작업 이력 포함 |

## dev, stg, main 승격 흐름 검토

사용자가 예상한 흐름은 다음과 같습니다.

1. Issue 만들기
2. 작업 브랜치를 `dev` 브랜치에 머지
3. 개발 서버 테스트
4. `dev` 브랜치 PR 작성 후 `stg` 브랜치에 머지
5. 검증 서버 테스트
6. `stg` 브랜치 PR 작성 후 `main` 브랜치에 머지

PDForge의 현재 정책은 다음과 같이 정리합니다.

| 단계 | Codex 자동 처리 가능성 | PDForge 정책 |
| --- | --- | --- |
| Issue 만들기 | 내용을 작성하는 것은 가능하지만 실제 생성은 GitHub 권한과 연동이 필요 | 사람 승인 후 생성 |
| 작업 브랜치 → dev PR | Codex가 PR 내용을 만들고 GitHub 연결 시 PR push를 도울 수 있음 | 허용, 대상은 `dev` |
| 개발 서버 테스트 | 명령이 준비되면 Codex가 실행 결과를 기록할 수 있음 | 코드 변경 시 필수 |
| dev → stg PR | 기술적으로 자동화 도구로 가능할 수 있음 | 현재 Codex 자동 PR/머지 금지 |
| 검증 서버 테스트 | CI/CD나 별도 환경 필요 | 하네스 확정 후 운영 |
| stg → main PR | 기술적으로 자동화 도구로 가능할 수 있음 | 현재 Codex 자동 PR/머지 금지 |

## 4, 5, 6단계 자동화 판단

Codex는 GitHub와 연결된 환경에서 작업 결과를 PR로 만들거나 PR 작성을 도울 수 있습니다. 하지만 OpenAI 공식 문서에서도 작업 결과는 검토, 병합, 내려받기 대상으로 설명되며, 보안 관련 문서에서도 패치는 사람 검토 후 PR로 전환되는 흐름을 전제로 합니다.

따라서 PDForge에서는 다음처럼 운영합니다.

1. Codex는 `dev` 대상 작업 PR 작성까지만 맡깁니다.
2. `dev → stg`, `stg → main` 승격 PR은 Codex가 제목/본문 초안을 작성할 수는 있지만 자동 생성·자동 머지는 하지 않습니다.
3. 승격 PR과 머지는 GitHub branch protection, CI 통과, 사람 승인 이후 진행합니다.
4. 자동화가 필요하면 Codex가 아니라 GitHub Actions와 보호 브랜치 규칙으로 별도 설계합니다.
5. 현재 사용자 지침상 PR 대상 브랜치는 `dev`이므로 이 저장소에서 Codex가 `stg`, `main` 대상 PR을 만드는 작업은 금지합니다.

## 작업 이력

| 작업일시 | 작업 에이전트 | 작성자 | 내용 한 줄 요약 |
| --- | --- | --- | --- |
| 2026-05-30 | Codex | Codex | PR 본문 첫 줄 작업브랜치명 표기 규칙 추가 |
