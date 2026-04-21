# 카테고리 1: Claude Code 공식 문서 정리 (22개)

> 출처: `code.claude.com/docs` (영문 + 한국어)
> 정리일: 2026-04-21
> 각 문서의 핵심만 실전 관점에서 압축.

---

## 📦 1. Overview (전체 개요)
🔗 https://code.claude.com/docs/en/overview

- **정의**: 터미널, IDE, 데스크톱 앱, 브라우저에서 동작하는 에이전트형 코딩 도구. 코드 읽기/편집/실행, 개발 도구 통합을 자연어 한 줄로 처리.
- **주요 서피스**: Terminal CLI / VS Code 확장 / JetBrains 플러그인 / Desktop 앱 / Web(claude.ai/code) / iOS 앱 / Slack / GitHub Actions
- **대표 유스케이스**: 테스트 자동 작성, 버그 추적·수정, git 커밋/PR 생성, MCP로 외부 도구 연결, 스케줄링된 정기 작업
- **명령 예**: `claude "write tests for the auth module, run them, and fix any failures"`
- **핵심 포인트**: 모든 서피스에서 동일한 엔진 사용 — CLAUDE.md, settings, MCP가 서피스 간 공유됨

---

## 📦 2. Best Practices (모범 사례 — 가장 중요)
🔗 https://code.claude.com/docs/en/best-practices

### 최상위 원칙: "Context window 관리가 1순위"
Context가 가득 차면 성능 저하 → 이전 지시를 "잊어버림".

### 8가지 핵심 베스트 프랙티스

1. **검증 방법 제공 (가장 높은 영향력)**
   - 테스트, 스크린샷, 예상 출력을 포함해 Claude가 스스로 확인할 수 있게 하라
   - 나쁜 예: "dashboard를 더 예쁘게"
   - 좋은 예: "\[screenshot] 이 디자인 구현 → 결과 스크린샷 찍고 차이점 나열 → 수정"

2. **Explore → Plan → Code → Commit 4단계 워크플로우**
   - Plan Mode(Shift+Tab×2) → 읽기 전용으로 탐색·계획
   - Ctrl+G로 plan 파일을 에디터에서 직접 편집
   - "한 문장으로 요약 가능한 변경"은 plan 건너뛰기

3. **구체적 컨텍스트 제공**
   - ❌ "버그 수정"
   - ✅ "세션 타임아웃 후 로그인 실패 → src/auth/ 토큰 리프레시 확인 → 재현하는 실패 테스트 작성 → 수정"
   - `@file` 참조, 이미지 붙여넣기, URL, `cat x.log | claude` 파이핑

4. **CLAUDE.md 작성**
   - `/init`으로 시작, 200줄 이하 유지
   - 포함할 것: bash 명령, 기본값과 다른 스타일, 테스트 러너, 환경변수, 함정
   - 제외할 것: 코드만 봐도 알 수 있는 것, 표준 언어 규칙, 자주 바뀌는 정보
   - "IMPORTANT"/"YOU MUST" 강조어로 준수 개선
   - 팀 공유를 위해 git 체크인

5. **권한 설정으로 중단 줄이기**
   - Auto mode: 분류기가 위험한 명령만 차단
   - `/permissions`: 안전한 명령 허용 목록
   - `/sandbox`: OS 레벨 격리

6. **확장 기능 활용**
   - CLI 도구(`gh`, `aws`) 사용을 CLAUDE.md에 명시
   - `claude mcp add`로 외부 서비스 연결
   - Hooks로 결정론적 자동화(포맷, 린팅)
   - `.claude/skills/`로 재사용 워크플로우
   - `.claude/agents/`로 서브에이전트 정의

7. **세션 관리**
   - `Esc` (중간 중지), `Esc+Esc`/`/rewind` (체크포인트 복원), `/clear` (컨텍스트 리셋)
   - 같은 문제 2번 수정 실패 → `/clear` 후 더 나은 프롬프트로 새로 시작
   - `/rename`으로 세션에 의미 있는 이름 부여 → `claude --resume`으로 재개

8. **병렬·자동화**
   - `claude -p "prompt"`: 비대화식 (CI, 스크립트)
   - `--worktree` 플래그: 격리된 git worktree에서 병렬 세션
   - Writer/Reviewer 패턴: 한 세션이 구현, 다른 세션이 검토
   - `for file in $(cat files.txt); do claude -p "..." --allowedTools "..."; done`

