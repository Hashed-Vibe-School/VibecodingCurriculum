# Chapter 23: Hooks & Commands

[English](./README.md) | **한국어**

---

## 💬 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이 챕터에서 배우는 것

- Hooks로 자동화 트리거 만들기
- Commands로 자주 쓰는 프롬프트 저장하기
- 실용적인 자동화 예시와 베스트 프랙티스
- Hooks + Commands 조합으로 강력한 워크플로우 구축

---

## 이전 챕터와의 연결

[Chapter 22: 설정 심화](../Chapter22-Advanced-Config/README.ko.md)에서 CLAUDE.md와 settings.json으로 Claude의 기본 동작을 설정하는 방법을 배웠습니다. 이번 챕터에서는 한 단계 더 나아가 **자동화**의 세계로 들어갑니다.

설정이 "규칙"이라면, Hooks와 Commands는 "자동 실행되는 액션"입니다.

> 💡 **비전공자 팁**: Hooks는 "~하면 자동으로 ~해줘"이고, Commands는 "내가 /명령어 치면 ~해줘"입니다. 둘 다 반복 작업을 줄여주는 마법 도구예요!

---

## 왜 필요한가요?

**실제 상황**: Claude에게 파일 수정을 요청할 때마다 수동으로 `npm run lint`를 실행해서 포맷팅을 확인합니다. 매번 반복하면 지치게 됩니다.

Hooks와 Commands는 반복적인 부분을 자동화해서 창의적인 작업에 집중할 수 있게 해줍니다.

### 실제 현업에서의 활용

| 상황 | Hooks/Commands 없이 | 있으면 |
|------|---------------------|--------|
| 파일 수정 후 린트 | 매번 수동 실행 | 자동 실행 |
| 커밋 메시지 작성 | 형식 기억해서 타이핑 | `/commit` 한 번 |
| PR 리뷰 요청 | 체크리스트 복사-붙여넣기 | `/review` 한 번 |
| 테스트 작성 | 템플릿 찾아서 복사 | `/test 파일명` |

### 쉬운 비유: 스마트홈 자동화

Hooks를 스마트홈 규칙처럼 생각해보시기 바랍니다:
- **"집을 나가면"** (트리거) --> **"불 끄기"** (동작)
- **"문을 열면"** (트리거) --> **"에어컨 켜기"** (동작)

Claude Code에서:
- **"Claude가 파일을 수정하면"** (트리거) --> **"린터 실행"** (동작)
- **"엔터를 누르면"** (트리거) --> **"컨텍스트 추가"** (동작)

Commands는 음성 단축키와 같습니다: "시리야, 아침 루틴 시작해" = `/commit`

> 🔥 **프로 팁**: 하루에 10번 하는 작업을 자동화하면, 한 달에 300번의 반복을 줄일 수 있습니다!

---

## 초보자가 흔히 빠지는 함정

### 함정 1: "자동화는 고급 기능이야"

많은 분들이 "나중에 익숙해지면 해야지"라고 미룹니다. 하지만:
- 간단한 Hook 하나가 매일 5분을 절약
- Command 하나가 오타와 실수를 방지
- 처음부터 설정하면 좋은 습관이 형성됨

> ⚠️ **주의사항**: 복잡한 자동화부터 시작하지 마세요. 가장 자주 하는 단순한 작업 하나부터 시작하세요!

### 함정 2: "Hook이 너무 많아서 뭐가 뭔지 모르겠어"

Hook을 너무 많이 만들면:
- 어떤 Hook이 실행되는지 추적이 어려움
- Hook 간 충돌 가능성
- 디버깅이 복잡해짐

### 함정 3: "Command에 모든 것을 담으려고 함"

하나의 Command에 너무 많은 기능을 넣으면:
- 유연성이 떨어짐
- 수정이 어려움
- 재사용이 불가능

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

> 💡 **비전공자 팁**: `$FILE_PATH`는 Claude가 수정한 파일의 경로를 자동으로 넣어주는 "변수"예요. 마치 엑셀의 셀 참조처럼요!

