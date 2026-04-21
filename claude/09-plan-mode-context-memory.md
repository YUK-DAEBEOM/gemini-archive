# 카테고리 8: Plan 모드·컨텍스트·메모리 전문 (8개)

> 출처: Armin Ronacher, DataCamp, codewithmukesh, Charlie Gleason, MindStudio, MadPlay, Claude Directory, Product Talk
> 정리일: 2026-04-21
> **생각하고·기억하고·관리하는 법** — Claude Code의 3대 메타 스킬.

---

## 🧠 1. Armin Ronacher — What Actually Is Plan Mode? ⭐
🔗 https://lucumr.pocoo.org/2025/12/17/what-is-plan-mode/

> Flask 창시자 Armin Ronacher의 Plan Mode **역공학**.

### 충격적 발견: Plan Mode는 "그냥 프롬프트"

> Plan Mode는 마크다운 파일을 생성하는 것 **이상의 특별한 기능이 아니며**, "사용자가 아직 실행을 원하지 않음"이라는 프롬프트 지시사항과 단계별 구조화된 워크플로우로 작동합니다.

### 4단계 워크플로우 (내부)
1. 이해(Understand)
2. 설계(Design)
3. 검토(Review)
4. 최종 계획(Final Plan)

### 중요 사실
- **도구 자체의 기능 제한은 없음**
- **프롬프트 강화만 있음**

### 저자의 결론
> 저자의 방식(마크다운 파일 기반 반복 작업)이 Plan Mode와 **본질적으로 유사**하며, 추가 UI 복잡성 없이 **자연스러운 대화로 충분**할 수 있다.

### 실무 시사점
- Plan Mode의 가치 = **구조화된 사고방식 + 승인 UX**
- 대안: 편집 가능한 마크다운 파일 기반이 더 많은 제어 가능

---

## 📐 2. DataCamp — Plan Mode Design Review-First Refactoring
🔗 https://www.datacamp.com/tutorial/claude-code-plan-mode

### 진입 3가지 방법
- `Shift+Tab` 두 번
- `/plan` 명령
- `--permission-mode plan` 플래그

### 3단계 디자인 리뷰 루프

#### ① 탐색
Claude가 코드베이스 읽고 의문점 제시.

#### ② 플랜 생성·편집
- `Ctrl+G`로 플랜 파일을 에디터에서 열기
- 주석 추가로 아키텍처 결정 미리 검토
- 승인 시 Normal Mode 자동 전환

#### ③ 구현
승인된 플랜에 따라 단계적 구현.

### 실전 사례: FastAPI 블로그 API 리팩터링
3개 파일 혼재 로직 → 모듈 분리. 플랜 승인 전 주석으로 **오류 사전 방지**.

---

## ⏱ 3. codewithmukesh — Think Before You Build
🔗 https://codewithmukesh.com/blog/plan-mode-claude-code/

### 정량적 효과 ⭐
> **"Plan Mode 사용으로 복잡 작업 시간 35분 → 12분"**
>
> **"계획 없이 진행 시 약 40% 확률로 처음부터 다시"**

### 저자의 실패 경험
ASP.NET Core API에 소프트 삭제 기능을 Plan Mode **없이** 추가:
- 14개 파일 수정
- 3개 엔드포인트 깨짐
- 복구 불가능한 상태

→ 이후 "읽기 전용 분석 후 실행" 워크플로우로 전환 → 실패 사라짐.

### Plan Mode 꼭 써야 할 때 (명확 기준)
> **"한 문장으로 변경 사항을 설명할 수 없으면 Plan Mode를 사용하세요"**

구체적으로:
- **3개 이상 파일 수정**
- 아키텍처 결정 포함
- **낯선 코드베이스**

---

## 🪟 4. Charlie Gleason — Understanding Context Windows
🔗 https://code.charliegleason.com/understanding-context-windows

### 개념
> **"AI 모델은 한 번에 제한된 양의 정보만 처리"**
> 토큰으로 측정.

### `/context` 명령으로 확인하는 5가지 영역
1. **시스템 프롬프트** — AI 행동 규칙
2. **시스템 도구** — 파일 읽기, 코드 검색 함수들
3. **메모리 파일** — `CLAUDE.md` 등 프로젝트 설정
4. **메시지** — 대화 내용 및 코드
5. **(Auto Memory 로드분)** — 약 200줄 또는 25KB

