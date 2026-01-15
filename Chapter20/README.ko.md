# Chapter 20: Hooks & Commands

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- Hooks로 자동화 트리거 만들기
- Commands로 자주 쓰는 프롬프트 저장
- 실용적인 자동화 예시

---

## 왜 필요한가요?

**실제 상황**: Claude에게 파일 수정을 요청할 때마다 수동으로 `npm run lint`를 실행해서 포맷팅을 확인합니다. 매번 반복하면 지치게 됩니다.

Hooks와 Commands는 반복적인 부분을 자동화해서 창의적인 작업에 집중할 수 있게 해줍니다.

### 쉬운 비유: 스마트홈 자동화

Hooks를 스마트홈 규칙처럼 생각해보시기 바랍니다:
- **"집을 나가면"** (트리거) --> **"불 끄기"** (동작)
- **"문을 열면"** (트리거) --> **"에어컨 켜기"** (동작)

Claude Code에서:
- **"Claude가 파일을 수정하면"** (트리거) --> **"린터 실행"** (동작)
- **"엔터를 누르면"** (트리거) --> **"컨텍스트 추가"** (동작)

Commands는 음성 단축키와 같습니다: "시리야, 아침 루틴 시작해" = `/commit`

---

## 첫 번째 Hook (여기서 시작하십시오)

모든 옵션을 알아보기 전에, 작동하는 hook을 하나 만들어봅시다:

### 단계 1: settings.json에 추가

`~/.claude/settings.json`에 다음을 추가하시기 바랍니다:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "echo '파일 수정됨!' >> ~/.claude/hook-log.txt"
      }
    ]
  }
}
```

### 단계 2: 테스트

Claude에게 아무 파일이나 수정해달라고 요청하시기 바랍니다:

```
> 이 파일에 주석 추가해줘: test.js
```

### 단계 3: 로그 확인

```bash
cat ~/.claude/hook-log.txt
```

"파일 수정됨!"이 보여야 합니다 - hook이 작동한 것입니다.

### 단계 4: 유용하게 만들기

이제 echo를 실용적인 것으로 바꿔봅시다:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "npx prettier --write $FILE_PATH"
      }
    ]
  }
}
```

이제 수정된 모든 파일이 자동으로 포맷팅됩니다!

---

## 왜 Hooks와 Commands를 알아야 할까?

설정만으로는 한계가 있습니다. Hooks와 Commands를 이해하면:

- **반복 제거**: 매번 같은 요청을 타이핑하지 않아도 됨
- **워크플로우 자동화**: 특정 동작 전후에 자동으로 처리
- **팀 표준화**: 팀 전체가 같은 방식으로 작업

---

## Hooks 시스템

Hooks는 특정 이벤트가 발생할 때 자동으로 실행되는 코드입니다.

### Hook 타입

```
┌─────────────────────────────────────────────────────────────────┐
│                        Hook 실행 시점                            │
└─────────────────────────────────────────────────────────────────┘

 사용자 입력
      │
      ▼
┌──────────────────┐
│ UserPromptSubmit │ ← 사용자가 엔터를 누르는 순간
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│   PreToolUse     │ ← 도구 실행 직전
└────────┬─────────┘
         │
         ▼
    [도구 실행]
         │
         ▼
┌──────────────────┐
│   PostToolUse    │ ← 도구 실행 직후
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│      Stop        │ ← 응답 완료 시
└──────────────────┘
```

### Hook 설정 파일

```json
// ~/.claude/settings.json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "command": "echo '파일 수정 시작: $FILE_PATH'"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Bash",
        "command": "echo '명령어 실행 완료'"
      }
    ]
  }
}
```

### 이것을 알면 무엇이 좋을까요?

**자동 린트 체크:**

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "npm run lint -- --fix $FILE_PATH"
      }
    ]
  }
}
```

파일을 수정할 때마다 자동으로 린트가 실행됩니다.

**자동 테스트:**

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "npm test -- --related $FILE_PATH"
      }
    ]
  }
}
```

수정한 파일과 관련된 테스트만 자동으로 실행됩니다.

