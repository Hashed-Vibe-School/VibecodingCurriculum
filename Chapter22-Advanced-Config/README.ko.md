# Chapter 22: 설정 심화

[English](./README.md) | **한국어**

---

## 💬 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이 챕터에서 배우는 것

- CLAUDE.md 3단계 설정 시스템의 완전한 이해
- settings.json 상세 옵션과 고급 설정
- 권한 관리와 커스텀 설정 전략
- 환경별 설정 분리와 팀 협업을 위한 설정

---

## 이전 챕터 복습

[Chapter 21: 아키텍처 이해](../Chapter21-Architecture/README.ko.md)에서 Claude Code의 내부 작동 원리를 배웠습니다. 이번 챕터에서는 한 단계 더 나아가 **설정의 심화 과정**을 다룹니다. 설정을 제대로 이해하면 Claude Code가 마치 오랜 동료처럼 여러분의 작업 스타일을 완벽하게 이해하게 됩니다.

> 💡 **비전공자 팁**: 설정은 한 번 배워두면 평생 써먹는 기술입니다. 처음에는 복잡해 보여도, 차근차근 따라하면 어렵지 않아요!

---

## 왜 필요한가요?

**실제 상황**: Claude Code를 시작할 때마다 "TypeScript 사용해, 팀 컨벤션 따라, any 타입 쓰지 마..." 매번 이렇게 타이핑하면 피곤하고 가끔 잊어버리기도 합니다.

설정 파일은 규칙을 한 번만 기억해두고 매번 자동으로 적용해줍니다.

### 실제 현업에서의 활용

현업에서 설정 파일이 중요한 이유를 살펴봅시다:

| 상황 | 설정 없이 | 설정 있으면 |
|------|----------|------------|
| 새 프로젝트 시작 | 매번 규칙 설명 | 바로 작업 시작 |
| 팀원 온보딩 | 구두로 규칙 전달 | 자동으로 규칙 적용 |
| 실수 방지 | 위험한 명령 실행 가능 | 자동 차단 |
| 작업 일관성 | 사람마다 다른 결과 | 항상 같은 품질 |

### 쉬운 비유: 스마트폰 설정

설정을 스마트폰 설정처럼 생각해보시기 바랍니다:
- **기본 벨소리** = Claude의 기본 동작
- **맞춤 벨소리** = 개인 CLAUDE.md
- **업무 프로필** = 프로젝트별 설정

한 번 설정하면 매번 알아서 적용됩니다. 반복할 필요 없습니다.

> 🔥 **프로 팁**: 설정 파일을 Git에 커밋하면 팀 전체가 같은 규칙으로 작업할 수 있습니다. 이것이 바로 "Infrastructure as Code"의 시작입니다!

---

## 초보자가 흔히 빠지는 함정

### 함정 1: "설정은 나중에 해도 돼"

많은 분들이 "일단 코딩부터 하고 설정은 나중에" 라고 생각합니다. 하지만 이렇게 하면:
- 매번 같은 규칙을 반복 타이핑
- 팀원마다 다른 결과물
- 실수로 위험한 명령 실행

> ⚠️ **주의사항**: 프로젝트 시작 시 10분만 투자해서 기본 설정을 만들어두세요. 나중에 몇 시간을 절약합니다!

### 함정 2: "설정 파일이 너무 복잡해 보여"

JSON 문법이 처음에는 어려워 보일 수 있습니다. 하지만:
- 실제로 사용하는 옵션은 몇 개 안 됩니다
- 복사-붙여넣기로 시작해도 됩니다
- 에러 메시지가 친절하게 알려줍니다

### 함정 3: "한 번 설정하면 끝"

설정은 프로젝트가 성장하면서 함께 진화해야 합니다:
- 새로운 기술 도입 시 업데이트
- 팀 규칙 변경 시 반영
- 보안 정책 강화 시 수정

---

## JSON 기초 (초보자용)

Claude Code 설정의 많은 부분이 JSON 형식을 사용합니다. 간단히 알아보겠습니다:

### JSON이 무엇인가요?

JSON (JavaScript Object Notation)은 데이터를 저장하는 간단한 방법입니다. 다음과 같이 생겼습니다:

```json
{
  "이름": "값",
  "숫자": 42,
  "참거짓": true,
  "목록": ["항목1", "항목2"],
  "중첩": {
    "내부": "값"
  }
}
```

> 💡 **비전공자 팁**: JSON은 마치 엑셀의 "이름: 값" 형식과 비슷합니다. 왼쪽이 제목, 오른쪽이 내용이라고 생각하세요!

### 핵심 규칙

1. **중괄호 `{}`**는 객체(키-값 쌍)를 감쌉니다
2. **대괄호 `[]`**는 목록(배열)을 감쌉니다
3. **키는 반드시 큰따옴표로** `"이렇게"`
4. **쉼표로 항목 구분** 단, 마지막 항목 뒤에는 쉼표가 없습니다
5. **주석 불가** (JavaScript와 다릅니다)

