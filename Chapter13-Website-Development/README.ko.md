# Chapter 13: 웹사이트 개발

[English](./README.md) | **한국어**

---

## 💬 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이 챕터에서 배우는 것

- Claude와 함께 처음부터 웹사이트 만들기
- HTML, CSS, JavaScript의 기본 개념
- 개인 포트폴리오 사이트 완성하기
- 반응형 웹 디자인의 원리
- 피드백 기반 반복 개발 방법

---

## 이전 챕터와의 연결

Chapter 11에서 Git으로 코드를 관리하는 방법을, Chapter 12에서 CLAUDE.md로 프로젝트 규칙을 설정하는 방법을 배웠습니다.

이제 이론은 끝났습니다. **실제로 웹사이트를 만들어볼 시간입니다!**

이번 챕터에서는 지금까지 배운 모든 것을 총동원해서 **개인 포트폴리오 웹사이트**를 처음부터 끝까지 만들어봅니다.

---

## 왜 필요합니까?

생각해보세요 - 모든 회사, 프리랜서, 전문가는 웹사이트가 있습니다. 누군가 당신을 온라인에서 검색하면 무엇을 찾을 것입니까?

### 포트폴리오 웹사이트가 필요한 실제 상황

| 상황 | 포트폴리오 없이 | 포트폴리오 있으면 |
|------|---------------|-----------------|
| **취업 준비** | 이력서만 제출 | "제 작업물 보세요" 링크 첨부 |
| **프리랜서** | "저 잘해요" 말로만 | 실제 작업 결과물 시연 |
| **개인 브랜딩** | 수천 명 중 하나 | 기억에 남는 첫인상 |
| **네트워킹** | 연락처 교환 | "제 사이트 방문해보세요" |

> 💡 **비전공자 팁**: 포트폴리오는 디지털 명함 같지만, 훨씬 더 강력합니다. 당신이 "할 수 있다고 말하는 것"이 아니라 "실제로 할 수 있는 것"을 보여줍니다.

### 쉬운 비유: 포트폴리오는 쇼윈도우

당신이 제빵사라고 상상해보세요:

```
옵션 A: "맛있는 케이크를 만듭니다"라고 적힌 명함
옵션 B: 아름다운 케이크로 가득 찬 가게 쇼윈도우
```

어느 쪽이 사람들이 사고 싶게 만들겠습니까?

포트폴리오 웹사이트는 바로 그 쇼윈도우입니다 - 당신의 최고의 작품을 세상에 전시합니다.

---

## 웹 개발의 기초 개념

실습에 들어가기 전에, 웹사이트가 어떻게 구성되는지 간단히 알아봅시다.

### HTML, CSS, JavaScript의 역할

웹사이트는 세 가지 기술로 만들어집니다:

```
🏗️ HTML  = 뼈대 (구조)
🎨 CSS   = 옷 (스타일)
⚡ JavaScript = 움직임 (동작)
```

**집을 짓는 것에 비유하면:**

| 기술 | 역할 | 집 비유 |
|------|------|---------|
| **HTML** | 콘텐츠와 구조 | 벽돌, 방, 문 |
| **CSS** | 디자인과 레이아웃 | 페인트, 가구, 인테리어 |
| **JavaScript** | 인터랙션과 동작 | 전기, 수도, 스위치 |

### 실제 코드 예시

```html
<!-- HTML: 구조 -->
<button>클릭하세요</button>
```

```css
/* CSS: 스타일 */
button {
  background-color: blue;
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
}
```

```javascript
// JavaScript: 동작
button.addEventListener('click', function() {
  alert('버튼이 클릭되었습니다!');
});
```

> ⚠️ **주의사항**: 코드를 외울 필요 없습니다! Claude가 대신 작성해줍니다. 여기서는 개념만 이해하면 됩니다.

---

## 따라해보기: 가장 간단한 포트폴리오

전체 프로젝트에 들어가기 전에, 최소한의 예제로 모든 게 작동하는지 확인해봅시다.

```
> "홍길동" 이라는 이름을 제목으로 하고
> 내가 뭘 하는 사람인지 한 문장만 들어간 HTML 파일 만들어줘. 그것만.
```

**Claude가 만들어주는 코드:**

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>홍길동</title>
</head>
<body>
    <h1>홍길동</h1>
    <p>웹 개발을 공부하는 대학생입니다.</p>