### 피해야 할 5가지 패턴
1. **Kitchen sink session**: 한 세션에 관련 없는 작업 섞기 → `/clear`
2. **반복 수정**: 두 번 수정 실패 → `/clear` 후 새 프롬프트
3. **과도한 CLAUDE.md**: 길면 핵심 규칙이 묻힘 → 무자비하게 정리
4. **신뢰-검증 간격**: 그럴듯해 보이는 코드 그대로 배포 → 항상 검증
5. **무한 탐색**: 범위 없는 "조사" → 서브에이전트로 격리

---

## 📦 3. Advanced Setup (설치·설정)
🔗 https://code.claude.com/docs/en/setup

- **시스템 요구사항**: macOS 13+, Windows 10 1809+, Ubuntu 20+, Debian 10+, Alpine 3.19+ / RAM 4GB+
- **Native 설치 (권장)**:
  - macOS/Linux/WSL: `curl -fsSL https://claude.ai/install.sh | bash`
  - Windows PowerShell: `irm https://claude.ai/install.ps1 | iex`
  - Windows CMD: `curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd`
  - **⚠️ Windows 네이티브는 Git for Windows 필수** (WSL은 불필요)
- **기타 설치**: Homebrew (`brew install --cask claude-code`), WinGet, npm (`npm install -g @anthropic-ai/claude-code`)
- **검증**: `claude --version`, `claude doctor`
- **인증**: Pro/Max/Team/Enterprise/Console 계정 필요 (무료 플랜 ❌). Bedrock/Vertex/Foundry도 지원
- **업데이트**: 기본 자동 업데이트, `autoUpdatesChannel: "stable"`로 안정 채널 고정 가능, `minimumVersion`으로 최소 버전 강제
- **언인스톨**: `~/.local/bin/claude`, `~/.claude/`, `~/.claude.json`, `.claude/`, `.mcp.json` 제거

---

## 📦 4. Common Workflows (일반 워크플로우)
🔗 https://code.claude.com/docs/en/common-workflows

### 주요 워크플로우 치트시트

| 작업 | 핵심 프롬프트 |
|------|------|
| **코드베이스 탐색** | "이 코드베이스 개요", "주요 아키텍처 패턴", "인증 처리 방식" |
| **버그 수정** | "npm test 실행 시 에러 → user.ts의 @ts-ignore 수정 방법 제시" |
| **리팩터링** | "deprecated API 사용처 찾기 → ES2024 기능으로 리팩터 → 테스트 실행" |
| **Plan Mode** | Shift+Tab×2 / `claude --permission-mode plan` / Ctrl+G로 plan 편집 |
| **테스트 작성** | "NotificationsService.swift에서 테스트 없는 함수 찾기 → 엣지 케이스 테스트 추가" |
| **PR 생성** | "create a pr" (`gh pr create` 후 `claude --from-pr <number>`로 재개 가능) |
| **이미지 분석** | 드래그 앤 드롭, Ctrl+V (Cmd+V 아님), 경로 지정 → "이 스크린샷의 에러 원인?" |
| **파일 참조** | `@src/utils/auth.js` (파일), `@src/components/` (디렉토리), `@github:repos/...` (MCP) |
| **확장 사고** | Ctrl+O로 verbose 모드, Option+T(Alt+T)로 토글, `ultrathink` 키워드 |
| **세션 재개** | `claude --continue`, `claude --resume`, `claude --from-pr 123`, `claude -n auth-refactor` |

### Git Worktree 병렬 작업 ⭐
```bash
claude --worktree feature-auth  # .claude/worktrees/feature-auth/ 생성
claude -w bugfix-123            # 단축 플래그
claude --worktree               # 랜덤 이름 생성
```
- 기본 브랜치는 `origin/HEAD` 기준 (재동기화: `git remote set-head origin -a`)
- `.gitignore`에 `.claude/worktrees/` 추가 권장
- `.worktreeinclude`로 `.env` 등 untracked 파일 자동 복사

### 알림 Hook (macOS)
```json
{
  "hooks": {
    "Notification": [{
      "matcher": "",
      "hooks": [{
        "type": "command",
        "command": "osascript -e 'display notification \"Claude Code needs your attention\" with title \"Claude Code\"'"
      }]
    }]
  }
}
```

### Unix 스타일 유틸로 사용
- `tail -200 app.log | claude -p "이상 징후 있으면 슬랙으로"`
- `git diff main --name-only | claude -p "보안 이슈 검토"`
- `--output-format json` / `stream-json` / `text`로 출력 제어

---

## 📦 5. Settings.json (설정 레퍼런스)
🔗 https://code.claude.com/docs/en/settings

