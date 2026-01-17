# Chapter 26: CI/CD 자동화

[English](./README.md) | **한국어**

---

## 💬 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이전 챕터와의 연결

[Chapter 25: MCP 연동](../Chapter25-MCP-Integration/README.ko.md)에서 MCP를 통해 Claude가 외부 서비스와 대화하는 방법을 배웠습니다. 데이터베이스 조회, GitHub 이슈 관리, Slack 알림 등을 직접 실행할 수 있게 되었죠.

이제 한 단계 더 나아가서 **자동화**를 배워봅시다:

| 챕터 | 내용 | 특징 |
|------|------|------|
| Chapter 25 (MCP) | Claude가 외부 서비스 접근 | 수동 요청 → 자동 실행 |
| **Chapter 26 (CI/CD)** | **코드 변경 시 자동 실행** | **푸시하면 모든 게 자동으로!** |

MCP는 여러분이 요청할 때 Claude가 움직였다면, CI/CD는 **코드를 푸시하기만 하면 알아서** 테스트, 빌드, 배포까지 진행됩니다!

### CI/CD와 이전 챕터들의 관계

```
┌─────────────────────────────────────────────────────────────────────┐
│                    CI/CD에서 사용하는 이전 챕터 개념들                   │
└─────────────────────────────────────────────────────────────────────┘

Chapter 22 (설정)
    │ CLAUDE.md로 프로젝트 규칙 정의
    └──────────────────────────────┐
                                    │
Chapter 23 (Hooks & Commands)       │
    │ 자동화 스크립트와 명령어        ├──▶ CI/CD 파이프라인에서
    └──────────────────────────────┤     통합 사용!
                                    │
Chapter 24 (Agents & Skills)        │
    │ 자동화된 역할과 워크플로우       │
    └──────────────────────────────┤
                                    │
Chapter 25 (MCP)                   │
    │ 외부 서비스 연동               │
    └──────────────────────────────┘
```

---

## 이 챕터에서 배우는 것

- CI/CD 개념과 필요성
- GitHub Actions로 자동화 구축
- YAML 파일 작성법
- Claude Code를 파이프라인에 통합
- 실전 워크플로우 구축
- 보안 관리와 비용 최적화

---

## 왜 CI/CD가 중요한가요?

### 실제 상황: 배포의 공포

**CI/CD 없이:**

```
금요일 오후 5시...

개발자: "작은 버그 수정 하나 배포해야 해"
       git push origin main

개발자: "어? 프로덕션에서 에러가..."

개발자: "테스트 실행 안 했네..."

개발자: "빌드도 깨져있었네..."

팀장: "금요일 저녁에 왜 프로덕션이..."

개발자: 😱 (주말 반납)
```

**CI/CD가 있으면:**

```
금요일 오후 5시...

개발자: "작은 버그 수정 하나 배포해야 해"
       git push origin main

GitHub Actions:
  ✅ Lint 통과
  ✅ 테스트 통과
  ✅ 빌드 성공
  ✅ 자동 배포 완료
  📢 Slack: "버그 수정 배포 완료!"

개발자: "주말 즐겨야지~" 😊
```

### 수동 vs 자동화 비교

| 단계 | 수동 | CI/CD |
|------|------|-------|
| 코드 검사 | 기억나면... | 매번 자동 실행 |
| 테스트 | 시간 있으면... | 매번 자동 실행 |
| 빌드 | 수동으로... | 매번 자동 실행 |
| 배포 | 수동으로... | 테스트 통과 시 자동 |
| 알림 | 기억나면... | 자동 발송 |
| **실패 발견 시점** | **프로덕션에서** | **코드 푸시 직후** |

---

## 왜 필요한가요?

**실제 상황**: 코드를 GitHub에 푸시합니다. 테스트 실행하는 것을 잊었습니다. 코드가 프로덕션을 망가뜨립니다. 팀이 화가 났습니다. 코드가 배포되기 전에 자동으로 모든 것을 확인하는 방법이 있으면 좋겠다고 생각하게 됩니다.

CI/CD가 바로 그것입니다 - 자동 체크와 배포가 실수로부터 당신을 지켜줍니다.

### 쉬운 비유: 공장 조립 라인

CI/CD 없이는 각 제품을 수작업으로 만듭니다:
- 빌드 (작동하길 바라며)
- 테스트 (기억나면)
- 출하 (손가락 꼬고)

CI/CD가 있으면 공장 조립 라인이 있는 것입니다:
- 원자재(코드)가 들어가고
- 품질 검사가 자동으로 일어나고
- 좋은 제품(작동하는 코드)만 출하됩니다

### 또 다른 비유: 자동차 생산 라인

```
┌─────────────────────────────────────────────────────────────────────┐
│                     수동 제작 vs 자동화 생산 라인                       │
└─────────────────────────────────────────────────────────────────────┘

수동 제작 (CI/CD 없음):
┌─────┐    ┌─────┐    ┌─────┐    ┌─────┐
│장인 1│ → │장인 2│ → │장인 3│ → │장인 4│
│코딩  │    │테스트 │    │빌드  │    │배포  │
└─────┘    └─────┘    └─────┘    └─────┘
    ↑          ↑          ↑          ↑
  수동       수동       수동       수동
(실수 가능) (생략 가능) (오류 가능) (위험!)

자동화 생산 라인 (CI/CD 있음):
    코드 푸시
        │
        ▼
┌───────────────────────────────────────────────────┐
│                자동화 컨베이어 벨트                  │
│  ┌────┐   ┌────┐   ┌────┐   ┌────┐   ┌────┐    │
│  │Lint│ → │Test│ → │Build│ → │Deploy│ → │Notify│ │
│  └────┘   └────┘   └────┘   └────┘   └────┘    │
│     ↓        ↓        ↓         ↓         ↓      │
│   자동     자동     자동      자동      자동     │
│  100%     100%     100%     100%     100%    │
└───────────────────────────────────────────────────┘
```

---

## 💡 비전공자 팁

