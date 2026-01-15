# Chapter 23: CI/CD 자동화

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- CI/CD 개념과 필요성
- GitHub Actions로 자동화 구축
- Claude Code를 파이프라인에 통합

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

### 흔한 YAML 실수

```yaml
# 나쁨 - 일관되지 않은 들여쓰기
steps:
  - name: First
     run: echo "hi"  # <-- 2칸이 아니라 3칸!

# 좋음 - 일관된 2칸 들여쓰기
steps:
  - name: First
    run: echo "hi"

# 나쁨 - 콜론 뒤에 공백 없음
name:value

# 좋음 - 콜론 뒤에 공백
name: value
```

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

---

## 왜 CI/CD를 알아야 할까?

수동 작업은 실수와 시간 낭비를 유발합니다:

```
수동:
1. 코드 수정
2. 테스트 실행 (잊어버림)
3. 빌드 (오류 발생)
4. 다시 수정
5. 배포 (설정 누락)
...

자동화:
1. 코드 수정
2. 나머지는 자동!
```

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

jobs:
  # 1. 코드 품질 검사
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
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
          node-version: '20'
      - run: npm ci
      - run: npm test -- --coverage

  # 3. 빌드
  build:
    needs: [quality, test]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: build
          path: dist/

  # 4. 배포 (main 브랜치만)
  deploy:
    if: github.ref == 'refs/heads/main'
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: build
      - run: vercel --prod --token=${{ secrets.VERCEL_TOKEN }}

  # 5. 알림
  notify:
    needs: deploy
    runs-on: ubuntu-latest
    steps:
      - name: Slack Notification
        run: |
          curl -X POST ${{ secrets.SLACK_WEBHOOK }} \
            -H 'Content-type: application/json' \
            -d '{"text": "배포 완료!"}'
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
- name: Cache dependencies
  uses: actions/cache@v4
  with:
    path: node_modules
    key: npm-${{ hashFiles('package-lock.json') }}
```

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

---

## 자주 하는 실수

1. **잘못된 들여쓰기**
   ```yaml
   # 나쁨 - 공백 대신 탭
   jobs:
   	test:  # 이건 탭이에요!

   # 좋음 - 2칸 공백
   jobs:
     test:
   ```

2. **checkout 잊기**
   ```yaml
   # 나쁨 - checkout 없음, 파일 접근 불가
   steps:
     - run: npm test

   # 좋음 - 먼저 checkout
   steps:
     - uses: actions/checkout@v4
     - run: npm test
   ```

3. **로그에 비밀 노출**
   ```yaml
   # 나쁨 - 로그에 비밀 출력!
   - run: echo ${{ secrets.API_KEY }}

   # 좋음 - 비밀을 직접 사용
   - run: my-command
     env:
       API_KEY: ${{ secrets.API_KEY }}
   ```

4. **모든 푸시에 실행**
   ```yaml
   # 나쁨 - 모든 브랜치에서 실행
   on: push

   # 더 나음 - main 브랜치만
   on:
     push:
       branches: [main]
   ```

5. **의존성 캐싱 안 하기**
   - 캐싱 없이 매번 `npm install` 실행
   - 캐싱을 추가하면 워크플로우 속도가 크게 향상

---

## 정리

이번 챕터에서 배운 것:
- [x] CI/CD 개념 (자동 테스트, 자동 배포)
- [x] GitHub Actions 기본 구조
- [x] 실용적인 워크플로우 예시
- [x] Claude Code CI 통합
- [x] 보안과 비용 최적화

**핵심 포인트**: 자동화는 한 번 설정하면 계속 가치를 제공합니다.

다음 챕터에서는 팀에서 Claude Code를 효과적으로 활용하는 방법을 배웁니다.

[Chapter 24: 팀 협업](../Chapter24/README.ko.md)으로 넘어가시기 바랍니다.
