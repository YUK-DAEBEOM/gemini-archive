# Claude Code 플러그인 완벽 가이드

> 2026-04-21 기준 · 공식 마켓플레이스 + 커뮤니티 생태계 전체 정리
> 출처: `anthropics/claude-plugins-official`, buildwithclaude.com, claudemarketplaces.com, awesome-claude-code 등

---

## 📘 0. 플러그인 시스템 개요

### 플러그인이란?
플러그인은 **skills + hooks + subagents + MCP 서버 + slash commands + output styles**를 하나의 설치 가능한 단위로 묶은 것입니다. 즉 설정 여러 개를 한 번에 배포하는 패키지.

### 설치 흐름
```bash
# 1. 마켓플레이스 추가 (처음 한 번만)
/plugin marketplace add anthropics/claude-plugins-official

# 2. 개별 플러그인 설치
/plugin install github@claude-plugins-official

# 3. 활성화 (재시작 없이)
/reload-plugins
```

### 스코프 3가지
- **User scope** (기본): `~/.claude/` — 모든 프로젝트에서 사용
- **Project scope**: `.claude/settings.json` — 팀과 공유 (git 커밋)
- **Local scope**: `.claude/settings.local.json` — 본인만, 현재 프로젝트만

### 마켓플레이스 관리
```bash
/plugin marketplace list         # 등록된 마켓플레이스 목록
/plugin marketplace update <n>   # 카탈로그 새로고침
/plugin marketplace remove <n>   # 제거 (설치된 플러그인도 삭제됨)
/plugin disable <name>           # 일시 비활성화
/plugin uninstall <name>         # 완전 제거
```

### ⚠️ 보안 주의
플러그인은 **사용자 권한으로 임의 코드를 실행**할 수 있습니다. 신뢰할 수 있는 소스에서만 설치. 조직은 `managed settings`로 허용 마켓플레이스를 제한 가능.

---

## 🏛 1. 공식 Anthropic 마켓플레이스 (`claude-plugins-official`)

