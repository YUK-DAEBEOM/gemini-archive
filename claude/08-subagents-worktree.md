# 카테고리 7: 서브에이전트·Git Worktree·병렬화 전문 (7개)

> 출처: Pragmatic Engineer, Lenny's Newsletter, Boris 본인, Tim Dietrich, MindStudio, Dan Does Code, Medium
> 정리일: 2026-04-21
> **Claude Code 창시자 Boris Cherny의 실제 워크플로우 + 병렬화 패턴 총정리**.

---

## 🏛 1. Pragmatic Engineer — How Claude Code is Built ⭐
🔗 https://newsletter.pragmaticengineer.com/p/how-claude-code-is-built

### 기술 스택 선택 철학

**사용 기술**: TypeScript, React, Ink, Yoga, Bun

**선택 기준**: **"분포 상의" 기술 선택**
→ Claude 모델이 **이미 능숙한 기술**들을 선택.

결과: **"Claude Code 자신이 90%를 작성"** (Boris 발언)
→ 모델이 능숙한 스택에선 도구가 자체적으로 진화 가능.

### 최소 비즈니스 로직 철학

- "모델을 최대한 원본 상태로 느끼도록"
- 새 모델 릴리스마다 **코드 삭제**
- **4.0 버전: 시스템 프롬프트 절반 제거**
- 복잡한 UI·스캐폴딩은 모델 능력 제한

### 보안 설계
- 로컬 실행 방식
- 파일 삭제 전 **간단한 사용자 승인**
- 프로젝트/사용자/회사 단위 설정

### 프로토타이핑 속도
- 투두 리스트 기능: **2일간 20개 프로토타입**
- AI 에이전트로 **하루 5-10개 프로토타입 검증** 가능

### 실무 교훈
Claude Code의 설계 원칙을 **사용자도 흉내낼 수 있음**:
- 스캐폴딩 줄이고 모델에 맡기기
- 프롬프트 자주 **삭제·정리**
- 빠른 반복 실험

---

## 🎙 2. Lenny's Newsletter — Boris Cherny 인터뷰
🔗 https://www.lennysnewsletter.com/p/head-of-claude-code-what-happens

### 성장 지표
- 1년 전: 단순 터미널 프로토타입
- 현재: **GitHub 공개 커밋의 4%**가 Claude Code 작성
- 지난달 DAU **2배 증가**

### Boris의 핵심 전망
> **"코딩이 해결되었다"**
>
> 소프트웨어 엔지니어링을 넘어 **모든 전문 업무의 역할**을 근본적으로 변화시키는 중.

### 개발 철학 (팀 운영)
- 팀에 **충분한 자원** 제공
- 제약 **최소화**
- **무제한 토큰 사용 허용**
→ 직관에 반하지만 더 우수한 AI 제품 탄생

### 실제 사용 사례
- Spotify 최상위 엔지니어들: **12월 이후 직접 코드 작성 중단**
- Boris 본인도 단기간 Cursor 거쳐 Claude Code로 복귀

---

## 👤 3. howborisusesclaudecode.com — Boris 본인 공개 ⭐
🔗 https://howborisusesclaudecode.com/

### ① 병렬 실행 (최우선)
**"5개 이상의 git worktree에서 동시 Claude 실행"**

```bash
claude --worktree feature-name
```

- 각 탭 독립 작동, 충돌 없음
- 터미널 탭을 **번호로 표시**해 관리

### ② CLAUDE.md 주 1-2회 업데이트
- Claude가 반복 실수하는 패턴 기록
- **PR 코멘트에 `@claude` 태그** → 자동으로 CLAUDE.md에 학습 추가

### ③ Plan Mode → 자동 승인
- 복잡 작업: `Shift+Tab` 두 번 → 계획 → 반복 정제
> **"좋은 계획이 있으면 나중에 문제를 피한다"** — Boris

### ④ Opus + Thinking 모드
**모든 작업에 Opus 사용.** 느리지만:
- 적은 지시 필요
- 더 나은 도구 사용
- **전체적으로 빠름**

### ⑤ 검증 최우선 (Boris의 #1 팁)
> **"Claude에게 작업을 검증할 방법을 주면 품질이 2-3배 향상"**

- 프론트: Chrome 확장
- 백엔드: 테스트 스위트
- DB: 쿼리 실행

### ⑥ 슬래시 명령 팀 공유
자주 쓰는 작업을 `.claude/commands/`에 저장 → 팀 전체 공유

---

## 🧩 4. Tim Dietrich — Sub-Agents for Parallel Work
🔗 https://timdietrich.me/blog/claude-code-parallel-subagents/

### 핵심 개념
> **"Sub-agents are separate Claude instances that the main agent spawns for specific jobs"**

### 활용 패턴
- 경쟁사 분석, 코드베이스 탐색, 문서 분석 등 **독립적** 태스크
- **명시적** 병렬 지시 필수
- 결과 종합을 위한 메인 에이전트 합성 요청

### 실전 예시
```
Research these 5 competitors in parallel using separate sub-agents.
Each sub-agent should analyze:
- Product roadmap
- Funding
- Customer reviews
Return a synthesis at the end.
```

### ⚠️ 주의사항
- **Sub-agents cannot spawn other sub-agents** (1 레벨)
- 병렬 쓰기 충돌 주의
- API 속도 제한 고려
- **읽기 중심 작업에 최적**

---

## 🔄 5. MindStudio — 5 Agentic Workflow Patterns
🔗 https://www.mindstudio.ai/blog/claude-code-agentic-workflow-patterns