### 4단계 계층 (상위가 우선)
| 계층 | 경로 | 공유 |
|------|------|------|
| **Managed** | 시스템 레벨 | IT 배포 |
| **Local** | `.claude/settings.local.json` | ❌ gitignore |
| **Project** | `.claude/settings.json` | ✅ git 커밋 |
| **User** | `~/.claude/settings.json` | ❌ 개인 |

### 핵심 필드
```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "permissions": {
    "allow": ["Bash(npm run *)", "Bash(git *)", "Read(./src/**)"],
    "ask":   ["Bash(git push *)"],
    "deny":  ["Bash(curl *)", "Read(./.env)", "Read(./.env.*)"],
    "defaultMode": "default",
    "additionalDirectories": ["../docs/"]
  },
  "env": { "NODE_ENV": "development", "CLAUDE_CODE_ENABLE_TELEMETRY": "1" },
  "hooks": { /* ... */ },
  "model": "claude-sonnet-4-6",
  "effortLevel": "high",
  "language": "korean",
  "sandbox": {
    "enabled": true,
    "network": { "allowedDomains": ["github.com", "*.npmjs.org"] }
  }
}
```

### 권한 규칙 평가 순서
`deny` → `ask` → `allow` (먼저 매칭되는 규칙이 적용)

### 권한 패턴
- `Bash` — 모든 bash 명령
- `Bash(npm run *)` — 접두사 매칭
- `Read(./.env)` — 특정 파일 차단
- `WebFetch(domain:example.com)` — 도메인 허용
- `mcp__<server>__<tool>` — MCP 도구

### 실전 팁
- **배열 필드는 모든 계층에서 병합됨** (덮어쓰지 않음)
- `$schema` 추가 시 IDE 자동완성·검증 지원
- `/status`로 활성 설정 확인, `/config`로 UI 열기

---

## 📦 6. Hooks (훅 레퍼런스)
🔗 https://code.claude.com/docs/en/hooks

### 8개 Hook Event (생명주기 순)
1. **SessionStart** — 세션 시작/재개 (matcher: startup/resume/clear/compact)
2. **UserPromptSubmit** — 사용자 프롬프트 제출 시 검증/차단
3. **PreToolUse** ⭐ — 도구 실행 직전 (allow/deny/ask/defer)
4. **PostToolUse** — 도구 성공 후 검증·로깅
5. **PostToolUseFailure** — 도구 실패 후 복구
6. **PermissionRequest** — 권한 다이얼로그
7. **Stop** — Claude 응답 종료
8. **SessionEnd** — 세션 종료

### Hook Type 4가지
- `command` (셸 명령, 90% 케이스)
- `http` (웹훅 POST)
- `prompt` (Claude 단일 턴 yes/no 평가)
- `agent` (서브에이전트 스폰)

### Exit Code
- `0` → 성공, JSON 출력 처리
- `2` → 차단 오류, stderr 표시
- `1, 기타` → 비차단 오류

### 실전 예시: 위험 명령 차단
```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash",
      "hooks": [{
        "type": "command",
        "if": "Bash(rm *)",
        "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/block-rm.sh"
      }]
    }]
  }
}
```

### 실전 예시: 편집 후 자동 린팅
```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{ "type": "command", "command": "npm run lint", "timeout": 30 }]
    }]
  }
}
```

### 확인: `/hooks` 명령으로 설정 브라우저 열림

---

## 📦 7. MCP (Model Context Protocol)
🔗 https://code.claude.com/docs/en/mcp

- **정의**: Claude를 외부 도구/DB/API에 연결하는 오픈 표준. Claude Code는 MCP 클라이언트
- **빠른 추가**: `claude mcp add <name> <command>` (환경변수는 `-e KEY=value`)
- **스코프**: Project(팀 공유, `.mcp.json`) / User(개인, `~/.claude.json`) / Local(현 프로젝트 개인)
- **대표 서버**: GitHub, GitLab, Notion, Jira/Confluence, Linear, Figma, Slack, Sentry, Supabase, Vercel, Firebase
- **리소스 참조**: `@server:resource` 형식 (예: `@github:repos/owner/repo/issues`)
- **관리**: `/mcp`로 연결 상태 확인, 재연결, OAuth 인증
- **⚠️ 주의**: 설정 변경 후 Claude Code 재시작 필수
- **토큰 최적화**: MCP 도구는 기본적으로 deferred (이름만 로드). `ENABLE_TOOL_SEARCH=auto/false`로 제어

---

## 📦 8. Subagents (서브에이전트)
🔗 https://code.claude.com/docs/en/sub-agents

