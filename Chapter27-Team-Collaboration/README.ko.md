# Chapter 27: 팀 협업

[English](./README.md) | **한국어**

---

## 💬 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이전 챕터와의 연결

축하합니다! 이것이 바이브코딩 커리큘럼의 **마지막 챕터**입니다!

지금까지 배운 모든 것을 되돌아보면:

| 파트 | 챕터 | 배운 내용 |
|------|------|----------|
| **Part 1: 시작** | 1-5 | 설치, 기본 사용법, 터미널 |
| **Part 2: 핵심** | 6-11 | 프로젝트 구조, 프롬프팅, Git |
| **Part 3: 실전 I** | 12-16 | 웹사이트, 배포, 게임 |
| **Part 4: 실전 II** | 17-20 | CLI 도구, 챗봇, 풀스택 앱 |
| **Part 5: 고급** | 21-27 | 아키텍처, 설정, MCP, CI/CD, **팀 협업** |

이전 챕터들에서 배운 모든 기능들:
- **CLAUDE.md** (Chapter 22): 프로젝트 규칙 정의
- **Hooks & Commands** (Chapter 23): 자동화 워크플로우
- **Agents & Skills** (Chapter 24): 역할과 스킬 정의
- **MCP** (Chapter 25): 외부 서비스 연동
- **CI/CD** ([Chapter 26](../Chapter26-CI-CD-Automation/README.ko.md)): 자동화 파이프라인

이 모든 것을 **팀 전체가 활용**할 수 있도록 표준화하는 방법을 배워봅시다!

---

## 이 챕터에서 배우는 것

- 팀에서 Claude Code 표준화하기
- 설정과 프롬프트 공유
- 협업 워크플로우 구축
- 새 팀원 온보딩 자동화
- 팀 규칙 문서화
- **전체 커리큘럼 요약**
- **다음 학습 단계 안내**

---

## 왜 팀 협업이 중요한가요?

### 실제 상황: 혼자 vs 팀

**혼자 사용할 때:**
```
나: "이 코드 리뷰해줘"
Claude: (각자의 스타일대로 리뷰)
```

**팀에서 사용할 때:**
```
팀원 A: "/review" → 표준화된 리뷰 프로세스
팀원 B: "/review" → 동일한 리뷰 프로세스
팀원 C: "/review" → 동일한 리뷰 프로세스

결과: 일관된 코드 품질!
```

### 표준화가 없으면 생기는 문제

```
┌─────────────────────────────────────────────────────────────────────┐
│                     표준화 없는 팀의 혼란                             │
└─────────────────────────────────────────────────────────────────────┘

팀원 A: "Claude야, 코드 리뷰해줘"
        → 자기만의 프롬프트 사용
        → 자기만의 스타일로 피드백

팀원 B: "이 코드 괜찮아?"
        → 다른 프롬프트 사용
        → 다른 스타일로 피드백

팀원 C: "@reviewer 리뷰 부탁"
        → 또 다른 프롬프트
        → 또 다른 스타일

새 팀원: "...어떻게 해야 하지? 😰"

결과:
- 일관성 없는 코드 품질
- 새 팀원 온보딩 어려움
- 팀원마다 다른 경험
- 지식 공유 실패
```

### 표준화하면 달라지는 것

```
┌─────────────────────────────────────────────────────────────────────┐
│                     표준화된 팀의 협업                               │
└─────────────────────────────────────────────────────────────────────┘

팀원 A: "/review"
팀원 B: "/review"
팀원 C: "/review"
새 팀원: "/review" (첫날부터 사용 가능!)

모두 동일한:
✓ 리뷰 관점
✓ 피드백 스타일
✓ 품질 기준
✓ 문서화 방식

결과:
- 일관된 코드 품질
- 빠른 온보딩
- 팀 전체 생산성 향상
- 지식 자동 공유
```

---

## 왜 필요한가요?

**실제 상황**: 팀이 Claude Code를 좋아하지만, 모두 다르게 사용합니다. 한 사람은 특정 명령어를 쓰고, 다른 사람은 다른 설정을 씁니다. 새 팀원들이 혼란스러워합니다. 코드 리뷰가 일관성이 없습니다.

팀을 위해 Claude Code를 표준화하면 모두가 같은 방식으로 일하고, 새 멤버를 온보딩하기 쉬워집니다.

### 쉬운 비유: 팀 플레이북

팀 설정을 스포츠 팀 플레이북처럼 생각해보시기 바랍니다:
- 모두가 같은 플레이를 알고 있습니다 (commands)
- 모두가 같은 용어를 씁니다 (agents)
- 새 선수가 빨리 배울 수 있습니다 (onboarding)

### 또 다른 비유: 맥도날드 매뉴얼

전 세계 맥도날드에서 같은 맛의 빅맥을 먹을 수 있는 이유:
- **표준화된 레시피** = CLAUDE.md (프로젝트 규칙)
- **표준화된 절차** = Commands (작업 프로세스)
- **역할 정의** = Agents (전문가 역할)
- **교육 매뉴얼** = 온보딩 문서

팀의 Claude Code 사용도 이렇게 표준화하면, **누가 사용해도 일관된 결과**를 얻을 수 있습니다!

---

## 💡 비전공자 팁

> **"표준화"가 왜 중요해요?**
>
> 요리를 예로 들면:
>
> **표준화 없이:**
> - 셰프 A: 소금 한 꼬집
> - 셰프 B: 소금 두 숟가락
> - 셰프 C: 소금... 넣었나?
> → 매번 다른 맛!
>
> **표준화하면:**
> - 모든 셰프: 레시피대로 소금 5g
> → 항상 같은 맛!
>
> 코드도 마찬가지입니다. 팀 전체가 같은 "레시피"를 따르면:
> - 코드 스타일이 일관됨
> - 리뷰 품질이 균일함
> - 새 팀원도 쉽게 적응함

---

## 왜 팀 협업을 알아야 할까?

혼자 쓸 때와 팀에서 쓸 때는 다릅니다:

- **일관성**: 팀원마다 다른 방식 → 혼란
- **온보딩**: 새 팀원이 바로 시작할 수 있어야 함
- **품질**: 팀 전체의 코드 품질 향상

### 개인 사용 vs 팀 사용 비교

