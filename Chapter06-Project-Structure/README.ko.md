# Chapter 06: 프로젝트 구조 이해

[English](./README.md) | **한국어**

---

## 💬 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 🎯 이 챕터의 목표

- 프로젝트 폴더 구조의 **철학**과 **이유** 이해하기
- 중요한 파일들의 역할과 **왜 그 위치에 있는지** 알기
- 다양한 프레임워크별 구조 비교하기
- Claude에게 프로젝트 설명 요청하는 고급 기법 익히기
- 좋은 구조와 나쁜 구조 구별하기

---

## ⏱️ 예상 소요 시간

- 읽기: **30분**
- 실습: **45분**

---

## 📋 필요한 것

- Claude Code 설치 및 로그인 완료
- 터미널 명령어 기본 이해 (Chapter 05)

---

## 🔗 이전 챕터 복습

Chapter 05에서 터미널 명령어를 배웠습니다. `ls`, `cd`, `mkdir` 같은 명령어로 폴더를 탐색할 수 있게 되었죠. 이제 이 능력을 활용해서 프로젝트 구조를 이해하는 방법을 배워봅시다.

---

## 왜 프로젝트 구조가 중요합니까?

코드를 작성하기 전에, 프로젝트가 어떻게 구성되어 있는지 이해해야 합니다. 마치 새 집에 이사 왔을 때 어디에 뭐가 있는지 파악하는 것과 같습니다.

**실제 상황:**
- GitHub에서 프로젝트를 다운받았는데, 뭐가 뭔지 모르겠다
- 어떤 파일을 수정해야 하는지 모르겠다
- 프로젝트가 어떻게 동작하는지 이해하고 싶다
- 팀에 합류했는데 코드베이스를 빠르게 파악해야 한다

### 쉬운 비유: 새 집 구경하기

새 집에 이사 오면 먼저 집 구조를 파악합니다:
- 현관은 어디인지 (진입점)
- 부엌은 어디인지 (핵심 기능)
- 창고는 어디인지 (보조 파일)
- 어떤 방이 어떤 용도인지 (폴더별 역할)

프로젝트도 마찬가지입니다. 폴더와 파일의 역할을 알면 필요한 것을 빠르게 찾을 수 있습니다.

> 💡 **비전공자 팁**
>
> 프로젝트 구조는 **개발자들 사이의 약속**입니다.
> "컴포넌트는 components 폴더에"라는 약속이 있으면,
> 누구든 프로젝트를 열었을 때 컴포넌트를 어디서 찾아야 하는지 바로 알 수 있습니다.
> 마치 모든 집에서 화장실이 비슷한 위치에 있는 것처럼요.

---

## 폴더 구조의 철학: 왜 이렇게 나누는가?

폴더 구조는 단순히 파일을 정리하는 것이 아닙니다. 좋은 구조에는 **철학**이 담겨 있습니다.

### 1. 관심사의 분리 (Separation of Concerns)

비슷한 역할을 하는 코드끼리 모아둡니다.

```
my-project/
├── src/
│   ├── components/    # UI 관련 코드만
│   ├── api/           # 서버 통신 관련만
│   ├── styles/        # 디자인 관련만
│   └── utils/         # 유틸리티 함수만
```

**왜 이렇게 할까요?**
- 버튼 디자인을 수정할 때, `components` 폴더만 보면 됩니다
- API가 바뀌어도 `api` 폴더만 수정하면 됩니다
- 한 곳의 변경이 다른 곳에 영향을 주지 않습니다

> 🔥 **프로 팁**
>
> 실무에서는 "이 파일 어디에 넣어야 하지?"라는 고민이 자주 생깁니다.
> 핵심 질문: **"이 파일의 주요 책임이 뭐지?"**
> - UI를 그린다면 → `components`
> - 데이터를 가져온다면 → `api` 또는 `services`
> - 여러 곳에서 쓰는 헬퍼 함수라면 → `utils`

### 2. 확장성 (Scalability)

프로젝트가 커져도 구조가 무너지지 않아야 합니다.

```
# 나쁜 예: 모든 파일이 한 곳에
src/
├── App.js
├── Button.js
├── Header.js
├── api.js
├── utils.js
├── Login.js
├── Dashboard.js
├── UserProfile.js
├── ... (100개의 파일)

# 좋은 예: 역할별로 분리
src/
├── components/
│   ├── common/      # 공통 컴포넌트
│   ├── auth/        # 인증 관련
│   └── dashboard/   # 대시보드 관련
├── pages/
├── api/
└── utils/
```