### 정의
- 격리된 context·시스템 프롬프트·도구 세트를 가진 별도 Claude 인스턴스
- 메인 대화를 오염시키지 않고 탐색·리서치 위임 가능

### 파일 위치·구조
`.claude/agents/<name>.md`
```markdown
---
name: security-reviewer
description: Reviews code for security vulnerabilities
tools: Read, Grep, Glob, Bash
model: opus
isolation: worktree  # 선택: 자체 worktree 사용
---
당신은 선임 보안 엔지니어입니다. 다음을 검토:
- 주입 취약점 (SQL, XSS, 명령 주입)
- 인증·권한 결함
- 비밀/자격 증명 노출
```

### 호출 방법
- 자동 위임: description 기반
- 명시적: `"use the security-reviewer subagent to ..."`
- `/agents`로 목록·생성 UI

### Agent Teams와 차이
- Subagents: 단일 세션 내 작업
- Agent Teams: 세션 간 조정 (messaging, shared tasks)

---

## 📦 9. Skills (스킬)
🔗 https://code.claude.com/docs/en/skills

### 정의
- 필요할 때만 로드되는 Markdown 기반 워크플로우 (CLAUDE.md와 달리 on-demand)
- `.claude/commands/`와 통합됨 (기존 파일은 그대로 작동)

### 위치 우선순위
Enterprise > Personal(`~/.claude/skills/`) > Project(`.claude/skills/`) > Plugin

### SKILL.md 구조
```yaml
---
name: api-conventions
description: REST API 설계 규칙 — API 엔드포인트 작성 시 사용
disable-model-invocation: false  # true면 Claude 자동 호출 차단
user-invocable: true              # false면 사용자 호출 차단
allowed-tools: Bash(git *) Read   # 자동 승인 도구
paths: ["src/api/**/*.ts"]        # 경로 기반 자동 로딩
context: fork                     # 서브에이전트에서 실행
agent: Explore
---
When writing API endpoints:
- Use kebab-case for URL paths
- Return consistent error formats
```

### 고급 기능
- **인자**: `$ARGUMENTS`, `$0`, `$1`, `${CLAUDE_SESSION_ID}`, `${CLAUDE_SKILL_DIR}`
- **Supporting files**: `reference.md`, `examples.md`, `scripts/` 번들
- **Dynamic context**: `` !`<command>` `` 또는 ` ```! ` 블록으로 실행 결과 주입
- **실시간 감지**: 재시작 없이 추가/수정 반영

### 호출
- 자동: description에 매칭되는 요청
- 수동: `/skill-name [arguments]`

### Skills vs Subagents vs Hooks vs CLAUDE.md
| 도구 | 로딩 시점 | 용도 |
|------|---------|------|
| CLAUDE.md | 매 세션 시작 | 항상 필요한 프로젝트 규칙 |
| Skills | on-demand | 때때로 필요한 도메인 지식/워크플로우 |
| Subagents | 위임 시 | 격리된 컨텍스트 탐색 |
| Hooks | 이벤트 발생 시 | 결정론적 자동화 |

---

## 📦 10. Memory (CLAUDE.md·Auto Memory)
🔗 https://code.claude.com/docs/en/memory

### 두 가지 메모리 시스템
|  | CLAUDE.md | Auto Memory |
|---|---|---|
| 작성자 | 사용자 | Claude 자신 |
| 내용 | 규칙·지시 | 학습한 패턴·빌드 명령 |
| 범위 | 프로젝트/사용자/조직 | worktree별 |
| 로딩 | 매 세션 전체 | 첫 200줄 또는 25KB |

### CLAUDE.md 위치 계층
| 범위 | 경로 | 용도 |
|------|------|------|
| Managed (정책) | macOS: `/Library/Application Support/ClaudeCode/CLAUDE.md`<br>Linux: `/etc/claude-code/CLAUDE.md`<br>Windows: `C:\Program Files\ClaudeCode\CLAUDE.md` | 조직 정책 |
| Project | `./CLAUDE.md` 또는 `./.claude/CLAUDE.md` | 팀 공유 |
| User | `~/.claude/CLAUDE.md` | 모든 프로젝트 |
| Local | `./CLAUDE.local.md` | 개인 (gitignore) |

**⚠️ 모든 파일이 concatenate됨 — 덮어쓰지 않음**

### Import 문법
```markdown
See @README.md for overview and @package.json for commands.

# Additional Instructions
- Git workflow: @docs/git-instructions.md
- Personal overrides: @~/.claude/my-project-instructions.md
```
최대 5 hop 재귀 import 지원.

### AGENTS.md 호환
```markdown
@AGENTS.md

