# Chapter 09: Subagents와 Skills

**한국어** | [English](./README.md)

## Prerequisites

이 Chapter를 시작하기 전에:
- [ ] Chapter 00-08 완료
- [ ] slash commands와 hooks 이해
- [ ] 마크다운 설정 파일 생성에 익숙함

---

## Introduction

Subagents와 Skills는 특화된 AI 어시스턴트를 만들고 Claude에게 도메인 특정 지식을 가르칠 수 있는 고급 기능입니다. Subagents는 특정 작업을 위한 독립적인 작업자이고, Skills는 자동으로 활성화되는 지식 모듈입니다.

### Subagents vs Skills

| 기능 | Subagents | Skills |
|------|-----------|--------|
| **목적** | 특정 작업 위임 | 도메인 지식 추가 |
| **활성화** | 명시적 (직접 호출) | 자동 (컨텍스트 기반) |
| **컨텍스트** | 별도 대화 | 공유 대화 |
| **도구** | 에이전트별 설정 가능 | 스킬별 설정 가능 |

---

## Topics

### 1. Subagents 이해하기

Subagents는 다음 특성을 가진 특화된 AI 어시스턴트입니다:
- 자체 대화 컨텍스트 보유
- 특정 도구로 설정 가능
- 독립적으로 실행하고 결과 보고

**내장 Subagents**:
| 이름 | 모델 | 목적 |
|------|------|------|
| `Explore` | Haiku | 빠른 코드베이스 탐색 |
| `Plan` | Sonnet | 구현 계획 |
| `General-purpose` | Sonnet | 복잡한 다단계 작업 |

### 2. 커스텀 Subagents 생성

`.claude/agents/`에 subagents 생성:

```markdown
<!-- .claude/agents/code-reviewer.md -->
---
name: code-reviewer
description: 전문 코드 리뷰 스페셜리스트. 철저한 코드 리뷰에 사용.
tools: Read, Grep, Glob
model: sonnet
---

# Code Reviewer Agent

## 역할
품질, 보안, 유지보수성에 집중하는 전문 코드 리뷰어입니다.

## 리뷰 체크리스트
1. **보안**: 취약점 찾기
2. **성능**: 병목 식별
3. **스타일**: 일관성 확인
4. **로직**: 정확성 검증

## 출력 형식
구조화된 피드백 제공:
- 심각도 (Critical/Major/Minor)
- 위치 (파일:라인)
- 문제 설명
- 제안된 수정
```

### 3. Subagent 설정

**Frontmatter 옵션**:
```yaml
---
name: agent-name           # 필수: 식별자 (소문자, 하이픈)
description: 간단한 설명    # 필수: 언제 사용하는지
tools: Read, Grep, Bash    # 선택: 쉼표로 구분된 도구 목록
model: sonnet              # 선택: sonnet, opus, haiku, inherit
permissionMode: default    # 선택: 권한 모드
skills: skill1, skill2     # 선택: 자동 로드할 스킬
---
```

**도구 제한**:
- `Read, Grep, Glob` - 읽기 전용 에이전트
- `Read, Edit, Write` - 파일 수정 가능
- `Bash` - 명령 실행 가능
- 생략하면 모든 도구 상속

### 4. Subagents 사용

```bash
# 명시적 호출
> Have the code-reviewer agent review @src/auth/

# Claude가 사용 제안할 수 있음
> I need a thorough code review
# Claude: I'll use the code-reviewer agent for this...

# 사용 가능한 에이전트 목록
/agents
```

### 5. Skills 이해하기

Skills는 관련 있을 때 Claude가 자동으로 로드하는 지식 모듈입니다:

```markdown
<!-- .claude/skills/react-patterns/SKILL.md -->
---
name: react-patterns
description: 이 프로젝트의 React 모범 사례와 패턴
allowed-tools: Read, Grep
---

# 이 프로젝트의 React 패턴

## 컴포넌트 구조
- hooks가 있는 함수형 컴포넌트 사용
- hooks는 컴포넌트 상단에 배치
- 로직은 커스텀 hooks로 추출

## 상태 관리
- 로컬 상태에 useState 사용
- 서버 상태에 React Query 사용
- 필요하지 않으면 Redux 피하기

## 파일 이름
- 컴포넌트: PascalCase.tsx
- Hooks: useHookName.ts
- Utils: camelCase.ts
```

### 6. Skill 구조

```
.claude/skills/
├── react-patterns/
│   ├── SKILL.md          # 메인 스킬 파일 (필수)
│   ├── examples/         # 코드 예시
│   │   └── hooks.md
│   └── anti-patterns.md  # 피해야 할 것
├── api-design/
│   └── SKILL.md
└── testing/
    └── SKILL.md
```

### 7. Skills 활성화 시점

Claude가 다음을 감지하면 Skills가 자동 활성화됩니다:
- 프롬프트의 키워드
- 관련 파일 유형
- 스킬 설명과 일치하는 컨텍스트

```bash
> Create a new React component

# Claude가 자동으로 react-patterns 스킬 로드
# 정의된 규칙을 따름
```

### 8. Skills vs 다른 기능

