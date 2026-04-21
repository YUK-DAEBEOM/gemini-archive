# gemini-archive

> AI 코딩 CLI 3종 — **Claude Code · Codex · Gemini CLI** — 사용법, 공식 문서, 베스트 프랙티스, 한국어 자료를 한곳에 모은 아카이브.
> 최종 정리일: 2026-04-21

---

## 📂 폴더 구성

### [`claude/`](./claude) — Claude Code (Anthropic)
공식 문서 22개, 플러그인·Hooks·Skills·MCP, CLAUDE.md 작성법, 서브에이전트·Worktree, Plan 모드·컨텍스트·메모리, TDD·CI/CD, 엔터프라이즈 비교, 한국어 자료까지 13개 카테고리로 정리한 종합 자료집.
- 주제별 14개 파일 + 마스터 URL 리스트 1개
- 상세 목차: [`claude/README.md`](./claude/README.md)

### [`codex/`](./codex) — Codex (OpenAI)
설치부터 실전 운영까지. `AGENTS.md` 활용, 검증 루프, 권한 정책, Skills / Plugins / MCP 개념과 차이점 정리.
- 6개 파일 (설치·운영·확장 개요·Skills·Plugins·MCP)
- 상세 목차: [`codex/README.md`](./codex/README.md)

### [`gemini/`](./gemini) — Gemini CLI (Google)
Gemini CLI 사용 가이드, 프로젝트 베이스라인 규칙, bkit(PDCA) 워크플로우 기반 작업 기록.
- `01-usage-guide.md` — 핵심 기능·명령어·bkit 워크플로우
- `02-project-rules.md` — 응답·인가·클린코드 규칙
- `task/` — 설치방법, 사용법, 스킬·확장, 지원 리스트 (4개)
- 상세 목차: [`gemini/README.md`](./gemini/README.md)

---

## 🔗 기타

- [`web_link.md`](./web_link.md) — 설치 관련 외부 링크 모음

---

## 🎯 어디부터 읽을까

| 상황 | 추천 시작점 |
| ---- | ---------- |
| Claude Code를 막 시작 | [`claude/01-official-docs.md`](./claude/01-official-docs.md) → [`claude/04-comprehensive-best-practices.md`](./claude/04-comprehensive-best-practices.md) |
| Codex를 처음 설치 | [`codex/01-install.md`](./codex/01-install.md) → [`codex/02-best-practices.md`](./codex/02-best-practices.md) |
| Gemini CLI 사용법 | [`gemini/01-usage-guide.md`](./gemini/01-usage-guide.md) |
| 국내 실무 사례 | [`claude/13-korean-resources.md`](./claude/13-korean-resources.md) |
| 세 도구 비교 | [`claude/12-comparison-enterprise.md`](./claude/12-comparison-enterprise.md) |
