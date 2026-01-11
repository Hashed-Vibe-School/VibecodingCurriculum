# Chapter 19: 자동화

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- MCP로 Claude 확장하기
- CI/CD로 자동 배포
- 헤드리스 모드 활용

---

## MCP란?

MCP(Model Context Protocol)는 Claude에게 새로운 능력을 추가하는 방법입니다.

### MCP로 할 수 있는 것

- 데이터베이스 직접 연결
- Slack/Discord 메시지 보내기
- 파일 시스템 확장
- 외부 API 연동

### 기본 개념

```
Claude Code ←→ MCP 서버 ←→ 외부 서비스
```

MCP 서버가 중간에서 Claude와 외부 서비스를 연결해줍니다.

---

## MCP 서버 설정하기

### 설정 파일 위치

`~/.claude/mcp_servers.json`:

```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-filesystem", "/path/to/allowed/files"]
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/db"
      }
    }
  }
}
```

### 자주 쓰는 MCP 서버

| 서버 | 기능 |
|------|------|
| `mcp-server-filesystem` | 파일 시스템 접근 |
| `mcp-server-postgres` | PostgreSQL 연결 |
| `mcp-server-github` | GitHub API |
| `mcp-server-slack` | Slack 메시지 |

### MCP 서버 추가하기

```
> GitHub MCP 서버 설정하는 법 알려줘
```

---

## 실습: MCP로 데이터베이스 연결

### 1. PostgreSQL MCP 설정

```json
{
  "servers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://localhost/mydb"
      }
    }
  }
}
```

### 2. Claude에서 사용

```
> 데이터베이스에서 사용자 목록 가져와줘

> users 테이블에 새 사용자 추가해줘
> 이름: 홍길동, 이메일: hong@email.com
```

Claude가 직접 데이터베이스를 조회하고 수정할 수 있습니다.

---

## CI/CD란?

CI/CD는 코드를 자동으로 테스트하고 배포하는 시스템입니다.

- **CI** (Continuous Integration): 코드 변경 시 자동 테스트
- **CD** (Continuous Deployment): 테스트 통과 시 자동 배포

### 왜 필요합니까?

수동으로:
1. 코드 수정
2. 테스트 실행
3. 빌드
4. 배포
5. 확인

CI/CD로:
1. 코드 수정
2. 나머지는 자동!

---

## GitHub Actions로 CI/CD

### 기본 워크플로우

`.github/workflows/deploy.yml`:

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

      - name: Build
        run: npm run build

      - name: Deploy to Vercel
        run: vercel --prod --token=${{ secrets.VERCEL_TOKEN }}
```

### Claude로 워크플로우 만들기

```
> 이 프로젝트용 GitHub Actions 워크플로우 만들어줘.
> main에 푸시하면 테스트하고 Vercel에 배포하게.
```

---

## Claude Code를 CI에서 사용하기

### 헤드리스 모드

`-p` 플래그로 Claude를 스크립트에서 실행할 수 있습니다:

```bash
# 기본 사용
claude -p "이 프로젝트 요약해줘"

# 특정 도구만 허용
claude -p "버그 수정해줘" --allowedTools "Read,Edit"

# JSON 출력
claude -p "함수 목록 추출해줘" --output-format json
```

### CI에서 코드 리뷰 자동화

```yaml
name: Claude Code Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            이 PR을 리뷰해주세요:
            - 버그 가능성
            - 보안 취약점
            - 코드 품질
```

### 자동 린트 수정

```yaml
- name: Auto-fix with Claude
  run: |
    npm run lint 2>&1 || true > lint-errors.txt
    claude -p "lint-errors.txt의 린트 에러들 고쳐줘" \
      --allowedTools "Read,Edit"
```

---

## 자동 배포 파이프라인

### 완전 자동화 예시

```yaml
name: Full Pipeline

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm test

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm run build

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: vercel --prod

  notify:
    needs: deploy
    runs-on: ubuntu-latest
    steps:
      - name: Notify Slack
        run: |
          curl -X POST $SLACK_WEBHOOK \
            -d '{"text": "배포 완료!"}'
```

---

## 보안 주의사항

### Secrets 관리

```yaml
# 나쁜 예
env:
  API_KEY: "sk-1234567890"  # 절대 안 됨!

# 좋은 예
env:
  API_KEY: ${{ secrets.API_KEY }}
```

GitHub Settings → Secrets에서 관리하세요.

### 권한 제한

```yaml
- name: Claude with limited permissions
  run: |
    claude -p "분석해줘" \
      --allowedTools "Read,Glob,Grep"  # 읽기만 허용
```

---

## 비용 최적화

### 변경된 파일만 처리

```yaml
- name: Get changed files
  id: changed
  run: |
    echo "files=$(git diff --name-only HEAD~1)" >> $GITHUB_OUTPUT