</body>
</html>
```

**실행 방법:**
1. 파일을 더블클릭
2. 브라우저에서 열림
3. 이름이 보이면 성공!

> 💡 **비전공자 팁**: 방금 첫 번째 웹페이지를 만들었습니다! 나머지는 전부 이 기초 위에 쌓아가는 것뿐입니다.

---

## 드디어 실전입니다

이번 챕터에서는 **개인 포트폴리오 웹사이트**를 처음부터 끝까지 만듭니다.

### 무엇을 만들 것입니까?

```
┌─────────────────────────────────────┐
│           헤더 (네비게이션)           │
├─────────────────────────────────────┤
│                                     │
│         자기소개 섹션                 │
│    (프로필 사진, 이름, 소개)          │
│                                     │
├─────────────────────────────────────┤
│                                     │
│         프로젝트 갤러리               │
│    [카드1] [카드2] [카드3]           │
│                                     │
├─────────────────────────────────────┤
│                                     │
│          연락처 폼                   │
│    이름 / 이메일 / 메시지 / 전송     │
│                                     │
├─────────────────────────────────────┤
│              푸터                    │
└─────────────────────────────────────┘
```

**완성되면 갖게 되는 것:**
- 자기소개 페이지
- 프로젝트 갤러리
- 연락처 폼
- 반응형 디자인 (모바일에서도 보기 좋게)

---

## 준비하기

### 1. 프로젝트 폴더 만들기

```
> my-portfolio 폴더 만들어줘
```

**Claude의 응답:**
```
my-portfolio 폴더를 만들었습니다.
```

### 2. CLAUDE.md 설정

Chapter 12에서 배운 대로, 먼저 프로젝트 규칙을 설정합니다.

```
> CLAUDE.md 만들어줘. 내용은:
> - HTML, CSS, JavaScript 사용
> - Tailwind CSS로 스타일링 (CDN 사용)
> - 모바일 반응형 필수
> - 한글 주석
> - 파일명은 소문자
```

**실제 대화 예시:**

```
사용자: CLAUDE.md 만들어줘. HTML, CSS, JS 프로젝트야.
        Tailwind CSS 쓰고, 모바일 반응형 필수.

Claude: CLAUDE.md를 만들었습니다:

# 포트폴리오 프로젝트

## 기술 스택
- HTML5
- CSS3 (Tailwind CSS CDN)
- JavaScript (Vanilla)

## 규칙
- 모바일 우선 반응형 디자인
- 시맨틱 HTML 태그 사용
- 한글 주석 작성
- 파일명은 소문자와 하이픈 사용

## 금지 사항
- 인라인 스타일 금지 (Tailwind 클래스 사용)
- jQuery 사용 금지
```

### 3. 기본 파일 구조

```
> 기본 파일 구조 만들어줘:
> - index.html (메인 페이지)
> - style.css (커스텀 스타일)
> - script.js (동작)
> - images/ 폴더 (이미지용)
```

**만들어진 구조:**
```
my-portfolio/
├── CLAUDE.md
├── index.html
├── style.css
├── script.js
└── images/
```

> 🔥 **프로 팁**: 처음부터 구조를 잡고 시작하면 나중에 파일 찾기가 훨씬 쉬워집니다.

---

## 단계별 개발

### 1단계: 기본 HTML 만들기

```
> index.html에 기본 구조 만들어줘.
> 헤더, 메인, 푸터 섹션으로 나눠줘.
> Tailwind CSS CDN 연결해줘.
```

**Claude가 만드는 코드:**

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>내 포트폴리오</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="style.css" rel="stylesheet">
</head>
<body class="bg-gray-50">
    <!-- 헤더: 네비게이션 영역 -->
    <header class="bg-white shadow">
        <!-- 네비게이션 내용 -->
    </header>

    <!-- 메인: 주요 콘텐츠 영역 -->
    <main>
        <!-- 자기소개 섹션 -->
        <section id="about">
        </section>

        <!-- 프로젝트 섹션 -->
        <section id="projects">
        </section>

        <!-- 연락처 섹션 -->
        <section id="contact">
        </section>
    </main>

    <!-- 푸터: 하단 정보 -->
    <footer class="bg-gray-800 text-white">
        <!-- 푸터 내용 -->
    </footer>

    <script src="script.js"></script>
</body>
</html>
```

