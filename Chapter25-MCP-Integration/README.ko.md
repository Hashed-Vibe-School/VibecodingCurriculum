# Chapter 25: MCP 연동

[English](./README.md) | **한국어**

---

## 💬 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이전 챕터와의 연결

[Chapter 24: Agents & Skills](../Chapter24-Agents-and-Skills/README.ko.md)에서 배운 Agents와 Skills가 기억나시나요?

- **Agents**: Claude에게 특정 역할과 성격을 부여
- **Skills**: 키워드로 자동화된 워크플로우 실행

이제 한 단계 더 나아갑니다:

| 개념 | 비유 | 특징 |
|------|------|------|
| Agents | 역할 연기 | Claude의 성격과 행동 방식 변경 |
| Skills | 자동화된 레시피 | 미리 정의된 작업 자동 수행 |
| **MCP** | **외부 전화 연결** | **Claude가 외부 서비스와 직접 대화** |

Chapter 24에서 배운 Agents와 Skills는 **Claude 내부에서** 일어나는 일이었다면, MCP는 **Claude를 외부 세계와 연결**하는 것입니다. Agent가 DB 전문가 역할을 해도 실제 DB에 접근할 수 없었지만, MCP를 연결하면 진짜로 데이터베이스를 조회할 수 있게 됩니다!

---

## 이 챕터에서 배우는 것

- MCP(Model Context Protocol) 개념과 동작 원리
- MCP 서버 설정 및 연결 방법
- 외부 서비스 연결하기 (DB, GitHub, Slack 등)
- 실용적인 MCP 서버 활용
- 보안과 권한 관리
- 커스텀 MCP 서버 만들기

---

## 왜 MCP가 중요한가요?

### 실제 상황: 복사-붙여넣기 지옥

**MCP 없이 작업하면:**

```
나: "최근 주문 중에 문제가 있어. 확인 좀 해줘"

Claude: "네, 데이터베이스에서 주문 정보를 조회해주세요."

나: (터미널 열고, PostgreSQL 접속하고, 쿼리 작성하고...)
    SELECT * FROM orders WHERE status = 'error' LIMIT 10;
    (결과 복사해서 Claude에 붙여넣기)

Claude: "user_id 123의 주문들이 문제네요. 해당 사용자 정보도 볼 수 있을까요?"

나: (다시 터미널로 가서...)
    SELECT * FROM users WHERE id = 123;
    (또 복사해서 붙여넣기)

Claude: "결제 정보도 확인하면 좋겠어요."

나: (또 터미널...)
    SELECT * FROM payments WHERE order_id IN (...)
    (또또 복사해서 붙여넣기)

... 이 과정 반복 ...
```

**MCP를 연결하면:**

```
나: "최근 주문 중에 문제가 있어. 확인 좀 해줘"

Claude: [DB 직접 조회]
       "orders 테이블을 확인해봤습니다. status가 'error'인 주문이 3건 있네요.

        - 주문 #1234: user_id 123, 결제 실패
        - 주문 #1235: user_id 123, 재고 부족
        - 주문 #1238: user_id 456, 배송지 오류

        user_id 123이 2건으로 가장 많네요. 해당 사용자 정보를 확인해보니
        이 사용자는 신용카드가 만료된 상태입니다. 카드 갱신 안내 메일을
        보내드릴까요?"

나: "오 좋아! 그렇게 해줘"
```

**결과**: 5분 걸리던 작업이 30초로 단축!

### MCP가 실제로 해결하는 문제들

| 문제 상황 | MCP 없이 | MCP 사용 시 |
|-----------|----------|-------------|
| DB 조회 | 터미널 → 쿼리 → 복사 → 붙여넣기 | "DB에서 최근 주문 보여줘" |
| GitHub 이슈 확인 | 브라우저 열기 → 찾기 → 복사 → 붙여넣기 | "이슈 #42 내용 확인해줘" |
| Slack 메시지 전송 | 앱 열기 → 채널 찾기 → 직접 타이핑 | "#dev 채널에 배포 완료 알려줘" |
| API 호출 | curl 명령어 작성 → 결과 복사 → 붙여넣기 | "날씨 API로 서울 날씨 확인해줘" |
| 문서 검색 | 여러 파일 열기 → 검색 → 복사 → 붙여넣기 | "프로젝트 문서에서 API 스펙 찾아줘" |

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

### 또 다른 비유: 비서에게 권한 주기

여러분이 CEO이고 Claude가 유능한 비서라고 상상해보세요:

**권한 없는 비서 (MCP 없음):**
> "사장님, 고객 데이터를 확인해야 하는데요. DB 담당자한테 연락해서 데이터 받아서 저한테 전달해주시면 분석해드릴게요."

**권한 있는 비서 (MCP 있음):**
> "사장님, 고객 데이터 확인해봤습니다. 지난 달 대비 신규 가입이 23% 증가했고, 특히 20대 여성 고객층이 크게 늘었네요. 마케팅팀에 이 분석 결과를 Slack으로 공유할까요?"

MCP는 비서에게 회사 시스템에 접근할 수 있는 **사원증**을 발급해주는 것입니다!

---

## 💡 비전공자 팁

> **"MCP 서버"가 뭔가요?**
>
> "서버"라는 말이 어렵게 들릴 수 있지만, MCP 서버는 그냥 **통역사 프로그램**입니다.
>
> - Claude는 한국어(Claude 언어)를 씁니다
> - PostgreSQL은 SQL(데이터베이스 언어)을 씁니다
> - GitHub는 REST API(웹 언어)를 씁니다
>
> MCP 서버가 이 사이에서 **통역**을 해줍니다:
> ```
> Claude: "사용자 정보 보여줘"
>    ↓ MCP 서버가 통역
> PostgreSQL: "SELECT * FROM users"
>    ↓ MCP 서버가 통역
> Claude: "사용자가 100명 있고, 그 중 활성 사용자는 80명입니다"
> ```
>
> 여러분은 통역사(MCP 서버)를 고용하기만 하면 됩니다. 통역 방법은 몰라도 됩니다!