### 5가지 패턴 총정리

#### ① Sequential (순차)
- 단계별 선형 실행
- 각 단계 결과 → 다음 입력
- **예**: "사양 읽기 → 함수 구현 → 테스트 작성 → 실행"

#### ② Operator (오케스트레이터)
- 관리 에이전트가 전문 하위 에이전트에 분담
- **언제**: 복잡 작업 분해 필요, 단일 컨텍스트 한계 도달
- **예**: Backend 에이전트 + Frontend 에이전트 + DB 에이전트 조율

#### ③ Split-and-Merge (분할-병합)
- 독립 작업 병렬 → 결과 합치기
- **예**: "50개 함수를 10개 배치로 나눠 동시 문서화"
- **최적**: 비슷·독립 작업 대량 처리

#### ④ Agent Teams
- 정의된 역할의 전문 에이전트 지속 협력
- **언제**: 장기 프로젝트, 다영역 조율 (코딩·테스트·검토·문서)

#### ⑤ Headless (자율)
- 인간 개입 없이 독립 작동
- **예**: "정기 저장소 감사, 의존성 업데이트 PR 생성, CI/CD 실패 분석"

### 선택 가이드

| 상황 | 패턴 |
|------|------|
| 단순 선형 | Sequential |
| 복잡 분해 | Operator |
| 대량 유사 작업 | Split-and-Merge |
| 다역할 조율 | Agent Teams |
| 완전 자동화 | Headless |

---

## 🌳 6. Dan Does Code — Parallel Vibe Coding with Worktrees
🔗 https://www.dandoescode.com/blog/parallel-vibe-coding-with-git-worktrees

### 핵심 설정
1. **`.claude/worktrees`를 `.gitignore`에 추가** (선행 필수)
2. 여러 터미널에서:
```bash
# Terminal 1
claude -w feature-payments

# Terminal 2
claude -w feature-auth
```

### 실제 효과
- 기존: 작업 A 완료 → 검토 → 병합 → 작업 B 시작 (순차)
- 변화: **진정한 병렬 처리**
- Claude는 여러 브랜치에서 동시 구현, 개발자는 **검토·방향 제시**에 집중

### 마무리
- 완료 시: "브랜치 푸시하고 PR 생성해달라"
- 세션 종료 시: worktree 유지 여부 결정

---

## 🌲 7. Medium — Mastering Git Worktrees
🔗 https://medium.com/@dtunai/mastering-git-worktrees-with-claude-code-...

### 개념 설명
> **"Git worktrees let you check out multiple branches into separate physical directories"**

동일 저장소 여러 분기를 독립 폴더로 관리 → 다중 Claude Code 세션 병렬 실행.

### 기본 명령어

```bash
# 새 브랜치로 worktree 생성
git worktree add ../ml-pipeline-training -b feature/model-training main
git worktree add ../ml-pipeline-inference -b feature/inference-api main

# IDE에서 각각 열기
code ../ml-pipeline-training
code ../ml-pipeline-inference
```

### 주요 장점
- 완전 독립 작업 환경
- **git stash/checkout 반복 제거**
- Claude가 각 분기별 **전체 컨텍스트 보존**
- **AI의 누적 지식 손실 방지**

### 실무 활용
- 모델 비교 (A/B 동시 개발)
- 안전한 마이그레이션 테스트
- 병렬 기능 개발
- 의존성 업그레이드 검증

### 자동화
bash 스크립트로 worktree 생성·정리 자동화 → 효율성 극대화.

---

## 🎯 종합: "Boris 워크플로우" 완전 모방 가이드

### 스타터 팩
```bash
# 1. 터미널 5개 탭 준비 (번호 표시)
# 2. 각 탭에서:
claude --worktree feature-N  # N=1~5

# 3. CLAUDE.md 주 1회 업데이트
# 4. 모든 작업은 Opus + Thinking
# 5. 복잡 작업은 Shift+Tab×2 (Plan Mode)
```

### 검증 인프라 (Boris #1 팁)
```
프론트 → Chrome 확장 설치
백엔드 → 테스트 스위트 보강
DB → 쿼리 실행 가능 환경
```

### 5가지 워크플로우 패턴 선택
- 혼자 작업 → Sequential
- 큰 기능 → Operator 또는 Split-and-Merge
- 팀 작업 → Agent Teams
- 야간 자동화 → Headless

---

## 🎯 병렬화 학습 로드맵

1. **개념**: #5 (MindStudio 5패턴) — 전체 조망
2. **철학**: #1 (Pragmatic Engineer) — 설계 원칙
3. **Boris 모방**: #3 (howborisusesclaudecode) — 실전 따라하기
4. **Worktree 기본**: #6 (Dan Does Code) → #7 (Medium)
5. **서브에이전트 심화**: #4 (Tim Dietrich)
6. **동기부여**: #2 (Lenny's Boris 인터뷰)

---

## 🚨 병렬화 함정 7가지

1. **Worktree 5개 넘기면 혼란** — 터미널 번호·색상 구분 필수
2. **같은 파일 동시 편집 충돌** — subagent는 읽기 중심으로
3. **서브에이전트는 1레벨만** — 중첩 불가
4. **병렬 API 속도 제한** — 팀 토큰 버짓 모니터링
5. **검증 없는 병렬 = 쓰레기 대량 생산** — 자동 검증 필수
6. **`.gitignore`에 `.claude/worktrees/` 빠뜨림** — 상위 레포에 누수
7. **`@claude` 태그 자동 학습 과부하** — CLAUDE.md가 스팸됨
