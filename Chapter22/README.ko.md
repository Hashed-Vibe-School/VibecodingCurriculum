# Chapter 22: MCP 연동

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- MCP(Model Context Protocol) 개념
- 외부 서비스 연결하기
- 실용적인 MCP 서버 활용

---

## 왜 필요한가요?

**실제 상황**: 이슈를 디버깅하는데 데이터베이스를 확인해야 합니다. 터미널 열고, PostgreSQL 연결하고, 쿼리 작성하고, 결과 복사하고, Claude에 붙여넣기... 그러면 Claude가 더 많은 데이터를 요청합니다. 이 과정을 반복하면 지치게 됩니다.

MCP는 Claude가 외부 서비스에 직접 연결할 수 있게 해줘서, "최근 주문 10개 보여줘"라고만 하면 됩니다.

### 쉬운 비유: Claude에게 전화기 주기

MCP 없이 Claude는 눈앞에 있는 것(파일들)만 볼 수 있습니다. 책상 위의 문서만 볼 수 있는 사람과 같습니다.

MCP를 연결하면 Claude에게 외부 서비스에 "전화"할 수 있는 "전화기"를 주는 것입니다:
- "데이터베이스에 전화해서 사용자 데이터 물어봐"
- "GitHub에 전화해서 이슈 확인해"
- "Slack에 전화해서 메시지 보내"

MCP 서버는 Claude의 연락처에 있는 전화번호와 같습니다.

---

## 첫 MCP 설정 (단계별)

간단한 MCP 서버를 설정해봅시다. 외부 계정이 필요 없는 filesystem 서버를 사용하겠습니다.

### 단계 1: 설정 파일 만들기

MCP 설정 파일을 만드시기 바랍니다:

```bash
mkdir -p ~/.claude
touch ~/.claude/mcp_servers.json
```

### 단계 2: 간단한 서버 추가

`~/.claude/mcp_servers.json`을 열고 추가하시기 바랍니다:

```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-filesystem", "/tmp"]
    }
  }
}
```

이렇게 하면 Claude가 MCP를 통해 `/tmp`의 파일에 접근할 수 있습니다.

### 단계 3: Claude Code 재시작

MCP 서버가 로드되려면 Claude Code를 종료하고 재시작하시기 바랍니다.

### 단계 4: 테스트

```
> MCP를 사용해서 /tmp의 파일 목록을 보여줘
```

작동하면 파일들이 보일 것입니다. 작동하지 않으면 아래 트러블슈팅 섹션을 확인하시기 바랍니다.

---

## JSON 설정 이해하기

MCP 설정은 JSON을 사용합니다. 각 부분이 무엇을 의미하는지 알아봅시다:

```json
{
  "servers": {                    // 모든 MCP 서버 목록
    "서버이름": {                  // 선택한 이름 (대화에서 사용)
      "command": "npx",           // 실행할 프로그램
      "args": ["-y", "패키지"],    // 명령의 인자
      "env": {                    // 환경 변수 (선택사항)
        "API_KEY": "your-key"
      }
    }
  }
}
```

### 흔한 설정 패턴

**패턴 1: npx 패키지**
```json
{
  "command": "npx",
  "args": ["-y", "@anthropic-ai/mcp-server-postgres"]
}
```

**패턴 2: 환경 변수**
```json
{
  "env": {
    "DATABASE_URL": "postgresql://user:pass@localhost/mydb"
  }
}
```

**패턴 3: 여러 서버**
```json
{
  "servers": {
    "server1": { ... },
    "server2": { ... }
  }
}
```

---

## 왜 MCP를 알아야 할까?

Claude Code는 기본적으로 파일과 터미널만 다룹니다. MCP를 연결하면:

- **데이터베이스 직접 연결**: SQL 쿼리 실행
- **외부 API 통합**: GitHub, Slack, JIRA 등
- **맞춤형 도구 추가**: 필요한 기능 확장

---

## MCP란?

MCP(Model Context Protocol)는 Claude에게 새로운 능력을 추가하는 표준 프로토콜입니다.

### 기본 구조

```
┌─────────────────────────────────────────────────────────────────┐
│                        MCP 아키텍처                              │
└─────────────────────────────────────────────────────────────────┘

┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Claude Code  │ ◀──▶│  MCP Server  │ ◀──▶│ 외부 서비스   │
│              │stdio│              │     │              │
│              │ or  │ (중간 다리)   │     │ DB, API 등   │
│              │HTTP │              │     │              │
└──────────────┘     └──────────────┘     └──────────────┘
```

MCP 서버가 Claude와 외부 서비스 사이에서 통역사 역할을 합니다.

### 이것을 알면 무엇이 좋을까요?

**복사-붙여넣기 제거:**