---

## MCP의 동작 원리

### 기본 아키텍처

```
┌─────────────────────────────────────────────────────────────────────┐
│                        MCP 아키텍처 상세도                            │
└─────────────────────────────────────────────────────────────────────┘

                    여러분의 요청
                         │
                         ▼
┌──────────────────────────────────────┐
│           Claude Code                 │
│  ┌────────────────────────────────┐  │
│  │     MCP 클라이언트 (내장)       │  │
│  │  - 서버 연결 관리               │  │
│  │  - 요청/응답 처리               │  │
│  │  - 도구 호출 조율               │  │
│  └────────────────────────────────┘  │
└──────────────────────────────────────┘
                         │
            stdio 또는 HTTP 프로토콜
                         │
         ┌───────────────┼───────────────┐
         ▼               ▼               ▼
┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│ MCP Server  │  │ MCP Server  │  │ MCP Server  │
│  (postgres) │  │  (github)   │  │  (slack)    │
│             │  │             │  │             │
│ 제공 도구:   │  │ 제공 도구:   │  │ 제공 도구:   │
│ - query     │  │ - get_issue │  │ - send_msg  │
│ - schema    │  │ - create_pr │  │ - read_ch   │
│ - tables    │  │ - review    │  │ - list_ch   │
└─────────────┘  └─────────────┘  └─────────────┘
         │               │               │
         ▼               ▼               ▼
┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│ PostgreSQL  │  │ GitHub API  │  │ Slack API   │
│  Database   │  │             │  │             │
└─────────────┘  └─────────────┘  └─────────────┘
```

### MCP 통신 흐름 상세

```
┌────────────────────────────────────────────────────────────────────┐
│                    MCP 요청-응답 흐름                                │
└────────────────────────────────────────────────────────────────────┘

1. 사용자 요청
   ┌─────────────────────────────────────────────────┐
   │ "최근 주문 중 오류가 있는 것 찾아서 분석해줘"       │
   └─────────────────────────────────────────────────┘
                            │
                            ▼
2. Claude 분석 및 도구 선택
   ┌─────────────────────────────────────────────────┐
   │ Claude: "이 요청을 처리하려면 postgres MCP의     │
   │         query 도구를 사용해야겠다"               │
   └─────────────────────────────────────────────────┘
                            │
                            ▼
3. MCP 서버 호출
   ┌─────────────────────────────────────────────────┐
   │ Claude → postgres MCP:                          │
   │ {                                               │
   │   "method": "tools/call",                       │
   │   "params": {                                   │
   │     "name": "query",                            │
   │     "arguments": {                              │
   │       "sql": "SELECT * FROM orders              │
   │               WHERE status = 'error'"           │
   │     }                                           │
   │   }                                             │
   │ }                                               │
   └─────────────────────────────────────────────────┘
                            │
                            ▼
4. MCP 서버가 외부 서비스 호출
   ┌─────────────────────────────────────────────────┐
   │ postgres MCP → PostgreSQL DB:                   │
   │ SELECT * FROM orders WHERE status = 'error'     │
   └─────────────────────────────────────────────────┘
                            │
                            ▼
5. 결과 반환
   ┌─────────────────────────────────────────────────┐
   │ PostgreSQL → postgres MCP → Claude:             │
   │ [                                               │
   │   {id: 1234, user_id: 123, status: "error"},   │
   │   {id: 1235, user_id: 123, status: "error"},   │
   │   ...                                           │
   │ ]                                               │
   └─────────────────────────────────────────────────┘
                            │
                            ▼
6. 사용자에게 응답
   ┌─────────────────────────────────────────────────┐
   │ Claude: "오류 상태인 주문이 3건 있습니다.        │
   │ 분석 결과..."                                   │
   └─────────────────────────────────────────────────┘
```

---

## 첫 MCP 설정 (단계별)

간단한 MCP 서버를 설정해봅시다. 외부 계정이 필요 없는 filesystem 서버를 사용하겠습니다.

### 단계 1: 설정 파일 만들기

프로젝트 루트에 MCP 설정 파일을 만드시기 바랍니다:

```bash
# macOS/Linux
touch .mcp.json

# Windows (PowerShell)
New-Item -ItemType File -Force -Path ".mcp.json"
```

### 단계 2: 간단한 서버 추가