---

## 왜 Hooks와 Commands를 알아야 할까?

설정만으로는 한계가 있습니다. Hooks와 Commands를 이해하면:

- **반복 제거**: 매번 같은 요청을 타이핑하지 않아도 됨
- **워크플로우 자동화**: 특정 동작 전후에 자동으로 처리
- **팀 표준화**: 팀 전체가 같은 방식으로 작업

### 자동화의 ROI (투자 대비 효과)

| 투자 시간 | 자동화 대상 | 절약 효과 |
|----------|------------|----------|
| 5분 | 파일 저장 후 린트 | 하루 20회 x 30초 = 10분/일 |
| 10분 | 커밋 메시지 작성 | 하루 5회 x 2분 = 10분/일 |
| 15분 | 테스트 템플릿 | 하루 3회 x 5분 = 15분/일 |

---

## Hooks 시스템 상세 설명

Hooks는 특정 이벤트가 발생할 때 자동으로 실행되는 코드입니다.

### Hook 타입

아래는 가장 자주 사용하는 주요 Hook의 실행 흐름입니다:

```
┌─────────────────────────────────────────────────────────────────┐
│                    주요 Hook 실행 시점                            │
└─────────────────────────────────────────────────────────────────┘

 사용자 입력
      │
      ▼
┌──────────────────┐
│ UserPromptSubmit │ ← 사용자가 엔터를 누르는 순간
└────────┬─────────┘   (입력 전처리, 컨텍스트 추가)
         │
         ▼
┌──────────────────┐
│   PreToolUse     │ ← 도구 실행 직전
└────────┬─────────┘   (검증, 백업, 로깅)
         │
         ▼
    [도구 실행]
         │
         ▼
┌──────────────────┐
│   PostToolUse    │ ← 도구 실행 직후
└────────┬─────────┘   (포맷팅, 테스트, 알림)
         │
         ▼
┌──────────────────┐
│      Stop        │ ← 응답 완료 시
└──────────────────┘   (정리, 요약, 로깅)
```

### 전체 Hook 이벤트 목록

Claude Code는 총 12가지 Hook 이벤트를 지원합니다:

| Hook 이벤트 | 설명 | Matcher 사용 | 주요 용도 |
|-------------|------|--------------|----------|
| `PreToolUse` | 도구 실행 직전 | ✅ | 백업, 검증, 차단 |
| `PostToolUse` | 도구 실행 성공 직후 | ✅ | 포맷팅, 테스트, 로깅 |
| `PostToolUseFailure` | 도구 실행 실패 시 | ✅ | 에러 로깅, 알림 |
| `PermissionRequest` | 권한 요청 다이얼로그 표시 시 | ✅ | 자동 승인, 로깅 |
| `UserPromptSubmit` | 사용자가 프롬프트 제출 시 | ❌ | 입력 변환, 컨텍스트 추가 |
| `Notification` | Claude Code가 알림을 보낼 때 | ❌ | 외부 알림 연동 |
| `Stop` | Claude Code 응답 완료 시 | ❌ | 요약, 정리, 로깅 |
| `SessionStart` | 세션 시작 또는 재개 시 | ❌ | 초기화, 환경 설정 |
| `SessionEnd` | 세션 종료 시 | ❌ | 정리, 저장, 로깅 |
| `SubagentStart` | 서브에이전트 시작 시 | ❌ | 모니터링, 로깅 |
| `SubagentStop` | 서브에이전트 완료 시 | ❌ | 결과 처리, 로깅 |
| `PreCompact` | 컨텍스트 압축 직전 | ❌ | 중요 정보 저장 |

> **참고**: Matcher는 도구 이름을 매칭하는 패턴입니다. `PreToolUse`, `PostToolUse`, `PostToolUseFailure`, `PermissionRequest`에서만 사용됩니다.

### Hook 설정 파일 상세

