# Codex의 Skills, Plugins, MCP 정리

분리본 (상세):

- [`04-skills.md`](./04-skills.md)
- [`05-plugins.md`](./05-plugins.md)
- [`06-mcp.md`](./06-mcp.md)

작성일: 2026-04-21

## 한줄 요약

- `Skill`은 Codex가 어떤 일을 어떻게 수행할지 알려주는 재사용 가능한 워크플로다.
- `Plugin`은 그런 skill들과 앱 연동, MCP 설정을 설치 가능한 패키지로 묶은 배포 단위다.
- `MCP`는 Codex가 외부 도구와 문맥에 접근하게 해주는 연결 규약이다.

즉, 아주 단순하게 보면:

- Skill = 작업 방법
- Plugin = 설치 패키지
- MCP = 외부 도구 연결 방식

## 1. Skill이란?

OpenAI 공식 문서 기준 skill은 Codex에 작업별 전문성을 추가하는 단위다.  
instructions, references, optional scripts를 묶어서 Codex가 특정 작업을 더 안정적으로 수행하게 만든다.

공식 문서 핵심:

- skill은 reusable workflow의 authoring format
- Codex CLI, IDE extension, Codex app에서 사용 가능
- `SKILL.md`가 핵심 파일
- 필요하면 `scripts/`, `references/`, `assets/`, `agents/openai.yaml`를 함께 둘 수 있음

### Skill의 기본 구조

```text
my-skill/
  SKILL.md
  scripts/
  references/
  assets/
  agents/
    openai.yaml
```

### Skill이 실행되는 방식

Codex는 skill을 두 방식으로 쓴다.

1. 명시적 호출
   CLI/IDE에서 `/skills`를 쓰거나 `$skill-name` 형태로 직접 호출
2. 암묵적 호출
   사용자의 요청이 skill의 `description`과 맞으면 Codex가 알아서 선택

즉, skill 설명을 정확하게 써야 자동 트리거도 잘 된다.

### Skill은 어디에 두나

공식 문서 기준 Codex는 여러 위치의 skill을 읽는다.

- 저장소 범위: `.agents/skills`
- 사용자 범위: `~/.agents/skills`
- 관리자 범위: `/etc/codex/skills`
- 시스템 범위: Codex가 기본 제공하는 built-in skills

### 내장된 예시

공식 문서에 직접 언급된 built-in 또는 기본 제공 예시는 다음과 같다.

- `$skill-creator`
  - 새 skill 골격 생성
- `plan` 계열 skills
  - 계획 수립 보조
- `$plugin-creator`
  - plugin 골격 생성
- `$imagegen`
  - 이미지 생성/수정 워크플로 명시 호출

### Skill을 직접 만들 수 있나

가능하다. 공식 문서는 먼저 built-in인 `$skill-creator`를 쓰는 방식을 권장한다.

```text
$skill-creator
```

또는 폴더를 만들고 `SKILL.md`를 직접 작성해도 된다.

### Curated skill 설치

공식 문서에는 built-ins 외의 curated skill을 로컬에 설치하는 방법도 있다.

예시:

```text
$skill-installer linear
```

즉, skill은 "내 로컬/내 저장소에서 바로 써보는 재사용 워크플로"에 적합하다.

## 2. Plugin이란?

공식 문서 기준 plugin은 reusable workflow를 배포하는 installable unit이다.

핵심 개념:

- skill은 authoring format
- plugin은 installable distribution unit

즉, 내가 혼자 로컬에서 실험할 때는 skill이 적합하고, 팀에 배포하거나 앱 연동까지 함께 묶고 싶으면 plugin이 맞다.

### Plugin이 담을 수 있는 것

plugin은 다음을 묶을 수 있다.

- Skills
- Apps
- MCP servers

공식 문서 예시:

- Gmail plugin
  - Gmail 읽기/관리
- Google Drive plugin
  - Drive, Docs, Sheets, Slides 작업
- Slack plugin
  - 채널 요약, 답변 초안 작성

OpenAI 공식 블로그 기준 2026-04-16 시점에는 90개 이상의 추가 plugin이 공개되었고, 예시로 Atlassian Rovo, CircleCI, CodeRabbit, GitLab Issues, Microsoft Suite, Neon by Databricks, Remotion, Render, Superpowers 등이 언급됐다. 이 목록은 계속 바뀔 수 있다.

### Plugin은 어디서 설치하나

- Codex app: Plugin Directory에서 설치
- Codex CLI: `codex` 실행 후 `/plugins`

CLI 흐름 예시:

```text
codex
/plugins
```

설치 후에는 새 thread에서 plugin을 사용하라고 요청하면 된다.

### Plugin 구조

공식 문서 기준 plugin의 필수 진입점은 아래 파일이다.

```text
.codex-plugin/plugin.json
```

plugin은 여기에 더해 아래 항목을 포함할 수 있다.

- `skills/`
- `.app.json`
- `.mcp.json`
- `assets/`

즉, plugin은 "skill + 외부 앱 연결 + MCP 설정 + 표시 메타데이터"까지 하나로 패키징하는 용도다.

### Plugin을 직접 만들 수 있나

