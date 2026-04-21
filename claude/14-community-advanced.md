# 카테고리 13: 커뮤니티 심화 자료 (6개)

> 출처: ClaudeLog, Till Freitag, DEV.to, VGV, Piebald-AI, rohitg00
> 정리일: 2026-04-21
> **전문가·파워유저·내부 구조** 수준의 심화 자료.

---

## 📚 1. ClaudeLog — Configuration Hub
🔗 https://claudelog.com/configuration/

### 구성
Claude Code 설정 전반의 **심층 참고 사이트**. 공식 문서보다 **커뮤니티 경험 기반** 해석.

### 다루는 주제
- 세부 설정 기제
- 권한 모드 비교
- Plugin/MCP/Skills 통합 패턴
- Statusline/Output Styles 고유 팁

### 고유 기여
- 공식 문서의 설정 항목을 **실전 사용 맥락**으로 재해석
- FAQ 형식으로 **자주 묻는 구체적 질문** 커버
- 새 버전 나올 때마다 **빠른 업데이트**

---

## 🎯 2. Till Freitag — 15 Hidden Features
🔗 https://till-freitag.com/en/blog/claude-code-hidden-features-en

### 덜 알려진 기능 5가지

**① 음성 코딩 (`/voice`)**
> **"Hold spacebar, talk, release"**
자연스럽게 코딩 지시 가능.

**② 텔레포트 세션**
데스크톱 시작 세션 → **모바일에서 계속**. 완전한 컨텍스트 이동.

**③ 루프와 스케줄**
```bash
/loop 5m /babysit          # 5분마다 자동
/schedule 9am daily /standup  # 반복 작업
```

**④ 배치 처리 (`/batch`)**
> **최대 50개 병렬 에이전트** 동시 작업
대규모 코드베이스 리팩터에 유용.

**⑤ Git 워크트리 (`-w`)**
별도 worktree에서 메인 브랜치 안전하게 보호.

---

## 💼 3. DEV.to — Complete Power User Guide
🔗 https://dev.to/numbpill3d/the-complete-claude-code-power-user-guide-...

### 파워유저 철학

**CLAUDE.md = Constitution**
리포의 "헌법" 파일. 세션 간 행동 일관성 유지.

**Slash Commands vs Skills**
- **Commands** (`.claude/commands/`): 수동 호출 단축
- **Skills** (`.claude/skills/`): 자율 발견·맥락 적용 (manual `/name` 또는 auto)

**Hooks as Guardrails**
CLAUDE.md의 **조언적** 지침 vs Hook의 **강제** 규칙.
- 테스트 통과 전 커밋 차단
- 편집 후 자동 포맷

**Session Discipline**
- 작업 간 `/clear`
- 작업 중간 `/compact`
- 토큰 사용 효율

**Integration Layers**
- MCP — 외부 서비스 연결
- Subagents — 복잡 워크플로우 격리
- Plugins — 재사용 컬렉션 번들

### 철학
> **"Claude가 정의한 가드레일 안에서 작동하도록 환경을 구조화하라.**
> **매 세션 컨텍스트를 재설명하지 말고."**

---

## 🦅 4. VGV Wingspan — Agentic Engineering Workflow
🔗 https://verygood.ventures/blog/vgv-wingspan-agentic-engineering-workflow/

### Wingspan 4단계 워크플로우

```
/brainstorm → /plan → /build → /review
```

**① `/brainstorm`** — 요구사항 정의
**② `/plan`** — 구현 계획 수립
**③ `/build`** — 코드 작성
**④ `/review`** — 검증·피드백

각 단계는 Claude Code 슬래시 명령으로 실행 → **"사양 기반 개발" 원칙**.

### 채택 이유
- **"네이티브 기술 지원과 강력한 추론 능력"**
- 개발자 터미널 환경과 자연스러운 통합

### 얻은 효과
> **무분별한 AI 코드 생성 문제 해결**
> - 아키텍처 일관성 보장
> - 테스트 누락 방지
> - 사전 계획에 따른 체계적 구현
> - **"배포 가능한 품질"** 산출물

---

## 🔬 5. Piebald-AI/claude-code-system-prompts ⭐
🔗 https://github.com/Piebald-AI/claude-code-system-prompts

> **Claude Code 내부 해부도** — 공식 공개하지 않는 내부를 리버스 엔지니어링.

### 제공 자원
**110+ 추출된 시스템 프롬프트**:
- 핵심 시스템 프롬프트
- 24+ 빌트인 도구 설명 (Write·Bash·WebFetch 등)
- Sub-agent 프롬프트 (Plan·Explore·Task 모드)
- 유틸리티 프롬프트 (CLAUDE.md·compact·statusline·security review·agent creation)

### 업데이트 주기
**각 Claude Code 릴리스 후 수 분 내** 업데이트.
**158+ 버전 추적** (v2.0.14부터).

### 학습 가치 5가지

