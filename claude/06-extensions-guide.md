# 카테고리 5: 슬래시 명령·Hooks·Skills·출력 스타일 전문 가이드

> 출처: BioErrorLog, DataCamp, disler/hooks-mastery, DEV.to, FreeCodeCamp, TowardsDataScience, awesome-claude-code-output-styles
> 정리일: 2026-04-21
> Claude Code **확장 기능의 바로 복붙 가능한 실전 예시** 총집합.

---

## 🔹 1. 커스텀 슬래시 명령

### 📚 1-1. BioErrorLog — Custom Slash Commands 기초
🔗 https://en.bioerrorlog.work/entry/claude-code-custom-slash-command

#### 파일 위치
- **프로젝트 명령**: `.claude/commands/<name>.md` (git 커밋, 팀 공유)
- **개인 명령**: `~/.claude/commands/<name>.md` (모든 프로젝트)

#### YAML Frontmatter

```markdown
---
allowed-tools: Bash(git status:*), Bash(git diff:*)
argument-hint: [message]
description: 커맨드 설명
model: claude-3-5-haiku-20241022
---
```

#### 인자 전달
- 위치 인자: `$1`, `$2`
- 전체 인자: `$ARGUMENTS`
- 파일 참조: `@경로명`
- 실행 결과 주입: `!명령어`

#### 실전 예시

**간단한 분석 명령 (`.claude/commands/analyze.md`)**
```markdown
코드를 분석하고 아키텍처를 설명하세요.
```
→ `/analyze`로 실행

**인자 받는 이슈 수정 명령 (`.claude/commands/fix-issue.md`)**
```markdown
이슈 #$1을 $2 우선순위로 해결하세요.
```
→ `/fix-issue 123 high`로 실행

**Git 상태 + diff 자동 주입**
```markdown
---
description: Generate commit message from current changes
---
다음 git 상태:
!git status
다음 git diff:
!git diff --cached
위 변경사항에 대한 Conventional Commit 형식 커밋 메시지 작성.
```

---

## 🔹 2. Hooks — 결정론적 자동화

### 📚 2-1. DataCamp — Practical Hooks Guide
🔗 https://www.datacamp.com/tutorial/claude-code-hooks

#### Hook 이벤트 5종 핵심

| 이벤트 | 실행 시점 | 대표 용도 |
|--------|----------|----------|
| **PreToolUse** | 작업 전 | 위험 파일 수정 차단 |
| **PostToolUse** | 작업 후 | 포맷팅, 테스트 실행 |
| **UserPromptSubmit** | 프롬프트 제출 | 프롬프트 로깅 |
| **Notification** | 알림 발송 | 시스템 알림 처리 |
| **SessionStart/End** | 세션 시작/종료 | 초기화, 정리 |

#### Exit Code 의미

| 코드 | 동작 | 용도 |
|------|------|------|
| **0** | 성공, 표준 출력 표시 | 일반 로깅/알림 |
| **2** | 작업 차단 + stderr 메시지 전송 | 보안 검사 |
| **3** | 지연 실행 | 조건부 처리 |

#### 실전 예시 1: 코드 포맷팅
```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write",
      "hooks": [{
        "type": "command",
        "command": "python -m black ."
      }]
    }]
  }
}
```

#### 실전 예시 2: 보안 검사 (Python)
```python
#!/usr/bin/env python3
import json, sys

input_data = json.load(sys.stdin)
file_path = input_data.get("tool_input", {}).get("file_path", "")

if any(p in file_path for p in ["/etc/", "production.conf"]):
    print("Blocked: 시스템 파일 수정 불가", file=sys.stderr)
    sys.exit(2)
sys.exit(0)
```

#### 실전 예시 3: Git 브랜치 보호
```bash
#!/bin/bash
BRANCH=$(git branch --show-current)
if [[ "$BRANCH" == "main" || "$BRANCH" == "master" ]]; then
    echo "main 브랜치 수정 금지" >&2
    exit 2
fi
exit 0
```

