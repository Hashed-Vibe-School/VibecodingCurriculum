# Chapter 12: 프로젝트 메모리

[English](./README.md) | **한국어**

---

## 💬 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이 챕터에서 배우는 것

- CLAUDE.md가 무엇이고 왜 강력한지
- 효과적인 프로젝트 메모리 작성법
- Claude가 내 프로젝트를 "기억"하게 만드는 방법
- 실무에서 CLAUDE.md를 활용하는 고급 기법

---

## 이전 챕터와의 연결

Chapter 11에서 Git을 배웠습니다. Git이 **코드의 버전을 기억**하는 도구라면, 이번에 배울 CLAUDE.md는 **AI가 프로젝트를 기억**하게 하는 도구입니다.

지금까지 Claude와 대화할 때 이런 경험이 있으셨을 겁니다:

```
"Tailwind CSS 써줘"
(새 대화 시작)
"아 맞다, Tailwind CSS 써야 해"
(또 새 대화 시작)
"Tailwind CSS로 해줘..."
```

같은 말을 반복하는 것은 비효율적입니다. CLAUDE.md가 이 문제를 해결합니다.

---

## CLAUDE.md란 무엇입니까?

CLAUDE.md는 "Claude에게 주는 프로젝트 설명서"입니다. 새 대화를 시작할 때마다 Claude는 이 파일을 자동으로 읽습니다. 그래서 매번 같은 말을 반복할 필요가 없습니다.

### 왜 이게 작동합니까?

LLM(대규모 언어 모델)은 "컨텍스트 윈도우"라는 것을 가지고 있습니다. 대화할 때 이 안에 들어있는 정보를 참고합니다. CLAUDE.md를 읽으면 그 내용이 컨텍스트에 포함되어, 마치 "아까 말했던 것"처럼 기억하는 효과가 생깁니다.

```
┌─────────────────────────────────────┐
│         컨텍스트 윈도우              │
│  ┌───────────────────────────────┐  │
│  │  CLAUDE.md 내용 (자동 로드)    │  │
│  │  - 프로젝트 규칙              │  │
│  │  - 기술 스택                  │  │
│  │  - 코딩 컨벤션                │  │
│  └───────────────────────────────┘  │
│  ┌───────────────────────────────┐  │
│  │  사용자 대화                   │  │
│  │  "버튼 만들어줘"              │  │
│  └───────────────────────────────┘  │
│           ↓                         │
│  Claude가 규칙에 맞게 버튼 생성     │
└─────────────────────────────────────┘
```

> 💡 **비전공자 팁**: CLAUDE.md를 "새 팀원에게 주는 온보딩 문서"라고 생각하세요. 팀에 새로 들어온 사람에게 "우리 팀은 이렇게 일해요"라고 설명하는 문서와 같습니다.

### 왜 필요합니까?

**CLAUDE.md 없이:**
```
사용자: 버튼 만들어줘

Claude: (어떤 스타일로 만들까?)
버튼을 만들었습니다. (기본 CSS 스타일)

사용자: 아, Tailwind CSS 써야 하는데...
         버튼 다시 만들어줘. Tailwind로.

Claude: (다시 처음부터) Tailwind로 버튼을 만들었습니다.

사용자: 그리고 TypeScript도 써야 해...
```

**CLAUDE.md 있으면:**
```
사용자: 버튼 만들어줘

Claude: (CLAUDE.md 읽음: Tailwind + TypeScript 프로젝트)
TypeScript와 Tailwind CSS를 사용해서 버튼을 만들었습니다.
```

매번 "Tailwind 써", "TypeScript 써" 같은 말을 안 해도 됩니다!

### 현업에서의 CLAUDE.md

실제 회사에서 CLAUDE.md는 이런 가치를 제공합니다:

| 상황 | CLAUDE.md 없이 | CLAUDE.md와 함께 |
|------|---------------|-----------------|
| **새 기능 개발** | 매번 기술 스택 설명 | 바로 작업 시작 |
| **버그 수정** | 코딩 컨벤션 반복 설명 | 일관된 스타일로 수정 |
| **팀원 변경** | 구두로 규칙 전달 (누락 위험) | 문서화된 규칙 공유 |
| **프로젝트 인수인계** | 긴 설명 필요 | 파일 하나로 맥락 전달 |

