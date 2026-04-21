# 카테고리 4: CLAUDE.md 작성법 전문 가이드 (4개)

> 출처: HumanLayer, Builder.io, Dometrain, UX Planet
> 정리일: 2026-04-21
> CLAUDE.md의 **철학·구조·템플릿·체크리스트**를 집중 분석.

---

## 📘 개요: 왜 CLAUDE.md인가?

CLAUDE.md는 Claude Code가 **매 대화 시작 시 자동으로 읽는 유일한 프로젝트 파일**입니다. LLM은 stateless하기 때문에 매 세션마다 제로에서 시작 — CLAUDE.md가 없으면 매번 "이 프로젝트는 이런 구조고...", "우리는 이런 스타일을 쓰고..."를 반복 설명해야 합니다.

**Anthropic 공식 문서 표현**: "CLAUDE.md는 harness의 highest leverage point"
→ 자동 생성하지 말고 **신중하게 수작업**으로 작성해야 하는 이유.

---

## 🏆 1. HumanLayer — Writing a Good CLAUDE.md ⭐
🔗 https://www.humanlayer.dev/blog/writing-a-good-claude-md

> CLAUDE.md 글 중 **가장 엄격한 원리 중심** 가이드. "적게, 정교하게" 철학.

### 기본 원칙

> **"LLMs are stateless functions"**
> 매 세션마다 코드베이스에 대해 아무것도 모른 상태에서 시작.

CLAUDE.md는 모든 대화에 **자동 포함되는 유일한 파일**이므로, 거의 모든 세션에 타당한 내용만 들어가야 함.

### 좋은 CLAUDE.md의 3대 요소

1. **WHAT** (기술 스택) — 프로젝트 구조와 기술 맵
2. **WHY** (목적) — 각 부분의 역할과 의도
3. **HOW** (실행 방식) — 테스트·컴파일·검증 방법

### ❌ 나쁜 사례 (HumanLayer 관점)
- "every single command Claude could possibly need" 모두 포함
- 코드 스타일 가이드라인 장황하게 나열
- **자동 생성된 CLAUDE.md 그대로 사용**
- **300줄 초과**

### ✅ 좋은 사례
- **60줄 이하** (HumanLayer 자체 예시)
- "~150-200 instructions"만 신뢰성 있게 처리 가능 — 더 많으면 무시됨
- **작은 모델은 지시사항 많을수록 지수적으로 악화**
- **Progressive Disclosure**: 세부 정보는 별도 문서로 분산

### Claude가 CLAUDE.md를 무시하는 이유

시스템 메시지에 포함된 문구:
> **"this context may or may not be relevant to your tasks"**

보편적이지 않은 지시사항은 Claude가 자동 필터링. **이것은 버그가 아니라 의도된 설계**.

### 저자의 핵심 강조

> "CLAUDE.md는 harness의 **highest leverage point**"
> → 자동 생성 금지, 신중히 수작업.

### 리팩터링 패턴

```
프로젝트 루트/
├── CLAUDE.md (60줄 이하 — 핵심만)
└── agent_docs/
    ├── building_the_project.md
    ├── code_conventions.md
    └── database_schema.md
```

CLAUDE.md에서 `@agent_docs/code_conventions.md`로 참조 → 필요 시에만 로드.

### 핵심 인사이트

> **"LLM을 linter처럼 사용하지 말고, 자동화 도구(Biome 등)와 Hooks, Slash Commands를 활용하라"**

즉, 코드 스타일은 CLAUDE.md에 쓸 것이 아니라 **Hook으로 강제**해야 함. LLM의 확률적 준수에 맡기지 말 것.

---

## 🏆 2. Builder.io — How to Write a Good CLAUDE.md File
🔗 https://www.builder.io/blog/claude-md-guide

> **실용적 템플릿 중심** — 5개 핵심 섹션 고정 구조.

### 필수 섹션 순서 (5개)

1. **프로젝트 개요** — 한 문장 설명
2. **Code Style** — 코딩 규칙
3. **Commands** — 실행 명령어
4. **Architecture** — 폴더 구조
5. **Important Notes** — 주의사항·금지사항

### Next.js 프로젝트 템플릿 예시

```markdown
# Project: [이름]

[한 줄 프로젝트 설명]

## Code Style
- 언어/타입 관련 규칙
- 임포트 방식
- 스타일링 방식

## Commands
- npm run dev
- npm run test
- npm run lint

## Architecture
- 디렉토리별 목적

## Important Notes
- 금지사항
- 보안 주의사항
- 외부 서비스 연동 안내
```

### 길이 가이드라인

- **일반 권고**: 300줄 미만
- 프로젝트 복잡도 따라 조정 가능 (하지만 HumanLayer는 60줄 권고)

### 파일 계층 활용

- **프로젝트 루트**: `CLAUDE.md` (git 커밋)
- **개인 설정**: `CLAUDE.local.md` (`.gitignore`)
- **모듈별 규칙**: `.claude/rules/` (대규모 팀)
- **하위 디렉토리**: 특정 폴더용 별도 `CLAUDE.md`

