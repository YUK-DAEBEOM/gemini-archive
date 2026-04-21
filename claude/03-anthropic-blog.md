# 카테고리 2: Anthropic 공식 블로그·리소스 정리 (8개)

> 출처: `claude.com/blog`, `anthropic.com/news`, `support.claude.com`, `skilljar.com`
> 정리일: 2026-04-21
> 공식 문서에 없는 **철학·맥락·실전 통찰** 위주로 압축.

---

## 📝 1. Using CLAUDE.md files
🔗 https://claude.com/blog/using-claude-md-files

### 공식 문서와 차별화되는 핵심 인사이트

- **"영구 컨텍스트"라는 관점** — CLAUDE.md는 프로젝트 구조/규칙을 매 대화마다 반복 설명할 필요를 없애는 장치. 일회성 메모가 아닌 "설정 파일"로 취급하라
- **`/init`은 시작점, 끝이 아니다** — 자동 생성 파일을 팀의 실제 관행에 맞춰 **반복 수정**하는 것이 핵심. "완성된 템플릿"이 아님
- **"탐색 → 계획 → 코드 → 커밋" 워크플로우를 CLAUDE.md에 명시** — 표준 프로세스 문서화로 불필요한 재작업 방지
- **`/clear`의 전략적 중요성** — 이전 작업의 누적 정보 제거 = 신선한 집중력. CLAUDE.md 사용과 병행해야 진가 발휘
- **Subagent와 CLAUDE.md의 역할 분리** — 구현 vs 보안 검토처럼 관점이 다른 작업은 각자 격리된 subagent에서 처리, CLAUDE.md는 공통 규칙만
- **점진적 확장 원칙**: "단순하게 시작 → 실제 마찰점 기반으로 확장". **API 키·민감 정보 절대 금지**
- **커스텀 슬래시 명령 연계**: `.claude/commands/*.md`에 반복 프롬프트 저장 + 인자 전달 지원

### 실무 교훈
CLAUDE.md 파일 자체보다 **"무엇을 적고 무엇을 안 적을지 판단하는 센스"** 가 더 중요. 블로그 전반에서 반복되는 메시지는 "최소한으로 시작하라".

---

## 🤖 2. Subagents in Claude Code
🔗 https://claude.com/blog/subagents-in-claude-code

### 핵심 메시지
Subagent = **격리된 Claude 인스턴스** (자체 컨텍스트 윈도우). 메인 세션에 노이즈 없이 병렬 처리.

### ✅ 써야 할 때
- **연구 집약**: 수십 파일 읽어야 하는 컨텍스트 수집 → 요약만 반환
- **독립 병렬**: 서로 의존 없는 작업 (API 엔드포인트 / DB 스키마 / 인증 흐름을 동시 탐색)
- **객관적 검토**: "이전 논의를 보지 않은 신선한 subagent" → 편향 제거
- **커밋 전 검증**: 과적합·엣지 케이스 누락 방지
- **파이프라인**: 설계 → 구현 → 테스트 단계별 포커스

### ❌ 쓰지 말아야 할 때
- **순차 의존**: 2단계가 1단계 전체 결과를 필요로 함
- **같은 파일 동시 편집**: 충돌 위험
- **소규모 작업**: 오버헤드 > 이점
- **Subagent 간 상호작용 필요**: 좌표화 불가

### 실전 호출 패턴
```
subagent를 사용하여 병렬로 다음을 탐색:
1. API 엔드포인트 찾기
2. 데이터베이스 스키마 파악
3. 인증 흐름 매핑
각각의 요약만 반환
```

### 실제 Anthropic 예시
- **연구 후 구현**: 사용자 알림 추가 전 subagent로 이메일 발송·알림 패턴·아키텍처 조사
- **병렬 수정**: 3개 파일 에러 핸들링 동시 업데이트 → 순차 처리보다 빠름
- **독립 검토**: 구현 편향 없이 보안·엣지 케이스·에러 검증

### 핵심 원칙
**"대화형으로 시작 → 반복 패턴 발견 시 자동화 추가"**
처음부터 `.claude/agents/`에 정의하지 말고, 즉석 위임으로 효과를 검증한 후 영구화하라.

---

## 🛡 3. Automate Security Reviews with Claude Code
🔗 https://claude.com/blog/automate-security-reviews-with-claude-code