```json
// ~/.claude/settings.json
{
  "hooks": {
    // 도구 실행 전 Hook
    "PreToolUse": [
      {
        "matcher": "Edit",           // Edit 도구에만 적용
        "command": "cp $FILE_PATH $FILE_PATH.backup"  // 백업 생성
      },
      {
        "matcher": "Bash",           // Bash 도구에만 적용
        "command": "echo '명령어 실행: $COMMAND' >> ~/.claude/cmd-log.txt"
      }
    ],

    // 도구 실행 후 Hook
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "npx prettier --write $FILE_PATH && npx eslint --fix $FILE_PATH"
      },
      {
        "matcher": "Write",
        "command": "echo '새 파일 생성: $FILE_PATH' >> ~/.claude/file-log.txt"
      }
    ],

    // 사용자 입력 시 Hook
    "UserPromptSubmit": [
      {
        "command": "echo '$(date): 새 요청 시작' >> ~/.claude/session-log.txt"
      }
    ],

    // 응답 완료 시 Hook
    "Stop": [
      {
        "command": "echo '$(date): 응답 완료' >> ~/.claude/session-log.txt"
      }
    ]
  }
}
```

### 사용 가능한 변수

| 변수 | 설명 | 사용 가능한 Hook |
|------|------|-----------------|
| `$FILE_PATH` | 파일 경로 | Edit, Write, Read |
| `$COMMAND` | 실행된 명령어 | Bash |
| `$TOOL_NAME` | 도구 이름 | 모든 도구 관련 Hook |
| `$EXIT_CODE` | 종료 코드 | PostToolUse, PostToolUseFailure |

> 🔥 **프로 팁**: Hook 명령어에서 `$()` 구문을 사용하면 쉘 명령어의 결과를 삽입할 수 있습니다. 예: `$(date)`, `$(git branch --show-current)`

---

## Hooks 실용 예시

### 1. 파일 백업 (안전망)

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "command": "cp $FILE_PATH $FILE_PATH.backup.$(date +%Y%m%d_%H%M%S)"
      }
    ]
  }
}
```

파일 수정 전에 타임스탬프가 포함된 백업을 만듭니다.

### 실제 대화 예시: 백업이 도움이 된 경우

```
사용자: 이 함수 리팩토링해줘

Claude: calculateTotal 함수를 리팩토링하겠습니다.
[파일 수정 실행 - Hook이 자동으로 백업 생성]

사용자: 아, 원래 버전이 더 나았어. 되돌려줘

Claude: 백업 파일이 있습니다. 복원하겠습니다.
utils.ts.backup.20240115_143022 → utils.ts
복원 완료!
```

### 2. 자동 린트 및 포맷팅

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "npx prettier --write $FILE_PATH"
      },
      {
        "matcher": "Edit",
        "command": "npx eslint --fix $FILE_PATH"
      }
    ]
  }
}
```

> 💡 **비전공자 팁**: Prettier는 코드를 예쁘게 정렬해주고, ESLint는 문법 오류를 잡아줍니다. 자동으로 실행하면 항상 깔끔한 코드를 유지할 수 있어요!

### 3. 변경 로깅

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "echo '$(date): 새 파일 생성 - $FILE_PATH' | tee -a ~/.claude/log.txt"
      },
      {
        "matcher": "Edit",
        "command": "echo '$(date): 파일 수정 - $FILE_PATH' | tee -a ~/.claude/log.txt"
      }
    ]
  }
}
```

모든 파일 변경을 로그에 기록합니다.

### 4. 보안 체크

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "if echo '$COMMAND' | grep -qE 'rm -rf|DROP|DELETE FROM'; then echo 'BLOCKED: 위험한 명령어' && exit 1; fi"
      }
    ]
  }
}
```

위험한 명령어를 실행 전에 차단합니다.

