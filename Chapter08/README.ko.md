# Chapter 08: MCP 서버

**한국어** | [English](./README.md)

## Prerequisites

이 Chapter를 시작하기 전에:
- [ ] Chapter 00-07 완료
- [ ] API 개념과 HTTP 이해
- [ ] JSON과 환경 변수에 대한 기본 지식

---

## Introduction

MCP (Model Context Protocol) 서버는 Claude Code를 외부 도구, 데이터베이스, 서비스에 연결하여 기능을 확장합니다. MCP를 로컬 파일을 넘어 세계와 상호작용할 수 있게 해주는 플러그인 시스템이라고 생각하세요.

### 왜 MCP인가?

- **데이터베이스 접근**: PostgreSQL, MongoDB 등 쿼리
- **API 통합**: GitHub, Sentry, Jira 연결
- **커스텀 도구**: 자체 통합 구축
- **팀 공유**: 프로젝트 간 MCP 설정 공유

---

## Topics

### 1. MCP 이해하기

MCP는 Claude가 다음을 수행할 수 있는 표준화된 방법을 제공합니다:
- 사용 가능한 도구 발견
- 외부 서비스 호출
- 구조화된 응답 수신

**전송 유형**:
| 유형 | 설명 | 사용 사례 |
|------|------|----------|
| `stdio` | 로컬 프로세스 통신 | 로컬 도구, 스크립트 |
| `http` | HTTP/HTTPS 연결 | 클라우드 서비스 |
| `sse` | Server-sent events (deprecated) | 레거시 시스템 |

### 2. MCP 서버 추가

#### CLI 명령 사용

```bash
# HTTP MCP 서버 추가
claude mcp add --transport http sentry https://mcp.sentry.dev/mcp

# 로컬 (stdio) MCP 서버 추가
claude mcp add --transport stdio postgres -- npx @anthropic-ai/mcp-postgres

# 환경 변수와 함께 추가
claude mcp add --transport stdio github -- \
  npx @anthropic-ai/mcp-github \
  --env GITHUB_TOKEN=your_token

# 설정된 서버 목록
claude mcp list

# 서버 제거
claude mcp remove sentry
```

### 3. 설정 파일

**프로젝트 MCP** (`.mcp.json` - git 추적):
```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["@anthropic-ai/mcp-postgres"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    },
    "github": {
      "command": "npx",
      "args": ["@anthropic-ai/mcp-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**사용자 MCP** (`~/.claude.json`):
```json
{
  "mcpServers": {
    "personal-api": {
      "type": "http",
      "url": "https://my-api.example.com/mcp"
    }
  }
}
```

### 4. 인기 MCP 서버

| 서버 | 목적 | 설치 |
|------|------|------|
| **PostgreSQL** | 데이터베이스 쿼리 | `npx @anthropic-ai/mcp-postgres` |
| **GitHub** | 이슈, PR, 저장소 | `npx @anthropic-ai/mcp-github` |
| **Sentry** | 에러 모니터링 | HTTP: `mcp.sentry.dev` |
| **Slack** | 메시징 | `npx @anthropic-ai/mcp-slack` |
| **Filesystem** | 확장 파일 작업 | `npx @anthropic-ai/mcp-filesystem` |

### 5. MCP 도구 사용

설정하면 Claude가 MCP 도구를 자연스럽게 사용할 수 있습니다:

```bash
# PostgreSQL MCP와 함께
> users 테이블에서 모든 프리미엄 구독자를 쿼리해줘

# GitHub MCP와 함께
> 이 저장소에서 "bug" 라벨이 붙은 모든 열린 이슈 나열

# Sentry MCP와 함께
> 가장 최근의 미해결 에러를 보여줘
```

### 6. 환경 변수

자격 증명의 안전한 처리:

```bash
# 환경 변수 설정
export DATABASE_URL="postgresql://user:pass@localhost/db"
export GITHUB_TOKEN="ghp_xxxx"

# 설정에서 ${VAR_NAME}으로 참조
```

**모범 사례**:
- 자격 증명을 절대 git에 커밋하지 않기
- 로컬 개발에 `.env` 파일 사용
- MCP 설정에서 변수 참조

### 7. 커스텀 MCP 서버 만들기

기본 MCP 서버 구조 (Node.js):

```javascript
// my-mcp-server.js
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new Server({
  name: "my-custom-server",
  version: "1.0.0",
});

// 도구 정의
server.setRequestHandler("tools/list", async () => ({
  tools: [
    {
      name: "get_weather",
      description: "위치의 날씨 가져오기",
      inputSchema: {
        type: "object",
        properties: {
          city: { type: "string" }
        }
      }
    }
  ]
}));