> 🔥 **프로 팁**: 좋은 CLAUDE.md는 새 팀원이 프로젝트에 빠르게 적응하는 데도 도움이 됩니다. AI만을 위한 문서가 아니라, 프로젝트의 "살아있는 문서"입니다.

---

## CLAUDE.md 만들기

### 어디에 만들까요?

프로젝트 폴더 맨 위(루트)에 만드세요.

```
my-project/
├── CLAUDE.md       <- 여기!
├── package.json
├── src/
│   ├── components/
│   ├── pages/
│   └── utils/
└── ...
```

> ⚠️ **주의사항**: 파일 이름은 정확히 `CLAUDE.md`여야 합니다. 대소문자를 구분합니다! `claude.md`나 `Claude.md`는 인식되지 않을 수 있습니다.

### Claude에게 시키기

**기본 요청:**
```
> 이 프로젝트용 CLAUDE.md 만들어줘
```

Claude가 프로젝트를 분석해서 만들어줍니다. `package.json`, 폴더 구조, 기존 코드 스타일 등을 보고 프로젝트의 특성을 파악합니다.

**실제 대화 예시:**

```
사용자: 이 프로젝트용 CLAUDE.md 만들어줘

Claude: 프로젝트를 분석할게요.

[package.json 확인]
- React 18
- TypeScript
- Tailwind CSS
- Vite

[폴더 구조 확인]
- src/components/ - 컴포넌트
- src/pages/ - 페이지
- src/hooks/ - 커스텀 훅

[기존 코드 스타일 분석]
- 함수형 컴포넌트 사용
- PascalCase 컴포넌트명
- 한글 주석

CLAUDE.md를 생성했습니다:

# My Project

## 기술 스택
- React 18 + TypeScript
- Tailwind CSS
- Vite

## 폴더 구조
- `src/components/` - 재사용 컴포넌트
- `src/pages/` - 페이지 컴포넌트
- `src/hooks/` - 커스텀 훅

## 코딩 규칙
- 함수형 컴포넌트만 사용
- 컴포넌트명: PascalCase
- 주석: 한글로 작성
```

**더 좋은 CLAUDE.md를 원한다면:**

```
> CLAUDE.md 만들어줘. 특히 네이밍 규칙이랑 폴더 구조 강조해줘.
> 우리 팀은 함수형 컴포넌트만 쓰고, any 타입은 절대 안 써.
```

강조하고 싶은 부분을 알려주면 그에 맞게 작성합니다.

### 직접 만들기

Claude 없이 직접 만들 수도 있습니다. 간단한 예시:

```markdown
# My Project

## 기술 스택
- React
- TypeScript
- Tailwind CSS

## 규칙
- 함수형 컴포넌트 사용
- 한글 주석 작성
```

> 💡 **비전공자 팁**: 처음엔 Claude에게 만들어달라고 하고, 그 다음부터 직접 수정하는 게 가장 쉬운 방법입니다.

---

## 무엇을 써야 합니까?

효과적인 CLAUDE.md의 핵심 구성요소를 알아봅시다.

### 1. 기술 스택 (필수)

이 프로젝트가 뭘 쓰는지 알려주세요.

```markdown
## 기술 스택
- Frontend: React 18 + TypeScript
- Styling: Tailwind CSS
- State: Zustand
- API: React Query
- 패키지 매니저: pnpm
```

**왜 중요합니까?**
- Claude가 올바른 문법과 패턴을 사용
- 버전에 맞는 API 사용
- 호환되지 않는 코드 방지

### 2. 폴더 구조 (필수)

파일들이 어디 있는지 알려주세요.

```markdown
## 폴더 구조
- `src/components/` - 재사용 가능한 UI 컴포넌트
- `src/components/ui/` - 기본 UI 요소 (Button, Input 등)
- `src/pages/` - 페이지 컴포넌트
- `src/hooks/` - 커스텀 훅
- `src/utils/` - 유틸리티 함수
- `src/types/` - TypeScript 타입 정의
- `src/api/` - API 관련 함수
```

