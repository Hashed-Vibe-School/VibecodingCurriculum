# Chapter 03: 코드 편집

**한국어** | [English](./README.md)

## Prerequisites

이 Chapter를 시작하기 전에 다음을 할 수 있어야 합니다:
- [ ] Glob과 Grep을 사용한 코드베이스 탐색
- [ ] @-mention으로 파일 참조
- [ ] 권한 모드 이해 (특히 Accept Edits)

---

## Introduction

코드 편집은 Claude Code가 진정으로 빛나는 영역입니다. 하지만 많은 개발자들이 바로 "fix this bug"라고 말하며 시작합니다. 이는 Claude가 바로 코딩으로 뛰어들게 만들고, 결과물의 품질이 떨어집니다.

이 Chapter에서는 Anthropic이 공식 권장하는 워크플로우를 중심으로, 실제로 생산성을 높이는 코드 편집 방법을 배웁니다.

### 핵심 원칙

> **"Explore → Plan → Code → Commit"** - Anthropic 공식 가이드에서 가장 강조하는 워크플로우입니다. 이 단계 없이 Claude는 바로 코딩으로 뛰어들고, 결과물이 당신의 코드베이스와 맞지 않을 수 있습니다.

---

## Topics

### 1. 핵심 워크플로우: Explore → Plan → Code → Commit

Anthropic이 권장하는 4단계 워크플로우입니다. **1-2단계가 없으면 Claude는 바로 코딩으로 뛰어듭니다.**

```bash
# 1단계: Explore - 먼저 이해하기 (코드 작성 없이)
> Read @src/auth/login.ts and explain how the authentication flow works
> What error handling patterns does this codebase use?

# 2단계: Plan - 계획 세우기
> I want to add OAuth support. Create a plan for what files need to change
> and in what order. Don't write any code yet.

# 3단계: Code - 구현하기 (점진적으로)
> Let's start with step 1 of the plan: create the OAuth config file
> Now implement step 2: add the OAuth routes

# 4단계: Commit - 검증하고 커밋
> Run the tests to make sure nothing broke
> Create a commit with these changes
```

**왜 이게 중요한가?**
- Claude가 기존 코드 패턴을 먼저 이해함
- 계획을 통해 당신이 방향을 제어할 수 있음
- 점진적 구현으로 문제 발생 시 롤백이 쉬움

### 2. 반복의 힘: 2-3회면 충분하다

> **"첫 번째 버전은 괜찮지만, 2-3번 반복하면 훨씬 좋아집니다."** - Anthropic Engineering

```bash
# 1차: 기본 구현
> Create a login form component

# 2차: 피드백 반영
> Add validation and error messages to the form
> The error message should appear below each field

# 3차: 마무리
> Add loading state and disable the button while submitting
> Follow the button styling in @components/Button.tsx
```

**반복할 때 효과적인 피드백:**
- 구체적인 문제점 지적
- 참조할 기존 코드 제시
- 원하는 결과물 설명

### 3. Test-Driven Development (TDD) 접근

테스트를 먼저 작성하면 Claude에게 명확한 목표를 제공합니다.

```bash
# 1단계: 테스트 먼저 작성
> Write tests for a validateEmail function that should:
> - Return true for valid emails like "user@example.com"
> - Return false for invalid emails like "test@" or "@example.com"
> - Return false for empty strings

# 2단계: 테스트 실행 (실패 확인)
> Run the tests - they should fail since we haven't implemented yet

# 3단계: 구현
> Now implement validateEmail to pass all the tests

# 4단계: 테스트 재실행 (성공 확인)
> Run the tests again to confirm they pass
```

**TDD의 장점:**
- Claude가 정확히 무엇을 구현해야 하는지 알게 됨
- 엣지 케이스를 미리 정의
- 자동 검증 가능

### 4. 코스 수정: 잘못된 방향 바로잡기

Claude가 잘못된 방향으로 가고 있을 때:

| 단축키 | 동작 |
|--------|------|
| `Esc` | 현재 작업 중단 |
| `Esc Esc` (두 번) | 이전 상태로 되돌리고 다시 시도 |
| `Ctrl+C` | 완전히 취소 |

```bash
# Claude가 잘못된 방향으로 갈 때
> [Esc 두 번 누르기]

# 더 구체적인 지시로 다시 시작
> Let me clarify: don't create a new file. Instead, modify the existing
> @src/auth/login.ts to add the OAuth logic alongside the current login.
```

### 5. Edit vs Write 도구

| 도구 | 언제 사용 | 동작 |
|------|----------|------|
| **Edit** | 기존 파일 일부 수정 | 특정 텍스트만 교체 |
| **Write** | 새 파일 생성 | 전체 파일 작성 |

Claude가 diff를 보여주면:
```diff
- const result = data.map(item => item.value);
+ const result = data
+   .filter(item => item.isActive)
+   .map(item => item.value);
```

**수락 전 확인:**
- 예상한 변경인가?
- 의도하지 않은 부작용은 없는가?
- 기존 코드 스타일과 일치하는가?