## Claude Code
Use plan mode for changes under `src/billing/`.
```

### `.claude/rules/` (경로별 규칙)
```markdown
---
paths:
  - "src/api/**/*.ts"
  - "lib/**/*.{ts,tsx}"
---
# API 개발 규칙
- 모든 엔드포인트 입력 검증
- 표준 에러 응답 포맷
```

### Auto Memory
- **위치**: `~/.claude/projects/<project>/memory/` (git repo 기준)
- **구조**: `MEMORY.md` (인덱스, 200줄 제한) + 토픽 파일들 (on-demand)
- **제어**: `/memory` 명령, `autoMemoryEnabled: false`, `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1`
- **사용자 요청 저장**: "always use pnpm, not npm" → Auto Memory에 저장
- **수동 편집**: 모두 평문 markdown, 직접 수정 가능

### 트러블슈팅
- `/memory`로 로드된 파일 목록 확인
- 너무 긴 CLAUDE.md → `.claude/rules/` 또는 skills로 분리
- `/compact` 후 지시 손실 → 루트 CLAUDE.md는 자동 재주입됨

---

## 📦 11. Context Window
🔗 https://code.claude.com/docs/en/context-window

- **크기**: Opus 4.7은 1M 토큰 (Pro/Max/Team/Enterprise), 기본 200k
- **주요 명령**: `/context` (사용량 상세), `/clear` (리셋), `/compact [instructions]` (요약 압축)
- **모니터링**: 터미널 하단 토큰 %, 커스텀 statusline 지원
- **컴팩션이 보존하는 것**: 코드 패턴, 파일 상태, 주요 결정. 루트 CLAUDE.md는 자동 재주입
- **절약 전략**: `/clear` 빈번 사용, 서브에이전트 위임, MCP deferred tools, CLI 도구(`gh`) 선호

---

## 📦 12. Checkpointing
🔗 https://code.claude.com/docs/en/checkpointing

- **자동 스냅샷**: 매 사용자 프롬프트마다 (Write/Edit/NotebookEdit 도구만 추적)
- **열기**: `Esc + Esc` 또는 `/rewind`
- **옵션**: 대화만 / 코드만 / 둘 다 복원 / 선택 메시지부터 요약 (`Summarize from here`)
- **보존**: 세션 간 30일 (설정 가능)
- **⚠️ 한계**: bash 명령(`rm`, `mv`, `cp`)으로 수정된 파일은 추적 불가. git 대체 아님

---

## 📦 13. Headless Mode (`claude -p`)
🔗 https://code.claude.com/docs/en/headless

### 기본 사용
```bash
claude -p "Find and fix the bug in auth.py" --allowedTools "Read,Edit,Bash"
```

### 핵심 플래그
- `--bare` — 시작 시간 단축 (hooks/skills/plugins/MCP 자동 발견 skip) — **CI 권장**
- `--output-format text|json|stream-json` — 출력 구조
- `--json-schema <schema>` — 구조화된 출력 보장
- `--allowedTools "Read,Edit,Bash"` — 권한 사전 승인
- `--permission-mode acceptEdits|auto|dontAsk` — 세션 전체 모드
- `--continue` / `--resume <session-id>` — 대화 재개
- `--append-system-prompt "..."` — 시스템 프롬프트 추가
- `--verbose --include-partial-messages` — 스트리밍 + 디버그

### 실전 예시
```bash
# PR diff 보안 리뷰
gh pr diff "$1" | claude -p \
  --append-system-prompt "You are a security engineer. Review for vulnerabilities." \
  --output-format json

# 세션 ID 캡처하고 재개
session_id=$(claude -p "Start a review" --output-format json | jq -r '.session_id')
claude -p "Continue that review" --resume "$session_id"

# JSON Schema로 구조화된 출력
claude -p "Extract function names from auth.py" \
  --output-format json \
  --json-schema '{"type":"object","properties":{"functions":{"type":"array","items":{"type":"string"}}}}'
```

### `system/api_retry` 이벤트
스트리밍 중 API 재시도 상황을 프로그램적으로 감지 가능.

---

## 📦 14. GitHub Actions
🔗 https://code.claude.com/docs/en/github-actions

### 빠른 설치
1. Claude Code 터미널에서 `/install-github-app` 실행 (리포 admin 필수)
2. `ANTHROPIC_API_KEY` 시크릿 추가
3. `.github/workflows/claude.yml` 배치

### v1.0 주요 변경 (Beta → GA)
| Beta | v1.0 |
|------|------|
| `mode: "tag"` | 자동 감지 |
| `direct_prompt` | `prompt` |
| `custom_instructions` | `claude_args: --append-system-prompt` |
| `max_turns`, `model`, `allowed_tools` | `claude_args: ...` |

### 기본 워크플로우
```yaml
name: Claude Code
on:
  issue_comment: { types: [created] }
  pull_request_review_comment: { types: [created] }