### 5. 자동 테스트 실행

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "if [[ $FILE_PATH == *.test.* ]]; then npm test -- --testPathPattern=$(basename $FILE_PATH); fi"
      }
    ]
  }
}
```

테스트 파일을 수정하면 해당 테스트만 자동으로 실행합니다.

### 6. 슬랙 알림

```json
{
  "hooks": {
    "Stop": [
      {
        "command": "curl -X POST $SLACK_WEBHOOK -H 'Content-type: application/json' -d '{\"text\": \"Claude 작업 완료!\"}'"
      }
    ]
  }
}
```

긴 작업이 끝나면 슬랙으로 알림을 보냅니다.

---

## 🎯 미니 퀴즈: Hooks 이해하기

**질문**: 다음 중 `PreToolUse` Hook의 올바른 용도는?

A) 파일 수정 후 포맷팅 실행
B) 파일 수정 전 백업 생성
C) 응답 완료 후 로그 기록

<details>
<summary>정답 보기</summary>

**정답: B**

- `PreToolUse`는 도구 실행 **전**에 실행됩니다
- 백업, 검증, 차단 등에 사용
- A는 `PostToolUse`, C는 `Stop` Hook 용도

</details>

---

## Commands 시스템 상세 설명

Commands는 자주 쓰는 프롬프트를 저장해두는 기능입니다.

### Commands 폴더 구조

```
.claude/
└── commands/
    ├── commit.md       # /commit 명령
    ├── review.md       # /review 명령
    ├── test.md         # /test 명령
    ├── explain.md      # /explain 명령
    └── refactor.md     # /refactor 명령
```

> 💡 **비전공자 팁**: `.claude/commands/` 폴더에 `.md` 파일을 만들면 파일 이름이 곧 명령어가 됩니다. `review.md` → `/review`

### 간단한 Command 예시

```markdown
<!-- .claude/commands/commit.md -->
현재 변경사항을 분석하고 의미 있는 커밋 메시지를 작성해주세요.

## 단계
1. git diff로 변경사항 확인
2. 변경 유형 파악 (feat, fix, refactor, docs, test 등)
3. 한글로 커밋 메시지 작성
4. git commit 실행

## 커밋 메시지 형식
type: 간결한 설명

- 상세 변경 내용 1
- 상세 변경 내용 2
```

사용법:
```
> /commit
```

### 변수가 있는 Command

```markdown
<!-- .claude/commands/explain.md -->
$ARGUMENTS 코드를 분석해주세요.

## 분석 항목
1. 이 코드가 하는 일 (한 문장 요약)
2. 주요 함수/클래스 설명
3. 데이터 흐름
4. 개선할 수 있는 점

## 출력 형식
### 요약
[한 문장 설명]

### 상세 분석
[함수별 설명]

### 개선 제안
[있다면 제안]
```

사용법:
```
> /explain src/auth/login.ts
```

`$ARGUMENTS`에 `src/auth/login.ts`가 들어갑니다.

### 동적 정보 포함

```markdown
<!-- .claude/commands/status.md -->
프로젝트 현재 상태를 보여주세요.

## 현재 정보
- 브랜치: $(git branch --show-current)
- 변경된 파일: $(git status --short)
- 최근 커밋: $(git log -1 --oneline)

## 요청사항
위 정보를 바탕으로:
1. 현재 작업 상태 요약
2. 완료하지 않은 작업 식별
3. 다음 할 일 제안
```

`$()` 안의 명령어가 실행되어 결과가 삽입됩니다.

### 실제 대화 예시: Command 사용

```
사용자: /status

Claude: 프로젝트 현재 상태를 분석하겠습니다.

## 현재 상태
- 브랜치: feature/user-auth
- 변경된 파일: 3개 (M src/auth/login.ts, A src/auth/signup.ts, M tests/auth.test.ts)
- 최근 커밋: abc1234 feat: 로그인 폼 UI 추가

## 분석
현재 사용자 인증 기능을 개발 중입니다.
로그인과 회원가입 기능이 진행 중이며,
테스트 파일도 함께 수정되고 있습니다.

## 다음 할 일 제안
1. 현재 변경사항에 대한 테스트 완료
2. 코드 리뷰를 위한 PR 생성
3. 회원가입 유효성 검사 추가
```

---

## Commands 실용 예시

### 1. 코드 리뷰 요청

```markdown
<!-- .claude/commands/review.md -->
$ARGUMENTS 파일을 코드 리뷰해주세요.