1. **아키텍처 이해**
   Claude Code 동작이 **단일 거대 프롬프트가 아닌** 환경·설정 기반 조건부 조합임을 확인.

2. **도구 설계 패턴**
   24+ 빌트인 도구 문서화 방식 연구 → **AI 에이전트용 인터페이스 설계** 학습.

3. **에이전트 조율**
   Plan·Explore·Task 모드의 subagent 프롬프트 → 멀티 에이전트 오케스트레이션.

4. **컨텍스트 관리**
   컴팩션·메모리 통합·요약 기법을 유틸리티 프롬프트로 학습.

5. **실전 레퍼런스**
   Claude API·SDK·Managed Agents 패턴이 시스템 데이터로 임베드.

### 활용
- **Claude Agent SDK로 자체 에이전트 만들 때** — 패턴 복사
- **프롬프트 엔지니어링 공부** — 프로덕션급 예시
- **Claude Code 행동 이해** — 왜 이렇게 응답하는지 디버깅

---

## 🧰 6. rohitg00/awesome-claude-code-toolkit ⭐
🔗 https://github.com/rohitg00/awesome-claude-code-toolkit

### 생태계 최대 규모 컬렉션

**850+ 리소스 총합**:
- **135 에이전트**
- **35 큐레이션 스킬** + SkillKit 연동 시 **400,000+**
- **42 명령**
- **176+ 플러그인**
- **20 훅**
- 15 규칙
- 7 템플릿
- 14 MCP 설정
- 26 컴패니언 앱
- 53 생태계 항목

### 에이전트 10대 도메인

1. Core development
2. Language expertise
3. Infrastructure
4. Quality assurance
5. Data/AI
6. Developer experience
7. Specialized domains
8. Business operations
9. Orchestration
10. Research analytics

각 에이전트: 시스템 프롬프트 + 도구 스펙 포함.

### Skill 프레임워크
번들 스킬 35개 + **SkillKit 마켓플레이스 연동** → ~400,000개 추가 학습 모듈.
"도메인 특화 패턴 + 코드 예시 + 안티패턴 + 체크리스트".

### 커뮤니티 참여
28+ 커뮤니티 기여 스킬:
- LinkedIn 자동화
- Solana 트레이딩
- 기타 특수 도메인

### 멀티 도구 지원
Claude Code 외에도:
- Cursor
- Windsurf
- Copilot
- 기타 AI 개발 플랫폼

→ **AI 기반 소프트웨어 개발을 위한 OS**에 가까움.

---

## 🎯 심화 학습 로드맵

### 레벨 1: 파워 유저 되기
- **DEV.to Complete Guide (#3)** — 철학·가드레일
- **Till Freitag (#2)** — 숨은 기능 15개
- **ClaudeLog (#1)** — 설정 심화

### 레벨 2: 워크플로우 엔지니어
- **VGV Wingspan (#4)** — 사양 기반 개발 패턴
- 자체 Wingspan 유사 워크플로우 설계

### 레벨 3: 내부 구조 이해
- **Piebald-AI (#5)** — 시스템 프롬프트 리버스 엔지니어링
- 자체 SDK 기반 에이전트 구축

### 레벨 4: 생태계 기여자
- **rohitg00 toolkit (#6)** — 기존 자산 학습
- 커뮤니티 스킬/에이전트 기여

---

## 🎯 6개 자료 메타 인사이트

1. **ClaudeLog** = 공식 문서의 **커뮤니티 재해석**
2. **Till Freitag** = 숨은 기능이 진짜 많음 (공식 문서에도 없는 것 포함)
3. **DEV.to 철학**: **"매 세션 재설명하지 말고 환경을 구조화"**
4. **VGV Wingspan**: `/brainstorm → /plan → /build → /review` **사양 기반 개발 공식**
5. **Piebald-AI**: Claude Code는 **단일 프롬프트가 아닌 조건부 조합**
6. **rohitg00**: **850+ 리소스**로 생태계 OS에 가까움

---

## 🚀 파워 유저 된 이후 할 일

### ① 자신만의 "Wingspan" 설계
```
.claude/commands/
├── brainstorm.md
├── plan.md
├── build.md
└── review.md
```
팀 표준 워크플로우 명령으로.

### ② 시스템 프롬프트 역공학
Piebald-AI 레포 공부 → **왜 Claude가 이렇게 응답하는지** 이해.

### ③ 기여하기
- awesome-claude-code-toolkit에 PR
- claudemarketplaces.com에 커스텀 플러그인 업로드
- ClaudeLog FAQ에 경험 공유

### ④ 측정 인프라 구축
- `/cost` 일일 트래킹
- 팀 DAU·만족도 조사
- TPM 사용률 그래프

### ⑤ 교육 프로그램 설계
- 신규 입사자용 Claude Code 101 복습
- 팀 CLAUDE.md 리뷰 세션
- 훅/스킬 공유 주간 미팅