> **"CI/CD"가 뭔 말이에요?**
>
> - **CI (Continuous Integration)**: "지속적 통합"
>   - 코드를 자주 합치고, 합칠 때마다 자동으로 검사
>   - 비유: 매일 청소하면 대청소가 필요 없듯이, 매번 검사하면 큰 문제가 생기지 않음
>
> - **CD (Continuous Deployment)**: "지속적 배포"
>   - 검사 통과하면 자동으로 배포
>   - 비유: 공장에서 품질 검사 통과한 제품이 자동으로 출하되는 것
>
> 쉽게 말해: **"푸시하면 알아서 검사하고 배포해주는 시스템"**

---

## CI/CD란?

### 기본 개념

```
┌─────────────────────────────────────────────────────────────────┐
│                        CI/CD 파이프라인                          │
└─────────────────────────────────────────────────────────────────┘

     코드 푸시
         │
         ▼
┌──────────────┐
│     CI       │  ← Continuous Integration
│  (자동 테스트) │
└──────┬───────┘
       │ 통과
       ▼
┌──────────────┐
│    Build     │
│  (빌드)      │
└──────┬───────┘
       │ 성공
       ▼
┌──────────────┐
│     CD       │  ← Continuous Deployment
│  (자동 배포)  │
└──────────────┘
```

- **CI**: 코드 변경 시 자동 테스트
- **CD**: 테스트 통과 시 자동 배포

### CI/CD 파이프라인 상세 구조

```
┌─────────────────────────────────────────────────────────────────────┐
│                    완전한 CI/CD 파이프라인 흐름                        │
└─────────────────────────────────────────────────────────────────────┘

개발자가 코드 푸시
        │
        ▼
┌───────────────┐
│  1. Trigger   │  코드 푸시/PR 생성 감지
│    (트리거)    │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│  2. Checkout  │  코드 다운로드
│   (체크아웃)   │
└───────┬───────┘
        │
        ├─────────────────┬─────────────────┐
        ▼                 ▼                 ▼
┌───────────────┐ ┌───────────────┐ ┌───────────────┐
│   3a. Lint    │ │  3b. Test     │ │ 3c. Security  │
│  (코드 검사)   │ │  (테스트)     │ │  (보안 검사)   │
└───────┬───────┘ └───────┬───────┘ └───────┬───────┘
        │                 │                 │
        └─────────────────┴─────────────────┘
                          │
                          ▼
                    모두 통과?
                    /        \
                  Yes         No
                  │           │
                  ▼           ▼
        ┌───────────────┐  ┌───────────────┐
        │   4. Build    │  │    알림       │
        │    (빌드)     │  │  (실패 통보)   │
        └───────┬───────┘  └───────────────┘
                │
                ▼
        ┌───────────────┐
        │  5. Deploy    │  main 브랜치만
        │   (배포)      │
        └───────┬───────┘
                │
                ▼
        ┌───────────────┐
        │  6. Notify    │
        │   (알림)      │
        └───────────────┘
```

---

## YAML 기초 (초보자용)

GitHub Actions는 YAML 형식을 사용합니다. 빠르게 알아봅시다:

### YAML이 무엇인가요?

YAML은 사람이 읽기 쉬운 설정 형식입니다. 괄호 대신 들여쓰기(파이썬처럼)를 사용합니다.

### JSON vs YAML 비교

```json
// JSON
{
  "name": "John",
  "age": 30,
  "hobbies": ["reading", "coding"]
}
```

```yaml
# YAML - 같은 데이터, 더 깔끔한 모습
name: John
age: 30
hobbies:
  - reading
  - coding
```

### 핵심 YAML 규칙

1. **들여쓰기가 중요합니다** (탭 말고 공백 2개)
2. **콜론으로 키와 값 분리** `키: 값`
3. **대시로 목록 만들기** `- 항목`
4. **주석은 #으로**

### YAML 기본 문법 상세

```yaml
# ========== 기본 값 ==========
string_value: "문자열"           # 문자열
number_value: 42                 # 숫자
boolean_value: true              # 불리언 (true/false)
null_value: null                 # null 값

# ========== 목록 (리스트) ==========
# 방법 1: 줄바꿈 스타일
fruits:
  - apple
  - banana
  - orange

# 방법 2: 인라인 스타일
fruits_inline: [apple, banana, orange]

# ========== 객체 (딕셔너리) ==========
# 방법 1: 줄바꿈 스타일
person:
  name: John
  age: 30
  city: Seoul

# 방법 2: 인라인 스타일
person_inline: {name: John, age: 30, city: Seoul}

# ========== 중첩 구조 ==========
company:
  name: TechCorp
  employees:
    - name: Alice
      role: Developer
    - name: Bob
      role: Designer
  location:
    country: Korea
    city: Seoul

# ========== 여러 줄 문자열 ==========
# | : 줄바꿈 유지
description: |
  첫 번째 줄
  두 번째 줄
  세 번째 줄

# > : 줄바꿈을 공백으로 변환
description_folded: >
  이 문장은 하나의
  긴 줄로 합쳐집니다
```

### 흔한 YAML 실수

```yaml
# ❌ 나쁨 - 일관되지 않은 들여쓰기
steps:
  - name: First
     run: echo "hi"  # <-- 2칸이 아니라 3칸!

# ✅ 좋음 - 일관된 2칸 들여쓰기
steps:
  - name: First
    run: echo "hi"

# ❌ 나쁨 - 콜론 뒤에 공백 없음
name:value

# ✅ 좋음 - 콜론 뒤에 공백
name: value

# ❌ 나쁨 - 탭 사용
jobs:
→test:  # 탭 문자 (보이지 않음)

# ✅ 좋음 - 공백 사용
jobs:
  test:  # 공백 2개
```

---

## 🔥 프로 팁

> **YAML 작성 시 에디터 설정**
>
> VS Code에서 YAML 작업 시 유용한 설정:
>
> ```json
> // settings.json
> {
>   "[yaml]": {
>     "editor.tabSize": 2,
>     "editor.insertSpaces": true,
>     "editor.autoIndent": "full"
>   }
> }
> ```
>
> 추천 확장 프로그램:
> - **YAML** (Red Hat): 문법 검사, 자동 완성
> - **GitHub Actions** (GitHub): 워크플로우 전용 자동 완성