### 3. 발견 가능성 (Discoverability)

파일을 쉽게 찾을 수 있어야 합니다.

```
# 나쁜 예: 이름만으로 알 수 없음
src/
├── stuff/
├── things/
├── misc/

# 좋은 예: 이름이 곧 설명
src/
├── authentication/
├── user-management/
├── payment-processing/
```

> ⚠️ **주의사항**
>
> 초보자가 흔히 빠지는 함정:
> 1. **과도한 폴더 중첩**: `src/components/ui/buttons/primary/index.js` (너무 깊음!)
> 2. **불명확한 이름**: `helpers`, `misc`, `other` (뭐가 들어있는지 알 수 없음)
> 3. **일관성 없음**: 어떤 건 폴더, 어떤 건 파일 (규칙이 없음)

---

## 일반적인 프로젝트 구조

대부분의 웹 프로젝트는 비슷한 구조를 가지고 있습니다.

### 웹 프로젝트 기본 구조

```
my-project/
├── src/                  # 소스 코드 (핵심) - 우리가 작성하는 코드
│   ├── components/       # 재사용 가능한 UI 조각 (버튼, 카드, 모달 등)
│   ├── pages/            # 페이지별 코드 (홈, 로그인, 설정 등)
│   ├── styles/           # CSS 스타일 파일들
│   ├── utils/            # 유틸리티 함수 (날짜 변환, 포맷팅 등)
│   ├── hooks/            # React 커스텀 훅 (상태 관리 로직)
│   ├── api/              # API 호출 함수들
│   └── types/            # TypeScript 타입 정의
├── public/               # 정적 파일 (이미지, 아이콘, favicon)
├── tests/                # 테스트 파일들
├── node_modules/         # 설치된 패키지 (건드리지 않음!)
├── package.json          # 프로젝트 설정 파일 (신분증)
├── package-lock.json     # 정확한 패키지 버전 기록
├── README.md             # 프로젝트 설명서
├── .gitignore            # Git이 무시할 파일 목록
├── .env                  # 환경 변수 (비밀 정보)
└── tsconfig.json         # TypeScript 설정 (있는 경우)
```

> 💡 **비전공자 팁**
>
> 이 구조를 외울 필요 없습니다!
> Claude에게 "이 프로젝트 구조 설명해줘"라고 하면 알려줍니다.
> 중요한 건 **구조가 있다**는 것을 아는 것입니다.
>
> 처음에는 `src` 폴더와 `package.json`만 기억하세요. 나머지는 필요할 때 배우면 됩니다.

---

## 프레임워크별 구조 비교

### Next.js 프로젝트 (App Router)

```
nextjs-project/
├── app/                  # 페이지와 라우팅 (폴더 = URL 경로)
│   ├── page.tsx          # 홈페이지 (/)
│   ├── layout.tsx        # 공통 레이아웃
│   ├── about/
│   │   └── page.tsx      # /about 페이지
│   └── api/
│       └── users/
│           └── route.ts  # /api/users API
├── components/           # UI 컴포넌트
├── lib/                  # 유틸리티 함수
├── public/               # 정적 파일
└── package.json
```

**특징**: 폴더 구조가 곧 URL 구조! `app/about/page.tsx` = `/about` 페이지

### React (Create React App)

```
react-project/
├── src/
│   ├── App.js            # 메인 앱 컴포넌트
│   ├── index.js          # 진입점
│   ├── components/
│   ├── pages/
│   └── styles/
├── public/
│   └── index.html        # HTML 템플릿
└── package.json
```

**특징**: `src/index.js`가 시작점, 라우팅은 별도 라이브러리 필요

### Vue.js 프로젝트

```
vue-project/
├── src/
│   ├── App.vue           # 메인 앱 컴포넌트
│   ├── main.js           # 진입점
│   ├── components/
│   ├── views/            # 페이지 컴포넌트
│   ├── router/           # 라우팅 설정
│   └── store/            # 상태 관리 (Vuex/Pinia)
└── package.json
```

**특징**: `.vue` 확장자 사용, `views` 폴더가 페이지 역할

### Python Django 프로젝트

```
django-project/
├── manage.py             # Django 명령어 실행
├── mysite/               # 프로젝트 설정
│   ├── settings.py       # 전체 설정
│   ├── urls.py           # URL 라우팅
│   └── wsgi.py           # 웹 서버 설정
├── myapp/                # 앱 (기능 단위)
│   ├── models.py         # 데이터베이스 모델
│   ├── views.py          # 요청 처리 로직
│   ├── urls.py           # 앱 URL
│   └── templates/        # HTML 템플릿
└── requirements.txt      # 패키지 목록 (package.json 역할)
```