## 리뷰 체크리스트
- [ ] 버그 가능성
- [ ] 보안 취약점
- [ ] 성능 이슈
- [ ] 코드 스타일 (팀 컨벤션)
- [ ] 테스트 커버리지
- [ ] 에러 처리

## 출력 형식

### 요약
[전체적인 코드 품질 평가]

### 발견된 이슈
| 심각도 | 위치 | 내용 | 제안 |
|--------|------|------|------|
| ... | ... | ... | ... |

### 잘된 점
[칭찬할 부분]

### 개선 제안
[리팩토링 아이디어]
```

```
> /review src/api/users.ts
```

### 2. 테스트 작성

```markdown
<!-- .claude/commands/test.md -->
$ARGUMENTS에 대한 테스트를 작성해주세요.

## 테스트 요구사항
- Jest 사용
- 단위 테스트 우선
- 엣지 케이스 포함 (null, undefined, 빈 값, 경계값)
- 테스트 파일: *.test.ts

## 테스트 구조
```typescript
describe('함수/클래스명', () => {
  describe('메서드명', () => {
    it('정상 케이스 설명', () => {});
    it('에러 케이스 설명', () => {});
    it('엣지 케이스 설명', () => {});
  });
});
```

## 모킹 규칙
- 외부 API: MSW 사용
- 데이터베이스: in-memory mock
- 시간: jest.useFakeTimers()
```

```
> /test src/utils/validation.ts
```

### 3. 문서화

```markdown
<!-- .claude/commands/docs.md -->
$ARGUMENTS에 대한 문서를 작성해주세요.

## 문서 포함 내용
- 함수/클래스 설명
- 파라미터 설명 (타입, 기본값, 필수 여부)
- 반환값
- 예외 상황
- 사용 예시 (최소 2개)

## 형식
JSDoc 또는 TSDoc 형식으로 코드에 직접 추가

## 예시
/**
 * 사용자 정보를 조회합니다.
 *
 * @param userId - 조회할 사용자의 ID
 * @returns 사용자 정보 객체
 * @throws {NotFoundError} 사용자를 찾을 수 없을 때
 *
 * @example
 * const user = await getUser('user123');
 * console.log(user.name);
 */
```

```
> /docs src/services/auth.ts
```

### 4. 리팩토링

```markdown
<!-- .claude/commands/refactor.md -->
$ARGUMENTS를 리팩토링해주세요.

## 리팩토링 원칙
- 동작은 그대로 유지
- 가독성 개선
- 중복 제거
- 함수 분리 (20줄 이하)
- 변수명 명확하게

## 진행 순서
1. 현재 코드 분석 및 문제점 파악
2. 리팩토링 계획 제시
3. 사용자 승인 후 진행
4. 각 단계에서 테스트 통과 확인

## 주의사항
- 한 번에 너무 많이 바꾸지 않기
- 기능 추가는 리팩토링이 아님
- 변경 이유 설명하기
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

## 2. 작업 계획
이슈 내용을 분석하고 구현 계획을 세워주세요.
- 예상 작업 시간
- 수정할 파일 목록
- 테스트 계획

## 3. 브랜치 생성
feature/$ARGUMENTS 브랜치를 만들어주세요.

## 4. 구현
계획이 승인되면 구현해주세요.

## 5. 테스트
관련 테스트를 실행하고 통과하는지 확인해주세요.

## 6. PR 생성
변경사항을 PR로 만들어주세요.
이슈와 연결: Closes #$ARGUMENTS
```

```
> /ticket 42
```

한 번의 명령으로 이슈 확인 → 브랜치 생성 → 구현 → PR까지 진행됩니다.

### 6. 버그 디버깅

```markdown
<!-- .claude/commands/debug.md -->
다음 에러를 디버깅해주세요:

$ARGUMENTS

## 디버깅 단계
1. 에러 메시지 분석
2. 스택 트레이스 확인
3. 관련 코드 찾기
4. 원인 파악
5. 해결 방법 제시
6. 재발 방지 방안

