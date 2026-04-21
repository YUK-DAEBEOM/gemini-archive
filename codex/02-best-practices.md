# codex-cli를 잘 사용하려면

작성일: 2026-04-21

## 한줄 결론

`codex-cli`를 잘 쓰는 핵심은 "좋은 프롬프트 몇 줄"보다 `작업 범위`, `지속 규칙`, `검증 루프`, `권한 설정`을 먼저 구조화하는 데 있다.

## 핵심 요약

1. Windows라면 가능하면 WSL2에서 쓰는 편이 낫다.
2. 로그인 방식은 먼저 정해야 한다. ChatGPT 로그인과 API key는 적용되는 권한/보존 정책이 다르다.
3. 프롬프트는 길게 쓰기보다 `Goal`, `Context`, `Constraints`, `Done when`을 분명히 주는 편이 낫다.
4. 복잡한 일은 바로 구현시키지 말고 `/plan`으로 먼저 작업 구조를 잡는다.
5. 반복해서 설명하는 규칙은 프롬프트에 계속 쓰지 말고 `AGENTS.md`에 넣는다.
6. 변경만 시키지 말고 테스트, 린트, 타입체크, diff review까지 한 세트로 요구해야 품질이 오른다.
7. 권한은 처음부터 세게 열지 말고 `workspace-write + on-request` 같은 보수적 기본값에서 시작한다.
8. `/diff`, `/review`, `/compact`, `/resume`, `/permissions`, `/model` 같은 세션 제어 명령을 익히면 생산성이 크게 올라간다.
9. 프론트엔드 작업은 텍스트만 주지 말고 스크린샷, 와이어프레임, 디자인 링크를 같이 주는 편이 낫다.
10. 자동화는 사람이 직접 안정적으로 돌리는 루프를 만든 다음 `MCP`, `skills`, `codex exec`, CI로 확장하는 순서가 좋다.

## 왜 이렇게 써야 하나

OpenAI 공식 블로그와 공식 문서를 같이 보면, Codex 계열 도구는 단순 코드 생성기가 아니라 "지시를 받아 작업하고, 검증하고, 다시 수정하는 에이전트"로 설계되어 있다. 그래서 잘 쓰는 사람은 질문을 잘하는 사람보다도 다음 네 가지를 먼저 정리하는 사람이다.

- 어디까지 바꿔도 되는가
- 무엇을 반드시 지켜야 하는가
- 무엇으로 성공을 판단하는가
- 어떤 권한까지 자동으로 허용할 것인가

## 실전 운영 원칙

### 1. 시작 환경

- 설치 후 첫 실행에서는 ChatGPT 계정 또는 API key로 로그인할 수 있다.
- CLI의 기본 인증 경로는 유효한 세션이 없을 때 ChatGPT 로그인이다.
- 현재 공식 문서 기준으로 Windows는 지원되지만, 최적 경험은 WSL2 워크스페이스 권장이다.

### 2. 프롬프트 방식

짧아도 아래 네 가지가 들어가면 결과가 안정적이다.

- Goal: 무엇을 바꾸려는가
- Context: 어떤 프로젝트/파일/기능인가
- Constraints: 금지 사항, 스타일, 호환성, 성능, 보안 제약
- Done when: 테스트 통과 조건, 리뷰 기준, 완료 판단

예시:

```text
Goal: 로그인 에러 처리 개선
Context: auth 폴더와 로그인 화면만 수정
Constraints: API 스펙 변경 금지, 기존 토스트 시스템 재사용
Done when: 실패 케이스 테스트 추가, lint/typecheck 통과, diff self-review
```

### 3. `/plan` 먼저, 구현은 나중

작업이 조금만 커져도 바로 코딩시키기보다 `/plan`으로 범위를 쪼개는 편이 낫다.

이 방식이 좋은 이유:

- 불필요한 대규모 수정이 줄어든다
- 완료 기준이 먼저 정해진다
- 위험한 변경을 초반에 걸러낼 수 있다

### 4. `AGENTS.md`를 반드시 활용

반복해서 알려줘야 하는 내용은 `AGENTS.md`로 고정하는 것이 가장 효율적이다.

넣어두면 좋은 항목:

- repo 구조와 중요한 디렉터리
- 실행 방법
- build/test/lint 명령
- 엔지니어링 규칙
- 바꾸면 안 되는 것
- 완료 기준

CLI의 `/init`으로 초안을 만든 뒤, 팀 규칙에 맞게 짧고 실용적으로 수정하는 편이 좋다.

### 5. "수정"이 아니라 "검증 루프"를 요청

좋은 요청:

- 필요한 테스트도 같이 작성해라
- 관련 테스트를 실행해라
- lint, format, typecheck를 확인해라
- 마지막에 diff를 검토해라

나쁜 요청:

- 이거 빨리 고쳐줘

Codex는 검증 루프를 같이 줄수록 결과가 안정적이다.

### 6. 권한은 보수적으로 시작

현재 문서 기준으로 `workspace-write`는 로컬 작업에 가장 실용적인 기본 모드다. 이 모드에서는 워크스페이스 안에서 파일 편집과 일반 명령 실행이 가능하지만, 네트워크는 기본적으로 꺼져 있다.

추천 기본값:

- `sandbox_mode = "workspace-write"`
- `approval_policy = "on-request"`

더 보수적으로 시작하고 싶다면:

- `sandbox_mode = "read-only"`
- `approval_policy = "untrusted"`

반복적으로 승인하는 안전한 명령만 `rules`로 점진적으로 열어주는 편이 낫다.

### 7. 자주 쓰면 좋은 슬래시 명령

