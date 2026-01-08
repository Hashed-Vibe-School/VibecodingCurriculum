# Chapter 01: 대화와 컨텍스트

**한국어** | [English](./README.md)

## Prerequisites

이 Chapter를 시작하기 전에 다음을 할 수 있어야 합니다:
- [ ] Claude Code 시작 및 종료
- [ ] 세 가지 권한 모드 이해
- [ ] 터미널에서 디렉토리 탐색

---

## Introduction

Claude Code와의 효과적인 소통은 단순히 질문하는 것이 아닙니다—올바른 컨텍스트를 제공하는 것입니다. 이 Chapter는 최적의 결과를 위해 프롬프트를 구성하고, 파일을 참조하고, 대화 컨텍스트를 유지하는 방법에 집중합니다.

### 왜 컨텍스트가 중요한가

Claude Code는 다음을 이해할 때만 효과적으로 도울 수 있습니다:
- **무엇**을 달성하려고 하는지
- **어디**에 관련 코드가 있는지
- **왜** 변경이 필요한지
- **어떻게** 기존 시스템이 작동하는지

---

## Topics

### 1. 효과적인 프롬프팅 기법

#### 구체적으로 말하기
```bash
# 나쁜 예
> fix the bug

# 좋은 예
> Fix the authentication bug in src/auth.ts where users can't log in
> after password reset. The error message is "Invalid token"
```

#### 컨텍스트 제공하기
```bash
# 나쁜 예
> add a button

# 좋은 예
> Add a "Save Draft" button to the BlogEditor component that saves
> the current content to localStorage. Follow the existing button
> styling in the codebase.
```

#### 복잡한 작업 분해하기
```bash
# 하나의 거대한 요청 대신 분해:
> First, let's understand the current authentication flow.
> Can you explain how login works in this project?

# 그 다음 후속 질문:
> Now let's add OAuth support. What files would need to change?
```

### 2. 특수 접두사

Claude Code에는 강력한 접두사 단축키가 있습니다:

| 접두사 | 설명 | 예시 |
|--------|------|------|
| `@` | 파일/폴더 참조 | `@src/auth.ts` |
| `!` | bash 명령 실행 및 출력 주입 | `!git status` |
| `#` | Claude 메모리에 저장 | `# Always use bun` |

```bash
# bash 실행 및 출력 주입 (토큰 낭비 없음)
> !bun test
# Claude가 테스트 결과를 즉시 확인

# 규칙을 메모리에 저장
> # Use TypeScript strict mode in this project
# Claude가 저장 위치를 묻는 프롬프트 표시
```

### 3. @-mention을 사용한 파일 참조

`@`를 사용하여 프롬프트에서 파일을 직접 참조:

```bash
> Look at @src/components/Button.tsx and create a similar
> component for form inputs

> Compare @package.json with @package-lock.json and check
> for version mismatches

> The bug is somewhere between @api/routes.ts and @api/handlers.ts
```

**자동완성**: `@`를 입력하고 파일명을 타이핑하면 Claude Code가 자동완성합니다.

### 4. 멀티턴 대화

Claude Code는 세션 내에서 대화 컨텍스트를 유지합니다:

```bash
> What does the UserService class do?
# Claude가 UserService 설명

> How does it connect to the database?
# Claude가 이전 컨텍스트를 사용해 DB 연결 설명

> Add a method to get users by email
# Claude가 여전히 UserService에 대해 이야기하고 있음을 앎
```

#### 컨텍스트 관리

| 명령어 | 설명 |
|--------|------|
| `/clear` | 대화 기록 삭제 |
| `/compact` | 컨텍스트 요약 및 압축 |
| `/history` | 대화 기록 보기 |
| `Ctrl+L` | 터미널 지우기 (컨텍스트 유지) |

### 5. 대화 패턴

#### 탐색 패턴
```bash
> What is the architecture of this project?
> How does data flow from frontend to backend?
> Where are API endpoints defined?
```

#### 디버깅 패턴
```bash
> I'm getting this error: [에러 붙여넣기]
> The error occurs when I try to [동작]
> Here's the relevant code: @path/to/file.ts
```

#### 구현 패턴
```bash
> I want to add [기능]
> Here are the requirements: [목록]
> Please follow the patterns in @existing/similar/code.ts
```

### 6. 고급 프롬프팅 기능

#### Ultrathink - 사고 깊이 제어

응답하기 전에 Claude가 얼마나 깊이 생각할지 제어하는 키워드:

| 키워드 | 사고 예산 | 사용 사례 |
|--------|----------|----------|
| `think` | 4k 토큰 | 일반 작업 |
| `think hard` | 10k 토큰 | 복잡한 문제 |
| `ultrathink` | 32k 토큰 | 아키텍처 결정 |

```bash
> ultrathink about the best architecture for a real-time chat system
```

#### 프롬프트 스태싱

`Ctrl+S`를 눌러 현재 프롬프트 초안을 스태시하고, 다른 것을 보내고, 나중에 자동 복원—프롬프트를 위한 git stash 같은 기능.

#### 프롬프트 제안

Claude가 작업을 완료한 후 회색으로 표시된 후속 제안이 나타납니다:
- `Tab`을 눌러 편집/수락
- `Enter`를 눌러 즉시 실행

### 7. Slash Commands 개요

