# Chapter 02: 코드 탐색

**한국어** | [English](./README.md)

## Prerequisites

이 Chapter를 시작하기 전에 다음을 할 수 있어야 합니다:
- [ ] @-mention으로 파일 참조하기
- [ ] 멀티턴 대화 효과적으로 유지하기
- [ ] 권한 모드 간의 차이 이해하기

---

## Introduction

Claude Code의 가장 큰 강점 중 하나는 코드베이스를 탐색하고 이해하는 능력입니다. 이 Chapter에서는 Claude의 탐색 도구를 사용하여 익숙하지 않은 프로젝트를 빠르게 탐색하고, 특정 코드 패턴을 찾고, 복잡한 아키텍처를 이해하는 방법을 배웁니다.

### 왜 코드 탐색이 중요한가

- **온보딩**: 새 프로젝트를 빠르게 이해
- **디버깅**: 문제의 원인 위치 파악
- **리팩토링**: 변경 전 관련 코드 모두 찾기
- **학습**: 숙련된 개발자가 코드를 구조화하는 방법 연구

---

## Topics

### 1. 핵심 탐색 도구

Claude Code는 코드 탐색을 위해 세 가지 주요 도구를 사용합니다:

| 도구 | 목적 | 사용 예시 |
|------|------|----------|
| **Glob** | 패턴으로 파일 찾기 | `*.tsx`, `src/**/*.test.js` |
| **Grep** | 파일 내용 검색 | 함수의 모든 사용처 찾기 |
| **Read** | 파일 내용 보기 | 특정 코드 검토 |

### 2. Glob으로 파일 탐색

Glob은 특정 패턴과 일치하는 파일을 찾는 데 도움이 됩니다:

```bash
# Claude에게 파일 찾기 요청
> Find all TypeScript files in the src directory
> Show me all test files in this project
> List all configuration files (*.config.*, *.json, *.yaml)
```

**일반적인 Glob 패턴**:
| 패턴 | 일치하는 파일 |
|------|-------------|
| `*.js` | 현재 디렉토리의 모든 JS 파일 |
| `**/*.ts` | 재귀적으로 모든 TS 파일 |
| `src/**/*.test.tsx` | src 내 테스트 파일 |
| `!node_modules/**` | node_modules 제외 |

### 3. Grep으로 내용 검색

Grep은 파일 내부에서 특정 패턴을 검색합니다:

```bash
# Claude에게 검색 요청
> Find all files that import "useState"
> Where is the "handleSubmit" function defined?
> Search for TODO comments in the codebase
> Find all API endpoints (look for router.get, router.post, etc.)
```

**Grep 팁**:
- 복잡한 패턴에는 정규식 사용
- 파일 타입 필터와 결합
- 함수 정의 vs 사용 검색 구분

### 4. 전략적 탐색 패턴

#### 프로젝트 구조 이해
```bash
> What's the overall architecture of this project?
> Explain the folder structure and what each directory contains
> What frameworks and libraries does this project use?
```

#### 데이터 흐름 추적
```bash
> How does user data flow from the login form to the database?
> Trace the request path for /api/users endpoint
> Where is the state for user authentication managed?
```

#### 의존성 찾기
```bash
> What files import the UserService class?
> Which components use the useAuth hook?
> What would be affected if I change the User type definition?
```

### 5. 탐색 워크플로우

코드를 이해하기 위한 체계적인 접근법:

```
1. 넓게 시작
   > What does this project do?
   > What's the tech stack?

2. 구조 이해
   > Show me the folder structure
   > What are the main entry points?

3. 핵심 파일 식별
   > What are the most important files?
   > Where is the main business logic?

4. 깊이 들어가기
   > Explain how @src/services/auth.ts works
   > Walk me through the @src/components/Dashboard.tsx component
```

### 6. 안전한 탐색을 위한 Plan Mode

변경 없이 탐색만 하고 싶을 때 Plan Mode를 사용하세요:

```bash
# plan mode로 시작
claude --permission-mode plan

# 또는 세션 중 전환
> Switch to plan mode  # 또는 Shift+Tab 두 번
```