### 6. 효과적인 편집 요청

#### 나쁜 요청 vs 좋은 요청

```bash
# 나쁜 예 - Claude가 추측해야 함
> fix the bug

# 좋은 예 - 구체적인 위치, 문제, 원하는 동작
> In @src/utils/validation.ts, the validateEmail function returns true
> for invalid emails like "test@". Fix this by checking that the domain
> part exists and contains at least one dot.
```

#### 기존 패턴 참조하기

```bash
# 좋은 예 - 따라야 할 패턴 제시
> Add a new endpoint for /api/products following the same pattern
> as @api/users.ts. Include validation middleware and error handling.
```

### 7. UI 작업: 시각적 피드백 활용

UI 작업 시 스크린샷을 공유하면 Claude가 결과를 "볼" 수 있습니다.

```bash
# 1차 구현 후 스크린샷 공유
> Here's how the form looks now: @screenshot.png
> The spacing between fields is too tight. Increase it to match
> the design in @designs/form-mockup.png

# 반복
> Better! But the error message color doesn't match our theme.
> Use the error color from @styles/theme.ts
```

### 8. 리팩토링 패턴

```bash
# 함수 추출
> Extract the validation logic in @src/components/Form.tsx (lines 45-60)
> into a separate validateForm function in @src/utils/validation.ts

# 이름 변경 (프로젝트 전체)
> Rename "getUserData" to "fetchUserProfile" across all files

# 코드 이동
> Move the helper functions from @src/components/helpers.ts
> to @src/utils/helpers.ts and update all imports

# 타입 추가
> Add TypeScript types to @src/api/client.js and rename to .ts
```

### 9. 안전장치

#### Permission 모드 활용

```bash
# 탐색 시: Plan Mode (읽기 전용)
claude --permission-mode plan

# 신뢰하는 프로젝트: Accept Edits
claude --permission-mode acceptEdits

# 기본: Default (매번 확인)
claude
```

#### Git을 활용한 안전망

```bash
# 주요 변경 전 커밋
> Let's commit the current state before the big refactor

# 문제 발생 시
> The refactor broke something. Let's revert to the last commit
> and try a different approach
```

---

## Resources

- [Claude Code 공식 문서](https://code.claude.com/docs)
- [Claude Code: Best practices for agentic coding](https://www.anthropic.com/engineering/claude-code-best-practices) - Anthropic 공식 가이드
- [Refactoring Guru](https://refactoring.guru/) - 리팩토링 패턴

---

## Checklist

면접에서 답변하듯이 다음 질문에 답해보세요:

1. **Anthropic이 권장하는 코드 편집 워크플로우는?**
   <details>
   <summary>힌트</summary>
   Explore → Plan → Code → Commit. 1-2단계 없이 Claude는 바로 코딩으로 뛰어듦.
   </details>

2. **왜 2-3번 반복이 중요한가요?**
   <details>
   <summary>힌트</summary>
   첫 버전은 괜찮지만 반복하면 훨씬 좋아짐. 피드백으로 방향 조정 가능.
   </details>

3. **TDD 접근법의 장점은?**
   <details>
   <summary>힌트</summary>
   명확한 목표 제공, 엣지 케이스 정의, 자동 검증 가능.
   </details>

4. **Claude가 잘못된 방향으로 갈 때 어떻게 하나요?**
   <details>
   <summary>힌트</summary>
   Esc 두 번으로 되돌리기, 더 구체적인 지시로 다시 시작.
   </details>

5. **좋은 편집 요청의 특징은?**
   <details>
   <summary>힌트</summary>
   구체적인 위치, 문제 설명, 원하는 동작, 참조할 패턴.
   </details>

---

## Mini Project

### Learning Goals

이 Chapter를 마스터하기 위해 다음 작업을 완료하세요:

- [ ] **Explore → Plan → Code → Commit** 워크플로우로 기능 하나 구현
- [ ] 테스트 먼저 작성 후 Claude에게 구현 요청 (TDD)
- [ ] 첫 결과물에 2-3번 피드백으로 개선
- [ ] `Esc Esc`로 잘못된 방향 수정 경험
- [ ] 기존 패턴을 @-mention으로 참조하여 새 코드 생성

### Try These Prompts

```bash
# Explore 단계
> Explain how error handling works in this project. Show me examples.

# Plan 단계
> I want to add user profile editing. Create a plan without writing code.

# TDD 접근
> Write tests for a formatCurrency function first, then implement it.

# 반복 개선
> This works, but add better error messages and loading states.

# 패턴 참조
> Create a new API endpoint following the pattern in @api/users.ts
```

---

## Advanced

- [ ] 복잡한 리팩토링을 여러 단계로 나누어 진행
- [ ] UI 컴포넌트를 스크린샷 피드백으로 반복 개선
- [ ] 대규모 코드베이스에서 안전한 일괄 수정
- [ ] Claude의 편집과 IDE 리팩토링 도구 조합 활용