jobs:
  claude:
    runs-on: ubuntu-latest
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
```

### 활용 패턴
- **대화형**: 이슈/PR에서 `@claude` 멘션으로 트리거
- **자동화**: `prompt:`로 매 이벤트에 헤드리스 실행
- **일일 리포트**: `cron: "0 9 * * *"`로 커밋·이슈 요약
- **PR 보안 리뷰**: `pull_request: types: [opened, synchronize]` 이벤트

### 엔터프라이즈 (Bedrock/Vertex)
- Workload Identity Federation / OIDC 사용 (정적 키 ❌)
- `use_bedrock: "true"` 또는 `use_vertex: "true"` 플래그

---

## 📦 15. VS Code Extension
🔗 https://code.claude.com/docs/en/vs-code

### 설치·시작
- Marketplace 검색 "Claude Code" 또는 Ctrl/Cmd+Shift+X
- 필요: VS Code 1.98+
- Spark 아이콘(편집기 우상단), Activity Bar, Status Bar, Command Palette에서 열기

### 주요 기능
- **인라인 diff**: 수락/거부 전 나란히 비교, 직접 편집 가능
- **@-mention**: 파일/폴더 참조, 퍼지 매칭 (`@auth` → `auth.js`, `AuthService.ts`)
- **선택 컨텍스트**: 선택 텍스트 자동 전달 (Option/Alt+K로 `@file#L5-10` 삽입)
- **Session 히스토리**: 검색/리네임, Claude.ai 웹 세션 재개(Remote 탭)
- **Plan 리뷰**: 마크다운으로 열어 인라인 코멘트로 피드백

### 단축키
| 기능 | 단축키 |
|------|--------|
| Focus 토글 (편집기↔Claude) | Cmd/Ctrl+Esc |
| 새 탭 열기 | Cmd/Ctrl+Shift+Esc |
| @-mention 삽입 | Option/Alt+K |

### URI 핸들러
```bash
open "vscode://anthropic.claude-code/open?prompt=review%20my%20changes"
```

### 주요 설정
- `useTerminal: true` — CLI 모드
- `initialPermissionMode: "plan"` — 기본 플랜 모드
- `preferredLocation: "sidebar"` — 사이드바 고정
- `allowDangerouslySkipPermissions` — Auto mode 활성화