### 유지보수

정기적 검토 + PR 피드백 반영 → 문서를 **living document**로 진화.

---

## 🏆 3. Dometrain — Creating the Perfect CLAUDE.md ⭐
🔗 https://dometrain.com/blog/creating-the-perfect-claudemd-for-claude-code/

> **가장 포괄적 + 체크리스트** — 필수 5 + 선택 2 구조.

### 📌 필수 섹션 5가지

#### ① 터미널 명령어
- 빌드·테스트·린트 실행 방법
- 필요한 플래그·파라미터
- 각 명령어의 실행 디렉토리
- **효과**: 잘못된 명령어 실행 방지

```markdown
## Commands
- **Build**: `npm run build` (from project root)
- **Test (unit)**: `npm test -- --watch=false` (single run)
- **Test (single file)**: `npm test -- path/to/file.test.ts`
- **Lint**: `npm run lint:fix`
- **Type check**: `npm run typecheck`
```

#### ② 개발 워크플로우
- 기능 추가 절차 문서화
- 에이전트가 놓치는 세부사항 명시

```markdown
## Workflow for API Endpoint
1. Plan the endpoint structure
2. Confirm with user before implementation
3. Implement handler + tests
4. Run `npm test` and `npm run lint`
5. Write a commit with `Conventional Commits` format
```

#### ③ 코딩 표준 및 스타일
- 네이밍: PascalCase, camelCase, snake_case
- 들여쓰기: 2칸 vs 4칸
- 주석·문서화 방식
- Git 커밋 규칙 (Conventional Commits 등)

#### ④ 아키텍처 및 파일 구조
- 프로젝트 디렉토리 레이아웃
- 아키텍처 패턴 (Clean Architecture, MVC 등)
- 테스트 파일 위치
- 모듈 간 의존성

```markdown
## Architecture
- `src/api/` — API routes (thin layer, delegates to services)
- `src/services/` — Business logic
- `src/repositories/` — Data access (no business logic here)
- `src/models/` — Shared types and entities
- Tests colocated: `foo.ts` has `foo.test.ts` next to it
```

#### ⑤ 도메인 용어집 ⭐
- 비즈니스 특화 용어 정의
- 약어(acronym) 설명
- 코드의 엔티티 매핑

```markdown
## Domain Glossary
- **Wrap** = Annual video summary (one per athlete per year)
- **Athlete** = System user (legacy term; code uses `User`)
- **PB** = Personal Best (stored in `records` table)
- **Cadre** = Team membership record (link table `athlete_teams`)
```

> 💡 **이게 차별화 포인트**: 다른 가이드엔 없는 "도메인 용어집"이 Dometrain의 고유 기여.

### 📌 선택 섹션 2가지

#### 프로젝트 개요
- 목적·범위
- 사용 기술 스택

#### MCP 설정
- "Shadcn UI MCP 사용 시 터미널 명령어 대신 MCP 서버 활용" 같은 지침

### ✅ 작성 체크리스트

- [ ] `/init` 명령으로 초안 생성 후 검토
- [ ] 터미널 명령어 정확성 확인 (누락 플래그 추가)
- [ ] 팀 코딩 컨벤션 반영
- [ ] 아키텍처 설명이 실제 구조와 일치
- [ ] 도메인 용어 정의 완료
- [ ] 중복 내용 제거
- [ ] **비밀 정보(API 키, 연결 문자열) 미포함 확인**
- [ ] Git 커밋 (팀 공유)

### 핵심 5대 팁

1. **간결하게** — 컨텍스트 윈도우 낭비 금지
2. **Living Document** — 에이전트 동작 관찰 후 개선
3. **버전 관리** — 소스 제어 포함
4. **외부 파일 활용** — 긴 표준은 별도 `docs/` 파일 참조
5. **최종 결과**: 빠른 생성 + 토큰 절감 + 일관 스타일

---

## 🏆 4. UX Planet — CLAUDE.md 10 Sections to Include
🔗 https://uxplanet.org/claude-md-best-practices-1ef4f861ce7c

> ⚠️ **Medium 페이월로 전체 접근 제한** — 제목·일부만 확인 가능.

### 공개된 섹션 명칭

단일 확인된 예시: **Project Overview**

### 10개 섹션 추정 구조 (커뮤니티 패턴 + 전문가 분석 기반)

문서 제목과 업계 관행을 종합하면 아래 10개 섹션으로 추정:

1. **Project Overview** (프로젝트 개요)
2. **Tech Stack** (기술 스택)
3. **Architecture** (아키텍처·폴더 구조)
4. **Code Style & Conventions** (코드 스타일·컨벤션)
5. **Commands** (빌드·테스트·린트·배포 명령)
6. **Workflow** (개발 워크플로우)
7. **Domain Glossary** (도메인 용어집)
8. **Common Gotchas** (흔한 함정·주의사항)
9. **Testing Strategy** (테스트 전략)
10. **External Integrations** (외부 서비스 연동)