### 효율 활용 4원칙
1. **구체적 요청** — 범위 좁게
2. **프로젝트 메모리 활용** — CLAUDE.md로 반복 설명 제거
3. **큰 작업 단계별 분해**
4. **전문 에이전트 신뢰** — subagent로 격리

---

## 🔧 5. MindStudio — 18 Token Management Hacks (Top 10)
🔗 https://www.mindstudio.ai/blog/claude-code-token-management-hacks-3

### ① CLAUDE.md로 반복 설명 제거
프로젝트 아키텍처·핵심 패턴·기술 스택 기록.

### ② 세션 시작 전 작업 범위 사전 설정
구체적 사양 정의 → 명확한 Q&A.

### ③ 파일 정리 후 포함
불필요한 주석·데드 코드 제거 → 필요 부분만.

### ④ 큰 작업 분할
**"한 세션에서 완료 가능한 단위"**로 나누기.

### ⑤ 구조화된 프롬프트 템플릿
일관된 양식(문맥/목표/파일/제약).

### ⑥ 불필요한 설명 억제
**"No explanations, just the code"** → 응답 토큰 **30-50% 절감**.

### ⑦ `/compact` 적극 활용
작업 완료 후 적절히 압축.

### ⑧ 짧고 직설적인 메시지
외교적 표현·반복 인사 제거.

### ⑨ 라인 번호로 코드 참조
이미 있는 코드는 재붙여넣지 말고 **"lines 42-58"**처럼.

### ⑩ diff 형식 출력 요청
**"전체 파일이 아닌 diff 형식"** → 변경사항에만 토큰 사용.

---

## 🎯 6. MadPlay — Context Management Strategy
🔗 https://madplay.github.io/en/post/claude-code-context-and-token-optimization

### 문제 진단
> **"토큰 소비는 대화 기록이 누적되면서 모든 요청마다 증가"**

### 4대 관리 기법

1. **자동 압축 (Auto-compact)**
   - 맥락 제한의 **75% 도달 시 자동**

2. **명시적 관리**
   - `/compact keep [내용]`로 필요 정보만 보존

3. **프로젝트 문서화**
   - CLAUDE.md로 반복 설명 제거

4. **세션 분할**
   - PR별·작업 변경 시 새 세션

### 비용 체크 (저자 경험)
- `/cost` 명령으로 세션 사용량 확인
- **일일 평균 약 $6** (API 기준) — 관리 중요

---

## 🧬 7. Claude Directory — Auto-Memory 심화 가이드
🔗 https://www.claudedirectory.org/blog/claude-code-auto-memory-guide

### 동작 방식
- 저장 위치: `~/.claude/projects/<project>/memory/`
- 마크다운 기반 파일 시스템
- Claude가 관련 메모리 읽고 **학습 결과 기록**

### 4가지 메모리 유형
1. **User** — 역할·전문성
2. **Feedback** — 수정·승인 기반 규칙
3. **Project** — 진행중인 작업·마감
4. **Reference** — 외부 시스템 링크

### CLAUDE.md vs Auto-Memory

| 구분 | CLAUDE.md | Auto-Memory |
|------|-----------|-------------|
| 작성자 | 개발자 | Claude |
| 성격 | 정적 | 동적 |
| 내용 | 프로젝트 규칙 | 세션별 학습 |
| 공유 | Git 커밋 | 로컬 개인 |

→ **상호 보완**: 기본 규칙은 CLAUDE.md, 세션 학습은 Auto-Memory

### ⭐ 핵심 팁
> **"긍정적 피드백도 명시해야 한다"**
>
> "회피할 사항"만 저장하면 Claude는 신중해도 **정확하지 않음**.

---

## 📝 8. Product Talk — Give Claude Code a Memory
🔗 https://www.producttalk.org/give-claude-code-a-memory/

> **비개발자(PM) 관점**의 Claude Code 메모리 활용.

### 3계층 메모리 구조

#### ① 전역 설정 (`~/.claude/CLAUDE.md`)
개인 선호도. 예: **"항상 계획을 먼저 검토하도록"**