// 도구 호출 처리
server.setRequestHandler("tools/call", async (request) => {
  if (request.params.name === "get_weather") {
    const { city } = request.params.arguments;
    // 날씨 데이터 가져오기...
    return { result: `${city}의 날씨: 맑음, 22°C` };
  }
});

// 서버 시작
const transport = new StdioServerTransport();
await server.connect(transport);
```

### 8. MCP 디버깅

```bash
# MCP 상태 확인
claude mcp list

# MCP 서버 수동 테스트
npx @anthropic-ai/mcp-postgres --help

# MCP 로그 보기
claude --debug

# 환경 변수 확인
echo $DATABASE_URL
```

---

## Resources

- [Claude Code 공식 문서](https://code.claude.com/docs)
- [MCP Integration Guide](https://code.claude.com/docs/en/mcp)
- [MCP 문서](https://modelcontextprotocol.io/)
- [MCP 서버 디렉토리](https://github.com/modelcontextprotocol/servers)
- [MCP 서버 구축 가이드](https://modelcontextprotocol.io/docs/building)

---

## Checklist

면접에서 답변하듯이 다음 질문에 답해보세요:

1. **MCP란 무엇이고 왜 사용하나요?**
   <details>
   <summary>힌트</summary>
   Claude를 외부 도구/서비스에 연결하는 프로토콜: 데이터베이스, API, 커스텀 도구
   </details>

2. **다양한 MCP 전송 유형은 무엇인가요?**
   <details>
   <summary>힌트</summary>
   stdio (로컬), http (원격), sse (deprecated)
   </details>

3. **MCP 설정은 어디에 저장해야 하나요?**
   <details>
   <summary>힌트</summary>
   프로젝트: .mcp.json (git 추적). 사용자: ~/.claude.json (개인)
   </details>

4. **MCP 서버의 자격 증명을 어떻게 안전하게 처리하나요?**
   <details>
   <summary>힌트</summary>
   환경 변수, ${VAR} 참조, 절대 git에 커밋하지 않기
   </details>

---

## Mini Project: 연결된 개발 환경

### Project Goals

MCP 기반 개발 환경을 구축하세요:

- [ ] 데이터베이스 통합 설정 (PostgreSQL, MySQL, 또는 MongoDB)
- [ ] 이슈 트래커 통합 설정 (GitHub Issues, Jira, 또는 Linear)
- [ ] 모니터링 통합 설정 (Sentry, Datadog, 또는 커스텀 로깅)
- [ ] 1개 이상의 선택 통합 추가 (Slack, Notion, CI/CD 등)
- [ ] 완전한 `.mcp.json` 설정 파일 생성
- [ ] 환경 변수로 자격 증명 안전하게 처리
- [ ] 각 통합이 end-to-end로 작동하는지 테스트

### Ideas to Try

- 자체 API를 위한 커스텀 MCP 서버 구축
- 모니터링을 코드 수정에 연결하는 워크플로우 생성
- Slack/Discord로 알림을 보내는 알림 설정
- 데이터 조사를 위해 데이터베이스 쿼리와 이슈 추적 결합

---

## Advanced

### 인기 있는 MCP 서버 탐색

[MCP 서버 디렉토리](https://github.com/modelcontextprotocol/servers)에서 유용한 서버를 찾아 설치해보세요:

```bash
# 파일시스템 서버 (로컬 파일 접근)
npx -y @anthropic-ai/mcp-server-filesystem

# 브라우저 자동화 서버
npx -y @anthropic-ai/mcp-server-puppeteer

# Git 서버 (고급 git 작업)
npx -y @anthropic-ai/mcp-server-git
```

### MCP 서버 조합 활용

여러 MCP 서버를 함께 사용하는 실전 워크플로우:

```bash
# GitHub Issues + Database 조합
> Look up issue #123 from GitHub, then query our database
> to find related user complaints from the last week

# Slack + Git 조합
> Summarize today's git commits and post to #dev-updates channel
```

### 간단한 커스텀 MCP 서버 만들기

정말 필요하다면, 최소한의 MCP 서버를 만들어보세요:

```javascript
// simple-mcp-server.js
import { Server } from "@anthropic-ai/mcp-server";

const server = new Server({
  name: "my-tools",
  version: "1.0.0"
});

server.addTool({
  name: "get_weather",
  description: "Get current weather for a city",
  parameters: { city: { type: "string" } },
  handler: async ({ city }) => {
    // 실제로는 날씨 API 호출
    return { temp: 22, condition: "sunny" };
  }
});

server.start();
```

> **참고**: 대부분의 경우 기존 MCP 서버로 충분합니다. 커스텀 서버는 정말 특수한 니즈가 있을 때만 만드세요.
