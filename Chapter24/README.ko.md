# Chapter 24: 팀 협업

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- 팀에서 Claude Code 표준화하기
- 설정과 프롬프트 공유
- 협업 워크플로우 구축

---

## 왜 필요한가요?

**실제 상황**: 팀이 Claude Code를 좋아하지만, 모두 다르게 사용합니다. 한 사람은 특정 명령어를 쓰고, 다른 사람은 다른 설정을 씁니다. 새 팀원들이 혼란스러워합니다. 코드 리뷰가 일관성이 없습니다.

팀을 위해 Claude Code를 표준화하면 모두가 같은 방식으로 일하고, 새 멤버를 온보딩하기 쉬워집니다.

### 쉬운 비유: 팀 플레이북

팀 설정을 스포츠 팀 플레이북처럼 생각해보시기 바랍니다:
- 모두가 같은 플레이를 알고 있습니다 (commands)
- 모두가 같은 용어를 씁니다 (agents)
- 새 선수가 빨리 배울 수 있습니다 (onboarding)

---

## 왜 팀 협업을 알아야 할까?

혼자 쓸 때와 팀에서 쓸 때는 다릅니다:

- **일관성**: 팀원마다 다른 방식 → 혼란
- **온보딩**: 새 팀원이 바로 시작할 수 있어야 함
- **품질**: 팀 전체의 코드 품질 향상

---

## 팀 표준화

### 공유해야 할 것들

```
my-project/
├── .claude/
│   ├── settings.json    # 팀 설정
│   ├── commands/        # 팀 Commands
│   ├── agents/          # 팀 Agents
│   └── skills/          # 팀 Skills
├── CLAUDE.md            # 프로젝트 규칙
└── src/
```

이 폴더들을 Git에 커밋하면 팀 전체가 같은 환경에서 작업합니다.

### CLAUDE.md 팀 버전

```markdown
# 프로젝트: My Team App

## 팀 정보
- 팀명: Frontend Team
- 연락처: #frontend-dev (Slack)

## 기술 스택
- React 18 + TypeScript
- Tailwind CSS
- Jest + React Testing Library
- GitHub Actions

## 코딩 컨벤션
- 컴포넌트: 함수형, PascalCase
- 훅: use 접두사, camelCase
- 파일명: kebab-case
- 테스트: *.test.tsx

## Git 규칙
- 커밋 메시지: Conventional Commits
- PR 필수: main 브랜치 직접 푸시 금지
- 리뷰어: 최소 1명

## 자주 쓰는 명령어
- npm run dev: 개발 서버
- npm test: 테스트
- npm run lint: 린트
- npm run build: 빌드

## 중요한 폴더
- src/components/: 재사용 컴포넌트
- src/features/: 기능별 모듈
- src/hooks/: 커스텀 훅
- src/utils/: 유틸리티
```

---

## 팀 Commands

### 표준 워크플로우

```markdown
<!-- .claude/commands/pr.md -->
# PR 생성 워크플로우

## 사전 체크
- [ ] 테스트 통과: $(npm test)
- [ ] 린트 통과: $(npm run lint)
- [ ] 빌드 성공: $(npm run build)

## PR 제목
feat|fix|docs|refactor: 간결한 설명

## PR 본문
### 변경 내용
- 무엇을 변경했는지

### 테스트 방법
- 어떻게 테스트하는지

### 스크린샷
(UI 변경 시)
```

```
> /pr
```

### 온보딩 도우미

```markdown
<!-- .claude/commands/onboarding.md -->
# 새 팀원 온보딩

새로 오신 분이시군요! 환영합니다.

## 1. 환경 설정
```bash
npm install
cp .env.example .env.local
npm run dev
```

## 2. 프로젝트 구조
$(tree -L 2 src/)

## 3. 중요 파일
- CLAUDE.md: 프로젝트 규칙
- .claude/commands/: 사용 가능한 명령어
- src/: 소스 코드

## 4. 첫 번째 작업
- 간단한 이슈부터 시작하세요
- 질문은 #frontend-dev 채널에

궁금한 게 있으면 물어보세요!
```

```
> /onboarding
```

### 코드 리뷰 요청

```markdown
<!-- .claude/commands/review-request.md -->
# 리뷰 요청

## 변경 파일
$(git diff --name-only HEAD~1)

## 변경 내용 요약
$ARGUMENTS

## 리뷰 포인트
- 로직이 맞는지
- 더 좋은 방법이 있는지
- 테스트가 충분한지

리뷰어: @team-lead
```

```
> /review-request "로그인 폼에 검증 로직 추가"
```

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
- 아키텍처: 설계가 적절한가
- 성능: 불필요한 연산이 있는가
- 유지보수: 다른 사람이 이해하기 쉬운가
- 테스트: 충분히 테스트되었는가

## 피드백 스타일
- 문제점과 개선 방향을 함께 제시
- "이게 나쁘다"보다 "이렇게 하면 더 좋다"
- 잘된 부분도 언급
```

```
> @senior-reviewer 이 PR 봐줘
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

