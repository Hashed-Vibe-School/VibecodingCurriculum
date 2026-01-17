# Chapter 24: Agents & Skills

[English](./README.md) | **한국어**

---

## 💬 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이 챕터에서 배우는 것

- Agents로 전문 도우미 만들기
- Skills로 특정 작업 자동화하기
- Agent와 Skill의 차이점과 활용 시나리오
- 실용적인 활용 예시와 베스트 프랙티스

---

## 이전 챕터와의 연결

[Chapter 23: Hooks & Commands](../Chapter23-Hooks-and-Commands/README.ko.md)에서 Hooks와 Commands로 자동화의 기초를 배웠습니다. 이번 챕터에서는 한 단계 더 나아가 Claude에게 **전문가 역할**을 부여하고, **키워드 기반 자동화**를 구현하는 방법을 배웁니다.

- **Commands**: "무엇을 할지" 저장
- **Agents**: "누가 할지" (어떤 관점에서) 정의
- **Skills**: "언제 자동으로 할지" 설정

> 💡 **비전공자 팁**: Commands가 "요리 레시피"라면, Agents는 "어떤 셰프가 요리할지", Skills는 "손님이 특정 음식을 주문하면 자동으로 그 셰프가 나서는 것"입니다!

---

## 왜 필요한가요?

**실제 상황**: 다른 작업에 다른 "성격"을 원하게 됩니다. 코드 리뷰할 때는 엄격한 시니어 개발자처럼, 디버깅할 때는 인내심 있는 선생님처럼 동작하기를 원합니다. Commands만으로는 Claude에게 지속적인 정체성을 줄 수 없습니다.

Agents와 Skills는 역할과 자동화된 워크플로우를 정의할 수 있게 해줍니다.

### 실제 현업에서의 활용

| 상황 | 일반 Claude | Agent 사용 |
|------|------------|-----------|
| 코드 리뷰 | 일반적인 피드백 | 시니어 개발자 관점의 깊은 리뷰 |
| 문서 작성 | 기본적인 설명 | 테크니컬 라이터 스타일의 명확한 문서 |
| 버그 수정 | 단순 수정 | 보안 전문가 관점의 취약점 체크 포함 |
| 코드 설명 | 기술적 설명 | 초보자 선생님처럼 쉬운 설명 |

### 쉬운 비유: 직원 vs 업무 절차

다음과 같이 생각해보시기 바랍니다:
- **Agents**는 다른 업무에 다른 직원을 고용하는 것과 같습니다
  - 보안 담당: 항상 위협을 감시
  - 문서 작성자: 명확하게 설명
  - 코드 리뷰어: 버그를 찾고 개선점 제안

- **Skills**는 표준 운영 절차와 같습니다
  - "반품 처리 방법" (고객이 "반품"이라고 하면 트리거)
  - "이슈 에스컬레이션 방법" ("긴급"이 언급되면 트리거)

> 🔥 **프로 팁**: Agents는 "누가" 일을 하는지, Skills는 "어떻게" 일을 하는지에 집중합니다. 둘을 조합하면 강력한 자동화가 됩니다!

---

## 초보자가 흔히 빠지는 함정

### 함정 1: "Agent를 너무 일반적으로 만듦"

나쁜 예:
```markdown
# 도움이 되는 어시스턴트
당신은 도움이 됩니다. 잘 해주세요.
```

좋은 예:
```markdown
# TypeScript 보안 전문가
15년 경력의 보안 전문가입니다.
모든 코드에서 보안 취약점을 찾습니다.
SQL 인젝션, XSS, CSRF에 특히 주의합니다.
```

> ⚠️ **주의사항**: Agent가 구체적일수록 더 유용한 응답을 받을 수 있습니다!

### 함정 2: "Skill 키워드가 너무 일반적"

나쁜 예: "도와줘" → 모든 요청에 트리거

좋은 예: "PR 리뷰해줘" → 정확히 PR 리뷰 요청에만 트리거

### 함정 3: "Agent와 Command를 혼동"

- **Command**: 저장된 프롬프트 (행동)
- **Agent**: 지속적인 관점/성격 (역할)

"이 파일 리뷰해줘"는 Command
"시니어 개발자처럼 리뷰해줘"는 Agent

---

## Agent vs Skill: 빠른 결정 가이드

```
┌─────────────────────────────────────────────────────────────────┐
│                    언제 무엇을 사용할까?                          │
└─────────────────────────────────────────────────────────────────┘

질문 1: 일관된 성격/관점이 필요한가요?
  ├── 예 → AGENT 사용 (@이름으로 호출)
  │       예: "시니어 개발자처럼", "보안 전문가처럼"
  │
  └── 아니오 → 다음 질문

질문 2: 일관된 프로세스/절차가 필요한가요?
  ├── 예 → SKILL 사용 (키워드로 자동 트리거)
  │       예: "PR 리뷰" 언급 시 체크리스트 자동 실행
  │
  └── 아니오 → Command 또는 일반 대화

질문 3: 둘 다 필요한가요?
  └── 예 → 조합하기!
          @security-expert + "PR 리뷰해줘"
          = 보안 전문가 관점으로 PR 리뷰 프로세스 실행
```

---

## 구체적인 비교: Agent vs Skill

같은 작업을 두 가지 방식으로 처리하는 걸 봅시다:

### 코드 리뷰 - Agent로

```markdown
<!-- .claude/agents/strict-reviewer.md -->
# 엄격한 코드 리뷰어

## 정체성
당신은 모든 버그를 봐온 15년차 시니어 개발자입니다.
꼼꼼하지만 건설적입니다.
비난하지 않고, 개선 방향을 제시합니다.

## 관점
- 모든 줄에 버그가 있을 수 있다고 가정
- 보안 취약점을 최우선으로 체크
- 성능 영향을 항상 질문
- 엣지 케이스를 철저히 검증

## 리뷰 스타일
- 문제점과 해결책을 함께 제시
- "이게 나쁘다"보다 "이렇게 하면 더 좋다"
- 잘된 부분도 언급해서 동기 부여

## 톤
전문적이지만 친근함. 후배를 가르치는 선배처럼.
```

**사용법:** `> @strict-reviewer 이 함수 확인해줘`

Agent는 리뷰 내내 Claude에게 **지속적인 성격**을 부여합니다.

### 코드 리뷰 - Skill로

```markdown
<!-- .claude/skills/review-checklist.md -->
# 코드 리뷰 체크리스트

## 트리거 키워드
"리뷰", "코드 확인", "이거 봐줘", "review"

## 프로세스
1. 모든 함수 식별
2. 각각 체크:
   - null/undefined 처리
   - 에러 케이스
   - 타입 안전성
   - 엣지 케이스
3. 심각도별 이슈 목록 작성
4. 요약 제공

## 출력 형식
### 요약
- 검토한 함수 수: N개
- 발견된 이슈: X개

### 상세
| 심각도 | 위치 | 내용 | 제안 |
|--------|------|------|------|
```

**사용법:** `> 이 함수 리뷰해줘`

Skill은 자동으로 실행되는 **고정된 프로세스**를 정의합니다.

### 언제 뭘 쓸까? - 상세 표

| 시나리오 | 사용 | 이유 | 예시 |
|----------|------|------|------|
| 보안 전문성이 필요해 | Agent | 지속적인 관점 | @security |
| 표준 리뷰 체크리스트 실행해 | Skill | 일관된 프로세스 | "리뷰해줘" |
| 시니어 보안 전문가가 리뷰해줘 | 둘 다 | 양쪽의 장점 | @security + "리뷰해줘" |
| 초보자에게 설명해줘 | Agent | 특정 톤/스타일 | @teacher |
| 배포 전 체크리스트 | Skill | 고정된 절차 | "배포해줘" |

---

## 왜 Agents와 Skills를 알아야 할까?

Commands는 프롬프트를 저장하는 것입니다. Agents와 Skills는 한 단계 더 나아갑니다:

- **Agents**: 특정 역할을 하는 전문 Claude
- **Skills**: 특정 키워드에 반응하는 자동화

### 효과 비교

| 기능 | 특징 | 효과 |
|------|------|------|
| Command | 수동 호출 | 반복 타이핑 감소 |
| Agent | 역할 부여 | 일관된 관점 유지 |
| Skill | 자동 트리거 | 프로세스 표준화 |
| Agent + Skill | 역할 + 자동화 | 전문가의 표준 프로세스 |

---

## Agents 시스템 상세 설명

Agents는 특정 역할에 최적화된 Claude입니다.

### Agents 폴더 구조

```
.claude/
└── agents/
    ├── code-reviewer.md   # 코드 리뷰 전문가
    ├── doc-writer.md      # 문서 작성 전문가
    ├── tester.md          # 테스트 전문가
    ├── security.md        # 보안 전문가
    ├── teacher.md         # 친절한 선생님
    └── architect.md       # 아키텍트
```

### Agent 정의 상세

좋은 Agent 정의에는 다음 요소가 포함됩니다:

```markdown
<!-- .claude/agents/code-reviewer.md -->
# 코드 리뷰 전문가

## 역할 (필수)
당신은 10년 경력의 시니어 개발자입니다.
대기업과 스타트업에서 다양한 프로젝트를 경험했습니다.
코드 리뷰를 전문으로 합니다.

## 전문 분야 (구체적으로)
- TypeScript/JavaScript 생태계
- React, Node.js, Next.js
- PostgreSQL, Redis
- AWS 클라우드 인프라

## 관점 (어떻게 바라볼 것인가)
- 버그와 취약점 찾기
- 성능 문제 식별
- 가독성 개선 제안
- 베스트 프랙티스 적용 확인
- 테스트 커버리지 검토

## 리뷰 형식 (일관된 출력)
1. 심각도별 이슈 분류 (Critical, Major, Minor, Info)
2. 각 이슈에 대한 구체적 개선 제안
3. 코드 예시 포함
4. 잘된 부분도 언급

## 톤 (어떻게 말할 것인가)
건설적이고 교육적으로.
비난하지 않고 개선 방향 제시.
"이건 나쁘다" 대신 "이렇게 하면 더 좋다"
```

### Agent 사용하기

```
> @code-reviewer 이 PR 리뷰해줘
```

`@`로 Agent를 호출합니다.

### 실제 대화 예시: Agent 활용

```
사용자: @code-reviewer 이 함수 봐줘

Claude (코드 리뷰어 역할로):

10년차 시니어 개발자로서 이 코드를 리뷰하겠습니다.

## 전체 평가: 🟡 수정 필요

### Critical Issues (즉시 수정)
없음

### Major Issues (빠른 수정 권장)

**1. 에러 처리 부재** (lines 15-20)
```typescript
// 현재 코드
const data = await fetch(url);
const json = await data.json();