## 출력 형식
### 에러 요약
[무슨 에러인지]

### 원인
[왜 발생했는지]

### 해결 방법
[어떻게 고치는지 - 코드 포함]

### 예방
[앞으로 어떻게 방지하는지]
```

```
> /debug "TypeError: Cannot read property 'map' of undefined at UserList.tsx:15"
```

---

## 프로젝트별 Commands

### 프론트엔드 프로젝트

```
.claude/
└── commands/
    ├── component.md   # 컴포넌트 생성
    ├── hook.md        # 커스텀 훅 생성
    ├── story.md       # Storybook 스토리 생성
    ├── style.md       # 스타일 추가
    └── page.md        # 페이지 생성
```

```markdown
<!-- .claude/commands/component.md -->
$ARGUMENTS 이름의 React 컴포넌트를 만들어주세요.

## 규칙
- 함수형 컴포넌트 (화살표 함수)
- TypeScript strict mode
- Tailwind CSS
- props 타입은 interface로 정의

## 파일 구조
src/components/$ARGUMENTS/
├── $ARGUMENTS.tsx        # 컴포넌트
├── $ARGUMENTS.test.tsx   # 테스트
├── $ARGUMENTS.stories.tsx # Storybook
└── index.ts              # export

## 컴포넌트 템플릿
```tsx
interface ${ARGUMENTS}Props {
  // props 정의
}

export const $ARGUMENTS = ({ }: ${ARGUMENTS}Props) => {
  return (
    <div>
      {/* 내용 */}
    </div>
  );
};
```
```

### 백엔드 프로젝트

```
.claude/
└── commands/
    ├── endpoint.md    # API 엔드포인트 생성
    ├── migration.md   # DB 마이그레이션
    ├── seed.md        # 시드 데이터
    ├── validate.md    # 입력 검증 추가
    └── middleware.md  # 미들웨어 생성
```

```markdown
<!-- .claude/commands/endpoint.md -->
$ARGUMENTS 리소스에 대한 CRUD API를 만들어주세요.

## 엔드포인트 구조
- GET /$ARGUMENTS - 목록 조회 (페이지네이션)
- GET /$ARGUMENTS/:id - 상세 조회
- POST /$ARGUMENTS - 생성
- PATCH /$ARGUMENTS/:id - 수정
- DELETE /$ARGUMENTS/:id - 삭제

## 파일 생성
- src/controllers/$ARGUMENTS.controller.ts
- src/services/$ARGUMENTS.service.ts
- src/repositories/$ARGUMENTS.repository.ts
- src/dto/$ARGUMENTS.dto.ts
- src/entities/$ARGUMENTS.entity.ts

## 요구사항
- 입력 검증 (zod)
- 에러 처리
- 페이지네이션
- Prisma 모델도 함께 생성
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

## 단계
1. git diff로 변경 확인
2. 테스트 실행 (Hook에서 자동 실행됨)
3. 커밋 메시지 작성 (Conventional Commits)
4. 커밋 실행

## 커밋 메시지 형식
type(scope): 설명

- type: feat, fix, docs, style, refactor, test, chore
- scope: 선택사항, 변경 범위
- 설명: 현재형, 한글로
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

## 진행 방식
1. 기능 분석 및 설계
2. 필요한 파일 생성 (Hook에서 자동 포맷팅)
3. 테스트 작성
4. 문서 업데이트

## 자동화
파일 생성 후 Hook에서 자동으로:
- Prettier 포매팅
- ESLint 수정
```

### 실제 대화 예시: 조합 활용

```
사용자: /feature user-profile

Claude: user-profile 기능을 만들겠습니다.

## 설계
- 사용자 프로필 조회/수정 기능
- 프로필 이미지 업로드
- 비밀번호 변경

## 생성할 파일
1. src/features/user-profile/
   ├── UserProfile.tsx
   ├── EditProfile.tsx
   ├── ChangePassword.tsx
   └── index.ts

생성을 시작하겠습니다...

