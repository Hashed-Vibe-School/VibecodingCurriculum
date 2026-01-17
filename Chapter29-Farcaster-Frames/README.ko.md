# Chapter 29: Farcaster Frames

## 💬 질문하기
[Discord](https://discord.gg/your-invite-link) - 막히면 여기서 질문하세요!

## 🎯 이 챕터의 목표

이 챕터를 완료하면:
- Farcaster와 Frames가 무엇인지 이해할 수 있습니다
- Frames가 기술적으로 어떻게 작동하는지 이해합니다 (메타 태그, API 라우트, 상태 관리)
- Claude와 함께 처음부터 인터랙티브 Frame을 만들 수 있습니다!
- Frame을 배포하고 Farcaster에 공유할 수 있습니다
- Frame에 온체인 액션을 추가할 수 있습니다 (팁, 민팅, 트랜잭션)
- 영속적인 상태를 가진 다중 단계 Frame을 만들 수 있습니다
- 두 가지 실습을 완료합니다 (투표 Frame + NFT 민팅 Frame)

## ⏱️ 예상 소요 시간
**3-4시간**

## 📋 필요한 것
- **Chapter 28 완료** (Web3 기초) - 지갑 설정, 트랜잭션, IPFS 기초
- Farcaster 계정 (Warpcast 앱)
- Vercel 계정 (배포용)
- Node.js 18+ 설치

## 🔗 Chapter 28 기반으로 진행

Chapter 28에서 **소비자**로서 Web3 기초를 배웠습니다. 이제 그 스킬을 사용해 소셜 미디어를 통해 **수천 명의 사용자**에게 도달하는 것을 만들 거예요!

```
Ch28에서 배운 것                →    Ch29에서 사용하는 방법
──────────────────────────────────────────────────────────
지갑 연결                       →    Frame 사용자가 트랜잭션 서명
트랜잭션 보내기                   →    Frames에서 NFT 민팅, 팁 보내기
IPFS 업로드                     →    Frame 이미지 저장
wagmi/viem 기초                 →    Frame 백엔드에서 같은 라이브러리
```

> 💡 **핵심 인사이트**: Frames는 소셜 미디어 포스트 안에 사는 미니 dapp을 만드는 것과 같아요. Chapter 28의 모든 Web3 개념이 여기에 적용됩니다!

---

## Farcaster란?

### 탈중앙화 소셜 미디어

트위터와 암호화폐 지갑이 아기를 낳았다고 상상해보세요:

```
전통적인 소셜 (Twitter/X):         Farcaster:
🏢 "아파트 임대"                   🏠 "집 소유"

└── 회사가 데이터 소유              └── 내가 내 신원 소유
└── 언제든 추방 가능                └── 플랫폼 강퇴 불가
└── 알고리즘이 당신을 통제           └── 직접 피드 선택
└── 팔로워가 플랫폼에 갇힘           └── 어디든 팔로워 데려가기
└── "그들의 규칙, 그들의 재산"       └── "내 키, 내 콘텐츠"
```

### 기술적인 측면 (간략화)

```
┌─────────────────────────────────────────────────────────────┐
│                  Farcaster 아키텍처                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  HUBS (탈중앙화 서버)                                        │
│  ┌─────┐  ┌─────┐  ┌─────┐                                 │
│  │Hub 1│──│Hub 2│──│Hub 3│  ← 누구나 hub 운영 가능           │
│  └─────┘  └─────┘  └─────┘                                 │
│      │        │        │                                    │
│      └────────┼────────┘                                    │
│               │                                             │
│               ▼                                             │
│  ┌─────────────────────────────┐                           │
│  │    앱 (클라이언트)            │                           │
│  │  • Warpcast (가장 인기)      │                           │
│  │  • Supercast                │                           │
│  │  • Farcord                  │                           │
│  └─────────────────────────────┘                           │
│               │                                             │
│               ▼                                             │
│  ┌─────────────────────────────┐                           │
│  │    당신의 신원                │                           │
│  │  • FID (Farcaster ID)       │  ← 블록체인에 저장          │
│  │  • 복구 주소                 │                           │
│  │  • 연결된 지갑들              │                           │
│  └─────────────────────────────┘                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 개발자들이 Farcaster를 좋아하는 이유

```
🚀 바이브코딩에 Farcaster가 완벽한 이유:

1. 내장된 관객
   - 50만+ 크립토 네이티브 사용자
   - 새로운 것을 시도하고 싶어 함
   - 당신의 창작물에 즉각적인 피드백

2. 앱 스토어 필요 없음
   - 링크 공유 → 당신의 앱이 작동
   - Apple/Google 승인 불필요
   - 몇 주가 아닌 몇 분 만에 배포

3. 컴포저블
   - 당신의 Frame이 아무 스마트 컨트랙트나 사용 가능
   - 다른 사람들이 당신의 Frame 위에 빌드 가능
   - 소셜 앱을 위한 레고 블록

4. 개발자 친화적
   - 오픈 프로토콜, 오픈 API
   - 풍부한 도구 생태계
   - 활발한 개발자 커뮤니티
```

### 핵심 용어

| 용어 | 설명 | "5살에게 설명하면" |
|------|------|-------------------|
| **Farcaster** | 탈중앙화 소셜 프로토콜 | 아무도 소유하지 않는 트위터 |
| **Warpcast** | Farcaster를 사용하는 가장 인기 있는 앱 | Gmail로 이메일 확인하는 것처럼 |
| **Cast** | Farcaster의 포스트 | Farcaster의 트윗 |
| **Frame** | 포스트 안의 인터랙티브 미니앱 | 뭔가를 할 수 있는 인스타그램 스토리 |
| **FID** | 당신의 Farcaster ID | 당신의 선수 번호 (영구적!) |
| **Hub** | Farcaster 데이터를 저장하는 서버 | 모든 포스트 사본을 보관하는 도서관 |
| **Signer** | 앱이 당신 대신 포스트할 수 있게 하는 키 | "이게 진짜 나야"라는 도장 |

> 💡 **마인드 블로운 순간**: Frame은 소셜 미디어 포스트 안에 전체 앱을 넣는 것과 같아요. 인스타그램 스토리가 NFT를 민팅하거나, 투표를 하거나, 게임을 플레이할 수 있다고 상상해보세요 - 그게 Frames가 하는 일이에요!

---

## Frames란?

### 인터랙티브 포스트

Frame은 지루한 링크를 **인터랙티브 경험**으로 바꿔줍니다:

```
🥱 일반 Cast:                     🎮 Frame:
┌────────────────────────┐          ┌────────────────────────┐
│ "내 프로젝트 확인해"     │          │ ┌────────────────────┐ │
│ [링크]                  │    →     │ │  🎯 Web3 퀴즈!      │ │
│                        │          │ │  점수: 0/5         │ │
└────────────────────────┘          │ └────────────────────┘ │
                                    │ [A] [B] [C] [D]        │
 "클릭해서 웹사이트 열기"             │ 클릭 → 마법이 일어남!   │
 (사용자가 떠나야 함)                └────────────────────────┘
                                     (사용자가 피드에 머무름!)
```

### "팝업 숍" 비유

```
소셜 미디어를 쇼핑몰이라고 생각해보세요:

📱 일반 포스트 = 전단지 나눠주기
   - "우리 웹사이트 방문하세요!"
   - 사람들이 몰을 떠나야 함
   - 대부분의 사람들은 귀찮아서 안 함

🏪 Frame = 몰 안의 팝업 숍
   - 사람들이 바로 거기서 상호작용 가능
   - 떠날 필요 없음
   - 즉각적인 참여
   - 결제도 가능!
```

### Frames가 기술적으로 작동하는 방법

```
┌─────────────────────────────────────────────────────────────┐
│                    Frame 요청/응답 사이클                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. 사용자가 FRAME을 봄                                       │
│     Warpcast가 당신의 URL 가져옴 → 메타 태그 읽음 → 표시     │
│                                                             │
│  2. 사용자가 버튼 클릭                                        │
│     ┌──────────────┐                                        │
│     │ [버튼 1]     │  ← 사용자 클릭                          │
│     └──────────────┘                                        │
│            │                                                │
│            ▼                                                │
│     당신의 서버로 POST 요청:                                  │
│     • buttonIndex (어떤 버튼: 1, 2, 3, 또는 4)               │
│     • fid (사용자의 Farcaster ID)                           │
│     • inputText (텍스트 입력이 있다면)                       │
│     • castId (어떤 cast에서 왔는지)                          │
│                                                             │
│  3. 당신의 서버가 응답                                        │
│     업데이트된 새 HTML 반환:                                  │
│     • fc:frame:image (보여줄 새 이미지)                      │
│     • fc:frame:button:1 (새 버튼 레이블)                     │
│     • fc:frame:post_url (다음 클릭이 가는 곳)                │
│                                                             │
│  4. FRAME 업데이트                                           │
│     Warpcast가 새 이미지와 버튼을 표시                       │
│                                                             │
│  이 사이클이 각 상호작용마다 반복됩니다!                      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Frame 메타 태그 설명

Warpcast가 당신의 URL을 방문할 때, 특별한 HTML 메타 태그를 찾습니다:

```html
<!-- 필수: Warpcast에게 "이것은 Frame이야"라고 알려줌 -->
<meta property="fc:frame" content="vNext" />

<!-- 필수: 표시할 이미지 (1.91:1 또는 1:1 비율) -->
<meta property="fc:frame:image" content="https://your-site.com/image.png" />

<!-- 선택: 버튼 (최대 4개) -->
<meta property="fc:frame:button:1" content="찬성 투표" />
<meta property="fc:frame:button:2" content="반대 투표" />
<meta property="fc:frame:button:3" content="결과 보기" />

<!-- 선택: 버튼 액션 -->
<meta property="fc:frame:button:1:action" content="post" />      <!-- 기본: 당신의 서버로 전송 -->
<meta property="fc:frame:button:2:action" content="link" />      <!-- 외부 URL 열기 -->
<meta property="fc:frame:button:3:action" content="tx" />        <!-- 블록체인 트랜잭션 실행 -->
<meta property="fc:frame:button:4:action" content="mint" />      <!-- NFT 민팅 -->

<!-- 선택: 버튼 클릭이 가는 곳 -->
<meta property="fc:frame:post_url" content="https://your-site.com/api/frame" />

<!-- 선택: 텍스트 입력 필드 -->
<meta property="fc:frame:input:text" content="답을 입력하세요..." />

<!-- 선택: 이미지 비율 -->
<meta property="fc:frame:image:aspect_ratio" content="1.91:1" />
```

> 💡 **바이브코딩 팁**: 이것들을 외울 필요 없어요! 원하는 걸 Claude에게 말하면 올바른 메타 태그를 생성해줍니다.

### 버튼 액션 타입

| 액션 | 하는 일 | 사용 예시 |
|------|--------|----------|
| `post` | 당신의 서버로 데이터 전송 | 투표, 네비게이션, 퀴즈 답변 |
| `post_redirect` | 포스트 후 URL로 리다이렉트 | "웹사이트에서 계속하기" |
| `link` | 브라우저에서 URL 열기 | 외부 링크, 문서 |
| `tx` | 블록체인 트랜잭션 실행 | 팁, 구매, 스왑 |
| `mint` | NFT 민팅을 위한 특별 액션 | 원클릭 NFT 민팅 |

### 만들 수 있는 것

상상력이 한계예요, 하지만 검증된 히트작들:

| Frame 타입 | 예시 | 왜 잘 되는지 | 난이도 |
|-----------|------|------------|--------|
| **투표** | "탭 vs 스페이스?" | 쉬운 참여, 모두가 의견이 있음 | ⭐ 쉬움 |
| **퀴즈** | "얼마나 Web3야?" | 바이럴 가능성 + 교육적 | ⭐⭐ 중간 |
| **게임** | 가위바위보 | 재미 + 경쟁 + 공유 가능 | ⭐⭐ 중간 |
| **NFT 민팅** | 원클릭 수집 | 사용자에게 마찰 없음 | ⭐⭐ 중간 |
| **팁 항아리** | 크리에이터 후원 | 직접 암호화폐 결제 | ⭐⭐ 중간 |
| **갤러리** | 아트 스와이프 | 시각적 + 인터랙티브 | ⭐⭐ 중간 |
| **래플** | "참여해서 이기세요!" | 흥분 + 참여 | ⭐⭐⭐ 어려움 |
| **토큰 게이팅** | "NFT 보유자만" | 독점성 + 커뮤니티 | ⭐⭐⭐ 어려움 |

> 💡 **프로 팁**: 최고의 Frames는 심플해요. 5초 안에 설명할 수 없으면, 단순화하세요.

---

## Step 1: Farcaster 계정 설정

### 1.1 Warpcast 다운로드

```
You: Frames 공유를 위한 Farcaster 계정 설정 도와줘

Claude: Warpcast 설정을 안내해드릴게요...
```

Claude가 도와줄 것:
1. iOS/Android에서 [Warpcast](https://warpcast.com/) 다운로드
2. 계정 생성 (스팸 방지를 위한 소액의 비용 필요)
3. 프로필 설정
4. 지갑 연결 (Web3 기능에 중요!)

### 1.2 Frames 둘러보기

직접 만들기 전에, 사용자로서 몇 가지 Frames를 경험해보세요:

```
You: 시도해볼 수 있는 멋진 Farcaster Frames 보여줘

Claude: 탐색할 인기 있는 Frames들:
        1. 투표 - 어떤 투표 cast를 찾아서 투표하기
        2. @base의 민팅 frames - 원클릭 NFT 민팅
        3. 퀴즈 게임 - Web3 지식 테스트
        4. 팁 frames - 크리에이터에게 팁 보내기
```

**Frames 찾는 방법:**
1. Warpcast 열기
2. "frame" 검색하거나 인기 cast 탐색
3. 인터랙티브 버튼이 있는 포스트 찾기
4. 클릭하고 상호작용하기!

### 1.3 Frame 개발자 도구 얻기

```
You: Frames 만들려면 어떤 도구가 필요해?

Claude: Frame 개발에 필요한 것:
        1. Node.js 18+ (이전 챕터에서 이미 있어야 함)
        2. 코드 에디터 (VS Code)
        3. 로컬 테스팅을 위한 ngrok 또는 유사한 것
        4. Warpcast Frame Validator: https://warpcast.com/~/developers/frames
```

---

## Step 2: Frame 프로젝트 구조 이해하기

### 2.1 Frame 프로젝트의 해부학

```
my-frame/
├── app/
│   ├── page.tsx              # 초기 Frame 메타 태그가 있는 홈 페이지
│   ├── api/
│   │   └── frame/
│   │       └── route.ts      # 버튼 클릭 처리 (POST 요청)
│   └── opengraph-image.tsx   # 동적 이미지 생성
├── public/
│   └── images/               # Frame용 정적 이미지
├── lib/
│   └── frame-utils.ts        # 헬퍼 함수
├── .env.local                # 환경 변수
├── next.config.js
└── package.json
```

### 2.2 프로젝트 생성

```
You: Next.js를 사용해서 Farcaster Frame 프로젝트 만들어줘.
     TypeScript와 최신 Frame 스펙으로 설정해줘.
     기본 "Hello World" frame 포함해줘.

Claude: 완전한 Frame 프로젝트를 설정할게요...
        [프로젝트 구조 생성]
        [의존성 설치: @coinbase/onchainkit, viem]
        [적절한 메타 태그로 초기 Frame 설정]
```

### 2.3 주요 파일 설명

**`app/page.tsx`** - Frame의 진입점:

```tsx
// 이 파일은 Warpcast가 읽는 HTML을 반환합니다
// 메타 태그가 Warpcast에게 무엇을 표시할지 알려줍니다

export default function Home() {
  return (
    <>
      {/* Frame 메타 태그는 head에 들어갑니다 */}
      <head>
        <meta property="fc:frame" content="vNext" />
        <meta property="fc:frame:image" content={`${process.env.NEXT_PUBLIC_URL}/api/og`} />
        <meta property="fc:frame:button:1" content="클릭해봐!" />
        <meta property="fc:frame:post_url" content={`${process.env.NEXT_PUBLIC_URL}/api/frame`} />
      </head>
      <body>
        <h1>내 첫 번째 Frame</h1>
        {/* 이 HTML은 브라우저용이지, Warpcast용이 아닙니다 */}
      </body>
    </>
  );
}
```

**`app/api/frame/route.ts`** - 버튼 클릭 처리:

```tsx
// 누군가 Frame의 버튼을 클릭하면 이게 실행됩니다
import { NextRequest, NextResponse } from 'next/server';

export async function POST(req: NextRequest) {
  // 들어오는 요청 파싱
  const body = await req.json();

  // 유용한 정보 추출
  const { untrustedData } = body;
  const buttonIndex = untrustedData.buttonIndex;  // 어떤 버튼 (1-4)
  const fid = untrustedData.fid;                  // 사용자의 Farcaster ID
  const inputText = untrustedData.inputText;      // 입력한 텍스트 (있다면)

  // 어떤 버튼이 클릭되었는지에 따라 다음에 보여줄 것 결정
  let newImage = '';
  let newButtons = [];

  if (buttonIndex === 1) {
    newImage = 'https://your-site.com/image-after-button-1.png';
    newButtons = ['계속', '돌아가기'];
  }

  // 새 Frame HTML 반환
  return new NextResponse(
    `<!DOCTYPE html>
    <html>
      <head>
        <meta property="fc:frame" content="vNext" />
        <meta property="fc:frame:image" content="${newImage}" />
        <meta property="fc:frame:button:1" content="${newButtons[0]}" />
        <meta property="fc:frame:button:2" content="${newButtons[1]}" />
      </head>
    </html>`
  );
}
```

> 💡 **핵심 인사이트**: Frame은 기본적으로 대화예요. 사용자 클릭 → 새 이미지/버튼으로 응답 → 사용자가 다시 클릭 → 반복!

---

## Step 3: 첫 번째 Frame 만들기 (퀴즈)

### 3.1 플로우 디자인

코딩 전에, Frame의 플로우를 계획하세요:

```
┌─────────────────────────────────────────────────────────────┐
│                    퀴즈 Frame 플로우                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  화면 1: 환영                                                │
│  ┌────────────────────┐                                     │
│  │   🎯 Web3 퀴즈!    │                                     │
│  │   당신의 지식을     │                                     │
│  │   테스트하세요!     │                                     │
│  └────────────────────┘                                     │
│  [퀴즈 시작]                                                 │
│       │                                                     │
│       ▼                                                     │
│  화면 2: 질문                                                │
│  ┌────────────────────┐                                     │
│  │  Q1: "HODL"이      │                                     │
│  │  무슨 뜻이야?       │                                     │
│  └────────────────────┘                                     │
│  [A] [B] [C] [D]                                            │
│       │                                                     │
│       ▼                                                     │
│  화면 3: 결과                                                │
│  ┌────────────────────┐                                     │
│  │  ✅ 정답!          │  또는  │  ❌ 오답!     │             │
│  │  +1점             │       │  정답은 B    │             │
│  └────────────────────┘                                     │
│  [다음 질문]                                                 │
│       │                                                     │
│       ▼                                                     │
│  화면 4: 최종 점수                                            │
│  ┌────────────────────┐                                     │
│  │  🏆 당신의 점수:    │                                     │
│  │     4/5           │                                     │
│  │  훌륭해요!         │                                     │
│  └────────────────────┘                                     │
│  [다시 하기] [공유하기]                                       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 프로젝트 생성

```
You: 이런 기능의 퀴즈 Frame 만들어줘:
     1. "퀴즈 시작" 버튼이 있는 환영 화면
     2. Web3/크립토에 대한 5개 질문
     3. 질문당 4개 답변 옵션 (A, B, C, D)
     4. 각 답변 후 정답/오답 표시
     5. 최종 점수 화면
     6. "다시 하기" 버튼

     Next.js와 TypeScript 사용해. @vercel/og로 동적 이미지 만들어줘.

Claude: 완전한 퀴즈 Frame을 만들게요...
        [프로젝트 구조 생성]
        [정답이 있는 질문 배열 설정]
        [동적 이미지 생성 생성]
        [URL 파라미터로 상태 관리 구현]
```

### 3.3 Frames에서 상태 관리

Frames는 **상태가 없습니다** - 각 요청이 독립적이에요. 진행 상황을 어떻게 추적할까요?

**옵션 1: URL 파라미터 (간단)**
```
/api/frame?question=2&score=1
```

**옵션 2: 인코딩된 상태 (더 안전)**
```tsx
// 상태를 하나의 문자열로 인코딩
const state = Buffer.from(JSON.stringify({
  question: 2,
  score: 1,
  answers: [1, 3]
})).toString('base64');

// frame을 통해 전달
<meta property="fc:frame:state" content={state} />
```

**옵션 3: 데이터베이스 (영속성을 위해)**
```tsx
// FID를 키로 데이터베이스에 저장
await db.set(`quiz:${fid}`, {
  question: 2,
  score: 1,
  startedAt: Date.now()
});
```

### 3.4 동적 이미지 생성

즉석에서 아름다운 이미지 생성:

```tsx
// app/api/og/route.tsx
import { ImageResponse } from '@vercel/og';

export async function GET(request: Request) {
  const { searchParams } = new URL(request.url);
  const question = searchParams.get('q') || '1';
  const score = searchParams.get('score') || '0';

  return new ImageResponse(
    (
      <div
        style={{
          display: 'flex',
          flexDirection: 'column',
          alignItems: 'center',
          justifyContent: 'center',
          width: '100%',
          height: '100%',
          backgroundColor: '#1a1a2e',
          color: 'white',
          fontSize: 48,
        }}
      >
        <div style={{ marginBottom: 20 }}>🎯 Web3 퀴즈</div>
        <div style={{ fontSize: 32 }}>질문 {question} / 5</div>
        <div style={{ fontSize: 24, marginTop: 10 }}>점수: {score}</div>
      </div>
    ),
    {
      width: 1200,
      height: 628,
    }
  );
}
```

---

## Step 4: Frame 테스트

### 4.1 ngrok으로 로컬 테스팅

Warpcast는 공개 URL이 필요하므로, 로컬 개발에 ngrok을 사용하세요:

```bash
# 터미널 1: 앱 실행
npm run dev

# 터미널 2: 공개적으로 노출
ngrok http 3000
```

```
You: Frame 테스팅을 위한 ngrok 설정 도와줘

Claude: ngrok을 실행해봅시다:
        1. 설치: npm install -g ngrok (또는 ngrok.com에서 다운로드)
        2. 앱 시작: npm run dev
        3. 다른 터미널에서: ngrok http 3000
        4. https URL 복사 (예: https://abc123.ngrok.io)
        5. .env.local 업데이트: NEXT_PUBLIC_URL=https://abc123.ngrok.io
        6. 개발 서버 재시작
```

### 4.2 Frame Validator 사용

공식 validator가 사용자들이 볼 것을 정확히 보여줍니다:

1. https://warpcast.com/~/developers/frames 이동
2. ngrok URL 붙여넣기
3. Frame 렌더링 확인
4. 버튼 클릭해서 플로우 테스트
5. 디버그 패널에서 에러 확인

```
You: Frame validator가 에러를 보여줘. 이게 내가 보는 것: [에러 붙여넣기]

Claude: 디버그해볼게요...
        일반적인 이슈:
        1. 이미지 URL 접근 불가 (CORS 확인)
        2. 필수 메타 태그 누락
        3. 이미지 크기 잘못됨 (1.91:1 또는 1:1이어야 함)
        4. post_url이 잘못된 content type 반환
```

### 4.3 일반적인 이슈 디버그

| 이슈 | 원인 | 해결책 |
|-----|------|--------|
| "Invalid frame" | 메타 태그 누락 | `fc:frame` 태그 존재 확인 |
| 이미지 로딩 안 됨 | URL이 공개적이지 않음 | ngrok 또는 배포된 URL 사용 |
| 버튼 작동 안 함 | 잘못된 post_url | API 라우트 경로 확인 |
| 상태 유지 안 됨 | 상태 전달 안 함 | URL 파라미터 또는 `fc:frame:state` 사용 |
| "Untrusted data" | 서명 미검증 | OnchainKit으로 검증 |

---

## Step 5: Vercel에 배포

### 5.1 배포 준비

```
You: Frame을 Vercel에 배포해줘

Claude: 단계별로 배포해봅시다:

        1. GitHub에 푸시:
           git add .
           git commit -m "Quiz frame complete"
           git push

        2. Vercel에 연결:
           - vercel.com 이동
           - GitHub 저장소 가져오기
           - Vercel이 Next.js 자동 감지

        3. 환경 변수 설정:
           NEXT_PUBLIC_URL = https://your-project.vercel.app

        4. 배포!
```

### 5.2 환경 변수

Vercel 대시보드 → Settings → Environment Variables에서:

| 변수 | 값 | 목적 |
|------|-----|------|
| `NEXT_PUBLIC_URL` | `https://your-app.vercel.app` | 절대 URL 생성용 |
| `NEYNAR_API_KEY` | Neynar 키 (선택사항) | Frame 서명 검증용 |

### 5.3 Frame 공유하기!

1. Vercel URL 복사
2. Warpcast 열기
3. 새 cast 만들기
4. URL만 붙여넣기 (다른 텍스트 필요 없음)
5. Frame이 자동으로 렌더링됩니다!

```
You: 내 Frame을 바이럴 시키려면 어떻게 해?

Claude: Frame 배포 팁:
        1. 적절한 시간에 포스트 - 관객이 활발할 때
        2. 컨텍스트 추가 - cast 텍스트에 Frame이 뭘 하는지 설명
        3. 관련 채널 태그 - /frames, /base 등
        4. 참여하기 - Frame을 사용하는 사람들에게 답글
        5. 반복하기 - 분석 확인하고 개선
```

---

## Step 6: 온체인 액션 추가

### 6.1 팁 버튼 추가

사용자가 Frame에서 직접 ETH를 보낼 수 있게 하기:

```
You: 사용자가 0.001 ETH 팁을 보낼 수 있는 버튼 추가해줘

Claude: 트랜잭션 버튼을 추가할게요:
```

```html
<!-- 트랜잭션 버튼 -->
<meta property="fc:frame:button:3" content="💰 0.001 ETH 팁" />
<meta property="fc:frame:button:3:action" content="tx" />
<meta property="fc:frame:button:3:target" content="https://your-site.com/api/tip-tx" />
```

```tsx
// app/api/tip-tx/route.ts
export async function POST(req: NextRequest) {
  const YOUR_WALLET = '0x1234...'; // 당신의 지갑 주소

  // 트랜잭션 데이터 반환
  return NextResponse.json({
    chainId: 'eip155:8453', // Base 메인넷
    method: 'eth_sendTransaction',
    params: {
      to: YOUR_WALLET,
      value: '1000000000000000', // 0.001 ETH in wei
    },
  });
}
```

### 6.2 NFT 민팅 버튼 추가

원클릭 NFT 민팅:

```
You: 내 컨트랙트에서 NFT를 민팅하는 버튼 추가해줘

Claude: 민팅 트랜잭션을 만들게요:
```

```html
<meta property="fc:frame:button:4" content="🖼️ NFT 민팅" />
<meta property="fc:frame:button:4:action" content="tx" />
<meta property="fc:frame:button:4:target" content="https://your-site.com/api/mint-tx" />
```

```tsx
// app/api/mint-tx/route.ts
import { encodeFunctionData } from 'viem';

export async function POST(req: NextRequest) {
  const body = await req.json();
  const userAddress = body.untrustedData.address;

  const NFT_CONTRACT = '0xYourContract...';

  // mint 함수 호출 인코딩
  const data = encodeFunctionData({
    abi: [{
      name: 'mint',
      type: 'function',
      inputs: [{ name: 'to', type: 'address' }],
      outputs: [],
    }],
    functionName: 'mint',
    args: [userAddress],
  });

  return NextResponse.json({
    chainId: 'eip155:84532', // Base Sepolia
    method: 'eth_sendTransaction',
    params: {
      to: NFT_CONTRACT,
      data: data,
    },
  });
}
```

### 6.3 트랜잭션 플로우

```
┌─────────────────────────────────────────────────────────────┐
│                 트랜잭션 버튼 플로우                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. 사용자가 "NFT 민팅" 버튼 클릭                             │
│                                                             │
│  2. Warpcast가 당신의 /api/mint-tx 엔드포인트 호출           │
│                                                             │
│  3. 당신의 서버가 트랜잭션 데이터 반환:                       │
│     {                                                       │
│       chainId: "eip155:84532",                             │
│       method: "eth_sendTransaction",                        │
│       params: { to: "0x...", data: "0x..." }               │
│     }                                                       │
│                                                             │
│  4. Warpcast가 사용자에게 트랜잭션 미리보기 표시              │
│     ┌────────────────────────┐                              │
│     │ NFT 민팅               │                              │
│     │ 컨트랙트: 0x1234...    │                              │
│     │ 네트워크: Base Sepolia │                              │
│     │                        │                              │
│     │ [확인] [취소]           │                              │
│     └────────────────────────┘                              │
│                                                             │
│  5. 사용자 확인 → 트랜잭션 전송 → 성공 화면!                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 6.4 데이터베이스로 사용자 액션 추적

투표 카운팅 같은 기능에 데이터베이스 사용:

```
You: 내 투표 Frame에서 투표를 추적하고 싶어. 데이터를 어떻게 저장해?

Claude: 투표 저장을 위해 Vercel KV (Redis)를 설정할게요:
```

```tsx
// Vercel KV 사용
import { kv } from '@vercel/kv';

export async function POST(req: NextRequest) {
  const body = await req.json();
  const fid = body.untrustedData.fid;
  const buttonIndex = body.untrustedData.buttonIndex;

  // 사용자가 이미 투표했는지 확인
  const hasVoted = await kv.sismember('voters', fid.toString());
  if (hasVoted) {
    return renderFrame({ message: "이미 투표했어요!" });
  }

  // 투표 기록
  await kv.sadd('voters', fid.toString());
  await kv.hincrby('votes', `option${buttonIndex}`, 1);

  // 현재 결과 가져오기
  const results = await kv.hgetall('votes');

  return renderFrame({
    message: "투표해주셔서 감사합니다!",
    results: results
  });
}
```

---

## 🔨 연습 1: 투표 Frame 만들기 (초급)

Claude에게 인터랙티브 투표를 만드는 것을 도와달라고 하세요:

```
You: 이런 기능을 가진 Farcaster 투표 Frame 만드는 거 도와줘:
     1. 질문: "가장 좋아하는 블록체인은?"
     2. 옵션: 이더리움, Base, 솔라나, 기타
     3. Vercel KV로 투표 추적
     4. 투표 후 결과 표시 (퍼센티지 포함)
     5. 중복 투표 방지 (FID로 체크)
     6. 총 투표 수 표시

     어두운 테마로 깔끔하고 모던하게 만들어줘.

Claude: 이 투표 Frame을 단계별로 만들어봅시다...
```

**예상 결과물:**
- 4개 옵션이 있는 작동하는 투표
- 영속적인 투표 추적
- 결과 시각화
- 중복 투표 방지

> 💡 **바이브코딩 접근법**: 기술적 세부사항 걱정 마세요. 원하는 걸 설명하고, 테스트하고, 다듬어나가세요!

---

## 🔨 연습 2: Allowlist가 있는 NFT 민팅 Frame 만들기 (고급)

이것은 여러 개념을 결합하는 더 도전적인 프로젝트입니다:

```
You: 이런 기능을 가진 고급 NFT 민팅 Frame을 만들어줘:

     기본 기능:
     1. NFT 미리보기 이미지를 보여주는 환영 화면
     2. "자격 확인" 버튼
     3. 민팅 가격 표시 (Base Sepolia에서 0.001 ETH)

     고급 기능:
     4. Allowlist 확인 - 특정 FID만 민팅 가능
     5. "이미 민팅됨" 확인 - 중복 민팅 방지
     6. 남은 수량을 보여주는 동적 이미지 (예: "42/100 민팅됨")
     7. 성공적인 민팅 후 "OpenSea에서 보기" 링크 버튼 표시
     8. 데이터베이스에 총 민팅 수 추적

     보너스:
     9. allowlist에 없으면 FID를 추가하는 "대기 명단 가입" 버튼 표시
     10. 지갑 연결 상태에 따라 다른 메시지 표시

     Base Sepolia 테스트넷 사용. 적절한 에러 처리 포함.

Claude: 포괄적인 Frame이네요! 모든 기능을 넣어서 만들게요...
```

### 예상 아키텍처:

```
┌─────────────────────────────────────────────────────────────┐
│               NFT 민팅 Frame 아키텍처                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  화면 1: 환영                                                │
│  ┌────────────────────┐                                     │
│  │  [NFT 미리보기]     │                                     │
│  │  "독점 NFT"        │                                     │
│  │  42/100 남음       │                                     │
│  └────────────────────┘                                     │
│  [자격 확인]                                                 │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────┬─────────┬─────────┐                           │
│  │ 리스트에 │리스트에  │ 이미     │                           │
│  │ 있음    │없음     │ 민팅함   │                           │
│  └────┬────┴────┬────┴────┬────┘                           │
│       │         │         │                                 │
│       ▼         ▼         ▼                                 │
│  화면 2a     화면 2b     화면 2c                              │
│  "자격있음!" "allowlist에 "이미 이 NFT를                     │
│  [지금 민팅] 없음"       가지고 있어요"                       │
│             [대기명단]   [OS에서 보기]                        │
│       │         │                                           │
│       ▼         │                                           │
│  화면 3        │                                            │
│  "민팅중..."   │                                            │
│  [tx 대기중]   │                                            │
│       │        │                                            │
│       ▼        │                                            │
│  화면 4       화면 5                                         │
│  "성공!"      "대기명단에                                    │
│  [OS 보기]    추가됨!"                                       │
│  [공유]                                                      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 해결해야 할 핵심 과제:

1. **Allowlist 관리**
   ```tsx
   // allowlist FID 저장
   const ALLOWLIST = new Set([12345, 67890, 11111]);

   // 또는 동적 allowlist를 위해 데이터베이스 사용
   const isAllowed = await kv.sismember('allowlist', fid.toString());
   ```

2. **민팅 추적**
   ```tsx
   // 누가 민팅했는지 추적
   await kv.sadd('minted', fid.toString());
   await kv.incr('totalMinted');
   ```

3. **동적 수량 이미지**
   ```tsx
   // 현재 수량으로 이미지 생성
   const minted = await kv.get('totalMinted') || 0;
   const remaining = 100 - minted;
   // OG 이미지 생성기에 전달
   ```

4. **트랜잭션 + 콜백**
   ```tsx
   // tx 성공 후 데이터베이스 업데이트
   // fc:frame:button:post_url을 사용해 콜백 처리
   ```

이 연습은 다음을 테스트합니다:
- 여러 화면에 걸친 상태 관리
- 데이터베이스 작업
- 트랜잭션 처리
- 조건부 렌더링
- 에러 상태

---

## ❌ 흔한 실수

| 실수 | 증상 | Claude에게 물어볼 것 |
|------|------|---------------------|
| Frame 렌더링 안 됨 | 일반 링크로 표시 | "Frame이 일반 링크로 표시돼. URL: [url]. 메타 태그 디버그해줘." |
| 버튼 작동 안 함 | 클릭해도 아무 일 없음 | "버튼 클릭해도 아무 일 안 돼. postUrl 설정과 API 라우트 확인해줘." |
| 이미지 로딩 안 됨 | 깨진 이미지 아이콘 | "Frame 이미지 안 보여. 이미지 URL: [url]. CORS와 크기 확인해줘." |
| "Invalid frame" 에러 | Validator가 에러 표시 | "Frame validator가 invalid라고 해. 전체 HTML 응답: [코드]" |
| 상태 유지 안 됨 | 클릭 사이에 데이터 유실 | "퀴즈 점수가 리셋돼. frames 사이에 상태 전달하는 거 도와줘." |
| 트랜잭션 실패 | tx 버튼에서 에러 | "트랜잭션 버튼이 실패해: [에러]. tx 엔드포인트 코드: [코드]" |
| 중복 액션 | 사용자가 두 번 투표 가능 | "사용자가 여러 번 투표할 수 있어. FID 기반 중복 제거 추가해줘." |

---

## 🆘 막힐 때

### Frame 개념 이해하기

```
You: Frame 버튼이 어떻게 작동하는지 모르겠어. 간단하게 설명해줘.

Claude: 사용자가 버튼을 클릭하면:
        1. 클릭이 당신의 서버로 감
        2. 당신의 서버가 다음에 뭘 보여줄지 결정
        3. 새 이미지 + 버튼을 돌려보냄

        사용자가 타이핑 대신 클릭하는 대화와 같아요.

        "선택지가 있는 모험" 책이라고 생각하세요:
        - 페이지를 보여줌 (이미지 + 버튼)
        - 사용자가 옵션 선택 (버튼 클릭)
        - 선택에 따라 다음 페이지 보여줌
```

### 배포 디버깅

```
You: Frame이 로컬에선 작동하는데 Vercel에선 안 돼. 도와줘!

Claude: 디버그해봅시다:
        1. NEXT_PUBLIC_URL이 올바르게 설정되었는지 확인
        2. Vercel 대시보드에서 환경 변수 검증
        3. Vercel 함수 로그에서 에러 확인
        4. 모든 API 라우트가 올바르게 export되었는지 확인
        5. Frame validator에서 배포된 URL 테스트
```

### Frame 데이터 검증

```
You: 버튼 클릭이 합법적인지 어떻게 알아?

Claude: Neynar 또는 OnchainKit으로 서명 검증 사용:

        import { validateFrameMessage } from '@coinbase/onchainkit';

        const { isValid, message } = await validateFrameMessage(body);
        if (!isValid) {
          return new Response('Invalid', { status: 400 });
        }

        // 이제 message.data를 신뢰할 수 있음
```

---

## ✅ 체크리스트

넘어가기 전에 확인하세요:

**설정:**
- [ ] Farcaster 계정 생성 (Warpcast)
- [ ] 개발 환경 준비 (Node.js, ngrok)
- [ ] Frame 메타 태그 이해
- [ ] 요청/응답 사이클 이해

**빌딩:**
- [ ] Claude와 함께 로컬에서 작동하는 Frame 만들기!
- [ ] 동적 이미지 생성
- [ ] 버튼 클릭 올바르게 처리
- [ ] frames 사이에 상태 관리

**테스팅:**
- [ ] Frame validator로 테스트
- [ ] 일반적인 이슈 디버그
- [ ] 실제 Warpcast 앱에서 테스트

**배포:**
- [ ] Vercel에 배포
- [ ] 환경 변수 설정
- [ ] Warpcast에서 공유하고 테스트

**연습:**
- [ ] 연습 1: 투표 Frame 완료
- [ ] 연습 2: Allowlist가 있는 NFT 민팅 Frame 완료

**이해:**
- [ ] 다른 사람에게 Frames가 어떻게 작동하는지 설명 가능
- [ ] 트랜잭션 버튼 이해
- [ ] 상태 영속화 방법 알기

---

## 📖 더 배우기

### 공식 문서
- [Farcaster Frames Docs](https://docs.farcaster.xyz/developers/frames/)
- [Frames.js Documentation](https://framesjs.org/)
- [OnchainKit Frame Tools](https://onchainkit.xyz/frame)
- [Warpcast Developer Docs](https://docs.farcaster.xyz/)

### 도구
- [Frame Validator](https://warpcast.com/~/developers/frames) - Frame 테스트
- [Neynar](https://neynar.com/) - Frame 인프라
- [Pinata](https://pinata.cloud/) - Frame 호스팅

### 예시 & 영감
- [Awesome Frames](https://github.com/farcasterxyz/awesome-frames)
- [Frame.js Examples](https://github.com/framesjs/frames.js/tree/main/examples)

### 비디오
- [Building Farcaster Frames](https://www.youtube.com/results?search_query=farcaster+frames+tutorial)
- [Frame Development Deep Dive](https://www.youtube.com/results?search_query=farcaster+frame+development)

---

## 다음은?

**Chapter 30**에서는 **스마트 컨트랙트 크리에이터**가 됩니다 - 직접 온체인 코드를 작성하고 배포해요!

### Web3 스킬의 진화

```
┌─────────────────────────────────────────────────────────────┐
│              당신의 Web3 개발자 여정                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Chapter 28: 소비자                                          │
│  "Web3 앱을 사용하고 어떻게 작동하는지 이해해요"                │
│  └── 지갑, 트랜잭션, 토큰/NFT 클레임                         │
│                                                             │
│  Chapter 29: 빌더 (지금 여기!)                               │
│  "인터랙티브 Web3 경험을 만들 수 있어요"                      │
│  └── 소셜 미디어를 통해 사용자에게 도달하는 Frames           │
│  └── 온체인 액션 (팁, 민팅, 트랜잭션)                        │
│  └── 상태 관리와 사용자 추적                                 │
│                                                             │
│  Chapter 30: 크리에이터                                      │
│  "내 스마트 컨트랙트를 만들 수 있어요"                        │
│  └── Solidity 작성, 컨트랙트 배포, dapp 빌드                │
│                                                             │
│  🎯 최종 프로젝트: NFT 기반 커뮤니티                          │
│  └── 모든 것을 결합: 컨트랙트 + 프론트엔드 + 토큰 게이팅     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Frames 다음에 왜 스마트 컨트랙트인가?

| Frames (Ch29)에서 | 스마트 컨트랙트 (Ch30)에서 |
|------------------|--------------------------|
| **기존** 컨트랙트 사용 | 나만의 컨트랙트 **생성** |
| 컨트랙트가 허용하는 것에 제한 | 로직에 대한 **완전한 통제** |
| 빠르게 빌드, 바이럴 배포 | 더 강력, 더 많은 책임 |
| 컨트랙트 함수 호출 | 컨트랙트 함수 정의 |

### Chapter 30에서 만들 것

> 💡 **미리보기**: Chapter 30에서는 직접 NFT 컨트랙트를 작성하고 NFT 보유자만 독점 콘텐츠에 접근할 수 있는 완전한 멤버십 사이트를 만들 거예요. 다음을 결합한다고 상상해보세요:
> - 나만의 스마트 컨트랙트 (TipJar 또는 NFT 같은)
> - 당신의 컨트랙트에서 민팅하는 Frame
> - 토큰 게이팅된 커뮤니티 웹사이트
>
> 이것은 배운 모든 것의 결정체입니다!

---

## 🌉 Chapter 30으로의 다리: 스마트 컨트랙트 미리보기

넘어가기 전에, 다음에 무엇이 올지 살짝 들여다봅시다! Chapter 30에서는 **기존 컨트랙트 사용**에서 **나만의 컨트랙트 생성**으로 전환합니다.

### 왜 나만의 스마트 컨트랙트를 작성할까?

이 챕터에서는 `encodeFunctionData`를 사용해 기존 컨트랙트를 호출했습니다:

```tsx
// Chapter 29에서 한 것 - 다른 사람의 컨트랙트 호출
const data = encodeFunctionData({
  abi: [...],
  functionName: 'mint',
  args: [userAddress],
});
```

하지만 다음을 원한다면요?:
- 커스텀 규칙이 있는 나만의 NFT 생성?
- 로직을 통제하는 팁 항아리 구축?
- 나만의 요구사항으로 멤버십 시스템 설정?

**Solidity**를 작성해야 합니다 - 스마트 컨트랙트의 언어!

### Solidity 첫 번째 맛보기

배울 것을 살짝 미리 보여드릴게요. 이것은 간단한 스마트 컨트랙트입니다:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MyFirstContract {
    // 블록체인에 영원히 저장되는 변수!
    string public message;

    // 누군가 컨트랙트를 배포할 때 실행됨
    constructor() {
        message = "Hello, Web3!";
    }

    // 누구나 이걸 호출해서 메시지 변경 가능
    function setMessage(string memory newMessage) public {
        message = newMessage;
    }
}
```

```
You: JavaScript랑 비슷해 보여!

Claude: 비슷해요! 주요 차이점:
        - 서버가 아닌 블록체인에서 실행
        - 모든 변경에 "가스" (작은 수수료) 발생
        - 배포되면 코드 변경 불가
        - 모든 것이 공개적이고 영구적

        걱정 마세요 - Chapter 30에서 같이 작성할 거예요!
```

### Frames + 스마트 컨트랙트가 함께 작동하는 방법

마스터할 강력한 조합입니다:

```
┌─────────────────────────────────────────────────────────────┐
│           완전한 Web3 빌더 스택                                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  CHAPTER 30: 당신의 스마트 컨트랙트                          │
│  ┌─────────────────────────────┐                           │
│  │ contract MembershipNFT {    │                           │
│  │   function mint() public {} │  ← 이걸 당신이 작성!       │
│  │   function hasPass() ...    │                           │
│  │ }                           │                           │
│  └──────────────┬──────────────┘                           │
│                 │                                           │
│                 ▼                                           │
│  CHAPTER 29: 당신의 Frame                                   │
│  ┌─────────────────────────────┐                           │
│  │ [NFT 미리보기 이미지]        │                           │
│  │ "우리 커뮤니티에 가입하세요!" │                           │
│  │ [멤버십 NFT 민팅]           │  ← 당신의 컨트랙트 호출!   │
│  └──────────────┬──────────────┘                           │
│                 │                                           │
│                 ▼                                           │
│  당신의 웹사이트: 토큰 게이팅된 콘텐츠                        │
│  ┌─────────────────────────────┐                           │
│  │ if (hasNFT) {               │                           │
│  │   show secret content       │  ← 당신의 컨트랙트 사용!   │
│  │ }                           │                           │
│  └─────────────────────────────┘                           │
│                                                             │
│  이것이 Chapter 30의 최종 프로젝트입니다!                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 안전 먼저: 반드시 알아야 할 한 가지

> ⚠️ **Chapter 30을 위한 중요한 사전 알림**

스마트 컨트랙트는 실제 돈을 다루고, 실수는 영구적일 수 있습니다. Chapter 30에서 배울 것:

```
스마트 컨트랙트를 안전하게 바이브코딩하기:

✅ 바이브코딩해도 안전한 것:
- 테스트넷에서 학습하고 실험하기
- 개념 이해하기
- 프로토타입 빌딩
- 작고 간단한 컨트랙트

❌ 바이브코딩하면 위험한 것:
- 실제 돈이 있는 프로덕션 컨트랙트
- 복잡한 DeFi 로직
- 상당한 가치를 다루는 모든 것

규칙: 테스트넷 먼저, 항상!
```

겁먹지 마세요 - 실수해도 비용이 없는 테스트넷에서 안전하게 연습할 거예요. 하지만 이 인식이 Chapter 30 준비가 된 이유입니다!

### 다가올 것

| Chapter 30 섹션 | 배울 것 |
|----------------|---------|
| 스마트 컨트랙트 기초 | 컨트랙트가 무엇인지, 어떻게 작동하는지 |
| 첫 번째 컨트랙트 | "Hello World"를 블록체인에 배포 |
| TipJar 컨트랙트 | ETH로 유용한 것 만들기 |
| 프론트엔드 연결 | Next.js 앱을 컨트랙트에 연결 |
| NFT 컨트랙트 | 나만의 ERC721 NFT 생성 |
| **최종 프로젝트** | NFT 기반 커뮤니티 (모든 것을 결합!) |

> 🚀 **준비됐어요!** 지갑 연결 (Ch28)을 마스터하고, 인터랙티브 Frames (Ch29)를 빌드했으니, 이제 스마트 컨트랙트 크리에이터가 될 준비가 됐어요. 가봅시다!

---

<p align="center">
  <a href="../Chapter28-Web3-Basics/README.ko.md">← 이전: Web3 기초</a> |
  <a href="../Chapter30-Base-Smart-Contracts/README.ko.md">다음: Base 스마트 컨트랙트 →</a>
</p>

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
