# Chapter 17: API 연동

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- API가 무엇인지
- 외부 API 사용하기
- 실전 API 프로젝트

---

## API란?

API는 프로그램끼리 대화하는 방법입니다.

레스토랑에 비유하면:
- **당신** = 클라이언트 (주문하는 사람)
- **웨이터** = API (요청을 전달하는 사람)
- **주방** = 서버 (요청을 처리하는 곳)

### 예시

날씨 앱이 날씨 데이터를 보여주는 과정:
1. 앱이 날씨 API에 "서울 날씨 알려줘" 요청
2. API가 날씨 서버에 전달
3. 서버가 데이터 응답
4. 앱이 화면에 표시

---

## API 호출하기

### fetch 사용법

```javascript
// 기본 GET 요청
const response = await fetch('https://api.example.com/data')
const data = await response.json()
console.log(data)
```

### POST 요청 (데이터 보내기)

```javascript
const response = await fetch('https://api.example.com/users', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    name: '홍길동',
    email: 'hong@email.com'
  })
})
```

---

## 무료 공개 API 모음

연습하기 좋은 무료 API들입니다:

| API | 설명 | 인증 필요 |
|-----|------|-----------|
| JSONPlaceholder | 테스트용 가짜 데이터 | X |
| OpenWeatherMap | 날씨 정보 | O (무료 키) |
| The Cat API | 고양이 사진 | X |
| Pokemon API | 포켓몬 데이터 | X |
| 공공데이터포털 | 한국 공공 데이터 | O (무료) |

---

## 프로젝트 1: 날씨 앱

### 준비

1. [OpenWeatherMap](https://openweathermap.org/api) 가입
2. 무료 API 키 발급

### 만들기

```
> 날씨 앱 만들어줘.
> - 도시 이름 입력
> - OpenWeatherMap API로 날씨 조회
> - 온도, 날씨 상태, 아이콘 표시
```

### 핵심 코드

```javascript
async function getWeather(city) {
  const API_KEY = 'your-api-key'
  const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}&units=metric&lang=kr`

  const response = await fetch(url)
  const data = await response.json()

  return {
    temp: data.main.temp,
    description: data.weather[0].description,
    icon: data.weather[0].icon
  }
}
```

### 개선하기

```
> 현재 위치 기반으로 날씨 보여줘

> 5일 예보도 보여줘

> 날씨에 따라 배경색 바꿔줘
```

---

## 프로젝트 2: 랜덤 명언 앱

### 만들기

```
> 명언 앱 만들어줘.
> - 페이지 로드하면 랜덤 명언 표시
> - "새 명언" 버튼 누르면 다른 명언
> - 명언 복사 기능
```

### API 사용

```javascript
async function getQuote() {
  const response = await fetch('https://api.quotable.io/random')
  const data = await response.json()

  return {
    content: data.content,
    author: data.author
  }
}
```

---

## 프로젝트 3: 포켓몬 도감

### 만들기

```
> 포켓몬 도감 만들어줘.
> - 포켓몬 번호나 이름으로 검색
> - PokeAPI 사용
> - 이미지, 타입, 능력치 표시
```

### API 사용

```javascript
async function getPokemon(nameOrId) {
  const response = await fetch(
    `https://pokeapi.co/api/v2/pokemon/${nameOrId}`
  )
  const data = await response.json()

  return {
    name: data.name,
    image: data.sprites.front_default,
    types: data.types.map(t => t.type.name),
    stats: data.stats
  }
}
```

### 개선하기

```
> 한글 이름으로도 검색되게 해줘

> 진화 정보도 보여줘

> 즐겨찾기 기능 추가해줘
```

---

## 프로젝트 4: 환율 계산기

### 만들기

```
> 환율 계산기 만들어줘.
> - 금액 입력
> - 원화 ↔ 달러/엔/유로 변환
> - 실시간 환율 사용
```

### API 사용

```javascript
async function getExchangeRate(from, to) {
  const response = await fetch(
    `https://api.exchangerate-api.com/v4/latest/${from}`
  )
  const data = await response.json()

  return data.rates[to]
}

