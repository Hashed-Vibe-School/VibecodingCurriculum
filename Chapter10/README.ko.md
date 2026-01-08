# Chapter 10: CI/CD 통합

**한국어** | [English](./README.md)

## Prerequisites

이 Chapter를 시작하기 전에:
- [ ] Chapter 00-09 완료
- [ ] CI/CD 개념과 파이프라인 이해
- [ ] Actions가 활성화된 GitHub 계정

---

## Introduction

Vibecoding의 궁극적인 표현은 자동화입니다—여러분이 자리에 없을 때도 Claude가 작업하게 하는 것. 이 Chapter에서는 자동화된 코드 리뷰, 테스팅, 심지어 코드 생성을 위해 Claude Code를 CI/CD 파이프라인에 통합하는 방법을 다룹니다.

### 왜 CI/CD 통합인가?

- **자동화된 코드 리뷰**: 모든 PR이 AI 리뷰를 받음
- **지속적인 품질**: 머지 전 문제 발견
- **문서화**: 변경 시 문서 자동 생성
- **테스팅**: AI 지원 테스트 생성 및 검증

---

## Topics

### 1. Headless 모드

Claude Code는 `-p` 플래그로 사람 상호작용 없이 실행할 수 있습니다:

```bash
# 기본 headless 명령
claude -p "Summarize this project"

# 특정 도구 사용
claude -p "Fix the bug in auth.ts" --allowedTools "Read,Edit,Bash"

# JSON 출력
claude -p "List all exported functions" --output-format json
```

#### 출력 형식

| 형식 | 설명 |
|------|------|
| `text` | 사람이 읽을 수 있는 형식 (기본) |
| `json` | 구조화된 JSON 출력 |
| `stream-json` | 스트리밍 JSON |

### 2. Claude Code Remote

웹에서 Claude를 실행하고 로컬에서 이어하기:

```bash
# "&"를 붙여 백그라운드에서 웹 실행
> Refactor the authentication module &
# Claude가 claude.ai/code에서 백그라운드로 실행

# teleport로 로컬에서 세션 이어하기
claude --teleport <session_id>
```

이 기능은 다음에 적합합니다:
- 자리를 비운 동안 장시간 실행 작업
- 디바이스 간 전환
- 클라우드로 무거운 연산 오프로딩

### 3. 구조화된 출력

예측 가능한 JSON 출력 얻기:

```bash
claude -p "Extract function names from utils.ts" \
  --output-format json \
  --json-schema '{
    "type": "object",
    "properties": {
      "functions": {
        "type": "array",
        "items": {"type": "string"}
      }
    }
  }'
```

### 4. GitHub Actions 통합

#### 공식 Action 사용

```yaml
# .github/workflows/claude-review.yml
name: Claude Code Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            이 PR을 다음 관점에서 리뷰해주세요:
            1. 보안 취약점
            2. 성능 문제
            3. 코드 스타일 위반

            실행 가능한 피드백을 제공해주세요.
```

#### 커스텀 Headless 워크플로우

```yaml
# .github/workflows/claude-custom.yml
name: Claude Custom Tasks

on:
  push:
    branches: [main]
  pull_request:

jobs:
  analyze:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Run Claude Analysis
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p "Analyze this codebase for potential bugs" \
            --allowedTools "Read,Grep,Glob" \
            --output-format json > analysis.json

      - name: Upload Results
        uses: actions/upload-artifact@v4
        with:
          name: claude-analysis
          path: analysis.json
```

### 5. 대화 계속하기

CI에서 이전 세션 계속:

```bash
# 첫 번째 실행 - 세션 저장
claude -p "Find all TODO comments" \
  --output-format json > step1.json

# 대화 계속
claude -p "Now fix the most critical one" \
  --continue \
  --allowedTools "Read,Edit,Bash"
```

### 6. CI에서 도구 권한

Claude가 할 수 있는 것 제어:

```bash
# 읽기 전용 분석
claude -p "Review code" --allowedTools "Read,Grep,Glob"

# 편집 포함
claude -p "Fix issues" --allowedTools "Read,Edit,Write,Bash"

# 전체 접근 (주의해서 사용)
claude -p "Complete this task" --dangerouslySkipPermissions
```

### 7. 일반적인 CI 패턴

#### 린팅 문제 자동 수정
```yaml
- name: Auto-fix with Claude
  run: |
    bun run lint 2>&1 || true > lint-errors.txt
    claude -p "Fix the linting errors in lint-errors.txt" \
      --allowedTools "Read,Edit"
```

#### 릴리스 노트 생성
```yaml
- name: Generate Changelog
  run: |
    CHANGES=$(git log --oneline ${{ github.event.before }}..${{ github.sha }})
    claude -p "Generate release notes from these commits: $CHANGES" \
      --output-format json > release-notes.json
```

#### 보안 스캔
```yaml
- name: Security Review
  run: |
    claude -p "Scan for security vulnerabilities in src/" \
      --allowedTools "Read,Grep,Glob" \
      --output-format json > security-report.json
```

### 8. 에러 처리

```yaml
- name: Claude Task with Error Handling
  continue-on-error: true
  run: |
    claude -p "Fix failing tests" \
      --allowedTools "Read,Edit,Bash" \
      --timeout 300000 2>&1 | tee claude-output.txt

    if grep -q "error" claude-output.txt; then
      echo "::warning::Claude encountered issues"
    fi
```

### 9. CI 모범 사례

1. **권한 제한**: 필요한 도구만 부여
2. **타임아웃 설정**: 무한 실행 방지
3. **결과 캐시**: 검토를 위해 출력 저장
4. **변경 검토**: AI 변경을 자동 머지하지 않기
5. **비용 모니터링**: API 사용량 추적

### 10. Ralph-Wiggum 플러그인: 자율 반복 루프

Ralph-Wiggum은 Claude가 자율적으로 반복하며 작업을 완료하게 해주는 공식 플러그인입니다:

```bash
# 기본 사용법
/ralph-loop "Your task description" --completion-promise "DONE"
```

Claude가 자동으로:
1. 작업 수행
2. 종료 시도
3. Stop hook이 종료 차단
4. 같은 프롬프트 다시 실행
5. 완료될 때까지 반복

**핵심 옵션**:
| 옵션 | 설명 |
|------|------|
| `--max-iterations <n>` | 최대 반복 횟수 (기본: 무제한) |
| `--completion-promise <text>` | 완료 신호 문구 |

**실전 예시**:
```bash
/ralph-loop "Build a REST API for todos. Requirements:
- CRUD operations
- Input validation
- Tests
Output <promise>COMPLETE</promise> when done." \
--completion-promise "COMPLETE" \
--max-iterations 50
```

**적합한 사용 사례**:
- 명확한 성공 기준이 있는 작업
- 테스트/디버깅이 필요한 반복 작업
- 새 프로젝트 생성

**주의할 사용 사례**:
- 사람의 판단이 필요한 작업
- 프로덕션 디버깅
- 불명확한 성공 기준