| 측면 | 개인 사용 | 팀 사용 |
|------|----------|--------|
| 설정 | 개인 취향대로 | 팀 표준 필요 |
| Commands | 자기만 알면 됨 | 모두가 사용 가능해야 함 |
| Agents | 자유롭게 | 역할 명확히 정의 |
| 문서화 | 선택사항 | 필수! |
| 온보딩 | 없음 | 매우 중요 |

---

## 팀 표준화

### 공유해야 할 것들

```
my-project/
├── .claude/
│   ├── settings.json    # 팀 설정
│   ├── commands/        # 팀 Commands
│   │   ├── pr.md
│   │   ├── review.md
│   │   ├── onboarding.md
│   │   └── issue-start.md
│   ├── agents/          # 팀 Agents
│   │   ├── senior-reviewer.md
│   │   ├── junior-helper.md
│   │   └── security-expert.md
│   └── skills/          # 팀 Skills
│       ├── deploy.md
│       └── debug.md
├── CLAUDE.md            # 프로젝트 규칙
├── .github/
│   └── workflows/       # CI/CD (Chapter 26)
└── src/
```

이 폴더들을 Git에 커밋하면 팀 전체가 같은 환경에서 작업합니다.

### 팀 설정 파일 예시

```json
// .claude/settings.json
{
  "project": {
    "name": "Team Project",
    "team": "Frontend Team"
  },
  "preferences": {
    "language": "ko",
    "codeStyle": "team-standard"
  },
  "permissions": {
    "autoApprove": [
      "Read",
      "Glob",
      "Grep",
      "Bash(npm test)",
      "Bash(npm run lint)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force)"
    ]
  },
  "hooks": {
    "preCommit": "npm run lint && npm test"
  }
}
```

### CLAUDE.md 팀 버전