#### 핵심 주의사항
1. **Matcher는 대소문자 구분** — `Write` ≠ `write`
2. **JSON 입력**: stdin에서 `json.load()`로 읽기
3. **실행 권한**: `chmod +x` 필수
4. **크로스 플랫폼**: macOS `say`, Linux `spd-say`, Windows PowerShell 차이
5. **무한 루프 방지**: Hook이 같은 이벤트 재트리거하지 않게 설계

---

### 📚 2-2. disler/claude-code-hooks-mastery ⭐
🔗 https://github.com/disler/claude-code-hooks-mastery

> **13개 훅 라이프사이클 완벽 구현** + UV 단일 파일 Python 스크립트 아키텍처.

#### 레포 구조
```
.claude/hooks/
├── user_prompt_submit.py   # 프롬프트 검증/로깅
├── pre_tool_use.py         # 위험 명령 차단
├── post_tool_use.py        # 이벤트 로깅
├── stop.py                 # 완료 메시지
├── validators/
│   ├── ruff_validator.py   # Python 린팅
│   └── ty_validator.py     # 타입 체크
└── utils/                  # TTS, LLM 유틸

.claude/agents/
├── team/
│   ├── builder.md          # 구현 (모든 도구)
│   └── validator.md        # 검증 (읽기 전용)
├── meta-agent.md           # 에이전트 생성 에이전트
└── crypto/                 # 암호화폐 분석 전문

.claude/output-styles/
├── genui          # HTML 임베드
├── table-based    # 마크다운 테이블
├── yaml-structured
└── ultra-concise

.claude/status_lines/  # v1~v9 진화 버전
```

#### 핵심 철학
> **"결정론적 제어"** — LLM 의사결정 없이 훅으로 Claude Code 동작을 명확하게 관리.

#### 빌더-검증자 페어링
`/plan_w_team` 명령:
1. **자체 검증**: 완료 후 자동 명세 유효성 확인
2. **에이전트 오케스트레이션**: TaskCreate/TaskUpdate
3. **템플릿화**: 예측 가능한 형식

"동일 작업 2회 수행" 전략으로 컴퓨트 증가하되 **신뢰성 극대화**.

#### 학습 경로
1. `.claude/hooks/` 간단한 훅부터
2. PostToolUse 검증자 (Ruff/Ty) 이해
3. exit code 2와 JSON 응답 활용
4. 팀 에이전트 + 메타-에이전트 탐색
5. 팀 워크플로우 + 에이전트 체이닝

#### 고유 Status Line 진화
- v5: 비용 추적 (모델·달러·라인 변경)
- v6: 컨텍스트 윈도우 사용량 바
- v8: 토큰/캐시 통계

---

### 📚 2-3. DEV.to — 20+ Ready-to-Use Hook Examples
🔗 https://dev.to/lukaszfryc/claude-code-hooks-complete-guide-with-20-ready-to-use-examples-2026-dcg

> 저자의 명언: **"Hooks give you deterministic control over a probabilistic system"**

#### 핵심 10개 훅 (JSON 그대로 복붙 가능)

**① 자동 포맷팅 (Prettier)**
```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{
        "type": "command",
        "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write 2>/dev/null; exit 0"
      }]
    }]
  }
}
```

**② 위험 명령어 차단**
```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash",
      "hooks": [{
        "type": "command",
        "command": "bash -c 'CMD=$(jq -r \".tool_input.command\" <<< \"$(cat)\"); if echo \"$CMD\" | grep -qiE \"rm -rf|DROP TABLE|push.*--force\"; then exit 2; fi'"
      }]
    }]
  }
}
```

**③ 민감 파일 보호 (`.env` 등)**
```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{
        "type": "command",
        "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/protect-files.sh"
      }]
    }]
  }
}
```

**④ Bash 명령 감사 로그**
```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Bash",
      "hooks": [{
        "type": "command",
        "command": "jq -r \".tool_input.command\" | while read cmd; do echo \"$(date +%Y-%m-%dT%H:%M:%S) $cmd\" >> \"$CLAUDE_PROJECT_DIR\"/.claude/command-audit.log; done; exit 0"
      }]
    }]
  }
}
```