Plan Mode에서:
- Claude는 모든 파일을 읽을 수 있음
- 쓰기 작업은 불가
- 학습과 감사에 완벽

---

## Resources

- [Claude Code 공식 문서](https://code.claude.com/docs)
- [Code Exploration Guide](https://code.claude.com/docs/en/common-tasks#exploring-code)
- [Glob 패턴 문법](https://en.wikipedia.org/wiki/Glob_(programming))
- [정규 표현식 가이드](https://regexlearn.com/)

---

## Checklist

면접에서 답변하듯이 다음 질문에 답해보세요:

1. **Glob과 Grep은 언제 사용하나요?**
   <details>
   <summary>힌트</summary>
   Glob: 이름/경로 패턴으로 파일 찾기. Grep: 파일 내용 검색.
   </details>

2. **특정 함수를 사용하는 모든 파일을 어떻게 찾나요?**
   <details>
   <summary>힌트</summary>
   함수 이름을 Grep으로 검색, import 문과 사용처를 필터링
   </details>

3. **새로운 코드베이스를 탐색하는 권장 접근법은?**
   <details>
   <summary>힌트</summary>
   넓게 시작(아키텍처), 구조 이해, 핵심 파일 식별, 그 다음 깊이 들어가기
   </details>

4. **코드 탐색에 Plan Mode가 유용한 이유는?**
   <details>
   <summary>힌트</summary>
   읽기 전용 접근, 실수로 수정 방지, 감사에 안전
   </details>

5. **애플리케이션을 통한 데이터 흐름을 어떻게 추적하나요?**
   <details>
   <summary>힌트</summary>
   진입점에서 시작, 함수 호출 따라가기, 상태 변화 추적, 경계 식별
   </details>

---

## Mini Project

### Learning Goals

이 Chapter를 마스터하기 위해 다음 작업을 완료하세요:

- [ ] 익숙하지 않은 오픈소스 프로젝트를 클론하고 Claude에게 아키텍처 설명 요청
- [ ] Glob을 사용하여 패턴으로 파일 찾기 (예: 모든 테스트 파일, 모든 설정 파일)
- [ ] Grep을 사용하여 파일 내용에서 특정 패턴 검색
- [ ] 후속 질문을 통해 애플리케이션의 데이터 흐름 추적
- [ ] Plan Mode를 사용하여 변경 없이 안전하게 탐색

### Try These Prompts

```bash
> What does this project do and what's the tech stack?
> Show me the folder structure and explain each directory
> Find all files that import "useState"
> Where is the main entry point of this application?
> Trace the request path for /api/users endpoint
```

---

## Advanced

### 실전 Grep 패턴

자주 사용하는 검색 패턴을 연습하세요:

```bash
# TODO/FIXME 찾기
> Find all TODO and FIXME comments in the codebase

# 특정 함수 호출 찾기
> Find all places where "fetchUser" is called

# 미사용 import 찾기
> Find files that import "lodash" but might not use it

# 에러 핸들링 패턴 찾기
> Show me all try-catch blocks in the api/ directory
```

### 낯선 코드베이스 탐색 챌린지

GitHub에서 처음 보는 프로젝트를 골라 다음을 시도하세요:

```bash
# 1. 5분 안에 프로젝트 파악하기
> I just cloned this repo. Give me a 2-minute overview of what it does,
> the tech stack, and the main entry points.

# 2. 특정 기능 추적하기
> How does user authentication work in this project?
> Walk me through the code path from login to session creation.

# 3. 수정 포인트 찾기
> If I wanted to add a new API endpoint, which files would I need to modify?
```

**추천 프로젝트**: Express.js, Fastify, 또는 관심 있는 중소규모 오픈소스

### 대규모 코드베이스 전략

큰 프로젝트에서는 컨텍스트 관리가 중요합니다:

```bash
# 전체를 한 번에 읽지 말고, 필요한 부분만
> Don't read all files. First, show me the directory structure,
> then I'll tell you which parts to explore.

# 탐색 범위 제한
> Focus only on the src/auth/ directory for now
```