### 핵심 기능 2가지

**1. `/security-review` 명령 (터미널)**
- 즉석 보안 분석
- 탐지 대상: SQL 인젝션, XSS, 인증 결함, 안전하지 않은 데이터 처리, 의존성 취약점
- 커밋 전 자가 점검용

**2. GitHub Action (자동화)**
- PR 생성 시 자동 리뷰
- 인라인 코멘트로 보안 우려 + 수정 권장
- 기존 CI/CD 통합, 커스텀 규칙 지원

### Anthropic 내부 검증된 실제 결과
**두 건의 중대 취약점 사전 차단 사례**:
- **DNS 리바인딩 통한 RCE** — 로컬 HTTP 서버 취약점
- **SSRF 공격** — 자격 증명 관리 프록시 시스템

둘 다 **머지 전에 식별·수정**. `/security-review`가 단순한 린터 수준이 아닌, **체인형 공격 패턴까지 추적 가능**함을 시사.

### 활용 제언
- 로컬에서 `/security-review`로 개인 점검
- 팀 차원에선 GitHub Action으로 자동화 (Anthropic 내부 방식과 동일)
- **자동 스캐너 대체가 아니라 보완**: 여전히 수동 리뷰 필요

---

## 🎯 4. Introduction to Agentic Coding
🔗 https://claude.com/blog/introduction-to-agentic-coding

### 패러다임 전환의 본질

| 전통적 AI 코딩 | 에이전틱 코딩 |
|---|---|
| 자동완성 (라인별 제안) | 목표 기반 자율 실행 |
| 수동 조율 필수 (copy/paste 반복) | AI가 프로젝트 전체 파악 |
| "다음 줄 뭐 써?" | "이 시스템이 뭘 만들어야 해?" |
| 조각난 구현 | 통합 기능 배포 |

### 인용 핵심 문장
> "Traditional coding assistants wait for you to type each line and suggest what comes next. Agentic systems take a high-level goal, break it into discrete steps, execute those steps independently"

### 실증: Rakuten 사례
- 정교한 알고리즘 구현
- **7시간**에 완료
- 수백만 줄 코드 중 **99.9% 정확도**
- 최소한의 인간 개입

### 사고방식 전환
- **"무엇을 코딩할지"가 아니라 "무엇을 만들 시스템인지"를 질문**
- 개발자 역할 변화: 구현자 → 검토자·승인자
- 작업 단위 변화: 함수 → 기능 배포

### 의미
이 블로그는 마케팅 글 같지만 **Anthropic이 Claude Code를 어떻게 포지셔닝하는지 공식 선언**. "agentic coding"이라는 용어 자체의 의미를 숙지할 필요가 있음.

---

## 🚀 5. Enabling Claude Code to Work More Autonomously
🔗 https://www.anthropic.com/news/enabling-claude-code-to-work-more-autonomously

### 자율성 강화 3축

**축 1: 체크포인트 시스템**
- 모든 변경 **직전** 자동 스냅샷
- `Esc Esc` 또는 `/rewind`로 복원
- 복원 범위 선택: 코드만 / 대화만 / 둘 다
- → **"위험한 시도를 두려워 말라"** 메시지

**축 2: 병렬 작업 지원**
- Subagent로 전문 작업 위임 (백엔드 구축 + 프론트엔드 개발 동시)
- Hooks로 자동 테스트 실행
- 백그라운드 태스크로 dev 서버 유지

**축 3: 플랫폼 확장**
- VS Code 네이티브 확장 (베타)
- 터미널 인터페이스 2.0
- Sonnet 4.5 기본값 (당시 기준, 현재는 Opus 4.7)

### 핵심 철학
"자율성 ≠ 무감독 실행". Anthropic은 **자율성 향상 = 되돌리기 쉬움 + 검증 가능성 증대**로 정의. 체크포인트가 첫 번째로 도입된 것은 우연이 아님.

---

## 🔧 6. Building Agents with the Claude Agent SDK
🔗 https://claude.com/blog/building-agents-with-the-claude-agent-sdk

### SDK 설계 철학
> **"Claude에게 컴퓨터 접근권을 제공한다"** (Give Claude access to a computer)
> 프로그래머처럼 파일 작성·명령 실행·반복 개선하도록.

