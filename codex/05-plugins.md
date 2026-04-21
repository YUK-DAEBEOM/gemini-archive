# 02. Codex Plugins 정리

작성일: 2026-04-21

## 한줄 요약

`Plugin`은 skill, 앱 연동, MCP 설정을 묶어서 설치 가능한 형태로 배포하는 패키지다.

## Plugin이란?

OpenAI 공식 문서 기준 plugin은 reusable workflow를 Codex에 설치 가능한 방식으로 배포하는 단위다.

공식 문서 핵심:

- skills는 authoring format
- plugins는 installable distribution unit

즉, skill은 "만드는 형식", plugin은 "배포하는 형식"으로 보면 된다.

## Plugin이 담을 수 있는 것

공식 문서 기준 plugin은 다음을 포함할 수 있다.

- Skills
- Apps
- MCP servers

즉, plugin은 단순 설명서 묶음이 아니라:

- Codex의 작업 지침
- 외부 서비스 연결
- 추가 툴 접근 설정

을 한 번에 묶는 설치 패키지다.

## 공식 문서 예시

OpenAI 문서에 나온 예시:

- Gmail plugin
  - Gmail 읽기/관리
- Google Drive plugin
  - Drive, Docs, Sheets, Slides 작업
- Slack plugin
  - 채널 요약, 답장 초안 작성

OpenAI 공식 블로그 2026-04-16 글에서는 90개 이상의 추가 plugin이 언급되며, 예시로 다음이 나온다.

- Atlassian Rovo
- CircleCI
- CodeRabbit
- GitLab Issues
- Microsoft Suite
- Neon by Databricks
- Remotion
- Render
- Superpowers

이 목록은 시간이 지나며 달라질 수 있다.

## Plugin 설치 방법

### Codex app

- Plugin Directory에서 browse 후 설치

### Codex CLI

```text
codex
/plugins
```

설치 흐름:

1. plugin 검색 또는 browse
2. 상세 페이지 열기
3. `Install plugin` 선택
4. 필요한 경우 외부 앱 인증
5. 새 thread에서 plugin 사용 요청

## Plugin을 설치한 뒤 어떻게 쓰나

공식 문서 설명 방식은 두 가지로 이해하면 된다.

1. 결과 중심으로 자연어 요청
   - 예: "오늘 안 읽은 Gmail thread를 요약해줘"
2. 특정 plugin을 염두에 둔 작업 요청
   - 필요 시 해당 plugin을 의식해서 지시

즉, plugin 설치 후에는 Codex가 적절한 연결과 skill을 동원해 작업을 수행한다.

## Plugin 구조

plugin의 필수 진입점은 아래 manifest다.

```text
.codex-plugin/plugin.json
```

공식 문서의 최소 예시는 이런 형태다.

```json
{
  "name": "my-first-plugin",
  "version": "1.0.0",
  "description": "Reusable greeting workflow",
  "skills": "./skills/"
}
```

즉, plugin은 하나 이상의 skill을 담을 수 있고, 거기에 앱 연동과 MCP 구성을 더 얹을 수 있다.

## Plugin 만들기

공식 문서는 먼저 `$plugin-creator`를 쓰라고 권장한다.

```text
$plugin-creator
```

이 built-in skill이 도와주는 것:

- `.codex-plugin/plugin.json` 생성
- local marketplace entry 생성
- 테스트 가능한 plugin 골격 구성

## Marketplace란?

공식 문서 기준 marketplace는 plugin 목록이 담긴 JSON 카탈로그다.

주요 위치:

- 저장소 범위: `$REPO_ROOT/.agents/plugins/marketplace.json`
- 개인 범위: `~/.agents/plugins/marketplace.json`

이 말은 곧:

- 팀 전용 curated plugin 모음
- 개인 전용 plugin 모음

을 따로 구성할 수 있다는 뜻이다.

## 언제 plugin을 쓰면 좋은가

plugin이 잘 맞는 경우:

- skill을 팀에 배포하고 싶다
- 여러 skill을 하나로 묶고 싶다
- Slack, Gmail, Drive 같은 앱 연결도 같이 배포하고 싶다
- MCP 설정까지 패키지로 묶고 싶다
- plugin directory / marketplace에서 설치 가능한 형태가 필요하다

예시:

- 문서 요약 + Slack 공유 workflow
- PR triage + 이슈 동기화 workflow
- 디자인 컨텍스트 + Figma + 이미지 생성 workflow

## Skill과 plugin의 차이

- Skill
  - 작업 방법 자체
  - 로컬 작성과 실험에 적합
- Plugin
  - 설치 가능한 패키지
  - 배포와 팀 공유에 적합

실무적으로는 보통:

1. skill로 먼저 만든다
2. 안정화되면 plugin으로 묶는다

## 공식 자료

- Plugins
  - https://developers.openai.com/codex/plugins

- Build plugins
  - https://developers.openai.com/codex/plugins/build

- OpenAI 블로그: Codex for (almost) everything
  - https://openai.com/index/codex-for-almost-everything/
