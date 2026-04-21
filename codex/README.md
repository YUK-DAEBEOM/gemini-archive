# Codex 자료집

> OpenAI Codex CLI 설치·운영·확장 가이드 모음.
> 기준일: 2026-04-21

---

## 📑 목차

| # | 파일 | 내용 |
| - | ---- | ---- |
| 01 | [`01-install.md`](./01-install.md) | Codex CLI 설치 (macOS/Linux/WSL2), 로그인(ChatGPT vs API key), 업데이트 |
| 02 | [`02-best-practices.md`](./02-best-practices.md) | 실전 운영 원칙 10선 — `AGENTS.md`, 검증 루프, 권한 정책, 모델 선택, 슬래시 명령 |
| 03 | [`03-extensions-overview.md`](./03-extensions-overview.md) | **확장 개요 (통합본)** — Skill / Plugin / MCP 비교·사용 시나리오 한눈에 |
| 04 | [`04-skills.md`](./04-skills.md) | Skill 상세 — 구조, 위치, 내장 예시, 직접 만드는 법 |
| 05 | [`05-plugins.md`](./05-plugins.md) | Plugin 상세 — `.codex-plugin/plugin.json`, 마켓플레이스, 배포 |
| 06 | [`06-mcp.md`](./06-mcp.md) | MCP 상세 — STDIO / Streamable HTTP, `config.toml`, CLI 등록 |

---

## 🧭 권장 학습 순서

1. **설치 & 첫 세션** → `01-install.md`
2. **실전 운영** → `02-best-practices.md` (`AGENTS.md`, 권한, 검증 루프)
3. **확장 개념 잡기** → `03-extensions-overview.md`
4. **심화** → `04-skills.md` → `05-plugins.md` → `06-mcp.md`

---

## 💡 핵심 요약

- **Skill** = Codex에게 일하는 방식을 가르치는 재사용 워크플로 (`SKILL.md`)
- **Plugin** = Skills + Apps + MCP를 묶은 설치 패키지 (`.codex-plugin/plugin.json`)
- **MCP** = 외부 도구(Figma, 문서 서버 등)에 연결하는 표준 프로토콜
- **AGENTS.md** = 저장소 루트에 두는 규칙·검증 기준 파일
- **권장 기본 권한**: `sandbox_mode = "workspace-write"` + `approval_policy = "on-request"`
