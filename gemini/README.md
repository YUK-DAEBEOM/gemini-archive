# Gemini CLI 자료집

> Google Gemini CLI 사용 가이드, 프로젝트 규칙, 작업 기록 모음.
> 기준일: 2026-04-21

---

## 📑 목차

| # | 파일 | 내용 |
| - | ---- | ---- |
| 01 | [`01-usage-guide.md`](./01-usage-guide.md) | **사용 가이드** — 핵심 기능, 주요 명령어(`/`·`@`·`!`), bkit(PDCA) 워크플로우, 서브에이전트, 세션 관리, 토큰 사용량 확인 |
| 02 | [`02-project-rules.md`](./02-project-rules.md) | **프로젝트 베이스라인 규칙** — 응답 규칙(한국어·주니어 기준), 인가 절차, 클린코드(네이밍·주석·금지사항), task 저장 규칙, uv 실행 규칙 |
| -  | [`task/`](./task) | 작업 기록 (`YYYYMMDD_HHMM_제목.md` 형식) |

---

## 📂 task/ 폴더

bkit(PDCA) 워크플로우로 생성한 작업 기록이 시간순으로 저장됩니다.

| 파일 | 주제 |
| ---- | ---- |
| [`20260421_0000_gemini_cli_설치방법.md`](./task/20260421_0000_gemini_cli_설치방법.md) | Gemini CLI 설치 (npm, Homebrew 등) |
| [`20260421_0001_gemini_cli_사용법_가이드.md`](./task/20260421_0001_gemini_cli_사용법_가이드.md) | 대화형/단발성 쿼리, 효과적인 사용법 |
| [`20260421_0002_gemini_cli_스킬_및_확장프로그램.md`](./task/20260421_0002_gemini_cli_스킬_및_확장프로그램.md) | Agent Skills & Extensions 개념 |
| [`20260421_0003_gemini_cli_지원_리스트.md`](./task/20260421_0003_gemini_cli_지원_리스트.md) | 내장 서브에이전트, 지원 스킬/확장 목록 |

---

## 🧭 권장 읽기 순서

1. **설치 & 첫 실행** → `task/20260421_0000_gemini_cli_설치방법.md`
2. **사용법 익히기** → `01-usage-guide.md` (명령어, bkit 워크플로우)
3. **프로젝트 적용** → `02-project-rules.md` (팀 규칙, 클린코드)
4. **심화 기능** → `task/20260421_0002_...md`, `task/20260421_0003_...md`

---

## 💡 핵심 포인트

- **GEMINI.md** 파일 자체가 Gemini CLI의 "시스템 프롬프트 확장판" 역할 — 프로젝트 컨벤션을 여기에 고정 (이 저장소의 `01-usage-guide.md`는 그 사용법을 설명하는 문서)
- **bkit(PDCA)**: Plan → Design → Do → Check → Act → Report 순서의 `/pdca` 명령어 체계
- **인가 규칙**: 파일 수정·삭제·이동·패키지 설치는 반드시 사전 설명 + 승인 후 진행
- **실행 환경**: Python은 반드시 `uv run` 또는 `uv` 가상환경 사용