### 데이터 타입 정리

| 타입 | 예시 | 설명 |
|------|------|------|
| 문자열 | `"hello"` | 따옴표로 감싼 텍스트 |
| 숫자 | `42`, `3.14` | 따옴표 없는 숫자 |
| 불리언 | `true`, `false` | 참/거짓 |
| 배열 | `["a", "b"]` | 여러 값의 목록 |
| 객체 | `{"key": "value"}` | 키-값 쌍 |
| null | `null` | 값 없음 |

### 흔한 실수와 해결법

```json
// 나쁨 - 마지막 쉼표 (Trailing Comma)
{
  "이름": "값",  // <-- 여기서 에러!
}

// 좋음 - 마지막 쉼표 없음
{
  "이름": "값"
}

// 나쁨 - 작은따옴표
{
  '이름': '값'
}

// 좋음 - 큰따옴표
{
  "이름": "값"
}

// 나쁨 - 주석 사용
{
  "이름": "값"  // 이건 주석이야
}

// 좋음 - 주석 없이
{
  "이름": "값"
}
```

> 🔥 **프로 팁**: VS Code에서 JSON 파일을 열면 실시간으로 문법 오류를 표시해줍니다. 빨간 줄이 보이면 그 부분을 확인하세요!

---

## 🎯 미니 퀴즈: JSON 기초

다음 중 올바른 JSON은 무엇일까요?

**A:**
```json
{
  'name': 'John'
}
```

**B:**
```json
{
  "name": "John",
}
```

**C:**
```json
{
  "name": "John"
}
```

<details>
<summary>정답 보기</summary>

**정답: C**

- A: 작은따옴표 사용 (오류)
- B: 마지막 쉼표 있음 (오류)
- C: 올바른 JSON 형식

</details>

---

## 첫 번째 설정 (여기서 시작하십시오)

모든 옵션을 알아보기 전에, 최소한의 작동하는 설정을 만들어봅시다:

### 단계 1: 프로젝트에 CLAUDE.md 만들기

프로젝트 루트에 `CLAUDE.md` 파일을 만드시기 바랍니다:

```markdown
# 프로젝트 규칙

이 프로젝트는 TypeScript입니다. 엄격한 타입을 사용하십시오.
```

이것으로 끝입니다. Claude가 이제 이 파일을 읽고 규칙을 따릅니다.

> 💡 **비전공자 팁**: CLAUDE.md는 마치 새 직원에게 주는 "업무 가이드북"과 같습니다. Claude가 프로젝트에 들어오면 가장 먼저 이 파일을 읽습니다!

### 단계 2: 개인 설정 만들기

설정 폴더와 파일을 만드시기 바랍니다:

```bash
# Windows (PowerShell)
New-Item -ItemType Directory -Force -Path "$HOME\.claude"
New-Item -ItemType File -Force -Path "$HOME\.claude\settings.json"

# Mac/Linux
mkdir -p ~/.claude
touch ~/.claude/settings.json
```

이 최소 설정을 추가하시기 바랍니다:

```json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"]
  }
}
```

이제 안전한 읽기 전용 도구들이 매번 권한을 묻지 않습니다.

### 단계 3: 작동 확인

```
> 내 CLAUDE.md에 무엇이 있는지 알려줘
```

Claude가 파일을 읽고 내용을 알려줄 수 있어야 합니다.

### 실제 대화 예시: 설정 확인

```
사용자: 내 CLAUDE.md에 무엇이 있는지 알려줘

Claude: CLAUDE.md 파일을 확인해보겠습니다.

프로젝트 루트에 있는 CLAUDE.md 파일의 내용입니다:

# 프로젝트 규칙
이 프로젝트는 TypeScript입니다. 엄격한 타입을 사용하십시오.

이 규칙에 따라 작업하겠습니다. TypeScript로 코드를 작성할 때
엄격한 타입 정의를 사용하고, any 타입 사용을 피하겠습니다.

다른 규칙을 추가하고 싶으시면 말씀해주세요!
```

---

## 왜 설정을 알아야 할까?

Claude Code는 기본값으로도 잘 작동합니다. 하지만 설정을 이해하면:

- **일관된 작업**: 매번 같은 규칙 반복 설명 불필요
- **안전한 사용**: 위험한 동작 사전 방지
- **맞춤형 환경**: 나만의 워크플로우 구축

### 설정의 ROI (투자 대비 효과)

| 투자 시간 | 절약 시간 | ROI |
|----------|----------|-----|
| 설정 10분 | 매일 5분 절약 | 20일 후 100분 절약 |
| 설정 30분 | 매일 10분 절약 | 10일 후 100분 절약 |
| 설정 1시간 | 매일 15분 절약 | 4일 후 시간 회수 |