> 💡 **비전공자 팁**: `<header>`, `<main>`, `<footer>` 같은 태그를 "시맨틱 태그"라고 합니다. 의미가 있는 태그라서 검색엔진이나 스크린 리더가 페이지를 더 잘 이해합니다.

### 2단계: 자기소개 섹션

```
> 자기소개 섹션 추가해줘.
> - 프로필 사진 자리 (원형)
> - 이름 (크게)
> - 한 줄 소개 (작게)
> - 자기소개 문단 (2-3줄)
> - 가운데 정렬
```

**실제 대화 예시:**

```
사용자: 자기소개 섹션 만들어줘. 프로필 사진, 이름, 소개 포함.

Claude: 자기소개 섹션을 추가했습니다:

<section id="about" class="py-20 px-4">
    <div class="max-w-4xl mx-auto text-center">
        <!-- 프로필 사진 -->
        <div class="mb-6">
            <img
                src="images/profile.jpg"
                alt="프로필 사진"
                class="w-32 h-32 rounded-full mx-auto object-cover border-4 border-blue-500"
            >
        </div>

        <!-- 이름 -->
        <h1 class="text-4xl font-bold text-gray-800 mb-2">
            홍길동
        </h1>

        <!-- 한 줄 소개 -->
        <p class="text-xl text-blue-600 mb-6">
            웹 개발자 | 문제 해결사 | 평생 학습자
        </p>

        <!-- 자기소개 문단 -->
        <p class="text-gray-600 max-w-2xl mx-auto leading-relaxed">
            안녕하세요! 사용자 경험을 개선하는 웹 애플리케이션을
            만드는 것을 좋아하는 개발자입니다. 새로운 기술을 배우고
            실제 문제를 해결하는 데 열정을 가지고 있습니다.
        </p>
    </div>
</section>

프로필 사진을 images/profile.jpg에 넣으시면 됩니다.
아직 사진이 없다면 플레이스홀더 이미지를 사용할까요?
```

### 3단계: 프로젝트 갤러리

```
> 프로젝트 섹션 추가해줘.
> - 카드 형태로 3개 프로젝트
> - 각 카드에 썸네일, 제목, 설명, 태그
> - 호버하면 살짝 올라오는 효과
> - 3열 그리드 (모바일에선 1열)
```

**Claude가 만드는 코드:**

```html
<section id="projects" class="py-20 px-4 bg-white">
    <div class="max-w-6xl mx-auto">
        <!-- 섹션 제목 -->
        <h2 class="text-3xl font-bold text-center text-gray-800 mb-12">
            프로젝트
        </h2>

        <!-- 프로젝트 카드 그리드 -->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">

            <!-- 프로젝트 카드 1 -->
            <div class="bg-gray-50 rounded-lg overflow-hidden shadow-md
                        hover:shadow-xl hover:-translate-y-2
                        transition-all duration-300">
                <!-- 썸네일 -->
                <img
                    src="images/project1.jpg"
                    alt="프로젝트 1"
                    class="w-full h-48 object-cover"
                >

                <!-- 카드 내용 -->
                <div class="p-6">
                    <h3 class="text-xl font-semibold mb-2">
                        할 일 목록 앱
                    </h3>
                    <p class="text-gray-600 mb-4">
                        React로 만든 할 일 관리 애플리케이션입니다.
                        로컬 스토리지로 데이터를 저장합니다.
                    </p>

                    <!-- 태그 -->
                    <div class="flex flex-wrap gap-2">
                        <span class="px-3 py-1 bg-blue-100 text-blue-600
                                     text-sm rounded-full">
                            React
                        </span>
                        <span class="px-3 py-1 bg-green-100 text-green-600
                                     text-sm rounded-full">
                            JavaScript
                        </span>
                    </div>
                </div>
            </div>

            <!-- 프로젝트 카드 2, 3도 비슷한 구조로... -->

        </div>
    </div>
</section>
```

> 🔥 **프로 팁**: `hover:shadow-xl hover:-translate-y-2`는 Tailwind CSS의 호버 효과입니다. 마우스를 올리면 그림자가 커지고 카드가 살짝 올라갑니다.