## 설명 스타일
1. 핵심 개념 먼저
2. 간단한 예시
3. 실제 코드에 적용
4. 연습 문제 제안
```

```
> @junior-helper React의 useEffect가 뭐야?
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
└──────┬───────┘
       │
       ▼
 2. 구현
      │
      ▼
┌──────────────┐
│ /pr          │  ← PR 생성, 테스트 확인
└──────┬───────┘
       │
       ▼
 3. 리뷰
      │
      ▼
┌──────────────┐
│@senior-reviewer│  ← 코드 리뷰
└──────┬───────┘
       │
       ▼
 4. 머지
      │
      ▼
┌──────────────┐
│ 자동 배포    │  ← CI/CD
└──────────────┘
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
git checkout -b feature/$ARGUMENTS
```

## 3. 작업 계획
이슈 내용을 분석하고 작업 계획을 세워주세요.

## 4. 시작
계획이 확정되면 구현을 시작하세요.
```

```
> /issue-start 42
```

---

## 팀 온보딩 자동화

### 새 팀원용 체크리스트

```markdown
<!-- .claude/commands/new-member.md -->
# 새 팀원 체크리스트

환영합니다! 아래 순서대로 진행하세요.

## Day 1
- [ ] Git 저장소 클론
- [ ] 개발 환경 설정 (npm install)
- [ ] 로컬에서 실행 확인
- [ ] CLAUDE.md 읽기
- [ ] 팀 Slack 채널 가입

## Day 2
- [ ] 프로젝트 구조 이해
- [ ] 간단한 good-first-issue 할당받기
- [ ] 첫 PR 올리기

## Week 1
- [ ] 주요 기능 코드 읽기
- [ ] 팀 미팅 참석
- [ ] 페어 프로그래밍 1회

궁금한 것이 있으면 언제든 물어보시기 바랍니다.
```

### 환경 설정 자동화

```markdown
<!-- .claude/commands/setup.md -->
# 개발 환경 설정

## 1. 의존성 설치
```bash
npm install
```

## 2. 환경변수 설정
```bash
cp .env.example .env.local
```

.env.local에서 다음을 설정하세요:
- DATABASE_URL: 로컬 DB 주소
- API_KEY: 개발용 API 키 (팀에 문의)

## 3. 데이터베이스 설정
```bash
npm run db:migrate
npm run db:seed
```

## 4. 실행 확인
```bash
npm run dev
```

http://localhost:3000에서 확인하시기 바랍니다.
```

---

## 팀 규칙 문서화

### README에 Claude 사용법

```markdown
# 프로젝트 README

## Claude Code 사용법

### 사용 가능한 Commands
| 명령어 | 설명 |
|--------|------|
| /pr | PR 생성 |
| /review | 코드 리뷰 요청 |
| /issue-start <번호> | 이슈 시작 |
| /onboarding | 온보딩 안내 |

### 사용 가능한 Agents
| Agent | 설명 |
|-------|------|
| @senior-reviewer | 시니어 관점 리뷰 |
| @junior-helper | 주니어 도우미 |

### 팀 규칙
- 모든 코드 변경은 PR로
- PR 전에 테스트 통과 확인
- 리뷰어 승인 후 머지
```

---

## 정리

이번 챕터에서 배운 것:
- [x] 팀 설정 표준화 (.claude/ 폴더)
- [x] 팀 Commands와 Agents
- [x] 협업 워크플로우 구축
- [x] 온보딩 자동화
- [x] 팀 규칙 문서화

**핵심 포인트**: 좋은 팀 설정은 모든 팀원이 일관되게 작업할 수 있게 합니다.

---

## 성공을 위한 빠른 팁

1. **작게 시작하십시오** - 한 번에 20개 명령어를 만들지 마십시오. 필수적인 2-3개로 시작하십시오.

2. **모든 것을 문서화하십시오** - README에 없으면 새 멤버에게는 존재하지 않는 것입니다.

3. **반복하십시오** - 첫 버전은 완벽하지 않습니다. 피드백을 바탕으로 개선하십시오.

4. **본을 보이십시오** - 팀 도구를 직접 일관되게 사용하십시오.

5. **성공을 축하하십시오** - 팀 도구가 시간을 아끼면, 그 성공을 공유하십시오.

---

## 축하합니다

바이브코딩 커리큘럼 24개 챕터를 모두 완료하셨습니다.

이제 Claude Code로 다음을 할 수 있습니다:
- 웹사이트 제작 및 배포
- 앱과 자동화 도구 개발
- 고급 워크플로우 구성 (Hooks, Skills, MCP)
- 팀과 효과적으로 협업

**기억하시기 바랍니다:**
- 처음부터 완벽할 필요 없습니다
- 피드백으로 계속 개선하면 됩니다
- Claude는 항상 도와줄 준비가 되어 있습니다

**이제 나가서 멋진 것들을 만드시기 바랍니다.**