> 🔥 **프로 팁**: 가장 자주 하는 반복 작업부터 자동화하세요. 작은 것부터 시작해서 점점 확장하면 됩니다.

---

## CLAUDE.md 3단계 시스템

Claude Code는 3개 레벨의 CLAUDE.md를 읽습니다.

### 계층 구조

```
┌─────────────────────────────────────────────────────────────────┐
│                    CLAUDE.md 우선순위                            │
└─────────────────────────────────────────────────────────────────┘

[1] 프로젝트 레벨 (가장 높은 우선순위) ⭐
    ./CLAUDE.md
    └── 현재 프로젝트 전용 규칙
    └── 예: "React 18 사용", "any 타입 금지"

[2] 루트 레벨 (중간 우선순위)
    ~/.claude/CLAUDE.md
    └── 모든 프로젝트에 적용되는 개인 규칙
    └── 예: "한글로 응답", "커밋 메시지는 한글로"

[3] 유저 레벨 (가장 낮은 우선순위)
    시스템 기본값
    └── Claude Code 기본 동작
```

### 우선순위 충돌 해결

만약 여러 레벨에서 같은 주제에 대해 다른 규칙이 있다면:

```markdown
# ~/.claude/CLAUDE.md (루트 레벨)
모든 응답은 영어로 해주세요.

# ./CLAUDE.md (프로젝트 레벨)
모든 응답은 한글로 해주세요.
```

이 경우, **프로젝트 레벨**의 규칙이 우선합니다. 즉, 한글로 응답합니다.

> 💡 **비전공자 팁**: 회사 규칙보다 팀 규칙이, 팀 규칙보다 프로젝트 규칙이 우선한다고 생각하면 쉬워요!

### 이것을 알면 무엇이 좋을까요?

**프로젝트마다 다른 규칙이 필요할 때:**

```markdown
# 프로젝트 A의 CLAUDE.md
이 프로젝트는 TypeScript + React를 사용합니다.
테스트는 Jest로 작성합니다.
CSS는 Tailwind를 사용합니다.

# 프로젝트 B의 CLAUDE.md
이 프로젝트는 Python + FastAPI를 사용합니다.
테스트는 pytest로 작성합니다.
문서화는 Sphinx를 사용합니다.
```

**공통 규칙은 루트 레벨에:**

```markdown
# ~/.claude/CLAUDE.md
모든 커밋 메시지는 한글로 작성합니다.
코드 리뷰 시 보안 취약점을 항상 체크합니다.
함수 이름은 동사로 시작합니다.
주석은 "왜"를 설명하고, "무엇"은 코드가 설명하게 합니다.
```

### 실제 대화 예시: 프로젝트별 규칙 적용

**프로젝트 A에서:**
```
사용자: 버튼 컴포넌트 만들어줘

Claude: React와 TypeScript, Tailwind를 사용해서 버튼 컴포넌트를 만들겠습니다.

// components/Button.tsx
interface ButtonProps {
  children: React.ReactNode;
  variant?: 'primary' | 'secondary';
  onClick?: () => void;
}

export function Button({
  children,
  variant = 'primary',
  onClick
}: ButtonProps) {
  const baseClasses = "px-4 py-2 rounded font-medium transition-colors";
  const variantClasses = {
    primary: "bg-blue-500 text-white hover:bg-blue-600",
    secondary: "bg-gray-200 text-gray-800 hover:bg-gray-300"
  };

  return (
    <button
      className={`${baseClasses} ${variantClasses[variant]}`}
      onClick={onClick}
    >
      {children}
    </button>
  );
}
```

**프로젝트 B에서 (같은 요청):**
```
사용자: 버튼 컴포넌트 만들어줘

Claude: FastAPI 프로젝트에서는 프론트엔드 컴포넌트가 없습니다.
혹시 API 엔드포인트를 만들고 싶으신 건가요?

아니면 FastAPI의 HTMLResponse를 사용해서
버튼이 포함된 HTML을 반환하는 엔드포인트를 만들 수 있습니다:

# routes/ui.py
from fastapi import APIRouter
from fastapi.responses import HTMLResponse

router = APIRouter()

@router.get("/button", response_class=HTMLResponse)
async def get_button():
    return """
    <button class="btn btn-primary">
        클릭하세요
    </button>
    """
```

---

## CLAUDE.md 작성 가이드

### 좋은 CLAUDE.md의 특징

좋은 CLAUDE.md는 다음 섹션들을 포함합니다:

```markdown
# 프로젝트 개요
이 프로젝트는 사용자 인증 서비스입니다.
주요 기능: 로그인, 회원가입, 비밀번호 재설정, 소셜 로그인

# 기술 스택
- Backend: Express.js + TypeScript
- Database: PostgreSQL
- ORM: Prisma
- 캐시: Redis
- 테스트: Jest + Supertest

# 코딩 컨벤션
- 함수명은 camelCase (예: getUserById)
- 파일명은 kebab-case (예: user-service.ts)
- 타입/인터페이스는 PascalCase (예: UserProfile)
- 상수는 UPPER_SNAKE_CASE (예: MAX_LOGIN_ATTEMPTS)

# 아키텍처 규칙
- 컨트롤러 → 서비스 → 리포지토리 레이어 구조
- 비즈니스 로직은 서비스 레이어에만
- 데이터베이스 접근은 리포지토리 레이어에만

# 자주 쓰는 명령어
- npm run dev: 개발 서버 실행
- npm test: 테스트 실행
- npm run lint: 린트 체크
- npm run build: 프로덕션 빌드

# 중요한 파일
- src/config/: 환경 설정 (DB, 캐시, 외부 API)
- src/middleware/: 인증, 로깅, 에러 핸들링 미들웨어
- prisma/schema.prisma: DB 스키마 정의

# 절대 하지 말아야 할 것
- console.log에 민감 정보 출력 금지
- any 타입 사용 금지
- 하드코딩된 비밀번호/API 키 금지
```

### 효과적인 요청 팁

```markdown
# 이것보다는
"에러 처리 잘 해줘"

# 이렇게 구체적으로
## 에러 처리 규칙
- 모든 API 응답은 { success, data, error } 형식
- HTTP 에러코드는 400/401/403/404/500만 사용
- 에러 로그는 winston으로 기록
- 사용자에게 보여주는 메시지와 개발자 로그는 분리
```

> ⚠️ **주의사항**: CLAUDE.md가 너무 길면 Claude가 중요한 규칙을 놓칠 수 있습니다. 핵심 규칙 위주로 간결하게 작성하세요!

### 나쁜 CLAUDE.md vs 좋은 CLAUDE.md

**나쁜 예시:**
```markdown
# 프로젝트
리액트 프로젝트임. 잘 만들어줘. 버그 없게. 빠르게.
```

**좋은 예시:**
```markdown
# 프로젝트: 전자상거래 관리자 대시보드

## 기술 스택
- React 18 + TypeScript 5
- 상태관리: Zustand
- 스타일: Tailwind CSS + shadcn/ui
- 데이터 fetching: TanStack Query

## 코드 스타일
- 컴포넌트: 함수형 + 화살표 함수
- Props 타입: interface (type 대신)
- 이벤트 핸들러: handle 접두사 (handleClick, handleSubmit)

## 성능 규칙
- 목록 렌더링에는 항상 key 사용
- 무거운 계산에는 useMemo 사용
- 콜백에는 useCallback 사용

## 테스트
- 컴포넌트: React Testing Library
- 유틸리티: Jest
- 커버리지 목표: 80% 이상
```

---

## settings.json 상세 설정

### 설정 파일 위치

```
# 전역 설정 (모든 프로젝트에 적용)
~/.claude/settings.json

# 프로젝트 설정 (해당 프로젝트에만 적용)
./.claude/settings.json
```

### 전체 구조

```json
{
  "permissions": {
    "autoApprove": [],
    "deny": []
  },
  "model": {
    "default": "sonnet",
    "preferredForPlanning": "opus"
  },
  "behavior": {
    "autoCompact": true,
    "compactThreshold": 80000
  },
  "output": {
    "format": "markdown",
    "verbosity": "normal"
  }
}
```

### 권한 설정 상세

```json
{
  "permissions": {
    // 자동 승인할 도구들 - 매번 "y" 누를 필요 없음
    "autoApprove": [
      "Read",      // 파일 읽기 - 안전함
      "Glob",      // 파일 검색 - 안전함
      "Grep",      // 내용 검색 - 안전함
      "WebFetch"   // URL 가져오기 - 안전함
    ],

    // 항상 차단할 패턴 - 실수 방지
    "deny": [
      "rm -rf",           // 재귀적 삭제
      "DROP TABLE",       // DB 테이블 삭제
      "DROP DATABASE",    // DB 삭제
      "force push",       // 강제 푸시
      "--no-verify"       // 훅 우회
    ]
  }
}
```

### 도구별 위험도 분석

| 도구 | 위험도 | 자동승인 추천 | 설명 |
|------|--------|--------------|------|
| Read | ⬜ 낮음 | ✅ 예 | 파일 읽기만 함 |
| Glob | ⬜ 낮음 | ✅ 예 | 파일 찾기만 함 |
| Grep | ⬜ 낮음 | ✅ 예 | 내용 검색만 함 |
| WebFetch | ⬜ 낮음 | ✅ 예 | URL 읽기만 함 |
| Edit | 🟨 중간 | ⚠️ 상황에 따라 | 파일 수정 |
| Write | 🟨 중간 | ⚠️ 상황에 따라 | 새 파일 생성 |
| Bash | 🟥 높음 | ❌ 아니오 | 모든 명령 실행 가능 |