### 4단계: 연락처 폼

```
> 연락처 섹션 추가해줘.
> - 이름 입력 (필수)
> - 이메일 입력 (필수, 이메일 형식)
> - 메시지 입력 (텍스트 영역)
> - 보내기 버튼
> - 폼 검증은 일단 없어도 됨
```

**Claude가 만드는 코드:**

```html
<section id="contact" class="py-20 px-4 bg-gray-100">
    <div class="max-w-2xl mx-auto">
        <h2 class="text-3xl font-bold text-center text-gray-800 mb-12">
            연락하기
        </h2>

        <form class="bg-white rounded-lg shadow-md p-8">
            <!-- 이름 입력 -->
            <div class="mb-6">
                <label
                    for="name"
                    class="block text-gray-700 font-medium mb-2"
                >
                    이름
                </label>
                <input
                    type="text"
                    id="name"
                    name="name"
                    required
                    class="w-full px-4 py-2 border border-gray-300
                           rounded-lg focus:ring-2 focus:ring-blue-500
                           focus:border-transparent outline-none"
                    placeholder="홍길동"
                >
            </div>

            <!-- 이메일 입력 -->
            <div class="mb-6">
                <label
                    for="email"
                    class="block text-gray-700 font-medium mb-2"
                >
                    이메일
                </label>
                <input
                    type="email"
                    id="email"
                    name="email"
                    required
                    class="w-full px-4 py-2 border border-gray-300
                           rounded-lg focus:ring-2 focus:ring-blue-500
                           focus:border-transparent outline-none"
                    placeholder="example@email.com"
                >
            </div>

            <!-- 메시지 입력 -->
            <div class="mb-6">
                <label
                    for="message"
                    class="block text-gray-700 font-medium mb-2"
                >
                    메시지
                </label>
                <textarea
                    id="message"
                    name="message"
                    rows="5"
                    required
                    class="w-full px-4 py-2 border border-gray-300
                           rounded-lg focus:ring-2 focus:ring-blue-500
                           focus:border-transparent outline-none resize-none"
                    placeholder="메시지를 입력하세요..."
                ></textarea>
            </div>

            <!-- 보내기 버튼 -->
            <button
                type="submit"
                class="w-full bg-blue-600 text-white py-3 rounded-lg
                       font-medium hover:bg-blue-700
                       transition-colors duration-200"
            >
                메시지 보내기
            </button>
        </form>
    </div>
</section>
```

### 5단계: 네비게이션 완성

```
> 헤더에 네비게이션 추가해줘.
> - 왼쪽에 로고 (텍스트로 "GD")
> - 오른쪽에 메뉴 (소개, 프로젝트, 연락처)
> - 클릭하면 해당 섹션으로 스크롤
> - 모바일에선 햄버거 메뉴
```

### 6단계: 반응형 만들기

```
> 모바일에서도 잘 보이게 확인해줘.
> 특히:
> - 네비게이션이 햄버거로 바뀌는지
> - 프로젝트 카드가 1열로 되는지
> - 글씨 크기가 적절한지
```

**실제 대화 예시:**

```
사용자: 모바일에서 레이아웃 확인해줘

Claude: 현재 반응형 설정을 확인해볼게요.

✅ 잘 되어 있는 것:
- 프로젝트 그리드: lg:grid-cols-3 → md:grid-cols-2 → grid-cols-1
- 패딩과 마진이 반응형으로 설정됨
- 이미지가 object-cover로 비율 유지

⚠️ 개선이 필요한 것:
- 네비게이션에 모바일 햄버거 메뉴 없음
- 제목 크기가 모바일에서 좀 큼

수정할까요?

사용자: 응, 수정해줘

Claude: 모바일 최적화를 적용했습니다:

1. 햄버거 메뉴 추가 (모바일에서만 표시)
2. 제목 크기 조정: text-4xl → text-2xl md:text-4xl
3. 섹션 패딩 조정: py-20 → py-12 md:py-20

브라우저 개발자 도구(F12)에서 모바일 화면으로 확인해보세요.
```

---

## 피드백으로 개선하기

첫 결과가 마음에 안 들어도 괜찮습니다. 피드백을 주세요!

### 레이아웃 피드백