- `/plan`: 구현 전 실행 계획 수립
- `/model`: 현재 세션 모델 변경
- `/permissions`: 승인 강도 조정
- `/diff`: 변경점 확인
- `/review`: 작업 트리 리뷰
- `/compact`: 긴 대화 요약
- `/resume`: 저장된 세션 재개
- `/mention`: 특정 파일을 대화에 붙여서 맥락 고정
- `/status`: 현재 모델, 권한, 토큰 상태 확인

### 8. 모델 선택

현재 공식 문서 기준으로 대부분의 작업은 `gpt-5.4`부터 시작하는 것이 권장된다.

- `gpt-5.4`: 대부분의 실무 작업 기본값
- `gpt-5.4-mini`: 더 빠르고 저렴한 가벼운 작업
- `gpt-5.3-codex-spark`: 매우 빠른 실시간 반복용 연구 프리뷰

### 9. 프론트엔드 작업은 시각 자료를 같이 준다

OpenAI 공식 문서와 블로그 모두 이미지/디자인 컨텍스트가 결과 품질을 높인다고 본다.

효과적인 입력:

- 에러 화면 스크린샷
- 레이아웃 이미지
- Figma 링크
- 원하는 상호작용 설명

### 10. 자동화는 나중에 붙인다

잘 쓰는 팀은 처음부터 모든 것을 자동화하지 않는다.

보통 순서는 이렇다.

1. 사람이 직접 CLI에서 안정적으로 반복 수행
2. `AGENTS.md`와 config 정리
3. `skills`나 `MCP`로 반복 작업 고정
4. `codex exec`와 CI로 비대화형 자동화 확장

특히 skills는 넓게 만들기보다 "트리거가 분명하고 출력이 좁은 작업"으로 쪼개는 편이 운영하기 좋다.

## 추천 기본 셋업

### 개인 기본값

- `~/.codex/config.toml`: 개인 기본 모델, 권한, MCP

### 프로젝트 기본값

- `.codex/config.toml`: 프로젝트별 설정
- `AGENTS.md`: 저장소 규칙과 검증 기준

### 추천 시작 습관

1. 각 저장소 루트에서 `/init`으로 `AGENTS.md` 초안 생성
2. 테스트/린트/타입체크 명령과 완료 기준 추가
3. 기본 권한은 보수적으로 유지
4. 복잡한 일은 `/plan`부터 시작
5. 마무리 전 `/diff`, `/review` 습관화

## 조사 결과에서 가장 중요한 포인트

- Codex는 "잘 질문하는 도구"이기보다 "잘 운영하는 도구"에 가깝다.
- 프롬프트 품질보다 `AGENTS.md`, config, 검증 루프가 장기적으로 더 중요하다.
- 문서 기준 현재 권장 기본 모델은 `gpt-5.4`다.
- 권한은 최소 권한으로 시작해서 필요한 범위만 점진적으로 넓히는 것이 공식 방향과 맞다.
- 팀 단위로는 `skills + AGENTS.md + scripts` 조합이 반복 업무를 가장 잘 흡수한다.

## 공식 자료 정리

### OpenAI 공식 블로그

- Introducing Codex
  - https://openai.com/index/introducing-codex/
  - 날짜: 2025-05-16
  - 핵심: Codex의 기본 작동 방식, AGENTS.md, 잘게 쪼갠 작업, 검증과 투명성

- Introducing upgrades to Codex
  - https://openai.com/index/introducing-upgrades-to-codex/
  - 날짜: 2025-09-15
  - 핵심: CLI/IDE/web 전반의 기능 강화, 실시간 협업성 증가

- Codex for (almost) everything
  - https://openai.com/index/codex-for-almost-everything/
  - 날짜: 2026-04-16
  - 핵심: Codex가 코딩을 넘어 장기 작업, 툴 연결, 메모리, 자동화로 확장되는 흐름

### OpenAI Developers 공식 문서

- CLI Overview
  - https://developers.openai.com/codex/cli

- Features
  - https://developers.openai.com/codex/cli/features

- Slash commands
  - https://developers.openai.com/codex/cli/slash-commands

- Best practices
  - https://developers.openai.com/codex/learn/best-practices

- Authentication
  - https://developers.openai.com/codex/auth

- Models
  - https://developers.openai.com/codex/models

- Agent approvals & security
  - https://developers.openai.com/codex/agent-approvals-security

- Config basics
  - https://developers.openai.com/codex/config-basic

- Rules
  - https://developers.openai.com/codex/rules

- AGENTS.md guide
  - https://developers.openai.com/codex/guides/agents-md

- Non-interactive mode
  - https://developers.openai.com/codex/noninteractive

### OpenAI Developers 공식 블로그

- Using skills to accelerate OSS maintenance
  - https://developers.openai.com/blog/skills-agents-sdk
  - 날짜: 2026-03-09
  - 핵심: `skills + AGENTS.md + GitHub Actions` 조합으로 반복 업무를 고정하는 실제 사례

- Building frontend UIs with Codex and Figma
  - https://developers.openai.com/blog/building-frontend-uis-with-codex-and-figma
  - 날짜: 2026-02-26
  - 핵심: 이미지와 디자인 컨텍스트를 연결해 프론트엔드 품질을 높이는 방법

## 바로 실행할 체크리스트

- `AGENTS.md`를 저장소 루트에 둔다
- 테스트/린트/타입체크 명령을 문서화한다
- `workspace-write + on-request`로 시작한다
- 큰 작업은 `/plan`부터 시작한다
- 변경 후 `/diff`와 `/review`를 습관화한다
- 반복 업무는 나중에 `skills`와 `codex exec`로 고정한다