`.mcp.json`을 열고 추가하시기 바랍니다:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-filesystem", "/tmp"]
    }
  }
}
```

이렇게 하면 Claude가 MCP를 통해 `/tmp`의 파일에 접근할 수 있습니다.

> **Windows 사용자**: `/tmp` 대신 `C:/Temp` 또는 원하는 폴더 경로를 사용하세요.

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

### JSON 설정 구조 상세 분석

```json
{
  "servers": {
    "my-database": {
      // ========== 필수 설정 ==========

      "command": "npx",
      // 실행할 프로그램
      // - "npx": npm 패키지 실행 (가장 흔함)
      // - "node": Node.js 스크립트 직접 실행
      // - "python": Python 스크립트 실행
      // - 경로: "/usr/local/bin/my-server"

      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      // 프로그램에 전달할 인자들
      // - "-y": npx에서 설치 확인 자동 승인
      // - 패키지명 또는 스크립트 경로
      // - 추가 옵션들

      // ========== 선택 설정 ==========

      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/db",
        "LOG_LEVEL": "info"
      },
      // 환경 변수 설정
      // - 민감한 정보 (API 키, 비밀번호)
      // - 설정값 (로그 레벨, 타임아웃 등)

      "cwd": "/path/to/working/directory",
      // 작업 디렉토리 (선택사항)
      // - 서버가 실행될 기준 경로

      "timeout": 30000
      // 타임아웃 (밀리초, 선택사항)
      // - 서버 응답 대기 시간
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

**패턴 4: 로컬 스크립트 실행**
```json
{
  "command": "node",
  "args": ["/home/user/my-mcp-servers/custom-server.js"],
  "cwd": "/home/user/my-mcp-servers"
}
```

**패턴 5: Python MCP 서버**
```json
{
  "command": "python",
  "args": ["-m", "my_mcp_server"],
  "env": {
    "PYTHONPATH": "/path/to/modules"
  }
}
```

---

## 🔥 프로 팁

> **환경 변수로 비밀 관리하기**
>
> API 키나 비밀번호를 설정 파일에 직접 쓰지 마세요!
>
> ```json
> // ❌ 나쁜 예 - 비밀이 노출됨
> {
>   "env": {
>     "GITHUB_TOKEN": "ghp_abc123xyz789..."
>   }
> }
>
> // ✅ 좋은 예 - 환경 변수 참조
> {
>   "env": {
>     "GITHUB_TOKEN": "${GITHUB_TOKEN}"
>   }
> }
> ```
>
> 그리고 쉘 설정 파일에서 환경 변수를 설정하세요:
> ```bash
> # ~/.bashrc 또는 ~/.zshrc
> export GITHUB_TOKEN="ghp_abc123xyz789..."
> ```
>
> 이렇게 하면:
> 1. 설정 파일을 Git에 커밋해도 비밀이 노출되지 않음
> 2. 팀원마다 다른 토큰 사용 가능
> 3. 토큰 교체가 쉬움

---

## 왜 MCP를 알아야 할까?

Claude Code는 기본적으로 파일과 터미널만 다룹니다. MCP를 연결하면:

- **데이터베이스 직접 연결**: SQL 쿼리 실행
- **외부 API 통합**: GitHub, Slack, JIRA 등
- **맞춤형 도구 추가**: 필요한 기능 확장

### 실제 현업에서의 MCP 활용

| 상황 | MCP 없이 | MCP 있으면 |
|------|----------|-----------|
| 버그 조사 | DB 결과를 수동으로 복사/붙여넣기 반복 | "이 에러 로그 관련 DB 레코드 확인해줘" |
| 코드 리뷰 | GitHub PR 내용 수동으로 복사 | "PR #123 리뷰하고 코멘트 달아줘" |
| 배포 알림 | Slack 직접 열어서 메시지 작성 | "배포 완료됐으니 #dev 채널에 알려줘" |
| API 테스트 | curl 결과를 복사해서 분석 요청 | "이 엔드포인트 응답 분석해줘" |
| 문서 업데이트 | 여러 서비스 오가며 정보 수집 | "변경사항 확인하고 README 업데이트해줘" |

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

## 자주 쓰는 MCP 서버 완전 가이드

### 1. 파일 시스템 (Filesystem)

**용도**: 특정 폴더의 파일 접근

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

**사용 예시:**

```
> /allowed/path 폴더에 있는 모든 JSON 파일 목록 보여줘
> config.json 파일 내용 읽어줘
> 새로운 설정 파일 만들어줘
```

**활용 시나리오:**
- 프로젝트 외부의 설정 파일 관리
- 로그 파일 분석
- 문서 파일 접근

### 2. PostgreSQL

**용도**: PostgreSQL 데이터베이스 직접 조회

```json
{
  "servers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:password@localhost:5432/mydb"
      }
    }
  }
}
```

**사용 예시:**

```
> 데이터베이스에 어떤 테이블들이 있어?
> users 테이블 구조 보여줘
> 최근 주문 10개 조회해줘
> 이 사용자의 구매 이력 분석해줘
> 이번 달 매출 통계 내줘
```

**MCP 제공 도구:**

| 도구 이름 | 설명 | 예시 |
|-----------|------|------|
| `list_tables` | 테이블 목록 조회 | 모든 테이블 보기 |
| `describe_table` | 테이블 구조 확인 | 컬럼, 타입, 키 정보 |
| `query` | SQL 쿼리 실행 | SELECT 문 실행 |

### 3. MySQL

**용도**: MySQL 데이터베이스 직접 조회

```json
{
  "servers": {
    "mysql": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-mysql"],
      "env": {
        "MYSQL_HOST": "localhost",
        "MYSQL_PORT": "3306",
        "MYSQL_USER": "root",
        "MYSQL_PASSWORD": "password",
        "MYSQL_DATABASE": "mydb"
      }
    }
  }
}
```

**사용 예시:**

```
> 데이터베이스 스키마 분석해줘
> products 테이블에서 재고가 부족한 상품 찾아줘
> 지난 주 신규 가입자 수 알려줘
```

### 4. GitHub

**용도**: GitHub 이슈, PR, 레포 관리

```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**사용 예시:**

```
> 이 레포의 최근 PR들 보여줘
> 이슈 #42 내용 확인해줘
> 버그 라벨이 붙은 열린 이슈 목록 보여줘
> 이 PR에 리뷰 코멘트 달아줘
> 최근 10개 커밋으로 릴리즈 노트 작성해줘
```

**MCP 제공 도구:**

| 도구 이름 | 설명 | 예시 |
|-----------|------|------|
| `list_issues` | 이슈 목록 조회 | 열린 이슈 보기 |
| `get_issue` | 이슈 상세 조회 | 이슈 내용, 코멘트 |
| `create_issue` | 이슈 생성 | 새 버그 리포트 |
| `list_prs` | PR 목록 조회 | 리뷰 대기 PR |
| `get_pr` | PR 상세 조회 | 변경사항 확인 |
| `create_pr_review` | PR 리뷰 작성 | 코멘트 달기 |

### 5. Slack

**용도**: Slack 메시지 읽기/보내기

```json
{
  "servers": {
    "slack": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-slack"],
      "env": {
        "SLACK_TOKEN": "${SLACK_BOT_TOKEN}"
      }
    }
  }
}
```

**Slack 토큰 얻기:**
1. https://api.slack.com/apps 접속
2. "Create New App" 클릭
3. "OAuth & Permissions"에서 필요한 스코프 추가
4. 워크스페이스에 앱 설치
5. "Bot User OAuth Token" 복사

**필요한 스코프:**
- `channels:read` - 채널 목록 읽기
- `channels:history` - 채널 메시지 읽기
- `chat:write` - 메시지 보내기

**사용 예시:**

```
> #dev 채널에 "배포 완료" 메시지 보내줘
> 오늘 #bugs 채널에 올라온 내용 요약해줘
> #general 채널의 최근 메시지 10개 보여줘
```

### 6. Notion

**용도**: Notion 페이지/데이터베이스 관리

```json
{
  "servers": {
    "notion": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-notion"],
      "env": {
        "NOTION_API_KEY": "${NOTION_API_KEY}"
      }
    }
  }
}
```

**사용 예시:**

```
> 프로젝트 문서 페이지 내용 읽어줘
> 태스크 데이터베이스에 새 항목 추가해줘
> 이번 주 완료된 작업들 정리해줘
```

### 7. Linear

**용도**: Linear 이슈 트래킹

```json
{
  "servers": {
    "linear": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-linear"],
      "env": {
        "LINEAR_API_KEY": "${LINEAR_API_KEY}"
      }
    }
  }
}
```

**사용 예시:**

```
> 내 담당 이슈 목록 보여줘
> 이번 스프린트 진행 상황 요약해줘
> 새 버그 이슈 만들어줘
```

---

## ⚠️ 주의사항

> **MCP 서버 연결 시 보안 주의점**
>
> MCP는 강력하지만, 잘못 설정하면 보안 위험이 있습니다:
>
> 1. **읽기 전용 권한 사용**: 가능하면 DB는 SELECT만 허용하는 계정 사용
> 2. **최소 권한 원칙**: GitHub 토큰은 필요한 레포에만 접근 허용
> 3. **민감 데이터 주의**: 개인정보가 있는 테이블 접근 시 주의
> 4. **프로덕션 DB 직접 연결 금지**: 개발/스테이징 DB 사용 권장
>
> ```markdown
> # CLAUDE.md에 보안 규칙 추가
>
> ## MCP 사용 규칙
> - DELETE, DROP, TRUNCATE 쿼리 금지
> - 개인정보 테이블(users.email, users.phone) 조회 시 목적 확인
> - 1000건 이상 조회 시 사전 확인
> - 프로덕션 환경에서의 쓰기 작업 금지
> ```

---

## MCP 서버 설정하기

### 설정 파일 위치

```
.mcp.json (프로젝트 루트)
```

> **참고**: 모든 프로젝트에 적용되는 사용자 전체 설정은 `~/.claude.json`을 사용할 수도 있습니다.

### 기본 구조

```json
{
  "mcpServers": {
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
  "mcpServers": {
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

## 실습: 데이터베이스 연동

### 1. MCP 서버 설정

```json
// .mcp.json
{
  "mcpServers": {
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

```sql
-- PostgreSQL에서 읽기 전용 사용자 생성
CREATE USER readonly_user WITH PASSWORD 'secure_password';
GRANT CONNECT ON DATABASE mydb TO readonly_user;
GRANT USAGE ON SCHEMA public TO readonly_user;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO readonly_user;

-- 향후 생성되는 테이블에도 자동 적용
ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT SELECT ON TABLES TO readonly_user;
```

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

### 2. GitHub 토큰 얻기

1. GitHub → Settings → Developer settings → Personal access tokens
2. "Generate new token (classic)" 클릭
3. 필요한 권한 선택:
   - `repo` - 프라이빗 레포 접근
   - `public_repo` - 퍼블릭 레포만
   - `read:org` - 조직 정보 읽기
4. 토큰 생성 후 안전하게 저장

### 3. 활용 예시

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
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "slack": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-slack"],
      "env": {
        "SLACK_TOKEN": "${SLACK_BOT_TOKEN}"
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

## Claude와의 실전 대화 예시

### 예시 1: 데이터베이스 분석

```
나: "이번 달 매출이 좀 이상해. 뭐가 문제인지 분석해줘"

Claude: [postgres MCP 사용하여 orders 테이블 조회]

       이번 달 매출 데이터를 분석해봤습니다.

       📊 매출 현황:
       - 이번 달 총 매출: ₩45,230,000
       - 지난 달 총 매출: ₩52,180,000
       - 감소율: -13.3%

       🔍 원인 분석:
       1. 주문 건수는 비슷 (1,234건 → 1,189건, -3.6%)
       2. 평균 주문 금액이 감소 (₩42,300 → ₩38,000, -10.2%)

       가장 큰 변화는 "프리미엄" 카테고리 매출이
       35% 감소한 것입니다.

       프리미엄 카테고리 상품별 상세 분석을 볼까요?

나: "응, 그리고 왜 프리미엄 판매가 줄었는지도 알아봐줘"

Claude: [추가 쿼리 실행]

       프리미엄 카테고리 상세 분석입니다:

       📉 판매 감소 상품:
       1. 프리미엄 노트북 A: -45% (재고 부족으로 5일간 품절)
       2. 프리미엄 태블릿 B: -28% (경쟁사 신제품 출시 시점과 일치)
       3. 프리미엄 이어폰 C: -15% (가격 인상 후 감소)

       💡 추천 조치:
       1. 노트북 A 재고 관리 강화 필요
       2. 태블릿 B 가격/프로모션 검토
       3. 이어폰 C 가격 전략 재검토

       이 분석 결과를 마케팅팀 Slack 채널에 공유할까요?
```

### 예시 2: 버그 조사 워크플로우

```
나: "프로덕션에서 결제 실패가 많다는 CS 문의가 왔어. 조사해줘"

Claude: [postgres MCP로 payments 테이블 조회]

       최근 24시간 결제 데이터를 분석했습니다.

       ⚠️ 이상 징후 발견:
       - 전체 결제 시도: 523건
       - 성공: 412건 (78.8%)
       - 실패: 111건 (21.2%)

       평소 실패율 5% 대비 크게 증가했습니다.

       실패 원인 분석:
       - "timeout" 에러: 89건 (80%)
       - "invalid_card": 15건 (14%)
       - "insufficient_funds": 7건 (6%)

       timeout 에러가 대부분이네요.
       결제 게이트웨이 API 응답 시간 문제로 보입니다.

       GitHub에서 관련 이슈가 있는지 확인해볼까요?

나: "응, 그리고 이런 문제가 이전에도 있었는지 확인해줘"

Claude: [github MCP로 이슈 검색]

       관련 GitHub 이슈를 찾았습니다:

       🔍 관련 이슈:
       - #234 "Payment timeout 증가" (2주 전, closed)
         → 해결책: 타임아웃 값 30초→60초로 증가

       - #156 "결제 재시도 로직 개선" (1달 전, closed)
         → API 타임아웃 시 자동 재시도 추가

       이전에도 비슷한 문제가 있었네요.
       #234의 해결책이 다시 필요할 수 있습니다.

       [postgres MCP로 추가 조회]

       타임아웃이 발생한 시간대를 보니,
       오늘 오후 2시부터 집중되어 있습니다.
       결제 게이트웨이 쪽 문제일 가능성이 높습니다.

       인프라팀에 Slack으로 알릴까요?

나: "그래, #infra 채널에 상황 공유해줘"

Claude: [slack MCP로 메시지 전송]

       #infra 채널에 다음 내용을 공유했습니다:

       "🚨 결제 타임아웃 증가 알림

       현황:
       - 최근 24시간 결제 실패율 21.2% (평소 5%)
       - 주원인: timeout 에러 (80%)
       - 발생 시점: 오늘 14:00부터

       가능한 원인:
       - 결제 게이트웨이 API 응답 지연

       관련 이슈: #234 (이전 유사 케이스)

       확인 부탁드립니다. 🙏"
```

### 예시 3: PR 리뷰 자동화

```
나: "PR #89 리뷰 좀 해줘"

Claude: [github MCP로 PR 내용 조회]

       PR #89 "사용자 인증 로직 리팩토링" 리뷰입니다.

       📋 변경 요약:
       - 변경 파일: 5개
       - 추가: +234줄
       - 삭제: -156줄
       - 작성자: @kim-dev

       ✅ 좋은 점:
       1. 인증 로직이 AuthService로 잘 분리됨
       2. 에러 처리가 상세해짐
       3. 테스트 커버리지 증가 (+15%)

       ⚠️ 개선 제안:
       1. auth.service.js:45 - 토큰 만료 시간이 하드코딩됨
          → 환경 변수로 분리 권장

       2. auth.service.js:78 - SQL 인젝션 위험
          → parameterized query 사용 필요

       3. auth.test.js:23 - 엣지 케이스 테스트 누락
          → 만료된 토큰 테스트 추가 권장

       이 내용으로 PR 코멘트를 작성할까요?

나: "보안 문제가 있는 부분 좀 더 자세히 봐줘"

Claude: [PR diff 상세 분석]

       보안 관련 상세 분석입니다:

       🔴 심각도: 높음
       📍 위치: auth.service.js:78-82

       현재 코드:
       ```javascript
       const query = `SELECT * FROM users
                      WHERE email = '${email}'`;
       ```

       문제점:
       - 사용자 입력(email)이 직접 쿼리에 삽입됨
       - SQL 인젝션 공격 가능

       공격 예시:
       email = "'; DROP TABLE users; --"

       권장 수정:
       ```javascript
       const query = 'SELECT * FROM users WHERE email = $1';
       const result = await db.query(query, [email]);
       ```

       이 보안 이슈에 대한 리뷰 코멘트를
       PR에 직접 달까요?

나: "응, 달아줘"

Claude: [github MCP로 PR 코멘트 작성]

       PR #89에 리뷰 코멘트를 작성했습니다:

       📝 추가된 코멘트:

       [auth.service.js:78]
       "🔴 보안 이슈: SQL 인젝션 위험

       현재 코드에서 사용자 입력이 직접 쿼리에
       삽입되고 있습니다. Parameterized query를
       사용해주세요.

       수정 예시:
       ```javascript
       const query = 'SELECT * FROM users WHERE email = $1';
       const result = await db.query(query, [email]);
       ```"

       리뷰 상태를 "Changes Requested"로 설정했습니다.
```

### 예시 4: 에러 발생 상황

```
나: "DB에서 매출 데이터 조회해줘"

Claude: [postgres MCP 호출 시도]

       ❌ MCP 서버 연결 오류가 발생했습니다.

       오류 메시지: "Connection refused: localhost:5432"

       가능한 원인:
       1. PostgreSQL 서버가 실행 중이 아님
       2. DATABASE_URL 설정이 잘못됨
       3. 방화벽이 연결을 차단

       확인해볼 사항:
       ```bash
       # PostgreSQL 실행 상태 확인
       pg_isready -h localhost -p 5432

       # 서비스 상태 확인 (macOS)
       brew services list | grep postgresql

       # 서비스 시작
       brew services start postgresql
       ```

       PostgreSQL 서버를 시작한 후 다시 시도해주세요.

나: "서버 켰어"

Claude: [postgres MCP 재시도]

       ✅ 연결 성공! 매출 데이터를 조회합니다.

       ...
```

---

## 커스텀 MCP 서버 만들기

### 왜 커스텀 MCP 서버가 필요할까?

기본 제공 MCP 서버로 커버되지 않는 경우:
- 회사 내부 API 연동
- 특수한 데이터 소스 접근
- 커스텀 비즈니스 로직 처리

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

### 실용적인 커스텀 MCP 예시: 사내 API 연동

```javascript
// internal-api-mcp.js
const { Server } = require('@modelcontextprotocol/sdk/server');
const axios = require('axios');

const server = new Server({
  name: 'internal-api',
  version: '1.0.0'
});

// 사내 직원 정보 조회
server.tool({
  name: 'get_employee',
  description: '사번으로 직원 정보 조회',
  inputSchema: {
    type: 'object',
    properties: {
      employeeId: {
        type: 'string',
        description: '직원 사번 (예: EMP001)'
      }
    },
    required: ['employeeId']
  },
  handler: async ({ employeeId }) => {
    try {
      const response = await axios.get(
        `${process.env.INTERNAL_API_URL}/employees/${employeeId}`,
        {
          headers: {
            'Authorization': `Bearer ${process.env.INTERNAL_API_TOKEN}`
          }
        }
      );
      return {
        success: true,
        employee: response.data
      };
    } catch (error) {
      return {
        success: false,
        error: error.message
      };
    }
  }
});

// 회의실 예약 현황 조회
server.tool({
  name: 'get_room_availability',
  description: '회의실 예약 가능 시간 조회',
  inputSchema: {
    type: 'object',
    properties: {
      roomId: { type: 'string' },
      date: { type: 'string', format: 'date' }
    },
    required: ['roomId', 'date']
  },
  handler: async ({ roomId, date }) => {
    const response = await axios.get(
      `${process.env.INTERNAL_API_URL}/rooms/${roomId}/availability`,
      { params: { date } }
    );
    return response.data;
  }
});

// 휴가 신청
server.tool({
  name: 'request_vacation',
  description: '휴가 신청 (본인만 가능)',
  inputSchema: {
    type: 'object',
    properties: {
      startDate: { type: 'string', format: 'date' },
      endDate: { type: 'string', format: 'date' },
      reason: { type: 'string' }
    },
    required: ['startDate', 'endDate']
  },
  handler: async ({ startDate, endDate, reason }) => {
    // 실제로는 인증된 사용자 정보 사용
    const response = await axios.post(
      `${process.env.INTERNAL_API_URL}/vacation/request`,
      { startDate, endDate, reason }
    );
    return response.data;
  }
});

server.listen();
```

**설정:**

```json
{
  "servers": {
    "internal": {
      "command": "node",
      "args": ["/path/to/internal-api-mcp.js"],
      "env": {
        "INTERNAL_API_URL": "https://api.company.internal",
        "INTERNAL_API_TOKEN": "${INTERNAL_API_TOKEN}"
      }
    }
  }
}
```

**사용 예시:**

```
> 김철수 사원(EMP001) 정보 조회해줘
> 내일 회의실 A 예약 가능한 시간 알려줘
> 다음 주 월요일부터 수요일까지 휴가 신청해줘
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

### 보안 체크리스트

| 항목 | 확인 |
|------|------|
| API 토큰이 환경 변수로 관리되는가? | ☐ |
| DB 계정이 읽기 전용인가? | ☐ |
| 필요한 최소 권한만 부여했는가? | ☐ |
| 프로덕션 DB 직접 연결을 피했는가? | ☐ |
| CLAUDE.md에 보안 규칙을 명시했는가? | ☐ |
| 민감 정보 테이블 접근 제한을 설정했는가? | ☐ |

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
- JSON 검증: `cat .mcp.json | jq .`
- 위치 확인: 반드시 프로젝트 루트의 `.mcp.json` (또는 사용자 전체 설정은 `~/.claude.json`)
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

### 문제 해결 플로우차트

```
MCP가 작동하지 않음
        │
        ▼
┌─────────────────┐
│ JSON 문법 확인   │ ──에러있음──▶ JSON 수정
│ jq . < 설정파일  │
└────────┬────────┘
         │ 정상
         ▼
┌─────────────────┐
│ 파일 위치 확인   │ ──잘못됨──▶ 프로젝트 루트의 .mcp.json으로 이동
│                 │
└────────┬────────┘
         │ 정상
         ▼
┌─────────────────┐
│ npx 설치 확인   │ ──없음──▶ Node.js/npm 설치
│ which npx      │
└────────┬────────┘
         │ 있음
         ▼
┌─────────────────┐
│ Claude Code     │
│ 재시작했나?     │ ──아니오──▶ 재시작
└────────┬────────┘
         │ 예
         ▼
┌─────────────────┐
│ 환경 변수 확인   │ ──누락──▶ 환경 변수 설정
│ echo $TOKEN    │
└────────┬────────┘
         │ 설정됨
         ▼
┌─────────────────┐
│ 네트워크 확인   │ ──문제──▶ VPN/방화벽 확인
│ ping 대상서버   │
└────────┬────────┘
         │ 정상
         ▼
    로그 상세 확인
```

---

## 자주 하는 실수

### 1. 설정에 비밀 하드코딩

```json
// 나쁨 - 평문 토큰
{ "env": { "TOKEN": "ghp_abc123..." } }

// 더 나음 - 환경 변수 사용
{ "env": { "TOKEN": "${GITHUB_TOKEN}" } }
```

### 2. 잘못된 JSON 형식

```json
// ❌ 잘못된 예 - 주석 사용
{
  "servers": {
    // 이건 안됨
    "github": { ... }
  }
}

// ❌ 잘못된 예 - 마지막 쉼표
{
  "servers": {
    "github": { ... },  // ← 이 쉼표 안됨
  }
}

// ✅ 올바른 예
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"]
    }
  }
}
```

- 기억하십시오: 주석 불가, 마지막 쉼표 불가
- 항상 `jq`로 검증하시기 바랍니다

### 3. 너무 허용적인 접근

```json
// ❌ 나쁨 - 전체 홈 폴더 접근
{
  "args": ["-y", "@anthropic-ai/mcp-server-filesystem", "~"]
}

// ✅ 좋음 - 특정 폴더만
{
  "args": ["-y", "@anthropic-ai/mcp-server-filesystem", "~/projects/myapp"]
}
```

- 전체 홈 폴더에 MCP 접근을 주지 마십시오
- 가능하면 읽기 전용 토큰을 사용하십시오

### 4. 재시작 잊기

- `.mcp.json` 변경 후 Claude Code를 재시작하십시오
- 변경이 자동으로 적용되지 않습니다

### 5. 로그 확인 안 하기

- 실패하면 Claude Code 로그를 확인하십시오
- MCP 서버가 종종 도움이 되는 에러 메시지를 출력합니다

---

## 🎯 미니 퀴즈

### 퀴즈 1: MCP의 역할
MCP가 하는 일은 무엇인가요?

<details>
<summary>정답 보기</summary>

**정답**: MCP는 Claude가 외부 서비스(데이터베이스, API 등)에 직접 접근할 수 있게 해주는 프로토콜입니다.

**설명**: MCP(Model Context Protocol)는 Claude와 외부 서비스 사이의 "통역사" 역할을 합니다. MCP 서버가 Claude의 요청을 해당 서비스가 이해할 수 있는 형태로 변환하고, 결과를 다시 Claude에게 전달합니다.
</details>

### 퀴즈 2: 설정 파일 위치
MCP 설정 파일은 어디에 위치해야 하나요?

<details>
<summary>정답 보기</summary>

**정답**: `.mcp.json` (프로젝트) 또는 `~/.claude.json` (사용자)

**설명**: MCP 설정은 프로젝트별 설정의 경우 프로젝트 루트의 `.mcp.json` 파일에, 사용자 전체 설정의 경우 `~/.claude.json` 파일에 저장됩니다. 이 파일을 수정한 후에는 Claude Code를 재시작해야 변경사항이 적용됩니다.
</details>

### 퀴즈 3: 보안 모범 사례
다음 중 MCP 설정에서 API 토큰을 안전하게 관리하는 방법은?

A) 설정 파일에 직접 토큰 값 입력
B) 환경 변수 참조 (`${GITHUB_TOKEN}`)
C) 주석으로 토큰 기록
D) README에 토큰 문서화