**⑤ ESLint 자동 수정**
```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{
        "type": "command",
        "command": "bash -c 'FILE=$(jq -r \".tool_input.file_path\" <<< \"$(cat)\"); if [[ \"$FILE\" == *.ts || \"$FILE\" == *.js ]]; then npx eslint --fix \"$FILE\" 2>/dev/null; fi; exit 0'"
      }]
    }]
  }
}
```

**⑥ TypeScript 타입 검증**
```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{
        "type": "command",
        "command": "bash -c 'FILE=$(jq -r \".tool_input.file_path\" <<< \"$(cat)\"); if [[ \"$FILE\" == *.ts ]]; then npx tsc --noEmit 2>&1 | head -20; fi; exit 0'",
        "timeout": 30
      }]
    }]
  }
}
```

**⑦ 테스트 강제 (Stop 훅)**
```json
{
  "hooks": {
    "Stop": [{
      "hooks": [{
        "type": "command",
        "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/verify-tests.sh",
        "timeout": 60
      }]
    }]
  }
}
```

**⑧ 컨텍스트 주입 (`/compact` 후)**
```json
{
  "hooks": {
    "SessionStart": [{
      "matcher": "compact",
      "hooks": [{
        "type": "command",
        "command": "echo \"현재 브랜치: $(git -C \\\"$CLAUDE_PROJECT_DIR\\\" branch --show-current)\""
      }]
    }]
  }
}
```

**⑨ 웹 접근 자동 승인**
```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "WebFetch|WebSearch",
      "hooks": [{
        "type": "command",
        "command": "echo '{\"decision\":\"allow\"}'",
        "timeout": 5
      }]
    }]
  }
}
```

**⑩ LLM 기반 완료도 검증**
```json
{
  "hooks": {
    "Stop": [{
      "hooks": [{
        "type": "prompt",
        "prompt": "작업이 완전히 완료되었나? {\"ok\": true} 또는 {\"ok\": false, \"reason\": \"...\"} 응답",
        "timeout": 30
      }]
    }]
  }
}
```

---

## 🔹 3. Skills — 재사용 워크플로우 패키지

### 📚 3-1. FreeCodeCamp — Build Your Own Claude Code Skill
🔗 https://www.freecodecamp.org/news/how-to-build-your-own-claude-code-skill/

#### 최소 구조
```
skill-name/
└── SKILL.md
```

확장:
```
skill-name/
├── SKILL.md
└── references/
    └── examples.md
```

#### SKILL.md 템플릿

```markdown
---
name: commit-message-writer
description: 설명 및 트리거 조건
---

## Instructions
- 명확한 질문 없이 즉시 결과물 제공
- 출력 형식을 명시적으로 정의

## Output Format
- Type: feat/fix/docs/refactor/chore
- Scope: 영향받는 모듈/파일
- Short description: 명령조, 72자 이하
```

#### 핵심 포인트

> **"description 필드가 스킬 로드 여부를 결정"**
> 다양한 표현 방식 포함 필수. 좁은 description은 변형 요청에 반응 못함.

#### 테스트
```bash
# 1. 명시적 호출
/commit-message-writer

# 2. 자연어 트리거
"write a commit message for my staged changes"
```

#### 개선 루프

| 문제 | 해결책 |
|------|--------|
| 스킬 미작동 | description에 트리거 구문 추가 |
| 출력 형식 편차 | 구체적 반례/정례 제시 |
| 범위 확대 요구 | 별도 스킬 분리 |

#### 크로스 플랫폼 호환
SKILL.md 형식은 Claude Code, GitHub Copilot, Cursor, Gemini CLI에서 **동일 포맷** 사용 가능.

---

### 📚 3-2. Towards Data Science — Production-Ready Skill ⭐
🔗 https://towardsdatascience.com/how-to-build-a-production-ready-claude-code-skill/

#### 설계 원칙: 구체성 > 추상성

❌ **나쁜 예**: "데이터 분석 스킬"
✅ **좋은 예**: "CSV 주문 데이터를 받아 KPI를 분해하고 액션 플랜을 생성"