- name: Review only changed files
  run: |
    claude -p "이 파일들만 리뷰해줘: ${{ steps.changed.outputs.files }}"
```

### 캐싱 활용

```yaml
- name: Cache dependencies
  uses: actions/cache@v3
  with:
    path: node_modules
    key: ${{ hashFiles('package-lock.json') }}
```

---

## 실습: CI/CD 파이프라인

### 기본 과제

```
# 1. GitHub Actions 설정
> 이 프로젝트용 CI/CD 워크플로우 만들어줘

# 2. 자동 테스트
> 푸시할 때마다 테스트 돌리게 해줘

# 3. 자동 배포
> main에 머지되면 Vercel에 자동 배포
```

### 추가 도전

```
> PR 올리면 자동으로 코드 리뷰해주는 워크플로우 추가해줘

> 배포 성공/실패 시 Slack으로 알림 보내줘

> 매일 자동으로 의존성 업데이트 체크해줘
```

---

## 미니 프로젝트: 자동화 파이프라인

완전 자동화된 개발 파이프라인을 구축해보세요.

### 목표

- 전체 개발 사이클 자동화
- 실무 수준의 CI/CD 경험

### 만들기

```
> 완전한 CI/CD 파이프라인 만들어줘.
> - PR 올리면 자동 테스트
> - Claude로 코드 리뷰
> - main 머지 시 자동 배포
> - Slack 알림 전송
```

### 추가 자동화 아이디어

```
> 매일 의존성 취약점 자동 스캔

> 주간 코드 품질 리포트 자동 생성

> 릴리즈 노트 자동 작성
```

### 심화 과제 (숙련자용)

```
> 카나리 배포 설정해줘 (일부 트래픽만 새 버전)

> 롤백 자동화 설정해줘 (에러율 높으면 자동 롤백)

> 멀티 환경 배포 (dev → staging → prod)

> Feature flag 시스템 연동해줘
```

---

## 심화: AI를 시스템의 일부로 활용하기

지금까지는 Claude Code를 대화형으로 사용했습니다. 하지만 진짜 힘은 **자동화된 시스템의 일부**로 활용할 때 나옵니다.

### 일회성 작업 vs 시스템

| 일회성 | 시스템 |
|-------|-------|
| "이 버그 고쳐줘" | PR마다 자동으로 코드 리뷰 |
| "커밋 메시지 써줘" | 매 커밋에 자동으로 의미 있는 메시지 생성 |
| "테스트 만들어줘" | 코드 변경 시 자동으로 테스트 보완 |

헤드리스 모드(`-p` 플래그)를 활용하면, Claude를 스크립트나 CI 파이프라인에 통합할 수 있습니다.

### 피드백 루프 만들기

자동화의 진짜 가치는 **개선이 누적**된다는 점입니다.

1. Claude가 코드 리뷰를 수행
2. 같은 패턴의 실수가 반복됨을 발견
3. CLAUDE.md에 해당 규칙 추가
4. 다음 리뷰부터는 그 실수를 미리 잡음

이렇게 시스템이 점점 똑똑해집니다. 코드 변경 없이 프롬프트와 설정만 개선해도 품질이 올라갑니다.

### MCP로 반복 작업 제거하기

매번 같은 정보를 복사해서 Claude에게 붙여넣고 있다면, MCP 서버로 자동화할 수 있습니다.

예를 들어:
- 이슈 트래커에서 작업 내용 가져오기
- 데이터베이스 스키마 확인하기
- 슬랙 채널에 결과 공유하기

이런 작업들이 자동으로 연결되면, 여러분은 정말 중요한 판단에만 집중할 수 있습니다.

### 개발자로서 성장하기

Claude Code를 잘 활용하려면, 결국 **좋은 개발 습관**이 필요합니다:

- 명확하게 요구사항 정의하기
- 작업을 적절한 크기로 나누기
- 결과를 검증하고 피드백하기
- 반복되는 패턴을 자동화하기

AI 도구는 이런 습관을 더 빠르게 실행할 수 있게 해줄 뿐, 습관 자체를 대체하지는 않습니다. AI와 함께 일하면서 이런 역량을 키워나가면, 도구가 발전할수록 여러분도 함께 성장합니다.

---

## 정리

이번 챕터에서 배운 것:
- [x] MCP로 Claude 확장
- [x] CI/CD 개념과 설정
- [x] GitHub Actions 사용
- [x] 헤드리스 모드
- [x] 자동 배포 파이프라인
- [x] 시스템으로서의 AI 활용

다음 챕터에서는 지금까지 배운 모든 것을 종합합니다.

[Chapter 20: 마스터](../Chapter20/README.ko.md)로 넘어가세요.