<details>
<summary>정답 보기</summary>

**정답**: B) 환경 변수 참조 (`${GITHUB_TOKEN}`)

**설명**:
- A는 보안 위험 (Git에 커밋 시 노출)
- B가 정답 - 환경 변수로 관리하면 설정 파일에 민감 정보가 포함되지 않음
- C는 JSON에서 주석이 지원되지 않음
- D는 절대 하면 안 됨 - 토큰이 공개됨
</details>

### 퀴즈 4: MCP 서버 역할
PostgreSQL MCP 서버가 하는 일은?

<details>
<summary>정답 보기</summary>

**정답**: Claude의 요청을 SQL 쿼리로 변환하여 PostgreSQL 데이터베이스에 실행하고, 결과를 Claude가 이해할 수 있는 형태로 반환합니다.

**과정**:
1. Claude: "최근 주문 10개 보여줘"
2. MCP 서버: `SELECT * FROM orders ORDER BY created_at DESC LIMIT 10` 변환
3. PostgreSQL: 쿼리 실행, 결과 반환
4. MCP 서버: 결과를 Claude에게 전달
5. Claude: 사람이 읽을 수 있는 형태로 응답
</details>

### 퀴즈 5: 문제 해결
MCP 설정을 변경했는데 작동하지 않습니다. 가장 먼저 해야 할 일은?