---

## 첫 CI/CD (가장 간단한 예제)

가장 간단한 CI 워크플로우를 만들어봅시다:

### 단계 1: 폴더 만들기

```bash
mkdir -p .github/workflows
```

### 단계 2: 워크플로우 파일 만들기

`.github/workflows/hello.yml` 만들기:

```yaml
name: Hello CI

on: push

jobs:
  say-hello:
    runs-on: ubuntu-latest
    steps:
      - name: Say Hello
        run: echo "Hello, CI/CD!"
```

### 단계 3: GitHub에 푸시

```bash
git add .github/workflows/hello.yml
git commit -m "Add first CI workflow"
git push
```

### 단계 4: 결과 확인

GitHub 저장소 > Actions 탭으로 가시기 바랍니다. 워크플로우가 실행되는 것이 보일 것입니다.

이것으로 끝입니다. 첫 CI/CD 파이프라인을 만들었습니다. 이제 각 부분이 무엇을 의미하는지 알아봅시다.

---

## 워크플로우 파일 이해하기

```yaml
name: Hello CI          # GitHub UI에 표시되는 이름

on: push                 # 트리거: 코드가 푸시될 때

jobs:                    # 실행할 작업 목록
  say-hello:             # 작업 이름 (당신이 선택)
    runs-on: ubuntu-latest  # 어떤 머신에서 실행할지
    steps:               # 이 작업의 단계들
      - name: Say Hello  # 단계 이름 (로그용)
        run: echo "Hello, CI/CD!"  # 실행할 명령
```

### 워크플로우 구성 요소 상세

```yaml
# ========== 1. 이름 ==========
name: My Workflow        # Actions 탭에 표시되는 이름

# ========== 2. 트리거 (on) ==========
# 언제 워크플로우가 실행될지 정의

# 단일 이벤트
on: push

# 여러 이벤트
on: [push, pull_request]

# 상세 설정
on:
  push:
    branches:
      - main              # main 브랜치 푸시 시만
      - 'feature/*'       # feature/로 시작하는 브랜치
    paths:
      - 'src/**'          # src 폴더 변경 시만
      - '!src/**/*.md'    # md 파일 제외
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 0 * * *'   # 매일 자정에 실행
  workflow_dispatch:       # 수동 실행 버튼 추가

# ========== 3. 작업 (jobs) ==========
jobs:
  # 작업 ID (소문자, 하이픈/언더스코어 사용)
  build-and-test:
    name: Build and Test  # UI에 표시되는 이름
    runs-on: ubuntu-latest

    # 작업 순서 제어
    needs: [lint]          # lint 작업이 먼저 완료되어야 함

    # 조건부 실행
    if: github.event_name == 'push'

    # 환경 변수
    env:
      NODE_ENV: test

    # 단계들
    steps:
      # 액션 사용
      - uses: actions/checkout@v4

      # 명령 실행
      - name: Run tests
        run: npm test

      # 여러 줄 명령
      - name: Multiple commands
        run: |
          echo "첫 번째 명령"
          echo "두 번째 명령"
          npm install
          npm test

      # 환경 변수와 함께
      - name: With env
        run: echo $MY_VAR
        env:
          MY_VAR: hello

# ========== 4. 환경 변수 ==========
env:
  # 전역 환경 변수
  CI: true

jobs:
  test:
    env:
      # 작업 레벨 환경 변수
      NODE_ENV: test
    steps:
      - name: Step
        env:
          # 단계 레벨 환경 변수
          API_KEY: ${{ secrets.API_KEY }}
```

---

## 자주 사용하는 트리거 패턴

| 트리거 | 용도 | 예시 |
|--------|------|------|
| `push` | 코드 푸시 시 | 테스트 실행 |
| `pull_request` | PR 생성/업데이트 시 | 코드 리뷰 자동화 |
| `schedule` | 정해진 시간에 | 야간 빌드, 정기 검사 |
| `workflow_dispatch` | 수동 실행 | 필요할 때 배포 |
| `release` | 릴리즈 생성 시 | 패키지 배포 |

```yaml
# 복합 트리거 예시
on:
  # main에 푸시하거나 PR이 생성될 때
  push:
    branches: [main]
  pull_request:
    branches: [main]

  # 매일 오전 9시 (KST)에 자동 실행
  schedule:
    - cron: '0 0 * * *'  # UTC 기준

  # 수동 실행 옵션
  workflow_dispatch:
    inputs:
      environment:
        description: '배포 환경'
        required: true
        default: 'staging'
        type: choice
        options:
          - staging
          - production
```

---

## GitHub Actions 기초

### 워크플로우 파일 위치

```
.github/
└── workflows/
    ├── ci.yml        # CI 워크플로우
    ├── deploy.yml    # 배포 워크플로우
    └── review.yml    # 코드 리뷰 워크플로우
```

### 기본 구조

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test
```

### 자주 사용하는 Actions

| Action | 용도 | 예시 |
|--------|------|------|
| `actions/checkout@v4` | 코드 체크아웃 | 거의 모든 워크플로우에 필요 |
| `actions/setup-node@v4` | Node.js 설정 | JavaScript/TypeScript 프로젝트 |
| `actions/setup-python@v5` | Python 설정 | Python 프로젝트 |
| `actions/cache@v4` | 캐시 저장/복원 | 빌드 속도 향상 |
| `actions/upload-artifact@v4` | 파일 업로드 | 빌드 결과물 저장 |
| `actions/download-artifact@v4` | 파일 다운로드 | 이전 작업 결과물 사용 |

---

## ⚠️ 주의사항

> **워크플로우 작성 시 보안 주의점**
>
> 1. **Secrets 노출 주의**
>    ```yaml
>    # ❌ 절대 하지 마세요!
>    - run: echo ${{ secrets.API_KEY }}  # 로그에 노출!
>
>    # ✅ 올바른 방법
>    - run: my-command
>      env:
>        API_KEY: ${{ secrets.API_KEY }}  # 환경 변수로 전달
>    ```
>
> 2. **외부 Action 사용 시 주의**
>    ```yaml
>    # ⚠️ 위험할 수 있음 - 최신 버전 자동 사용
>    - uses: some-action@main
>
>    # ✅ 안전함 - 특정 버전 고정
>    - uses: some-action@v2.1.0
>    ```
>
> 3. **PR에서 Secrets 사용 제한**
>    - Fork된 PR에서는 기본적으로 Secrets에 접근 불가
>    - 이유: 악의적인 코드가 Secrets를 훔칠 수 있음

---

## 실용적인 워크플로우 예시

### 1. 기본 CI 파이프라인

```yaml
name: CI