> 💡 **비전공자 팁**: 처음에는 Read, Glob, Grep만 자동승인하고 시작하세요. 익숙해지면 필요에 따라 추가하면 됩니다!

### 이것을 알면 무엇이 좋을까요?

**승인 피로 줄이기:**

읽기 전용 도구들을 자동 승인하면 매번 "y"를 누를 필요가 없습니다.

```json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"]
  }
}
```

**위험한 명령 차단:**

실수로 실행하면 안 되는 명령을 미리 차단합니다.

```json
{
  "permissions": {
    "deny": [
      "rm -rf /",
      "rm -rf ~",
      "DROP DATABASE",
      "git push --force origin main",
      "git reset --hard"
    ]
  }
}
```

### 실제 대화 예시: 차단 작동

```
사용자: 이 폴더 삭제해줘

Claude: rm -rf 명령을 실행하려고 했으나,
settings.json의 deny 규칙에 의해 차단되었습니다.

이 명령은 위험할 수 있어서 보호되고 있습니다.
정말로 삭제하시려면 터미널에서 직접 실행하시거나,
deny 규칙을 수정해주세요.

대신, 삭제할 파일 목록을 먼저 보여드릴까요?
```

---

## 모델 설정

### 기본 모델 지정

```json
{
  "model": {
    "default": "sonnet"
  }
}
```

### 작업별 모델 전략

```json
{
  "model": {
    "default": "sonnet",
    "preferredForPlanning": "opus",
    "preferredForSimpleTasks": "haiku"
  }
}
```

### 모델별 특성 비교

| 모델 | 강점 | 약점 | 추천 작업 |
|------|------|------|----------|
| **Opus** | 깊은 사고, 복잡한 문제 | 느림, 비용 높음 | 아키텍처 설계, 복잡한 리팩토링 |
| **Sonnet** | 균형 잡힌 성능 | 중간 수준 | 대부분의 개발 작업 |
| **Haiku** | 빠름, 저렴함 | 복잡한 문제에 약함 | 단순 작업, 빠른 질문 |

### 이것을 알면 무엇이 좋을까요?

**비용과 품질 자동 조절:**

- 복잡한 계획: Opus (품질 우선)
- 일반 작업: Sonnet (균형)
- 단순 작업: Haiku (속도 우선)

```
> /model opus
> 이 시스템 아키텍처 설계해줘

> /model haiku
> 이 함수에 로그 추가해줘
```

> 🔥 **프로 팁**: 월말에 API 비용이 걱정되면 Haiku를 기본으로 설정하고, 복잡한 작업에만 Sonnet/Opus를 사용하세요!

---

## 출력 설정

### 형식 설정

```json
{
  "output": {
    "format": "markdown",
    "codeBlockStyle": "fenced",
    "verbosity": "normal"
  }
}
```

### 상세도 조절

```json
{
  "output": {
    // "minimal": 핵심만 - 경험자용
    // "normal": 기본값 - 일반 사용자용
    // "verbose": 상세 설명 포함 - 학습자용
    "verbosity": "normal"
  }
}
```

### 상세도별 응답 예시

**minimal:**
```
함수 추가 완료.
```

**normal:**
```
utils.ts에 formatDate 함수를 추가했습니다.
ISO 형식의 날짜 문자열을 "YYYY년 MM월 DD일" 형식으로 변환합니다.
```

**verbose:**
```
utils.ts 파일에 formatDate 함수를 추가했습니다.

## 함수 설명
formatDate 함수는 ISO 8601 형식의 날짜 문자열
(예: "2024-01-15T09:30:00Z")을
한국어 형식 "YYYY년 MM월 DD일"로 변환합니다.

## 구현 상세
- Date 객체의 toLocaleDateString 메서드 사용
- 로케일: 'ko-KR'
- 옵션: year, month, day를 numeric으로 설정

## 사용 예시
formatDate("2024-01-15T09:30:00Z")
// 결과: "2024년 1월 15일"

## 테스트 제안
이 함수에 대한 단위 테스트도 추가하시겠습니까?
```

---

## 환경별 설정

### 개발 환경 설정

```json
// ~/.claude/settings.json (개발용)
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep", "Edit", "Write", "Bash"]
  },
  "behavior": {
    "sandbox": false
  },
  "output": {
    "verbosity": "verbose"
  }
}
```

> ⚠️ **주의사항**: 개발 환경에서도 `rm -rf`, `DROP` 같은 위험 명령은 deny 목록에 넣어두세요!

### 프로덕션 환경 설정

