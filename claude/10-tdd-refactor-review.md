# 카테고리 9: TDD·리팩터·리뷰 전문 (5개)

> 출처: The New Stack, alexop.dev, Steve Kinney, Milvus, DataCamp
> 정리일: 2026-04-21
> Claude Code로 **검증 가능한 개발**을 실현하는 법.

---

## 🧪 1. The New Stack — Claude Code and the Art of TDD
🔗 https://thenewstack.io/claude-code-and-the-art-of-test-driven-development/

### 핵심
> **"TDD works quite well with LLM assistance, as the human developer can fix the quality barriers"**

### Claude가 TDD에 효과적인 3가지 이유
1. **지시 따르기** — 미리 작성된 테스트를 따라 구현
2. **설계 통제** — 개발자가 테스트로 기능 명확 정의
3. **인간 감시** — AI가 자동 테스트 실행하지 않아 **논리 오류 인간 검토 가능**

### 실전 사례: 은행 계좌 시스템
1. 초기 테스트 작성
2. Claude에게 테스트 통과 코드 생성 요청
3. 버그 발견 시 Claude와 협력 수정

경험 많은 개발자와 **주니어 엔지니어의 전통적 협업** 방식 재현.

---

## 🔴🟢🔵 2. alexop.dev — Agentic Red-Green-Refactor Loop ⭐
🔗 https://alexop.dev/posts/custom-tdd-workflow-claude-code-vue/

### 문제 진단
Claude Code **기본값은 구현 우선**. TDD 원칙 무시, 엣지 케이스 놓침.

### 해결책: Skills + Subagents 조합

**Context Pollution 제거**
> 같은 컨텍스트에서 모든 단계를 보면 LLM이 **무의식적으로 구현을 예상하며 테스트 설계**.

서브에이전트로 각 단계 **완전 격리** → 각자 독립 작업, 다른 단계 추론 안 보임.

### 3단계 오케스트레이션
1. **RED (Test Writer)** — 실패 통합 테스트 격리 작성
2. **GREEN (Implementer)** — 최소 코드로 테스트 통과
3. **REFACTOR (Refactorer)** — 의사결정 프레임워크로 개선

### 활성화 챌린지 해결
- Skills만으론 **20% 활성화율** — 불만족
- **Hooks 추가** → 라이프사이클에서 스킬 명시 평가 강제
- **84% 활성화율** 달성

### ROI
- 초기 설정: **약 2시간**
- 효과: **기능 요청마다 자동으로 Red-Green-Refactor 사이클**

---

## 🎓 3. Steve Kinney — TDD with Claude
🔗 https://stevekinney.com/courses/ai-development/test-driven-development-with-claude

### 5단계 순환
1. Write Tests First
2. 테스트 실패 확인
3. 실패한 테스트 커밋
4. 구현 코드 작성
5. 최종 커밋

### 실전 기법
- **명확한 지시**: `"Write a new unit test in tests/auth_test.py"`
- **자동화**: PostToolUse 훅으로 린터·테스트 자동 실행 → 즉각 피드백
- **다중 에이전트 검증**: Subagent로 구현이 테스트에만 과적합 방지

### 핵심 원칙
> **"Claude performs best when it has a clear, verifiable target"**

---

## 🔧 4. Milvus — Legacy Code Refactoring
🔗 https://milvus.io/ai-quick-reference/can-claude-code-refactor-legacy-code

> ⚠️ URL 접근 불가(redirect loop), 이전 검색 요약으로 대체.

### 가능 여부: YES
Claude Code는 레거시 리팩터링에 **특히 가치** — 큰 복잡한 코드베이스를 빠르게 이해.

### 권장 접근법
- 큰 작업을 **작고 테스트 가능한 단위**로 분해 → 점진 검토·승인
- "변경 점진화" — 급진적 일괄 변경 ❌
- 이해 먼저: Claude에게 **레거시 함수 설명 요청** → 의존성 매핑
- 현재 동작 잠금: **테스트 먼저 생성** 후 리팩터

### 실제 사례
- **210줄 Python 함수 + 복잡도 16** → **30줄 미만 + 복잡도 3-6**

---

## 📚 5. DataCamp — Practical Examples
🔗 https://www.datacamp.com/tutorial/claude-code

### 5대 실전 예시

**① 코드 리팩터링**
Supabase 클라이언트 import를 **논리 섹션으로 그룹화** → 가독성 향상

**② 문서화 자동 추가**
모듈 레벨 docstring으로 파일 목적 설명 → 코드 이해도 증진

**③ 버그 수정**
타입 오류 import → `# type: ignore` 주석으로 IDE 경고 제거

**④ Hooks 자동화**
PostToolUse로 파일 쓰기 후 자동 린터·포매터 실행

**⑤ 플러그인 통합**
GitHub/Slack 연동 → 코드 변경 시 자동 PR 생성·팀 알림

---

## 🎯 종합: 검증 가능한 개발 구축 가이드

### TDD 전체 파이프라인 (alexop.dev 기반)

```
.claude/
├── skills/
│   ├── red-test-writer/SKILL.md       # 테스트만 작성, 구현 지식 차단
│   ├── green-implementer/SKILL.md     # 최소 구현으로 테스트 통과
│   └── refactor-improver/SKILL.md     # 품질 개선
├── agents/
│   ├── test-writer.md                 # 격리 컨텍스트
│   ├── implementer.md
│   └── refactorer.md
└── settings.json
    hooks:
      PreToolUse → 스킬 활성화 강제
      PostToolUse → 자동 테스트 실행
```

### 리팩터 사이클

1. Claude에게 **이해** 요청: "레거시 함수 설명"
2. **테스트 먼저**: 현재 동작 잠금
3. **작은 증분 리팩터** (한 번에 한 함수)
4. 각 변경 후 **테스트 재실행**
5. 서브에이전트로 **독립 검토**

---

## 🎯 핵심 메타 인사이트

1. **TDD의 자연스러운 파트너가 LLM** — 테스트가 명확한 목표 제공
2. **Context Pollution 개념** — 같은 컨텍스트에서 모든 단계 보면 편향 발생
3. **Hooks로 스킬 활성화율 20% → 84%** — 결정론적 강제 필요
4. **레거시 코드는 이해 먼저, 리팩터 나중** — 테스트 잠금 후 증분
5. **210줄·복잡도 16 → 30줄·복잡도 3-6** — 정량 개선 가능