// 사용
const rate = await getExchangeRate('USD', 'KRW')
console.log(`1 달러 = ${rate} 원`)
```

---

## API 에러 처리

### 기본 에러 처리

```javascript
async function fetchData(url) {
  try {
    const response = await fetch(url)

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }

    return await response.json()
  } catch (error) {
    console.error('API 에러:', error)
    return null
  }
}
```

### 사용자에게 알리기

```
> API 에러나면 사용자에게 친절하게 알려줘.
> - 네트워크 에러: "인터넷 연결을 확인해주세요"
> - 404: "데이터를 찾을 수 없습니다"
> - 500: "서버에 문제가 생겼습니다"
```

---

## 로딩 상태 관리

### 로딩 표시

```javascript
async function fetchWithLoading() {
  showLoading()  // 로딩 시작

  try {
    const data = await fetchData(url)
    displayData(data)
  } catch (error) {
    showError(error)
  } finally {
    hideLoading()  // 로딩 끝
  }
}
```

### 스켈레톤 로딩

```
> 데이터 로딩 중에 스켈레톤 UI 보여줘
```

---

## API 키 보안

### 프론트엔드에서는 조심

```
// 나쁜 예 - API 키가 노출됩니다!
const API_KEY = 'sk-1234567890'
fetch(`https://api.example.com?key=${API_KEY}`)
```

### 해결 방법

1. **백엔드 서버 경유**
```
> API 호출을 대신해주는 백엔드 만들어줘.
> 프론트에서는 백엔드만 호출하게.
```

2. **서버리스 함수 사용**
```
> Vercel Edge Function으로 API 프록시 만들어줘
```

---

## 캐싱으로 최적화

### 같은 요청 반복 방지

```javascript
const cache = new Map()

async function fetchWithCache(url) {
  if (cache.has(url)) {
    return cache.get(url)
  }

  const data = await fetchData(url)
  cache.set(url, data)

  return data
}
```

### localStorage 활용

```javascript
async function fetchWithLocalStorage(key, url) {
  const cached = localStorage.getItem(key)
  if (cached) {
    return JSON.parse(cached)
  }

  const data = await fetchData(url)
  localStorage.setItem(key, JSON.stringify(data))

  return data
}
```

---

## 실습: API 앱 만들기

### 기본 과제

위 프로젝트 중 하나를 완성하세요.

### 추가 도전

```
> GitHub API로 내 프로필 보여주는 앱 만들어줘

> 영화 검색 앱 만들어줘 (TMDB API)

> 뉴스 헤드라인 앱 만들어줘 (News API)
```

---

## 미니 프로젝트: API 매시업

여러 API를 조합해서 멋진 앱을 만들어보세요!

### 목표

- 여러 API 통합
- 복합적인 데이터 활용

### 만들기: 여행 도우미

```
> 여행 도우미 앱 만들어줘.
> - 도시 검색
> - 날씨 API로 현재 날씨
> - 환율 API로 현지 화폐
> - 지도 API로 위치 표시
```

### 다른 아이디어

```
> 개발자 대시보드 만들어줘.
> - GitHub API로 내 저장소
> - Stack Overflow API로 인기 질문
> - Hacker News API로 최신 뉴스

> 건강 트래커 만들어줘.
> - 날씨 API로 운동하기 좋은 날
> - 레시피 API로 건강 식단
> - 운동 API로 추천 운동
```

### 심화 과제 (숙련자용)

```
> WebSocket으로 실시간 데이터 스트리밍 구현해줘

> Rate limiting 처리 로직 추가해줘

> API 응답 캐싱 레이어 만들어줘

> 여러 API 요청을 병렬로 처리해줘 (Promise.all)
```

---

## 정리

이번 챕터에서 배운 것:
- [x] API 개념 이해
- [x] fetch로 API 호출
- [x] 다양한 API 프로젝트
- [x] 에러 처리
- [x] 보안과 최적화

축하합니다! Part 4 (실전 프로젝트 II)를 완료했습니다.

다음 Part에서는 Claude Code의 고급 기능을 배웁니다.

[Chapter 18: 고급 기능](../Chapter18/README.ko.md)으로 넘어가세요.
