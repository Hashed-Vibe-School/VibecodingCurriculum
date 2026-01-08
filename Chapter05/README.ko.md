# Chapter 05: 프로젝트 메모리

**한국어** | [English](./README.md)

## Prerequisites

이 Chapter를 시작하기 전에 다음을 할 수 있어야 합니다:
- [ ] Claude로 코드베이스 탐색 및 탐구
- [ ] 코드 편집 및 커밋
- [ ] LLM 대화에서 컨텍스트 개념 이해

---

## Introduction

모든 프로젝트에는 부족의 지식이 있습니다—코딩 규칙, 아키텍처 결정, 팀 선호도—새 팀원(그리고 AI 어시스턴트)이 배워야 하는 것들입니다. Claude Code의 프로젝트 메모리 시스템을 사용하면 이 지식을 캡처하여 Claude가 처음부터 프로젝트를 이해할 수 있습니다.

### 왜 프로젝트 메모리가 중요한가

메모리 없이는 매 세션마다 같은 지시를 반복해야 합니다:
- "우리는 스타일링에 Tailwind CSS를 사용해"
- "항상 TypeScript strict 모드 사용해"
- "기존 파일 구조를 따라"

CLAUDE.md가 있으면 Claude가 이를 자동으로 알게 됩니다.

---

## Topics

### 1. CLAUDE.md 개요

CLAUDE.md는 Claude가 모든 세션 시작 시 읽는 특별한 마크다운 파일입니다.

**위치** (우선순위 순서):
1. `./CLAUDE.md` - 프로젝트 루트 (최고 우선순위)
2. `./.claude/CLAUDE.md` - 숨겨진 claude 폴더
3. `~/.claude/CLAUDE.md` - 사용자 전역 (최저 우선순위)

### 2. 무엇을 포함할까

#### 프로젝트 아키텍처
```markdown
# Project Overview

## Tech Stack
- Frontend: React 18 + TypeScript
- Backend: Node.js + Express
- Database: PostgreSQL with Prisma ORM
- Styling: Tailwind CSS

## Directory Structure
- `src/components/` - React 컴포넌트
- `src/pages/` - 페이지 컴포넌트 (Next.js 라우팅)
- `src/api/` - API 라우트 핸들러
- `src/lib/` - 유틸리티 함수
```

#### 코딩 규칙
```markdown
## Coding Standards

### TypeScript
- strict 모드 사용
- 객체에는 type보다 interface 선호
- 함수에는 항상 반환 타입 정의

### Naming
- 컴포넌트: PascalCase (UserProfile.tsx)
- 유틸리티: camelCase (formatDate.ts)
- 상수: UPPER_SNAKE_CASE

### File Organization
- 파일당 하나의 컴포넌트
- 소스 파일과 테스트 파일 같이 배치
- 타입이 아닌 기능별로 그룹화
```

#### 팀 선호도
```markdown
## Preferences

- 클래스 컴포넌트 말고 hooks가 있는 함수형 컴포넌트 사용
- default export보다 named export 선호
- public API에만 JSDoc 주석 작성
- 컴포넌트는 200줄 미만 유지
```

#### 금지 패턴
```markdown
## Do NOT

- TypeScript에서 `any` 타입 사용 금지
- .env 파일 커밋 금지
- 인라인 스타일 사용 금지 (Tailwind 사용)
- 논의 없이 새 의존성 추가 금지
```

### 3. 효과적인 CLAUDE.md 구조

```markdown
# Project Name

## Quick Reference
[Claude가 알아야 할 가장 중요한 것들]

## Architecture
[고수준 시스템 설계]

## Conventions
[코딩 표준 및 패턴]

## Common Tasks
[실행, 테스트, 배포 방법]

## Gotchas
[혼란스러울 수 있는 것들]
```

### 4. 팀 vs 개인 메모리

| 파일 | 범위 | 사용 사례 |
|------|------|----------|
| `./CLAUDE.md` | 프로젝트 (git 추적) | 팀 전체 표준 |
| `~/.claude/CLAUDE.md` | 개인 | 개인 선호도 |

**개인 선호도 예시**:
```markdown
# My Preferences

- 상세한 설명 선호
- 코드 주석에 항상 예시 포함
- Vim 키바인딩 사용
```

### 5. 메모리 모범 사례

> **Pro Tip**: "CLAUDE.md를 git에 공유하고, Claude가 잘못할 때마다 업데이트하세요. 이는 시간이 지남에 따라 Claude의 동작을 개선하는 피드백 루프를 만듭니다."

#### 간결하게 유지
- Claude가 매 세션마다 읽음
- 긴 파일은 토큰 낭비
- 인라인 대신 상세 문서 링크

#### 정기적으로 업데이트
- 규칙이 진화하면 새로 추가
- 오래된 정보 제거
- 분기별 검토
- **Claude가 실수하면 업데이트** - 반복되는 오류를 방지하는 규칙 추가

#### 구체적으로
```markdown
# 나쁜 예
좋은 변수 이름 사용

# 좋은 예
설명적인 변수 이름 사용:
- `userData` not `d`
- `isLoading` not `loading`
- `handleSubmit` not `submit`
```

### 6. 메모리 계층 구조

Claude는 여러 소스의 메모리를 결합합니다:

```
1. 프로젝트 CLAUDE.md (특정 규칙)
      ↓
2. 상위 디렉토리 CLAUDE.md (상속)
      ↓
3. 사용자 CLAUDE.md (개인 선호도)
```

### 7. Rules 디렉토리

`.claude/rules/`에 특정 규칙을 별도 파일로 관리할 수 있습니다:

```
.claude/rules/
├── security.md     # 보안 규칙
├── testing.md      # 테스트 규칙
└── api-design.md   # API 설계 규칙
```

**예시** (`.claude/rules/security.md`):
```markdown
# 보안 규칙

항상 확인할 것:
- 입력 살균 (sanitization)
- SQL 인젝션 방지
- XSS 취약점 확인
- 민감한 데이터 로깅 금지
```

Rules는 자동으로 로드되며 모든 대화에 적용됩니다.

### 8. 응답 언어 설정

Claude의 응답 언어를 설정 파일에서 제어할 수 있습니다:

```json
// .claude/settings.json
{
  "language": "korean"
}
```

지원 언어: `korean`, `japanese`, `english`, `chinese`, `spanish` 등

---

## Resources

- [Claude Code 공식 문서](https://code.claude.com/docs)
- [Memory & Project Context](https://code.claude.com/docs/en/memory)
- [Settings Reference](https://code.claude.com/docs/en/settings)
- [마크다운 가이드](https://www.markdownguide.org/)

---

## Checklist

면접에서 답변하듯이 다음 질문에 답해보세요:

1. **CLAUDE.md란 무엇이고 어디에 배치해야 하나요?**
   <details>
   <summary>힌트</summary>
   프로젝트 메모리 파일, 루트, .claude 폴더, 또는 홈 디렉토리에 배치 가능
   </details>

2. **어떤 유형의 정보를 포함해야 하나요?**
   <details>
   <summary>힌트</summary>
   기술 스택, 규칙, 선호도, 금지 패턴, 일반적인 작업
   </details>

3. **메모리 계층 구조는 어떻게 작동하나요?**
   <details>
   <summary>힌트</summary>
   프로젝트 > 상위 디렉토리 > 사용자 전역, 더 구체적인 것이 일반적인 것을 오버라이드
   </details>

4. **프로젝트 메모리와 개인 메모리의 차이는?**
   <details>
   <summary>힌트</summary>
   프로젝트: git 추적, 팀 전체. 개인: 개인 선호도, 공유 안 함.
   </details>

5. **CLAUDE.md에서 상세함과 간결함의 균형을 어떻게 맞추나요?**
   <details>
   <summary>힌트</summary>
   토큰 절약을 위해 간결하게, 상세 내용은 문서 링크, 정기 업데이트
   </details>

---

## Mini Project

### Learning Goals

이 Chapter를 마스터하기 위해 다음 작업을 완료하세요:

- [ ] 프로젝트 루트에 기술 스택과 디렉토리 구조가 포함된 CLAUDE.md 파일 생성
- [ ] CLAUDE.md에 최소 5개의 코딩 규칙과 3개의 "하지 마라" 규칙 추가
- [ ] 새 Claude 세션을 시작하고 Claude가 프로젝트 규칙을 이해하는지 확인
- [ ] `~/.claude/CLAUDE.md`에 개인 선호도가 포함된 개인 메모리 파일 생성
- [ ] Claude에게 기능 추가를 요청하고 CLAUDE.md의 규칙을 따르는지 확인

### Try These Prompts

```bash
> Explain this project based on the CLAUDE.md
> Add a new component following our coding conventions
> What are the "do not" rules for this project?
> Show me how to run the tests based on project documentation
```

---

## Advanced

### 모노레포용 CLAUDE.md 구조

모노레포에서는 계층적 CLAUDE.md를 활용하세요:

```
monorepo/
├── CLAUDE.md              # 공통 규칙
├── packages/
│   ├── frontend/
│   │   └── CLAUDE.md      # React 특화 규칙
│   └── backend/
│       └── CLAUDE.md      # Node.js 특화 규칙
```

루트 CLAUDE.md 예시:
```markdown
# Monorepo Rules
- All packages use TypeScript strict mode
- Run `bun test` before committing
- See package-specific CLAUDE.md for details
```

### CLAUDE.md 효과 실험

같은 작업을 CLAUDE.md 유무로 비교해보세요:

```bash
# 1. CLAUDE.md 없이 시작
mv CLAUDE.md CLAUDE.md.bak
claude
> Create a new API endpoint for /users

# 2. CLAUDE.md 복원 후 다시 시도
mv CLAUDE.md.bak CLAUDE.md
claude
> Create a new API endpoint for /users

# 차이점 비교: 코딩 스타일, 에러 핸들링, 파일 위치 등
```

### Claude가 실수할 때 CLAUDE.md 업데이트

Claude가 규칙을 어기면 바로 기록하세요:

```bash
# Claude가 var를 사용했다면
> # Never use var, always use const or let

# Claude가 any 타입을 썼다면
> # Avoid TypeScript any type, define proper interfaces
```

> **Pro Tip**: "Claude의 코드에도 사람의 코드와 같은 기준을 적용하세요." 실수가 발생할 때마다 CLAUDE.md를 업데이트하면, 같은 실수가 반복되지 않습니다.
