# Chapter 14: 배포하기

[English](./README.md) | **한국어**

---

## 💬 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이 챕터에서 배우는 것

- 배포가 무엇이고 왜 중요한지
- Vercel로 무료 배포하기 (프론트엔드)
- Railway로 백엔드 배포하기
- 자동 배포와 CI/CD 개념
- 커스텀 도메인 연결하기
- 배포 문제 해결 방법

---

## 이전 챕터와의 연결

Chapter 13에서 멋진 포트폴리오 웹사이트를 만들었습니다. 하지만 지금은 **당신만 볼 수 있습니다**.

```
현재 상태:
👤 나 → 💻 내 컴퓨터 → 🌐 localhost:3000
          ↑
       (여기서만 작동)
```

맛있는 음식을 만들어 놓고 잠긴 방에서 혼자 먹는 것과 같습니다. 이제 문을 열 시간입니다!

---

## 왜 필요합니까?

### 배포가 필요한 실제 상황

| 상황 | 배포 없이 | 배포 후 |
|------|---------|---------|
| **취업 지원** | "제 포트폴리오... 나중에 보여드릴게요" | "제 포트폴리오입니다: portfolio.vercel.app" |
| **클라이언트 미팅** | 화면 공유 미팅 잡아야 함 | 링크 하나로 바로 확인 |
| **팀 협업** | "내 컴퓨터로 와서 봐" | "이 URL에서 테스트해봐" |
| **피드백 수집** | 스크린샷 보내고 설명 | "직접 써보고 의견 줘" |

> 배포하기 전까지, 당신의 프로젝트는 바깥 세상에 **"존재하지 않습니다"**.

### 쉬운 비유: 레시피에서 레스토랑으로

```
레시피 개발 (localhost)
→ 집 부엌에서 요리 연습
→ 나만 먹을 수 있음
→ 다른 사람은 내 요리 못 봄

레스토랑 오픈 (배포)
→ 가게 오픈!
→ 누구나 와서 음식 맛볼 수 있음
→ 진짜 손님 피드백 받음
```

`localhost:3000` = 당신의 부엌 (당신만 먹을 수 있음)
`yoursite.vercel.app` = 당신의 레스토랑 (누구나 방문 가능)

> 💡 **비전공자 팁**: 배포는 "내 작업물을 인터넷에 올리는 것"입니다. YouTube에 영상 올리듯이, 내 웹사이트를 올리는 거예요.

---

## 따라해보기: 5분 만에 배포하기

깊이 들어가기 전에, 이게 얼마나 쉬운지 증명해봅시다.

### 사전 준비