[파일 생성됨: UserProfile.tsx]
[Hook: prettier 실행 완료]
[Hook: eslint 실행 완료]

[파일 생성됨: EditProfile.tsx]
[Hook: prettier 실행 완료]
[Hook: eslint 실행 완료]

모든 파일이 생성되고 자동으로 포맷팅되었습니다.
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
│       ├── deploy.md
│       └── hotfix.md
├── CLAUDE.md
└── src/
```

`.claude/commands/`를 git에 커밋하면 팀 전체가 같은 Commands를 사용할 수 있습니다.

### README에 문서화

```markdown
# 팀 Commands

## 사용 가능한 명령어

| 명령어 | 설명 | 예시 |
|--------|------|------|
| `/commit` | 변경사항 커밋 | `/commit` |
| `/review <file>` | 코드 리뷰 | `/review src/api.ts` |
| `/test <file>` | 테스트 작성 | `/test src/utils.ts` |
| `/deploy` | 스테이징 배포 | `/deploy` |
| `/hotfix <issue>` | 긴급 수정 | `/hotfix 123` |

## 사용 방법
Claude Code에서 위 명령어를 입력하면 됩니다.

## 커스텀 명령어 추가
1. `.claude/commands/` 폴더에 `.md` 파일 생성
2. 파일 내용에 프롬프트 작성
3. Git에 커밋
```

---

## 📝 실습 과제

### 실습 1: 간단한 Hook 만들기 (초급)

Claude가 Bash 명령을 실행할 때마다 로그를 남기는 hook을 만드시기 바랍니다:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Bash",
        "command": "echo '$(date): Bash 명령 실행됨' >> ~/.claude/bash-log.txt"
      }
    ]
  }
}
```

**테스트**: Claude에게 `ls` 명령을 실행해달라고 요청 후 로그 파일 확인

### 실습 2: 첫 Command 만들기 (초급)

1. commands 폴더 만들기: `mkdir -p .claude/commands`
2. `.claude/commands/hello.md` 파일 만들기:

```markdown
인사하고 내가 어떤 프로젝트에서 작업하는지 알려줘.
폴더 구조를 보고 간단히 요약해줘.
```

3. 사용하기: `> /hello`

### 실습 3: 변수가 있는 Command (중급)

`.claude/commands/explain.md` 만들기:

```markdown
$ARGUMENTS 파일을 쉽게 설명해 주십시오.

## 설명 항목
1. 이 파일의 목적
2. 주요 함수/클래스
3. 다른 파일과의 관계
4. 개선할 수 있는 점
```

사용하기: `> /explain src/index.ts`

### 실습 4: Hook + Command 조합 (고급)

1. Hook: 파일 수정 시 자동 백업
2. Command: 백업 목록 보기
3. Command: 특정 백업으로 복원

---

## 🏆 도전 과제

### 도전 1: 완전한 개발 워크플로우

다음을 모두 자동화하는 시스템을 만들어보세요:
- 이슈 시작 (`/issue-start`)
- 개발 중 백업 (Hook)
- 테스트 자동 실행 (Hook)
- PR 생성 (`/pr`)

### 도전 2: 팀 온보딩 Command

새 팀원이 `/onboarding` 명령어로 프로젝트의 모든 것을 배울 수 있게 만들어보세요.

### 도전 3: 알림 시스템

긴 작업 완료 시 슬랙/디스코드로 알림을 보내는 Hook을 구현해보세요.

---

## 문제가 발생하면?

### 문제: Hook이 실행되지 않습니다

**가능한 원인:**
1. 잘못된 matcher 이름 (대소문자 구분)
2. JSON 문법 에러
3. 잘못된 섹션에 hook 추가

**해결 방법:**
- matcher 이름 확인: `Edit`, `Write`, `Bash`, `Read` 등 (대소문자 정확히)
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
   - `PreToolUse`: 도구 실행 전 (차단, 백업용)
   - `PostToolUse`: 도구 실행 후 (포맷팅, 테스트용)
   - 이것을 혼동하면 예상치 못한 동작이 발생합니다