**특징**: `앱` 단위로 기능 분리, `models.py`에서 DB 구조 정의

> 🔥 **프로 팁**
>
> 프레임워크가 달라도 **핵심 개념**은 같습니다:
> - 진입점(Entry Point)이 있다
> - 컴포넌트/뷰가 있다
> - 라우팅(URL 연결)이 있다
> - 설정 파일이 있다
>
> 한 프레임워크를 익히면 다른 것도 빠르게 배울 수 있습니다!

---

## 핵심 파일들 심층 분석

### package.json - 프로젝트의 신분증

모든 Node.js 프로젝트에 있는 가장 중요한 파일입니다. 프로젝트에 대한 모든 정보가 담겨 있습니다.

```json
{
  "name": "my-awesome-project",      // 프로젝트 이름
  "version": "1.0.0",                // 버전 번호
  "description": "할 일 관리 앱",     // 설명
  "main": "index.js",                // 진입점 파일
  "scripts": {                       // 실행 가능한 명령어
    "dev": "next dev",               // 개발 서버 시작
    "build": "next build",           // 배포용 빌드
    "start": "next start",           // 프로덕션 서버 시작
    "test": "jest",                  // 테스트 실행
    "lint": "eslint ."               // 코드 검사
  },
  "dependencies": {                  // 실행에 필요한 패키지
    "react": "^18.2.0",
    "next": "^14.0.0"
  },
  "devDependencies": {               // 개발에만 필요한 패키지
    "typescript": "^5.0.0",
    "eslint": "^8.0.0"
  }
}
```

**이 파일이 알려주는 것:**
- `name`: 프로젝트 이름
- `scripts`: 실행 가능한 명령어 (`npm run dev` 등)
- `dependencies`: 앱이 동작하는 데 필수적인 라이브러리
- `devDependencies`: 개발할 때만 필요한 도구들

> 💡 **비전공자 팁**
>
> `dependencies`와 `devDependencies`의 차이:
> - `dependencies`: 앱이 사용자에게 보여질 때 필요한 것 (React, Next.js 등)
> - `devDependencies`: 개발자가 코드 작성할 때만 필요한 것 (ESLint, TypeScript 등)
>
> 비유하자면, 레스토랑에서:
> - `dependencies` = 요리 재료 (손님에게 제공되는 것)
> - `devDependencies` = 주방 도구 (요리사만 사용하는 것)

### README.md - 프로젝트 설명서

프로젝트가 무엇인지, 어떻게 실행하는지 설명하는 파일입니다.

**좋은 README가 포함하는 내용:**

```markdown
# 프로젝트 이름

프로젝트에 대한 간단한 설명

## 설치 방법

npm install

## 실행 방법

npm run dev

## 주요 기능
- 기능 1
- 기능 2

## 기술 스택
- React 18
- Next.js 14
- TypeScript

## 폴더 구조 설명
(선택적이지만 매우 유용)
```

좋은 프로젝트는 README만 읽어도 시작할 수 있습니다.

### .gitignore - 무시할 파일 목록

Git에 올리지 않을 파일들을 지정합니다.

```gitignore
# 의존성 폴더 (npm install로 다시 생성 가능)
node_modules/

# 환경 변수 (비밀 정보가 들어있음!)
.env
.env.local

# 빌드 결과물 (빌드 명령어로 다시 생성 가능)
.next/
build/
dist/

# 운영체제 생성 파일
.DS_Store          # macOS
Thumbs.db          # Windows

# IDE 설정 (개인 설정이라 공유할 필요 없음)
.vscode/
.idea/

# 로그 파일
*.log
npm-debug.log*
```

> ⚠️ **주의사항**
>
> `.env` 파일은 절대 Git에 올리면 안 됩니다!
> API 키, 데이터베이스 비밀번호 같은 민감한 정보가 들어있습니다.
> 한 번 올리면 히스토리에 남아서 완전히 지우기 어렵습니다.

### .env - 환경 변수 파일

비밀 정보를 저장하는 파일입니다.

```env
# 절대 Git에 올리지 마세요!

# API 키
NEXT_PUBLIC_API_KEY=your-api-key-here
SECRET_API_KEY=super-secret-key

# 데이터베이스
DATABASE_URL=postgresql://user:password@localhost:5432/mydb

# 외부 서비스
STRIPE_SECRET_KEY=sk_test_xxxxx
```