on: [push, pull_request]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm run lint

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm test

  build:
    needs: [lint, test]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm run build
```

### 2. 자동 배포

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
        run: npm ci

      - name: Build
        run: npm run build

      - name: Deploy to Vercel
        run: |
          npm install -g vercel
          vercel --prod --token=${{ secrets.VERCEL_TOKEN }}
```

### 3. PR 리뷰 자동화

```yaml
name: PR Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Claude Code Review
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            이 PR을 리뷰해주세요:
            - 버그 가능성
            - 보안 취약점
            - 코드 품질
```

### 4. 다양한 환경에서 테스트 (Matrix)

```yaml
name: Cross-platform Test

on: [push, pull_request]

jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node-version: [18, 20, 22]
      fail-fast: false  # 하나가 실패해도 다른 테스트 계속

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      - run: npm ci
      - run: npm test
```

---

## Claude Code를 CI에서 사용하기

### 헤드리스 모드

`-p` 플래그로 스크립트에서 Claude를 실행할 수 있습니다:

```bash
# 기본 사용
claude -p "이 프로젝트 요약해줘"

# 특정 도구만 허용
claude -p "코드 분석해줘" --allowedTools "Read,Glob,Grep"

# JSON 출력
claude -p "함수 목록 추출해줘" --output-format json
```

### CI에서 코드 리뷰

```yaml
- name: Claude Code Review
  run: |
    # 변경된 파일 목록 가져오기
    CHANGED_FILES=$(git diff --name-only HEAD~1)

    # Claude로 리뷰
    claude -p "다음 파일들을 리뷰해줘: $CHANGED_FILES" \
      --allowedTools "Read,Glob,Grep"
```

### 자동 문서 생성

```yaml
- name: Generate Docs
  run: |
    claude -p "src/ 폴더의 함수들에 대한 문서 생성해줘" \
      --allowedTools "Read,Write,Glob"

    git add docs/
    git commit -m "docs: auto-generated documentation"
    git push
```

### Claude Code CI 통합 워크플로우

```yaml
name: Claude Code Integration

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  claude-review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0  # 전체 히스토리 필요

      - name: Setup Claude Code
        run: |
          npm install -g @anthropic-ai/claude-code

      - name: Get changed files
        id: changed
        run: |
          FILES=$(git diff --name-only origin/main...HEAD)
          echo "files<<EOF" >> $GITHUB_OUTPUT
          echo "$FILES" >> $GITHUB_OUTPUT
          echo "EOF" >> $GITHUB_OUTPUT

      - name: Code Review with Claude
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p "다음 변경된 파일들을 리뷰해줘:
          ${{ steps.changed.outputs.files }}

          리뷰 포인트:
          1. 버그 가능성
          2. 성능 문제
          3. 보안 취약점
          4. 코드 스타일" \
            --allowedTools "Read,Glob,Grep" \
            --output-format json > review.json

      - name: Post Review Comment
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            const review = JSON.parse(fs.readFileSync('review.json', 'utf8'));

            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: `## Claude Code Review\n\n${review.result}`
            });
```

---

## 실전 파이프라인 구축

### 완전한 CI/CD 예시

```yaml
name: Full Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

env:
  NODE_VERSION: '20'

jobs:
  # 1. 코드 품질 검사
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - run: npm ci
      - run: npm run lint
      - run: npm run type-check

  # 2. 테스트
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - run: npm ci
      - run: npm test -- --coverage

      - name: Upload coverage
        uses: codecov/codecov-action@v4
        with:
          token: ${{ secrets.CODECOV_TOKEN }}

  # 3. 빌드
  build:
    needs: [quality, test]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - run: npm ci
      - run: npm run build

      - uses: actions/upload-artifact@v4
        with:
          name: build
          path: dist/
          retention-days: 7

  # 4. 배포 (main 브랜치만)
  deploy:
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'
    needs: build
    runs-on: ubuntu-latest
    environment: production  # 환경 보호 규칙 적용
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: build
          path: dist/

      - name: Deploy to Vercel
        run: |
          npm install -g vercel
          vercel deploy --prod --token=${{ secrets.VERCEL_TOKEN }}

  # 5. 알림
  notify:
    needs: deploy
    runs-on: ubuntu-latest
    if: always()  # 성공/실패 관계없이 실행
    steps:
      - name: Slack Notification
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ needs.deploy.result }}
          fields: repo,message,commit,author,action
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK }}
```

---

## Claude와의 실전 대화 예시

### 예시 1: CI 워크플로우 생성

```
나: "우리 Node.js 프로젝트에 CI 파이프라인 만들어줘"