데이터베이스 결과를 복사해서 Claude에게 붙여넣는 대신:

```
> users 테이블에서 최근 가입자 10명 보여줘
```

Claude가 직접 데이터베이스를 조회합니다.

---

## MCP 서버 설정하기

### 설정 파일 위치

```
~/.claude/mcp_servers.json
```

### 기본 구조

```json
{
  "servers": {
    "서버이름": {
      "command": "실행할 명령어",
      "args": ["인자1", "인자2"],
      "env": {
        "환경변수": "값"
      }
    }
  }
}
```

### 예시: PostgreSQL 연결

```json
{
  "servers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost:5432/mydb"
      }
    }
  }
}
```

---

## 자주 쓰는 MCP 서버

### 1. 파일 시스템

```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-filesystem", "/allowed/path"]
    }
  }
}
```

지정된 경로의 파일에 접근할 수 있습니다.

### 2. PostgreSQL

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

```
> users 테이블 구조 보여줘
> 최근 주문 10개 조회해줘
> 이 사용자의 구매 이력 분석해줘
```

### 3. GitHub

```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_xxxx"
      }
    }
  }
}
```

```
> 이 레포의 최근 PR들 보여줘
> 이슈 #42 내용 확인해줘
> 이 PR에 리뷰 코멘트 달아줘
```

### 4. Slack

```json
{
  "servers": {
    "slack": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-slack"],
      "env": {
        "SLACK_TOKEN": "xoxb-xxxx"
      }
    }
  }
}
```

```
> #dev 채널에 배포 완료 메시지 보내줘
> 오늘 #bugs 채널에 올라온 내용 요약해줘
```

---

## 실습: 데이터베이스 연동

### 1. MCP 서버 설정

```json
// ~/.claude/mcp_servers.json
{
  "servers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://localhost/ecommerce"
      }
    }
  }
}
```

### 2. 실제 사용

```
> 데이터베이스에 어떤 테이블들이 있어?
```

Claude가 직접 스키마를 조회합니다.

```
> orders 테이블에서 이번 달 매출 계산해줘
```

SQL 쿼리를 작성하고 실행합니다.

```
> 가장 많이 팔린 상품 Top 10 분석해줘
```

복잡한 분석도 직접 수행합니다.

### 3. 안전하게 사용하기

**읽기 전용 계정 사용:**

```
DATABASE_URL=postgresql://readonly_user:pass@localhost/mydb
```

**쿼리 제한:**

CLAUDE.md에 규칙 추가:

```markdown
# DB 접근 규칙
- SELECT만 허용
- DELETE, DROP, TRUNCATE 금지
- 대량 UPDATE 전에 반드시 확인 요청
```

---

## 실습: GitHub 연동

### 1. 설정

```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_your_token_here"
      }
    }
  }
}
```

### 2. 활용 예시

**이슈 관리:**

```
> 열린 이슈 중에 버그 라벨 붙은 것들 보여줘
> 이슈 #123 내용 요약해줘
> 이 이슈 해결하려면 어디를 고쳐야 해?
```

**PR 리뷰:**

```
> 이 PR의 변경사항 분석해줘
> 코드 품질 문제가 있으면 알려줘
> 리뷰 코멘트 작성해줘
```

**릴리즈 노트:**

```
> 최근 10개 커밋 분석해서 릴리즈 노트 작성해줘
```

---

## 여러 MCP 서버 조합

### 통합 워크플로우

```json
{
  "servers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://localhost/mydb"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_xxxx"
      }
    },
    "slack": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-slack"],
      "env": {
        "SLACK_TOKEN": "xoxb-xxxx"
      }
    }
  }
}
```

### 통합 사용 예시

```
> GitHub 이슈 #42 확인하고, 관련 DB 데이터 분석해서,
> 결과를 #dev 채널에 공유해줘
```

Claude가:
1. GitHub에서 이슈 내용 확인
2. 데이터베이스에서 관련 데이터 조회
3. 분석 결과를 Slack에 전송

---

## 커스텀 MCP 서버 만들기

### 간단한 MCP 서버 구조

```javascript
// my-mcp-server.js
const { Server } = require('@modelcontextprotocol/sdk/server')

const server = new Server({
  name: 'my-custom-server',
  version: '1.0.0'
})

// 도구 정의
server.tool({
  name: 'my_custom_tool',
  description: '내 커스텀 도구',
  inputSchema: {
    type: 'object',
    properties: {
      query: { type: 'string' }
    }
  },
  handler: async ({ query }) => {
    // 도구 로직
    return { result: `처리 결과: ${query}` }
  }
})

server.listen()
```

### 설정에 추가

```json
{
  "servers": {
    "my-custom": {
      "command": "node",
      "args": ["/path/to/my-mcp-server.js"]
    }
  }
}
```

---

