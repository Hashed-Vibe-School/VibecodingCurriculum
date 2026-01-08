# Chapter 07: Hooks

**한국어** | [English](./README.md)

## Prerequisites

이 Chapter를 시작하기 전에:
- [ ] Chapter 00-06 완료
- [ ] JSON 설정 파일 이해
- [ ] 쉘 스크립팅 기초에 익숙함

---

## Introduction

Hooks를 사용하면 Claude Code 이벤트에 대응하여 커스텀 쉘 명령을 자동으로 실행할 수 있습니다. Git hooks와 비슷하지만, AI 어시스턴트를 위한 것입니다—검증, 포매팅, 로깅, 커스텀 워크플로우를 가능하게 합니다.

### 왜 Hooks인가?

- **자동화**: 편집 후 코드 자동 포맷
- **검증**: 변경 사항 확인
- **통합**: 외부 도구 및 서비스 연결
- **규정 준수**: 규칙 자동 적용

---

## Topics

### 1. Hook 이벤트

Claude Code는 다음 hook 이벤트를 지원합니다:

| 이벤트 | 발생 시점 | Matcher 지원 |
|--------|----------|-------------|
| `PreToolUse` | 도구 실행 전 (차단 가능) | Yes |
| `PostToolUse` | 도구 완료 후 | Yes |
| `PermissionRequest` | 권한 요청 시 | Yes |
| `Notification` | 알림 발송 시 | Yes (선택) |
| `UserPromptSubmit` | 사용자 입력 처리 전 | No |
| `Stop` | 메인 에이전트 완료 시 | No |
| `SubagentStop` | 서브에이전트 완료 시 | No |
| `PreCompact` | 컨텍스트 압축 전 | Yes |
| `SessionStart` | 세션 시작 시 | Yes |
| `SessionEnd` | 세션 종료 시 | No |

### 2. Hook 설정

Hooks는 설정 파일에서 구성합니다:

**설정 파일 위치**:
- `~/.claude/settings.json` - 사용자 설정
- `.claude/settings.json` - 프로젝트 설정
- `.claude/settings.local.json` - 로컬 프로젝트 설정 (커밋 안 함)

**프로젝트 hooks** (`.claude/settings.json`):
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/format.sh",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

**사용자 hooks** (`~/.claude/settings.json`):
```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Session started at $(date)' >> ~/.claude/session.log"
          }
        ]
      }
    ]
  }
}
```

### 3. Hook 구조

```json
{
  "hooks": {
    "EventName": [
      {
        "matcher": "ToolName 또는 정규식",
        "hooks": [
          {
            "type": "command",
            "command": "쉘-명령어",
            "timeout": 60
          }
        ]
      }
    ]
  }
}
```

### 4. 사용 가능한 환경 변수

Hooks는 다음 환경 변수에 접근할 수 있습니다:

| 변수 | 설명 |
|------|------|
| `CLAUDE_PROJECT_DIR` | 프로젝트 루트 절대 경로 |
| `CLAUDE_CODE_REMOTE` | 웹이면 "true", CLI면 빈 값 |
| `CLAUDE_ENV_FILE` | 환경 변수 저장 파일 경로 (SessionStart만) |

### 5. Hook 입력 (stdin)

모든 hooks는 stdin으로 JSON을 받습니다:

```json
{
  "session_id": "abc123",
  "transcript_path": "/path/to/transcript.jsonl",
  "cwd": "/current/working/directory",
  "permission_mode": "default",
  "hook_event_name": "PreToolUse",
  "tool_name": "Edit",
  "tool_input": { ... }
}
```

### 6. Matcher 패턴

Matcher는 hooks가 언제 실행되는지 제어합니다 (대소문자 구분):

| 패턴 | 일치 |
|------|------|
| `Edit` | Edit 도구만 |
| `Bash` | Bash 도구만 |
| `Edit\|Write` | Edit 또는 Write (정규식) |
| `Notebook.*` | Notebook으로 시작하는 도구 (정규식) |
| `*` 또는 `""` | 모든 도구 |

### 7. Hook 반환 값

**종료 코드**:
- **0**: 성공 (stdout은 JSON 또는 컨텍스트로 처리)
- **2**: 차단 오류 (stderr만 사용, 실행 중단)
- **기타**: 비차단 오류 (stderr는 verbose 모드에서 표시)