| 기능 | 자동 활성화 | 범위 | 최적 용도 |
|------|-----------|------|----------|
| **Skills** | 예 | 도메인 지식 | 프로젝트 규칙 |
| **Commands** | 아니오 (명시적) | 재사용 프롬프트 | 반복 작업 |
| **CLAUDE.md** | 예 (항상) | 프로젝트 규칙 | 전역 가이드라인 |
| **Subagents** | 아니오 (위임) | 별도 컨텍스트 | 복잡한 작업 |

### 9. 스킬 핫 리로드

스킬이 생성되거나 수정되면 자동으로 다시 로드됩니다—세션 재시작 필요 없음:

```bash
# 스킬 생성 또는 편집
vim ~/.claude/skills/my-skill/SKILL.md

# 변경 사항이 현재 세션에 즉시 적용됨
```

### 10. 포크된 Subagent 컨텍스트

스킬이나 명령을 격리된 컨텍스트에서 실행할 수 있습니다:

```yaml
---
name: isolated-task
description: 별도 컨텍스트에서 실행
context: fork
agent: custom-agent
---
# 이 스킬은 자체 컨텍스트에서 실행됨
```

`context: fork`를 사용하면 메인 대화 컨텍스트에 영향을 주지 않고 독립적으로 작업을 수행합니다.

---

## Resources

- [Claude Code 공식 문서](https://code.claude.com/docs)
- [Agents & Subagents](https://code.claude.com/docs/en/agents)
- [Skills Reference](https://code.claude.com/docs/en/skills)

---

## Checklist

면접에서 답변하듯이 다음 질문에 답해보세요:

1. **Subagents와 Skills의 차이점은 무엇인가요?**
   <details>
   <summary>힌트</summary>
   Subagents: 위임된 작업, 별도 컨텍스트. Skills: 지식 모듈, 자동 활성화.
   </details>

2. **Subagent와 Skill 중 언제 어떤 것을 사용하나요?**
   <details>
   <summary>힌트</summary>
   Subagent: 복잡하고 독립적인 작업. Skill: 광범위하게 적용되는 도메인 지식.
   </details>

3. **Subagent가 사용할 수 있는 도구를 어떻게 제한하나요?**
   <details>
   <summary>힌트</summary>
   frontmatter의 tools 필드: Read, Grep, Edit 등
   </details>

4. **Claude가 Skill을 언제 활성화할지 어떻게 결정하나요?**
   <details>
   <summary>힌트</summary>
   키워드, 파일 유형, 스킬 설명과 일치하는 컨텍스트
   </details>

---

## Mini Project: 전문화된 AI 팀

### Project Goals

특화된 subagents와 skills 팀을 구축하세요:

- [ ] 보안 리뷰를 위한 읽기 전용 도구를 가진 Security Auditor subagent 생성
- [ ] 테스트 파일을 작성할 수 있는 Test Generator subagent 생성
- [ ] API 문서와 README를 위한 Documentation Writer subagent 생성
- [ ] 프로젝트 구조를 문서화하는 Project Architecture 스킬 생성
- [ ] 코딩 규칙을 정의하는 Code Style 스킬 생성
- [ ] 테스트 구성을 설명하는 Testing Patterns 스킬 생성
- [ ] 2개 이상의 선택 subagents 또는 skills 추가 (리팩토링, 성능 등)
- [ ] 에이전트와 스킬이 함께 작동하는지 테스트

### Ideas to Try

- 복잡한 작업에서 서로 조율하는 에이전트 생성
- 외부 문서를 참조하는 스킬 구축
- 버전별 스킬 만들기 (예: React 18 vs React 17)
- 프레임워크 업그레이드를 위한 마이그레이션 어시스턴트 subagent 생성

---

## Advanced

### 문서 기반 Skill 만들기

외부 문서를 참조하는 실용적인 skill을 만들어보세요:

```markdown
<!-- .claude/skills/react-19-migration.md -->
---
name: react-19-migration
description: React 19 마이그레이션 가이드
globs: ["**/*.tsx", "**/*.jsx"]
---

# React 19 Migration Guide

## 주요 변경사항
- use() 훅 추가
- Server Components 기본 지원
- ref를 prop으로 전달 가능

## 마이그레이션 체크리스트
1. forwardRef 제거하고 ref prop 사용
2. useFormStatus, useFormState 활용
3. 서버 컴포넌트 / 클라이언트 컴포넌트 분리

자세한 내용: https://react.dev/blog/2024/04/25/react-19
```

### Subagent 실전 활용

특정 작업에 subagent를 활용하는 패턴:

```bash
# Security Auditor 활용
> Have the security-auditor agent review the authentication flow
> in src/auth/. Focus on token handling and session management.

# 결과를 기반으로 수정
> Based on the security review, fix the identified issues
```

### 팀 내 공유 방법

`.claude/` 폴더를 git에 포함하면 팀 전체가 사용할 수 있습니다:

```
.claude/
├── agents/
│   ├── security-auditor.md    # 팀 공통 에이전트
│   └── api-designer.md
├── skills/
│   ├── our-coding-style.md    # 팀 코딩 컨벤션
│   └── api-patterns.md
└── commands/
    └── review.md
```

> **Tip**: 새 팀원이 합류하면 `git pull` 한 번으로 모든 설정이 자동 적용됩니다.