---

## Hooks 실용 예시

### 1. 파일 백업

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "command": "cp $FILE_PATH $FILE_PATH.backup"
      }
    ]
  }
}
```

파일 수정 전에 자동으로 백업을 만듭니다.

### 2. 변경 알림

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "echo '새 파일 생성됨: $FILE_PATH' | tee -a ~/.claude/log.txt"
      }
    ]
  }
}
```

파일 생성을 로그에 기록합니다.

### 3. 보안 체크

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "if echo '$COMMAND' | grep -q 'rm -rf'; then exit 1; fi"
      }
    ]
  }
}
```

위험한 명령어를 실행 전에 차단합니다.

### 4. 포매팅 자동화

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "npx prettier --write $FILE_PATH"
      }
    ]
  }
}
```

파일 수정 후 자동으로 포매팅합니다.

---

## Commands 시스템

Commands는 자주 쓰는 프롬프트를 저장해두는 기능입니다.

### Commands 폴더 구조

```
.claude/
└── commands/
    ├── commit.md       # /commit 명령
    ├── review.md       # /review 명령
    └── test.md         # /test 명령
```

### 간단한 Command 예시

```markdown
<!-- .claude/commands/commit.md -->
현재 변경사항을 분석하고 의미 있는 커밋 메시지를 작성해주세요.

- git diff로 변경사항 확인
- 변경 유형 파악 (feat, fix, refactor 등)
- 한글로 커밋 메시지 작성
- git commit 실행
```

사용법:
```
> /commit
```

### 변수가 있는 Command

```markdown
<!-- .claude/commands/explain.md -->
$ARGUMENTS 코드를 분석해주세요.

1. 이 코드가 하는 일
2. 중요한 로직
3. 개선할 점
```

사용법:
```
> /explain src/auth/login.ts
```

### 동적 정보 포함

```markdown
<!-- .claude/commands/status.md -->
프로젝트 현재 상태를 보여주세요.

현재 브랜치: $(git branch --show-current)
변경된 파일: $(git status --short)

위 정보를 바탕으로:
1. 현재 작업 상태 요약
2. 다음 할 일 제안
```

`$()` 안의 명령어가 실행되어 결과가 삽입됩니다.

---

## Commands 실용 예시

### 1. 코드 리뷰 요청

```markdown
<!-- .claude/commands/review.md -->
$ARGUMENTS 파일을 코드 리뷰해주세요.

체크할 항목:
- [ ] 버그 가능성
- [ ] 보안 취약점
- [ ] 성능 이슈
- [ ] 코드 스타일

개선 제안이 있으면 구체적으로 알려주세요.
```

```
> /review src/api/users.ts
```

### 2. 테스트 작성

```markdown
<!-- .claude/commands/test.md -->
$ARGUMENTS에 대한 테스트를 작성해주세요.

요구사항:
- Jest 사용
- 단위 테스트 우선
- 엣지 케이스 포함
- 테스트 파일: *.test.ts
```

```
> /test src/utils/validation.ts
```

### 3. 문서화

```markdown
<!-- .claude/commands/docs.md -->
$ARGUMENTS에 대한 문서를 작성해주세요.

포함할 내용:
- 함수/클래스 설명
- 파라미터 설명
- 반환값
- 사용 예시

JSDoc 형식으로 코드에 추가해주세요.
```

```
> /docs src/services/auth.ts
```

### 4. 리팩토링

```markdown
<!-- .claude/commands/refactor.md -->
$ARGUMENTS를 리팩토링해주세요.

원칙:
- 가독성 개선
- 중복 제거
- 함수 분리 (20줄 이하)
- 변수명 명확하게

변경 전에 먼저 계획을 보여주세요.
```

```
> /refactor src/components/Dashboard.tsx
```

### 5. 이슈 → 구현 워크플로우

실무에서 자주 쓰는 패턴입니다. 이슈를 받아서 구현까지 한 번에:

```markdown
<!-- .claude/commands/ticket.md -->
이슈 #$ARGUMENTS를 처리해주세요.

## 1. 이슈 확인
$(gh issue view $ARGUMENTS)

## 2. 브랜치 생성
feature/$ARGUMENTS 브랜치를 만들어주세요.

## 3. 구현 계획
이슈 내용을 분석하고 구현 계획을 세워주세요.

## 4. 구현
계획대로 구현해주세요.

## 5. PR 생성
변경사항을 PR로 만들어주세요.
```

```
> /ticket 42
```

한 번의 명령으로 이슈 확인 → 브랜치 생성 → 구현 → PR까지 진행됩니다.

---

## 프로젝트별 Commands

### 프론트엔드 프로젝트

```
.claude/
└── commands/
    ├── component.md   # 컴포넌트 생성
    ├── hook.md        # 커스텀 훅 생성
    ├── story.md       # Storybook 스토리 생성
    └── style.md       # 스타일 추가
```

```markdown
<!-- .claude/commands/component.md -->
$ARGUMENTS 이름의 React 컴포넌트를 만들어주세요.

규칙:
- 함수형 컴포넌트
- TypeScript
- Tailwind CSS
- props 타입 정의

파일: src/components/$ARGUMENTS/$ARGUMENTS.tsx
```

### 백엔드 프로젝트

```
.claude/
└── commands/
    ├── endpoint.md    # API 엔드포인트 생성
    ├── migration.md   # DB 마이그레이션
    ├── seed.md        # 시드 데이터
    └── validate.md    # 입력 검증 추가
```

```markdown
<!-- .claude/commands/endpoint.md -->
$ARGUMENTS 리소스에 대한 CRUD API를 만들어주세요.

구조:
- GET /$ARGUMENTS - 목록
- GET /$ARGUMENTS/:id - 상세
- POST /$ARGUMENTS - 생성
- PATCH /$ARGUMENTS/:id - 수정
- DELETE /$ARGUMENTS/:id - 삭제

Prisma 모델도 함께 생성해주세요.
```

---

## Hooks + Commands 조합

### 커밋 워크플로우 자동화

```json
// settings.json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "if echo '$COMMAND' | grep -q 'git commit'; then npm test; fi"
      }
    ]
  }
}
```

```markdown
<!-- .claude/commands/commit.md -->
변경사항을 커밋해주세요.

1. git diff로 변경 확인
2. 테스트 실행 (Hook에서 자동 실행됨)
3. 커밋 메시지 작성
4. 커밋 실행
```

이렇게 하면:
1. `/commit` 실행
2. 커밋 전에 Hook이 테스트 자동 실행
3. 테스트 통과하면 커밋 진행

### 파일 생성 워크플로우

```json
// settings.json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "npx prettier --write $FILE_PATH && npx eslint --fix $FILE_PATH"
      }
    ]
  }
}
```

```markdown
<!-- .claude/commands/feature.md -->
$ARGUMENTS 기능을 만들어주세요.

파일 생성 후 Hook에서 자동으로:
- Prettier 포매팅
- ESLint 수정

이 자동화를 믿고 코드를 작성하시기 바랍니다.
```

---

## 팀에서 Commands 공유

### Git에 커밋하기

```
my-project/
├── .claude/
│   └── commands/     # 팀 공통 Commands
│       ├── commit.md
│       ├── review.md
│       └── deploy.md
├── CLAUDE.md
└── src/
```

`.claude/commands/`를 git에 커밋하면 팀 전체가 같은 Commands를 사용할 수 있습니다.

### README에 문서화

```markdown
# 팀 Commands

## 사용 가능한 명령어

- `/commit` - 변경사항 커밋
- `/review <file>` - 코드 리뷰
- `/deploy` - 스테이징 배포
- `/hotfix <issue>` - 긴급 수정
```

---

## 따라해보십시오

### 실습 1: 간단한 Hook 만들기