#### ② 프로젝트 지정 지시 (각 폴더의 CLAUDE.md)
작업 유형별 규칙. 글쓰기·코딩·업무관리 등.

#### ③ 참조 맥락 파일
필요시만 불러오는 문서. 제품정보·타겟고객·경쟁사 분석.

### PM·마케팅 활용 사례
Claude Code는 **로컬 파일 접근 가능** → 한 번만 설정한:
- 고객 프로필
- 제품 정보
- 경쟁사 분석

→ **모든 대화에서 재활용 가능**

### 결과
> **"고품질 맞춤형 산출물을 반복 설명 없이"**

---

## 🎯 종합: 생각·기억·관리 3대 스킬 매트릭스

### 언제 Plan Mode를 써야 하나?

| 상황 | Plan Mode 필요성 |
|------|-----------------|
| 오타·로그 줄 추가 | ❌ 불필요 |
| 한 문장 설명 가능 변경 | ❌ 불필요 |
| 3+ 파일 수정 | ✅ 강력 권장 |
| 아키텍처 변경 | ✅ 필수 |
| 낯선 코드베이스 | ✅ 필수 |
| 마이그레이션 | ✅ 필수 |

### 컨텍스트 모니터링 체크리스트
- [ ] `/context` 명령 정기 실행 (하루 1회)
- [ ] 토큰 40% 넘으면 경계
- [ ] 관련 없는 작업 간 `/clear`
- [ ] 75% 도달 시 auto-compact 허용 또는 `/compact` 수동 실행
- [ ] `/cost`로 일일 사용량 확인 ($6 baseline)
- [ ] 세션 전환: PR마다 새 세션

### 메모리 전략
- **CLAUDE.md** (60-200줄): 팀 공유 정적 규칙
- **CLAUDE.local.md**: 개인 프로젝트 설정 (gitignore)
- **Auto-Memory**: Claude가 자동 기록 — 긍정 피드백도 주기
- **Skills**: on-demand 워크플로우
- **`.claude/rules/`**: 경로별 조건부 규칙

---

## 🚀 오늘 적용할 5가지 Action

### 1. Plan Mode 습관화 (codewithmukesh)
**"한 문장으로 변경 설명 불가 → Plan Mode"** 룰 적용. 3개월 안에 40% 재작업 감소 체감.

### 2. `/context`·`/cost` 일일 체크 (MadPlay)
세션 시작·종료 시 두 명령 확인. 주간 비용 패턴 발견.

### 3. 토큰 10개 핵 중 3개 즉시 적용 (MindStudio)
- 응답 "No explanations, just code" 추가
- 파일 라인 번호로 참조
- diff 형식 요청

### 4. Auto-Memory 긍정 피드백 주기 (Claude Directory)
Claude가 잘한 선택에 "keep doing that" 명시 → 메모리에 positive 규칙 추가.

### 5. 3계층 CLAUDE.md 구축 (Product Talk)
- `~/.claude/CLAUDE.md` — 개인 선호
- `./CLAUDE.md` — 프로젝트
- `./CLAUDE.local.md` — 개인 프로젝트 세팅

---

## 📖 추천 학습 순서

1. **개념 이해**: #4 (Charlie Gleason 컨텍스트) → #1 (Armin Plan Mode 역공학)
2. **Plan Mode 실전**: #3 (codewithmukesh 정량 효과) → #2 (DataCamp 3단계)
3. **토큰 관리**: #5 (MindStudio 10핵) → #6 (MadPlay 전략)
4. **메모리 시스템**: #7 (Claude Directory 4유형) → #8 (Product Talk 3계층)

---

## 🎯 메타 인사이트 8개

1. **Plan Mode는 프롬프트 엔지니어링** (Armin) — 기능이 아닌 워크플로우
2. **"한 문장 룰"** — 설명 불가하면 Plan Mode (codewithmukesh)
3. **40% 재작업 방지** — Plan Mode의 정량 효과
4. **35분 → 12분** — 계획이 빠른 이유는 역설 아님
5. **`/context` 일일 체크** — Claude 사용자의 "gauge"
6. **"No explanations, just code"** — 출력 토큰 30-50% 절감
7. **긍정 피드백 메모리** — "회피할 것"만 저장하면 부정확
8. **3계층 메모리** — 전역·프로젝트·참조 파일 분리