```
> 헤더가 너무 커. 높이 줄여줘.
> 프로젝트 카드 사이 간격 넓혀줘.
> 연락처 폼이 너무 좁아. 더 넓게.
```

### 색상/스타일 피드백

```
> 버튼 색이 별로야. 더 진한 파란색으로.
> 배경이 너무 밋밋해. 그라데이션 넣어줘.
> 글씨가 너무 작아. 본문 크기 키워줘.
```

### 효과 피드백

```
> 페이지 로드할 때 페이드인 효과 넣어줘.
> 스크롤할 때 요소들이 순차적으로 나타나게.
> 버튼에 클릭 효과 추가해줘.
```

**실제 대화 예시 (피드백 반복):**

```
사용자: 헤더가 너무 밋밋해

Claude: 어떤 스타일로 바꿔드릴까요?
1. 그라데이션 배경
2. 투명 배경 + 스크롤시 흰색
3. 진한 색상 배경

사용자: 2번으로

Claude: 스크롤하면 배경이 나타나는 헤더로 바꿨습니다.

[JavaScript 추가]
window.addEventListener('scroll', function() {
    const header = document.querySelector('header');
    if (window.scrollY > 50) {
        header.classList.add('bg-white', 'shadow');
    } else {
        header.classList.remove('bg-white', 'shadow');
    }
});

스크롤해서 확인해보세요!

사용자: 좋아! 근데 transition 효과 넣어줘. 너무 갑자기 바뀜.

Claude: transition을 추가했습니다.

header {
    transition: all 0.3s ease;
}

이제 부드럽게 변합니다.
```

> 💡 **비전공자 팁**: 2-3번 피드백 주면 원하는 디자인이 나옵니다. 처음부터 완벽할 필요 없습니다!

---

## 실제 데이터 넣기

더미 데이터를 실제 내용으로 바꾸세요.

### 프로필 정보 수정

```
> 프로필 이름을 "김철수"로 바꿔줘.
> 소개는 "프론트엔드 개발을 공부하는 대학생입니다"로.
> 한 줄 소개는 "React 러버 | UI/UX 관심"으로.
```

### 프로젝트 정보 수정

```
> 프로젝트 1번을:
> - 제목: "영화 검색 앱"
> - 설명: "TMDB API를 활용한 영화 정보 검색 서비스"
> - 태그: React, API, Tailwind
> 로 바꿔줘.
```

### 이미지 추가 방법

**옵션 1: 플레이스홀더 이미지 사용**
```
> 프로필 사진 없으면 플레이스홀더로 대체해줘
```

**옵션 2: 실제 이미지 사용**
```
images/ 폴더에 파일을 넣고:
- profile.jpg (프로필 사진)
- project1.jpg (프로젝트 1 썸네일)
- project2.jpg (프로젝트 2 썸네일)
```

> ⚠️ **주의사항**: 이미지 파일명의 대소문자에 주의하세요! `Profile.jpg`와 `profile.jpg`는 다릅니다.

---

## 결과 확인하기

### 브라우저에서 보기

```
> index.html 브라우저에서 열어줘
```

또는 직접 파일을 더블클릭해서 열어보세요.

### 개발 서버로 보기 (더 좋은 방법)

```
> 개발 서버 띄워줘
```

Claude가 간단한 서버를 실행해줍니다:

```
사용자: 개발 서버 띄워줘

Claude: Python 간단한 서버를 실행할게요.

[실행: python -m http.server 3000]

서버가 시작되었습니다!
브라우저에서 http://localhost:3000 을 열어보세요.

종료하려면 Ctrl+C를 누르세요.
```

> 🔥 **프로 팁**: 개발 서버를 사용하면 파일을 수정할 때마다 새로고침만 하면 됩니다. 더블클릭보다 편리합니다.

---

## 자주 쓰는 요청들

### 레이아웃 관련

```
> 2단 레이아웃으로 바꿔줘. 왼쪽에 사이드바, 오른쪽에 메인 컨텐츠.
> 카드들 3열로 배치해줘.
> 가운데 정렬해줘.
> 요소들 간격 더 넓게/좁게.
```

### 스타일 관련

```
> 그림자 효과 추가해줘.
> 둥근 모서리로 바꿔줘.
> 호버 효과 추가해줘.
> 색상 팔레트 파란색 계열로 통일해줘.
> 폰트를 Pretendard로 바꿔줘.
```

