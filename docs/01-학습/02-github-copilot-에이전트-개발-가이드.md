# GitHub Copilot 에이전트 개발 가이드

## 참고 자료

이 문서는 사용자가 제공한 요즘IT 글과 GitHub 공식 문서를 함께 참고하여 PDForge에 맞게 정리한 학습 가이드입니다.

- 요즘IT: <https://yozm.wishket.com/magazine/detail/3750/>
- GitHub Copilot Agents: <https://github.com/features/copilot/agents>
- GitHub Copilot cloud agent 문서: <https://docs.github.com/copilot/how-tos/agents/copilot-coding-agent>
- GitHub Copilot custom agents 문서: <https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents>
- GitHub Copilot coding agent 튜토리얼: <https://docs.github.com/en/copilot/tutorials/coding-agent/improve-a-project>

## Copilot 에이전트로 할 수 있는 일

GitHub Copilot의 에이전트 기능은 단순 코드 자동완성보다 넓은 범위를 다룹니다. Issue를 기반으로 저장소를 조사하고, 계획을 세우고, 코드를 수정하고, PR을 만드는 흐름에 사용할 수 있습니다.

PDForge에서는 다음 용도로 활용합니다.

- 문서 기준으로 작업 범위 이해
- Issue의 완료 조건을 기준으로 구현
- 작은 단위의 PR 생성
- 테스트/린터 실행 결과 요약
- 리뷰 코멘트 반영

## Copilot과 Codex를 함께 쓰는 방식

| 도구 | 추천 용도 |
| --- | --- |
| GitHub Copilot | GitHub Issue 기반 작업, IDE 안에서의 코드 수정, PR 생성 보조 |
| Codex | 저장소 전체 맥락 분석, 문서 구조화, 복잡한 변경 작업, 명령 실행이 필요한 검증 |
| 사람 | 최종 의사결정, 제품 방향, PR 승인, 릴리스 판단 |

## custom agent 개념

custom agent는 특정 역할을 가진 에이전트 프로필입니다. 예를 들어 “기획 문서만 다루는 에이전트”, “테스트만 강화하는 에이전트”, “접근성 리뷰를 하는 에이전트”처럼 역할을 좁혀 만들 수 있습니다.

PDForge에서 나중에 만들 수 있는 에이전트 예시는 다음과 같습니다.

```text
pdforge-planner.agent.md      서비스 기획/요건 정리 담당
pdforge-architect.agent.md    구조 설계/기술 선택 담당
pdforge-builder.agent.md      구현 담당
pdforge-reviewer.agent.md     테스트/보안/접근성 검토 담당
pdforge-retro.agent.md        회고/문서 정리 담당
```

## 에이전트에게 일을 맡기는 기본 흐름

1. `docs` 문서에 기준을 작성합니다.
2. GitHub Issue를 한글로 작성합니다.
3. Issue에 참고 문서 링크와 완료 조건을 적습니다.
4. Copilot coding agent 또는 Codex에게 작업을 맡깁니다.
5. 에이전트가 만든 변경을 PR로 검토합니다.
6. 리뷰에서 빠진 부분을 다시 요청합니다.
7. 병합 후 회고 문서를 작성합니다.

## 좋은 Issue 예시

```md
# [기획] PDF 뷰어 첫 사용자 흐름 정의

## 배경

PDForge는 pdfrend와 독립된 제품으로 개발한다. 첫 버전에서 사용자가 PDF를 열고 읽는 핵심 흐름을 정의해야 한다.

## 목표

- 첫 사용자 흐름을 3단계 이내로 정리한다.
- 필수 기능과 제외 기능을 표로 분리한다.
- 결과를 docs/02-기획 문서에 반영한다.

## 참고 문서

- docs/01-학습/01-ai-주도-개발-준비과정.md
- docs/02-기획/01-서비스-요건-정리-방법.md

## 완료 조건

- 한글 문서가 추가된다.
- 코드 변경은 하지 않는다.
- 회고 문서가 추가된다.
```

## 에이전트 작업 요청 프롬프트 예시

```text
이 Issue를 기준으로 작업해줘.
문서 제목과 PR 제목/본문은 한글로 작성해줘.
PR 대상 브랜치는 dev로 해줘.
이번 작업은 문서 작업이므로 테스트와 린터는 실행하지 마.
작업 후 docs/05-회고에 회고 문서를 추가해줘.
```

## 주의할 점

- 에이전트는 문서를 읽을 수 있지만, 문서를 올렸다고 자동으로 작업하지 않습니다.
- Issue, 에이전트 할당, 채팅 요청 같은 명시적 시작점이 필요합니다.
- 에이전트가 만든 코드는 반드시 사람이 리뷰해야 합니다.
- 큰 작업보다 작은 작업을 여러 개 맡기는 방식이 안전합니다.
- 에이전트가 모르는 프로젝트 의사결정은 문서로 남겨야 다음 작업에서 재사용됩니다.

## 작업 이력

| 작업일시 | 작업 에이전트 | 작성자 | 내용 한 줄 요약 |
| --- | --- | --- | --- |
| 2026-05-30 | Codex | Codex | GitHub Copilot 에이전트 개발 가이드 문서 이력 등록 |