> 🔥 **프로 팁**
>
> `NEXT_PUBLIC_` 접두사가 있는 변수는 브라우저에서도 접근 가능합니다.
> 비밀 키에는 절대 이 접두사를 붙이지 마세요!
>
> - `NEXT_PUBLIC_API_URL` = 공개 가능 (API 주소)
> - `SECRET_KEY` = 비공개 (서버에서만 사용)

---

## 폴더별 역할 상세 설명

### src/ 폴더

**소스 코드**가 들어있는 핵심 폴더입니다.

| 폴더 | 역할 | 비유 | 예시 파일 |
|------|------|------|----------|
| `components/` | 재사용 가능한 UI 조각 | 레고 블록 | `Button.tsx`, `Modal.tsx` |
| `pages/` | 각 페이지의 코드 | 책의 각 장 | `Home.tsx`, `Login.tsx` |
| `styles/` | 디자인, 색상, 레이아웃 | 인테리어 | `globals.css`, `theme.ts` |
| `utils/` | 도우미 함수들 | 공구함 | `formatDate.ts`, `validate.ts` |
| `api/` | 서버와 통신하는 코드 | 전화기 | `userApi.ts`, `authApi.ts` |
| `hooks/` | React 커스텀 훅 | 자주 쓰는 레시피 | `useAuth.ts`, `useFetch.ts` |
| `types/` | TypeScript 타입 정의 | 설계도 | `User.ts`, `Product.ts` |
| `constants/` | 상수 값들 | 규칙집 | `routes.ts`, `messages.ts` |
| `context/` | React Context | 공유 게시판 | `AuthContext.tsx` |

### components/ 폴더 구조화 패턴

프로젝트가 커지면 컴포넌트도 세분화됩니다.

```
components/
├── common/           # 공통 컴포넌트 (어디서든 사용)
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.styles.ts
│   │   └── index.ts
│   ├── Modal/
│   └── Card/
├── layout/           # 레이아웃 컴포넌트
│   ├── Header/
│   ├── Footer/
│   └── Sidebar/
├── forms/            # 폼 관련 컴포넌트
│   ├── Input/
│   ├── Select/
│   └── Checkbox/
└── features/         # 기능별 컴포넌트
    ├── auth/
    │   ├── LoginForm/
    │   └── SignupForm/
    └── dashboard/
        ├── Stats/
        └── Chart/
```

> 💡 **비전공자 팁**
>
> 처음에는 이렇게 복잡하게 안 해도 됩니다!
> 파일이 10개 이하라면 그냥 `components/` 폴더에 모아두세요.
> 파일이 많아져서 찾기 어려워지면 그때 분리하면 됩니다.
>
> 이것을 **점진적 리팩토링**이라고 합니다.

### public/ 폴더

**정적 파일**이 들어있습니다. 이미지, 아이콘, 폰트 등 변하지 않는 파일들입니다.

```
public/
├── images/           # 이미지 파일들
│   ├── logo.png
│   └── hero-bg.jpg
├── fonts/            # 웹폰트 파일들
│   └── Pretendard.woff2
├── favicon.ico       # 브라우저 탭 아이콘
└── robots.txt        # 검색엔진 크롤러 설정
```

**특징**: 이 폴더의 파일은 URL로 직접 접근 가능합니다.
- `public/images/logo.png` → `http://localhost:3000/images/logo.png`

### node_modules/ 폴더

**절대 건드리지 마세요!**

`npm install`을 실행하면 자동으로 생성되는 폴더입니다. 수천 개의 파일이 있지만, 직접 수정할 일은 없습니다.

```
node_modules/
├── react/            # React 라이브러리
├── next/             # Next.js 프레임워크
├── lodash/           # 유틸리티 라이브러리
└── ... (수백~수천 개의 폴더)
```

> ⚠️ **주의사항**
>
> - 절대 직접 수정하지 마세요
> - Git에 올리지 마세요 (`.gitignore`에 포함)
> - 삭제해도 `npm install`로 복구 가능
> - 용량이 커서 (수백 MB) Git에 올리면 안 됩니다

---

## 좋은 구조 vs 나쁜 구조

### 나쁜 구조의 예와 문제점

```
bad-project/
├── app.js                # 모든 로직이 한 파일에 😱
├── helpers.js            # 뭐가 들어있는지 알 수 없음
├── stuff/                # 이름이 불명확
│   ├── thing1.js
│   └── thing2.js
├── old_code/             # 쓰는지 안 쓰는지 모름
├── test.js               # 테스트 파일이 루트에
├── Button.js             # 컴포넌트가 루트에
├── styles.css            # 스타일도 루트에
└── misc/                 # 온갖 잡동사니
```