### 동작 관련

```
> 스크롤하면 헤더가 고정되게 해줘.
> 부드럽게 스크롤되게 해줘.
> 이미지 클릭하면 크게 보이게 해줘.
> 맨 위로 가기 버튼 추가해줘.
> 다크 모드 토글 추가해줘.
```

---

## 🎯 미니 퀴즈

### 퀴즈 1
Q: HTML, CSS, JavaScript의 역할은 각각 무엇입니까?

<details>
<summary>정답 보기</summary>

- **HTML**: 구조와 콘텐츠 (뼈대)
- **CSS**: 디자인과 스타일 (옷)
- **JavaScript**: 인터랙션과 동작 (움직임)
</details>

### 퀴즈 2
Q: Tailwind CSS에서 `hover:shadow-xl`은 무엇을 의미합니까?

<details>
<summary>정답 보기</summary>

마우스를 올렸을 때(hover) 큰 그림자(shadow-xl)가 적용됩니다.
</details>

### 퀴즈 3
Q: `grid-cols-1 md:grid-cols-3`는 무엇을 의미합니까?

<details>
<summary>정답 보기</summary>

기본(모바일)에서는 1열, 중간 화면(md) 이상에서는 3열로 표시됩니다. 반응형 디자인입니다.
</details>

---

## 📝 실습 과제

### 난이도 1: 기본 포트폴리오 (초급)

위 단계를 따라 기본 틀 만들기:

1. 프로젝트 폴더 생성
2. CLAUDE.md 설정
3. 기본 HTML 구조
4. 자기소개 섹션
5. 프로젝트 카드 3개
6. 연락처 폼

**완료 체크:**
- [ ] 모든 섹션이 있음
- [ ] 기본 스타일이 적용됨
- [ ] 브라우저에서 볼 수 있음

### 난이도 2: 스타일 커스터마이징 (중급)

기본 포트폴리오에 개성 더하기:

```
> 색상 테마를 민트 그린으로 바꿔줘
> 모든 버튼에 호버 효과 추가
> 카드에 그라데이션 테두리 추가
> 폰트를 Noto Sans KR로 바꿔줘
```

**완료 체크:**
- [ ] 고유한 색상 테마
- [ ] 애니메이션 효과들
- [ ] 커스텀 폰트 적용

### 난이도 3: 기능 추가 (고급)

```
> 다크 모드 토글 버튼 추가해줘
> 프로젝트 카드 필터링 기능 (태그로)
> 스크롤 프로그레스 바 추가
> 타이핑 애니메이션으로 소개 문구 표시
```

**완료 체크:**
- [ ] 다크/라이트 모드 전환
- [ ] 프로젝트 필터링 작동
- [ ] 고급 애니메이션

---

## 🏆 도전 과제

### 도전 1: 다중 페이지
```
> 별도의 about.html 페이지 만들어줘.
> 더 자세한 경력과 스킬 목록 포함.
> 네비게이션에서 연결해줘.
```

### 도전 2: 애니메이션 마스터
```
> 스크롤하면 요소들이 순차적으로 나타나게 해줘.
> Intersection Observer 사용해서.
```

### 도전 3: 블로그 추가
```
> 블로그 섹션 추가해줘.
> - 글 목록 페이지
> - 개별 글 페이지
> - 마크다운 형식 지원
```

---

## 문제가 발생하면

### HTML 파일을 열었는데 아무것도 안 보입니다
- 파일이 `.html` 확장자로 저장되었는지 확인하세요 (`.txt`가 아니라)
- 파일을 우클릭해서 "연결 프로그램" > 브라우저 선택해보세요
- 코드에 오류가 없는지 Claude에게 확인 요청

```
> index.html에 문법 에러 있는지 확인해줘
```

### 페이지가 너무 밋밋합니다 / 스타일이 없습니다

```
> Tailwind CSS CDN이 제대로 연결됐는지 확인해줘
```

흔한 원인:
- CDN 스크립트가 `<head>` 안에 없음
- 인터넷 연결 필요 (CDN은 온라인이어야 작동)
- 클래스명 오타

### 이미지가 안 나옵니다

```
> 이미지 경로 확인해줘
```