```json
// ~/.claude/settings.json (프로덕션용)
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": [
      "rm",
      "DROP",
      "DELETE",
      "TRUNCATE",
      "force",
      "--hard",
      "sudo"
    ]
  },
  "behavior": {
    "sandbox": true
  },
  "output": {
    "verbosity": "normal"
  }
}
```

### 환경별 설정 파일 구조

```
~/.claude/
├── settings.json          # 기본 (개발용)
├── settings.prod.json     # 프로덕션용 백업
└── settings.safe.json     # 안전 모드용 백업
```

필요할 때 파일을 교체:
```bash
# 프로덕션 작업할 때
cp ~/.claude/settings.prod.json ~/.claude/settings.json

# 개발로 돌아올 때
cp ~/.claude/settings.dev.json ~/.claude/settings.json
```

> 🔥 **프로 팁**: 프로덕션 서버에서 Claude Code를 사용할 때는 반드시 안전 설정을 적용하세요!

---

## 프로젝트별 설정

### .claude/ 폴더 활용

```
my-project/
├── .claude/
│   ├── settings.json    # 프로젝트 전용 설정
│   ├── commands/        # 프로젝트 전용 명령어
│   └── templates/       # 자주 쓰는 프롬프트
├── CLAUDE.md            # 프로젝트 규칙
└── src/
```

### 프로젝트 전용 settings.json

```json
// my-project/.claude/settings.json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": [
      "npm publish",           // 실수로 배포 방지
      "git push origin main",  // main 직접 푸시 방지
      "prisma migrate deploy"  // 프로덕션 마이그레이션 방지
    ]
  }
}
```

### 설정 우선순위

```
프로젝트 설정 > 사용자 설정 > 시스템 기본값

./.claude/settings.json
      ↓ (덮어씀)
~/.claude/settings.json
      ↓ (덮어씀)
시스템 기본값
```

---

## 실용적인 설정 예시

### 프론트엔드 프로젝트

```markdown
# CLAUDE.md
## 프로젝트
React + TypeScript + Tailwind CSS 기반 대시보드

## 컨벤션
- 컴포넌트는 함수형 + 화살표 함수
- 상태 관리는 Zustand
- 스타일은 Tailwind 클래스 사용 (인라인 스타일 금지)
- 아이콘은 lucide-react 사용

## 컴포넌트 구조
src/components/
├── ui/          # 기본 UI 컴포넌트 (Button, Input 등)
├── layout/      # 레이아웃 컴포넌트 (Header, Sidebar 등)
└── features/    # 기능별 컴포넌트

## 테스트
- 테스트 파일: *.test.tsx
- React Testing Library 사용
- 각 컴포넌트에 최소 1개 테스트

## 금지
- any 타입 사용 금지
- inline style 금지
- console.log 커밋 금지
- 하드코딩된 색상 금지 (Tailwind 사용)
```

```json
// .claude/settings.json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": [
      ": any",
      "style={{",
      "console.log",
      "#[0-9a-fA-F]{6}"
    ]
  }
}
```

### 백엔드 프로젝트

```markdown
# CLAUDE.md
## 프로젝트
Express + TypeScript + Prisma 기반 REST API

## API 규칙
- RESTful 설계 원칙 준수
- 응답 형식: { success: boolean, data?: T, error?: string }
- 인증: JWT Bearer 토큰
- 페이지네이션: { page, limit, total, items }

## 레이어 구조
src/
├── controllers/  # HTTP 요청 처리
├── services/     # 비즈니스 로직
├── repositories/ # 데이터 접근
├── middleware/   # 인증, 로깅, 에러 핸들링
└── utils/        # 유틸리티 함수

## 보안
- SQL 인젝션 주의 (Prisma 사용하면 자동 방지)
- 입력값 검증 필수 (zod 사용)
- 민감 정보 로그 금지 (password, token, apiKey)
- rate limiting 적용

## DB
- 마이그레이션: prisma migrate dev
- 시드: prisma db seed
- 절대 prisma migrate deploy를 개발환경에서 실행하지 않기
```

```json
// .claude/settings.json
{
  "permissions": {
    "deny": [
      "DROP TABLE",
      "DROP DATABASE",
      "DELETE FROM",
      "TRUNCATE",
      "console.log(password",
      "console.log(token",
      "console.log(apiKey",
      "prisma migrate deploy"
    ]
  }
}
```

### 풀스택 프로젝트

```markdown
# CLAUDE.md
## 구조
my-app/
├── frontend/     # Next.js 14 (App Router)
├── backend/      # NestJS
├── shared/       # 공유 타입 정의
└── infrastructure/ # Docker, K8s 설정

## 개발 서버
- frontend: npm run dev (포트 3000)
- backend: npm run start:dev (포트 4000)
- 동시 실행: npm run dev:all

## 환경변수
- .env.local: 로컬 설정 (git에 올리지 않음)
- .env.example: 템플릿 (필수 변수 목록)
- .env.test: 테스트용 설정

## 타입 공유
shared/types.ts의 타입을 양쪽에서 import
```