Claude: Node.js 프로젝트용 CI 파이프라인을 만들어드리겠습니다.

       [프로젝트 구조 분석]

       package.json을 확인해보니:
       - Node.js 20 사용
       - npm test로 테스트 실행
       - npm run lint로 린트 실행
       - npm run build로 빌드

       다음 워크플로우를 생성했습니다:

       📁 .github/workflows/ci.yml

       ```yaml
       name: CI

       on:
         push:
           branches: [main, develop]
         pull_request:
           branches: [main]

       jobs:
         lint:
           runs-on: ubuntu-latest
           steps:
             - uses: actions/checkout@v4
             - uses: actions/setup-node@v4
               with:
                 node-version: '20'
                 cache: 'npm'
             - run: npm ci
             - run: npm run lint

         test:
           runs-on: ubuntu-latest
           steps:
             - uses: actions/checkout@v4
             - uses: actions/setup-node@v4
               with:
                 node-version: '20'
                 cache: 'npm'
             - run: npm ci
             - run: npm test

         build:
           needs: [lint, test]
           runs-on: ubuntu-latest
           steps:
             - uses: actions/checkout@v4
             - uses: actions/setup-node@v4
               with:
                 node-version: '20'
                 cache: 'npm'
             - run: npm ci
             - run: npm run build
       ```

       이 워크플로우는:
       1. main과 develop 브랜치 푸시 시 실행
       2. PR 생성 시 실행
       3. lint, test가 병렬 실행
       4. 둘 다 통과하면 build 실행
       5. npm 캐시로 빌드 속도 향상

       커밋해도 될까요?

나: "좋아, 커밋해줘"

Claude: [파일 생성 및 커밋]

       ✅ 커밋 완료:

       "ci: Add GitHub Actions workflow for CI pipeline

        - Add lint, test, and build jobs
        - Enable npm caching for faster builds
        - Run on push to main/develop and PR to main"

       이제 GitHub에 푸시하면 Actions 탭에서 실행 결과를 확인할 수 있습니다.
```

### 예시 2: 워크플로우 디버깅

```
나: "CI가 계속 실패해. 왜 그런지 봐줘"

Claude: [.github/workflows/ci.yml 분석]

       워크플로우 파일을 확인해봤습니다.
       몇 가지 문제점을 발견했습니다:

       ❌ 문제 1: 들여쓰기 오류 (line 15)
       ```yaml
       steps:
         - name: Install
          run: npm ci  # 공백 1개 부족!
       ```

       ❌ 문제 2: checkout 누락
       ```yaml
       jobs:
         test:
           runs-on: ubuntu-latest
           steps:
             # checkout이 없어서 코드에 접근 불가!
             - run: npm test
       ```

       ❌ 문제 3: Node.js 버전 불일치
       ```yaml
       node-version: '18'  # 하지만 package.json은 20 요구
       ```

       수정 버전을 적용할까요?

나: "응, 고쳐줘"

Claude: [파일 수정]

       수정 완료:

       1. 들여쓰기 수정
       2. checkout 액션 추가
       3. Node.js 버전을 20으로 변경

       ```yaml
       jobs:
         test:
           runs-on: ubuntu-latest
           steps:
             - uses: actions/checkout@v4  # 추가
             - uses: actions/setup-node@v4
               with:
                 node-version: '20'  # 수정
             - name: Install
               run: npm ci  # 들여쓰기 수정
             - run: npm test
       ```

       커밋하고 푸시할까요?
```

### 예시 3: 배포 자동화 추가

```
나: "Vercel로 자동 배포되게 해줘"

Claude: Vercel 자동 배포 워크플로우를 추가하겠습니다.

       먼저 확인이 필요합니다:

       ⚠️ 필요한 Secrets:
       - VERCEL_TOKEN: Vercel API 토큰
       - VERCEL_ORG_ID: 조직 ID
       - VERCEL_PROJECT_ID: 프로젝트 ID

       Secrets 설정 방법:
       1. GitHub 저장소 → Settings → Secrets → Actions
       2. "New repository secret" 클릭
       3. 위 세 개 추가

       Vercel 토큰은 https://vercel.com/account/tokens 에서 생성할 수 있습니다.

       Secrets가 설정되어 있나요?

나: "응, 다 설정했어"

Claude: 좋습니다! 배포 워크플로우를 생성합니다.

       [파일 생성: .github/workflows/deploy.yml]

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
                 cache: 'npm'

             - name: Install dependencies
               run: npm ci

             - name: Build
               run: npm run build

             - name: Deploy to Vercel
               uses: amondnet/vercel-action@v25
               with:
                 vercel-token: ${{ secrets.VERCEL_TOKEN }}
                 vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
                 vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
                 vercel-args: '--prod'
       ```

       이 워크플로우는:
       - main 브랜치에 푸시될 때만 실행
       - CI 워크플로우(lint, test)와 별개로 실행
       - 빌드 후 Vercel에 자동 배포

       💡 팁: CI가 통과해야만 배포되게 하려면
       `needs: [ci]`를 추가하면 됩니다.

       커밋할까요?
```

### 예시 4: 에러 상황

```
나: "workflow가 실행은 되는데 npm ci에서 실패해"

Claude: npm ci 실패 원인을 분석해보겠습니다.

       [GitHub Actions 로그 확인 요청]

       로그를 확인하기 위해 몇 가지 방법이 있습니다:

       1. GitHub 저장소 → Actions 탭 → 실패한 워크플로우 클릭
       2. 실패한 step의 로그 확인

       에러 메시지를 복사해서 알려주시겠어요?
       또는 `gh run view --log-failed` 명령을 실행해볼 수 있습니다.

나: "npm ERR! code ERESOLVE 라고 나와"

Claude: ERESOLVE 에러는 의존성 충돌입니다.

       🔍 원인 분석:
       - npm ci는 package-lock.json을 엄격하게 따름
       - 로컬에서 npm install 후 lock 파일 커밋 안 함
       - 또는 다른 버전의 npm으로 생성된 lock 파일

       ✅ 해결 방법:

       방법 1: package-lock.json 재생성 (권장)
       ```bash
       rm package-lock.json
       npm install
       git add package-lock.json
       git commit -m "fix: Regenerate package-lock.json"
       git push
       ```

       방법 2: CI에서 npm ci 대신 npm install 사용 (비권장)
       ```yaml
       # npm ci 대신
       - run: npm install --legacy-peer-deps
       ```

       방법 1을 실행할까요?

나: "방법 1로 해줘"

Claude: [명령 실행]

       ```bash
       rm package-lock.json
       npm install
       ```

       ✅ package-lock.json이 재생성되었습니다.

       변경사항:
       - 의존성 버전이 최신화됨
       - 충돌 해결됨

       커밋하고 푸시할까요?

나: "응"

Claude: [커밋 및 푸시]

       ✅ 완료:

       "fix: Regenerate package-lock.json to resolve dependency conflicts"

       GitHub Actions에서 새 빌드가 시작됩니다.
       Actions 탭에서 확인해보세요.

       몇 분 후에 결과를 알려드릴까요?
```