### CLI vs Extension
CLI가 더 많은 기능 지원 (tab completion, `!` bash shortcut, 전체 commands).
확장에서 CLI 기능 쓰려면 integrated terminal (Cmd/Ctrl+\`)에서 `claude` 실행.

### Chrome 브라우저 연동
`@browser`로 Claude in Chrome 확장 통해 웹앱 테스트·콘솔 확인 가능.

---

## 📦 16. JetBrains IDEs
🔗 https://code.claude.com/docs/en/jetbrains

- **지원 IDE**: IntelliJ, PyCharm, Android Studio, WebStorm, PhpStorm, GoLand
- **설치**: JetBrains Marketplace → "Claude Code" 검색 → 재시작 (여러 번 필요할 수 있음)
- **주요 기능**:
  - Cmd/Ctrl+Esc로 빠른 실행
  - IDE diff viewer에서 변경 확인
  - 현재 선택/탭 자동 공유
  - Cmd+Option+K (Mac) / Alt+Ctrl+K (Win/Linux)로 `@File#L1-99` 삽입
  - Diagnostics(lint/syntax 에러) 자동 공유
- **외부 터미널 연결**: `/ide` 명령으로 수동 연결
- **설정**: Settings → Tools → Claude Code [Beta]
- **ESC 문제**: Settings → Tools → Terminal → "Move focus to editor with Escape" 체크 해제
- **⚠️ Remote Dev**: 플러그인은 Remote Host에 설치해야 함 (로컬 아님)
- **WSL**: Claude command를 `wsl -d Ubuntu -- bash -lic "claude"`로 설정

---

## 📦 17. Output Styles (출력 스타일)
🔗 https://code.claude.com/docs/en/output-styles

### 내장 스타일
- **Default** — 소프트웨어 엔지니어링 최적화
- **Explanatory** — "Insights"로 코드베이스/구현 설명 추가
- **Learning** — 대화형 학습, `TODO(human)` 마커로 사용자가 직접 코드 작성하도록 유도

### 사용
- `/config` → Output style 선택 (`.claude/settings.local.json`에 저장)
- 직접 설정: `"outputStyle": "Explanatory"`

### 커스텀 스타일
위치: `~/.claude/output-styles/` (user) 또는 `.claude/output-styles/` (project)
```markdown
---
name: My Custom Style
description: Brief description
keep-coding-instructions: false  # true면 코딩 지시 유지
---
# Custom Style Instructions
You are an interactive CLI tool...
```

### Output Styles vs 다른 기능
- **vs CLAUDE.md**: 스타일은 시스템 프롬프트 교체, CLAUDE.md는 유저 메시지로 추가
- **vs Agents**: 스타일은 메인 루프에만 영향, Agents는 tools/model까지 설정
- **vs Skills**: 스타일은 항상 활성, Skills는 on-demand

---

## 📦 18. Plugins & Marketplace
🔗 https://code.claude.com/docs/en/discover-plugins

### 공식 마켓플레이스 (`claude-plugins-official`)
자동 사용 가능. `/plugin` → Discover 탭 또는 `claude.com/plugins`에서 조회.

### 설치
```bash
/plugin install github@claude-plugins-official
```

### 카테고리별 대표 플러그인
- **Code Intelligence** (LSP 통합): `typescript-lsp`, `pyright-lsp`, `rust-analyzer-lsp`, `gopls-lsp` 등
- **External Integrations**: `github`, `gitlab`, `atlassian`, `linear`, `notion`, `figma`, `vercel`, `sentry`
- **Development Workflows**: `commit-commands`, `pr-review-toolkit`, `plugin-dev`
- **Output Styles**: `explanatory-output-style`, `learning-output-style`

### 마켓플레이스 추가
- GitHub: `/plugin marketplace add anthropics/claude-code`
- URL: `/plugin marketplace add https://gitlab.com/org/plugins.git#v1.0.0`
- 로컬: `/plugin marketplace add ./my-marketplace`

### 설치 스코프
- User scope (기본) / Project scope (`.claude/settings.json`) / Local scope

### 관리
- `/plugin` → Installed 탭 (disable/enable/uninstall)
- `/reload-plugins` — 재시작 없이 변경 적용
- `/plugin marketplace update <name>` — 카탈로그 새로고침

### 팀 마켓플레이스
```json
{
  "extraKnownMarketplaces": {
    "my-team-tools": {
      "source": { "source": "github", "repo": "your-org/claude-plugins" }
    }
  }
}
```

### Code Intelligence 효과
1. **자동 diagnostics**: 편집 후 LSP가 타입 에러/import 누락 즉시 리포트
2. **정확한 심볼 네비게이션**: grep 대체

---

## 📦 19. Costs (비용 관리)
🔗 https://code.claude.com/docs/en/costs

### 벤치마크
- 엔터프라이즈 평균: **active day당 $13, 월 $150-250/개발자**
- 90% 사용자는 $30/day 이하

### 비용 추적
- `/cost` — 세션 토큰 사용량 + 추정 비용 (API 사용자 대상)
- `/stats` — 구독자용 사용 패턴
- Console의 Usage 페이지 — 권위 있는 청구 정보

### 팀 레이트 리밋 권장 (per user)
| 팀 규모 | TPM | RPM |
|--------|-----|-----|
| 1-5명 | 200-300k | 5-7 |
| 20-50명 | 50-75k | 1.25-1.75 |
| 500+명 | 10-15k | 0.25-0.35 |

### 토큰 절감 9가지 전략
1. **`/clear` 빈번**: 관련 없는 작업 간 리셋. `/rename` 후 `/resume`으로 복귀 가능
2. **`/compact` 커스텀**: `/compact Focus on code samples and API usage`
3. **모델 선택**: 대부분 Sonnet으로 충분, Opus는 복잡한 아키텍처·멀티스텝 추론에만
4. **서브에이전트 모델 지정**: 단순 작업엔 `model: haiku`
5. **MCP 최소화**: `/context` 확인, CLI 도구(`gh`) 선호, `/mcp`로 미사용 비활성화
6. **Code Intelligence 플러그인**: grep 대신 LSP로 정확한 네비게이션
7. **Hook으로 전처리**: 10,000줄 로그 → grep ERROR → 수백 토큰만 전달
8. **CLAUDE.md → Skills 이동**: on-demand 로딩
9. **Extended Thinking 조정**: 단순 작업엔 `/effort` 낮추거나 `MAX_THINKING_TOKENS=8000`

### Agent Teams 비용 주의
**약 7배 토큰 사용** (각 팀원이 자체 context). `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` 필요. Sonnet 사용, 작게 유지.

### 프롬프트 캐싱
자동 적용 — 시스템 프롬프트 등 반복 콘텐츠 비용 절감.

---

## 📦 20. Changelog (최근 주요 업데이트 TOP 5)
🔗 https://code.claude.com/docs/en/changelog

1. **v2.1.111 — Opus 4.7 + Auto Mode**: Max 구독자 Auto mode, `xhigh` effort 레벨
2. **v2.1.110 — Fullscreen 무깜빡임**: `/tui fullscreen` 명령
3. **v2.1.84 — PowerShell Tool 정식**: Windows 사용자용 네이티브 PS 도구
4. **v2.1.76 — MCP 엘리시테이션**: 실행 중 대화형 폼/URL 입력 요청
5. **v2.1.75 — 1M 컨텍스트 기본**: Max/Team/Enterprise 플랜에서 Opus 기본

---

## 📦 21. 한국어 Quickstart
🔗 https://code.claude.com/docs/ko/quickstart

### 8단계 시작 가이드
1. **설치**: `curl -fsSL https://claude.ai/install.sh | bash`
2. **로그인**: `claude` 실행 → 브라우저 인증 (또는 `/login`)
3. **첫 세션**: 프로젝트 폴더에서 `claude`
4. **첫 질문**: "이 프로젝트는 무엇을 하나요?"
5. **첫 코드 변경**: "주 파일에 hello world 함수 추가"
6. **Git 활용**: "어떤 파일을 변경했나요?", "설명적인 메시지로 변경 사항 커밋"
7. **버그 수정/기능 추가**: "사용자 등록 양식에 입력 유효성 검사 추가"
8. **리팩터/테스트/문서**: 자연어로 요청

### 필수 명령
| 명령 | 설명 |
|------|------|
| `claude` | 대화형 모드 |
| `claude -p "query"` | 일회성 실행 후 종료 |
| `claude -c` | 최근 대화 계속 |
| `claude -r` | 대화 재개 picker |
| `/clear` | 대화 기록 지우기 |
| `/help` | 명령 도움말 |

### 초보자 팁
- 구체적으로 요청 ("로그인 버그 수정" ❌ → "빈 자격 증명 입력 후 빈 화면이 보이는 로그인 버그 수정" ✅)
- 복잡 작업은 단계별로 분해
- 먼저 탐색하도록 지시 (`"데이터베이스 스키마 분석"` 후 `"대시보드 구축"`)
- `?`로 단축키, Tab으로 명령 완성, ↑로 기록

---

## 📦 22. 한국어 Best Practices
🔗 https://code.claude.com/docs/ko/best-practices

[영문 Best Practices의 한국어 번역본] — 위 #2 내용과 동일. 한국어로 참조하고 싶을 때 활용.

핵심 5가지 권장사항 (한국어):
1. **검증 방법 제공** — 테스트/스크린샷/예상 출력으로 Claude가 자가 검증 가능하게
2. **탐색 → 계획 → 코드 → 커밋** 4단계 워크플로우 준수
3. **구체적 컨텍스트** — `@파일`, 스크린샷, 구체적 파일 경로·시나리오 명시
4. **CLAUDE.md 작성** — `/init`으로 시작, 200줄 이하, 검토/정리 반복
5. **컨텍스트 적극 관리** — `/clear` 빈번, 서브에이전트 위임, `/compact` 활용

---

## ✅ 학습 권장 순서

**1단계 — 설치·시작 (30분)**
→ 21(한국어 Quickstart) → 3(Setup) → 1(Overview)

**2단계 — 핵심 사용법 (1-2시간)**
→ 22(한국어 Best Practices) → 4(Common Workflows) → 10(Memory/CLAUDE.md)

**3단계 — 커스터마이징 (1시간)**
→ 5(Settings) → 17(Output Styles) → 19(Costs)

**4단계 — 확장 (2-3시간)**
→ 9(Skills) → 8(Subagents) → 6(Hooks) → 7(MCP)

**5단계 — 자동화·통합 (2시간)**
→ 11(Context) → 12(Checkpointing) → 13(Headless) → 14(GitHub Actions)

**6단계 — IDE (30분)**
→ 15(VS Code) 또는 16(JetBrains)

**7단계 — 고급**
→ 18(Plugins) → 2(Best Practices 전체) → 20(Changelog)