**문제점:**
1. 파일이 어디 있는지 찾기 어려움
2. 새 팀원이 이해하기 어려움
3. 파일이 늘어날수록 혼란 가중
4. 비슷한 파일들이 흩어져 있음

### 좋은 구조의 예

```
good-project/
├── src/
│   ├── components/       # UI 컴포넌트 모음
│   │   ├── Button/
│   │   │   ├── Button.tsx
│   │   │   ├── Button.test.tsx
│   │   │   └── Button.styles.ts
│   │   └── Modal/
│   ├── pages/            # 페이지 컴포넌트
│   ├── hooks/            # 커스텀 훅
│   ├── utils/            # 유틸리티 함수
│   ├── api/              # API 호출
│   ├── types/            # 타입 정의
│   └── constants/        # 상수
├── tests/                # E2E 테스트
├── public/               # 정적 파일
├── docs/                 # 문서
└── package.json
```

**장점:**
1. 파일을 바로 찾을 수 있음
2. 새 팀원이 쉽게 이해
3. 확장하기 쉬움
4. 테스트와 스타일이 컴포넌트와 함께

### 구조 비교표

| 항목 | 나쁜 구조 | 좋은 구조 |
|------|----------|----------|
| 파일 위치 | 예측 불가 | 직관적 |
| 폴더 이름 | `stuff`, `misc` | `components`, `utils` |
| 관련 파일 | 흩어져 있음 | 함께 모여 있음 |
| 확장성 | 파일 늘면 혼란 | 체계적 확장 가능 |
| 테스트 | 어디 있는지 모름 | 컴포넌트 옆에 |

---

## 대규모 프로젝트에서의 구조

실제 회사에서 사용하는 대규모 프로젝트 구조를 살펴봅시다.

### 기능 기반 구조 (Feature-based Structure)

```
large-project/
├── src/
│   ├── features/              # 기능별로 분리
│   │   ├── auth/              # 인증 기능
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── api/
│   │   │   ├── types/
│   │   │   └── index.ts
│   │   ├── dashboard/         # 대시보드 기능
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   └── ...
│   │   ├── payment/           # 결제 기능
│   │   └── user-profile/      # 사용자 프로필
│   ├── shared/                # 공유 코드
│   │   ├── components/        # 공통 컴포넌트
│   │   ├── hooks/             # 공통 훅
│   │   └── utils/             # 공통 유틸
│   └── app/                   # 앱 설정
│       ├── routes/
│       └── providers/
```

**장점:**
- 기능 단위로 독립적
- 한 기능의 모든 코드가 한 곳에
- 팀별로 담당 기능 분리 가능

> 🔥 **프로 팁**
>
> 대규모 프로젝트 구조는 처음부터 필요 없습니다.
>
> **권장 진행 순서:**
> 1. 시작: 단순한 `src/components`, `src/pages` 구조
> 2. 성장: 컴포넌트가 30개 넘으면 `common/`, `features/` 분리
> 3. 확장: 기능이 복잡해지면 기능 기반 구조로 전환
>
> **YAGNI 원칙**: "You Aren't Gonna Need It"
> 필요할 때 추가하세요. 미리 복잡하게 만들지 마세요.

### 모노레포 구조 (Monorepo)

여러 프로젝트를 하나의 저장소에서 관리하는 방식입니다.

```
monorepo/
├── apps/                     # 애플리케이션들
│   ├── web/                  # 웹 앱
│   ├── mobile/               # 모바일 앱
│   └── admin/                # 관리자 앱
├── packages/                 # 공유 패키지
│   ├── ui/                   # 공유 UI 컴포넌트
│   ├── utils/                # 공유 유틸리티
│   └── config/               # 공유 설정
└── package.json
```

**사용 예:** 네이버, 카카오 같은 대기업에서 여러 서비스가 공통 컴포넌트를 공유할 때

---

## Claude에게 프로젝트 파악 요청하기

### 전체 구조 파악

```
> 이 프로젝트 구조 설명해줘
```

Claude가 폴더와 파일들을 분석해서 설명합니다.

```
> 이 프로젝트 뭐 하는 프로젝트야?
```

README.md와 코드를 분석해서 목적을 알려줍니다.

### 특정 파일 역할 물어보기

```
> @package.json 이 파일 설명해줘
```