---

## 보안 관리

### Secrets 설정

GitHub Settings → Secrets and variables → Actions에서 설정:

- `ANTHROPIC_API_KEY`: Claude API 키
- `VERCEL_TOKEN`: Vercel 배포 토큰
- `SLACK_WEBHOOK`: Slack 알림 URL

### 권한 제한

```yaml
- name: Read-only Claude
  run: |
    claude -p "코드 분석해줘" \
      --allowedTools "Read,Glob,Grep"  # 쓰기 도구 제외
```

### 환경 보호 규칙

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production  # 환경 지정

    steps:
      - name: Deploy
        run: ./deploy.sh
```

GitHub에서 환경 보호 규칙 설정:
1. Settings → Environments → production
2. Required reviewers 추가 (배포 전 승인 필요)
3. Wait timer 설정 (예: 5분 대기)

---

## 비용 최적화

### 변경된 파일만 처리

```yaml
- name: Get changed files
  id: changed
  run: |
    echo "files=$(git diff --name-only HEAD~1)" >> $GITHUB_OUTPUT

- name: Review only changed
  run: |
    claude -p "이 파일들만 리뷰해줘: ${{ steps.changed.outputs.files }}"
```

### 캐싱 활용

```yaml
# npm 캐싱
- name: Cache dependencies
  uses: actions/cache@v4
  with:
    path: ~/.npm
    key: npm-${{ hashFiles('package-lock.json') }}
    restore-keys: npm-

# setup-node의 내장 캐싱
- uses: actions/setup-node@v4
  with:
    node-version: '20'
    cache: 'npm'  # 자동 캐싱!
```

### 조건부 실행

```yaml
jobs:
  # 특정 파일 변경 시만 실행
  test-frontend:
    if: contains(github.event.head_commit.modified, 'frontend/')
    runs-on: ubuntu-latest
    steps:
      - run: npm test

  # main 브랜치에서만 배포
  deploy:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - run: ./deploy.sh

  # PR에서만 리뷰
  review:
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest
    steps:
      - run: npm run review
```

### GitHub Actions 무료 한도

| 계정 유형 | 월간 무료 시간 |
|-----------|--------------|
| Free | 2,000분 |
| Pro | 3,000분 |
| Team | 3,000분 |
| Enterprise | 50,000분 |

**시간 절약 팁**:
- 캐싱 적극 활용
- 불필요한 워크플로우 비활성화
- 병렬 실행으로 시간 단축
- 필요한 브랜치에서만 실행

---

## 따라해보십시오

### 실습 1: 첫 워크플로우 만들기

1. 가장 간단한 워크플로우 만들기 (위의 "첫 CI/CD" 섹션 참고)
2. GitHub에 푸시
3. Actions 탭에서 실행되는 것을 확인

### 실습 2: 실제 테스트 추가

워크플로우를 확장해서 실제 테스트 실행:

```yaml
name: Test

on: push

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm test
```

### 실습 3: 여러 작업 추가

lint와 test가 병렬로 실행되는 워크플로우 만들기:

```yaml
name: CI

on: push

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm run lint

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm test
```

---

## 문제가 발생하면?

### 문제: 워크플로우가 트리거되지 않습니다

**가능한 원인:**
1. YAML 문법 에러
2. 워크플로우 파일이 잘못된 위치에 있음
3. 브랜치 이름이 일치하지 않음

**해결 방법:**
- 온라인 검증기로 YAML 문법 체크
- 파일이 `.github/workflows/` 폴더에 있어야 함
- `on:` 트리거가 브랜치와 일치하는지 확인

### 문제: "Invalid workflow file"

**가능한 원인:**
1. YAML 들여쓰기가 잘못됨
2. 필수 필드가 빠짐
3. 액션 이름 오타

**해결 방법:**
- 정확히 2칸 들여쓰기 사용
- 모든 워크플로우에 필요: `name`, `on`, `jobs`
- 액션 이름 철자 확인

### 문제: 로컬에서는 테스트가 통과하는데 CI에서 실패합니다

**가능한 원인:**
1. 다른 Node/Python 버전
2. 환경 변수 누락
3. 다른 OS (당신의 Mac vs Ubuntu)

**해결 방법:**
- 워크플로우의 버전을 로컬 버전과 맞추기
- 워크플로우에 환경 변수 추가
- CI와 같은 OS에서 테스트

### 문제: Secrets가 작동하지 않습니다

**가능한 원인:**
1. Secret 이름 오타
2. 저장소에 Secret을 추가하지 않음
3. 잘못된 Secret 범위

**해결 방법:**
- Settings > Secrets에서 정확한 Secret 이름 확인
- 올바른 저장소에 Secret 추가
- `${{ secrets.SECRET_NAME }}` 형식 사용

### 문제 해결 플로우차트

```
워크플로우가 실행되지 않음
        │
        ▼
┌─────────────────┐
│ YAML 문법 확인   │ ──에러있음──▶ 온라인 검증기로 수정
│                 │
└────────┬────────┘
         │ 정상
         ▼
┌─────────────────┐
│ 파일 위치 확인   │ ──잘못됨──▶ .github/workflows/로 이동
│                 │
└────────┬────────┘
         │ 정상
         ▼
┌─────────────────┐
│ 트리거 확인     │ ──불일치──▶ on: 섹션 수정
│ (브랜치 이름)   │
└────────┬────────┘
         │ 일치
         ▼
┌─────────────────┐
│ Actions 탭에서  │ ──에러──▶ 로그 확인 후 수정
│ 로그 확인       │
└────────┬────────┘
         │ 성공
         ▼
      실행 완료!