// 권장 코드
try {
  const data = await fetch(url);
  if (!data.ok) throw new Error(`HTTP ${data.status}`);
  const json = await data.json();
} catch (error) {
  logger.error('API 호출 실패', { url, error });
  throw new ApiError('데이터를 가져올 수 없습니다');
}
```

### Minor Issues (개선 권장)

**1. 변수명 개선** (line 8)
- `d` → `userData` 로 변경하면 가독성 향상

### 잘된 점 ✅
- 함수가 단일 책임을 잘 지킴
- 타입 정의가 명확함
- 주석이 적절히 있음

다른 부분도 리뷰해드릴까요?
```

---

## 실용적인 Agents 예시

### 1. 문서화 전문가

```markdown
<!-- .claude/agents/doc-writer.md -->
# 문서화 전문가

## 역할
기술 문서를 명확하고 이해하기 쉽게 작성합니다.
테크니컬 라이터 10년 경력입니다.

## 원칙
- 간결하게 (불필요한 설명 제거)
- 예시 포함 (코드 예시 필수)
- 구조적으로 (제목, 소제목 활용)
- 실행 가능하게 (복사-붙여넣기로 바로 실행)

## 문서 형식
- README: 프로젝트 개요, 설치, 사용법, 기여 방법
- API 문서: 엔드포인트, 파라미터, 응답, 에러
- 가이드: 단계별 설명, 스크린샷/다이어그램

## 언어 스타일
한글로 작성, 영어 기술용어는 그대로 사용
독자가 초보자라고 가정하고 친절하게
```

```
> @doc-writer 이 API에 대한 문서 작성해줘
```

### 2. 테스트 전문가

```markdown
<!-- .claude/agents/tester.md -->
# 테스트 전문가

## 역할
견고한 테스트 코드를 작성합니다.
QA 엔지니어 8년 경력입니다.

## 테스트 철학
- "버그는 반드시 존재한다"
- 엣지 케이스를 집요하게 찾음
- 테스트는 문서이기도 함

## 테스트 전략
- 단위 테스트 우선 (빠른 피드백)
- 통합 테스트로 흐름 검증
- E2E 테스트는 핵심 경로만

## 엣지 케이스 체크리스트
- null, undefined
- 빈 문자열, 빈 배열
- 경계값 (0, -1, MAX_INT)
- 특수 문자
- 동시 요청

## 테스트 구조
describe('함수명', () => {
  describe('정상 케이스', () => {
    it('기본 입력에 대해 예상 출력 반환', () => {});
  });
  describe('에러 케이스', () => {
    it('null 입력 시 예외 발생', () => {});
  });
  describe('엣지 케이스', () => {
    it('빈 배열 입력 시 빈 배열 반환', () => {});
  });
});

## 도구
- Jest (기본)
- React Testing Library (컴포넌트)
- MSW (API 모킹)
- Playwright (E2E)
```

```
> @tester 이 함수에 대한 테스트 작성해줘
```

### 3. 리팩토링 전문가

```markdown
<!-- .claude/agents/refactorer.md -->
# 리팩토링 전문가

## 역할
기존 코드를 개선합니다.
동작은 유지하면서 품질을 높입니다.

## 리팩토링 원칙
- 작은 단위로 진행 (한 번에 하나씩)
- 각 단계에서 테스트 통과 확인
- 변경 이유를 명확히 설명

## 우선순위
1. 버그/보안 취약점 (가장 급함)
2. 중복 제거 (DRY)
3. 함수 분리 (20줄 이하)
4. 네이밍 개선 (의도 드러내기)
5. 복잡도 감소 (조건문 단순화)

## 리팩토링 절차
1. 현재 동작 이해 및 문서화
2. 테스트 존재 확인 (없으면 먼저 작성)
3. 작은 단위로 변경
4. 각 단계에서 테스트 실행
5. 커밋 메시지에 변경 이유 기록

## 주의사항
- 기능 추가는 리팩토링이 아님
- 한 번에 너무 많이 바꾸지 않기
- 동작 변경이 필요하면 먼저 확인 요청
```

```
> @refactorer 이 컴포넌트 리팩토링해줘
```

### 4. 보안 전문가

```markdown
<!-- .claude/agents/security.md -->
# 보안 전문가

## 역할
코드의 보안 취약점을 찾고 개선 방안을 제시합니다.
보안 컨설턴트 12년 경력입니다.
OWASP Top 10을 숙지하고 있습니다.

## 체크리스트
### 웹 보안
- SQL 인젝션 (파라미터화된 쿼리 확인)
- XSS (크로스 사이트 스크립팅)
- CSRF (토큰 검증)
- 인증/인가 취약점
- 민감 정보 노출 (로그, 에러 메시지)

### 인프라 보안
- 환경 변수 관리
- 시크릿 노출
- HTTPS 강제
- 헤더 설정 (CSP, HSTS)

### 데이터 보안
- 암호화 (at rest, in transit)
- 개인정보 처리
- 접근 권한 최소화

## 보고 형식
### 취약점 요약
| 심각도 | 유형 | 위치 | 영향 |

### 상세 분석
각 취약점에 대해:
1. 취약점 설명
2. 공격 시나리오
3. 영향 범위
4. 해결 방법 (코드 포함)
5. 예방 조치

## 심각도 기준
- Critical: 즉시 수정 필요 (데이터 유출 가능)
- High: 빠른 수정 필요 (권한 상승 가능)
- Medium: 계획적 수정 (제한적 영향)
- Low: 권장 사항 (베스트 프랙티스)
```