```
> @src/components/ 이 폴더에 뭐가 있어?
```

### 시작 방법 물어보기

```
> 이 프로젝트 어떻게 실행해?
```

Claude가 package.json의 scripts를 확인하고 방법을 안내합니다.

### 고급 질문 예시

```
# 아키텍처 이해
> 이 프로젝트의 데이터 흐름 설명해줘

# 의존성 분석
> 이 프로젝트에서 사용하는 주요 라이브러리 목록이랑 역할 알려줘

# 진입점 찾기
> 이 앱은 어디서 시작해? 처음 실행되는 코드가 뭐야?

# 관계 파악
> UserProfile 컴포넌트가 어디서 사용되고 있어?
```

---

## 🎯 미니 퀴즈

다음 질문에 답해보세요.

### 퀴즈 1
다음 중 `node_modules` 폴더에 대한 설명으로 **틀린** 것은?

A) `npm install` 실행 시 자동 생성된다
B) 직접 수정해도 괜찮다
C) Git에 올리면 안 된다
D) 삭제해도 다시 설치 가능하다

<details>
<summary>정답 보기</summary>

**정답: B**

`node_modules`는 절대 직접 수정하면 안 됩니다.
다음 `npm install` 때 덮어쓰여지기 때문입니다.

</details>

### 퀴즈 2
`.env` 파일에 대한 설명으로 **맞는** 것은?

A) Git에 올려서 팀과 공유해야 한다
B) API 키 같은 비밀 정보를 저장한다
C) 브라우저에서 직접 접근 가능하다
D) package.json 대신 사용한다

<details>
<summary>정답 보기</summary>

**정답: B**

`.env` 파일은 비밀 정보를 저장하며,
**절대 Git에 올리면 안 됩니다**.
`.gitignore`에 반드시 포함해야 합니다.

</details>

### 퀴즈 3
다음 구조의 문제점은 무엇일까요?

```
my-project/
├── stuff/
├── things/
├── misc/
└── other/
```

<details>
<summary>정답 보기</summary>

**문제점:**
- 폴더 이름이 불명확하여 무엇이 들어있는지 알 수 없음
- 새로운 팀원이 코드를 찾기 어려움
- `components`, `utils`, `api` 같은 명확한 이름을 사용해야 함

</details>

---

## 📝 실습 과제

### 🟢 초급: 프로젝트 구조 만들기

```
> my-first-app 폴더 만들고, 그 안에 src, public, styles 폴더 만들어줘
> src 안에 components, pages, utils 폴더도 만들어줘
```

**예상 결과:** 기본 폴더 구조가 생성됨

### 🟡 중급: package.json 생성 및 분석

```
> my-first-app 폴더에서 npm 초기화해줘

> @package.json 이 파일에 scripts 섹션 추가해줘
> dev는 "echo 개발 서버 시작", build는 "echo 빌드 중"으로
```

**예상 결과:** package.json에 scripts가 추가됨

### 🔴 고급: 기존 프로젝트 분석

GitHub에서 아무 프로젝트나 클론해서:

```bash
git clone https://github.com/vercel/next.js.git
cd next.js
claude
```

```
> 이 프로젝트 분석해줘. 뭐 하는 프로젝트야?
> 폴더 구조 중에서 가장 중요한 폴더 5개 알려줘
> 이 프로젝트는 어떤 구조 패턴을 사용하고 있어?
```

---

## 🏆 도전 과제

### 도전 1: 프레임워크 비교 분석

3개의 다른 프레임워크 프로젝트를 클론하여 구조를 비교해보세요.

```bash
# React
npx create-react-app react-sample
# Next.js
npx create-next-app nextjs-sample
# Vue
npm create vue@latest vue-sample
```

각 프로젝트에서 Claude에게 물어보세요:
```
> 이 프로젝트 구조 분석해줘
> 진입점 파일이 뭐야?
> 컴포넌트는 어디에 넣어야 해?
```

공통점과 차이점을 정리해보세요.

### 도전 2: 나만의 구조 가이드 작성

자신만의 프로젝트 구조 가이드를 만들어보세요.

```
> 웹 개발 프로젝트를 위한 폴더 구조 가이드 만들어줘
> 각 폴더의 역할과 예시 파일을 포함해서
> STRUCTURE_GUIDE.md 파일로 저장해줘
```

---

## 🚨 흔한 에러와 해결법

### 에러 1: "node_modules가 없음"

**상황:** 프로젝트를 다운받았는데 실행이 안 됨

**원인:** 의존성이 설치되지 않음

