# Chapter 06: Slash Commands

**한국어** | [English](./README.md)

## Prerequisites

이 Chapter를 시작하기 전에:
- [ ] Chapter 00-05 (Foundation) 완료
- [ ] CLAUDE.md와 프로젝트 메모리 이해
- [ ] 마크다운 문법에 익숙함

---

## Introduction

Slash commands를 사용하면 간단한 `/command`로 호출할 수 있는 재사용 가능한 프롬프트를 만들 수 있습니다. 일반적인 작업—코드 리뷰, 디버깅, 문서 생성 등—을 위한 매크로라고 생각하세요.

### 왜 Slash Commands인가?

- **일관성**: 매번 같은 프롬프트 구조
- **속도**: 복잡한 지시 재입력 불필요
- **팀 공유**: 프로젝트 명령어는 git 추적
- **모범 사례**: 전문가 지식을 명령어로 인코딩

---

## Topics

### 1. 내장 명령어

Claude Code는 많은 내장 명령어와 함께 제공됩니다:

#### 필수 명령어

| 명령어 | 설명 |
|--------|------|
| `/help` | 모든 명령어 표시 |
| `/clear` | 대화 삭제 |
| `/compact` | 컨텍스트 압축 |
| `/model` | 모델 변경 |
| `/cost` | 사용량 표시 |
| `/config` | 설정 |
| `/doctor` | 문제 진단 |

#### 파워 유저 명령어

| 명령어 | 설명 |
|--------|------|
| `/vim` | 전체 vim 스타일 편집 활성화 (h/j/k/l, ciw, dd 등) |
| `/context` | 토큰 윈도우에서 무엇이 소비되는지 확인 |
| `/stats` | Claude Code 사용 통계 확인 |
| `/rename <name>` | 현재 세션에 이름 붙이기 |
| `/statusline` | 상태 표시줄 디스플레이 커스터마이즈 |
| `/chrome` | 웹 작업을 위한 브라우저 상호작용 활성화 |

### 2. 커스텀 명령어 생성

명령어는 `.claude/commands/` 디렉토리의 마크다운 파일입니다.

**프로젝트 명령어** (팀과 공유):
```
.claude/commands/
├── review.md
├── fix-issue.md
└── generate-tests.md
```

**개인 명령어** (본인만):
```
~/.claude/commands/
├── my-review-style.md
└── daily-standup.md
```

### 3. 명령어 파일 구조

간단한 명령어:

```markdown
<!-- .claude/commands/review.md -->
이 코드를 다음 관점에서 리뷰해주세요:
1. 잠재적 버그
2. 성능 문제
3. 보안 취약점
4. 코드 스타일 위반

구체적인 라인 참조와 수정 제안을 제공해주세요.
```

호출: `/review`

### 4. 인수 사용

동적 입력을 위해 `$ARGUMENTS` 플레이스홀더 사용:

```markdown
<!-- .claude/commands/fix-issue.md -->
GitHub 이슈 #$ARGUMENTS를 찾아서 수정해주세요

단계:
1. 이슈 설명 읽기
2. 관련 코드 위치 찾기
3. 수정 구현
4. 해당되면 테스트 작성
5. 변경 사항 요약
```

호출: `/fix-issue 123`

### 5. 다단계 명령어

복잡한 워크플로우 생성:

```markdown
<!-- .claude/commands/feature.md -->
# 기능 구현: $ARGUMENTS

## Phase 1: 조사
- 요구사항 이해
- 영향받는 파일 식별
- 유사 구현 확인

## Phase 2: 구현
- 필요한 파일 생성
- 기존 패턴 따르기
- 적절한 에러 처리 추가

## Phase 3: 품질
- 단위 테스트 작성
- 문서 업데이트
- 문제 자체 검토

## Phase 4: 완료
- 모든 변경 사항 요약
- 후속 작업 나열
```

### 6. 명령어 모범 사례

#### 구체적으로
```markdown
<!-- 나쁜 예 -->
코드 리뷰해줘

<!-- 좋은 예 -->
@src/auth/의 보안 문제를 리뷰해주세요:
- SQL 인젝션 확인
- 입력 살균 검증
- 적절한 인증 확인
- 노출된 시크릿 찾기
```

#### 컨텍스트 포함
```markdown
<!-- 프로젝트 특정 컨텍스트 포함 -->
우리 기술 스택은 React + TypeScript입니다.
@src/components/Button.tsx의 패턴을 따라주세요.
데이터 페칭에는 커스텀 `useApi` hook을 사용하세요.
```