### 에이전트 피드백 루프 4단계

```
① 맥락 수집 → ② 실행 → ③ 검증 → ④ 반복
```

**① 맥락 수집 (Gather Context)**
- 파일 시스템 검색 (grep, tail, bash 스크립트)
- 의미론적 검색 (벡터 임베딩 — 고속이지만 정확도↓)
- Subagent (병렬 + 컨텍스트 격리)
- 컴팩션 (긴 대화 자동 요약)

**② 실행 (Take Action)**
- **Tools**: 빈번한 작업을 주요 도구로 정의
- **Bash 스크립트**: PDF 변환, 웹 검색 등 유연 작업
- **코드 생성**: 복잡 작업을 정확·재사용 가능한 코드로
- **MCP**: Slack, GitHub, Asana 등 표준화 통합

**③ 검증 (Verify Work)**
- **규칙 기반**: 린팅 등 명확한 룰
- **시각적**: 이메일 렌더·UI 스크린샷
- **LLM 심판**: 별도 모델이 품질 평가 (고비용)

**④ 반복 (Iterate)**
- 위 3단계를 목표 달성까지 반복

### 대표 유스케이스
- 금융 에이전트 (포트폴리오·투자 평가)
- 개인 비서 (여행 예약·일정)
- 고객 지원 (모호 요청 처리)
- 심화 연구 (대규모 문서 분석)

### 에이전트 품질 향상 체크리스트
Anthropic이 제시하는 4가지 자가 점검:
1. 검색 API 구조를 개선하면 정보 발견이 쉬워질까?
2. 반복 실패에 공식 규칙을 추가할 수 있을까?
3. 더 창의적인 도구 제공이 문제 해결에 도움될까?
4. 기능 추가 시 대표 테스트셋으로 성능 평가하고 있나?