Claude가 Bash 명령을 실행할 때마다 로그를 남기는 hook을 만드시기 바랍니다:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Bash",
        "command": "echo 'Bash 명령 실행됨' >> ~/.claude/bash-log.txt"
      }
    ]
  }
}
```

### 실습 2: 첫 Command 만들기

1. commands 폴더 만들기: `mkdir -p .claude/commands`
2. `.claude/commands/hello.md` 파일 만들기:

```markdown
인사하고 내가 어떤 프로젝트에서 작업하는지 알려줘.
폴더 구조를 보고 간단히 요약해줘.
```

3. 사용하기: `> /hello`

### 실습 3: 변수가 있는 Command

`.claude/commands/explain.md` 만들기:

```markdown
$ARGUMENTS 파일을 쉽게 설명해 주십시오.
무슨 일을 하는지, 핵심 함수는 무엇인지 알려주십시오.
```

사용하기: `> /explain src/index.ts`

---

## 문제가 발생하면?

### 문제: Hook이 실행되지 않습니다

**가능한 원인:**
1. 잘못된 matcher 이름 (대소문자 구분)
2. JSON 문법 에러
3. 잘못된 섹션에 hook 추가

**해결 방법:**
- matcher 이름 확인: `Edit`, `Write`, `Bash`, `Read` 등
- JSON 검증: `cat ~/.claude/settings.json | jq .`
- `"hooks": { }` 섹션 안에 있는지 확인

### 문제: Hook 명령이 조용히 실패합니다

**가능한 원인:**
1. 명령어를 찾을 수 없음
2. 권한 거부
3. 잘못된 파일 경로

**해결 방법:**
- 터미널에서 먼저 수동으로 명령 테스트
- 명령이 있는지 확인: `which npm`, `which npx`
- 가능하면 절대 경로 사용

### 문제: Command를 찾을 수 없습니다

**가능한 원인:**
1. `.claude/commands/` 폴더에 파일이 없음
2. 파일 확장자가 `.md`가 아님
3. 폴더가 잘못된 위치에 있음

**해결 방법:**
- 폴더 존재 확인: `ls -la .claude/commands/`
- 파일이 `.md`로 끝나는지 확인
- commands 폴더는 프로젝트 루트나 `~/.claude/`에 있어야 함

### 문제: $ARGUMENTS가 작동하지 않습니다

**가능한 원인:**
1. 잘못된 변수 이름 사용
2. 호출할 때 인자를 제공하지 않음

**해결 방법:**
- 정확히 `$ARGUMENTS` 사용 (대소문자 구분)
- 인자 제공: `/explain`이 아니라 `/explain src/file.ts`

---

## 자주 하는 실수

1. **Hook 타이밍 혼동**
   - `PreToolUse`: 도구 실행 전 (차단 가능)
   - `PostToolUse`: 도구 실행 후 (후속 작업용)
   - 이것을 혼동하면 예상치 못한 동작이 발생합니다

2. **특수 문자 이스케이프 잊기**
   ```json
   // 나쁨 - 이스케이프 안 된 따옴표
   "command": "echo "안녕""

   // 좋음 - 작은따옴표 사용
   "command": "echo '안녕'"
   ```

3. **멈추는 명령어**
   - hook 명령이 입력을 기다리면 Claude Code가 멈춤
   - 항상 비대화형 명령 사용

4. **Hook을 너무 복잡하게 만들기**
   - 간단하게 시작하고 점차 복잡성 추가
   - 하나의 hook이 하나의 일을 하면 디버깅이 쉬움

5. **명령을 먼저 수동으로 테스트하지 않기**
   - 항상 터미널에서 먼저 명령 실행
   - 거기서 안 되면 hook에서도 안 됨

---

## 정리

이번 챕터에서 배운 것:
- [x] Hooks 시스템 (Pre/Post ToolUse, UserPromptSubmit, Stop)
- [x] Commands로 프롬프트 재사용
- [x] 변수와 동적 정보 활용
- [x] Hooks + Commands 조합
- [x] 팀에서 Commands 공유

**핵심 포인트**: 반복되는 작업은 Hooks와 Commands로 자동화하시기 바랍니다.

다음 챕터에서는 Agents와 Skills로 더 강력한 확장을 배웁니다.

[Chapter 21: Agents & Skills](../Chapter21/README.ko.md)로 넘어가시기 바랍니다.
