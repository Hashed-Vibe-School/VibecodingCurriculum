# Chapter 18: 고급 기능

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- 슬래시 명령어 활용
- Hooks로 자동화
- Skills 사용하기

---

## 슬래시 명령어 마스터

지금까지 몇 가지 슬래시 명령어를 봤습니다. 이제 전부 알아봅시다.

### 기본 명령어

| 명령어 | 기능 |
|--------|------|
| `/help` | 도움말 보기 |
| `/clear` | 대화 내용 지우기 |
| `/compact` | 대화 내용 요약 (토큰 절약) |

### 상태 명령어

| 명령어 | 기능 |
|--------|------|
| `/status` | 현재 상태 확인 |
| `/cost` | 비용 확인 |

### 설정 명령어

| 명령어 | 기능 |
|--------|------|
| `/config` | 설정 열기 |
| `/model` | 모델 변경 |
| `/permissions` | 권한 설정 |

### 메모리 명령어

| 명령어 | 기능 |
|--------|------|
| `/memory` | 메모리 관리 |
| `/init` | 프로젝트 설정 초기화 |

---

## 자주 쓰는 슬래시 명령어

### /compact - 대화 압축

대화가 길어지면 토큰을 많이 씁니다. `/compact`로 압축하세요.

```
> /compact
```

Claude가 지금까지의 대화를 요약해서 저장합니다.
컨텍스트는 유지되면서 토큰은 절약됩니다.

### /clear - 새로 시작

```
> /clear
```

완전히 새로운 대화를 시작합니다.
이전 내용은 모두 잊어버립니다.

### /config - 설정 변경

```
> /config
```

Claude Code 설정을 변경할 수 있습니다:
- 모델 선택
- 권한 설정
- 테마 변경

---

## Hooks: 자동화의 시작

Hooks는 특정 이벤트가 발생할 때 자동으로 실행되는 스크립트입니다.

### Hooks란?

예를 들어:
- 파일 저장할 때 → 자동으로 포맷팅
- 커밋할 때 → 자동으로 테스트
- Claude가 코드 수정할 때 → 자동으로 린트

### Hooks 설정하기

`.claude/settings.json`에서 설정:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "command": "echo 'Claude가 파일을 수정하려고 합니다'"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "npm run lint --fix"
      }
    ]
  }
}
```

### 자주 쓰는 Hooks

#### 파일 수정 후 자동 포맷팅

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "prettier --write \"${file}\""
      }
    ]
  }
}
```

#### 커밋 전 테스트 실행

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash(git commit*)",
        "command": "npm test"
      }
    ]
  }
}
```

### Hooks 종류

| Hook | 실행 시점 |
|------|-----------|
| `PreToolUse` | 도구 사용 전 |
| `PostToolUse` | 도구 사용 후 |
| `Notification` | 알림 발생 시 |
| `Stop` | Claude가 멈출 때 |

---

## Skills: 확장 기능

Skills는 Claude Code에 새로운 기능을 추가하는 방법입니다.

### 기본 Skills 사용하기

Claude Code에는 기본 Skills가 포함되어 있습니다:

```
> /commit
```
자동으로 변경사항을 분석하고 커밋 메시지를 작성해줍니다.

```
> /review-pr
```
PR을 리뷰해줍니다.

### 커스텀 Skills 만들기

`.claude/commands/` 폴더에 마크다운 파일을 추가:

```markdown
# .claude/commands/deploy.md

프로젝트를 배포합니다.

## 단계
1. 빌드 실행
2. 테스트 확인
3. Vercel에 배포
4. 결과 확인

## 명령어
- npm run build
- npm test
- vercel --prod
```

이제 `/deploy` 명령어를 쓸 수 있습니다.

### Skills 예시

#### 코드 리뷰 요청

```markdown
# .claude/commands/review.md

이 파일의 코드를 리뷰해주세요.

## 확인할 점
- 버그 가능성
- 성능 이슈
- 보안 취약점
- 가독성

## 출력 형식
각 이슈를 다음 형태로:
- 위치: 줄 번호
- 문제: 설명
- 제안: 개선 방법
```

#### 테스트 생성

```markdown
# .claude/commands/gen-tests.md

현재 파일에 대한 테스트 파일을 생성합니다.

## 규칙
- 파일명: 원본파일.test.ts
- 모든 함수에 대한 테스트
- 엣지 케이스 포함
- Jest 사용
```

---

## 서브에이전트 (Subagents)

복잡한 작업을 할 때 Claude는 여러 "서브에이전트"를 사용합니다.

### 서브에이전트란?

메인 Claude가 특정 작업을 전문 에이전트에게 위임하는 것입니다:

- **탐색 에이전트**: 코드베이스 분석
- **계획 에이전트**: 작업 계획 수립
- **실행 에이전트**: 코드 작성

### 서브에이전트 활용

큰 작업을 요청하면 자동으로 서브에이전트가 동작합니다:

```
> 이 프로젝트 전체 분석하고
> 성능 개선 포인트 찾아서
> 리팩토링 계획 세워줘
```

Claude가:
1. 탐색 에이전트로 프로젝트 분석
2. 계획 에이전트로 개선 계획 수립
3. 결과를 종합해서 보여줌

---

## 고급 설정

### settings.json 전체 예시

```json
{
  "model": "claude-sonnet-4-20250514",
  "language": "korean",
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "denyByDefault": false
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "prettier --write \"${file}\""
      }
    ]
  },
  "customInstructions": "항상 한국어로 응답하고, 코드에 한글 주석을 추가해주세요."
}
```

### 권한 자동 승인

자주 쓰는 안전한 도구는 자동 승인:

```json
{
  "permissions": {
    "autoApprove": [
      "Read",
      "Glob",
      "Grep",
      "WebFetch"
    ]
  }
}
```

### 커스텀 지시사항

```json
{
  "customInstructions": "코드 작성 시 항상 TypeScript strict mode를 사용하고, 모든 함수에 JSDoc 주석을 추가해주세요."
}
```

---

## 실습: 나만의 워크플로우

### 기본 과제

```
# 1. 커스텀 Skill 만들기
> .claude/commands/ 폴더에 나만의 skill 추가해봐

# 2. Hook 설정하기
> 파일 수정 후 자동 포맷팅 Hook 설정해봐

# 3. 권한 최적화
> 자주 쓰는 도구는 자동 승인되게 설정해봐
```

### 추가 도전

```
> 배포 자동화 Skill 만들어줘
> 테스트 + 빌드 + 배포 한 번에

> 일일 리포트 생성 Hook 만들어줘
> 하루 작업 끝날 때 자동으로 요약
```

---

## 정리

이번 챕터에서 배운 것:
- [x] 슬래시 명령어 마스터
- [x] Hooks로 자동화
- [x] Skills 만들고 사용하기
- [x] 서브에이전트 이해
- [x] 고급 설정

다음 챕터에서는 더 강력한 자동화 도구인 MCP와 CI/CD를 배웁니다.

[Chapter 19: 자동화](../Chapter19/README.ko.md)로 넘어가세요.