#### 메타데이터 토큰 예산
- 이름 + 설명 **약 100토큰** 제한
- Claude가 이 100토큰으로 **로딩 여부 결정**
- 모호한 설명은 작동 안 함

#### 프로덕션 스킬 디렉토리 구조
```
my-skill/
├── SKILL.md          # 필수 (500줄 이하)
├── scripts/          # Python/JS 처리
├── references/       # 긴 참조 문서
└── assets/           # 템플릿, 이미지
```

#### 3가지 구현 패턴

| 패턴 | 적합 용도 |
|------|----------|
| **A: 프롬프트 전용** | 가이드라인·코딩 표준·체크리스트 |
| **B: 프롬프트 + 스크립트** | 데이터 변환·PDF 처리·보고서 생성 |
| **C: 외부 통합** | MCP 서버·Subagent 호출 필요 시 |

#### 현실적 테스트 방법

> **"실제 사용자 표현으로 테스트하라"**
> - 깔끔한 프롬프트: "Generate a financial report" ← 개발용
> - 실제 프로덕션: "Q4 최종 XLSX 파일에서 마진율 추가" ← 비정형

깔끔한 테스트만으론 실패. **실제 유저의 비정형 표현**으로 반복 테스트.

#### 배포 경로
- **로컬**: `.claude/skills/`
- **팀 공유**: ZIP (폴더가 루트에 있어야 함)
- **확대**: 플러그인 마켓플레이스, Skills API

---

## 🔹 4. Output Styles — 출력 페르소나

### 📚 4-1. hesreallyhim/awesome-claude-code-output-styles
🔗 https://github.com/hesreallyhim/awesome-claude-code-output-styles-that-i-really-like

> **기술 능력은 유지하고 의사소통 방식만 변환** — 파일 조작·스크립트·TODO 추적 모두 정상 작동.

#### 인기 출력 스타일 6가지

**① 기술 전도사 (Technical Evangelist)**
- 특징: 열정적·전문적 설명
- 적합: 새 기술 도입, 아키텍처 결정, 팀 교육
- 효과: 개념을 명확하고 설득력 있게

**② 선불사 (Zen Master)**
- 특징: 명상적·철학적, 은유와 깊이
- 적합: 복잡한 문제, 창의적 사고, 장기 전략
- 명언: "고통스러운 버그를 명상의 대상으로"

**③ 타블로이드 기자 (Tabloid Journalist)**
- 특징: "🔥 속보!" 극적·선정적
- 적합: 반복 작업 재미 추가, 팀 분위기 전환

**④ 하이쿠 도우미 (Haiku Helper)**
- 특징: 모든 응답을 5-7-5 시
- 적합: 창의 작업, 간결 필요 시
- 효과: "같은 진실, 새로운 깃털"

**⑤ 실존주의 시인 (Existentialist Poet)**
- 특징: 철학적·심오한 메타포
- 적합: 개념적 깊이 필요
- 명언: "스택 트레이스에서 사르트르적 불합리성 발견"

**⑥ Vim 영업사원 (Door-to-Door Vim Salesman)**
- 특징: Vim을 과도하게 추천하는 유머 페르소나
- 적합: 팀 분위기 개선, 재미 학습
- 효과: 텍스트 에디터 효율성 교육

#### 사용법
```bash
/output-style [스타일명]
```

커스텀 스타일: `.claude/output-styles/<name>.md` 배치.

---

## 🎯 종합 적용 전략

### A. 개인 개발자 최소 세팅
```
~/.claude/settings.json 에:
  - PostToolUse: Prettier 자동 포맷 (예시 ①)
  - PreToolUse: rm -rf 차단 (예시 ②)
  - Stop: 테스트 검증 (예시 ⑦)

~/.claude/commands/ 에:
  - analyze.md (아키텍처 분석)
  - fix-issue.md (이슈 번호 수정)
  - commit.md (Git diff 기반 커밋 메시지)

~/.claude/skills/ 에:
  - commit-message-writer/
  - pr-description-writer/
```