**해결:**
```
> npm install 해줘
```

### 에러 2: "어떤 파일을 수정해야 할지 모르겠음"

**상황:** 뭔가 바꾸고 싶은데 어디를 건드려야 할지 모름

**해결:**
```
> 이 프로젝트에서 [바꾸고 싶은 것] 수정하려면 어떤 파일을 봐야 해?
```

예시:
```
> 메인 페이지 제목 바꾸려면 어떤 파일을 봐야 해?
> 로그인 버튼 색상을 바꾸려면 어디를 수정해야 해?
```

### 에러 3: "package.json이 없음"

**상황:** npm 명령어가 안 됨

**원인:** 프로젝트 루트가 아닌 다른 폴더에 있음

**해결:**
```
> package.json 어디 있어?
> 그 폴더로 이동해줘
```

### 에러 4: "모듈을 찾을 수 없음" 에러

**상황:** `Cannot find module 'xxx'` 에러 발생

**원인:**
- 패키지가 설치되지 않았거나
- import 경로가 잘못됨

**해결:**
```
> 이 에러 왜 나는 거야? [에러 메시지]
> 필요한 패키지 설치해줘
```

---

## ❌ 자주 하는 실수

### 1. node_modules 직접 수정하기
이 폴더는 `npm install`로 자동 생성됩니다. 절대 직접 수정하지 마세요.

> ⚠️ **왜 위험한가?**
>
> - 다음 `npm install` 때 변경사항이 사라짐
> - 팀원과 다른 환경이 됨
> - 디버깅이 매우 어려워짐

### 2. 프로젝트 루트 확인 안 하기
명령어는 프로젝트 루트(package.json이 있는 곳)에서 실행해야 합니다.
```
> 지금 어디야?
```
로 항상 확인하세요.

### 3. README 안 읽기
좋은 프로젝트는 README에 모든 것이 설명되어 있습니다. Claude에게 물어보기 전에:
```
> @README.md 읽어줘
```

### 4. 한 번에 모든 것 이해하려 하기
처음에는 필요한 부분만 파악하면 됩니다. 전체를 이해하려고 하지 마세요.

> 💡 **비전공자 팁**
>
> "필요할 때 필요한 만큼" 전략이 효과적입니다.
>
> 1. 먼저 프로젝트가 뭘 하는지 파악
> 2. 내가 수정할 부분만 집중해서 이해
> 3. 관련된 파일들만 살펴봄
> 4. 나머지는 나중에 필요할 때

### 5. .env 파일 Git에 올리기

**절대 하면 안 됩니다!**

```
# .gitignore에 반드시 추가
.env
.env.local
.env.production
```

---

## 🖼️ 예상 화면

### 프로젝트 구조 설명 요청

```
> 이 프로젝트 구조 설명해줘

이 프로젝트는 Next.js 14 기반 웹 애플리케이션입니다.

📁 주요 폴더 구조:

src/
├── app/          # Next.js App Router (페이지와 라우팅)
│   ├── page.tsx  # 홈페이지 (/)
│   ├── layout.tsx # 공통 레이아웃
│   └── api/      # API 라우트
├── components/   # 재사용 가능한 UI 컴포넌트
│   ├── Header/
│   └── Button/
└── lib/          # 유틸리티 함수

📦 주요 의존성:
- React 18.2.0
- Next.js 14.0.0
- TypeScript 5.0.0

🚀 실행 방법:
npm run dev (개발 서버)
npm run build (프로덕션 빌드)
```

### 특정 폴더 분석

```
> @src/components/ 이 폴더 분석해줘

components 폴더에는 5개의 컴포넌트가 있습니다:

1. Button/ - 다양한 스타일의 버튼 컴포넌트
   - Button.tsx: 메인 컴포넌트
   - Button.styles.ts: 스타일 정의
   - index.ts: 내보내기

2. Modal/ - 팝업 모달 컴포넌트
   - 열기/닫기 애니메이션 포함

3. Card/ - 정보 표시용 카드
   - 이미지, 제목, 설명 지원

4. Header/ - 페이지 상단 네비게이션

5. Footer/ - 페이지 하단 정보

패턴: 각 컴포넌트가 개별 폴더에 있으며,
관련 파일들(스타일, 테스트)이 함께 있습니다.
```

---

## 미니 프로젝트: 프로젝트 탐험가

아무 오픈소스 프로젝트를 분석해봅니다.

### 🟢 쉬움: 기본 분석

```
> 이 프로젝트 뭐야?
> 어떻게 실행해?
> 가장 중요한 파일 3개 알려줘
```