체크할 것:
- 이미지 파일이 프로젝트 폴더에 있는지
- 파일명 대소문자가 정확한지 (`Photo.jpg` vs `photo.jpg`)
- 경로가 올바른지 (`images/photo.jpg` vs `./images/photo.jpg`)

### 수정한 게 안 보입니다

1. 브라우저 강력 새로고침: `Ctrl+Shift+R` (Windows) 또는 `Cmd+Shift+R` (Mac)
2. 파일을 저장했는지 확인
3. 올바른 파일을 수정했는지 확인

### 모바일에서 레이아웃이 이상합니다

```
> 모바일 반응형 클래스 확인해줘
```

흔한 문제:
- `viewport` 메타 태그가 없음
- 반응형 클래스(md:, lg:)가 적용 안 됨
- 고정 width 사용 (px 대신 % 또는 rem 사용)

---

## 자주 하는 실수

### 실수 1: 처음부터 너무 복잡하게 시작하기

```
# ❌ 나쁜 접근
> 애니메이션, 3D 효과, 비디오 배경, 파티클 효과,
> 챗봇이 있는 포트폴리오 만들어줘.

# ✅ 좋은 접근
> 자기소개, 프로젝트, 연락처 섹션이 있는
> 간단한 포트폴리오 만들어줘.
```

**왜 문제입니까?**
- 기본이 안 되면 고급 기능도 안 됨
- 디버깅이 어려워짐
- 완성하지 못하고 포기하게 됨

> 💡 **비전공자 팁**: 간단하게 시작하고, 기능은 나중에 추가하세요. "완성된 간단한 것"이 "미완성된 복잡한 것"보다 낫습니다.

### 실수 2: 모바일에서 테스트 안 하기

```
# ❌ 나쁜 접근
(큰 모니터에서만 확인)

# ✅ 좋은 접근
> 이거 모바일에서 어떻게 보이는지 확인해줘
```

**왜 문제입니까?**
대부분의 사람들은 핸드폰으로 웹사이트를 봅니다. 모바일에서 깨지면 방문자의 절반 이상이 떠납니다.

### 실수 3: 플레이스홀더 콘텐츠 바꾸는 걸 잊기

Claude는 "Lorem ipsum" 같은 더미 텍스트와 예시 이미지를 사용합니다.

```
# 배포 전 체크리스트
- [ ] 모든 "Lorem ipsum" 텍스트가 실제 내용으로 바뀌었는가?
- [ ] 플레이스홀더 이미지가 실제 이미지로 바뀌었는가?
- [ ] 예시 연락처 정보가 실제 정보로 바뀌었는가?
```

### 실수 4: 폰트와 색상을 너무 많이 쓰기

```
# ❌ 나쁜 예
5가지 다른 폰트, 10가지 색상

# ✅ 좋은 예
폰트 1-2개, 메인 색상 2-3개
```

**왜 문제입니까?**
- 지저분하고 비전문적으로 보임
- 일관성 없는 디자인
- 방문자가 집중하기 어려움

### 실수 5: 아무 데도 안 가는 링크

```
> 이 프로젝트의 모든 링크 확인해서 끊어진 거 알려줘
```

모든 링크는 어딘가로 연결되어야 합니다:
- `#` 링크는 페이지 상단으로 (의도한 것인지 확인)
- 외부 링크는 실제로 작동하는지
- 내부 링크(앵커)의 ID가 존재하는지

---

## 완성 후 체크리스트

배포 전에 모두 확인하세요:

**콘텐츠:**
- [ ] 모든 더미 텍스트가 실제 내용으로 바뀌었는가?
- [ ] 오타는 없는가?
- [ ] 이미지가 다 나오는가?

**기능:**
- [ ] 모든 링크가 동작하는가?
- [ ] 폼 입력이 되는가?
- [ ] 네비게이션이 작동하는가?

**반응형:**
- [ ] 모바일에서 잘 보이는가?
- [ ] 태블릿에서 잘 보이는가?
- [ ] 큰 화면에서도 깨지지 않는가?

**성능:**
- [ ] 이미지가 최적화되었는가? (너무 크지 않게)
- [ ] 페이지 로드 속도가 괜찮은가?

---

## 미니 프로젝트: 개인 블로그

포트폴리오를 만들었으니, 이번엔 블로그를 만들어보세요.