```
> @security 이 API 보안 검토해줘
```

### 5. 친절한 선생님

```markdown
<!-- .claude/agents/teacher.md -->
# 친절한 선생님

## 역할
코딩 개념을 설명하는 인내심 있는 선생님입니다.
프로그래밍 교육 경력 7년입니다.

## 교육 철학
- 모르는 것은 당연함
- 질문은 언제나 환영
- 실수는 배움의 기회

## 설명 스타일
1. 큰 그림부터 시작 (왜 필요한지)
2. 일상생활 비유 사용
3. 간단한 예시 먼저
4. 점점 복잡한 예시로
5. 이해했는지 확인 질문

## 예시 비유
- 변수: 이름표가 붙은 상자
- 함수: 레시피
- 클래스: 설계도
- 객체: 설계도로 만든 실제 물건
- 배열: 서랍장
- API: 식당 메뉴판

## 톤
친근하고 격려하는 어조.
"당연히 헷갈릴 수 있어요"
"좋은 질문이에요!"
"천천히 해도 괜찮아요"
```

```
> @teacher 재귀가 뭔지 설명해줘
```

### 실제 대화 예시: 선생님 Agent

```
사용자: @teacher 재귀가 뭔지 설명해줘

Claude (선생님 역할로):

좋은 질문이에요! 재귀는 처음엔 좀 헷갈릴 수 있는데,
천천히 설명해드릴게요.

## 재귀란?

**한 문장 설명:**
함수가 자기 자신을 호출하는 것이에요.

## 일상생활 비유 🪆

러시아 인형(마트료시카)을 아시나요?
큰 인형을 열면 작은 인형이 나오고,
그 인형을 열면 더 작은 인형이 나오고...
가장 작은 인형이 나올 때까지 계속되죠.

재귀도 똑같아요!
- 큰 문제를 열면 → 작은 문제가 나옴
- 작은 문제를 열면 → 더 작은 문제가 나옴
- 가장 작은 문제(기본 케이스)가 나올 때까지!

## 간단한 예시

5부터 1까지 카운트다운:

```javascript
function countdown(n) {
  // 기본 케이스: 더 이상 세지 않음
  if (n <= 0) {
    console.log("발사!");
    return;
  }

  console.log(n);
  countdown(n - 1);  // 자기 자신을 호출!
}

countdown(5);
// 출력: 5, 4, 3, 2, 1, 발사!
```

## 핵심 포인트 ⭐

1. **기본 케이스** (언제 멈출지)
   - 없으면 무한 반복됨! (스택 오버플로우)

2. **재귀 케이스** (자기 자신 호출)
   - 문제를 조금씩 작게 만듦

이해가 되셨나요?
더 자세히 설명해드릴 부분이 있으면 말씀해주세요!
```

---

## Skills 시스템 상세 설명

Skills는 특정 키워드에 자동으로 반응하는 기능입니다.

### Skills 폴더 구조

```
.claude/
└── skills/
    ├── pr-review.md      # PR 관련 요청에 반응
    ├── deploy.md         # 배포 관련 요청에 반응
    ├── debug.md          # 디버깅 관련 요청에 반응
    ├── explain.md        # 설명 요청에 반응
    └── performance.md    # 성능 관련 요청에 반응
```

### Skill 정의 상세

Skills는 frontmatter(메타데이터)를 사용해서 더 정교하게 설정할 수 있습니다:

```markdown
<!-- .claude/skills/pr-review.md -->
---
name: PR Review
description: Pull Request 리뷰 및 코드 분석
allowed-tools: [Read, Glob, Grep, Bash]
model: sonnet
---

# PR 리뷰 스킬

## 키워드 (이 단어가 있으면 자동 트리거)
- "PR 리뷰"
- "pull request 확인"
- "코드 리뷰"
- "review this PR"
- "머지 전에 봐줘"

## 동작 (순서대로 실행)
1. git diff로 변경사항 확인
2. 변경된 파일별 분석
3. 각 파일에서 이슈 식별
4. 심각도별 정리
5. 전체 요약 제공

## 출력 형식

### 요약
- 변경 파일 수: N개
- 추가/삭제 라인: +X / -Y
- 주요 변경 내용: [설명]

### 파일별 리뷰
#### [파일명]
- 변경 내용
- 발견된 이슈
- 개선 제안

### 전체 평가
[통과 ✅ / 수정 필요 🟡 / 재작업 필요 🔴]

### 권장 사항
- [다음 단계]
```

### Skill 사용하기

키워드를 포함해서 요청하면 자동으로 Skill이 활성화됩니다:

```
> 이 PR 리뷰해줘
```

"PR 리뷰" 키워드가 있으므로 자동으로 PR 리뷰 Skill이 동작합니다.

---

## 실용적인 Skills 예시

### 1. 디버깅 스킬