### 실용 포인트
- SDK는 Python + TypeScript 지원
- Claude Code 기능의 **상향 추상화** — Claude Code로 배운 패턴을 그대로 프로그래머블하게 적용 가능
- 기존 Claude Code 사용자는 [migration guide](https://docs.claude.com/en/docs/claude-code/sdk/migration-guide)로 이전

---

## 💡 7. Claude Code Power User Tips (Help Center) ⭐
🔗 https://support.claude.com/en/articles/14554000-claude-code-power-user-tips

> **이 문서가 공식 팁 중 가장 실용적** — 12개 섹션 모두 정독 권장.

### ① 병렬 작업
- `claude --worktree` 또는 `claude --worktree my_worktree`
- 3~5개 세션 동시 실행이 sweet spot
- Subagent에 `isolation: worktree` 추가로 격리 병렬
- `/batch`: 대규모 마이그레이션 자동화

### ② 계획 먼저
- **Shift+Tab**으로 Plan Mode → 상세 계획 → 자동 수락 실행
- **"Opus + thinking"** 조합 추천: "더 큰 모델이지만 적게 조정해도 되고 도구 활용이 뛰어나 결국 더 빠름"
- `/effort [low|medium|high|max|auto]`로 난이도 조절

### ③ 효과적 프롬프팅
- **압박형 프롬프트**: "Grill me on these changes and don't make a PR until I pass your test"
- **동작 증명 요구**: "Prove to me this works"
- `/btw`: 진행 중 빠른 질문 (도구 호출 없이, 세션 컨텍스트 오염 없음)

### ④ 검증 (Anthropic이 가장 강조) ⭐
- [Chrome 확장](https://code.claude.com/docs/en/chrome) 설치로 프론트엔드 검증
- Desktop 앱이 웹 서버 자동 시작·테스트
- `/simplify`: 품질·재사용성·효율성 검토

### ⑤ CLAUDE.md & 메모리
- 루트 CLAUDE.md → 팀 전체 공유
- **실수할 때마다 규칙 추가**: "Update your CLAUDE.md so you don't make that mistake again"
- `/memory`로 자동 메모리 활성화
- GitHub PR 댓글에서 `@claude` 멘션 → 학습 항목 자동 추가

### ⑥ 명령·스킬·서브에이전트
- `.claude/skills/<name>/SKILL.md` → 반복 워크플로우
- `.claude/agents/` → 에이전트 정의
- `claude --agent=<name>` → 커스텀 에이전트 실행

### ⑦ 권한 & 안전
- `/permissions`: 안전 명령 사전 허용 (`"Bash(bun run *)"`)
- `claude --enable-auto-mode`: 자동 권한 결정
- `/sandbox`: 파일/네트워크 격리

### ⑧ 반복 작업 스케줄링
- `/loop 5m /babysit` — 5분마다 로컬 반복 (최대 3일)
- `/schedule` — 클라우드에서 스케줄링 (노트북 닫아도 실행)

### ⑨ 모바일·원격
- 모바일 앱 Code 탭
- `/teleport` — 디바이스 간 세션 이동
- `/remote-control` — 폰에서 로컬 세션 제어

### ⑩ MCP 통합
- `claude mcp add` — Slack, BigQuery, Sentry 등
- 명언: **"Personally, I haven't written a line of SQL in 6+ months"** (DB CLI를 Claude가 처리)

### ⑪ 환경 커스터마이징
- `/config` — 라이트/다크 모드
- `/color` — 프롬프트 색상 (여러 세션 구분)
- `/voice` — 음성 입력
- `/keybindings` — 단축키 재설정

### ⑫ SDK & 멀티 레포
- `--bare` 플래그 → **시작 속도 10배**
- `--add-dir` 또는 `/add-dir` → 추가 폴더 권한
- `/branch` → 기존 세션 분기

### 문서 전체 결론
> **"검증 기능을 구현하면 Claude가 자체적으로 피드백 루프를 닫고 반복하므로 최종 결과 품질이 크게 향상됩니다."**

---

## 🎓 8. Claude Code 101 (무료 공식 강의)
🔗 https://anthropic.skilljar.com/claude-code-101

### 강의 개요
- Anthropic 공식 무료 강좌
- 대상: 소프트웨어 엔지니어링 입문자 + AI 코딩 에이전트 관심자
- 형식: 비디오 + 퀴즈

### 커리큘럼
1. **Claude Code 소개 및 작동 원리** — AI 코딩 에이전트 개념
2. **에이전트 루프·컨텍스트 윈도우·도구·권한** 작동 원리
3. **설치 및 설정** — 터미널, VS Code, JetBrains 등
4. **효과적인 프롬프트 작성**
5. **탐색 → 계획 → 코드 → 커밋 워크플로우**
6. **CLAUDE.md 파일**
7. **서브에이전트**
8. **MCP 서버 연결**
9. **훅(Hooks) 커스터마이징**
10. **최종 퀴즈**

### 학습 후 얻는 것
- Claude Code 작동 원리를 **내부 메커니즘 수준**으로 이해
- 공식 베스트 프랙티스를 체계적으로 습득
- 컨텍스트 관리 기술 체득

### 활용 팁
- 공식 문서를 읽기 전 **이 강의부터 듣는 것이 빠름** (시각적 설명)
- 팀 신규 입사자 온보딩 자료로 적합

---

## 🎯 핵심 메타 인사이트 (8개 자료 종합)

Anthropic 공식 자료 전반에서 **반복되는 메시지**:

1. **검증이 1순위** — 최소 3개 문서에서 강조. "Verification"이 Claude Code 효과의 80%를 결정
2. **Agentic ≠ 무감독** — 자율성을 높일수록 체크포인트·되돌리기를 함께 제공
3. **대화형으로 시작 → 패턴 발견 시 자동화** — CLAUDE.md·Skills·Subagents 모두 이 순서
4. **"무엇을 안 적을지"의 감각** — CLAUDE.md, 프롬프트, 훅 모두 미니멀리즘
5. **병렬화가 생산성의 본질** — 1→3~5 Claude 세션이 분수령
6. **사용자가 시스템의 일부** — 결과 품질이 낮을 때는 도구가 아니라 **프롬프트·컨텍스트·검증 구조**를 의심하라

---

## 📖 추천 학습 순서

1. **개념 이해**: #4 (Agentic Coding 소개) → #8 (Claude Code 101 강의)
2. **핵심 사용법**: #7 (Power User Tips ⭐) → #1 (CLAUDE.md)
3. **심화**: #2 (Subagents) → #5 (자율성)
4. **전문 영역**: #3 (보안 리뷰) → #6 (Agent SDK)