```

---

## 자주 하는 실수

### 1. 잘못된 들여쓰기

```yaml
# ❌ 나쁨 - 공백 대신 탭
jobs:
→test:  # 이건 탭이에요!

# ✅ 좋음 - 2칸 공백
jobs:
  test:
```

### 2. checkout 잊기

```yaml
# ❌ 나쁨 - checkout 없음, 파일 접근 불가
steps:
  - run: npm test

# ✅ 좋음 - 먼저 checkout
steps:
  - uses: actions/checkout@v4
  - run: npm test
```

### 3. 로그에 비밀 노출

```yaml
# ❌ 나쁨 - 로그에 비밀 출력!
- run: echo ${{ secrets.API_KEY }}

# ✅ 좋음 - 비밀을 직접 사용
- run: my-command
  env:
    API_KEY: ${{ secrets.API_KEY }}
```

### 4. 모든 푸시에 실행

```yaml
# ❌ 나쁨 - 모든 브랜치에서 실행
on: push

# ✅ 더 나음 - main 브랜치만
on:
  push:
    branches: [main]
```

### 5. 의존성 캐싱 안 하기

```yaml
# ❌ 느림 - 매번 설치
steps:
  - uses: actions/setup-node@v4
    with:
      node-version: '20'
  - run: npm ci

# ✅ 빠름 - 캐싱 사용
steps:
  - uses: actions/setup-node@v4
    with:
      node-version: '20'
      cache: 'npm'  # 캐싱!
  - run: npm ci
```

### 6. needs 순서 실수

```yaml
# ❌ 나쁨 - 존재하지 않는 작업에 의존
jobs:
  build:
    needs: [test]  # test가 아래에 정의됨
    runs-on: ubuntu-latest
    steps: ...

  test:  # build가 이걸 기다리는데 위에 정의됨
    runs-on: ubuntu-latest
    steps: ...

# ✅ 좋음 - 순서 명확
jobs:
  test:
    runs-on: ubuntu-latest
    steps: ...

  build:
    needs: [test]  # test가 위에 정의됨
    runs-on: ubuntu-latest
    steps: ...
```

---

## 🎯 미니 퀴즈

### 퀴즈 1: CI/CD 정의
CI/CD에서 CI가 의미하는 것은?

<details>
<summary>정답 보기</summary>

**정답**: Continuous Integration (지속적 통합)

**설명**: CI는 코드 변경사항을 자주 메인 브랜치에 통합하고, 매번 자동으로 빌드와 테스트를 실행하는 것입니다. 이를 통해 문제를 조기에 발견하고 해결할 수 있습니다.
</details>

### 퀴즈 2: 워크플로우 파일 위치
GitHub Actions 워크플로우 파일은 어디에 위치해야 하나요?

<details>
<summary>정답 보기</summary>

**정답**: `.github/workflows/` 폴더

**설명**: GitHub Actions는 저장소의 `.github/workflows/` 폴더에 있는 `.yml` 또는 `.yaml` 파일을 자동으로 인식합니다. 이 위치가 아니면 워크플로우가 실행되지 않습니다.
</details>

### 퀴즈 3: 트리거 이해
다음 워크플로우는 언제 실행되나요?

```yaml
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
```

<details>
<summary>정답 보기</summary>

**정답**:
1. main 브랜치에 직접 푸시할 때
2. main 브랜치를 대상으로 하는 PR이 생성되거나 업데이트될 때

**설명**: `push`와 `pull_request`가 둘 다 `branches: [main]`으로 설정되어 있어서, main 브랜치와 관련된 이 두 가지 이벤트 모두에서 실행됩니다.
</details>

### 퀴즈 4: Jobs 실행 순서
다음 워크플로우에서 build 작업은 언제 실행되나요?

```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps: ...

  test:
    runs-on: ubuntu-latest
    steps: ...

  build:
    needs: [lint, test]
    runs-on: ubuntu-latest
    steps: ...
```

<details>
<summary>정답 보기</summary>

**정답**: lint와 test 작업이 모두 성공적으로 완료된 후에 실행됩니다.

**설명**:
- `needs: [lint, test]`는 build가 lint와 test에 의존한다는 의미입니다
- lint와 test는 병렬로 실행됩니다 (서로 needs 관계가 없음)
- 둘 다 성공해야 build가 실행됩니다
- 하나라도 실패하면 build는 실행되지 않습니다
</details>

### 퀴즈 5: Secrets 사용
다음 중 Secrets를 올바르게 사용하는 방법은?

A) `run: echo ${{ secrets.API_KEY }}`
B) `run: curl -H "Auth: ${{ secrets.API_KEY }}"`
C) 환경 변수로 전달 후 사용

<details>
<summary>정답 보기</summary>

**정답**: C) 환경 변수로 전달 후 사용

**설명**:
- A는 로그에 비밀이 그대로 노출됨
- B도 명령줄에 비밀이 포함되어 로그에 노출될 수 있음
- C가 가장 안전함:
  ```yaml
  - run: curl -H "Auth: $API_KEY"
    env:
      API_KEY: ${{ secrets.API_KEY }}
  ```
  GitHub가 자동으로 로그에서 secrets 값을 마스킹하지만, 환경 변수로 전달하는 것이 가장 안전한 방법입니다.
</details>

---

## 📝 실습 과제

### 과제 1: 기초 - Hello CI 만들기 [난이도: ★☆☆]

**목표**: 첫 번째 CI 워크플로우 작성 및 실행

**단계**:
1. `.github/workflows/hello.yml` 파일 생성
2. 간단한 echo 명령 실행하는 워크플로우 작성
3. GitHub에 푸시
4. Actions 탭에서 결과 확인

**템플릿**:
```yaml
name: Hello CI

on: push

jobs:
  hello:
    runs-on: ubuntu-latest
    steps:
      - name: Say Hello
        run: echo "Hello from CI!"