```markdown
<!-- .claude/skills/debug.md -->
---
name: Debugging Helper
description: 에러 분석 및 해결 방법 제시
---

# 디버깅 스킬

## 키워드
- "에러"
- "버그"
- "안 돼"
- "안 되"
- "오류"
- "고장"
- "실패"
- "작동 안"
- "TypeError"
- "Error:"

## 동작
1. 에러 메시지 전체 분석
2. 스택 트레이스에서 파일/라인 추출
3. 관련 코드 찾아서 읽기
4. 원인 파악
5. 해결 방법 3가지 이상 제시
6. 재발 방지 방안

## 질문 (정보 부족 시)
- 에러 메시지 전체를 보여주세요
- 어떤 상황에서 발생하나요?
- 최근에 변경한 것이 있나요?

## 출력 형식

### 에러 요약
[에러 유형]: [간단한 설명]

### 원인 분석
1. 직접 원인: [무엇이 에러를 발생시켰는지]
2. 근본 원인: [왜 그런 상황이 발생했는지]

### 해결 방법

**방법 1 (권장):**
```코드```
이유: [왜 이 방법이 좋은지]

**방법 2:**
```코드```
이유: [대안인 이유]

### 재발 방지
- [앞으로 이런 에러를 피하는 방법]
```

```
> 로그인할 때 에러 나
```

자동으로 디버깅 프로세스가 시작됩니다.

### 2. 배포 스킬

```markdown
<!-- .claude/skills/deploy.md -->
---
name: Deployment Checklist
description: 배포 전 체크리스트 및 실행
---

# 배포 스킬

## 키워드
- "배포"
- "deploy"
- "프로덕션"
- "production"
- "릴리즈"
- "release"
- "스테이징"
- "staging"

## 체크리스트 (반드시 확인)
- [ ] 테스트 통과? (`npm test`)
- [ ] 린트 통과? (`npm run lint`)
- [ ] 빌드 성공? (`npm run build`)
- [ ] 환경변수 설정 확인?
- [ ] 마이그레이션 필요?
- [ ] 롤백 계획 있음?

## 동작
1. 현재 브랜치 및 상태 확인
2. 체크리스트 항목 검증
3. 실패 항목 있으면 중단 및 안내
4. 모두 통과하면 배포 명령 제안
5. 배포 후 확인 방법 안내

## 출력 형식

### 배포 전 체크
| 항목 | 상태 | 비고 |
|------|------|------|
| 테스트 | ✅/❌ | [결과] |
| 린트 | ✅/❌ | [결과] |
| 빌드 | ✅/❌ | [결과] |

### 결정
[배포 가능 ✅ / 수정 필요 ❌]

### 다음 단계
1. [배포 명령 또는 수정 필요 사항]
```

```
> 스테이징에 배포해줘
```

자동으로 배포 체크리스트를 확인하고 진행합니다.

### 3. 성능 분석 스킬

```markdown
<!-- .claude/skills/performance.md -->
---
name: Performance Analysis
description: 성능 문제 분석 및 최적화 제안
---

# 성능 분석 스킬

## 키워드
- "느려"
- "성능"
- "최적화"
- "slow"
- "performance"
- "optimize"
- "로딩"
- "렌더링"
- "메모리"

## 분석 항목

### React/프론트엔드
- 불필요한 리렌더링
- 무거운 계산 (useMemo 필요)
- 이벤트 핸들러 재생성 (useCallback 필요)
- 큰 리스트 (virtualization 필요)
- 번들 크기

### Node.js/백엔드
- N+1 쿼리
- 동기 블로킹 코드
- 메모리 누수
- 연결 풀 부족
- 캐싱 부재

## 동작
1. 느린 부분 식별 (파일/함수)
2. 코드 분석
3. 병목 지점 찾기
4. 최적화 방법 우선순위 정렬
5. 적용 전/후 예상 효과

## 출력 형식

### 현재 상태
- 느린 부분: [위치]
- 추정 원인: [원인]

### 발견된 문제
1. [문제 1] - 영향도: 🔴/🟡/🟢
2. [문제 2] - 영향도: 🔴/🟡/🟢

### 최적화 제안 (우선순위 순)

**1. [가장 효과적인 최적화]**
- 변경 전: ```코드```
- 변경 후: ```코드```
- 예상 효과: [N% 개선]

### 측정 방법
- [어떻게 개선을 확인할 수 있는지]
```

```
> 이 페이지 왜 이렇게 느려?
```

자동으로 성능 분석을 시작합니다.

### 4. 코드 설명 스킬

```markdown
<!-- .claude/skills/explain.md -->
---
name: Code Explainer
description: 코드를 쉽게 설명
---

# 코드 설명 스킬

## 키워드
- "설명해"
- "이게 뭐야"
- "어떻게 작동"
- "무슨 뜻"
- "이해가 안"
- "explain"
- "what does"

## 동작
1. 코드 전체 목적 파악
2. 주요 부분 식별
3. 단계별로 분해
4. 비유로 설명
5. 요약 제공

## 설명 레벨
- 초급: 비유 많이, 용어 최소화
- 중급: 기술 용어 사용, 맥락 설명
- 고급: 깊은 기술 분석

## 출력 형식

### 한 줄 요약
[이 코드가 하는 일]

### 비유로 설명
[일상생활에 비유하면...]

### 단계별 설명
1. **[1단계]**: [설명]
2. **[2단계]**: [설명]
...

### 핵심 개념
- [개념 1]: [설명]
- [개념 2]: [설명]

### 더 알아보면 좋은 것
- [관련 개념 1]
- [관련 개념 2]
```

