# Chapter 19: 설정 심화

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- CLAUDE.md 3단계 설정 시스템
- settings.json 상세 옵션
- 권한 관리와 커스텀 설정

---

## 왜 필요한가요?

**실제 상황**: Claude Code를 시작할 때마다 "TypeScript 사용해, 팀 컨벤션 따라, any 타입 쓰지 마..." 매번 이렇게 타이핑하면 피곤하고 가끔 잊어버리기도 합니다.

설정 파일은 규칙을 한 번만 기억해두고 매번 자동으로 적용해줍니다.

### 쉬운 비유: 스마트폰 설정

설정을 스마트폰 설정처럼 생각해보시기 바랍니다:
- **기본 벨소리** = Claude의 기본 동작
- **맞춤 벨소리** = 개인 CLAUDE.md
- **업무 프로필** = 프로젝트별 설정

한 번 설정하면 매번 알아서 적용됩니다. 반복할 필요 없습니다.

---

## JSON 기초 (초보자용)

Claude Code 설정의 많은 부분이 JSON 형식을 사용합니다. 간단히 알아보겠습니다:

### JSON이 무엇인가요?

JSON (JavaScript Object Notation)은 데이터를 저장하는 간단한 방법입니다. 다음과 같이 생겼습니다:

```json
{
  "이름": "값",
  "숫자": 42,
  "목록": ["항목1", "항목2"],
  "중첩": {
    "내부": "값"
  }
}
```

### 핵심 규칙

1. **중괄호 `{}`**는 객체(키-값 쌍)를 감쌉니다
2. **대괄호 `[]`**는 목록(배열)을 감쌉니다
3. **키는 반드시 큰따옴표로** `"이렇게"`
4. **쉼표로 항목 구분** 단, 마지막 항목 뒤에는 쉼표가 없습니다
5. **주석 불가** (JavaScript와 다릅니다)

### 흔한 실수

```json
// 나쁨 - 마지막 쉼표
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
```

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

### 단계 2: 개인 설정 만들기

설정 폴더와 파일을 만드시기 바랍니다:

```bash
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

---

## 왜 설정을 알아야 할까?

Claude Code는 기본값으로도 잘 작동합니다. 하지만 설정을 이해하면:

- **일관된 작업**: 매번 같은 규칙 반복 설명 불필요
- **안전한 사용**: 위험한 동작 사전 방지
- **맞춤형 환경**: 나만의 워크플로우 구축

---

## CLAUDE.md 3단계 시스템

Claude Code는 3개 레벨의 CLAUDE.md를 읽습니다.

### 계층 구조

```
┌─────────────────────────────────────────────────────────────────┐
│                    CLAUDE.md 우선순위                            │
└─────────────────────────────────────────────────────────────────┘

[1] 프로젝트 레벨 (가장 높은 우선순위)
    ./CLAUDE.md
    └── 현재 프로젝트 전용 규칙

[2] 루트 레벨 (중간 우선순위)
    ~/.claude/CLAUDE.md
    └── 모든 프로젝트에 적용되는 개인 규칙

[3] 유저 레벨 (가장 낮은 우선순위)
    시스템 기본값
    └── Claude Code 기본 동작
```

### 이것을 알면 무엇이 좋을까요?

**프로젝트마다 다른 규칙이 필요할 때:**

```markdown
# 프로젝트 A의 CLAUDE.md
이 프로젝트는 TypeScript + React를 사용합니다.
테스트는 Jest로 작성합니다.

# 프로젝트 B의 CLAUDE.md
이 프로젝트는 Python + FastAPI를 사용합니다.
테스트는 pytest로 작성합니다.
```

**공통 규칙은 루트 레벨에:**

```markdown
# ~/.claude/CLAUDE.md
모든 커밋 메시지는 한글로 작성합니다.
코드 리뷰 시 보안 취약점을 항상 체크합니다.
```

---

## CLAUDE.md 작성 가이드

### 좋은 CLAUDE.md의 특징

```markdown
# 프로젝트 개요
이 프로젝트는 사용자 인증 서비스입니다.

# 기술 스택
- Backend: Express.js + TypeScript
- Database: PostgreSQL
- ORM: Prisma

# 코딩 컨벤션
- 함수명은 camelCase
- 파일명은 kebab-case
- 타입은 PascalCase

# 자주 쓰는 명령어
- npm run dev: 개발 서버 실행
- npm test: 테스트 실행
- npm run lint: 린트 체크

# 중요한 파일
- src/config/: 환경 설정
- src/middleware/: 인증, 로깅 미들웨어
- prisma/schema.prisma: DB 스키마
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
```

---

## settings.json 상세 설정

### 설정 파일 위치

```
~/.claude/settings.json
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

### 권한 설정

