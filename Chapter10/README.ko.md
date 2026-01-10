# Chapter 10: 프로젝트 메모리

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- CLAUDE.md가 뭔지
- 프로젝트 정보 저장하기
- Claude가 내 프로젝트를 기억하게 하기

---

## CLAUDE.md란?

CLAUDE.md는 "Claude에게 주는 메모"입니다.

새 대화를 시작할 때마다 Claude는 이 파일을 읽습니다. 그래서 매번 같은 말을 반복할 필요가 없습니다.

### 왜 필요한가요?

CLAUDE.md 없이:
```
> 버튼 만들어줘
# → Claude가 아무 스타일로나 만듦

> 아, Tailwind CSS 써야 하는데...
> 버튼 다시 만들어줘. Tailwind로.
```

CLAUDE.md 있으면:
```
> 버튼 만들어줘
# → Claude가 CLAUDE.md를 읽고 알아서 Tailwind로 만듦
```

매번 "Tailwind 써", "TypeScript 써" 같은 말을 안 해도 됩니다.

---

## CLAUDE.md 만들기

### 어디에 만들까?

프로젝트 폴더 맨 위(루트)에 만드세요.

```
my-project/
├── CLAUDE.md       <- 여기!
├── package.json
├── src/
└── ...
```

### Claude에게 시키기

```
> 이 프로젝트용 CLAUDE.md 만들어줘
```

Claude가 프로젝트를 분석해서 알아서 만들어줍니다.

### 직접 만들기

간단한 예시:

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

---

## 뭘 써야 하나요?

### 1. 기술 스택

이 프로젝트가 뭘 쓰는지 알려주세요.

```markdown
## 기술 스택
- Frontend: React + TypeScript
- Styling: Tailwind CSS
- 패키지 매니저: npm
```

### 2. 폴더 구조

파일들이 어디 있는지 알려주세요.

```markdown
## 폴더 구조
- `src/components/` - 컴포넌트들
- `src/pages/` - 페이지들
- `src/utils/` - 유틸 함수들
```

### 3. 코딩 규칙

코드 스타일을 알려주세요.

```markdown
## 코딩 규칙
- 변수 이름은 영어로
- 주석은 한글로
- 들여쓰기는 2칸
```

### 4. 하지 말아야 할 것

금지 사항을 알려주세요.

```markdown
## 하지 마
- `any` 타입 사용 금지
- 인라인 스타일 금지
- console.log 남기지 않기
```

### 5. 자주 쓰는 명령어

```markdown
## 명령어
- 개발 서버: `npm run dev`
- 빌드: `npm run build`
- 테스트: `npm test`
```

---

## 실제 예시

### 간단한 웹사이트 프로젝트

```markdown
# 내 포트폴리오

## 소개
개인 포트폴리오 웹사이트

## 기술 스택
- HTML, CSS, JavaScript
- 스타일: Tailwind CSS

## 규칙
- 파일 이름은 소문자로 (예: about.html)
- 색상은 Tailwind 기본 색상 사용
- 모바일 반응형 필수
```

### React 프로젝트

```markdown
# Todo App

## 기술 스택
- React 18
- TypeScript
- Tailwind CSS

## 폴더 구조
- `src/components/` - 재사용 컴포넌트
- `src/hooks/` - 커스텀 훅
- `src/types/` - TypeScript 타입

## 코딩 규칙
- 컴포넌트: PascalCase (예: TodoItem.tsx)
- 훅: camelCase (예: useTodos.ts)
- 함수형 컴포넌트 + hooks 사용

## 하지 마
- class 컴포넌트 사용 금지
- any 타입 금지
- 한 컴포넌트에 200줄 넘기지 않기

## 명령어
- `npm run dev` - 개발 서버
- `npm run build` - 빌드
- `npm test` - 테스트
```

---

## 개인 메모리 vs 프로젝트 메모리

### 프로젝트 메모리 (팀용)

위치: `프로젝트폴더/CLAUDE.md`

- 팀 전체가 공유
- Git에 저장됨
- 프로젝트 규칙

### 개인 메모리 (나만의 설정)

위치: `~/.claude/CLAUDE.md` (홈 폴더)

- 나만 사용
- 모든 프로젝트에 적용
- 개인 선호도

```markdown
# 내 설정

## 선호도
- 설명은 한글로 해줘
- 코드에 주석 많이 달아줘
- 예시 코드 항상 보여줘
```

### 우선순위

```
프로젝트 CLAUDE.md > 개인 CLAUDE.md
```

프로젝트 규칙이 더 우선입니다.

---

## Claude가 규칙을 어기면?

CLAUDE.md를 업데이트하세요!

### 예시 상황

Claude가 `var`를 썼다면:

```markdown
## 하지 마
- var 사용 금지, const나 let 사용
```

Claude가 영어로 주석을 달았다면:

```markdown
## 규칙
- 주석은 무조건 한글로
```

이렇게 추가하면 다음부터는 안 그럽니다.

---

## 응답 언어 설정

Claude가 계속 영어로 대답하면, 설정 파일로 바꿀 수 있습니다.

`.claude/settings.json` 파일:

```json
{
  "language": "korean"
}
```

또는 CLAUDE.md에:

```markdown
## 언어
- 항상 한국어로 응답
```

---

## 실습해보기

### 실습 1: CLAUDE.md 만들기

```
# 새 폴더 만들고
> my-test-project 폴더 만들고 이동해줘

# CLAUDE.md 만들기
> 이 프로젝트용 CLAUDE.md 만들어줘.
> React + TypeScript 프로젝트야.
```

### 실습 2: 규칙 확인하기

```
# Claude가 규칙을 아는지 확인
> 이 프로젝트 규칙이 뭐야?

# 규칙대로 하는지 확인
> 버튼 컴포넌트 만들어줘
```

### 실습 3: 규칙 추가하기

```
> CLAUDE.md에 "테스트 파일은 .test.ts로 끝나야 함" 추가해줘
```

---

## 팁: CLAUDE.md 관리

### 짧게 유지하기

Claude가 매번 읽으니까 너무 길면 낭비입니다.

```markdown
# 나쁜 예 (너무 김)
우리 프로젝트는 2024년 1월에 시작해서
처음에는 JavaScript였다가 TypeScript로 바꿨고
그 이유는... (100줄)

# 좋은 예 (간결함)
## 기술 스택
- TypeScript
- React
```

### 정기적으로 업데이트

- 새 규칙 생기면 추가
- 안 쓰는 규칙은 삭제
- Claude가 실수하면 바로 추가

### Git에 저장

CLAUDE.md도 코드처럼 버전 관리하세요.

```
> CLAUDE.md 수정한 거 커밋해줘
```

---

## 정리

이번 챕터에서 배운 것:
- [x] CLAUDE.md가 뭔지
- [x] 어디에 만드는지
- [x] 뭘 써야 하는지
- [x] 개인 vs 프로젝트 메모리
- [x] 규칙 업데이트하기

CLAUDE.md를 잘 써두면 Claude가 내 프로젝트를 정확히 이해합니다.

축하합니다! Part 2 (핵심 기능)를 완료했습니다.

다음 Part에서는 실제 프로젝트를 만들어봅니다.

[Chapter 11: 웹사이트 개발](../Chapter11/README.ko.md)으로 넘어가세요.