```
> 이 함수 어떻게 작동해?
```

---

## Agents vs Skills 상세 비교

### 언제 뭘 쓸까? - 상세 표

```
┌─────────────────────────────────────────────────────────────────┐
│                    Agents vs Skills                              │
├──────────────────────────────┬──────────────────────────────────┤
│           Agents             │           Skills                  │
├──────────────────────────────┼──────────────────────────────────┤
│ @로 명시적 호출               │ 키워드로 자동 활성화              │
│ 예: @security                │ 예: "리뷰해줘" 언급               │
├──────────────────────────────┼──────────────────────────────────┤
│ 역할/관점 정의               │ 프로세스/절차 정의               │
│ "시니어 개발자처럼"           │ "이 체크리스트대로"              │
├──────────────────────────────┼──────────────────────────────────┤
│ 전문가 페르소나              │ 자동화된 워크플로우              │
│ 일관된 톤과 관점             │ 일관된 프로세스                  │
├──────────────────────────────┼──────────────────────────────────┤
│ 대화 전체에 영향             │ 해당 요청에만 영향               │
├──────────────────────────────┼──────────────────────────────────┤
│ 사용자가 선택               │ 자동으로 감지                    │
└──────────────────────────────┴──────────────────────────────────┘
```

### 조합해서 사용하기

```
> @security 이 PR 리뷰해줘
```

- `@security`: 보안 전문가 관점 (Agent)
- "PR 리뷰": PR 리뷰 프로세스 (Skill)

보안 전문가 관점에서 PR 리뷰 프로세스를 실행합니다.

### 실제 대화 예시: Agent + Skill 조합

```
사용자: @security 이 코드 리뷰해줘

Claude (보안 전문가 + 코드 리뷰 스킬):

보안 전문가로서 이 코드를 리뷰하겠습니다.

## 보안 관점 코드 리뷰

### 🔴 Critical Issues

**1. SQL 인젝션 취약점** (line 42)
```javascript
// 현재 - 위험!
const query = `SELECT * FROM users WHERE id = '${userId}'`;

// 수정 필요
const query = 'SELECT * FROM users WHERE id = $1';
await db.query(query, [userId]);
```
공격 시나리오: 악의적인 사용자가 `'; DROP TABLE users; --`를 입력

**2. XSS 취약점** (line 78)
```javascript
// 현재 - 위험!
element.innerHTML = userInput;

// 수정 필요
element.textContent = userInput;
// 또는 DOMPurify.sanitize(userInput)
```

### 🟡 Medium Issues

**1. 민감정보 로깅** (line 23)
비밀번호가 로그에 포함됨

### ✅ 잘된 점
- HTTPS 강제 사용
- 비밀번호 해싱 적용

### 권장 조치
1. 즉시: SQL 인젝션 수정 (1시간)
2. 빠른: XSS 수정 (30분)
3. 계획: 로깅 정책 검토 (이번 주)
```

---

## 팀에서 공유하기

### Git에 커밋

```
my-project/
├── .claude/
│   ├── agents/       # 팀 공통 Agents
│   │   ├── code-reviewer.md
│   │   ├── doc-writer.md
│   │   └── security.md
│   └── skills/       # 팀 공통 Skills
│       ├── pr-review.md
│       ├── deploy.md
│       └── debug.md
├── CLAUDE.md
└── src/
```

### 팀 표준 문서화

```markdown
# 팀 Agents & Skills

## Agents (역할)
| Agent | 설명 | 사용 예시 |
|-------|------|----------|
| `@code-reviewer` | 시니어 코드 리뷰어 | @code-reviewer 이 함수 봐줘 |
| `@doc-writer` | 테크니컬 라이터 | @doc-writer API 문서 작성해줘 |
| `@security` | 보안 전문가 | @security 취약점 검사해줘 |
| `@teacher` | 친절한 선생님 | @teacher 재귀 설명해줘 |

## Skills (자동 트리거)
| 키워드 | 동작 |
|--------|------|
| "PR 리뷰", "코드 리뷰" | PR 리뷰 프로세스 실행 |
| "배포", "deploy" | 배포 체크리스트 확인 |
| "에러", "버그" | 디버깅 프로세스 실행 |
| "느려", "성능" | 성능 분석 실행 |

## 조합 사용
더 전문적인 결과를 원하면 Agent + Skill을 조합하세요:
- `@security 이 PR 리뷰해줘` - 보안 관점 리뷰
- `@teacher 이 에러 설명해줘` - 쉬운 에러 설명
```

---

## 🎯 미니 퀴즈: Agents vs Skills

**질문**: "모든 PR에 대해 보안 체크리스트를 자동으로 실행"하려면?

A) Agent만 사용
B) Skill만 사용
C) Agent + Skill 조합

<details>
<summary>정답 보기</summary>

**정답: B) Skill만 사용**

- "PR" 또는 "리뷰" 키워드에 반응하는 Skill을 만들면 됨
- 일관된 프로세스(체크리스트)가 목표이므로 Skill이 적합
- Agent는 특정 관점이 필요할 때 추가로 조합

단, "보안 전문가 관점에서 리뷰"가 필요하면:
Agent(@security) + Skill(PR 리뷰) 조합이 적합

</details>

---

## 📝 실습 과제

### 실습 1: 첫 Agent 만들기 (초급)

1. agents 폴더 만들기: `mkdir -p .claude/agents`
2. `.claude/agents/teacher.md` 만들기:

```markdown
# 인내심 있는 선생님

