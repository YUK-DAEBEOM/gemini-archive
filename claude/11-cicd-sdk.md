# 카테고리 10: CI/CD·Headless·SDK 전문 (3개)

> 출처: systemprompt.io, MindStudio, DataCamp
> 정리일: 2026-04-21
> **자동화·프로그래밍 방식** 활용 집중.

---

## 🤖 1. systemprompt.io — GitHub Actions 실전 셋업
🔗 https://systemprompt.io/guides/claude-code-github-actions

### 3단계 셋업

**① API 키 설정**
Settings → Secrets and variables → Actions에 `ANTHROPIC_API_KEY` 추가.

**② 워크플로우 파일**
`.github/workflows/claude.yml`:
```yaml
on:
  pull_request:
    types: [opened, synchronize]
jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: anthropics/claude-code-action@v1
```

**③ 프롬프트 최적화**
**"특정하고 실행 가능한 피드백"**에만 집중. 스타일 검토 제외.

### ⚠️ 4가지 함정

1. **비용 폭증** — `synchronize` 사용 시 모든 푸시마다 실행
   → 해결: `types: [opened]`로만 제한
2. **Fork PR 보안** — 기본 시크릿 접근 불가 (안전)
   → `pull_request_target` 사용 시 **주의 필요**
3. **권한 문제** — `contents: write`, `pull-requests: write` 명시 필수
4. **Git 깊이 부족** — `fetch-depth: 0`으로 전체 히스토리

### 5가지 실전 레시피
- PR 리뷰
- 이슈 → PR 자동화
- 문서 갱신
- 테스트 생성
- 릴리스 노트 생성

---

## 🌙 2. MindStudio — Headless Mode 자동화
🔗 https://www.mindstudio.ai/blog/claude-code-headless-mode-autonomous-agents-2

### 개념
> **"pass the prompt as a flag at invocation, Claude runs the task, and the output is returned to stdout"**

### Cron 스케줄링 예시
```bash
0 8 * * * source /home/user/.env && \
  claude -p "작업 내용" >> /var/log/claude.log 2>&1
```

### 핵심 설정 팁
- **전체 경로** 사용 필수 (cron 환경변수 제한)
- 환경변수 **명시적 설정** (`ANTHROPIC_API_KEY`)
- 로그 리다이렉션으로 실행 기록 관리

### 4대 활용 사례
1. **자동 코드 리뷰** — PR마다 보안 검사
2. **의존성 모니터링** — 주간 취약점 스캔
3. **로그 분석** — 오류 패턴 자동 요약
4. **문서 동기화** — README와 코드 비교

### 🚨 보안 경고
> `--dangerously-skip-permissions`는 **"only when you fully understand what task you're giving Claude"**

→ 격리 환경(컨테이너·CI 러너)에서만 사용.

---

## 🛠 3. DataCamp — Claude Agent SDK 튜토리얼
🔗 https://www.datacamp.com/tutorial/how-to-use-claude-agent-sdk

### SDK 개요
**"Give Claude access to a computer"** 원칙. 터미널·파일시스템 접근 제공.

### 주요 기능
- **Python / TypeScript** 지원
- 스트리밍 + 단일 요청 모드
- 내장 도구 (파일·코드 실행·웹 검색)
- **MCP로 커스텀 도구 확장**
- 미세한 권한 제어 + 보안 훅

### 3개 프로젝트 예시

**① 원샷 블로그 개요**
- 도구 없이 기본 사용법

**② InspireBot**
- 웹 검색 + 커스텀 fallback 도구

**③ NoteSmith**
- 다중 도구 + 안전 훅 + 사용량 추적

### 구현 단계

```bash
pip install claude-agent-sdk
```

```python
from claude_agent_sdk import tool, create_sdk_mcp_server

@tool
def my_custom_tool(query: str) -> str:
    # 커스텀 로직
    return result

server = create_sdk_mcp_server(tools=[my_custom_tool])
```

→ MCP 도구로 등록되어 Claude가 호출 가능.

---

## 🎯 자동화 파이프라인 설계 가이드

### 레벨 1: CI/CD 통합
GitHub Actions로 **이벤트 기반** 자동화:
- PR 생성 시 리뷰
- 이슈 생성 시 분석
- 머지 후 릴리스 노트

### 레벨 2: 스케줄 기반
Cron + Headless mode:
- 매일 오전 보안 스캔
- 주간 의존성 감사
- 야간 로그 분석

### 레벨 3: Agent SDK 커스텀
Python/TypeScript로 **완전 제어**:
- 커스텀 도구 정의
- 독자적 에이전트 오케스트레이션
- 다른 시스템과의 깊은 통합

---

## 🚨 공통 함정 5가지

1. **`--dangerously-skip-permissions` 남발** — 격리 환경에서만
2. **비용 모니터링 안 함** — `/cost` 정기 확인 + 워크스페이스 예산
3. **GitHub Actions 트리거 과다** — `synchronize` 대신 `opened`
4. **자격 증명 평문 저장** — 반드시 Secrets 사용
5. **SDK 에이전트 타임아웃 미설정** — 무한 루프 방지

---

## 🎯 3가지 패턴 선택

| 상황 | 권장 |
|------|------|
| 팀 전체 PR 자동화 | GitHub Actions |
| 정기 배치 작업 | Headless + Cron |
| 특수 워크플로우 자동화 | Agent SDK |