### 🟡 보통: 구조 파악

```
> 폴더 구조 설명해줘
> 이 프로젝트에서 메인 페이지는 어디 있어?
> 컴포넌트는 어떤 패턴으로 구성되어 있어?
> 스타일링은 어떻게 하고 있어?
```

### 🔴 도전: 심층 분석

```
> 이 프로젝트에서 사용하는 라이브러리 목록 알려줘
> 이 프로젝트의 진입점(entry point)이 어디야?
> 데이터 흐름을 설명해줘
> 이 프로젝트의 아키텍처 패턴이 뭐야?
> 개선할 수 있는 구조적 문제점이 있어?
```

---

## 🆘 안 되면?

| 증상 | 해결 방법 |
|------|----------|
| 구조가 이해 안 됨 | "비전공자한테 설명하듯이 알려줘" 추가 |
| 어떤 파일인지 모름 | Claude에게 "@파일명 이게 뭐야?" |
| 폴더가 너무 많음 | "가장 중요한 폴더 3개만 알려줘" |
| 프로젝트가 안 돌아감 | "왜 안 돌아가는지 알려줘" |
| 에러 메시지가 이해 안 됨 | 에러 메시지를 복사해서 Claude에게 물어보기 |

---

## 📚 관련 용어 사전

| 용어 | 설명 |
|------|------|
| **진입점 (Entry Point)** | 앱이 시작되는 첫 번째 파일 |
| **의존성 (Dependency)** | 프로젝트가 필요로 하는 외부 라이브러리 |
| **컴포넌트 (Component)** | 재사용 가능한 UI 조각 |
| **라우팅 (Routing)** | URL과 페이지를 연결하는 것 |
| **정적 파일 (Static Files)** | 변하지 않는 파일 (이미지, 폰트 등) |
| **환경 변수 (Environment Variables)** | 설정 값을 저장하는 변수 |
| **빌드 (Build)** | 개발 코드를 배포용으로 변환하는 것 |
| **모노레포 (Monorepo)** | 여러 프로젝트를 하나의 저장소에서 관리 |

---

## 📚 더 알아보기

**공식 문서:**
- [Next.js 문서](https://nextjs.org/docs) - Next.js 공식 가이드
- [React 문서](https://react.dev/) - React 공식 문서
- [Vue.js 가이드](https://vuejs.org/guide/introduction.html) - Vue.js 시작하기

**영상 자료:**
- [Project Structure Best Practices (YouTube)](https://www.youtube.com/results?search_query=project+structure+best+practices) - 프로젝트 구조 설계
- [React Folder Structure (YouTube)](https://www.youtube.com/results?search_query=react+folder+structure) - React 폴더 구조
- [Clean Code Architecture (YouTube)](https://www.youtube.com/results?search_query=clean+code+architecture) - 클린 아키텍처

**읽을거리:**
- [Feature-Sliced Design](https://feature-sliced.design/) - 기능 기반 구조 설계
- [Bulletproof React](https://github.com/alan2207/bulletproof-react) - React 프로젝트 구조 가이드

**템플릿:**
- [Create Next App](https://nextjs.org/docs/pages/api-reference/create-next-app) - Next.js 프로젝트 시작
- [Create React App](https://create-react-app.dev/) - React 프로젝트 시작
- [Vite](https://vitejs.dev/) - 빠른 빌드 도구

---

## ✅ 체크리스트

학습을 마치기 전에 확인하세요:

- [ ] 일반적인 프로젝트 폴더 구조를 알고 있다
- [ ] package.json의 역할과 주요 항목을 안다
- [ ] src/, public/, node_modules/ 폴더의 차이를 안다
- [ ] .gitignore와 .env 파일의 중요성을 안다
- [ ] 좋은 구조와 나쁜 구조의 차이를 안다
- [ ] Claude에게 프로젝트 구조를 물어볼 수 있다
- [ ] 다양한 프레임워크의 구조 차이를 이해한다

---

## ➡️ 다음 단계 미리보기

다음 챕터에서는 **컨텍스트와 메모리**를 활용하는 방법을 학습합니다.

배우게 될 내용:
- AI가 대화를 어떻게 기억하는지
- CLAUDE.md 파일로 프로젝트 규칙 설정하기
- 효과적인 대화 관리 전략
- 메모리 한계 극복 방법

[Chapter 07: 컨텍스트와 메모리](../Chapter07-Context-and-Memory/README.ko.md)로 진행하십시오.

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