**왜 중요합니까?**
- Claude가 새 파일을 올바른 위치에 생성
- 기존 파일을 쉽게 찾음
- 프로젝트 구조 일관성 유지

### 3. 코딩 규칙 (필수)

코드 스타일을 알려주세요.

```markdown
## 코딩 규칙

### 네이밍
- 컴포넌트: PascalCase (예: `UserProfile.tsx`)
- 훅: camelCase + use 접두어 (예: `useAuth.ts`)
- 유틸: camelCase (예: `formatDate.ts`)
- 상수: SCREAMING_SNAKE_CASE (예: `MAX_RETRY_COUNT`)

### 스타일
- 들여쓰기: 2칸
- 문자열: 작은따옴표 (`'`)
- 세미콜론: 사용 안 함

### 컴포넌트
- 함수형 컴포넌트만 사용
- props는 인터페이스로 정의
- 한 파일에 한 컴포넌트
```

### 4. 하지 말아야 할 것 (매우 중요!)

금지 사항을 명확하게 알려주세요.

```markdown
## 금지 사항 (절대 하지 마)
- `any` 타입 사용 금지
- 인라인 스타일 금지 (Tailwind 사용)
- `console.log` 커밋 금지
- class 컴포넌트 사용 금지
- jQuery 사용 금지
- 한 컴포넌트 200줄 초과 금지
```

**왜 중요합니까?**
Claude는 "하지 마"라는 명시적 지시를 잘 따릅니다. 모호하게 "X를 피해"보다 "X 사용 금지"가 더 효과적입니다.

> 🔥 **프로 팁**: 금지 사항에는 그 이유도 함께 적으면 Claude가 더 잘 이해합니다. 예: "`any` 타입 사용 금지 (타입 안전성 유지)"

### 5. 자주 쓰는 명령어

```markdown
## 명령어
- 개발 서버: `pnpm dev`
- 빌드: `pnpm build`
- 테스트: `pnpm test`
- 린트: `pnpm lint`
- 타입 체크: `pnpm typecheck`
```

### 6. 환경 설정 (선택)

```markdown
## 환경 변수
- `VITE_API_URL` - 백엔드 API 주소
- `VITE_GA_ID` - Google Analytics ID

## 브랜치 전략
- `main` - 프로덕션
- `develop` - 개발
- `feature/*` - 기능 개발
```

---

## 실제 예시: 다양한 프로젝트 유형

### 예시 1: 간단한 웹사이트 프로젝트

```markdown
# 내 포트폴리오

## 소개
개인 포트폴리오 웹사이트입니다.

## 기술 스택
- HTML5, CSS3, JavaScript (ES6+)
- 스타일: Tailwind CSS (CDN)
- 아이콘: Font Awesome

## 파일 구조
- `index.html` - 메인 페이지
- `about.html` - 소개 페이지
- `projects.html` - 프로젝트 목록
- `css/` - 스타일 파일
- `js/` - JavaScript 파일
- `images/` - 이미지 파일

## 규칙
- 파일 이름은 소문자와 하이픈 사용 (예: about-me.html)
- 색상은 Tailwind 기본 색상 사용
- 모바일 반응형 필수 (mobile-first)
- 이미지는 WebP 형식 선호

## 금지 사항
- jQuery 사용 금지
- 인라인 스타일 금지
- !important 사용 최소화
```

### 예시 2: React + TypeScript 프로젝트

```markdown
# Todo App

## 기술 스택
- React 18
- TypeScript 5.x
- Tailwind CSS
- Zustand (상태 관리)
- React Query (서버 상태)
- Vite (빌드 도구)

## 폴더 구조
```
src/
├── components/       # 재사용 컴포넌트
│   ├── ui/          # 기본 UI (Button, Input, Modal)
│   └── features/    # 기능별 컴포넌트
├── pages/           # 페이지 컴포넌트
├── hooks/           # 커스텀 훅
├── stores/          # Zustand 스토어
├── types/           # TypeScript 타입
├── utils/           # 유틸리티 함수
├── api/             # API 함수
└── constants/       # 상수
```