1. **GitHub 계정**: [github.com](https://github.com) 가입
2. **Vercel 계정**: [vercel.com](https://vercel.com) (GitHub으로 가입)
3. **Chapter 13의 프로젝트**: 포트폴리오 웹사이트

### 5분 배포

```
# 1단계: GitHub에 올리기
> git 초기화하고, "my-first-deploy"라는 GitHub 저장소 만들고,
> 모든 파일 푸시해줘. public으로 해줘.
```

```
# 2단계: Vercel 연결
1. vercel.com 접속
2. "New Project" 클릭
3. GitHub 저장소 선택 (my-first-deploy)
4. "Deploy" 클릭

# 3단계: 기다리기 (1-2분)

# 4단계: 완료!
🎉 https://my-first-deploy-abc123.vercel.app
```

잘 됐다면, 이제 라이브 웹사이트가 생겼습니다! **지금 바로 친구에게 링크를 공유해보세요.**

> 🔥 **프로 팁**: 실패해도 괜찮습니다. 이 챕터에서 문제 해결 방법을 자세히 다룹니다.

---

## 배포란 무엇입니까?

배포(Deployment)는 **내 컴퓨터에 있는 코드를 인터넷 서버에 올려서 누구나 접근할 수 있게 하는 것**입니다.

### 배포 전 vs 배포 후

```
배포 전:
┌──────────────┐
│  내 컴퓨터    │  → localhost:3000
│  (로컬)      │     ↑ 나만 접근 가능
└──────────────┘

배포 후:
┌──────────────┐     ┌──────────────┐
│  내 컴퓨터    │  →  │ 클라우드 서버 │  → mysite.vercel.app
│  (로컬)      │     │  (Vercel)    │     ↑ 전 세계 누구나 접근
└──────────────┘     └──────────────┘
```

| 상태 | 주소 | 누가 볼 수 있습니까 |
|------|------|-----------------|
| 배포 전 | `http://localhost:3000` | 나만 |
| 배포 후 | `https://my-site.vercel.app` | 전 세계 누구나 |

### 배포하면 얻는 것

- **공개 URL**: 누구에게나 공유 가능한 링크
- **HTTPS**: 보안 연결 (자물쇠 아이콘)
- **24/7 접근**: 내 컴퓨터가 꺼져도 사이트는 작동
- **글로벌 접근**: 세계 어디서나 빠르게 로드

---

## 배포 서비스 비교

### 정적 사이트 (프론트엔드)

| 서비스 | 무료 제공 | 특징 | 추천 대상 |
|--------|-----------|------|-----------|
| **Vercel** | 무제한 | 가장 쉬움, Next.js 최적화 | 초보자, React |
| **Netlify** | 무제한 | Vercel과 비슷, 폼 기능 | Vercel 대안 |
| **GitHub Pages** | 무제한 | GitHub 통합, 정적만 | 간단한 사이트 |
| **Cloudflare Pages** | 무제한 | 빠름, 엣지 컴퓨팅 | 성능 중시 |

### 백엔드/풀스택

| 서비스 | 무료 제공 | 특징 | 추천 대상 |
|--------|-----------|------|-----------|
| **Railway** | 월 $5 크레딧 | 쉬움, DB 지원 | Node.js, Python |
| **Render** | 750시간/월 | 다양한 언어 | 범용 백엔드 |
| **Fly.io** | 무료 티어 | Docker 기반, 글로벌 | 고급 사용자 |
| **Supabase** | 무료 티어 | PostgreSQL + API | BaaS 원하면 |

### 어떤 걸 선택해야 합니까?

```
                    뭘 만들었나요?
                         │
           ┌─────────────┴─────────────┐
           ▼                           ▼
    HTML/CSS/JS만              서버/API 필요
    (정적 사이트)              (백엔드 있음)
           │                           │
           ▼                           ▼
    ┌─────────────┐           ┌─────────────┐
    │   Vercel    │           │   Railway   │
    │  (추천!)    │           │  (추천!)    │
    └─────────────┘           └─────────────┘
```

> 💡 **비전공자 팁**: 처음엔 무조건 Vercel로 시작하세요. 나중에 백엔드가 필요해지면 그때 Railway를 추가하면 됩니다.

---

## Vercel: 가장 쉬운 배포

Vercel은 프론트엔드 배포에 가장 인기 있는 서비스입니다.

### 왜 Vercel입니까?

- **무료**: 개인 프로젝트는 완전 무료
- **쉬움**: 클릭 몇 번이면 끝
- **빠름**: 전 세계 CDN으로 빠른 로딩
- **자동 HTTPS**: 보안 인증서 자동 발급
- **자동 배포**: Git push하면 자동으로 업데이트
- **미리보기**: PR마다 테스트용 URL 생성

### 준비물

1. GitHub 계정
2. Vercel 계정 (GitHub으로 가입 권장)
3. 배포할 프로젝트

---

## Vercel 배포 상세 가이드

### 1단계: GitHub에 코드 올리기

```
> 이 프로젝트 GitHub에 올려줘.
> 저장소 이름은 my-portfolio로.
> public으로 만들어줘.
```

**실제 대화 예시:**

```
사용자: 이 프로젝트 GitHub에 올려줘. 저장소 이름은 my-portfolio

Claude: GitHub에 올릴게요.

[실행: git init]
[실행: git add .]
[실행: git commit -m "Initial commit"]
[실행: gh repo create my-portfolio --public]
[실행: git push -u origin main]

✅ GitHub 저장소가 생성되었습니다!
URL: https://github.com/yourusername/my-portfolio

이제 Vercel에서 이 저장소를 연결하면 됩니다.
```

### 2단계: Vercel 연결

**웹사이트에서 하기:**

1. [vercel.com](https://vercel.com) 접속
2. "Sign Up" → "Continue with GitHub"
3. GitHub 연동 승인
4. "New Project" 클릭
5. 저장소 목록에서 `my-portfolio` 찾기
6. "Import" 클릭
7. 설정 확인 (보통 기본값 그대로 OK)
8. "Deploy" 클릭

**Claude에게 확인 요청:**

```
> Vercel 배포 설정이 맞는지 확인해줘.
> 이 프로젝트는 순수 HTML/CSS/JS야.
```

```
Claude: 순수 HTML/CSS/JS 프로젝트의 Vercel 설정:

Framework Preset: Other
Build Command: (비워두기)
Output Directory: (비워두기 또는 . )
Install Command: (비워두기)

이 설정으로 배포하면 됩니다.
index.html이 루트에 있으면 자동으로 인식됩니다.
```

### 3단계: 배포 완료

배포가 끝나면 주소를 받습니다:

```
✅ 배포 완료!

Production: https://my-portfolio-abc123.vercel.app
Preview: https://my-portfolio-git-main-yourusername.vercel.app

이 주소로 누구나 내 웹사이트를 볼 수 있습니다.
```

> ⚠️ **주의사항**: 첫 배포는 1-3분 정도 걸릴 수 있습니다. "Building..." 상태면 기다려주세요.

---

## 자동 배포 (CI/CD)

Vercel의 가장 강력한 기능: **코드를 수정하면 자동으로 다시 배포됩니다.**

### 자동 배포 흐름

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ 코드 수정    │ →  │ Git Push    │ →  │ 자동 배포   │
│ (로컬)      │    │ (GitHub)    │    │ (Vercel)   │
└─────────────┘    └─────────────┘    └─────────────┘
                                            │
                                            ▼
                                    ┌─────────────┐
                                    │ 1-2분 후    │
                                    │ 새 버전 반영 │
                                    └─────────────┘
```

### 실제 워크플로우

```
# 1. 코드 수정
> 제목을 "안녕하세요"에서 "환영합니다"로 바꿔줘

# 2. 커밋하고 푸시
> 변경사항 커밋하고 푸시해줘

# 3. 자동으로 배포됨!
# Vercel 대시보드에서 "Building..." 표시
# 1-2분 후 새 버전이 라이브!

# 4. 확인
브라우저에서 새로고침하면 변경 반영
```

### 배포 상태 확인

```
> Vercel 배포 상태 확인해줘
```

또는 Vercel 대시보드에서 직접 확인:
- **Production**: 현재 라이브 버전
- **Preview**: 테스트 배포
- **Building**: 빌드 중
- **Error**: 문제 발생 (로그 확인 필요)

> 🔥 **프로 팁**: PR(Pull Request)을 만들면 Vercel이 자동으로 미리보기 URL을 만들어줍니다. 팀 리뷰할 때 매우 유용합니다!

---

## 커스텀 도메인 연결

기본 주소 (`xxx.vercel.app`) 대신 내 도메인을 쓸 수 있습니다.

### 도메인 구매 옵션

| 서비스 | 특징 | .com 가격 (연간) |
|--------|------|------------------|
| [가비아](https://www.gabia.com) | 한국, 한글 지원 | ~15,000원 |
| [Namecheap](https://www.namecheap.com) | 글로벌, 저렴 | ~$10 |
| [Google Domains](https://domains.google) | 간편, 깔끔 | ~$12 |
| [Cloudflare](https://www.cloudflare.com) | 무료 DNS, 보안 | 원가 |

### Vercel에 도메인 연결하기

**1. Vercel에서 도메인 추가:**
```
1. Vercel 대시보드 → 프로젝트 선택
2. Settings → Domains
3. 도메인 입력 (예: myname.com)
4. "Add" 클릭
```

**2. DNS 설정:**

Vercel이 안내하는 DNS 레코드를 도메인 업체에 추가:

```
타입: A
이름: @
값: 76.76.21.21

타입: CNAME
이름: www
값: cname.vercel-dns.com
```

**3. 확인:**
```
> 커스텀 도메인 연결됐는지 확인해줘
```

> ⚠️ **주의사항**: DNS 전파에 최대 48시간이 걸릴 수 있습니다. 보통은 몇 분에서 몇 시간 내에 됩니다.

---

## Railway: 백엔드 배포

정적 웹사이트 말고 서버(API, 데이터베이스)가 필요하면 Railway를 씁니다.

### 언제 Railway가 필요합니까?

```
✅ Vercel만으로 충분한 경우:
- 포트폴리오 사이트
- 블로그
- 정적 랜딩 페이지
- 클라이언트 사이드만 있는 앱

🔄 Railway가 필요한 경우:
- 사용자 데이터 저장 (DB)
- 로그인/회원가입 기능
- API 서버 운영
- 서버 사이드 로직
```

### Railway 예시: 간단한 API 서버

```
> Express 서버 만들어줘.
> /api/hello 하면 { message: "안녕하세요" } 응답하는.
```

**Claude가 만드는 코드:**

```javascript
// server.js
const express = require('express');
const app = express();

// 포트 설정 (Railway는 PORT 환경변수 사용)
const PORT = process.env.PORT || 3000;

// API 엔드포인트
app.get('/api/hello', (req, res) => {
    res.json({ message: '안녕하세요' });
});

// 서버 시작
app.listen(PORT, () => {
    console.log(`서버가 포트 ${PORT}에서 실행 중입니다`);
});
```

```json
// package.json
{
  "name": "my-api",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.0"
  }
}
```

### Railway 배포 단계

**1. Railway 가입:**
- [railway.app](https://railway.app) 접속
- GitHub으로 로그인

**2. 프로젝트 만들기:**
```
1. "New Project" 클릭
2. "Deploy from GitHub repo" 선택
3. 저장소 선택
4. 자동으로 배포 시작!
```

**3. 환경 변수 설정 (필요하면):**
```
1. 프로젝트 → Variables 탭
2. "New Variable" 클릭
3. 이름: DATABASE_URL, 값: (연결 문자열)
```

**4. 도메인 확인:**
```
배포 완료 후 Railway가 URL 제공:
https://my-api-production-abc123.up.railway.app

테스트:
https://my-api-production-abc123.up.railway.app/api/hello
→ { "message": "안녕하세요" }
```

> 💡 **비전공자 팁**: Railway는 월 $5 크레딧을 무료로 줍니다. 간단한 학습/사이드 프로젝트에는 충분합니다.

---

## 🎯 미니 퀴즈

### 퀴즈 1
Q: localhost:3000과 yoursite.vercel.app의 차이는?

<details>
<summary>정답 보기</summary>

- `localhost:3000`: 내 컴퓨터에서만 접근 가능 (로컬 개발용)
- `yoursite.vercel.app`: 인터넷에 공개되어 전 세계 누구나 접근 가능 (배포됨)
</details>

### 퀴즈 2
Q: Git push하면 Vercel에서 자동으로 배포되는 이유는?

<details>
<summary>정답 보기</summary>

Vercel이 GitHub 저장소를 "감시"하고 있기 때문입니다. 새 커밋이 push되면 자동으로 감지해서 빌드하고 배포합니다. 이를 CI/CD (지속적 통합/지속적 배포)라고 합니다.
</details>

### 퀴즈 3
Q: Vercel과 Railway 중 어떤 걸 써야 합니까?

<details>
<summary>정답 보기</summary>

- **Vercel**: 정적 사이트 (HTML/CSS/JS, React 등 프론트엔드)
- **Railway**: 서버/API가 필요할 때 (Node.js, Python 백엔드, 데이터베이스)

대부분의 경우 Vercel로 시작하고, 백엔드가 필요해지면 Railway를 추가합니다.
</details>

---

## 📝 실습 과제

### 난이도 1: 첫 배포 (초급)

Chapter 13에서 만든 포트폴리오를 배포하세요.

```
# 1. GitHub에 올리기
> 이 프로젝트 GitHub에 올려줘. 저장소 이름은 my-portfolio.

# 2. Vercel 연결
웹사이트에서 vercel.com → New Project → 저장소 선택 → Deploy

# 3. URL 확인
배포된 URL을 브라우저에서 열어보기

# 4. 친구에게 공유
링크를 친구에게 보내서 확인 요청
```

**완료 체크:**
- [ ] GitHub에 저장소 생성됨
- [ ] Vercel 배포 성공
- [ ] 공개 URL로 접근 가능
- [ ] 다른 사람도 볼 수 있음 확인

### 난이도 2: 자동 배포 테스트 (중급)

```
# 1. 코드 수정
> 푸터에 "Made with Claude Code" 추가해줘

# 2. 커밋하고 푸시
> 커밋하고 푸시해줘

# 3. Vercel 대시보드 확인
빌드 상태가 "Building" → "Ready"로 바뀌는지 확인

# 4. 사이트 확인
새로고침해서 변경사항 반영됐는지 확인
```

**완료 체크:**
- [ ] 로컬 수정사항 커밋됨
- [ ] GitHub에 push됨
- [ ] Vercel이 자동 빌드 시작
- [ ] 새 버전이 라이브에 반영됨

### 난이도 3: 커스텀 도메인 (고급)

도메인을 구매하고 연결해보세요.

```
# 1. 도메인 구매 (선택 사항)
가비아, Namecheap 등에서 원하는 도메인 구매

# 2. Vercel에 도메인 추가
Settings → Domains → 도메인 입력

# 3. DNS 설정
도메인 업체에서 Vercel이 안내하는 DNS 레코드 추가

# 4. 확인
https://yourdomain.com 으로 접근 테스트
```

**완료 체크:**
- [ ] 도메인 구매/소유
- [ ] DNS 레코드 설정
- [ ] HTTPS 작동 확인
- [ ] www와 non-www 모두 작동

---

## 🏆 도전 과제

### 도전 1: API 서버 배포
```
> 간단한 API 서버 만들어줘.
> GET /api/projects 하면 프로젝트 목록 JSON으로 반환.
> Railway에 배포할 수 있게.
```

### 도전 2: 프론트엔드 + 백엔드 연결
```
> 포트폴리오에서 Railway API의 프로젝트 목록을 가져와서 보여줘.
> fetch로 API 호출하게.
```

### 도전 3: 환경 변수 활용
```
> API URL을 환경 변수로 관리하게 해줘.
> 로컬에서는 localhost, 배포 시에는 실제 URL 사용하게.
```

---

## 배포 문제 해결

### "Git command not found" 오류

Git이 설치 안 됐습니다.

```
> 내 컴퓨터에 git 설치하는 법 알려줘
```

### GitHub 푸시 실패 / 인증 오류

```
> GitHub 인증 설정 도와줘. 푸시 에러가 나와.
```

흔한 해결책:
1. GitHub 로그인 상태 확인
2. Personal Access Token 생성 및 사용
3. SSH 키 설정

**실제 대화 예시:**

```
사용자: git push가 안 돼. permission denied 에러 나와.

Claude: GitHub 인증 문제네요. 해결 방법:

1. Personal Access Token 방식 (추천):
   - GitHub → Settings → Developer settings
   - Personal access tokens → Tokens (classic)
   - Generate new token
   - "repo" 권한 체크
   - 토큰 복사

2. 다시 push 시도:
   Username: (GitHub 아이디)
   Password: (복사한 토큰 붙여넣기)

또는 SSH 키를 설정할 수도 있어요. 어떤 방식으로 할까요?
```

### Vercel 빌드 실패

```
> Vercel 빌드 에러 났어: [에러 메시지 붙여넣기]
> 뭐가 문제고 어떻게 고쳐?
```

**흔한 빌드 에러:**

| 에러 | 원인 | 해결 |
|------|------|------|
| `Module not found` | 패키지 없음 | `npm install` 후 push |
| `Build command failed` | 빌드 스크립트 오류 | package.json 확인 |
| `Output directory not found` | 출력 폴더 설정 오류 | Vercel 설정에서 수정 |

### 배포 후 404 에러

```
> 배포된 사이트가 404 떠. 파일 구조 확인해줘.
```

**흔한 원인:**
- 메인 파일 이름이 `index.html`이 아님
- 파일이 하위 폴더에 있음 (루트가 아니라)
- 대소문자 불일치 (`Index.html` vs `index.html`)

### 스타일이 깨져 보임

```
> 배포된 사이트에 스타일이 없어. CSS 링크 확인해줘.
```

**흔한 원인:**
- CSS 파일 경로가 잘못됨 (상대 경로 vs 절대 경로)
- 파일명 대소문자 불일치
- CDN 연결 문제 (Tailwind 등)

### 환경 변수가 작동 안 함

```
> API 키가 작동 안 해. 환경 변수 확인 방법 알려줘.
```

**체크리스트:**
1. Vercel 대시보드에서 환경 변수 추가했는지
2. 변수 이름이 정확한지 (대소문자 구분)
3. 추가 후 재배포 했는지
4. 프론트엔드에서는 `VITE_` 또는 `NEXT_PUBLIC_` 접두어 필요

---

## 자주 하는 실수

### 실수 1: 민감한 정보 커밋하기

**절대 GitHub에 커밋하면 안 되는 것들:**
- API 키
- 비밀번호
- 데이터베이스 연결 문자열
- `.env` 파일

```
> 푸시하기 전에 내 코드에 민감한 정보 있는지 확인해줘
```

**예방법:**
```
# .gitignore 파일에 추가
.env
.env.local
.env.production
```

> ⚠️ **주의사항**: 실수로 비밀을 커밋했다면, **즉시 키를 변경하세요**. Git 기록에 남아있어서 삭제해도 볼 수 있습니다!

### 실수 2: .gitignore에 node_modules 안 넣기

```
# ❌ node_modules가 커밋됨
(수천 개의 파일이 GitHub에 올라감)
(저장소가 엄청 커짐)
(푸시/풀이 매우 느려짐)

# ✅ .gitignore에 추가
> .gitignore에 node_modules 있는지 확인해줘
```

### 실수 3: 로컬 절대 경로 사용하기

```html
<!-- ❌ 잘못됨 - 내 컴퓨터에서만 작동 -->
<img src="C:/Users/홍길동/project/images/photo.png">
<link href="D:/projects/style.css">

<!-- ✅ 올바름 - 상대 경로 사용 -->
<img src="./images/photo.png">
<link href="./style.css">
```

### 실수 4: 배포 완료 기다리지 않기

```
❌ Push 직후 바로 사이트 확인 → 이전 버전 보임
✅ Vercel 대시보드에서 "Ready" 확인 후 → 새 버전 보임
```

배포는 1-3분 걸립니다. 조금 기다려주세요.

### 실수 5: 잘못된 빌드 설정

```
> 내 프로젝트의 올바른 Vercel 빌드 설정이 뭐야?
```

**프레임워크별 설정:**

| 프로젝트 타입 | Framework Preset | Build Command | Output Directory |
|--------------|------------------|---------------|------------------|
| 순수 HTML | Other | (비움) | (비움) |
| React (CRA) | Create React App | `npm run build` | `build` |
| React (Vite) | Vite | `npm run build` | `dist` |
| Next.js | Next.js | (자동) | (자동) |
| Vue | Vue.js | `npm run build` | `dist` |

---

## 배포 체크리스트

배포 전에 확인하세요:

**코드:**
- [ ] 코드에 에러 없음 (로컬에서 정상 작동)
- [ ] 민감한 정보 (API 키) 코드에 없음
- [ ] .gitignore에 node_modules, .env 포함

**파일:**
- [ ] 메인 파일이 index.html (또는 프레임워크 규칙 준수)
- [ ] 모든 이미지 경로가 상대 경로
- [ ] 파일명에 한글/특수문자 없음

**설정:**
- [ ] package.json의 scripts가 올바름 (필요한 경우)
- [ ] 환경 변수가 Vercel에 설정됨 (필요한 경우)
- [ ] 빌드 설정이 프레임워크와 맞음

**테스트:**
- [ ] 로컬에서 최종 확인
- [ ] 배포 후 모든 페이지 접근 테스트
- [ ] 모바일에서도 확인

---

## 용어 사전

| 용어 | 설명 |
|------|------|
| **배포 (Deployment)** | 코드를 서버에 올려 공개하는 것 |
| **CI/CD** | 지속적 통합/지속적 배포 - 자동화된 빌드와 배포 |
| **빌드 (Build)** | 코드를 실행 가능한 형태로 변환하는 과정 |
| **정적 사이트** | 서버 로직 없이 HTML/CSS/JS만으로 구성된 사이트 |
| **CDN** | 전 세계에 분산된 서버로 빠르게 콘텐츠 제공 |
| **환경 변수** | 코드 외부에서 설정하는 값 (API 키 등) |
| **도메인** | 웹사이트 주소 (예: google.com) |
| **DNS** | 도메인 이름을 IP 주소로 변환하는 시스템 |
| **HTTPS** | 보안된 HTTP 연결 (자물쇠 아이콘) |
| **SSL/TLS** | HTTPS를 위한 보안 인증서 |

---

## 정리

이번 챕터에서 배운 것:
- [x] 배포가 무엇이고 왜 필요한지
- [x] Vercel로 프론트엔드 배포하기
- [x] Railway로 백엔드 배포하기
- [x] 자동 배포 (CI/CD) 개념
- [x] 커스텀 도메인 연결하기
- [x] 배포 문제 해결 방법

> 💡 **핵심 포인트**: 이제 만든 프로젝트를 전 세계에 공개할 수 있습니다. 배포는 개발의 마지막이 아니라, **실제 사용자를 만나는 시작**입니다!

---

## 다음 챕터 미리보기

Chapter 15에서는 **데이터 저장**을 배웁니다.

지금까지 만든 웹사이트는 "정적"입니다. 새로고침하면 입력한 내용이 사라지죠.

다음 챕터에서는:
- localStorage로 브라우저에 데이터 저장하기
- CRUD (Create, Read, Update, Delete) 개념
- 할 일 목록 앱 만들기
- 데이터베이스 연동 기초

사용자의 데이터를 기억하는 "진짜" 앱을 만들어봅시다!

[Chapter 15: 데이터 저장](../Chapter15-Data-Storage/README.ko.md)으로 넘어가세요.

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [Vercel 공식 문서](https://vercel.com/docs) - Vercel 완전 가이드
- [Netlify 공식 문서](https://docs.netlify.com/) - Netlify 배포 가이드
- [Railway 공식 문서](https://docs.railway.app/) - Railway 백엔드 배포

**영상 자료:**
- [Vercel 배포 튜토리얼 (YouTube)](https://www.youtube.com/results?search_query=vercel+deployment+tutorial) - Vercel 배포 방법
- [CI/CD 개념 설명 (YouTube)](https://www.youtube.com/results?search_query=ci+cd+explained+beginners) - 자동 배포 개념

**읽을거리:**
- [GitHub Pages 가이드](https://pages.github.com/) - 무료 정적 호스팅
- [Cloudflare Pages](https://pages.cloudflare.com/) - 빠른 정적 호스팅

**관련 도구:**
- [Vercel](https://vercel.com/) - 프론트엔드 배포 플랫폼
- [Netlify](https://www.netlify.com/) - 정적 사이트 호스팅
- [Railway](https://railway.app/) - 백엔드 배포 플랫폼
- [Namecheap](https://www.namecheap.com/) - 도메인 구매

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