> **참고**: ralph-wiggum은 [공식 플러그인](https://github.com/anthropics/claude-code/tree/main/plugins/ralph-wiggum)입니다.

---

## Resources

- [Claude Code 공식 문서](https://code.claude.com/docs)
- [CI/CD Integration Guide](https://code.claude.com/docs/en/github-actions)
- [GitHub Actions 문서](https://docs.github.com/en/actions)
- [Ralph-Wiggum Plugin](https://github.com/anthropics/claude-code/tree/main/plugins/ralph-wiggum)
- [Anthropic API 가격](https://www.anthropic.com/pricing)

---

## Checklist

면접에서 답변하듯이 다음 질문에 답해보세요:

1. **Headless 모드란 무엇이고 언제 사용하나요?**
   <details>
   <summary>힌트</summary>
   -p 플래그를 사용한 비대화형 모드, CI/CD와 자동화에 사용
   </details>

2. **CI에서 Claude가 사용할 수 있는 도구를 어떻게 제어하나요?**
   <details>
   <summary>힌트</summary>
   --allowedTools 플래그와 쉼표로 구분된 목록
   </details>

3. **text와 json 출력 형식의 차이는?**
   <details>
   <summary>힌트</summary>
   Text: 사람이 읽을 수 있음. JSON: 구조화됨, 스크립트로 파싱 가능
   </details>

4. **CI에서 Claude 대화를 어떻게 계속할 수 있나요?**
   <details>
   <summary>힌트</summary>
   이전 세션을 계속하려면 --continue 플래그
   </details>

5. **CI 통합에 중요한 보안 고려사항은?**
   <details>
   <summary>힌트</summary>
   도구 제한, API 키 보호, AI 변경 검토, 타임아웃 설정
   </details>

---

## Mini Project: 자동화된 개발 파이프라인

### Project Goals

Claude Code 자동화가 포함된 완전한 CI/CD 파이프라인을 구축하세요:

- [ ] 모든 PR을 자동으로 리뷰하는 PR 리뷰 워크플로우 생성
- [ ] 실패한 CI 문제를 수정하려고 시도하는 자동 수정 파이프라인 생성
- [ ] API 변경 시 문서를 업데이트하는 문서 생성기 생성
- [ ] 커밋에서 변경 로그를 생성하는 릴리스 노트 워크플로우 생성
- [ ] 2개 이상의 선택 워크플로우 추가 (테스트 생성기, 보안 감사자 등)
- [ ] 적절한 secrets와 에러 처리 구성
- [ ] 모든 워크플로우가 성공적으로 실행되는지 테스트

### Ideas to Try

- Claude Code를 래핑하는 커스텀 GitHub Action 구축
- 워크플로우의 GitLab CI 또는 Jenkins 버전 생성
- 캐싱 전략으로 비용 최적화 구현
- CI Claude 사용량과 비용을 모니터링하는 대시보드 구축

---

## Advanced

### CI 비용 최적화

Claude API 비용을 줄이는 실전 팁:

```yaml
# 1. 변경된 파일만 리뷰
- name: Get changed files
  id: changed
  run: |
    echo "files=$(gh pr view ${{ github.event.pull_request.number }} --json files -q '.files[].path' | tr '\n' ' ')" >> $GITHUB_OUTPUT

- name: Review only changed files
  run: |
    claude -p "Review only these files: ${{ steps.changed.outputs.files }}"

# 2. 작은 모델로 시작
- name: Quick review with Haiku
  run: |
    claude -p "Quick lint check" --model claude-haiku

# 3. 조건부 실행
- name: Deep review for large PRs only
  if: github.event.pull_request.additions > 100
  run: |
    claude -p "Thorough security review"
```

### 재사용 가능한 워크플로우

여러 리포지토리에서 재사용할 수 있는 워크플로우를 만드세요:

```yaml
# .github/workflows/reusable-claude-review.yml
name: Reusable Claude Review

on:
  workflow_call:
    inputs:
      review_type:
        required: true
        type: string
    secrets:
      ANTHROPIC_API_KEY:
        required: true

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Claude Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          npm install -g @anthropic-ai/claude-code
          claude -p "Perform a ${{ inputs.review_type }} review"
```

다른 리포에서 사용:
```yaml
jobs:
  review:
    uses: your-org/workflows/.github/workflows/reusable-claude-review.yml@main
    with:
      review_type: "security"
    secrets:
      ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

### 실패 시 알림 설정

Claude 작업 실패 시 Slack/Discord로 알림:

```yaml
- name: Notify on failure
  if: failure()
  run: |
    curl -X POST ${{ secrets.SLACK_WEBHOOK }} \
      -H 'Content-type: application/json' \
      -d '{"text":"Claude review failed for PR #${{ github.event.pull_request.number }}"}'
```