```

**확인 사항**:
- [ ] 워크플로우가 자동 실행됨
- [ ] Actions 탭에서 녹색 체크마크
- [ ] 로그에서 "Hello from CI!" 확인

### 과제 2: 중급 - 테스트 자동화 [난이도: ★★☆]

**목표**: 실제 프로젝트에 테스트 자동화 추가

**단계**:
1. Node.js 프로젝트에 테스트 워크플로우 추가
2. npm ci로 의존성 설치
3. npm test로 테스트 실행
4. 캐싱 추가로 속도 개선

**템플릿**:
```yaml
name: Test

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test
```

**확인 사항**:
- [ ] 테스트가 CI에서 실행됨
- [ ] 캐싱으로 두 번째 실행이 빠름
- [ ] 테스트 실패 시 빨간 X 표시

### 과제 3: 심화 - 완전한 CI/CD 파이프라인 [난이도: ★★★]

**목표**: lint → test → build → deploy 파이프라인 구축

**단계**:
1. 병렬 lint, test 작업 설정
2. 둘 다 통과 후 build 실행
3. main 브랜치에서만 배포
4. Slack 알림 추가

**템플릿**:
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm test

  build:
    needs: [lint, test]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: dist
          path: dist/

  deploy:
    needs: build
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: dist
      - name: Deploy
        run: echo "Deploying to production..."
        # 실제 배포 명령 추가
```

---

## 🏆 도전 과제

### 도전 1: Claude Code 리뷰 자동화

PR이 생성되면 Claude가 자동으로 코드 리뷰하는 워크플로우 만들기:

1. PR 생성 시 트리거
2. 변경된 파일 목록 추출
3. Claude에게 리뷰 요청
4. PR에 코멘트로 결과 게시

### 도전 2: Matrix 빌드

여러 Node.js 버전과 OS에서 테스트하는 워크플로우:

1. Node.js 18, 20, 22
2. Ubuntu, Windows, macOS
3. 모든 조합에서 테스트 실행

### 도전 3: 릴리즈 자동화

GitHub Release 생성 시 자동으로:

1. 릴리즈 노트 생성
2. npm 패키지 배포
3. Docker 이미지 빌드 및 푸시
4. Slack 알림 발송

---

## 정리

이번 챕터에서 배운 것:
- [x] CI/CD 개념 (자동 테스트, 자동 배포)
- [x] YAML 문법과 작성법
- [x] GitHub Actions 기본 구조
- [x] 실용적인 워크플로우 예시
- [x] Claude Code CI 통합
- [x] 보안과 비용 최적화

**핵심 포인트**: 자동화는 한 번 설정하면 계속 가치를 제공합니다. CI/CD 파이프라인으로 실수를 예방하고, 배포 과정을 안전하게 만들 수 있습니다.

---

## 용어 사전

| 용어 | 설명 |
|------|------|
| **CI** | Continuous Integration. 코드 변경 시 자동으로 빌드/테스트 |
| **CD** | Continuous Deployment. 테스트 통과 시 자동 배포 |
| **Pipeline** | 코드가 거치는 일련의 자동화된 단계들 |
| **Workflow** | GitHub Actions에서 자동화 프로세스를 정의한 파일 |
| **Job** | 워크플로우 내의 개별 작업 단위 |
| **Step** | Job 내의 개별 명령 또는 액션 |
| **Trigger** | 워크플로우를 시작하게 하는 이벤트 |
| **Runner** | 워크플로우를 실행하는 서버 (ubuntu-latest 등) |
| **Artifact** | 워크플로우에서 생성된 파일 (빌드 결과물 등) |
| **Secret** | 민감한 정보를 안전하게 저장하는 변수 |
| **Matrix** | 여러 환경 조합에서 테스트하는 전략 |
| **Cache** | 빌드 속도 향상을 위해 파일을 저장해두는 것 |

---

## 다음 챕터 미리보기

Chapter 27에서는 **팀 협업**을 배웁니다. 커리큘럼의 마지막 챕터입니다!

- 팀에서 Claude Code 활용 전략
- 표준화된 설정 공유
- 협업 워크플로우 구성
- 온보딩 가이드 작성
- **전체 커리큘럼 요약**
- **다음 학습 단계 안내**

CI/CD로 자동화를 배웠으니, 이제 팀 전체가 효과적으로 Claude Code를 활용하는 방법을 알아보겠습니다!

---

[Chapter 27: 팀 협업](../Chapter27-Team-Collaboration/README.ko.md)으로 넘어가시기 바랍니다.

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [GitHub Actions 공식 문서](https://docs.github.com/en/actions) - GitHub Actions 완벽 가이드
- [GitHub Actions 마켓플레이스](https://github.com/marketplace?type=actions) - 재사용 가능한 Actions
- [YAML 공식 문서](https://yaml.org/) - YAML 스펙

**영상 자료:**
- [GitHub Actions 입문 (YouTube)](https://www.youtube.com/results?search_query=github+actions+tutorial+korean) - GitHub Actions 기초 (한국어)
- [CI/CD 개념 설명 (YouTube)](https://www.youtube.com/results?search_query=CI+CD+explained+beginners) - CI/CD 개념 설명
- [DevOps Tutorial (YouTube)](https://www.youtube.com/results?search_query=devops+tutorial+beginners) - DevOps 기초

**읽을거리:**
- [GitHub Actions Quickstart](https://docs.github.com/en/actions/quickstart) - 빠른 시작 가이드
- [Awesome GitHub Actions](https://github.com/sdras/awesome-actions) - 유용한 Actions 모음
- [CI/CD Best Practices](https://www.atlassian.com/continuous-delivery/principles/continuous-integration-vs-delivery-vs-deployment) - CI/CD 베스트 프랙티스

**관련 도구:**
- [act](https://github.com/nektos/act) - 로컬에서 GitHub Actions 실행
- [Vercel](https://vercel.com/) - 프론트엔드 배포 플랫폼
- [Railway](https://railway.app/) - 백엔드 배포 플랫폼
- [Codecov](https://codecov.io/) - 코드 커버리지 분석

**실습 자료:**
- [GitHub Learning Lab](https://github.com/skills) - GitHub 공식 실습 코스

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