```json
{
  "permissions": {
    // 자동 승인할 도구들
    "autoApprove": [
      "Read",      // 파일 읽기
      "Glob",      // 파일 검색
      "Grep",      // 내용 검색
      "WebFetch"   // URL 가져오기
    ],

    // 항상 차단할 패턴
    "deny": [
      "rm -rf",
      "DROP TABLE",
      "force push"
    ]
  }
}
```

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
      "DROP DATABASE",
      "git push --force origin main"
    ]
  }
}
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
    // "minimal": 핵심만
    // "normal": 기본값
    // "verbose": 상세 설명 포함
    "verbosity": "normal"
  }
}
```

---

## 환경별 설정

### 개발 환경

```json
// ~/.claude/settings.json (개발용)
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep", "Edit", "Write", "Bash"]
  },
  "behavior": {
    "sandbox": false
  }
}
```

### 프로덕션 환경

```json
// ~/.claude/settings.json (프로덕션용)
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": ["rm", "DROP", "DELETE", "force"]
  },
  "behavior": {
    "sandbox": true
  }
}
```

### 이것을 알면 무엇이 좋을까요?

**환경에 맞는 안전 수준:**

- 개발: 빠른 작업을 위해 많은 것을 자동 승인
- 프로덕션: 실수 방지를 위해 엄격하게 제한

---

## 프로젝트별 설정

### .claude/ 폴더 활용

```
my-project/
├── .claude/
│   ├── settings.json    # 프로젝트 전용 설정
│   └── templates/       # 자주 쓰는 프롬프트
├── CLAUDE.md           # 프로젝트 규칙
└── src/
```

### 프로젝트 전용 settings.json

```json
// my-project/.claude/settings.json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": ["npm publish"]
  }
}
```

---

## 실용적인 설정 예시

### 프론트엔드 프로젝트

```markdown
# CLAUDE.md
## 프로젝트
React + TypeScript + Tailwind CSS

## 컨벤션
- 컴포넌트는 함수형으로
- 상태 관리는 Zustand
- 스타일은 Tailwind 클래스 사용

## 테스트
- 테스트 파일: *.test.tsx
- React Testing Library 사용

## 금지
- any 타입 사용 금지
- inline style 금지
```

```json
// .claude/settings.json
{
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": [": any", "style={{"]
  }
}
```

### 백엔드 프로젝트

```markdown
# CLAUDE.md
## 프로젝트
Express + TypeScript + Prisma

## API 규칙
- RESTful 설계
- 응답: { success, data, error }
- 인증: JWT Bearer 토큰

## 보안
- SQL 인젝션 주의
- 입력값 검증 필수
- 민감 정보 로그 금지

## DB
- 마이그레이션: prisma migrate
- 시드: prisma db seed
```

```json
// .claude/settings.json
{
  "permissions": {
    "deny": [
      "DROP TABLE",
      "DELETE FROM",
      "console.log(password",
      "console.log(token"
    ]
  }
}
```

### 풀스택 프로젝트

```markdown
# CLAUDE.md
## 구조
- frontend/: Next.js
- backend/: NestJS
- shared/: 공유 타입

## 개발 서버
- frontend: npm run dev (포트 3000)
- backend: npm run start:dev (포트 4000)

## 환경변수
- .env.local: 로컬 설정 (git에 올리지 않음)
- .env.example: 템플릿
```

---

## 설정 디버깅

### 현재 설정 확인

```
> /config
```

현재 적용된 설정을 보여줍니다.

### 설정이 안 먹힐 때

1. **파일 위치 확인**: `~/.claude/settings.json`
2. **JSON 문법 확인**: 쉼표, 따옴표 등
3. **Claude Code 재시작**: 설정 변경 후 필요할 수 있음

```bash
# JSON 문법 검증
cat ~/.claude/settings.json | jq .
```

---

## 따라해보십시오

### 실습 1: 첫 CLAUDE.md 만들기

1. 프로젝트 폴더로 이동하십시오
2. 프로젝트 규칙이 담긴 `CLAUDE.md` 파일을 만드십시오
3. Claude Code를 시작하고 "이 프로젝트의 규칙이 무엇인가요?"라고 물어보십시오

### 실습 2: 자동 승인 설정하기

1. `~/.claude/settings.json`을 만드십시오
2. 읽기 전용 도구들에 대해 자동 승인을 추가하십시오
3. 파일 검색을 시도해보십시오 - 더 이상 'y'를 누르지 않아도 됩니다

### 실습 3: 차단 패턴 추가하기

위험한 명령을 차단하는 패턴을 추가하십시오:

```json
{
  "permissions": {
    "deny": ["rm -rf"]
  }
}
```

`rm -rf`로 무언가를 삭제해달라고 요청해보십시오 - 차단될 것입니다.

---

## 문제가 발생하면?

### 문제: CLAUDE.md가 읽히지 않습니다

**가능한 원인:**
1. 파일명이 정확히 `CLAUDE.md`가 아님 (대소문자 구분)
2. 파일이 잘못된 위치에 있음
3. 파일 인코딩 문제

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

## 정리

이번 챕터에서 배운 것:
- [x] CLAUDE.md 3단계 시스템 (프로젝트/루트/유저)
- [x] settings.json 상세 옵션
- [x] 권한 관리 (자동 승인/차단)
- [x] 환경별 설정 전략
- [x] 프로젝트별 맞춤 설정

**핵심 포인트**: 좋은 설정은 반복 작업을 줄이고, 실수를 방지합니다.

다음 챕터에서는 Hooks와 Commands로 더 강력한 자동화를 배웁니다.

[Chapter 20: Hooks & Commands](../Chapter20/README.ko.md)로 넘어가시기 바랍니다.