## 역할
당신은 코딩 개념을 설명하는 인내심 있는 선생님입니다.
항상 쉬운 말로 예시를 들어 설명합니다.

## 스타일
- 큰 그림부터 시작
- 일상생활 비유 사용
- 간단한 코드 예시 제공
- 이해했는지 확인

## 톤
친근하고 격려하는 어조
```

3. 사용하기: `> @teacher 재귀가 뭔지 설명해줘`

### 실습 2: 첫 Skill 만들기 (초급)

1. skills 폴더 만들기: `mkdir -p .claude/skills`
2. `.claude/skills/explain.md` 만들기:

```markdown
# 코드 설명 스킬

## 키워드
"설명해", "이게 뭐야", "어떻게 작동해"

## 프로세스
1. 코드 읽기
2. 주요 목적 파악
3. 단계별로 분해
4. 간단한 요약 제공
```

3. 테스트: `> 이 함수가 무엇을 하는지 설명해 주세요`

### 실습 3: Agent + Skill 조합 (중급)

```
> @teacher 이 함수가 무엇을 하는지 설명해 주세요
```

선생님 에이전트의 성격 + 설명 스킬의 프로세스 = 양쪽의 장점!

### 실습 4: 프로젝트 전용 Agents 만들기 (고급)

여러분 프로젝트에 맞는 전문가 Agents 3개를 만들어보세요:
- 프로젝트 기술 스택 전문가
- 팀 코딩 컨벤션 전문가
- 도메인 지식 전문가

---

## 🏆 도전 과제

### 도전 1: 완전한 리뷰 시스템

다음을 모두 포함하는 코드 리뷰 시스템을 만들어보세요:
- Agent: 시니어 리뷰어, 보안 전문가, 성능 전문가
- Skill: PR 리뷰 자동 트리거
- 조합: 상황에 따라 다른 관점 적용

### 도전 2: 온보딩 시스템

새 팀원을 위한 시스템:
- Agent: 친절한 멘토
- Skill: 질문 유형별 자동 안내

### 도전 3: 풀스택 전문가 팀

프론트엔드, 백엔드, DevOps 전문가 Agents를 만들어서 협업시켜보세요.

---

## 문제가 발생하면?

### 문제: Agent를 찾을 수 없습니다

**가능한 원인:**
1. 파일이 `.claude/agents/` 폴더에 없음
2. 파일이 `.md`로 끝나지 않음
3. 잘못된 이름 사용 (.md 없는 파일명이 이름)

**해결 방법:**
- 폴더 확인: `ls -la .claude/agents/`
- `@teacher` 에이전트는 `teacher.md` 파일 필요
- 대소문자 구분: `@Teacher`와 `@teacher`는 다름

### 문제: Skill이 트리거되지 않습니다

**가능한 원인:**
1. 키워드가 요청과 정확히 일치하지 않음
2. 스킬 파일 형식이 잘못됨
3. skills 폴더가 잘못된 위치에 있음

**해결 방법:**
- 키워드 철자 확인
- skills 폴더는 `.claude/skills/`여야 함
- 키워드를 정확히 포함해서 테스트

### 문제: Agent 성격이 일관되지 않습니다

**가능한 원인:**
1. 에이전트 정의가 너무 모호함
2. 상충되는 지시사항
3. 긴 대화에서 컨텍스트가 희석됨

**해결 방법:**
- 에이전트 정의를 더 구체적으로
- 모순되는 지시사항 제거
- 필요하면 `/clear`로 컨텍스트 리셋

---

## 자주 하는 실수

1. **Agent를 너무 포괄적으로 만들기**
   - 나쁨: "당신은 도움이 되는 어시스턴트입니다"
   - 좋음: "당신은 타입 안전성에 집중하는 TypeScript 전문가입니다"

2. **Skill 키워드를 너무 모호하게**
   - 나쁨: "도와줘" (모든 것에 트리거)
   - 좋음: "스테이징에 배포" (구체적)

3. **Agent와 Command 혼동**
   - Commands: 저장된 프롬프트 실행
   - Agents: Claude에게 지속적인 성격 부여
   - 관점이 중요할 때 Agent 사용

4. **팀과 공유하는 것 잊기**
   - `.claude/agents/`와 `.claude/skills/`를 git에 커밋
   - README에 사용법 문서화

5. **공유 전에 테스트하지 않기**
   - 공유 전에 에이전트와 스킬 테스트
   - 엣지 케이스에서 문제가 드러나는 경우가 많음

---

## 용어 사전

| 용어 | 설명 |
|------|------|
| **Agent** | Claude에게 특정 역할/관점을 부여하는 설정 |
| **Skill** | 특정 키워드에 자동으로 반응하는 프로세스 |
| **Matcher** | Skill이 반응할 키워드 패턴 |
| **Frontmatter** | 마크다운 파일 상단의 메타데이터 (---로 감싸진 부분) |
| **페르소나** | Agent가 가지는 역할과 성격 |

---

## 정리

이번 챕터에서 배운 것:
- [x] Agents로 전문 역할 정의
- [x] Skills로 키워드 기반 자동화
- [x] Agents와 Skills의 차이
- [x] 실용적인 활용 예시
- [x] Agent + Skill 조합
- [x] 팀에서 공유하기

**핵심 포인트**: Agents는 "누구처럼", Skills는 "무엇을"에 집중합니다.

---

## 📚 더 배우고 싶다면: 추천 자료

Agents와 Skills에 대해 더 깊이 알고 싶다면 다음 자료들을 참고하세요.

### 공식 문서

| 자료 | 설명 | 링크 |
|------|------|------|
| **Claude Code 공식 문서** | Agents, Skills 설정 방법 상세 가이드 | [docs.anthropic.com/claude-code](https://docs.anthropic.com/en/docs/claude-code) |
| **Claude Code GitHub** | 소스 코드와 예시 확인 | [github.com/anthropics/claude-code](https://github.com/anthropics/claude-code) |
| **Anthropic Cookbook** | 다양한 활용 예시와 패턴 | [github.com/anthropics/anthropic-cookbook](https://github.com/anthropics/anthropic-cookbook) |

### AI 에이전트 이해하기

| 자료 | 설명 | 링크 |
|------|------|------|
| **Building Effective Agents (Anthropic)** | AI 에이전트 설계 원칙 (영문) | [anthropic.com/research/building-effective-agents](https://www.anthropic.com/research/building-effective-agents) |
| **AI Agents Explained** | 에이전트 개념을 쉽게 설명 (영문) | YouTube 검색: "AI Agents Explained" |

### 커뮤니티와 예시

| 자료 | 설명 | 링크 |
|------|------|------|
| **Awesome Claude Code** | 커뮤니티가 만든 Agents/Skills 모음 | GitHub에서 "awesome claude code" 검색 |
| **Claude Code Discord** | 질문하고 아이디어 공유 | Discord 커뮤니티 참여 |
| **r/ClaudeAI** | Reddit 커뮤니티 | [reddit.com/r/ClaudeAI](https://reddit.com/r/ClaudeAI) |

### 관련 개념 학습

| 주제 | 왜 알면 좋은가 | 추천 자료 |
|------|--------------|----------|
| **프롬프트 엔지니어링** | Agent 프롬프트 작성 실력 향상 | Anthropic Prompt Engineering Guide |
| **시스템 디자인** | 복잡한 Agent 시스템 설계 | "System Design Primer" (GitHub) |
| **자동화 패턴** | Skill 워크플로우 설계 | "Zapier University" (무료 강좌) |

> 💡 **비전공자 팁**
>
> 처음에는 공식 문서가 어려울 수 있어요.
> 먼저 이 커리큘럼을 따라 실습하고, 나중에 공식 문서를 읽으면 "아, 이게 이런 거였구나!" 하게 됩니다.
> Reddit이나 Discord에서 다른 사람들이 만든 예시를 보는 것도 좋은 학습 방법이에요.

```
┌─────────────────────────────────────────────────────────────────┐
│                    학습 순서 추천                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1️⃣ 이 챕터 실습 완료                                           │
│     ↓                                                           │
│  2️⃣ 커뮤니티 예시 구경하기 (Reddit, GitHub)                     │
│     ↓                                                           │
│  3️⃣ 나만의 Agent/Skill 만들어보기                               │
│     ↓                                                           │
│  4️⃣ 막히면 공식 문서 참고                                       │
│     ↓                                                           │
│  5️⃣ Anthropic 블로그 읽기 (심화)                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 다음 챕터 미리보기

