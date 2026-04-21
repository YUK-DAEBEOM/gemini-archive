# 카테고리 6: MCP 서버·IDE 통합 전문 (4개)

> 출처: Scott Spence, Builder.io, Docker, JetBrains AI 공식 블로그
> 정리일: 2026-04-21
> MCP 설정과 IDE 통합의 **실전 노하우**.

---

## 🔌 1. Scott Spence — MCP Tools 설정의 Better Way
🔗 https://scottspence.com/posts/configuring-mcp-tools-in-claude-code

### 핵심 주장: CLI보다 설정 파일 직접 편집이 낫다

**CLI 방식의 문제점**
- 오타 발생 시 전체 과정 재시작
- 복잡한 설정(API 키, 경로, 환경변수) 어려움
- 수정 시 여러 프롬프트 반복

**직접 편집의 장점**
- 모든 구성을 한눈에 파악
- 복잡한 설정 관리 용이
- 버전 관리·백업 간편
- 빠른 수정

### 설정 파일 위치
`~/.claude.json`
> ⚠️ WSL 사용자는 **Linux 파일시스템**에 있어야 함 (Windows 마운트 경로 ❌)

### 실전 설정 예시

**기본 (Sequential Thinking)**
```json
"sequential-thinking": {
  "type": "stdio",
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
}
```

**복합 (여러 API 키)**
```json
"mcp-omnisearch": {
  "type": "stdio",
  "command": "npx",
  "args": ["-y", "mcp-omnisearch"],
  "env": {
    "TAVILY_API_KEY": "",
    "BRAVE_API_KEY": ""
  }
}
```

### 적용 후 확인
1. Claude Code **재시작** (필수)
2. `/mcp`로 연결 상태 확인

---

## 🔌 2. Builder.io — MCP 서버 설정 가이드
🔗 https://www.builder.io/blog/claude-code-mcp-servers

### 기본 추가
```bash
claude mcp add --transport http github https://api.githubcopilot.com/mcp/
```

### 3단계 스코프
- **Local** (개인용, 프로젝트별)
- **Project** (팀 공유, `.mcp.json`)
- **User** (모든 프로젝트)

### 추천 MCP 서버 TOP 5
1. **GitHub** — PR 검토·이슈 관리
2. **Playwright** — 브라우저 자동화 테스트
3. **Sentry** — 프로덕션 에러 모니터링
4. **PostgreSQL/Supabase** — 데이터베이스 접근
5. **Figma** — 디자인 투 코드

### ⭐ 핵심 팁: Tool Search로 토큰 85% 절감
> Tool Search 활성화 → 컨텍스트 사용량 **72,000 → 8,700 토큰** (85% 감축)

Sonnet 4 이상 모델에서 **자동 활성화**.

---

## 🔌 3. Docker — MCP Toolkit 연동
🔗 https://www.docker.com/blog/add-mcp-servers-to-claude-code-with-mcp-toolkit/

### Docker MCP Toolkit이란?
**200+ 사전 구축된 컨테이너화 MCP 서버** 플랫폼. Jira·GitHub·DB 등을 원클릭 연결.

### 5대 장점
1. **의존성 관리 불필요** — 컨테이너 격리
2. **원클릭 배포** — Docker Desktop에서 간단 설정
3. **보안 자격증명 관리** — OAuth + 암호화 저장
4. **플랫폼 일관성** — Mac/Windows/Linux 동일
5. **자동 업데이트**

### 빠른 설정
```bash
curl -fsSL https://claude.ai/install.sh | sh
docker mcp client connect claude-code
claude code
```

### 실제 사례: TODO-to-Jira 자동화
Claude Code가 자동 수행:
1. **Filesystem MCP**: 코드베이스에서 TODO/FIXME 추출
2. **GitHub MCP**: git blame으로 작성자·시간 파악
3. **Atlassian MCP**: 우선순위별 Jira 티켓 생성

→ 수작업 **20-30분 → 자동화 2분**

### 컨테이너화의 숨은 이점
개발자마다 **2-6시간 소비되던 설정**이 분 단위로 완료. Node.js 버전·Python 의존성·평문 자격증명 걱정 없음.

---

## 🔌 4. JetBrains AI Blog — Claude Agent 통합
🔗 https://blog.jetbrains.com/ai/2025/09/introducing-claude-agent-in-jetbrains-ides/

### 핵심
JetBrains + Anthropic 파트너십으로 **첫 번째 서드파티 에이전트** 통합. JetBrains AI 구독에 **추가 비용 없음**.

### 주요 기능
- IDE 내 AI 채팅에서 **직접 접근**
- 다중 파일 작업·diff 미리보기·승인 기반
- **Plan 모드**로 단계별 전략 미리 확인
- Claude 4.5 Sonnet 기반 (당시)

### Claude Code CLI vs JetBrains AI 에이전트

| 구분 | Claude Code CLI | JetBrains AI Claude Agent |
|------|----------------|--------------------------|
| 설치 | 별도 설치 필요 | JetBrains AI 구독에 포함 |
| 로그인 | Anthropic 계정 | JetBrains 계정 (통합) |
| UI | 터미널 / 플러그인 | AI 채팅 네이티브 |
| 모델 | 모든 Claude 모델 | Sonnet 4.5 (당시) |

### 사용법
AI 채팅 열기 → **Claude Agent 선택** → 자동 다운로드 및 초기화

---

## 🎯 종합 적용 가이드

### 개인 개발자 추천 조합
```bash
# 기본: 직접 파일 편집 방식 (Scott Spence)
~/.claude.json에 3-5개 MCP만 등록:
- sequential-thinking (추론 강화)
- github
- playwright (UI 테스트)
- 프로젝트 DB (Supabase/PostgreSQL)

# 토큰 절감: Tool Search 자동 활성화 확인
/context 명령으로 MCP가 정말 deferred 로딩인지 확인
```

### 팀 프로젝트
```bash
# .mcp.json에 팀 공통 MCP 등록 (Project 스코프)
# 자격증명은 .env에서 환경변수로 주입
# team member가 clone 후 즉시 사용 가능
```

### 엔터프라이즈
```bash
# Docker MCP Toolkit 권장
# - 200+ 사전 빌드 서버
# - OAuth 중앙 관리
# - 개발자 온보딩 시간 2-6시간 → 2분
```

### IDE 환경별 선택
| 환경 | 권장 |
|------|------|
| **Vim·터미널 우선** | Claude Code CLI |
| **VS Code** | Claude Code 확장 + CLI 병행 |
| **JetBrains 전용** | JetBrains AI Claude Agent (통합 UX) |
| **복수 IDE 팀** | CLAUDE.md 공유 + 각자 선호 IDE |

---

## 🚨 공통 함정 5가지

1. **MCP 설정 변경 후 재시작 안 함** — `/mcp`로 재연결 확인 필수
2. **WSL에서 Windows 경로 사용** — Linux 파일시스템 내부에 배치
3. **MCP 너무 많이 추가** — deferred 로딩이어도 tool list가 컨텍스트 소비
4. **자격증명을 JSON에 평문 저장** — 환경변수 또는 Docker 비밀로
5. **JetBrains AI vs Claude Code CLI 혼동** — 서로 다른 제품, 라이선스도 다름