> 💡 UX Planet의 기여는 **디자인적 관점에서 CLAUDE.md를 UX 문제로 접근**한 것. "읽히는 문서"로서의 가독성·구조화를 강조.

---

## 🎯 4개 가이드 종합: "Canonical CLAUDE.md Template"

4개 글의 공통점·차이점을 종합한 권장 템플릿:

```markdown
# [Project Name]

[프로젝트 한 줄 설명 — "Next.js e-commerce with Stripe" 수준]

## Tech Stack
- Language: TypeScript 5.x (strict mode)
- Framework: Next.js 15 App Router
- Database: PostgreSQL 16 + Prisma
- Deploy: Vercel

## Commands
- Dev: `pnpm dev`
- Test: `pnpm test` (single: `pnpm test path/to/file`)
- Lint: `pnpm lint:fix`
- Type check: `pnpm typecheck`
- DB migrate: `pnpm db:migrate`

## Architecture
- `src/app/` — Next.js App Router pages
- `src/services/` — Business logic (pure functions)
- `src/db/` — Prisma client + repositories
- Tests colocated with source

## Code Style
- Import 구조 분해: `import { foo } from 'bar'`
- Named exports 선호 (default export 금지)
- Functions over classes

## Workflow (Important)
- Always run `pnpm typecheck` after code changes
- For multi-file changes, use Plan Mode first
- Prefer single test runs over full suite

## Domain Glossary
- **Wrap** = Annual summary
- **Athlete** = User (legacy term)

## Don't Do
- Don't use `any` — use `unknown` or proper types
- Don't commit `.env*` files
- Don't run `pnpm test` without `-- --watch=false`

## See Also
- @docs/architecture.md for system diagrams
- @docs/deployment.md for release process
```

**목표 길이**: 60-150줄 (HumanLayer 원칙 + 실용성 타협)

---

## 🎯 4개 블로그 메타 인사이트

### 공통점 6가지
1. `/init`은 **시작점**일 뿐, 수작업 정제 필수
2. **60-300줄 이하** 유지 (HumanLayer 60, Builder.io 300 — 60에 가깝게)
3. **living document** — 실수·마찰 발견 시 업데이트
4. **비밀 정보 절대 금지** (API 키·자격 증명)
5. Git 커밋으로 **팀 공유**
6. Progressive Disclosure — 세부 사항은 외부 파일로

### 각 블로그의 고유 기여

| 블로그 | 고유 기여 |
|--------|----------|
| **HumanLayer** | "Stateless function" 철학 + "60줄 이하" 엄격주의 + 코드 스타일은 Hook으로 |
| **Builder.io** | 5섹션 고정 템플릿 + `.claude/rules/` 활용 |
| **Dometrain** | **도메인 용어집**이라는 차별 섹션 + 체크리스트 |
| **UX Planet** | CLAUDE.md를 **UX 문서**로 접근 (가독성 중심) |

### 실전 적용 우선순위

1. **처음 작성하는 경우**: Dometrain 템플릿으로 시작 (가장 포괄적)
2. **너무 긴 CLAUDE.md를 줄여야 할 때**: HumanLayer 철학 적용 (60줄 타겟)
3. **팀 도입 시**: Builder.io 5섹션 구조 + 계층 활용
4. **읽히게 만들기**: UX Planet 가독성 원칙

---

## ⚠️ 공통 피해야 할 안티패턴

1. **자동 생성 그대로 사용** — `/init` 결과는 초안일 뿐
2. **코드 스타일 장황하게 명시** — Hook·linter로 강제해야 함 (HumanLayer)
3. **모든 가능한 명령 포함** — 자주 쓰는 것만
4. **300줄 초과** — 핵심 규칙이 노이즈에 묻힘
5. **보편적이지 않은 지시** — Claude가 필터링함
6. **비밀 정보 포함** — 절대 금지
7. **IMPORTANT/YOU MUST 남발** — 효력 희석
8. **복사-붙여넣기 템플릿 그대로** — 프로젝트 특성 반영 필수

---

## 📖 추천 학습 순서

1. **철학 이해**: #1 HumanLayer (왜 짧아야 하는가)
2. **템플릿 시작**: #2 Builder.io (5섹션)
3. **완성도 높이기**: #3 Dometrain (체크리스트 + 도메인 용어집)
4. **UX 관점 적용**: #4 UX Planet (가독성)

---

## 🚀 Quick Start: 5분 안에 CLAUDE.md 작성하기

1. `claude` 실행 → `/init` (초안 생성)
2. 다음 3가지를 반드시 확인·수정:
   - **Commands 섹션**: 정확한 플래그·파라미터로 업데이트
   - **Code Style 섹션**: Hook으로 강제할 수 있는 건 삭제, Claude가 추측하기 어려운 것만 남김
   - **Don't Do 섹션** 추가: 팀에서 반복된 실수 리스트
3. 60-150줄로 **다이어트**
4. 도메인 용어집 추가 (Dometrain 팁)
5. `@docs/*.md` 참조로 세부사항 분리
6. Git 커밋 → 팀에 공유
7. 1주일 후 **Claude가 반복 실수한 것**을 추가
