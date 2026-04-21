# 01. Codex Skills 정리

작성일: 2026-04-21

## 한줄 요약

`Skill`은 Codex가 특정 일을 더 잘 하도록 가르치는 재사용 가능한 워크플로다.

## Skill이란?

OpenAI 공식 문서 기준 skill은 Codex에 task-specific capability를 추가하는 방식이다.

- instructions
- references
- optional scripts

를 묶어서, Codex가 어떤 작업을 더 안정적으로 수행하게 만든다.

공식 문서 표현으로 보면:

- skill은 reusable workflow의 authoring format
- plugin은 그런 skill을 배포하는 installable unit

즉, skill은 "작업 방법 자체"에 가깝다.

## 기본 구조

skill은 보통 하나의 디렉터리로 구성되며, 최소한 `SKILL.md`가 필요하다.

```text
my-skill/
  SKILL.md
  scripts/
  references/
  assets/
  agents/
    openai.yaml
```

핵심 파일:

- `SKILL.md`
  - 필수
  - `name`, `description` 포함

선택 항목:

- `scripts/`
  - 실행 코드
- `references/`
  - 참고 문서
- `assets/`
  - 템플릿, 리소스
- `agents/openai.yaml`
  - UI 메타데이터, 의존성, 호출 정책

## Codex가 skill을 쓰는 방식

공식 문서 기준 두 가지 방식이 있다.

1. 명시적 호출
   - CLI/IDE에서 `/skills`
   - 또는 `$skill-name` 형태로 직접 호출
2. 암묵적 호출
   - 사용자의 요청이 skill의 `description`과 맞으면 Codex가 자동 선택

그래서 `description`은 아주 중요하다.  
"언제 쓰고 언제 쓰지 말아야 하는지"까지 분명하게 써야 자동 매칭이 정확해진다.

## Skill의 핵심 장점

- 반복 작업을 매번 다시 설명하지 않아도 됨
- 팀 규칙과 검증 루프를 워크플로로 고정 가능
- 필요한 때만 전체 지침을 불러오는 progressive disclosure 방식이라 컨텍스트 효율이 좋음

## Skill은 어디에 저장하나

공식 문서 기준 Codex는 여러 범위의 skill을 읽는다.

- 저장소 범위: `.agents/skills`
- 사용자 범위: `~/.agents/skills`
- 관리자 범위: `/etc/codex/skills`
- 시스템 범위: Codex 기본 내장 skills

실무적으로는 보통 이렇게 쓴다.

- 프로젝트 전용 규칙: repo의 `.agents/skills`
- 개인 생산성용 규칙: `~/.agents/skills`

## 내장 skill 예시

공식 문서에 직접 언급된 기본 제공 예시:

- `$skill-creator`
  - 새 skill 생성
- `plan` 관련 skills
  - 계획 수립 보조
- `$imagegen`
  - 이미지 생성/편집 워크플로
- `$plugin-creator`
  - plugin 생성 보조
- `$skill-installer`
  - curated skill 설치

## Skill 만들기

공식 문서는 먼저 built-in creator 사용을 권장한다.

```text
$skill-creator
```

또는 수동으로 만들 수도 있다.

```text
---
name: skill-name
description: Explain exactly when this skill should and should not trigger.
---

Skill instructions for Codex to follow.
```

## Curated skill 설치

built-in 외의 skill을 로컬에 설치할 수도 있다.

예시:

```text
$skill-installer linear
```

즉, skill은 "내 환경에서 빠르게 써보는 로컬 워크플로"에 매우 적합하다.

## 언제 skill을 쓰면 좋은가

skill이 잘 맞는 경우:

- 특정 작업 절차를 반복한다
- 테스트/검증 루프를 고정하고 싶다
- 저장소/팀 규칙을 Codex에 매번 설명하기 싫다
- 우선 로컬에서 빠르게 실험하고 싶다

예시:

- 코드 리뷰 체크리스트
- OpenAI API 업그레이드 절차
- 릴리즈 준비 루틴
- 특정 폴더 전용 리팩터링 규칙

## Skill 작성 팁

공식 문서 권장사항:

- 하나의 skill은 하나의 job에 집중
- scripts보다 instructions 우선
- 입력과 출력이 분명한 imperative step 작성
- trigger description을 실제 프롬프트로 테스트

## Skill과 AGENTS.md의 차이

- `AGENTS.md`
  - 저장소 전반의 규칙과 제약
- `Skill`
  - 특정 업무를 수행하는 재사용 워크플로

보통 둘을 같이 쓰는 편이 가장 좋다.

## 공식 자료

- Agent Skills
  - https://developers.openai.com/codex/skills