---

## 설정 디버깅

### 현재 설정 확인

```
> /config
```

현재 적용된 설정을 보여줍니다.

### 설정이 안 먹힐 때 체크리스트

1. **파일 위치 확인**:
   ```bash
   ls ~/.claude/settings.json
   ls ./.claude/settings.json
   ```

2. **JSON 문법 확인**:
   ```bash
   # Mac/Linux
   cat ~/.claude/settings.json | jq .

   # Windows PowerShell
   Get-Content ~/.claude/settings.json | ConvertFrom-Json
   ```

3. **Claude Code 재시작**: 설정 변경 후 필요할 수 있음

4. **캐시 삭제**:
   ```bash
   # Claude Code 완전 종료 후 재시작
   ```

### 실제 대화 예시: 설정 문제 해결

```
사용자: 자동승인 설정했는데 계속 물어봐

Claude: 설정이 제대로 적용되지 않은 것 같습니다.
몇 가지 확인해볼게요.

1. 설정 파일 위치 확인:
   ~/.claude/settings.json 파일이 있나요?

2. JSON 형식 확인:
   jq로 검증해보겠습니다...

오류 발견! 12번 줄에 마지막 쉼표가 있습니다:
"autoApprove": ["Read", "Glob",]
                            ↑ 이 쉼표를 삭제하세요

수정 후 Claude Code를 재시작해주세요.
```

---

## 📝 실습 과제

### 실습 1: 첫 CLAUDE.md 만들기 (초급)

1. 프로젝트 폴더로 이동하십시오
2. 다음 내용으로 `CLAUDE.md` 파일을 만드십시오:
   ```markdown
   # 내 프로젝트

   ## 규칙
   - 모든 응답은 한글로
   - 코드에는 주석 달기
   ```
3. Claude Code를 시작하고 "이 프로젝트의 규칙이 무엇인가요?"라고 물어보십시오

**성공 기준**: Claude가 CLAUDE.md의 규칙을 읽고 응답합니다.

### 실습 2: 자동 승인 설정하기 (초급)

1. `~/.claude/settings.json`을 만드십시오
2. 읽기 전용 도구들에 대해 자동 승인을 추가하십시오:
   ```json
   {
     "permissions": {
       "autoApprove": ["Read", "Glob", "Grep"]
     }
   }
   ```
3. Claude Code를 재시작하고 "src 폴더에 무슨 파일 있어?"라고 물어보십시오

**성공 기준**: 매번 승인 요청 없이 파일 목록을 보여줍니다.

### 실습 3: 차단 패턴 추가하기 (중급)

위험한 명령을 차단하는 패턴을 추가하십시오:

```json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": [
      "rm -rf",
      "DROP",
      "DELETE FROM"
    ]
  }
}
```

**테스트 방법**:
- `rm -rf`로 무언가를 삭제해달라고 요청해보십시오
- Claude가 차단 메시지를 보여주면 성공!

### 실습 4: 프로젝트별 설정 만들기 (고급)

1. 프로젝트에 `.claude/` 폴더 만들기
2. 프로젝트 전용 settings.json 작성
3. CLAUDE.md와 함께 Git에 커밋
4. 다른 프로젝트에서 다른 설정이 적용되는지 확인

---

## 🏆 도전 과제

### 도전 1: 완벽한 CLAUDE.md 작성

여러분의 실제 프로젝트에 맞는 완벽한 CLAUDE.md를 작성해보세요.
포함해야 할 내용:
- 프로젝트 개요
- 기술 스택
- 코딩 컨벤션
- 아키텍처 규칙
- 금지 사항

### 도전 2: 팀용 설정 패키지 만들기

팀에서 공유할 수 있는 설정 패키지를 만들어보세요:
- `.claude/` 폴더 구조
- settings.json
- CLAUDE.md
- README (사용법 설명)

### 도전 3: 환경별 설정 스위처 만들기

개발/테스트/프로덕션 환경별로 다른 설정을 적용하는 스크립트를 만들어보세요.

---

## 문제가 발생하면?

### 문제: CLAUDE.md가 읽히지 않습니다

**가능한 원인:**
1. 파일명이 정확히 `CLAUDE.md`가 아님 (대소문자 구분)
2. 파일이 잘못된 위치에 있음
3. 파일 인코딩 문제 (UTF-8이어야 함)

**해결 방법:**
- 파일명 확인: `ls -la CLAUDE.md`
- 프로젝트 루트(`claude`를 실행하는 곳)에 있는지 확인
- 내용 확인: `cat CLAUDE.md`