## 보안 주의사항

### 1. 토큰 관리

```json
// 나쁜 예 - 하드코딩
{
  "env": {
    "GITHUB_TOKEN": "ghp_actual_token_here"
  }
}

// 좋은 예 - 환경변수 참조
{
  "env": {
    "GITHUB_TOKEN": "${GITHUB_TOKEN}"
  }
}
```

### 2. 권한 최소화

- 읽기 전용 토큰 사용
- 필요한 레포/채널만 접근 허용
- 정기적으로 토큰 갱신

### 3. 민감 데이터 보호

CLAUDE.md에 규칙 추가:

```markdown
# 보안 규칙
- 개인정보 조회 시 목적 확인
- 비밀번호, 토큰 등 민감정보 출력 금지
- 대량 데이터 접근 시 확인 요청
```

---

## 따라해보십시오

### 실습 1: Filesystem MCP 설정

위의 단계별 가이드를 따라 filesystem MCP 서버를 설정하시기 바랍니다. 허용된 경로에서 파일을 읽어달라고 요청해서 테스트하시기 바랍니다.

### 실습 2: GitHub MCP 추가 (토큰이 있다면)

1. GitHub 개인 접근 토큰 받기
2. 설정에 추가:

```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "여기에-토큰"
      }
    }
  }
}
```

3. 테스트: `> 내 최근 GitHub 저장소 보여줘`

### 실습 3: 여러 서버 조합

filesystem과 GitHub 둘 다 설정에 추가:

```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-filesystem", "/tmp"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "your-token"
      }
    }
  }
}
```

---

## 문제가 발생하면?

### 문제: MCP 서버가 로드되지 않습니다

**가능한 원인:**
1. 설정의 JSON 문법 에러
2. 파일 위치가 잘못됨
3. npm/npx가 없음

**해결 방법:**
- JSON 검증: `cat ~/.claude/mcp_servers.json | jq .`
- 위치 확인: 반드시 `~/.claude/mcp_servers.json`
- npx 확인: `which npx` (경로가 보여야 함)

### 문제: "권한 거부" 에러

**가능한 원인:**
1. 파일/폴더 접근이 제한됨
2. API 토큰이 없거나 유효하지 않음
3. 네트워크 방화벽이 차단

**해결 방법:**
- 파일 권한 확인
- API 토큰이 유효한지 검증
- `/tmp` 같은 더 간단한 경로로 시도

### 문제: MCP 서버는 시작하는데 Claude가 사용하지 못합니다

**가능한 원인:**
1. 서버 이름 불일치
2. 시작 후 서버가 크래시
3. 도구 이름이 예상과 다름

**해결 방법:**
- 설정의 서버 이름이 요청하는 것과 일치하는지 확인
- Claude Code 로그에서 에러 확인
- Claude Code 재시작

### 문제: 느리거나 타임아웃

**가능한 원인:**
1. 네트워크 문제
2. 서버 과부하
3. 너무 많은 데이터 요청

**해결 방법:**
- 인터넷 연결 확인
- 더 작은 요청으로 시작
- 더 구체적인 쿼리 사용

---

## 자주 하는 실수

1. **설정에 비밀 하드코딩**
   ```json
   // 나쁨 - 평문 토큰
   { "env": { "TOKEN": "ghp_abc123..." } }

   // 더 나음 - 환경 변수 사용
   { "env": { "TOKEN": "${GITHUB_TOKEN}" } }
   ```

2. **잘못된 JSON 형식**
   - 기억하십시오: 주석 불가, 마지막 쉼표 불가
   - 항상 `jq`로 검증하시기 바랍니다

3. **너무 허용적인 접근**
   - 전체 홈 폴더에 MCP 접근을 주지 마십시오
   - 가능하면 읽기 전용 토큰을 사용하십시오

4. **재시작 잊기**
   - `mcp_servers.json` 변경 후 Claude Code를 재시작하십시오
   - 변경이 자동으로 적용되지 않습니다

5. **로그 확인 안 하기**
   - 실패하면 Claude Code 로그를 확인하십시오
   - MCP 서버가 종종 도움이 되는 에러 메시지를 출력합니다

---

## 정리

이번 챕터에서 배운 것:
- [x] MCP 개념과 아키텍처
- [x] MCP 서버 설정 방법
- [x] 자주 쓰는 MCP 서버 (DB, GitHub, Slack)
- [x] 여러 서버 조합 사용
- [x] 보안 주의사항

**핵심 포인트**: MCP로 Claude의 능력을 무한 확장할 수 있습니다.

다음 챕터에서는 CI/CD로 자동화 파이프라인을 구축하는 방법을 배웁니다.

[Chapter 23: CI/CD 자동화](../Chapter23/README.ko.md)로 넘어가시기 바랍니다.