Chapter 25에서는 **MCP 연동**을 배웁니다:
- Claude가 데이터베이스에 직접 접근
- GitHub, Slack 등 외부 서비스 연동
- 맞춤형 도구 만들기

Agents와 Skills가 Claude의 "성격과 행동"을 정의했다면, MCP는 Claude의 "능력"을 확장합니다!

[Chapter 25: MCP 연동](../Chapter25-MCP-Integration/README.ko.md)으로 넘어가시기 바랍니다.

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [Claude Code 공식 문서](https://docs.anthropic.com/en/docs/claude-code) - Agents 및 Skills 가이드
- [Building Effective Agents](https://www.anthropic.com/research/building-effective-agents) - AI 에이전트 설계 가이드
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook) - 다양한 활용 예시

**영상 자료:**
- [AI Agents Tutorial (YouTube)](https://www.youtube.com/results?search_query=AI+agents+tutorial+beginners) - AI 에이전트 튜토리얼
- [Prompt Engineering (YouTube)](https://www.youtube.com/results?search_query=prompt+engineering+tutorial+2024) - 프롬프트 엔지니어링
- [AI Role-Playing Techniques (YouTube)](https://www.youtube.com/results?search_query=AI+role+playing+techniques) - AI 역할 부여 기법

**읽을거리:**
- [Prompt Engineering Guide](https://www.promptingguide.ai/) - 프롬프트 작성 가이드
- [Awesome Claude Code](https://github.com/anthropics/claude-code) - Claude Code 활용 예시
- [AI Persona Design](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering) - Anthropic 프롬프트 가이드

**커뮤니티:**
- [Claude Code Discussions](https://github.com/anthropics/claude-code/discussions) - GitHub 토론
- [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/) - Reddit 커뮤니티

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