| 명령어 | 설명 |
|--------|------|
| `/help` | 모든 명령어 목록 |
| `/model` | 현재 모델 변경 또는 보기 |
| `/cost` | 토큰 사용량 및 비용 표시 |
| `/config` | 설정 보기/수정 |
| `/bug` | 버그 신고 |
| `/doctor` | 일반적인 문제 진단 |
| `/context` | 토큰 윈도우에서 무엇이 소비되고 있는지 보기 |
| `/plan` | Plan 모드 진입 |
| `/vim` | Vim 편집 모드 활성화 |

### 8. Vim 편집 모드

`/vim` 명령어로 터미널에서 Vim 스타일 편집을 활성화할 수 있습니다:

**모드 전환**:
- `Esc` → Normal 모드
- `i`, `a`, `o` → Insert 모드

**Navigation (Normal 모드)**:
| 키 | 동작 |
|----|------|
| `h`/`j`/`k`/`l` | 좌/하/상/우 이동 |
| `w`/`b` | 다음/이전 단어 |
| `0`/`$` | 줄 처음/끝 |

**편집 (Normal 모드)**:
| 키 | 동작 |
|----|------|
| `x` | 문자 삭제 |
| `dd` | 줄 삭제 |
| `yy` | 줄 복사 |
| `p` | 붙여넣기 |
| `>>`, `<<` | 들여쓰기/내어쓰기 |

---

## Resources

- [Claude Code 공식 문서](https://code.claude.com/docs)
- [Interactive Mode 문서](https://code.claude.com/docs/en/interactive-mode)
- [Slash Commands 레퍼런스](https://code.claude.com/docs/en/slash-commands)
- [프롬프트 엔지니어링 가이드](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)

---

## Checklist

면접에서 답변하듯이 다음 질문에 답해보세요:

1. **Claude Code에서 "효과적인" 프롬프트란 무엇인가요?**
   <details>
   <summary>힌트</summary>
   구체성, 컨텍스트, 명확한 목표, 복잡성 분해
   </details>

2. **@-mention은 어떻게 작동하고 왜 유용한가요?**
   <details>
   <summary>힌트</summary>
   직접 파일 참조, 자동완성, 코드 붙여넣기 없이 컨텍스트 제공
   </details>

3. **`/clear`와 `/compact`는 언제 사용해야 하나요?**
   <details>
   <summary>힌트</summary>
   /clear: 새로 시작. /compact: 컨텍스트 유지하면서 토큰 사용량 줄이기
   </details>

4. **대화 컨텍스트가 Claude의 응답에 어떤 영향을 미치나요?**
   <details>
   <summary>힌트</summary>
   멀티턴 이해, 이전 답변 기반으로 구축, 컨텍스트 윈도우 제한
   </details>

5. **Claude에게 버그 수정을 요청할 때 어떤 정보를 제공해야 하나요?**
   <details>
   <summary>힌트</summary>
   에러 메시지, 재현 단계, 관련 파일(@-mention), 예상 동작 vs 실제 동작
   </details>

---

## Mini Project

### Learning Goals

이 Chapter를 마스터하기 위해 다음 작업을 완료하세요:

- [ ] Claude에게 프로젝트 구조 설명을 요청하고 답변을 기반으로 3-4개의 후속 질문하기
- [ ] 프롬프트에서 @-mention을 사용하여 특정 파일 참조하기
- [ ] 디버깅 패턴 연습: 버그를 도입하고 좋은 컨텍스트 단서와 함께 Claude에게 찾도록 요청
- [ ] 구현 패턴 연습: 넓게 시작하여 좁혀가며 기능 요청
- [ ] `/clear` 또는 `/compact`를 사용하여 대화 컨텍스트 관리

### Try These Prompts

```bash
> What is the architecture of this project?
> Look at @src/components/Button.tsx and explain how it works
> I'm getting this error: [paste error]. The error occurs when I try to [action]
> Add a new utility function following the pattern in @src/utils/helpers.ts
```

---

## Advanced

### 컨텍스트 관리 심화

긴 대화에서 컨텍스트를 효율적으로 관리하는 방법을 연습하세요:

```bash
# 현재 컨텍스트 상태 확인
/context

# 대화가 길어지면 압축 시도
/compact

# 압축 전후 토큰 사용량 비교
/cost
```

### 프롬프트 스타일 실험

같은 작업을 다른 방식으로 요청해보고 결과를 비교하세요:

```bash
# 스타일 1: 간단한 요청
> Add validation to the form

# 스타일 2: 구체적인 요청
> Add email validation to the signup form in @components/SignupForm.tsx
> Show error message below the field when invalid

# 스타일 3: 예시 기반 요청
> Add validation like the one in @components/LoginForm.tsx to SignupForm
```

**실험 포인트**: 어떤 스타일이 가장 정확한 결과를 내는지 기록해보세요.

### @-mention vs 코드 붙여넣기

```bash
# 방법 1: @-mention (권장)
> Fix the bug in @src/utils/parser.ts line 45

# 방법 2: 코드 직접 붙여넣기
> Fix this code:
> function parse(input) { ... }

# 언제 무엇을 사용?
# - @-mention: 파일 전체 컨텍스트가 필요할 때
# - 붙여넣기: 특정 스니펫만 보여주고 싶을 때
```
