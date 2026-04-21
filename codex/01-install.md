# codex-cli 설치 방법

작성일: 2026-04-21

## 요약

OpenAI 공식 문서 기준 현재 `codex-cli` 설치의 기본 경로는 `npm` 설치다.  
macOS와 Linux는 일반 설치로 바로 진행하면 되고, Windows는 지원되지만 아직 실험적이므로 OpenAI는 `WSL2 workspace` 사용을 가장 좋은 Windows 경험으로 안내한다.

## 1. macOS / Linux 설치

### 설치

```bash
npm i -g @openai/codex
```

### 실행

```bash
codex
```

처음 실행하면 로그인하라는 안내가 나오며, 다음 두 방식 중 하나로 인증할 수 있다.

- ChatGPT 계정 로그인
- API key 로그인

## 2. Windows 설치

### 권장 방식: WSL2에서 설치

OpenAI 공식 문서는 Windows 지원을 실험적으로 보며, 가장 좋은 Windows 경험은 `WSL2 workspace` 사용이라고 안내한다.

#### 1) 관리자 권한 PowerShell 또는 Windows Terminal에서 WSL 설치

```powershell
wsl --install
```

설치 후 WSL 셸로 들어간다.

```powershell
wsl
```

#### 2) WSL 내부에서 Node.js 설치

OpenAI Windows 가이드는 WSL 안에서 `nvm`으로 Node.js를 설치하는 절차를 예시로 제공한다.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
nvm install 22
```

#### 3) WSL 내부에서 codex-cli 설치 및 실행

```bash
npm i -g @openai/codex
codex
```

#### 4) 저장소 위치 권장

WSL에서 작업할 때는 `/mnt/c/...` 같은 Windows 마운트 경로보다, Linux 홈 디렉터리 아래에서 작업하는 편이 더 빠르고 권한 문제도 적다고 OpenAI 문서가 안내한다.

예시:

```bash
mkdir -p ~/code && cd ~/code
git clone https://github.com/your/repo.git
cd repo
codex
```

### 참고: 네이티브 Windows

공식 문서상 네이티브 Windows에서도 CLI 사용은 가능하지만, Windows 지원은 여전히 experimental로 표기되어 있다. 따라서 설치 자체는 `npm i -g @openai/codex`로 같더라도, 안정성과 작업 경험을 우선하면 WSL2 쪽이 더 권장된다.

## 3. 로그인 방법

처음 실행 시 `codex`가 로그인 흐름을 시작한다.

### ChatGPT 로그인

- CLI에서 유효한 세션이 없으면 기본 로그인 경로는 ChatGPT 로그인이다.
- 브라우저 창이 열리고 로그인 완료 후 토큰이 CLI로 돌아온다.
- ChatGPT credits 기반 기능은 ChatGPT 로그인일 때만 쓸 수 있다.

### API key 로그인

- OpenAI dashboard에서 API key를 발급해 사용할 수 있다.
- 과금은 OpenAI Platform API 요금 기준으로 적용된다.
- 공식 문서는 CI/CD 같은 programmatic workflow에는 API key 인증을 권장한다.

## 4. 업데이트 방법

```bash
npm i -g @openai/codex@latest
```

새 버전은 정기적으로 배포되며, 자세한 변경 사항은 changelog에서 확인하면 된다.

## 5. 설치 후 바로 해볼 것

### 현재 버전 실행 확인

```bash
codex
```

### 첫 질문 예시

```text
Explain this repository structure and tell me how to run tests.
```

### 권장 시작 위치

- 반드시 프로젝트 루트에서 실행
- Git 저장소 안에서 시작
- 큰 작업은 바로 수정시키지 말고 먼저 `/plan` 사용

## 6. 설치가 안 될 때 체크

- `npm` 명령이 없는 경우: Node.js/npm 환경부터 확인
- Windows에서 권한/성능 문제가 있는 경우: WSL2 경로로 전환
- 회사 PC에서 sandbox 설정이 막히는 경우: Windows 정책 또는 보안 정책 확인

## 공식 자료

- CLI Overview
  - https://developers.openai.com/codex/cli

- Windows guide
  - https://developers.openai.com/codex/windows

- Authentication
  - https://developers.openai.com/codex/auth

## 핵심 원문 요약

- 설치: `npm i -g @openai/codex`
- 실행: `codex`
- 첫 실행 로그인: ChatGPT 또는 API key
- 업데이트: `npm i -g @openai/codex@latest`
- Windows: 지원은 experimental, 권장은 WSL2

