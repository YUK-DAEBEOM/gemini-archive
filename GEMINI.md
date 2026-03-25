# Project Baseline Rules

## 0. 사용해야하는 언어모델

- Preferred Model: gemini-3.1-pro-preview

---

## 1. 응답 규칙

- 모든 답변은 **한국어**로 작성합니다.
- 코드 작성 시 **Step-by-Step**으로 단계를 나눠 설명합니다.
- 사용자는 **주니어 개발자**이므로, 적절한 예시와 설명을 항상 함께 제공합니다.

---

## 2. 인가 규칙

### 2-1. 인가가 필요한 상황

아래 상황에서는 **반드시 설명 후 승인을 받고** 실행합니다. 절대 먼저 실행하지 않습니다.

| 상황 | 해당 명령어 / 작업 예시 |
| ---- | ---- |
| 디렉토리 이동 | `cd` |
| 파일·디렉토리 삭제 | `rm`, `rmdir` |
| 파일·디렉토리 이동·이름 변경 | `mv` |
| 패키지·라이브러리 설치 | `uv add`, `pip install` |
| 코드 직접 수정·작성·삭제 | 함수 추가, 파일 생성, 기존 코드 변경 등 |

---

### 2-2. 인가 절차 흐름
```
사용자 요청
    ↓
① 작업 계획 설명   ← 무엇을, 왜, 어떻게 할 것인지 먼저 설명
    ↓
② 인가 요청        ← "진행할까요?" 로 명시적으로 확인
    ↓
③ 사용자 승인
    ↓
④ Step-by-Step 실행
    ↓
⑤ 결과 보고 + task 저장
```

---

### 2-3. 인가 요청 형식

작업 전 아래 형식으로 설명합니다.
```
[작업 계획]

변경 대상: auth/login.py
작업 내용: 로그인 엔드포인트 함수 추가
이유: 현재 라우터에 로그인 처리 로직이 없어 신규 작성이 필요합니다.
예상 결과: POST /login 요청을 처리하는 함수가 생성됩니다.

진행할까요?
```

> 사용자가 "응", "ㅇㅇ", "해줘" 등 긍정 의사를 표현한 경우에만 실행합니다.
> 애매한 경우 다시 확인합니다.

---

### 2-4. 인가 없이 진행 가능한 작업

아래는 인가 없이 바로 수행합니다.

- 코드 읽기·분석·설명
- 오류 원인 파악 및 수정 방법 제안 (실제 수정 전까지)
- task 파일 내용 작성 (저장 전 내용 미리 보여주기)

---

## 3. 클린코드 규칙

### 3-1. 네이밍 규칙

| 대상 | 규칙 | 좋은 예 | 나쁜 예 |
| ---- | ---- | ------- | ------- |
| 파일명 | 명사 (도메인/역할만) | `user_service.py`, `classifier.py` | `doLogin.py`, `classifier_output.py` |
| 클래스명 | 명사만 사용 | `QueryExecution`, `Context` | `QueryExecutionResult`, `ContextOutput` |
| 함수명 | 동사 시작 | `get_user()`, `validate_token()` | `user()`, `token()` |
| 변수명 | 의미 있는 명사 | `user_id`, `expire_at` | `x`, `tmp`, `data2` |

#### 금지 접미사

파일명과 클래스명 모두 아래 접미사 사용을 금지합니다.
`output` / `result` / `response` / `request` / `data` / `dto`
> **예외**: DB 모델, Pydantic 모델 등 데이터 구조를 명시적으로 표현해야 하는 경우
> `UserModel`, `ProductSchema` 처럼 `Model` / `Schema` 접미사는 허용합니다.

```python
# ❌ 나쁜 예
# classifier_output.py
class ContextOutput:
    content: str
    score: float

# ✅ 좋은 예
# classifier.py
class Context:
    content: str
    score: float
```

---

### 3-2. 함수 주석 규칙

모든 함수에 **기능 / Args / Returns / Raises** 를 포함한 docstring을 작성합니다.
```python
# ✅ 좋은 예
def get_user_by_id(user_id: int) -> User:
    """
    기능: ID로 사용자 정보를 조회합니다.

    Args:
        user_id (int): 조회할 사용자의 고유 ID

    Returns:
        User: 사용자 정보 객체

    Raises:
        ValueError: user_id가 0 이하일 경우
        UserNotFoundError: 해당 ID의 사용자가 없을 경우
    """
    if user_id <= 0:
        raise ValueError("user_id는 1 이상이어야 합니다.")
    ...


# ❌ 나쁜 예 — 주석 없음
def get_user_by_id(user_id):
    if user_id <= 0:
        raise ValueError
    ...
```

---

### 3-3. 금지 사항

#### ❌ 별칭·래핑 금지

호환성을 이유로 기존 함수를 감싸는 래핑 함수를 만들지 않습니다.
```python
# ❌ 나쁜 예 — 불필요한 래핑
def legacy_get_user(user_id: int) -> User:
    """하위 호환용"""
    return get_user_by_id(user_id)

# ✅ 좋은 예 — 호출하는 쪽 코드를 직접 수정
get_user_by_id(user_id)
```

---

#### ❌ `from __future__ import annotations` 금지

Python 3.11에서는 필요하지 않으며, 런타임 동작을 바꿔 혼란을 유발합니다.
```python
# ❌ 나쁜 예
from __future__ import annotations

def get_user(user_id: int) -> User:
    ...

# ✅ 좋은 예 — 전방 참조가 필요하면 문자열 리터럴 사용
def get_user(user_id: int) -> "User":
    ...
```

---

#### ❌ 불필요한 `__init__.py` 금지

Python namespace package 방식을 사용합니다. `__init__.py`는 만들지 않습니다.
```
# ❌ 나쁜 예
my_package/
  __init__.py    ← 불필요
  user_service.py
  token_validator.py

# ✅ 좋은 예
my_package/
  user_service.py
  token_validator.py
```

---

## 4. task 파일 저장 규칙

작업 완료 후, `task/` 폴더에 마크다운 파일로 저장합니다.

### 파일명 형식
```
task/YYYYMMDD_HHMM_제목.md
```

예: `task/20250324_1530_로그인_API_추가.md`

## 5. 코드 테스트 규칙

- 프로젝트의 파이썬 실행 및 테스트는 반드시 `uv` 가상환경을 사용하여 진행합니다.
- 시스템 전역 파이썬(`python3`) 사용을 금지하며, 스크립트 실행 시 `uv run python ...`을 사용하거나 해당 가상환경에 진입하여 올바른 파이썬 버전(예: 3.11) 환경에서 검증해야 합니다.