```markdown
# 프로젝트: My Team App

## 팀 정보
- 팀명: Frontend Team
- 연락처: #frontend-dev (Slack)
- 팀 리더: @team-lead

## 기술 스택
- React 18 + TypeScript
- Tailwind CSS
- Jest + React Testing Library
- GitHub Actions

## 코딩 컨벤션

### 네이밍 규칙
| 대상 | 규칙 | 예시 |
|------|------|------|
| 컴포넌트 | PascalCase | `UserProfile.tsx` |
| 훅 | camelCase, use 접두사 | `useAuth.ts` |
| 파일 | kebab-case | `user-profile.tsx` |
| 테스트 | *.test.tsx | `user-profile.test.tsx` |
| 상수 | SCREAMING_SNAKE_CASE | `MAX_RETRY_COUNT` |

### 코드 스타일
- 함수형 컴포넌트만 사용
- Props는 interface로 정의
- 에러 처리 필수 (try-catch)
- 주석은 한국어로

## Git 규칙

### 브랜치 전략
- `main`: 프로덕션 배포
- `develop`: 개발 통합
- `feature/*`: 기능 개발
- `fix/*`: 버그 수정
- `hotfix/*`: 긴급 수정

### 커밋 메시지
Conventional Commits 사용:
- `feat:` 새 기능
- `fix:` 버그 수정
- `docs:` 문서
- `refactor:` 리팩토링
- `test:` 테스트
- `chore:` 기타

예시: `feat(auth): Add social login support`

### PR 규칙
- PR 필수: main 브랜치 직접 푸시 금지
- 리뷰어: 최소 1명
- CI 통과 필수

## 자주 쓰는 명령어

| 명령어 | 설명 |
|--------|------|
| `npm run dev` | 개발 서버 |
| `npm test` | 테스트 |
| `npm run lint` | 린트 |
| `npm run build` | 빌드 |
| `npm run typecheck` | 타입 체크 |

## 중요한 폴더

| 폴더 | 설명 |
|------|------|
| `src/components/` | 재사용 컴포넌트 |
| `src/features/` | 기능별 모듈 |
| `src/hooks/` | 커스텀 훅 |
| `src/utils/` | 유틸리티 함수 |
| `src/types/` | TypeScript 타입 |
| `src/api/` | API 호출 |

## Claude Code 사용 규칙

### 필수 확인 사항
- 코드 변경 전 테스트 실행
- 보안 관련 코드 수정 시 @security-expert 호출
- 대규모 리팩토링 전 팀 논의

### 금지 사항
- 프로덕션 DB 직접 조작
- 환경 변수 파일(.env) 수정
- package.json 임의 수정
```

---

## 🔥 프로 팁

> **팀 CLAUDE.md 유지보수 팁**
>
> 1. **단일 관리자 지정**: 한 명이 CLAUDE.md 관리 담당
> 2. **정기 업데이트**: 스프린트마다 검토 및 업데이트
> 3. **버전 관리**: 변경 이력 기록
> 4. **피드백 수집**: 팀원들의 개선 제안 반영
>
> ```markdown
> ## 변경 이력
> | 날짜 | 내용 | 작성자 |
> |------|------|--------|
> | 2024-01-15 | 초기 작성 | @kim |
> | 2024-02-01 | Git 규칙 추가 | @lee |
> | 2024-02-15 | 테스트 규칙 업데이트 | @park |
> ```

---

## 팀 Commands

### 표준 워크플로우

```markdown
<!-- .claude/commands/pr.md -->
# PR 생성 워크플로우

## 사전 체크
자동으로 다음을 확인합니다:
- [ ] 테스트 통과: $(npm test)
- [ ] 린트 통과: $(npm run lint)
- [ ] 타입 체크: $(npm run typecheck)
- [ ] 빌드 성공: $(npm run build)

## PR 제목 형식
```
<type>(<scope>): <description>

예시:
feat(auth): Add social login
fix(cart): Fix quantity update bug
```

## PR 본문 템플릿
```markdown
### 변경 내용
- 무엇을 변경했는지

### 변경 이유
- 왜 이 변경이 필요한지

### 테스트 방법
1. 단계별 테스트 방법

### 스크린샷
(UI 변경 시 첨부)

### 체크리스트
- [ ] 테스트 추가/수정
- [ ] 문서 업데이트
- [ ] 마이그레이션 필요 여부 확인
```

## 실행 방법
```
> /pr
```
```

### 코드 리뷰 Command

```markdown
<!-- .claude/commands/review.md -->
# 코드 리뷰

## 리뷰 범위
변경된 파일:
$(git diff --name-only HEAD~1)

## 리뷰 관점

### 1. 기능성
- 요구사항을 충족하는가?
- 엣지 케이스가 처리되는가?

### 2. 코드 품질
- 팀 컨벤션을 따르는가?
- 중복 코드가 있는가?
- 함수/변수 이름이 명확한가?

### 3. 성능
- 불필요한 연산이 있는가?
- 메모리 누수 가능성이 있는가?

### 4. 보안
- 사용자 입력 검증이 되는가?
- 민감 정보 노출이 없는가?

### 5. 테스트
- 테스트가 충분한가?
- 테스트 커버리지는 적절한가?

## 피드백 형식
```
[심각도] 파일:라인 - 설명

예시:
[필수] auth.ts:45 - 비밀번호 검증 로직 필요
[권장] user.ts:23 - 변수명을 더 명확하게
[참고] api.ts:89 - 이런 방법도 있어요
```

## 실행 방법
```
> /review
```
```

### 온보딩 도우미

```markdown
<!-- .claude/commands/onboarding.md -->
# 새 팀원 온보딩

새로 오신 분이시군요! 환영합니다.

## 1. 환경 설정
```bash
# 저장소 클론
git clone [repo-url]
cd [project-name]

# 의존성 설치
npm install

# 환경변수 설정
cp .env.example .env.local
# .env.local 파일 수정 (팀에 문의)

# 개발 서버 실행
npm run dev
```

## 2. 프로젝트 구조
$(tree -L 2 src/)

## 3. 필수 읽기 자료
- [ ] CLAUDE.md: 프로젝트 규칙
- [ ] README.md: 프로젝트 소개
- [ ] .claude/commands/: 사용 가능한 명령어

## 4. 사용 가능한 Commands
| 명령어 | 설명 |
|--------|------|
| /pr | PR 생성 |
| /review | 코드 리뷰 |
| /issue-start <번호> | 이슈 시작 |
| /debug | 디버깅 도움 |

## 5. 사용 가능한 Agents
| Agent | 설명 |
|-------|------|
| @senior-reviewer | 시니어 관점 리뷰 |
| @junior-helper | 주니어 도우미 |
| @security-expert | 보안 전문가 |

## 6. 첫 번째 작업
1. `good-first-issue` 라벨이 붙은 이슈 선택
2. `/issue-start <번호>`로 시작
3. 구현 후 `/pr`로 PR 생성

## 7. 질문하기
- Slack: #frontend-dev
- 팀 미팅: 매주 월요일 10시

궁금한 게 있으면 언제든 물어보세요!
```

```
> /onboarding
```

### 이슈 시작 Command

```markdown
<!-- .claude/commands/issue-start.md -->
# 이슈 시작

이슈 번호: $ARGUMENTS

## 1. 이슈 확인
$(gh issue view $ARGUMENTS)

## 2. 브랜치 생성
```bash
git checkout develop
git pull origin develop
git checkout -b feature/$ARGUMENTS
```

## 3. 작업 계획
이슈 내용을 분석하고 작업 계획을 세워주세요:

### 분석
- 요구사항 정리
- 영향 범위 파악

### 작업 항목
- [ ] 할 일 1
- [ ] 할 일 2
- [ ] 테스트 작성
- [ ] 문서 업데이트

### 예상 소요 시간
- 예상: X시간

## 4. 시작
계획이 확정되면 구현을 시작하세요.

완료 후 `/pr`로 PR을 생성하세요.
```

```
> /issue-start 42
```

---

## ⚠️ 주의사항

> **팀 설정 공유 시 주의점**
>
> 1. **민감 정보 제외**: API 키, 비밀번호 등은 .gitignore에 추가
>    ```
>    # .gitignore
>    .env
>    .env.local
>    *.secret
>    ```
>
> 2. **개인 설정 분리**: 팀 설정과 개인 설정 구분
>    - 팀 설정: `.claude/` (커밋)
>    - 개인 설정: `~/.claude/` (커밋 안 함)
>
> 3. **버전 충돌 주의**: 여러 명이 동시에 수정 시 충돌 가능
>    - 한 명이 관리하거나
>    - PR로 변경사항 관리

---

## 팀 Agents

### 시니어 리뷰어

```markdown
<!-- .claude/agents/senior-reviewer.md -->
# 시니어 개발자

## 역할
10년차 시니어 개발자입니다.
코드 리뷰를 꼼꼼하게 하되, 건설적으로 피드백합니다.

## 리뷰 관점

### 아키텍처
- 설계가 적절한가
- 확장성은 있는가
- 의존성 관리는 적절한가

### 성능
- 불필요한 연산이 있는가
- N+1 쿼리 문제는 없는가
- 메모리 사용은 적절한가

### 유지보수
- 다른 사람이 이해하기 쉬운가
- 코드 중복은 없는가
- 테스트하기 쉬운 구조인가

### 테스트
- 충분히 테스트되었는가
- 엣지 케이스가 커버되는가
- 테스트가 독립적인가

## 피드백 스타일
- 문제점과 개선 방향을 함께 제시
- "이게 나쁘다"보다 "이렇게 하면 더 좋다"
- 잘된 부분도 언급
- 학습 자료 링크 첨부

## 사용 예시
```
@senior-reviewer 이 PR 봐줘
@senior-reviewer 이 아키텍처 설계 검토해줘
```
```

### 주니어 도우미

```markdown
<!-- .claude/agents/junior-helper.md -->
# 주니어 도우미

## 역할
주니어 개발자를 돕습니다.
어려운 개념을 쉽게 설명하고, 단계별로 안내합니다.

## 원칙
- 모르는 게 당연하다는 태도
- 예시를 많이 사용
- 왜 그런지 이유 설명
- 질문하도록 격려
- 용어 설명 포함

## 설명 스타일
1. 핵심 개념 먼저 (한 문장으로)
2. 쉬운 비유
3. 간단한 예시 코드
4. 실제 프로젝트에 적용
5. 연습 문제 제안

## 금지 사항
- 어려운 전문 용어만 나열
- "이건 기본인데..."라는 표현
- 답만 알려주기 (과정 설명 없이)

## 사용 예시
```
@junior-helper React의 useEffect가 뭐야?
@junior-helper 이 에러 메시지 무슨 뜻이야?
@junior-helper 테스트 작성 방법 알려줘
```
```

### 보안 전문가

```markdown
<!-- .claude/agents/security-expert.md -->
# 보안 전문가

## 역할
보안 관점에서 코드를 검토합니다.
잠재적 취약점을 찾고 해결 방안을 제시합니다.

## 검토 항목

### 인증/인가
- 인증 로직이 안전한가
- 권한 검사가 적절한가
- 세션 관리는 안전한가

### 입력 검증
- 사용자 입력이 검증되는가
- SQL 인젝션 방지
- XSS 방지
- CSRF 방지

### 데이터 보호
- 민감 데이터가 암호화되는가
- 로그에 민감 정보가 노출되는가
- 에러 메시지에 내부 정보가 있는가

### 의존성
- 취약점이 있는 패키지는 없는가
- 최신 보안 패치가 적용되었는가

## 보고 형식
```
[심각도: 높음/중간/낮음]
위치: 파일:라인
문제: 설명
영향: 무슨 일이 일어날 수 있는가
해결: 어떻게 수정해야 하는가
참고: OWASP 등 참고 자료
```

## 사용 예시
```
@security-expert 이 인증 코드 검토해줘
@security-expert 이 API 보안 점검해줘
```
```

---

## 협업 워크플로우

### 이슈 → 구현 → 리뷰 → 머지

```
┌─────────────────────────────────────────────────────────────────┐
│                        팀 워크플로우                              │
└─────────────────────────────────────────────────────────────────┘

 1. 이슈 할당
      │
      ▼
┌──────────────┐
│ /issue-start │  ← 브랜치 생성, 이슈 분석
│   #42        │
└──────┬───────┘
       │
       ▼
 2. 구현
      │ Claude와 대화하며 개발
      │
      ▼
┌──────────────┐
│ /pr          │  ← PR 생성, 자동 테스트
└──────┬───────┘
       │
       ▼
 3. 리뷰
      │
      ▼
┌──────────────┐
│@senior-      │  ← 코드 리뷰
│ reviewer     │
└──────┬───────┘
       │ 피드백 반영
       │
       ▼
 4. 승인 & 머지
      │
      ▼
┌──────────────┐
│ CI/CD        │  ← 자동 테스트, 자동 배포
│ Pipeline     │
└──────┬───────┘
       │
       ▼
 5. 완료!
      │
      ▼
┌──────────────┐
│ Slack 알림   │  ← "PR #123이 머지되었습니다"
└──────────────┘
```

### 상세 워크플로우 예시

```
1. 이슈 시작
   > /issue-start 42
   Claude: 이슈 #42 분석...
           브랜치 feature/42 생성...
           작업 계획 제안...

2. 개발 중
   > 로그인 폼 컴포넌트 만들어줘
   Claude: [코드 작성]

   > @junior-helper 이 부분 설명해줘
   Claude: [친절한 설명]

3. 코드 리뷰
   > @senior-reviewer 이 코드 봐줘
   Claude: [상세 리뷰]
           - 좋은 점: ...
           - 개선점: ...

   > @security-expert 보안 검토해줘
   Claude: [보안 검토]
           - 발견된 이슈: ...
           - 해결 방안: ...

4. PR 생성
   > /pr
   Claude: [자동 테스트 실행]
           [PR 본문 생성]
           [PR 생성 완료]

5. 머지 후
   CI/CD: 자동 배포
   Slack: "이슈 #42 배포 완료!"
```

---

## 팀 온보딩 자동화

### 새 팀원용 체크리스트

```markdown
<!-- .claude/commands/new-member.md -->
# 새 팀원 체크리스트

환영합니다! 아래 순서대로 진행하세요.

## Day 1: 환경 설정

### 필수 설치
- [ ] Node.js 20.x
- [ ] Git
- [ ] VS Code
- [ ] Claude Code CLI

### 저장소 설정
- [ ] Git 저장소 클론
- [ ] 개발 환경 설정 (`npm install`)
- [ ] 환경변수 설정 (`.env.local`)
- [ ] 로컬에서 실행 확인 (`npm run dev`)

### 문서 읽기
- [ ] CLAUDE.md 읽기
- [ ] README.md 읽기
- [ ] .claude/commands/ 폴더 확인

### 팀 채널 가입
- [ ] Slack #frontend-dev
- [ ] GitHub 팀 추가

## Day 2: 첫 작업

### 프로젝트 이해
- [ ] 프로젝트 구조 파악
- [ ] 주요 기능 흐름 이해
- [ ] 기존 코드 읽어보기

### 첫 기여
- [ ] `good-first-issue` 이슈 할당받기
- [ ] `/issue-start <번호>`로 시작
- [ ] 작은 수정 후 PR 올리기

## Week 1: 팀 합류

### 팀 활동
- [ ] 팀 미팅 참석 (월요일 10시)
- [ ] 페어 프로그래밍 1회
- [ ] 다른 팀원 PR 리뷰해보기

### 학습
- [ ] 주요 기능 코드 분석
- [ ] 아키텍처 문서 읽기
- [ ] 테스트 코드 작성 연습

## 도움 요청
- 기술 질문: @senior-developer
- 프로세스 질문: @team-lead
- 일반 질문: #frontend-dev
- Claude 도움: @junior-helper

궁금한 것이 있으면 언제든 물어보세요!
```

### 환경 설정 자동화

```markdown
<!-- .claude/commands/setup.md -->
# 개발 환경 설정

## 1. 필수 프로그램 확인
```bash
# Node.js 버전 확인 (20.x 필요)
node --version

# npm 버전 확인
npm --version

# Git 버전 확인
git --version
```

## 2. 의존성 설치
```bash
npm install
```

## 3. 환경변수 설정
```bash
cp .env.example .env.local
```

`.env.local`에서 다음을 설정하세요:

| 변수 | 설명 | 얻는 방법 |
|------|------|----------|
| DATABASE_URL | 로컬 DB 주소 | 팀에 문의 |
| API_KEY | 개발용 API 키 | 팀에 문의 |
| NEXT_PUBLIC_API_URL | API 서버 주소 | 보통 localhost:3001 |

## 4. 데이터베이스 설정
```bash
# 마이그레이션 실행
npm run db:migrate

# 시드 데이터 추가
npm run db:seed
```

## 5. 개발 서버 실행
```bash
npm run dev
```

## 6. 확인
- http://localhost:3000 접속
- 로그인 테스트: test@example.com / test1234

## 문제가 있으면?
- 에러 메시지와 함께 #frontend-dev에 질문
- 또는 `@junior-helper 이 에러 뭐야?`로 Claude에게 물어보기
```

---

## 팀 규칙 문서화

### README에 Claude 사용법

```markdown
# 프로젝트 README

## Claude Code 사용법

이 프로젝트는 Claude Code로 개발 생산성을 높입니다.

### 시작하기
```bash
# Claude Code 설치
npm install -g @anthropic-ai/claude-code

# 프로젝트 폴더에서 실행
claude
```

### 사용 가능한 Commands

| 명령어 | 설명 | 사용 시점 |
|--------|------|----------|
| `/pr` | PR 생성 | 기능 완료 후 |
| `/review` | 코드 리뷰 | 커밋 전 |
| `/issue-start <번호>` | 이슈 시작 | 작업 시작 시 |
| `/onboarding` | 온보딩 안내 | 새 팀원 |
| `/setup` | 환경 설정 | 초기 설정 |
| `/debug` | 디버깅 도움 | 에러 발생 시 |

### 사용 가능한 Agents

| Agent | 설명 | 사용 시점 |
|-------|------|----------|
| `@senior-reviewer` | 시니어 관점 리뷰 | PR 전 |
| `@junior-helper` | 친절한 설명 | 개념 학습 |
| `@security-expert` | 보안 검토 | 보안 코드 |

### 팀 규칙

#### 코드 변경
- 모든 코드 변경은 PR로
- PR 전에 `/review`로 셀프 리뷰
- 리뷰어 승인 후 머지

#### 커밋
- Conventional Commits 형식
- 커밋 전 테스트 통과 확인
- 큰 변경은 작은 커밋으로 분리

#### 리뷰
- 24시간 내 리뷰 응답
- 건설적인 피드백
- 잘된 점도 언급

### 예시 워크플로우

```bash
# 1. 이슈 시작
> /issue-start 42

# 2. 개발 (Claude와 대화)
> 로그인 폼 만들어줘

# 3. 셀프 리뷰
> /review

# 4. 보안 검토 (필요시)
> @security-expert 이 코드 봐줘

# 5. PR 생성
> /pr

# 6. 리뷰어 지정 후 대기
```
```

---

## Claude와의 실전 대화 예시

### 예시 1: 새 팀원 온보딩

```
새 팀원: "/onboarding"

Claude: 안녕하세요! Frontend Team에 오신 것을 환영합니다!

        온보딩을 시작하겠습니다.

        📋 Day 1 체크리스트:

        1. ✅ 환경 설정
           - Node.js 20.x 설치됨
           - npm 9.x 설치됨

        2. ⏳ 저장소 설정
           다음 명령어를 실행해주세요:
           ```bash
           npm install
           cp .env.example .env.local
           ```

        3. ⏳ 환경변수 설정
           .env.local에서 다음을 설정해야 합니다:
           - DATABASE_URL
           - API_KEY

           이 값들은 #frontend-dev 채널에 고정된 메시지에 있습니다.

        환경변수 설정이 완료되면 알려주세요!

새 팀원: "설정 완료!"

Claude: 좋습니다! 개발 서버를 시작해볼게요.

        ```bash
        npm run dev
        ```

        ✅ 서버가 시작되었습니다!
        http://localhost:3000 에서 확인해보세요.

        📚 다음 단계:
        1. CLAUDE.md 파일을 읽어보세요 (프로젝트 규칙)
        2. `good-first-issue` 라벨이 붙은 이슈를 확인해보세요

        첫 작업을 시작하려면 `/issue-start <이슈번호>`를 사용하세요!

새 팀원: "@junior-helper React의 useEffect가 뭐야?"

Claude: [주니어 도우미 모드]

        좋은 질문이에요!

        🎯 한 줄 요약:
        useEffect는 "컴포넌트가 렌더링된 후에 할 일"을 정의합니다.

        🎭 쉬운 비유:
        영화관에 비유하면:
        - 영화(렌더링)가 시작된 후
        - "자막 켜기", "조명 끄기" 같은 추가 작업을 하는 것

        💻 간단한 예시:
        ```tsx
        function Profile({ userId }) {
          const [user, setUser] = useState(null);

          // userId가 바뀔 때마다 사용자 정보를 가져옴
          useEffect(() => {
            fetchUser(userId).then(data => setUser(data));
          }, [userId]);  // 의존성 배열

          return <div>{user?.name}</div>;
        }
        ```

        📝 핵심 포인트:
        1. 첫 번째 인자: 실행할 함수
        2. 두 번째 인자: 의존성 배열 (이 값이 바뀔 때 실행)

        우리 프로젝트에서 사용 예시를 볼까요?
```

### 예시 2: 팀 PR 워크플로우

```
개발자: "/issue-start 42"

Claude: 이슈 #42를 분석합니다.

        📌 이슈: "로그인 폼에 비밀번호 보기 토글 추가"

        현재 상태: Open
        담당자: @developer
        라벨: enhancement, good-first-issue

        🌿 브랜치 생성:
        ```bash
        git checkout develop
        git pull origin develop
        git checkout -b feature/42-password-toggle
        ```

        ✅ 브랜치가 생성되었습니다!

        📋 작업 계획:
        1. PasswordInput 컴포넌트 수정
        2. 눈 아이콘 추가 (heroicons)
        3. toggle 상태 관리
        4. 테스트 작성

        예상 소요: 2시간

        시작할까요?

개발자: "응, 시작하자"

Claude: [코드 작성...]

        PasswordInput 컴포넌트를 수정했습니다:

        ```tsx
        // src/components/PasswordInput.tsx
        export function PasswordInput({ ... }) {
          const [showPassword, setShowPassword] = useState(false);

          return (
            <div className="relative">
              <input
                type={showPassword ? "text" : "password"}
                ...
              />
              <button
                type="button"
                onClick={() => setShowPassword(!showPassword)}
                aria-label={showPassword ? "비밀번호 숨기기" : "비밀번호 보기"}
              >
                {showPassword ? <EyeOffIcon /> : <EyeIcon />}
              </button>
            </div>
          );
        }
        ```

        테스트도 작성해볼까요?

개발자: "응, 그리고 /review 해줘"

Claude: [테스트 작성 후 리뷰 실행]

        📝 코드 리뷰 결과:

        ✅ 좋은 점:
        - 접근성 고려됨 (aria-label)
        - 상태 관리가 깔끔함
        - 테스트 커버리지 양호

        ⚠️ 개선 제안:
        1. [권장] 비밀번호 최소 길이 안내 추가
        2. [참고] 아이콘 크기 디자인 시스템과 맞춤

        📊 자동 검사:
        - Lint: ✅ 통과
        - Test: ✅ 통과 (3/3)
        - TypeCheck: ✅ 통과

        PR을 생성할까요?

개발자: "/pr"

Claude: PR을 생성합니다.

        📝 PR 정보:

        제목: feat(auth): Add password visibility toggle

        본문:
        ```markdown
        ## 변경 내용
        - 로그인 폼에 비밀번호 보기/숨기기 토글 추가
        - 접근성을 위한 aria-label 추가
        - 테스트 케이스 추가

        ## 관련 이슈
        Closes #42

        ## 테스트 방법
        1. 로그인 페이지 접속
        2. 비밀번호 입력
        3. 눈 아이콘 클릭하여 토글 확인

        ## 스크린샷
        [첨부]
        ```

        ✅ PR #123이 생성되었습니다!
        https://github.com/team/project/pull/123

        리뷰어로 @senior-dev를 추가했습니다.
```

### 예시 3: 보안 검토

```
개발자: "@security-expert 이 인증 코드 검토해줘"

Claude: [보안 전문가 모드]

        🔒 보안 검토 결과

        검토 파일: src/api/auth.ts

        ---

        [심각도: 높음]
        📍 위치: auth.ts:23
        🔴 문제: SQL 인젝션 취약점
        ```typescript
        // 현재 코드
        const query = `SELECT * FROM users WHERE email = '${email}'`;
        ```

        💥 영향: 공격자가 임의의 SQL 실행 가능

        ✅ 해결:
        ```typescript
        // 수정 코드
        const query = 'SELECT * FROM users WHERE email = $1';
        const result = await db.query(query, [email]);
        ```

        📚 참고: OWASP SQL Injection

        ---

        [심각도: 중간]
        📍 위치: auth.ts:45
        🟡 문제: 에러 메시지에 내부 정보 노출
        ```typescript
        catch (error) {
          throw new Error(`DB 연결 실패: ${error.message}`);
        }
        ```

        ✅ 해결:
        ```typescript
        catch (error) {
          console.error('DB 연결 실패:', error);  // 로그에만
          throw new Error('인증에 실패했습니다');  // 사용자에게
        }
        ```

        ---

        📊 요약:
        - 높음: 1건 (즉시 수정 필요)
        - 중간: 1건 (빠른 수정 권장)
        - 낮음: 0건

        수정 코드를 적용할까요?
```

---

## 🎯 미니 퀴즈

### 퀴즈 1: 팀 표준화의 이점
팀에서 Claude Code를 표준화하면 얻는 이점이 아닌 것은?

A) 일관된 코드 품질
B) 새 팀원 온보딩 용이
C) 개인 취향대로 설정 가능
D) 지식 공유 자동화

<details>
<summary>정답 보기</summary>

**정답**: C) 개인 취향대로 설정 가능

**설명**: 팀 표준화의 핵심은 개인 취향보다 팀 전체의 일관성을 우선하는 것입니다. 물론 팀 표준 위에 개인 설정을 추가할 수 있지만, 기본은 팀 표준을 따릅니다.
</details>

### 퀴즈 2: 공유해야 할 파일
Git에 커밋하여 팀과 공유해야 하는 파일이 아닌 것은?

A) CLAUDE.md
B) .claude/commands/
C) .env.local
D) .claude/agents/

<details>
<summary>정답 보기</summary>

**정답**: C) .env.local

**설명**: .env.local에는 API 키, 비밀번호 등 민감한 정보가 포함되어 있어 Git에 커밋하면 안 됩니다. .gitignore에 추가해야 합니다.
</details>

### 퀴즈 3: Commands 사용
새 팀원이 프로젝트를 시작할 때 사용해야 하는 Command는?

<details>
<summary>정답 보기</summary>

**정답**: `/onboarding` 또는 `/setup`

**설명**:
- `/onboarding`: 전체 온보딩 프로세스 안내
- `/setup`: 개발 환경 설정 가이드
- `/new-member`: 체크리스트 제공

새 팀원은 이 Commands를 통해 빠르게 프로젝트에 적응할 수 있습니다.
</details>

### 퀴즈 4: Agents 역할
보안 관련 코드를 리뷰받고 싶을 때 호출해야 하는 Agent는?

<details>
<summary>정답 보기</summary>

**정답**: `@security-expert`

**설명**: 각 Agent는 전문 역할이 있습니다:
- `@senior-reviewer`: 일반 코드 리뷰, 아키텍처
- `@junior-helper`: 개념 설명, 학습 도움
- `@security-expert`: 보안 검토, 취약점 분석
</details>

### 퀴즈 5: 워크플로우 순서
올바른 팀 워크플로우 순서는?

<details>
<summary>정답 보기</summary>

**정답**: 이슈 시작 → 구현 → 리뷰 → PR 생성 → 머지

**상세 순서**:
1. `/issue-start <번호>`: 브랜치 생성, 작업 계획
2. 구현: Claude와 대화하며 개발
3. `/review`: 셀프 리뷰
4. `/pr`: PR 생성
5. 리뷰어 승인 후 머지
6. CI/CD 자동 배포
</details>

---

## 📝 실습 과제

### 과제 1: 기초 - 팀 CLAUDE.md 작성 [난이도: ★☆☆]

**목표**: 팀용 CLAUDE.md 파일 작성

**단계**:
1. 프로젝트 루트에 CLAUDE.md 생성
2. 다음 섹션 포함:
   - 팀 정보
   - 기술 스택
   - 코딩 컨벤션
   - Git 규칙
   - 자주 쓰는 명령어
3. 팀원에게 리뷰 받기

**확인 사항**:
- [ ] 새 팀원이 읽고 바로 작업 시작 가능
- [ ] 팀 규칙이 명확하게 정의됨
- [ ] 연락처/질문 방법이 포함됨

### 과제 2: 중급 - 팀 Commands 만들기 [난이도: ★★☆]

**목표**: 팀 워크플로우를 위한 Commands 3개 작성

**필수 Commands**:
1. `/pr`: PR 생성 워크플로우
2. `/review`: 코드 리뷰 체크리스트
3. `/onboarding`: 새 팀원 온보딩

**단계**:
1. `.claude/commands/` 폴더 생성
2. 각 Command 파일 작성
3. 팀원들과 테스트
4. 피드백 반영

**확인 사항**:
- [ ] 모든 팀원이 Commands 사용 가능
- [ ] 일관된 결과 출력
- [ ] README에 사용법 문서화

### 과제 3: 심화 - 전체 팀 설정 구축 [난이도: ★★★]

**목표**: 완전한 팀 표준 환경 구축

**구성 요소**:
1. CLAUDE.md (프로젝트 규칙)
2. Commands (5개 이상)
3. Agents (3개 이상)
4. settings.json (팀 설정)
5. README 문서화

**단계**:
1. 팀과 표준 논의
2. 각 구성 요소 작성
3. 테스트 및 피드백
4. 문서화 및 교육
5. 정기 업데이트 프로세스 수립

**확인 사항**:
- [ ] 새 팀원이 첫날부터 생산성 발휘
- [ ] 코드 리뷰 품질 향상
- [ ] 팀 전체 일관성 확보

---

## 🏆 도전 과제

### 도전 1: 온보딩 자동화

새 팀원의 첫 주를 완전 자동화:

1. Day 1 환경 설정 자동화
2. Day 2 첫 작업 가이드
3. Week 1 체크리스트
4. 진행 상황 자동 추적

### 도전 2: 품질 게이트 구축

PR 전 자동 품질 검사 시스템:

1. 자동 린트 검사
2. 자동 테스트
3. 보안 스캔
4. 코드 커버리지 체크
5. 리뷰 자동 요청

### 도전 3: 팀 메트릭스 대시보드

Claude Code 활용 현황 대시보드:

1. Commands 사용 통계
2. 생산성 지표
3. 코드 품질 추이
4. 팀원별 활용 패턴

---

## 정리

이번 챕터에서 배운 것:
- [x] 팀 설정 표준화 (.claude/ 폴더)
- [x] 팀 Commands와 Agents
- [x] 협업 워크플로우 구축
- [x] 온보딩 자동화
- [x] 팀 규칙 문서화

**핵심 포인트**: 좋은 팀 설정은 모든 팀원이 일관되게 작업할 수 있게 합니다. 한 번 잘 구축하면 계속해서 가치를 제공합니다.

---

## 용어 사전

| 용어 | 설명 |
|------|------|
| **표준화** | 팀 전체가 동일한 방식으로 작업하도록 규칙을 정하는 것 |
| **온보딩** | 새 팀원이 프로젝트에 적응하는 과정 |
| **Commands** | `/명령어` 형태로 실행하는 미리 정의된 워크플로우 |
| **Agents** | `@이름` 형태로 호출하는 특정 역할의 AI 페르소나 |
| **CLAUDE.md** | 프로젝트 규칙과 컨텍스트를 정의하는 파일 |
| **플레이북** | 팀의 표준 작업 방식을 정리한 문서 |
| **워크플로우** | 작업을 수행하는 일련의 단계 |
| **코드 리뷰** | 다른 사람의 코드를 검토하는 과정 |

---

## 성공을 위한 빠른 팁

1. **작게 시작하십시오** - 한 번에 20개 명령어를 만들지 마십시오. 필수적인 2-3개로 시작하십시오.

2. **모든 것을 문서화하십시오** - README에 없으면 새 멤버에게는 존재하지 않는 것입니다.

3. **반복하십시오** - 첫 버전은 완벽하지 않습니다. 피드백을 바탕으로 개선하십시오.

4. **본을 보이십시오** - 팀 도구를 직접 일관되게 사용하십시오.

5. **성공을 축하하십시오** - 팀 도구가 시간을 아끼면, 그 성공을 공유하십시오.

---

# 전체 커리큘럼 요약

## 바이브코딩 커리큘럼 완료!

축하합니다! 26개 챕터를 모두 완료하셨습니다!

### 학습 여정 돌아보기

```
┌─────────────────────────────────────────────────────────────────────┐
│                    바이브코딩 커리큘럼 전체 구조                        │
└─────────────────────────────────────────────────────────────────────┘

Part 1: 기초 (Chapter 1-10)
├── Chapter 1-3: 설치와 첫 시작
├── Chapter 4-6: 기본 사용법 마스터
└── Chapter 7-10: 첫 프로젝트 완성

Part 2: 실전 (Chapter 11-20)
├── Chapter 11-13: 웹사이트 개발
├── Chapter 14-16: 앱 개발
└── Chapter 17-20: 배포와 운영

Part 3: 고급 (Chapter 22-27)
├── Chapter 22: 설정 심화
├── Chapter 23: Hooks & Commands
├── Chapter 24: Agents & Skills
├── Chapter 25: MCP 연동
├── Chapter 26: CI/CD 자동화
└── Chapter 27: 팀 협업 ← 현재 위치!
```

### 배운 핵심 개념 총정리

| 챕터 범위 | 핵심 개념 | 실제 활용 |
|-----------|----------|----------|
| 1-10 | Claude Code 기초 | 첫 프로젝트 완성 |
| 11-15 | 웹 개발 | 웹사이트/앱 제작 |
| 16-20 | 배포/운영 | 실제 서비스 운영 |
| **22** | **CLAUDE.md, settings.json** | **프로젝트 규칙 정의** |
| **23** | **Hooks, Commands** | **자동화 워크플로우** |
| **24** | **Agents, Skills** | **역할 기반 작업** |
| **25** | **MCP** | **외부 서비스 연동** |
| **26** | **CI/CD** | **자동 테스트/배포** |
| **27** | **팀 협업** | **팀 표준화** |

### 이제 할 수 있는 것들

#### 기본 역량
- Claude Code 설치 및 설정
- 자연어로 코딩 요청
- 파일 생성, 수정, 삭제
- 프로젝트 구조 이해

#### 개발 역량
- 웹사이트/앱 제작
- API 연동
- 데이터베이스 작업
- 테스트 작성

#### 운영 역량
- 배포 자동화
- CI/CD 파이프라인
- 모니터링 설정
- 문제 해결

#### 고급 역량
- 커스텀 설정 (CLAUDE.md)
- 자동화 Hooks
- 재사용 Commands
- Agent 페르소나
- MCP 서비스 연동

#### 협업 역량
- 팀 표준 구축
- 온보딩 자동화
- 코드 리뷰 프로세스
- 지식 공유 시스템

---

## 다음 학습 단계

### 레벨 1: 심화 학습

#### Claude Code 공식 문서
- [Claude Code 공식 문서](https://docs.anthropic.com/claude-code)
- 최신 기능 업데이트 확인
- 고급 설정 옵션

#### MCP 서버 개발
- 커스텀 MCP 서버 만들기
- 회사 내부 API 연동
- 특수 도구 개발

### 레벨 2: 실전 프로젝트

#### 추천 프로젝트
1. **개인 포트폴리오 사이트**
   - 정적 사이트 생성
   - 반응형 디자인
   - 배포 자동화

2. **팀 생산성 도구**
   - 이슈 트래커
   - 문서 관리 시스템
   - 대시보드

3. **자동화 봇**
   - Slack 봇
   - GitHub 봇
   - 모니터링 봇

### 레벨 3: 커뮤니티 참여

#### 기여 방법
1. **버그 리포트**
   - 문제 발견 시 이슈 등록
   - 재현 방법 상세히 기술

2. **기능 제안**
   - 새로운 아이디어 공유
   - 사용 사례 설명

3. **문서 기여**
   - 오타/오류 수정
   - 예시 추가
   - 번역 참여

4. **경험 공유**
   - 블로그 포스트 작성
   - 튜토리얼 제작
   - 커뮤니티 질문 답변

---

## 추천 학습 자료

### 공식 자료

| 자료 | 링크 | 설명 |
|------|------|------|
| Claude Code 문서 | docs.anthropic.com | 공식 문서 |
| Anthropic 블로그 | anthropic.com/blog | 최신 소식 |
| GitHub 예제 | github.com/anthropic | 예제 코드 |

### 커뮤니티 자료

| 자료 | 설명 |
|------|------|
| Discord | 실시간 질문/답변 |
| Reddit | 경험 공유, 팁 |
| YouTube | 튜토리얼 영상 |
| Dev.to | 블로그 포스트 |

### 관련 기술 학습

| 기술 | 추천 자료 |
|------|----------|
| Git/GitHub | GitHub Docs |
| JavaScript/TypeScript | MDN Web Docs |
| React | React 공식 문서 |
| Node.js | Node.js 공식 문서 |
| GitHub Actions | GitHub Actions 문서 |

---

## 커뮤니티 참여

### 디스코드 커뮤니티

[![Discord](https://img.shields.io/badge/Discord-바이브코딩_커뮤니티-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

**채널 소개**:
- `#질문답변`: 학습 중 궁금한 점
- `#프로젝트-공유`: 만든 프로젝트 자랑
- `#팁-공유`: 유용한 팁 공유
- `#취업-커리어`: 취업 정보 교환

### GitHub 저장소

- 이슈 등록: 버그, 기능 제안
- PR: 문서 개선, 오류 수정
- 스타: 프로젝트 응원

### 경험 공유하기

학습 경험을 공유해주세요:
- 블로그 포스트 작성
- 튜토리얼 영상 제작
- SNS에 공유 (#바이브코딩)

---

## 축하합니다

바이브코딩 커리큘럼 27개 챕터를 모두 완료하셨습니다!

이제 Claude Code로 다음을 할 수 있습니다:
- 웹사이트 제작 및 배포
- 앱과 자동화 도구 개발
- 고급 워크플로우 구성 (Hooks, Skills, MCP)
- CI/CD 파이프라인 구축
- 팀과 효과적으로 협업

### 기억하시기 바랍니다

```
┌─────────────────────────────────────────────────────────────────────┐
│                         마지막 조언                                  │
└─────────────────────────────────────────────────────────────────────┘

🎯 처음부터 완벽할 필요 없습니다
   작은 프로젝트부터 시작하세요.
   실패해도 괜찮습니다. 배우는 과정입니다.

🔄 피드백으로 계속 개선하면 됩니다
   코드, 설정, 워크플로우 모두
   처음 버전이 최선일 필요 없습니다.

🤝 Claude는 항상 도와줄 준비가 되어 있습니다
   막힐 때, 모를 때, 언제든 물어보세요.
   "이게 뭐야?", "어떻게 해?" 모든 질문 환영!

💪 이제 나가서 멋진 것들을 만드시기 바랍니다
   여러분의 아이디어가 현실이 됩니다.
   코딩의 즐거움을 느껴보세요!
```

---

**감사합니다!**

바이브코딩 커리큘럼과 함께해주셔서 감사합니다.

여러분의 개발 여정을 응원합니다!

---

[![Discord](https://img.shields.io/badge/Discord-커뮤니티_참여-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)
[![GitHub](https://img.shields.io/badge/GitHub-프로젝트_스타-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/your-repo)

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [Claude Code 공식 문서](https://docs.anthropic.com/en/docs/claude-code) - Claude Code 완전 가이드
- [GitHub 팀 협업 가이드](https://docs.github.com/en/get-started/quickstart/collaborating-with-issues-and-pull-requests) - GitHub 협업 기초
- [Code Review Best Practices](https://google.github.io/eng-practices/review/) - Google 코드 리뷰 가이드

**영상 자료:**
- [팀 협업 워크플로우 (YouTube)](https://www.youtube.com/results?search_query=team+collaboration+workflow+development) - 개발 팀 협업
- [Git 브랜치 전략 (YouTube)](https://www.youtube.com/results?search_query=git+branching+strategy+korean) - Git 브랜치 전략
- [효과적인 코드 리뷰 (YouTube)](https://www.youtube.com/results?search_query=effective+code+review+tutorial) - 코드 리뷰 기법

**읽을거리:**
- [Atlassian Git Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows) - Git 워크플로우 비교
- [Conventional Commits](https://www.conventionalcommits.org/ko/v1.0.0/) - 커밋 메시지 규칙 (한국어)
- [Trunk Based Development](https://trunkbaseddevelopment.com/) - 트렁크 기반 개발

**팀 도구:**
- [Slack](https://slack.com/) - 팀 커뮤니케이션
- [Notion](https://www.notion.so/) - 문서 협업
- [Linear](https://linear.app/) - 이슈 트래킹
- [Figma](https://www.figma.com/) - 디자인 협업

**추가 학습:**
- [Engineering Practices at Google](https://google.github.io/eng-practices/) - Google 엔지니어링 가이드
- [The Pragmatic Programmer](https://pragprog.com/titles/tpp20/the-pragmatic-programmer-20th-anniversary-edition/) - 프로그래머 필독서

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