### 문제: settings.json 변경이 적용되지 않습니다

**가능한 원인:**
1. 잘못된 JSON 문법
2. 파일이 잘못된 위치에 있음
3. Claude Code 재시작 필요

**해결 방법:**
- JSON 검증: `cat ~/.claude/settings.json | jq .`
- 위치 확인: 반드시 `~/.claude/settings.json`
- Claude Code 재시작

### 문제: 자동 승인이 작동하지 않습니다

**가능한 원인:**
1. 도구 이름 철자 오류 (대소문자 구분)
2. JSON 문법 에러

**해결 방법:**
- 정확한 도구 이름 확인: `Read`, `Glob`, `Grep`, `Edit`, `Write`, `Bash`
- JSON 형식 검증

---

## 자주 하는 실수

1. **JSON에서 마지막 쉼표**
   ```json
   // 나쁨
   { "autoApprove": ["Read",] }

   // 좋음
   { "autoApprove": ["Read"] }
   ```

2. **잘못된 파일 위치**
   - 설정: `~/.claude/settings.json` (`~/settings.json` 아님)
   - 프로젝트 규칙: 프로젝트 루트의 `./CLAUDE.md`

3. **대소문자 구분**
   - `CLAUDE.md`는 `claude.md`와 다릅니다
   - `Read`는 `read`와 다릅니다

4. **너무 제한적으로 설정**
   - 너무 많은 패턴을 차단하면 Claude가 쓸모없어집니다
   - 허용적으로 시작하고, 필요할 때 제한을 추가하십시오

5. **프로젝트별 요구사항 잊기**
   - 한 프로젝트에서 작동하는 것이 다른 프로젝트에서는 문제가 될 수 있습니다
   - 예외 처리는 프로젝트 레벨 `.claude/settings.json`을 사용하십시오

---

## 용어 사전

| 용어 | 설명 |
|------|------|
| **CLAUDE.md** | Claude에게 프로젝트 규칙을 알려주는 마크다운 파일 |
| **settings.json** | Claude Code의 동작을 설정하는 JSON 파일 |
| **autoApprove** | 자동 승인할 도구 목록 |
| **deny** | 차단할 명령어 패턴 목록 |
| **JSON** | JavaScript Object Notation, 데이터 형식 |
| **마크다운** | 간단한 문서 작성 형식 (.md 파일) |

---

## 정리

이번 챕터에서 배운 것:
- [x] CLAUDE.md 3단계 시스템 (프로젝트/루트/유저)
- [x] settings.json 상세 옵션
- [x] 권한 관리 (자동 승인/차단)
- [x] 환경별 설정 전략
- [x] 프로젝트별 맞춤 설정
- [x] 설정 디버깅 방법

**핵심 포인트**: 좋은 설정은 반복 작업을 줄이고, 실수를 방지합니다.

---

## 다음 챕터 미리보기

Chapter 23에서는 **Hooks와 Commands**를 배웁니다:
- 파일 수정 시 자동으로 린트 실행
- 자주 쓰는 프롬프트를 명령어로 저장
- 반복 작업의 완전 자동화

설정을 마스터했으니, 이제 자동화의 세계로 들어갈 준비가 되었습니다!

[Chapter 23: Hooks & Commands](../Chapter23-Hooks-and-Commands/README.ko.md)로 넘어가시기 바랍니다.

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [Claude Code 설정 가이드](https://docs.anthropic.com/en/docs/claude-code/settings) - settings.json 상세 옵션
- [CLAUDE.md 작성 가이드](https://docs.anthropic.com/en/docs/claude-code/memory) - 프로젝트 메모리 설정
- [Claude Code 권한 설정](https://docs.anthropic.com/en/docs/claude-code/security) - 보안 및 권한 관리

**영상 자료:**
- [Claude Code Configuration (YouTube)](https://www.youtube.com/results?search_query=claude+code+configuration+tutorial) - 설정 튜토리얼
- [JSON Basics Tutorial (YouTube)](https://www.youtube.com/results?search_query=json+tutorial+beginners) - JSON 기초 강의
- [Markdown Tutorial (YouTube)](https://www.youtube.com/results?search_query=markdown+tutorial+korean) - 마크다운 작성법

**읽을거리:**
- [JSON 공식 사이트](https://www.json.org/json-ko.html) - JSON 형식 설명 (한글)
- [Markdown Guide](https://www.markdownguide.org/) - 마크다운 완벽 가이드
- [dotfiles 관리 가이드](https://dotfiles.github.io/) - 설정 파일 관리 베스트 프랙티스

**유용한 도구:**
- [JSON Validator](https://jsonlint.com/) - JSON 문법 검증기
- [VS Code JSON Extension](https://marketplace.visualstudio.com/items?itemName=ZainChen.json) - VS Code JSON 도구

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