2. **특수 문자 이스케이프 잊기**
   ```json
   // 나쁨 - 이스케이프 안 된 따옴표
   "command": "echo "안녕""

   // 좋음 - 작은따옴표 사용
   "command": "echo '안녕'"

   // 좋음 - 이스케이프
   "command": "echo \"안녕\""
   ```

3. **멈추는 명령어**
   - hook 명령이 입력을 기다리면 Claude Code가 멈춤
   - 항상 비대화형 명령 사용 (`-y`, `--yes` 플래그)

4. **Hook을 너무 복잡하게 만들기**
   - 간단하게 시작하고 점차 복잡성 추가
   - 하나의 hook이 하나의 일을 하면 디버깅이 쉬움

5. **명령을 먼저 수동으로 테스트하지 않기**
   - 항상 터미널에서 먼저 명령 실행
   - 거기서 안 되면 hook에서도 안 됨

---

## 용어 사전

| 용어 | 설명 |
|------|------|
| **Hook** | 특정 이벤트 발생 시 자동 실행되는 코드 |
| **Command** | 저장된 프롬프트를 `/명령어`로 실행 |
| **Matcher** | Hook이 반응할 도구를 지정하는 패턴 |
| **$ARGUMENTS** | Command에 전달되는 인자를 받는 변수 |
| **$FILE_PATH** | 수정된 파일의 경로를 담는 변수 |
| **PreToolUse** | 도구 실행 전 Hook |
| **PostToolUse** | 도구 실행 후 Hook |

---

## 정리

이번 챕터에서 배운 것:
- [x] Hooks 시스템 (Pre/Post ToolUse, UserPromptSubmit, Stop)
- [x] Commands로 프롬프트 재사용
- [x] 변수와 동적 정보 활용
- [x] Hooks + Commands 조합
- [x] 팀에서 Commands 공유

**핵심 포인트**: 반복되는 작업은 Hooks와 Commands로 자동화하시기 바랍니다.

---

## 다음 챕터 미리보기

Chapter 24에서는 **Agents와 Skills**를 배웁니다:
- Agents: Claude에게 전문가 역할 부여
- Skills: 키워드 기반 자동 워크플로우
- 코드 리뷰어, 테스터, 문서 작성자 등 전문 도우미 만들기

Hooks와 Commands가 "무엇을"에 집중했다면, Agents와 Skills는 "누가, 어떻게"에 집중합니다!

[Chapter 24: Agents & Skills](../Chapter24-Agents-and-Skills/README.ko.md)로 넘어가시기 바랍니다.

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [Claude Code Hooks 가이드](https://code.claude.com/docs/ko/hooks) - Hooks 설정 및 사용법
- [Claude Code Slash Commands](https://code.claude.com/docs/ko/slash-commands) - 명령어 가이드
- [Claude Code 자동화](https://code.claude.com/docs/ko/common-workflows) - 자동화 튜토리얼

**영상 자료:**
- [Git Hooks Tutorial (YouTube)](https://www.youtube.com/results?search_query=git+hooks+tutorial) - Git Hooks 기초 (유사 개념)
- [Automation Best Practices (YouTube)](https://www.youtube.com/results?search_query=developer+automation+best+practices) - 개발자 자동화 베스트 프랙티스
- [Shell Scripting Basics (YouTube)](https://www.youtube.com/results?search_query=shell+scripting+beginners) - 쉘 스크립트 기초

**읽을거리:**
- [Bash Scripting Guide](https://www.gnu.org/software/bash/manual/) - Bash 공식 매뉴얼
- [Pre-commit Hooks](https://pre-commit.com/) - 커밋 전 자동화 프레임워크
- [Husky](https://typicode.github.io/husky/) - Git Hooks 관리 도구

**관련 도구:**
- [Prettier](https://prettier.io/) - 코드 포맷터
- [ESLint](https://eslint.org/) - JavaScript 린터
- [lint-staged](https://github.com/okonet/lint-staged) - 스테이징된 파일만 린트

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