### B. 팀 프로젝트 세팅
```
.claude/settings.json (팀 공유):
  - 위 훅들 + command-audit.log
  - .env 파일 보호 훅 (예시 ③)

.claude/commands/:
  - 팀 표준 워크플로우
  - /deploy-staging, /release 등

.claude/agents/:
  - builder.md + validator.md 페어 (disler 패턴)
  - security-reviewer.md
```

### C. 프로덕션 레벨 (disler 스타일)
```
.claude/hooks/:
  - 모든 13개 라이프사이클 구현
  - UV 단일 Python 스크립트
  - logs/ 디렉토리로 JSON 로그

.claude/agents/:
  - meta-agent 활용한 동적 에이전트 생성
  - team/ 구조로 역할 분리
```

---

## 🧩 5가지 기능 비교표

| 기능 | 로딩 시점 | 강제력 | 주 용도 | 파일 위치 |
|------|---------|-------|--------|----------|
| **CLAUDE.md** | 매 세션 시작 | 확률적 (조언) | 프로젝트 규칙 | 루트 / `~/.claude/` |
| **Skills** | on-demand (매칭 시) | 확률적 | 재사용 워크플로우 | `.claude/skills/` |
| **Slash Commands** | 명시적 호출 | 확률적 | 반복 프롬프트 | `.claude/commands/` |
| **Hooks** | 이벤트 트리거 | **결정론적** | 자동화·보호 | `settings.json` |
| **Subagents** | 위임 시 | 확률적 | 격리된 탐색 | `.claude/agents/` |
| **Output Styles** | 세션 전체 | 결정론적 | 의사소통 톤 | `.claude/output-styles/` |

---

## ⚡ Quick Action: 지금 당장 적용할 5가지

### 1. `rm -rf` 차단 훅 (5분)
위 예시 ②를 `~/.claude/settings.json`에 추가 → 파괴적 명령 사전 차단.

### 2. 포맷터 훅 (3분)
위 예시 ① (Prettier) 또는 ⑤ (ESLint) 추가 → 코드 스타일 통일 자동화.

### 3. `/commit` 커스텀 명령 (5분)
```bash
mkdir -p ~/.claude/commands
cat > ~/.claude/commands/commit.md << 'EOF'
---
description: Generate Conventional Commit message from staged changes
---
Git status:
!git status --short
Git diff:
!git diff --cached
위 변경사항에 대한 Conventional Commit 메시지 작성 (type/scope/short desc).
EOF
```

### 4. `commit-message-writer` 스킬 (10분)
`~/.claude/skills/commit-message-writer/SKILL.md` 생성 → FreeCodeCamp 템플릿 활용.

### 5. Stop 훅으로 테스트 검증 (15분)
프로젝트 루트에 `.claude/hooks/verify-tests.sh` 작성 + 위 예시 ⑦ 설정.

---

## 📖 추천 학습 순서

1. **슬래시 명령 입문**: BioErrorLog (가장 간단)
2. **Hooks 기본**: DataCamp (Exit code, 실전 예시)
3. **Hooks 복붙**: DEV.to 20+ 예시 (즉시 적용 가능)
4. **Hooks 마스터**: disler/hooks-mastery (13개 훅 완전 구현)
5. **Skills 입문**: FreeCodeCamp (commit-message-writer)
6. **Skills 프로덕션**: Towards Data Science (3패턴·테스트)
7. **재미·차별화**: awesome-output-styles (페르소나)

---

## 🚨 실전 주의사항

1. **훅 실수가 Claude 세션 망가뜨릴 수 있음** — 새 훅은 먼저 로그만 찍어 테스트
2. **JSON 파싱 오류**: `jq` 없으면 훅 실패 — 먼저 `jq --version` 확인
3. **Windows 환경**: bash 훅은 WSL 또는 Git Bash에서만 동작
4. **PreToolUse 무한 루프**: 훅이 도구를 재호출하는 구조 피하기
5. **timeout 미지정 시 기본값**: command 600s, prompt 30s
6. **훅 디버깅**: `/hooks`로 설정 확인, 로그 파일 활용
7. **에이전트는 시스템 프롬프트**: 에이전트 파일 내용은 **사용자 프롬프트 아님**
8. **description 100토큰 한계**: 스킬은 짧고 구체적으로