가능하다. OpenAI는 `$plugin-creator` 사용을 먼저 권장한다.

```text
$plugin-creator
```

이 skill은 다음을 도와준다.

- `.codex-plugin/plugin.json` 생성
- 로컬 marketplace entry 생성
- 테스트 가능한 plugin 골격 구성

## 3. Marketplace는 무엇인가

plugin은 marketplace를 통해 배포/노출할 수 있다.

공식 문서 기준 marketplace는 plugin 목록이 담긴 JSON 카탈로그다.

주요 위치:

- 공식 Plugin Directory를 구동하는 curated marketplace
- 저장소 범위: `$REPO_ROOT/.agents/plugins/marketplace.json`
- 개인 범위: `~/.agents/plugins/marketplace.json`

즉, 팀 내부용 plugin 묶음을 repo 단위 marketplace로 둘 수도 있고, 개인용 plugin 묶음을 홈 디렉터리 기준으로 둘 수도 있다.

## 4. MCP는 무엇인가

MCP는 Model Context Protocol이다.

공식 문서 정의:

- models를 tools와 context에 연결하는 프로토콜
- third-party docs, browser, Figma 같은 외부 도구에 Codex를 연결할 때 사용

Codex는 현재 다음 MCP 유형을 지원한다고 문서에 적혀 있다.

- STDIO servers
- Streamable HTTP servers

그리고 공식 문서 기준 MCP는 Codex CLI와 IDE extension에서 지원된다.

설정은 보통 다음 위치의 `config.toml`에 넣는다.

- `~/.codex/config.toml`
- `.codex/config.toml`

CLI 예시:

```text
codex mcp add context7 -- npx -y @upstash/context7-mcp
```

TUI 안에서는 `/mcp`로 활성 MCP 서버를 확인할 수 있다.

## 5. Skill, Plugin, MCP를 어떻게 구분하면 되나

### Skill이 맞는 경우

- 내 저장소 안에서 반복되는 워크플로를 고정하고 싶다
- 팀 규칙, 절차, 검증 루프를 Codex에 가르치고 싶다
- 우선 로컬에서 빠르게 실험하고 싶다

예시:

- PR 리뷰 기준
- 릴리즈 체크리스트
- 특정 폴더 전용 리팩터링 규칙
- OpenAI API 업그레이드 가이드

### Plugin이 맞는 경우

- 여러 skill을 하나로 배포하고 싶다
- Slack, Gmail, Drive 같은 앱 연동까지 묶고 싶다
- 팀이나 조직이 설치 가능한 패키지로 쓰게 하고 싶다
- plugin directory나 marketplace에 올려 재사용하고 싶다

예시:

- 고객지원 triage plugin
- 문서 요약 + Slack 공유 plugin
- 빌드/배포/리뷰 통합 plugin

### MCP가 맞는 경우

- 외부 도구를 Codex가 직접 호출해야 한다
- 문서 서버, 브라우저, Figma, 내부 시스템 API 같은 연결이 필요하다
- skill이나 plugin 안에서 실제 툴 호출 기반 능력이 필요하다

예시:

- Figma 디자인 읽기
- 문서 검색 서버 연결
- 사내 지식베이스 조회

## 6. 실무적으로 어떤 순서가 좋은가

가장 실용적인 순서는 다음이다.

1. 먼저 로컬 skill로 워크플로를 만든다
2. 반복 사용이 확인되면 정리한다
3. 외부 앱 연동이나 MCP 구성이 필요하면 plugin으로 패키징한다
4. 팀 공유가 필요하면 marketplace에 등록한다

즉, 보통은:

- 처음부터 plugin으로 크게 시작하지 말고
- 먼저 skill로 검증한 뒤
- 필요할 때 plugin으로 승격하는 편이 좋다

## 7. 지금 기준으로 이해하면 되는 핵심

- skill은 "Codex에게 일하는 방식을 가르치는 것"
- plugin은 "그 방식을 설치 가능한 제품처럼 배포하는 것"
- MCP는 "외부 도구와 연결하는 표준 인터페이스"

헷갈릴 때는 아래처럼 기억하면 된다.

- `AGENTS.md` = 저장소 규칙
- `Skill` = 특정 작업용 재사용 워크플로
- `Plugin` = skills/apps/MCP를 묶은 설치 패키지
- `MCP` = 외부 툴 연결 레이어

## 8. 추천 사용 전략

- 개인 생산성 향상: skill부터 시작
- 팀 공용 자동화: plugin으로 묶기
- 외부 SaaS/문서/디자인 툴 연결: MCP 추가
- 저장소별 규칙: `AGENTS.md`와 skill을 같이 사용

## 공식 자료

- Agent Skills
  - https://developers.openai.com/codex/skills

- Plugins
  - https://developers.openai.com/codex/plugins

- Build plugins
  - https://developers.openai.com/codex/plugins/build

- Model Context Protocol
  - https://developers.openai.com/codex/mcp

- Features
  - https://developers.openai.com/codex/cli/features

- OpenAI 블로그: Codex for (almost) everything
  - https://openai.com/index/codex-for-almost-everything/