**JSON 출력 형식** (종료 코드 0):
```json
{
  "decision": "approve|block|deny|ask",
  "reason": "설명",
  "continue": true,
  "stopReason": "continue=false일 때 메시지",
  "suppressOutput": false,
  "systemMessage": "사용자 경고 메시지"
}
```

### 8. 일반적인 Hook 패턴

#### 편집 후 자동 포맷
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/format.sh",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

#### 모든 도구 사용 로깅
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo \"$(date): tool used\" >> ~/.claude/tool.log"
          }
        ]
      }
    ]
  }
}
```

### 9. Hooks 디버깅

```bash
# hook 명령 수동 테스트
CLAUDE_PROJECT_DIR="/path/to/project" ./your-hook-script.sh

# hook 로그 보기
claude --debug

# 설정 확인
claude /config
```

### 10. 보안 고려사항

> ⚠️ **주의**: Hooks는 시스템에서 임의의 쉘 명령을 실행합니다.

**모범 사례**:
1. 입력 검증 및 살균
2. 쉘 변수는 항상 따옴표 사용: `"$VAR"` (not `$VAR`)
3. 경로 탐색 차단 (`..` 확인)
4. 스크립트에 절대 경로 사용
5. 민감한 파일 접근 금지 (`.env`, `.git/`, 키 파일)

---

## Resources

- [Claude Code 공식 문서](https://code.claude.com/docs)
- [Hooks Reference](https://code.claude.com/docs/en/hooks)
- [Settings Reference](https://code.claude.com/docs/en/settings)
- [쉘 스크립팅 가이드](https://www.shellscript.sh/)

---

## Checklist

면접에서 답변하듯이 다음 질문에 답해보세요:

1. **Hooks란 무엇이고 언제 사용하나요?**
   <details>
   <summary>힌트</summary>
   이벤트에 명령 자동 실행: 포매팅, 검증, 로깅, 통합
   </details>

2. **PreToolUse와 PostToolUse의 차이점은?**
   <details>
   <summary>힌트</summary>
   Pre: 도구 실행 전 (차단 가능). Post: 도구 완료 후.
   </details>

3. **Hook 설정에서 matcher는 어떻게 작동하나요?**
   <details>
   <summary>힌트</summary>
   도구 이름 또는 정규식으로 필터링 (대소문자 구분)
   </details>

4. **Hooks는 어떻게 위험한 작업을 차단할 수 있나요?**
   <details>
   <summary>힌트</summary>
   종료 코드 2를 반환하거나 JSON으로 "decision": "block" 반환
   </details>

---

## Mini Project: 개발 워크플로우 자동화

### Project Goals

종합적인 hooks 시스템을 구축하세요:

- [ ] 편집 후 Prettier/ESLint를 실행하는 자동 포매팅 hook 생성
- [ ] 세션 시간과 도구 사용을 추적하는 세션 로깅 hook 생성
- [ ] 2개 이상의 선택 hooks 추가 (알림, 백업 등)
- [ ] 모든 hooks를 `.claude/settings.json`에 설정
- [ ] hooks가 올바르게 작동하는지 테스트

### Ideas to Try

- 특정 이벤트에 Slack/Discord 알림을 보내는 hook 생성
- 파괴적 작업 전 파일을 저장하는 백업 hook 구축
- stdin JSON을 파싱하여 조건부 동작 구현

---

## Advanced

### stdin JSON 파싱 예시

hook에서 stdin JSON을 파싱하여 정보를 추출합니다:

```bash
#!/bin/bash
# .claude/hooks/format-on-edit.sh

# stdin에서 JSON 읽기
INPUT=$(cat)

# jq로 파일 경로 추출
FILE_PATH=$(echo "$INPUT" | jq -r '.tool_input.file_path // empty')

if [ -n "$FILE_PATH" ] && [ -f "$CLAUDE_PROJECT_DIR/$FILE_PATH" ]; then
  npx prettier --write "$CLAUDE_PROJECT_DIR/$FILE_PATH"
fi

exit 0
```

### Hook 디버깅 팁

hook이 예상대로 동작하지 않을 때:

```bash
# 1. 스크립트 단독 테스트
echo '{"tool_input":{"file_path":"test.ts"}}' | CLAUDE_PROJECT_DIR="$(pwd)" ./hook.sh

# 2. 디버그 모드로 실행
claude --debug

# 3. hook 설정 확인
cat .claude/settings.json | jq '.hooks'
```