### 목표
- 다중 페이지 사이트 구축
- 콘텐츠 관리 이해

### 블로그 만들기

```
> 개인 블로그 만들어줘.
> - 홈: 최신 글 목록 (카드 형식)
> - 글 페이지: 개별 글 내용
> - 소개 페이지
> - 카테고리별 분류
```

### 기능 추가

```
> 검색 기능 추가해줘
> 태그 시스템 추가해줘
> 다크 모드 지원해줘
> 글 읽기 예상 시간 표시해줘
```

### 심화 과제 (숙련자용)

```
> 마크다운으로 글 작성할 수 있게 해줘
> 정적 사이트 생성기처럼 빌드하게 해줘
> SEO 최적화해줘 (meta 태그, Open Graph)
```

---

## 용어 사전

| 용어 | 설명 |
|------|------|
| **HTML** | 웹페이지의 구조를 정의하는 마크업 언어 |
| **CSS** | 웹페이지의 스타일을 정의하는 스타일시트 언어 |
| **JavaScript** | 웹페이지에 동적 기능을 추가하는 프로그래밍 언어 |
| **Tailwind CSS** | 유틸리티 기반 CSS 프레임워크 |
| **반응형 디자인** | 화면 크기에 따라 레이아웃이 조절되는 디자인 |
| **시맨틱 태그** | 의미가 있는 HTML 태그 (header, main, footer 등) |
| **CDN** | 콘텐츠 전송 네트워크, 외부 라이브러리를 빠르게 로드 |
| **호버 효과** | 마우스를 올렸을 때 나타나는 시각적 효과 |
| **viewport** | 브라우저에서 보이는 영역 |

---

## 정리

이번 챕터에서 배운 것:
- [x] 웹 개발의 기초 개념 (HTML, CSS, JavaScript)
- [x] 프로젝트 구조 만들기
- [x] Claude와 함께 단계별 개발
- [x] 피드백으로 디자인 개선하기
- [x] 반응형 웹사이트 만들기

> 💡 **핵심 포인트**: 완벽한 첫 결과를 기대하지 마세요. 피드백을 주고, 수정하고, 반복하는 것이 진짜 개발 과정입니다. Claude와 함께라면 이 과정이 훨씬 빨라집니다.

---

## 다음 챕터 미리보기

Chapter 14에서는 **배포하기**를 배웁니다.

지금 만든 웹사이트는 내 컴퓨터에만 있습니다. 다른 사람들이 볼 수 없죠. 다음 챕터에서는:

- Vercel로 무료 배포하기
- 나만의 URL 갖기 (yourname.vercel.app)
- 자동 배포 설정하기
- 커스텀 도메인 연결하기

드디어 내 웹사이트를 전 세계에 공개할 시간입니다!

[Chapter 14: 배포하기](../Chapter14-Deployment/README.ko.md)로 넘어가세요.

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [MDN Web Docs](https://developer.mozilla.org/ko/) - 웹 개발 완전 가이드 (한글)
- [Tailwind CSS 공식 문서](https://tailwindcss.com/docs) - Tailwind 완전 가이드
- [HTML Living Standard](https://html.spec.whatwg.org/) - HTML 표준 문서

**영상 자료:**
- [HTML/CSS 입문 (YouTube)](https://www.youtube.com/results?search_query=html+css+tutorial+korean) - HTML/CSS 기초 강의
- [Tailwind CSS 튜토리얼 (YouTube)](https://www.youtube.com/results?search_query=tailwind+css+tutorial+korean) - Tailwind 활용법
- [반응형 웹 디자인 (YouTube)](https://www.youtube.com/results?search_query=responsive+web+design+tutorial) - 반응형 디자인 기법

**읽을거리:**
- [웹 접근성 가이드](https://www.w3.org/WAI/fundamentals/accessibility-intro/ko) - 웹 접근성 기초
- [CSS Tricks](https://css-tricks.com/) - CSS 팁과 기법

**관련 도구:**
- [Can I Use](https://caniuse.com/) - 브라우저 호환성 체크
- [Coolors](https://coolors.co/) - 색상 팔레트 생성
- [Google Fonts](https://fonts.google.com/) - 무료 웹 폰트
- [Font Awesome](https://fontawesome.com/) - 아이콘 라이브러리

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