<details>
<summary>정답 보기</summary>

**정답**: Claude Code를 재시작합니다.

**설명**: MCP 설정 변경은 자동으로 적용되지 않습니다. 반드시 Claude Code를 종료하고 다시 시작해야 새로운 설정이 로드됩니다. 재시작 후에도 문제가 지속되면 JSON 문법, 파일 위치, 환경 변수 등을 순서대로 확인합니다.
</details>

---

## 📝 실습 과제

### 과제 1: 기초 - Filesystem MCP 설정 [난이도: ★☆☆]

**목표**: 첫 MCP 서버 설정 및 테스트

**단계**:
1. 프로젝트 루트에 `.mcp.json` 파일 생성
2. Filesystem MCP 서버 설정 추가
3. 허용할 폴더 경로 지정 (예: `/tmp` 또는 프로젝트 폴더)
4. Claude Code 재시작
5. "MCP로 [지정 폴더] 파일 목록 보여줘"로 테스트

**확인 사항**:
- [ ] 설정 파일이 올바른 위치에 있음
- [ ] JSON 문법이 올바름
- [ ] Claude가 파일 목록을 반환함

### 과제 2: 중급 - GitHub MCP 연동 [난이도: ★★☆]

**목표**: GitHub MCP를 연결하고 이슈/PR 관리