#### 제약 조건 추가
```markdown
<!-- 명확한 경계 설정 -->
규칙:
- 기존 테스트 수정 금지
- 변경사항은 하위 호환 유지
- 최대 3개 파일만 변경
```

### 7. 명령어 구성

권장 구조:
```
.claude/commands/
├── code/
│   ├── review.md
│   ├── refactor.md
│   └── debug.md
├── docs/
│   ├── api-docs.md
│   └── readme.md
├── git/
│   ├── commit.md
│   └── pr.md
└── test/
    ├── unit.md
    └── e2e.md
```

---

## Resources

- [Claude Code 공식 문서](https://code.claude.com/docs)
- [Slash Commands Reference](https://code.claude.com/docs/en/slash-commands)
- [마크다운 가이드](https://www.markdownguide.org/)

---

## Checklist

면접에서 답변하듯이 다음 질문에 답해보세요:

1. **Slash commands란 무엇이고 왜 사용하나요?**
   <details>
   <summary>힌트</summary>
   재사용 가능한 프롬프트 템플릿, 일관성, 속도, 팀 공유
   </details>

2. **프로젝트 vs 개인 명령어는 어디에 저장되나요?**
   <details>
   <summary>힌트</summary>
   프로젝트: .claude/commands/ (git 추적). 개인: ~/.claude/commands/
   </details>

3. **명령어에 동적 인수를 어떻게 전달하나요?**
   <details>
   <summary>힌트</summary>
   명령어 파일에 $ARGUMENTS 플레이스홀더 사용
   </details>

4. **좋은 slash command의 특징은?**
   <details>
   <summary>힌트</summary>
   구체적, 컨텍스트 포함, 제약 조건 있음, 잘 조직됨
   </details>

---

## Mini Project: Command Library

### Project Goals

종합적인 slash command 라이브러리를 구축하세요:

- [ ] 보안 및 스타일 검사를 위한 `/review` 명령어 생성
- [ ] 이슈 번호로 버그를 수정하는 `/fix-bug $ARGUMENTS` 명령어 생성
- [ ] 파일에 대한 테스트를 생성하는 `/test $ARGUMENTS` 명령어 생성
- [ ] 문서를 생성하는 `/docs $ARGUMENTS` 명령어 생성
- [ ] 4개 이상의 창의적 명령어 생성 (예: `/explain`, `/optimize`, `/refactor`)
- [ ] 적절한 폴더 구조로 명령어 구성
- [ ] 각 명령어가 올바르게 작동하는지 테스트

### Ideas to Try

- 보안 중심 리뷰를 위한 `/security-audit` 명령어 생성
- 커밋에서 변경 로그를 생성하는 `/changelog` 명령어 구축
- 일일 스탠드업 요약을 생성하는 `/standup` 명령어 만들기
- 복잡한 워크플로우를 위해 연결되는 명령어 생성

---

## Advanced

### 프레임워크 특화 명령어

자주 쓰는 프레임워크용 명령어를 만들어보세요:

**React 컴포넌트 생성** (`.claude/commands/react-component.md`):
```markdown
Create a new React component named $ARGUMENTS with:
- TypeScript + functional component
- Props interface defined
- Basic unit test file
- Storybook story (if stories/ exists)
Follow patterns in @src/components/Button.tsx
```

**API 엔드포인트 생성** (`.claude/commands/api-endpoint.md`):
```markdown
Create a new API endpoint for $ARGUMENTS:
- Follow REST conventions
- Include validation with zod
- Add error handling
- Create test file
Follow patterns in @src/api/users.ts
```

### 팀 온보딩 명령어 세트

새 팀원이 바로 생산적이 될 수 있는 명령어들:

```markdown
<!-- .claude/commands/onboarding/setup.md -->
Help me set up this project:
1. Explain the project architecture
2. Show me how to run it locally
3. Point out the main files I should know
4. Explain the testing strategy

<!-- .claude/commands/onboarding/first-task.md -->
I'm new to this codebase. Help me with my first task: $ARGUMENTS
- Explain relevant code sections
- Suggest which files to modify
- Warn about common pitfalls
```

### 명령어 조합 패턴

복잡한 작업을 위해 명령어를 순차적으로 사용하세요:

```bash
# 1. 먼저 리뷰
/review

# 2. 발견된 이슈 수정
/fix-bug found in review

# 3. 테스트 추가
/test src/utils/newFeature.ts

# 4. 문서화
/docs src/utils/newFeature.ts
```
