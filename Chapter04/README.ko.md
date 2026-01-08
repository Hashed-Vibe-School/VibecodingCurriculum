# Chapter 04: Git 워크플로우

**한국어** | [English](./README.md)

## Prerequisites

이 Chapter를 시작하기 전에 다음을 할 수 있어야 합니다:
- [ ] 기본 Git 명령어 사용 (add, commit, push, pull)
- [ ] 브랜치와 머지 이해
- [ ] Claude Code로 코드 편집

---

## Introduction

Git은 현대 개발 워크플로우의 근간입니다. Claude Code는 Git과 깊이 통합되어 더 나은 커밋 메시지 작성, PR 생성, 코드 변경 리뷰를 도와줍니다. 이 Chapter에서는 간소화된 Git 워크플로우를 위해 Claude를 활용하는 방법을 배웁니다.

### 왜 Claude + Git인가?

- **더 나은 커밋**: Claude가 변경 사항을 분석하고 의미 있는 메시지 작성
- **PR 생성**: 자동화된, 잘 구조화된 풀 리퀘스트
- **코드 리뷰**: AI 지원 변경 사항 검토
- **충돌 해결**: 머지 충돌 이해 및 해결 지원

---

## Topics

### 1. Claude의 Git 통합 이해

Claude Code는 다음을 할 수 있습니다:
- git status, diff, log 읽기
- 변경 사항 스테이징 및 커밋
- 브랜치 생성 및 관리
- 풀 리퀘스트 생성 (`gh` CLI 사용)
- 명시적 허가 없이 절대 push 하지 않음

### 2. 커밋 워크플로우

#### Claude가 커밋 메시지 작성하게 하기

```bash
> Commit these changes with an appropriate message
```

Claude는:
1. `git status`로 변경 사항 확인
2. `git diff`로 무엇이 변경되었는지 파악
3. `git log`로 커밋 스타일 확인
4. 규칙을 따르는 커밋 메시지 작성

#### 커밋 메시지 형식

Claude는 기본적으로 이 형식을 따릅니다:
```
type: 간결한 설명

필요한 경우 더 긴 설명.

Co-Authored-By: Claude <noreply@anthropic.com>
```

**Types**: feat, fix, docs, style, refactor, test, chore

### 3. 풀 리퀘스트 생성

Claude에게 PR 생성 요청:

```bash
> Create a pull request for these changes
```

Claude는:
1. 브랜치가 push 되었는지 확인
2. 브랜치의 모든 커밋 분석
3. PR 제목과 설명 생성
4. `gh pr create`로 제출

**PR 형식**:
```markdown
## Summary
- 변경 사항 bullet points

## Test plan
- [ ] 변경 사항 검증 방법

🤖 Generated with Claude Code
```

### 4. Claude로 코드 리뷰

#### 자신의 변경 사항 리뷰
```bash
> Review my changes before I commit
> Are there any issues with the code I modified?
```

#### PR 리뷰
```bash
> Review PR #123 and summarize the changes
> Check PR #123 for potential bugs or issues
```

Claude는:
- 변경 사항 요약
- 잠재적 버그 식별
- 개선점 제안
- 보안 문제 확인

### 5. 브랜치 관리

```bash
> Create a new branch for the login feature
> What branches exist and what are they for?
> Merge the feature branch into main
```

### 6. 머지 충돌 처리

```bash
> I have merge conflicts in @file.ts. Help me resolve them.
> Explain what each side of this conflict represents
```

Claude는:
- 각 버전이 무엇을 하는지 설명
- 최선의 해결책 제안
- 수정 적용

### 7. Git 안전 규칙

Claude Code는 엄격한 안전 규칙을 따릅니다:

| 안전 | 위험 (명시적 허가 필요) |
|------|----------------------|
| `git status` | `git push --force` |
| `git diff` | `git reset --hard` |
| `git log` | `git push` (전부) |
| `git add` | `git rebase -i` |
| `git commit` | git config 수정 |

---

## Resources

- [Claude Code 공식 문서](https://code.claude.com/docs)
- [Git Workflow Guide](https://code.claude.com/docs/en/common-tasks#working-with-git)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [GitHub CLI 문서](https://cli.github.com/manual/)

---

## Checklist

면접에서 답변하듯이 다음 질문에 답해보세요:

1. **Claude는 커밋 메시지를 어떻게 생성하나요?**
   <details>
   <summary>힌트</summary>
   git status/diff 분석, log에서 스타일 확인, conventional commits 따르기
   </details>

2. **좋은 PR에는 어떤 정보가 포함되어야 하나요?**
   <details>
   <summary>힌트</summary>
   변경 사항 요약, 동기/컨텍스트, 테스트 계획, 관련 이슈
   </details>

3. **Claude가 자동으로 하지 않는 git 작업은?**
   <details>
   <summary>힌트</summary>
   Force push, hard reset, 원격에 push, interactive rebase
   </details>

4. **Claude가 코드 리뷰에 어떻게 도움이 되나요?**
   <details>
   <summary>힌트</summary>
   변경 사항 요약, 버그 식별, 개선점 제안, 보안 확인
   </details>

5. **머지 충돌 해결에 Claude를 어떻게 사용하나요?**
   <details>
   <summary>힌트</summary>
   각 측 설명 요청, 해결책 제안, 수정 적용
   </details>

---

## Mini Project

### Learning Goals

이 Chapter를 마스터하기 위해 다음 작업을 완료하세요:

- [ ] Claude에게 변경 사항을 커밋하도록 요청하고 커밋 메시지가 규칙을 따르는지 확인
- [ ] 기능 브랜치를 생성하고 Claude의 도움으로 여러 커밋 수행
- [ ] Claude에게 적절한 요약과 테스트 계획이 있는 풀 리퀘스트 생성 요청
- [ ] 커밋하기 전에 Claude를 사용하여 변경 사항 검토
- [ ] Claude의 도움으로 머지 충돌 해결

### Try These Prompts

```bash
> Commit these changes with an appropriate message
> Create a new branch for the login feature
> Create a pull request for these changes
> Review my changes before I commit
> I have merge conflicts in @file.ts. Help me resolve them
```

---

## Advanced

### 커밋 메시지 템플릿 설정

CLAUDE.md에 팀의 커밋 규칙을 추가하세요:

```markdown
## Commit Convention
- Format: type(scope): description
- Types: feat, fix, docs, style, refactor, test, chore
- Example: feat(auth): add OAuth2 login support
- Keep subject line under 50 characters
```

그 다음 테스트:
```bash
> Commit these changes following our commit convention
```

### GitHub CLI 연동

`gh` CLI로 이슈와 PR을 Claude와 함께 관리하세요:

```bash
# 이슈 목록 확인 후 작업
> !gh issue list
> Let's work on issue #42. Read the issue first, then create a plan.

# PR 생성 자동화
> Create a PR for this branch. Use the issue #42 description as context.
```

### 복잡한 PR 리뷰 연습

10개 이상 파일이 변경된 PR을 찾아 리뷰해보세요:

```bash
# PR 변경사항 가져오기
> !gh pr diff 123

# 체계적 리뷰 요청
> Review this PR focusing on:
> 1. Breaking changes
> 2. Security issues
> 3. Performance concerns
> Organize feedback by severity.
```