> **350+ 플러그인** 제공 · 자동 활성화됨 · 카탈로그: [claude.com/plugins](https://claude.com/plugins)

### 🔧 1-1. 언어 서버 (Code Intelligence) — 타입 지정 언어 필수

편집 후 자동 진단, 정확한 심볼 네비게이션 제공.

| 플러그인 | 언어 | 필요 바이너리 |
|---------|------|-------------|
| `typescript-lsp` | TypeScript/JavaScript | `typescript-language-server` |
| `pyright-lsp` | Python | `pyright-langserver` |
| `rust-analyzer-lsp` | Rust | `rust-analyzer` |
| `gopls-lsp` | Go | `gopls` |
| `clangd-lsp` | C/C++ | `clangd` |
| `csharp-lsp` | C# | `csharp-ls` |
| `jdtls-lsp` | Java | `jdtls` |
| `kotlin-lsp` | Kotlin | `kotlin-language-server` |
| `lua-lsp` | Lua | `lua-language-server` |
| `php-lsp` | PHP | `intelephense` |
| `swift-lsp` | Swift | `sourcekit-lsp` |

💡 **효과**: Claude가 편집 후 타입 에러/import 누락을 자동으로 잡고 한 턴에 수정. grep 대신 LSP로 "go to definition" 사용.

### 🔗 1-2. 외부 서비스 통합 (MCP 서버 번들)

**소스 컨트롤**
- `github` — Issues/PR/코드 리뷰/저장소 검색
- `gitlab` — 저장소·MR·CI/CD 파이프라인·이슈 관리

**프로젝트 관리**
- `atlassian` — Jira/Confluence (이슈 검색, 스프린트 관리)
- `linear` — 이슈 생성·프로젝트 관리·상태 업데이트
- `asana` — 작업 생성·진행 추적·워크플로우
- `notion` — 페이지 검색·문서 생성 및 업데이트

**디자인**
- `figma` — 컴포넌트 정보, 디자인 토큰, 코드 변환
- `miro` — 보드 안전 접근, 다이어그램 생성

**배포 & 인프라**
- `vercel` — 배포·빌드·로그·도메인 관리
- `firebase` — Firestore, 인증, Cloud Functions, 호스팅, Storage
- `supabase` — DB·인증·스토리지·실시간 구독
- `cloudflare` — Workers·Durable Objects·MCP 서버
- `railway` — 앱·DB·인프라 배포
- `aws-amplify` — 풀스택 앱 (인증, 데이터 모델, GraphQL)
- `aws-serverless` — 서버리스 앱 설계·배포·테스트
- `deploy-on-aws` — 아키텍처 권장·비용 추정·IaC 배포
- `azure-skills` — Azure MCP 통합

**커뮤니케이션**
- `slack` — 메시지 검색·스레드 접근
- `discord` — 메시징 (접근 제어 포함)
- `intercom` — 대화 검색·고객 지원 패턴 분석
- `imessage`, `telegraph` — 메시징

**모니터링**
- `sentry` — 에러 보고서·스택 추적 분석
- `posthog` — 분석·기능 플래그·A/B 테스트·에러 추적
- `amplitude` — 차트 분석·대시보드·실험
- `pagerduty` — 배포 위험 평가

### 💾 1-3. 데이터베이스

- `mongodb` — MongoDB MCP + 쿼리 최적화
- `neon` — Postgres 프로젝트 관리
- `prisma` — Prisma Postgres + 스키마 마이그레이션
- `planetscale` — 조직·DB·브랜치·스키마
- `cockroachdb` — 스키마 탐색·SQL 최적화
- `pinecone` — 벡터 DB·의미 검색
- `atlan` — 데이터 카탈로그·계보 추적
- `databases-on-aws` — RDS·DynamoDB·마이그레이션
- `dataverse` — Microsoft Dataverse

### 🔒 1-4. 보안

- `security-guidance` — 편집 시 명령 주입·XSS·시크릿 노출 경고
- `semgrep` — 실시간 취약점 탐지
- `sonarqube` — 7,000+ 규칙·40+ 언어·시크릿 스캔
- `aikido` — SAST·시크릿·IaC 취약점
- `autofix-bot` — 보안 취약점·품질·하드코딩 시크릿
- `sonatype-guide` — 의존성 취약점·안전 버전 권장
- `nightvision` — DAST 및 API Discovery
- `ai-plugins` (Endor Labs) — 공급망 보안 스캔

### 🛠 1-5. 개발 워크플로우

- `commit-commands` — git 커밋/푸시/PR 생성 슬래시 커맨드 모음
- `code-review` — 자동 PR 코드 리뷰 (신뢰도 필터링)
- `coderabbit` — 40+ 정적 분석기·보안 탐지
- `pr-review-toolkit` — PR 리뷰 전문 에이전트 (댓글·테스트·에러 처리·타입 설계)
- `feature-dev` — 코드베이스 탐색·아키텍처 설계·품질 검토
- `plugin-dev` — Claude Code 플러그인 개발 도구 (7개 전문 스킬)
- `mcp-server-dev` — MCP 서버 설계·구축
- `agent-sdk-dev` — Claude Agent SDK 개발 키트
- `skill-creator` — 스킬 생성·개선·벤치마크
- `claude-md-management` — CLAUDE.md 품질 감사·세션 학습 기록
- `claude-code-setup` — 코드베이스 분석 후 자동화 권장
- `hookify` — 마크다운 기반 커스텀 훅 생성
- `code-simplifier` — 코드 명확성·일관성·유지보수성 개선
- `greptile` — 자연어 기반 AI 코드베이스 검색
- `sourcegraph` — 저장소 간 참조 추적

### 🎨 1-6. UI/프론트엔드

- `frontend-design` — 고품질 프로덕션급 UI 생성 (제네릭 AI 미학 회피)
- `playwright` — 브라우저 자동화·E2E 테스트·스크린샷
- `chrome-devtools-mcp` — 라이브 Chrome 제어·성능 추적
- `stagehand` — 브라우저 자동화 스킬
- `playground` — 대화형 HTML 플레이그라운드 생성
- `liquid-skills` — Shopify Liquid·CSS/JS/HTML·WCAG 접근성

### 🧠 1-7. AI/ML

- `huggingface-skills` — 오픈소스 모델·데이터셋·Spaces 구축
- `pydantic-ai` — Pydantic AI 코드 작성·의사결정 트리
- `sagemaker-ai` — SageMaker로 모델 구축·훈련·배포
- `fiftyone` — 컴퓨터 비전 데이터셋·모델
- `atomic-agents` — Atomic Agents 프레임워크 워크플로우
- `firecrawl` — 웹 스크래핑·마크다운 변환·사이트 크롤

### 📚 1-8. 문서·출력 스타일

- `mintlify` — Mintlify 문서 사이트 구축 (MDX)
- `microsoft-docs` — Azure·.NET·Windows 공식 문서
- `context7` — Upstash Context7 MCP (버전별 문서)
- `explanatory-output-style` — 교육적 통찰 추가
- `learning-output-style` — 대화형 학습 모드 (TODO 마커)

### 💼 1-9. 플랫폼 특화

- `shopify`, `shopify-ai-toolkit` — GraphQL·Liquid·Hydrogen·Polaris (18개 스킬)
- `stripe` — Stripe 개발
- `sumup` — POS·온라인 결제·Cloud API
- `spotify-ads-api` — Spotify 광고 캠페인 (OAuth)
- `adspirer-ads-agent` — Google/Meta/TikTok/LinkedIn 광고 관리 (91개 도구)
- `wordpress.com` — WordPress Studio
- `wix` — Wix 사이트·앱·대시보드 확장
- `expo` — React Native (Expo 공식)
- `laravel-boost` — Artisan·Eloquent·라우팅·마이그레이션
- `sanity` — GROQ 쿼리·스키마 설계
- `cds-mcp` — SAP CAP 프로젝트
- `ui5` / `ui5-typescript-conversion` — SAPUI5/OpenUI5
- `adlc` — Salesforce 에이전트 개발 생명주기
- `rc` / `revenuecat` — RevenueCat 인앱 구매
- `base44` — Base44 풀스택 앱

### 🏗 1-10. 인프라 & IaC

- `terraform` — Terraform 생태계 통합
- `postman` — API 라이프사이클 관리
- `fastly-agent-toolkit` — Fastly 플랫폼
- `helius` — Solana 블록체인
- `astronomer-data-agents` — Apache Airflow DAG·파이프라인 디버깅
- `data-engineering` — 데이터웨어하우스·Airflow
- `zapier` — 8,000+ 앱 연결

### 🧪 1-11. 특화 도메인

- `math-olympiad` — IMO/Putnam/USAMO 수준 수학
- `legalzoom` — 법률 문서 검토·위험 식별
- `bigdata-com` — 금융 연구·분석
- `searchfit-seo` — 무료 SEO 도구
- `amazon-location-service` — 지도·지오코딩·라우팅
- `zoom-plugin` — Zoom REST API·SDK·웹훅
- `voila-api` — 배송 생성·추적

### 💾 1-12. 메모리·세션

- `superpowers` — 브레인스토밍·TDD·체계적 디버깅 (137k GitHub stars)
- `remember` — 일일 로그 압축 기반 지속 메모리
- `goodmem` — 임베더·스페이스·메모리 관리
- `session-report` — 세션 사용량 HTML 리포트 (토큰·캐시 효율)
- `ralph-loop` — 반복 개발용 자기참조 AI 루프

---

## 🌐 2. 대표 커뮤니티 마켓플레이스

### 2-1. buildwithclaude.com
- 500+ 실용 확장 모음 (Claude.ai, Claude Code, Claude API 공용)

### 2-2. claudemarketplaces.com
- 커뮤니티 투표·댓글 기반 큐레이션
- **Top Agent Skills**:
  - `find-skills` — 1.1M+ 설치 (Vercel Labs)
  - `vercel-react-best-practices` — 320k+ 설치
  - `frontend-design` — 300k+ 설치 (Anthropic)
  - `web-design-guidelines` — 256k+ 설치
  - `remotion-best-practices` — 243k+ 설치 (영상 제작)
  - `agent-browser` — 187k+ 설치
- **Top MCP 서버**: Fetch, Git, Memory, Time, Everything, Sequentialthinking

### 2-3. aitmpl.com
- Claude Skills/Agents/Commands/Hooks/Plugins 통합 허브
- Claude Desktop, Agent SDK, OpenClaw도 지원

### 2-4. 대표 GitHub Awesome 리스트
- `hesreallyhim/awesome-claude-code` — 가장 포괄적 큐레이션
- `rohitg00/awesome-claude-code-toolkit` — 135 에이전트·42 명령·176+ 플러그인
- `ykdojo/claude-code-tips` — 45개 실전 팁
- `shanraisshan/claude-code-best-practice` — 바이브→에이전틱 전환

---

## 🧩 3. 서브에이전트 컬렉션 (`wshobson/agents`)

> 184개 전문 에이전트 · 31.3k stars · 72개 포커스 플러그인

### 주요 카테고리
- **개발 (6개)**: Backend Developer, Frontend Developer, Debugger
- **문서화 (4개)**: API Documentation Agent, C4 Architecture Agent, Code Documenter
- **AI & ML (4개)**: LLM Application Specialist, RAG Architect, MLOps Engineer
- **인프라 (5개)**: Kubernetes Architect, Cloud Infrastructure Engineer, CI/CD Pipeline Designer
- **보안 (6개)**: Security Auditor, Compliance Officer, Dependency Analyzer
- **언어별 (10개)**: Python Pro, TypeScript Pro, Systems Engineer (C/Rust)

### 주목할 에이전트 (from awesome-claude-code-toolkit)
- **Fullstack Engineer** — 프론트·백·DB 전 영역 기능 배포
- **Code Reviewer** — 보안·성능 중심 PR 분석
- **API Designer** — RESTful 설계·OpenAPI·버저닝
- **Cloud Architect** — AWS/GCP/Azure IaC
- **Security Engineer** — IAM·mTLS·Vault 시크릿 관리
- **ML Engineer** — 파이프라인·훈련·모델 평가
- **Frontend Architect** — 컴포넌트 설계·상태 관리
- **Kubernetes Specialist** — Operators·CRDs·service mesh
- **Product Manager** — PRD·유저 스토리·RICE 우선순위

---

## ⌨ 4. 슬래시 명령어 컬렉션 (`wshobson/commands`)

### 워크플로우 (15개)
- `/workflows:feature-development` — 전체 기능 구현
- `/workflows:smart-fix` — 지능형 문제 해결
- `/workflows:tdd-cycle` — TDD Red-Green-Refactor
- `/workflows:legacy-modernize` — 레거시 코드 현대화
- `/workflows:security-hardening` — 보안 강화

### DevOps/인프라 (5개)
- `/tools:k8s-manifest` — Kubernetes 설정
- `/tools:docker-optimize` — 컨테이너 최적화
- `/tools:deploy-checklist` — 배포 체크리스트
- `/tools:monitor-setup` — 모니터링 설정
- `/tools:slo-implement` — SLO/SLI 정의

### 테스트/개발 (6개)
- `/tools:test-harness` — 테스트 스위트 생성
- `/tools:tdd-red` / `/tools:tdd-green` — TDD 사이클
- `/tools:api-scaffold` — API 엔드포인트 생성
- `/tools:api-mock` — Mock 데이터

### 보안/규정 (3개)
- `/tools:security-scan` — 취약점 스캔
- `/tools:compliance-check` — 규정 확인
- `/tools:accessibility-audit` — WCAG 감사

### 데이터 (3개)
- `/tools:data-pipeline` — ETL/ELT
- `/tools:db-migrate` — DB 마이그레이션
- `/tools:data-validation` — 품질 검증

### 문서/협업 (3개)
- `/tools:doc-generate` — API 문서 자동화
- `/tools:pr-enhance` — PR 최적화
- `/tools:standup-notes` — 상태 보고서

### 디버깅 (4개)
- `/tools:error-trace` — 프로덕션 오류 분석
- `/tools:debug-trace` — 런타임 분석
- `/tools:smart-debug` — 지원 디버깅

### AI/ML (4개)
- `/tools:ai-assistant`, `/tools:langchain-agent`, `/tools:prompt-optimize`, `/tools:ai-review`

---

## 🎯 5. 주목할 만한 대형 커뮤니티 플러그인

### 5-1. 세션·메모리 관리
- **claude-mem** (35.9k stars) — Claude 활동 자동 캡처·압축·재주입
- **claude-scaffold** — CLAUDE.md·hooks·18개 도메인 스킬을 한 번에 배포
- **claude-devtools** — 데스크톱 앱에서 세션 로그 분석·컨텍스트 시각화

### 5-2. 워크플로우 자동화
- **gstack** (68.2k stars) — Garry Tan의 6개 엄선 도구 세트
- **pro-workflow** (1.8k stars) — 자기교정 메모리·병렬 worktree·8가지 훅
- **oh-my-claudecode** (9.9k stars) — 19 에이전트·28 스킬, 팀 중심
- **great_cto** — 7 에이전트·12각 코드 리뷰·13 규정 프레임워크
- **AB Method** — 스펙 기반 워크플로우 (큰 문제 → 작은 미션)
- **Ralph for Claude Code** — 완료까지 반복 실행하는 자율 개발 프레임워크
- **Claude Code PM** — 에이전트·슬래시 명령 기반 포괄 프로젝트 관리

### 5-3. 훅 도구
- **claudekit** — 자동 저장, 품질 훅, 20+ 서브에이전트 CLI 키트
- **Dippy** — 안전한 bash는 자동 승인, 파괴적 작업만 프롬프트
- **cchooks** — Python 기반 훅 작성 SDK
- **TypeScript Quality Hooks** — ESLint·Prettier 실시간 검증

### 5-4. 분석·모니터링
- **ccusage** (11.5k stars) — 사용량 분석, 청구·세션 리포트
- **ccflare** — 웹 대시보드로 메트릭·비용 추적
- **recall** — 전체 텍스트 검색·빠른 세션 재개
- **claude-pace** / **ccstatusline** — 상태 라인 (rate limit·토큰 사용량 표시)

### 5-5. 대안 클라이언트
- **opcode** (21k stars) — Tauri 기반 데스크톱 GUI (세션 관리·커스텀 에이전트)
- **claude-tmux** — tmux 안에서 여러 Claude Code 인스턴스 관리

### 5-6. 보안·품질 게이트
- **Bouncer** — Gemini로 Claude 출력 독립 감사
- **Prism Scanner** — 스킬/플러그인 보안 스캐너 (39+ 규칙·AST 오염 추적)
- **ClawSearch** — Trust Score 기반 안전 스킬 검색
- **Trail of Bits Security Skills** — CodeQL·Semgrep 기반 감사

---

## 💎 6. 거대 스킬 컬렉션

### 6-1. 공식 확장
- **anthropics/skills** (111k stars) — 공식 에이전트 스킬
- **obra/superpowers** (137k stars) — 에이전트 스킬 프레임워크

### 6-2. 커뮤니티 대형
- **claude-skills** (5.3k stars) — 엔지니어링·마케팅·제품 192개 프로덕션 스킬
- **deep-dive** — DAG 기반 리서치 (질문을 의존성 그래프로 분해 + 병렬 서브에이전트)
- **MUSE** — 순수 Markdown 메모리 OS (48 스킬·대화 간 리콜)
- **Axiom** — EARS 요구사항·ATDD 기반 계약 강제 자율 파이프라인
- **AuraKit** — 풀스택 (46 모드·23 서브에이전트·6레이어 OWASP+ 보안)

### 6-3. SkillKit
- **SkillKit Marketplace** — 큐레이션 35개 외 **400,000+ 스킬** 접근

---

## 🎯 7. 추천 스타터 팩 (목적별)

### 🌱 신규 개인 개발자
```bash
/plugin install typescript-lsp@claude-plugins-official   # 또는 pyright-lsp
/plugin install github@claude-plugins-official
/plugin install commit-commands@claude-plugins-official
/plugin install code-review@claude-plugins-official
/plugin install security-guidance@claude-plugins-official
/plugin install learning-output-style@claude-plugins-official  # 학습 모드
```

### 👥 팀 협업 (풀스택)
```bash
/plugin install github@claude-plugins-official
/plugin install linear@claude-plugins-official            # 또는 atlassian
/plugin install slack@claude-plugins-official
/plugin install coderabbit@claude-plugins-official
/plugin install pr-review-toolkit@claude-plugins-official
/plugin install playwright@claude-plugins-official
```

### 🚀 DevOps/SRE
```bash
/plugin install aws-serverless@claude-plugins-official    # 또는 azure-skills
/plugin install terraform@claude-plugins-official
/plugin install sentry@claude-plugins-official
/plugin install pagerduty@claude-plugins-official
/plugin install posthog@claude-plugins-official
```

### 🎨 프론트엔드 디자이너
```bash
/plugin install frontend-design@claude-plugins-official
/plugin install figma@claude-plugins-official
/plugin install typescript-lsp@claude-plugins-official
/plugin install chrome-devtools-mcp@claude-plugins-official
/plugin install playwright@claude-plugins-official
```

### 🤖 AI/ML 엔지니어
```bash
/plugin install huggingface-skills@claude-plugins-official
/plugin install pydantic-ai@claude-plugins-official
/plugin install sagemaker-ai@claude-plugins-official
/plugin install pyright-lsp@claude-plugins-official
/plugin install context7@claude-plugins-official
```

### 🛒 E-커머스 / SaaS
```bash
/plugin install shopify-ai-toolkit@claude-plugins-official  # 또는 stripe
/plugin install sentry@claude-plugins-official
/plugin install posthog@claude-plugins-official
/plugin install intercom@claude-plugins-official
```

### 🔒 보안 심화
```bash
/plugin install semgrep@claude-plugins-official
/plugin install sonarqube@claude-plugins-official
/plugin install aikido@claude-plugins-official
/plugin install sonatype-guide@claude-plugins-official
/plugin install security-guidance@claude-plugins-official
```

---

## 🚨 8. 핵심 주의사항

1. **보안**: 플러그인은 사용자 권한으로 코드 실행 → 검증된 소스만
2. **충돌**: 같은 이름의 스킬이 여러 레벨에 있으면 **Enterprise > Personal > Project** 순
3. **재시작 불필요**: `/reload-plugins`로 즉시 반영
4. **비용**: 많은 플러그인 동시 활성화 시 컨텍스트 사용량↑ → `/context` 모니터링
5. **관리 정책**: 조직은 `strictKnownMarketplaces`로 허용 마켓플레이스 제한 가능
6. **자동 업데이트**: 공식 마켓플레이스는 기본 활성화, 서드파티는 기본 비활성화
7. **MCP 중복 주의**: 플러그인과 수동 MCP가 같은 서버를 제공할 수 있음 — `/mcp`로 확인

---

## 📚 관련 문서

- 공식 플러그인 디스커버리: [code.claude.com/docs/en/discover-plugins](https://code.claude.com/docs/en/discover-plugins)
- 플러그인 레퍼런스: [code.claude.com/docs/en/plugins](https://code.claude.com/docs/en/plugins)
- 마켓플레이스 생성: [code.claude.com/docs/en/plugin-marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
- Anthropic 공식 카탈로그: [claude.com/plugins](https://claude.com/plugins)
- 커뮤니티 허브: [claudemarketplaces.com](https://claudemarketplaces.com), [buildwithclaude.com](https://buildwithclaude.com), [aitmpl.com](https://aitmpl.com)

---

## 🔍 플러그인 찾기 실전 팁

1. **Claude Code 내부 검색이 가장 빠름**: `/plugin` → Discover 탭 → 검색어 입력
2. **GitHub Stars가 품질 지표**: 10k+ = 안정, 1k-10k = 검증됨, <1k = 신중
3. **설명에 키워드 매칭 확인**: Claude는 description으로 자동 호출 결정 → 애매한 description은 로드 안 됨
4. **스킬 vs 서브에이전트 vs 훅 vs MCP 중 뭘 쓸지**:
   - "항상 필요한 규칙" → CLAUDE.md
   - "때때로 필요한 워크플로우" → Skill
   - "격리된 리서치" → Subagent
   - "반드시 매번 실행" → Hook
   - "외부 서비스 연결" → MCP
5. **팀 환경**: `.claude/settings.json`에 `extraKnownMarketplaces` 등록하면 팀원이 자동으로 설치 프롬프트 받음