## 코딩 규칙
- 컴포넌트: PascalCase (예: TodoItem.tsx)
- 훅: camelCase + use (예: useTodos.ts)
- 타입: PascalCase + 접미어 (예: TodoItemProps, UserResponse)
- 상수: SCREAMING_SNAKE_CASE
- 함수형 컴포넌트 + hooks만 사용
- 한 컴포넌트 최대 150줄

## 컴포넌트 작성 패턴
```tsx
// 좋은 예
interface ButtonProps {
  children: React.ReactNode
  onClick?: () => void
  variant?: 'primary' | 'secondary'
}

export function Button({ children, onClick, variant = 'primary' }: ButtonProps) {
  return (...)
}
```

## 금지 사항
- any 타입 절대 금지
- class 컴포넌트 금지
- 인라인 스타일 금지
- console.log 커밋 금지
- 하드코딩된 문자열 금지 (상수로 분리)

## 명령어
- `pnpm dev` - 개발 서버 (http://localhost:5173)
- `pnpm build` - 프로덕션 빌드
- `pnpm preview` - 빌드 미리보기
- `pnpm lint` - ESLint 검사
- `pnpm test` - 테스트 실행
```

### 예시 3: 백엔드 API 프로젝트

```markdown
# User API Service

## 기술 스택
- Node.js 20.x
- Express.js
- TypeScript
- PostgreSQL
- Prisma ORM
- Jest (테스트)

## 폴더 구조
```
src/
├── controllers/     # 요청 처리
├── services/        # 비즈니스 로직
├── repositories/    # 데이터베이스 접근
├── middlewares/     # 미들웨어
├── routes/          # 라우트 정의
├── types/           # TypeScript 타입
├── utils/           # 유틸리티
└── prisma/          # Prisma 스키마
```

## API 규칙
- RESTful 설계 원칙 준수
- 응답 형식: { success: boolean, data?: T, error?: string }
- 에러 핸들링: try-catch + next(error)
- 인증: JWT Bearer 토큰

## 네이밍 규칙
- 컨트롤러: `UserController.ts`
- 서비스: `UserService.ts`
- 라우트: `/api/v1/users`
- 함수: camelCase (예: `getUserById`)

## 금지 사항
- any 타입 금지
- SQL 직접 쿼리 금지 (Prisma 사용)
- 하드코딩된 시크릿 금지 (환경변수 사용)
- console.log 금지 (logger 사용)

## 명령어
- `pnpm dev` - 개발 서버 (nodemon)
- `pnpm build` - TypeScript 컴파일
- `pnpm start` - 프로덕션 실행
- `pnpm test` - Jest 테스트
- `pnpm db:migrate` - DB 마이그레이션
- `pnpm db:seed` - 시드 데이터 삽입
```

---

## 개인 메모리 vs 프로젝트 메모리

Claude에는 두 가지 유형의 메모리가 있습니다.

### 프로젝트 메모리 (팀용)

위치: `프로젝트폴더/CLAUDE.md`

- **팀 전체가 공유**
- Git에 저장됨
- 프로젝트 규칙 정의
- 팀원 모두에게 동일하게 적용

### 개인 메모리 (나만의 설정)

위치: `~/.claude/CLAUDE.md` (홈 폴더)

- **나만 사용**
- 모든 프로젝트에 적용
- 개인 선호도 설정

```markdown
# 내 개인 설정

## 선호도
- 설명은 항상 한글로 해줘
- 코드에 주석 많이 달아줘
- 예시 코드 항상 보여줘
- 변경 전에 먼저 계획을 설명해줘

## 자주 쓰는 프레임워크
- React + TypeScript 선호
- Tailwind CSS 선호
- pnpm 사용
```

### 우선순위

```
프로젝트 CLAUDE.md > 개인 CLAUDE.md
```

프로젝트 규칙이 더 우선입니다. 프로젝트에서 "CSS Modules 사용"이라고 하면, 개인 설정의 "Tailwind 선호"는 무시됩니다.

> 💡 **비전공자 팁**: 처음에는 개인 CLAUDE.md에 "한글로 설명해줘" 정도만 넣어두면 편합니다. 프로젝트 규칙은 각 프로젝트의 CLAUDE.md에 작성하세요.

---

## Claude가 규칙을 어기면 어떻게 합니까?

CLAUDE.md가 있어도 Claude가 가끔 규칙을 어길 수 있습니다. 이럴 때는 CLAUDE.md를 업데이트하세요.

### 패턴 1: 규칙이 모호할 때

**문제:**
```markdown
## 규칙
- TypeScript 선호
```

Claude: "JavaScript로도 괜찮을 것 같아서 JS로 작성했어요"

**해결:**
```markdown
## 규칙
- TypeScript 필수 사용. JavaScript 절대 금지.
```

### 패턴 2: 규칙이 누락됐을 때

**상황:** Claude가 `var`를 사용함

**해결:**
```markdown
## 금지 사항
- var 사용 금지, const나 let만 사용
```

### 패턴 3: 규칙이 충돌할 때

**문제:**
```markdown
- 간결한 코드 작성
- 모든 곳에 자세한 주석 추가
```

**해결:** 우선순위 명시
```markdown
## 코드 스타일
1. 먼저 가독성 좋은 코드 작성
2. 복잡한 로직에만 주석 추가
3. 자명한 코드에는 주석 불필요
```

### 실제 대화 예시: 규칙 위반 처리

```
사용자: 버튼 만들어줘

Claude: (inline style로 버튼 생성)

사용자: 우리는 인라인 스타일 안 써. Tailwind로 다시 해줘.
       그리고 CLAUDE.md에 "인라인 스타일 절대 금지" 추가해줘.

Claude: 죄송해요, CLAUDE.md를 확인하지 못했네요.
        Tailwind로 다시 만들고, CLAUDE.md도 업데이트할게요.

[파일 수정]

다음부터는 인라인 스타일을 사용하지 않을게요.
```

> 🔥 **프로 팁**: Claude에게 같은 피드백을 두 번 이상 했다면, 그건 CLAUDE.md에 추가해야 한다는 신호입니다!

---

## 응답 언어 설정

Claude가 계속 영어로 대답하면, 설정 파일로 바꿀 수 있습니다.

### 방법 1: 설정 파일

`.claude/settings.json` 파일:

```json
{
  "language": "korean"
}
```

### 방법 2: CLAUDE.md에 추가

```markdown
## 언어
- 항상 한국어로 응답
- 코드 주석도 한국어로
- 변수명만 영어 사용
```

---

## 심화: CLAUDE.md를 더 잘 활용하기

### 기법 1: 규칙에 이유를 붙이기

"이렇게 해라"보다 "이런 이유로 이렇게 해라"가 더 효과적입니다. Claude가 비슷한 상황에서 스스로 올바른 판단을 내릴 수 있기 때문입니다.

```markdown
# 이유 없이
- TypeScript strict mode 사용

# 이유와 함께 (더 효과적)
- TypeScript strict mode 사용 (암묵적 any 타입으로 인한 버그 방지)
- 컴포넌트 150줄 제한 (유지보수성과 테스트 용이성을 위해)
- 한 함수 한 가지 역할 (디버깅과 재사용 용이)
```

### 기법 2: 예시 코드 포함하기

규칙만 나열하는 것보다 예시를 보여주면 Claude가 더 정확하게 따릅니다.

```markdown
## 컴포넌트 작성 패턴

### 좋은 예
```tsx
interface ButtonProps {
  children: React.ReactNode
  variant?: 'primary' | 'secondary'
  onClick?: () => void
}

export function Button({ children, variant = 'primary', onClick }: ButtonProps) {
  return (
    <button
      className={`btn btn-${variant}`}
      onClick={onClick}
    >
      {children}
    </button>
  )
}
```

### 나쁜 예
```tsx
// ❌ any 타입
function Button(props: any) { ... }

// ❌ 인라인 스타일
<button style={{color: 'red'}}>Click</button>
```
```

### 기법 3: 간결함이 핵심

CLAUDE.md가 길어질수록 효과가 떨어집니다. Claude도 사람처럼 너무 많은 지시를 받으면 일부를 놓칠 수 있습니다.

```markdown
# ❌ 나쁜 예 (너무 김)
우리 프로젝트는 2024년 1월에 시작해서
처음에는 JavaScript였다가 TypeScript로 바꿨고
그 이유는... (100줄)

# ✅ 좋은 예 (간결함)
## 기술 스택
- TypeScript (strict mode)
- React 18
- Tailwind CSS
```

**권장 길이: 50-100줄 이하**

### 기법 4: 반복되는 수정 = CLAUDE.md에 추가할 신호

```
1회: "주석은 한글로 써줘" → Claude가 수정함
2회: "또 영어로 썼네. 한글로 해줘" → CLAUDE.md에 추가할 때!
```

이렇게 축적하면 시간이 지날수록 Claude가 여러분의 프로젝트를 더 잘 이해하게 됩니다.

### 기법 5: 외부 메모리로 장기 작업 관리하기

대화 맥락은 언젠가 사라지지만, 파일은 남습니다. 복잡하거나 긴 작업을 할 때는 진행 상황을 파일로 기록해두세요.

```markdown
# progress.md

## 현재 진행 상황
- [x] 로그인 UI 완성
- [x] 로그인 API 연동
- [ ] 에러 처리 구현 중
- [ ] 비밀번호 찾기 예정

## 결정 사항
- 세션 대신 JWT 사용
- 토큰 만료: 7일
- 리프레시 토큰 구현 예정

## 다음에 할 것
- 에러 메시지 UI
- 리프레시 토큰 구현
- 로그아웃 기능
```

이렇게 하면:
- 새 대화를 시작해도 Claude에게 파일을 읽게 해서 이어갈 수 있습니다
- 어디까지 했는지 한눈에 파악됩니다
- 팀원과 공유할 때도 유용합니다

---

## 🎯 미니 퀴즈

### 퀴즈 1
Q: CLAUDE.md는 어디에 위치해야 합니까?

<details>
<summary>정답 보기</summary>

프로젝트 폴더의 맨 위(루트)에 위치해야 합니다.
```
my-project/
├── CLAUDE.md  <- 여기!
├── src/
└── ...
```
</details>

### 퀴즈 2
Q: 프로젝트 CLAUDE.md와 개인 CLAUDE.md가 충돌하면 어떻게 됩니까?

<details>
<summary>정답 보기</summary>

프로젝트 CLAUDE.md가 우선입니다. 프로젝트의 규칙이 개인 설정보다 더 중요하게 적용됩니다.
</details>

### 퀴즈 3
Q: CLAUDE.md에 "TypeScript 선호"라고 썼는데 Claude가 JavaScript를 사용했다면?

<details>
<summary>정답 보기</summary>

규칙이 모호하기 때문입니다. "TypeScript 필수 사용. JavaScript 절대 금지."처럼 더 명확하게 수정해야 합니다.
</details>

---

## 📝 실습 과제

### 난이도 1: 첫 CLAUDE.md 만들기 (초급)

```
# 새 폴더 만들기
> my-project라는 폴더 만들고 들어가줘

# Claude에게 CLAUDE.md 만들어달라고 하기
> 이 프로젝트용 CLAUDE.md 만들어줘.
> Tailwind CSS 쓰는 React + TypeScript 프로젝트야.
```

**완료 체크:**
- [ ] CLAUDE.md 파일이 생성됨
- [ ] 기술 스택이 명시됨
- [ ] 폴더 구조가 정의됨

### 난이도 2: 규칙 테스트하기 (중급)

```
# Claude에게 뭔가 만들어달라고 하기
> 버튼 컴포넌트 만들어줘

# Claude가 규칙을 따랐는지 확인
> CLAUDE.md에 있는 대로 TypeScript랑 Tailwind 썼어?

# 문제가 있으면 규칙 추가
> CLAUDE.md에 "인라인 스타일 절대 금지" 추가해줘
```

**완료 체크:**
- [ ] 버튼 컴포넌트가 규칙대로 생성됨
- [ ] TypeScript 사용됨
- [ ] Tailwind 사용됨

### 난이도 3: 종합 CLAUDE.md 구축 (고급)

실제 프로젝트처럼 상세한 CLAUDE.md를 만들어보세요:

```
> CLAUDE.md를 다음 내용으로 만들어줘:
> 1. 기술 스택 (React, TypeScript, Tailwind, Zustand)
> 2. 상세 폴더 구조
> 3. 네이밍 규칙 (컴포넌트, 훅, 유틸)
> 4. 코딩 컨벤션 (좋은 예/나쁜 예 포함)
> 5. 금지 사항 (이유 포함)
> 6. 명령어 목록
```

**완료 체크:**
- [ ] 모든 섹션이 포함됨
- [ ] 예시 코드가 있음
- [ ] 금지 사항에 이유가 있음

---

## 🏆 도전 과제

### 도전 1: 규칙 위반 감지
```
> CLAUDE.md 규칙을 일부러 어기는 코드를 만들어봐.
> 그리고 왜 그게 규칙 위반인지 설명해줘.
```

### 도전 2: 팀 CLAUDE.md 시뮬레이션
가상의 3인 팀 프로젝트를 위한 CLAUDE.md를 만들어보세요:
- 프론트엔드 개발자
- 백엔드 개발자
- 디자이너

### 도전 3: 자동 업데이트
```
> 오늘 대화에서 내가 준 피드백들을 정리해서
> CLAUDE.md에 추가해야 할 규칙이 있으면 제안해줘.
```

---

## 문제가 발생하면

**Claude가 CLAUDE.md를 안 읽는 것 같습니까?**
- 파일 이름 확인: 정확히 `CLAUDE.md`여야 합니다 (대소문자 구분)
- 위치 확인: 프로젝트 루트 폴더에 있어야 합니다
- 시도: "CLAUDE.md 읽고 뭐가 있는지 알려줘"

**Claude가 특정 규칙을 무시합니까?**
- 규칙을 더 명시적이고 직접적으로 작성하세요
- 나쁜 예: "TypeScript 선호"
- 좋은 예: "항상 TypeScript 사용. JavaScript 절대 사용 금지."

**CLAUDE.md가 너무 길어지고 있습니까?**
- 가능하면 50-100줄 이하로 유지하세요
- 오래된 규칙 제거
- 프로젝트 역사가 아닌 지금 적용되는 규칙에 집중

**규칙끼리 충돌합니까?**
- 우선순위 정하기: 중요한 규칙을 먼저
- 구체적으로: "컴포넌트에는 X, utils에는 Y 사용"

**개인 규칙과 프로젝트 규칙이 충돌합니까?**
- 프로젝트 규칙 (로컬 CLAUDE.md)이 개인 규칙보다 우선
- 필요하면 해당 프로젝트에 맞게 개인 설정 조정

---

## 자주 하는 실수

### 실수 1: 규칙 대신 소설 쓰기

```markdown
# ❌ 나쁜 예 - 너무 많은 배경 설명
우리 팀은 2023년에 이 프로젝트를 시작했는데
재고 관리를 더 잘 할 방법이 필요하다고 결정했기 때문입니다.
많은 논의 끝에 React를 선택했는데 그 이유는...
(200줄 계속)

# ✅ 좋은 예 - 규칙만
## 기술 스택
- React 18, TypeScript, Tailwind CSS

## 규칙
- 함수형 컴포넌트만
- 인라인 스타일 금지
```

### 실수 2: 너무 모호하게 쓰기

```markdown
# ❌ 나쁜 예
- 좋은 코드 작성
- 모범 사례 사용
- 깔끔하게

# ✅ 좋은 예
- TypeScript strict mode 사용
- any 타입 금지
- 컴포넌트 200줄 이하
- 함수 하나당 한 가지 역할
```

### 실수 3: 규칙 바뀌었을 때 업데이트 안 하기

```
# 문제: 팀이 CSS Modules에서 Tailwind로 전환하기로 결정
# 하지만 CLAUDE.md는 여전히 "CSS Modules 사용"이라고 함

# 해결책
> CLAUDE.md 업데이트: 이제 CSS Modules 대신 Tailwind CSS 사용
```

### 실수 4: 이유 설명 안 하기

```markdown
# ❌ 나쁜 예 - 이유 없는 규칙
- 컴포넌트 200줄 이하 유지

# ✅ 좋은 예 - 이유 있는 규칙 (Claude가 더 잘 이해)
- 컴포넌트 200줄 이하 유지 (유지보수성과 테스트 용이성을 위해)
```

### 실수 5: 규칙 테스트 안 하기

```
# ❌ 나쁜 예 - 확인 없이 규칙 추가
> CLAUDE.md에 추가: "파일 이름은 camelCase로"
[실제로 Claude가 따르는지 확인 안 함]

# ✅ 좋은 예 - 추가 후 테스트
> CLAUDE.md에 추가: "파일 이름은 camelCase로"
> 유틸리티 파일 만들어줘
> (확인: 파일 이름이 camelCase인가?)
```

---

## 용어 사전

| 용어 | 설명 |
|------|------|
| **CLAUDE.md** | Claude에게 프로젝트 정보를 전달하는 마크다운 파일 |
| **컨텍스트 윈도우** | LLM이 한 번에 참조할 수 있는 정보의 양 |
| **프로젝트 메모리** | 프로젝트별 CLAUDE.md (팀 공유) |
| **개인 메모리** | 홈 폴더의 CLAUDE.md (개인 설정) |
| **코딩 컨벤션** | 코드 작성 규칙과 스타일 |
| **네이밍 규칙** | 변수, 함수, 파일 등의 이름 짓는 규칙 |

---

## 정리

이번 챕터에서 배운 것:
- [x] CLAUDE.md가 무엇이고 왜 필요한지
- [x] 어디에 만들고 무엇을 써야 하는지
- [x] 개인 vs 프로젝트 메모리의 차이
- [x] 규칙 위반 시 대처법
- [x] 효과적인 CLAUDE.md 작성 기법

CLAUDE.md는 LLM의 컨텍스트 메커니즘을 활용한 것입니다. 잘 작성해두면 매번 같은 설명을 반복하지 않아도 Claude가 프로젝트 맥락을 이해한 상태에서 작업을 시작합니다.

> 💡 **핵심 포인트**: CLAUDE.md는 한 번 쓰고 끝나는 문서가 아닙니다. 프로젝트가 진행되면서 계속 다듬어가는 **살아있는 문서**입니다. 새로운 규칙이 생기면 추가하고, 더 이상 적용되지 않는 규칙은 삭제하세요.

축하합니다! Part 2 (핵심 기능)를 완료했습니다.

---

## 다음 챕터 미리보기

Chapter 13에서는 **웹사이트 개발**을 실습합니다.

지금까지 배운 것들을 총동원해서 실제 포트폴리오 웹사이트를 처음부터 끝까지 만들어봅니다:
- HTML/CSS/JavaScript 기초
- Claude와 함께 단계별 개발
- 디자인 피드백과 반복
- 반응형 웹 구현

이론은 끝났습니다. 이제 진짜 만들어봅시다!

[Chapter 13: 웹사이트 개발](../Chapter13-Website-Development/README.ko.md)로 넘어가세요.

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [Claude Code Memory 가이드](https://docs.anthropic.com/en/docs/claude-code/memory) - CLAUDE.md 공식 문서
- [Anthropic 프롬프트 가이드](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering) - 효과적인 지시 작성법

**영상 자료:**
- [Claude Code 설정 (YouTube)](https://www.youtube.com/results?search_query=claude+code+configuration+tutorial) - Claude Code 설정 가이드
- [프로젝트 문서화 (YouTube)](https://www.youtube.com/results?search_query=project+documentation+best+practices) - 문서화 베스트 프랙티스

**읽을거리:**
- [Markdown 가이드](https://www.markdownguide.org/) - Markdown 작성법
- [좋은 README 작성하기](https://www.makeareadme.com/) - README 작성 가이드

**관련 도구:**
- [Markdown Editor](https://stackedit.io/) - 온라인 Markdown 에디터
- [Claude Code GitHub](https://github.com/anthropics/claude-code) - 공식 저장소

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
