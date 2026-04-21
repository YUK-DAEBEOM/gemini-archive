# 03. Codex MCP 정리

작성일: 2026-04-21

## 한줄 요약

`MCP`는 Codex가 외부 도구와 외부 문맥에 접근하도록 연결하는 표준 프로토콜이다.

## MCP란?

MCP는 Model Context Protocol의 약자다.

OpenAI 공식 문서 설명:

- models를 tools와 context에 연결하는 프로토콜
- third-party documentation, browser, Figma 같은 외부 도구를 Codex에 연결할 때 사용

즉, MCP는 "Codex가 바깥 세상과 연결되는 방식"이라고 이해하면 된다.

## MCP로 할 수 있는 것

공식 문서 기준 예시는 다음과 같다.

- 외부 문서 서버 연결
- 브라우저 연결
- Figma 연결
- 각종 개발 도구 및 외부 시스템 연결

즉, Codex가 로컬 파일만 보는 것을 넘어서:

- 문서를 읽고
- 외부 툴을 호출하고
- 별도 시스템의 문맥을 가져오는

구조를 만들 수 있다.

## Codex에서 MCP 지원 범위

공식 문서 기준 MCP는 다음 클라이언트에서 지원된다.

- Codex CLI
- Codex IDE extension

또한 문서 기준 현재 지원 타입은 다음과 같다.

- STDIO servers
- Streamable HTTP servers

추가로 인증 관련 지원:

- Bearer token
- OAuth

OAuth가 되는 서버는 예시로 다음 명령 흐름을 문서가 안내한다.

```text
codex mcp login <server-name>
```

## MCP 설정 위치

공식 문서 기준 MCP 설정은 `config.toml`에 저장된다.

- 사용자 범위: `~/.codex/config.toml`
- 프로젝트 범위: `.codex/config.toml`

즉, 어떤 MCP를 개인 기본값으로 둘지, 어떤 MCP를 프로젝트 전용으로 둘지를 분리할 수 있다.

## MCP 추가 방법

공식 문서는 두 방식을 안내한다.

1. CLI 사용
2. `config.toml` 직접 편집

CLI 예시:

```text
codex mcp add <server-name> --env VAR1=VALUE1 --env VAR2=VALUE2 -- <stdio-server-command>
```

공식 문서의 예시:

```text
codex mcp add context7 -- npx -y @upstash/context7-mcp
```

즉, MCP 서버를 하나의 로컬 실행 프로세스나 HTTP 엔드포인트로 등록해 Codex가 호출할 수 있게 만든다.

## TUI 안에서 확인하는 방법

Codex TUI에서:

```text
/mcp
```

를 사용하면 현재 활성 MCP 서버를 볼 수 있다.

## MCP와 skill / plugin의 관계

이 부분이 가장 헷갈리기 쉽다.

- MCP
  - 외부 도구 연결 방식
- Skill
  - 그 도구를 언제 어떻게 쓸지 설명하는 워크플로
- Plugin
  - skills, app mappings, MCP config를 함께 배포하는 패키지

즉:

- MCP는 연결 레이어
- skill은 행동 레이어
- plugin은 배포 레이어

로 이해하면 정리가 된다.

## 언제 MCP를 써야 하나

MCP가 잘 맞는 경우:

- Codex가 외부 문서를 검색해야 한다
- Figma나 브라우저 같은 도구와 연결해야 한다
- 사내 시스템이나 문서 서버와 연결해야 한다
- 로컬 저장소 바깥의 문맥이 필요하다

예시:

- OpenAI 문서 MCP
- Figma MCP
- 내부 지식베이스 MCP
- 브라우저/개발 도구 MCP

## MCP를 실무에서 쓰는 방식

보통은 아래 순서가 가장 자연스럽다.

1. MCP 서버 연결
2. 그 연결을 활용하는 skill 작성
3. 필요하면 skill + MCP 설정을 plugin으로 패키징

즉, MCP만 연결한다고 자동으로 좋은 워크플로가 생기지는 않는다.  
실전에서는 skill이나 plugin과 함께 설계해야 가장 쓸모가 커진다.

## 공식 자료

- Model Context Protocol
  - https://developers.openai.com/codex/mcp