**단계**:
1. GitHub Personal Access Token 생성
2. 환경 변수로 토큰 설정
3. MCP 설정에 GitHub 서버 추가
4. 다양한 GitHub 작업 테스트:
   - 이슈 목록 조회
   - 특정 이슈 상세 보기
   - PR 목록 확인

**설정 템플릿**:
```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**테스트 명령어**:
```
> 이 레포의 열린 이슈 보여줘
> 이슈 #1 내용 확인해줘
> 최근 PR 5개 보여줘
```

### 과제 3: 심화 - 다중 MCP 서버 통합 [난이도: ★★★]

**목표**: 여러 MCP 서버를 조합한 워크플로우 구성

**시나리오**: 데이터베이스 + GitHub 연동

**단계**:
1. PostgreSQL MCP와 GitHub MCP 모두 설정
2. 두 서버를 활용한 통합 워크플로우 실행:
   - GitHub 이슈 내용 확인
   - 관련 데이터베이스 데이터 조회
   - 분석 결과 정리

**설정 템플릿**:
```json
{
  "servers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**통합 테스트**:
```
> GitHub 이슈 #42 확인하고, 관련된 DB 데이터가 있으면 함께 분석해줘
```

---

## 🏆 도전 과제

### 도전 1: 커스텀 MCP 서버 만들기

자신만의 MCP 서버를 만들어보세요:

1. 회사 내부 API 연동
2. 자주 사용하는 외부 서비스 연동
3. 특수한 데이터 소스 접근

**힌트**: `@modelcontextprotocol/sdk` 패키지 사용

### 도전 2: 보안 강화 구성

프로덕션 수준의 보안 구성 만들기:

1. 읽기 전용 DB 계정 생성
2. 최소 권한 API 토큰 생성
3. CLAUDE.md에 상세 보안 규칙 작성
4. 환경별 설정 분리 (개발/스테이징/프로덕션)

### 도전 3: 팀용 MCP 표준 구성

팀 전체가 사용할 수 있는 MCP 표준 구성 만들기:

1. 공통 MCP 서버 목록 정의
2. 설정 템플릿 문서화
3. 환경 변수 가이드 작성
4. 트러블슈팅 가이드 작성

---

## 정리

이번 챕터에서 배운 것:
- [x] MCP 개념과 아키텍처
- [x] MCP 서버 설정 방법
- [x] 자주 쓰는 MCP 서버 (DB, GitHub, Slack)
- [x] 여러 서버 조합 사용
- [x] 커스텀 MCP 서버 만들기
- [x] 보안 주의사항

**핵심 포인트**: MCP로 Claude의 능력을 무한 확장할 수 있습니다. 데이터베이스, GitHub, Slack 등 외부 서비스와 직접 연결하여 복사-붙여넣기 작업을 없애고 효율적인 워크플로우를 만들 수 있습니다.

---

## 용어 사전

| 용어 | 설명 |
|------|------|
| **MCP** | Model Context Protocol. Claude가 외부 서비스와 통신하기 위한 표준 프로토콜 |
| **MCP 서버** | Claude와 외부 서비스 사이에서 통역 역할을 하는 프로그램 |
| **stdio** | Standard Input/Output. MCP 서버와 Claude가 통신하는 방식 중 하나 |
| **npx** | npm 패키지를 설치 없이 실행하는 도구 |
| **환경 변수** | 시스템에 설정된 변수. API 키 등 민감 정보 저장에 사용 |
| **토큰** | API 접근을 위한 인증 정보 |
| **읽기 전용** | SELECT만 가능하고 수정/삭제가 불가능한 권한 |
| **JSON** | JavaScript Object Notation. 설정 파일 형식 |

---

## 다음 챕터 미리보기

Chapter 26에서는 **CI/CD 자동화**를 배웁니다:

- GitHub Actions 기초
- Claude Code를 CI/CD 파이프라인에 통합
- 자동 코드 리뷰 설정
- PR 자동 체크 구성
- 배포 자동화

MCP로 외부 서비스에 연결하는 방법을 배웠으니, 이제 이것을 자동화 파이프라인에 통합하는 방법을 알아보겠습니다!

---

[Chapter 26: CI/CD 자동화](../Chapter26-CI-CD-Automation/README.ko.md)로 넘어가시기 바랍니다.

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [MCP 공식 문서](https://modelcontextprotocol.io/) - Model Context Protocol 공식 사이트
- [MCP Servers List](https://github.com/modelcontextprotocol/servers) - 공식 MCP 서버 목록
- [Claude Code MCP 가이드](https://docs.anthropic.com/en/docs/claude-code/mcp) - Claude Code에서 MCP 사용법

**영상 자료:**
- [MCP Introduction (YouTube)](https://www.youtube.com/results?search_query=model+context+protocol+MCP+tutorial) - MCP 소개 영상
- [Building MCP Servers (YouTube)](https://www.youtube.com/results?search_query=MCP+server+tutorial) - MCP 서버 만들기
- [API Integration Tutorial (YouTube)](https://www.youtube.com/results?search_query=API+integration+tutorial+beginners) - API 연동 기초

**읽을거리:**
- [MCP Specification](https://spec.modelcontextprotocol.io/) - MCP 프로토콜 스펙
- [PostgreSQL 튜토리얼](https://www.postgresqltutorial.com/) - PostgreSQL 학습
- [GitHub API 문서](https://docs.github.com/en/rest) - GitHub REST API

**MCP 서버 저장소:**
- [Anthropic MCP Servers](https://github.com/anthropics) - Anthropic 공식 MCP 서버
- [Community MCP Servers](https://github.com/topics/mcp-server) - 커뮤니티 MCP 서버

**관련 도구:**
- [Postman](https://www.postman.com/) - API 테스트 도구
- [DBeaver](https://dbeaver.io/) - 데이터베이스 관리 도구

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
