# Tech Stack Specification

## Core Tool

- **Claude Code** - AI coding assistant by Anthropic
  - Permission modes: Plan, Normal, Accept Edits
  - Slash commands, Hooks, Skills, MCP, Headless mode

## Frontend

| Category | Primary | Alternatives |
|----------|---------|--------------|
| Markup/Style | HTML, CSS | - |
| CSS Framework | Tailwind CSS | styled-components, CSS Modules |
| JavaScript | Vanilla JS, ES6+ | TypeScript |
| Framework | React | Next.js, Vue, Svelte |
| Build Tool | Vite | Webpack, Parcel |
| Visualization | Chart.js | D3.js, Recharts |

## Backend

| Category | Primary | Alternatives |
|----------|---------|--------------|
| Runtime | Node.js 20+ | Deno, Bun |
| Framework | Express | Fastify, Hono |
| Package Manager | npm | pnpm, yarn |

## Database & Storage

| Use Case | Recommended | Alternatives | Notes |
|----------|-------------|--------------|-------|
| **Serverless DB** | Supabase | Firebase, PlanetScale | 무료 티어 충분 |
| **Local Dev** | SQLite | - | 빠른 프로토타이핑 |
| **Production SQL** | PostgreSQL (Supabase) | Neon, CockroachDB | Supabase = Postgres 호스팅 |
| **NoSQL** | Firebase Firestore | MongoDB Atlas | 문서 기반 데이터 |
| **Cache** | Upstash Redis | - | 서버리스 Redis |
| **File Storage** | Supabase Storage | Cloudflare R2, S3 | 이미지/파일 업로드 |

### DB 선택 가이드

```
빠른 시작 → Supabase (PostgreSQL + Auth + Storage 올인원)
문서형 데이터 → Firebase Firestore
무료 + 엣지 → Turso (SQLite at edge)
대용량 분석 → BigQuery, ClickHouse
```

### 최적화 체크리스트

- [ ] 인덱스 설정 (자주 조회하는 컬럼)
- [ ] RLS 정책 설정 (보안)
- [ ] Connection pooling 활성화
- [ ] 페이지네이션 적용 (대량 데이터)

## Deployment & Infrastructure

| Category | Primary | Alternatives |
|----------|---------|--------------|
| Static/Frontend | Vercel | Netlify, Cloudflare Pages |
| Backend/API | Railway | Render, Fly.io |
| Serverless Functions | Vercel Functions | Cloudflare Workers |
| CI/CD | GitHub Actions | - |

### 배포 선택 가이드

```
정적 사이트 → Vercel (무료, 가장 쉬움)
풀스택 앱 → Vercel + Supabase
백엔드 API → Railway 또는 Render
글로벌 엣지 → Cloudflare Workers
```

## APIs & Services

| Category | Services |
|----------|----------|
| AI | OpenAI API, Anthropic API, Replicate |
| Auth | Supabase Auth, Clerk, Auth0 |
| Payment | Stripe, Toss Payments |
| Email | Resend, SendGrid |
| Analytics | Vercel Analytics, Plausible |

## Development Tools

| Category | Tools |
|----------|-------|
| Version Control | Git, GitHub |
| Code Quality | Prettier, ESLint, TypeScript |
| Testing | Vitest, Playwright |
| File Formats | JSON, CSV, YAML, Markdown |
| Config | .env, .gitignore, CLAUDE.md |

## Key Libraries

```bash
# Core
@supabase/supabase-js   # Supabase SDK
openai                   # OpenAI SDK

# Data Processing
csv-parser              # CSV parsing
json2csv                # JSON to CSV
zod                     # Schema validation

# Utilities
sharp                   # Image processing
cheerio                 # Web scraping
date-fns                # Date utilities
```

## Performance Optimization

### Frontend
- 이미지: WebP/AVIF 포맷, lazy loading
- 번들: Tree shaking, code splitting
- 캐싱: Service Worker, CDN

### Backend
- DB 쿼리: 인덱싱, N+1 문제 방지
- API: 응답 캐싱, rate limiting
- 연결: Connection pooling

### Monitoring
- 에러 트래킹: Sentry
- 성능: Vercel Analytics, Web Vitals
