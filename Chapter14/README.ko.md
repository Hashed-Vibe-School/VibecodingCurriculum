# Chapter 14: 미니 게임

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- Claude로 게임 만들기
- JavaScript 게임 로직
- 재미있는 프로젝트 완성하기

---

## 왜 게임인가요?

게임 만들기는 프로그래밍을 배우는 가장 재미있는 방법입니다.

게임에는 모든 게 들어있습니다:
- 화면에 그리기 (HTML/CSS)
- 사용자 입력 받기 (키보드, 마우스)
- 로직 처리 (JavaScript)
- 상태 관리 (점수, 레벨)

**게임 요청 팁:**

```
> 숫자 맞추기 게임 만들어줘. 1~100 범위로.
> 시도 횟수도 보여주고, 10번 안에 맞추면 축하 메시지 띄워줘.
```

게임 규칙과 원하는 기능을 구체적으로 설명하면 더 완성도 높은 게임이 나옵니다.

---

## 게임 1: 숫자 맞추기

가장 간단한 게임부터 시작합니다.

### 게임 설명

- 컴퓨터가 1~100 사이 숫자를 정함
- 플레이어가 맞출 때까지 추측
- "더 크게" / "더 작게" 힌트 제공

### 만들기

```
> 숫자 맞추기 게임 만들어줘.
> 1부터 100 사이 숫자를 맞추는 게임이야.
> 힌트도 보여줘.
```

### 결과 예시

```html
<div id="game">
    <h1>숫자 맞추기</h1>
    <p>1부터 100 사이의 숫자를 맞춰보세요!</p>

    <input type="number" id="guess" placeholder="숫자 입력">
    <button onclick="checkGuess()">확인</button>

    <p id="result"></p>
    <p>시도 횟수: <span id="attempts">0</span></p>
</div>
```

```javascript
let answer = Math.floor(Math.random() * 100) + 1
let attempts = 0

function checkGuess() {
    const guess = parseInt(document.getElementById('guess').value)
    attempts++
    document.getElementById('attempts').textContent = attempts

    if (guess === answer) {
        document.getElementById('result').textContent =
            `정답입니다! ${attempts}번 만에 맞췄습니다!`
    } else if (guess < answer) {
        document.getElementById('result').textContent = '더 크게!'
    } else {
        document.getElementById('result').textContent = '더 작게!'
    }
}
```

### 개선하기

```
> 다시 시작 버튼 추가해줘
```
상태 초기화 패턴 - 게임 루프의 기본

```
> 최고 기록 저장해줘
```
`localStorage` 기반 데이터 영속성

```
> 예쁘게 스타일 입혀줘
```
게임 UI/UX 디자인 기초

---

## 게임 2: 가위바위보

클래식 게임을 만들어봅니다.

### 만들기

```
> 가위바위보 게임 만들어줘.
> 컴퓨터랑 대결하는 게임이야.
> 점수도 표시해줘.
```

### 핵심 로직

```javascript
function play(playerChoice) {
    const choices = ['가위', '바위', '보']
    const computer = choices[Math.floor(Math.random() * 3)]

    if (playerChoice === computer) {
        return '비겼습니다!'
    }

    if (
        (playerChoice === '가위' && computer === '보') ||
        (playerChoice === '바위' && computer === '가위') ||
        (playerChoice === '보' && computer === '바위')
    ) {
        return '이겼습니다!'
    }

    return '졌습니다...'
}
```

### 개선하기

```
> 이모지로 가위바위보 표시해줘
```
유니코드/이모지 렌더링 처리

```
> 5전 3선승제로 바꿔줘
```
라운드 기반 게임 로직 설계

```
> 승률 통계 보여줘
```
게임 통계 및 데이터 시각화

---

## 게임 3: 타이핑 게임

키보드 연습 게임입니다.

### 만들기

```
> 타이핑 게임 만들어줘.
> 단어가 나오면 빨리 타이핑하는 게임.
> 시간 제한 30초.
> 점수도 표시해.
```

### 핵심 로직

```javascript
const words = ['apple', 'banana', 'cherry', 'dog', 'elephant']
let score = 0
let timeLeft = 30

function startGame() {
    showRandomWord()
    startTimer()
}

function showRandomWord() {
    const word = words[Math.floor(Math.random() * words.length)]
    document.getElementById('word').textContent = word
}

function checkInput() {
    const input = document.getElementById('input').value
    const word = document.getElementById('word').textContent

    if (input === word) {
        score++
        document.getElementById('score').textContent = score
        document.getElementById('input').value = ''
        showRandomWord()
    }
}
```

### 개선하기

```
> 한글 단어로 바꿔줘
```
다국어 콘텐츠 관리

```
> 난이도별로 단어 길이 다르게 해줘
```
난이도 곡선(difficulty curve) 설계

```
> 타이핑 속도(WPM) 계산해줘
```
시간 기반 성과 지표 계산

---

## 게임 4: 반응 속도 테스트

반응 속도를 측정하는 게임입니다.

### 만들기

```
> 반응 속도 테스트 게임 만들어줘.
> 화면이 초록색으로 바뀌면 클릭.
> 반응 시간을 밀리초로 보여줘.
```

### 핵심 로직

```javascript
let startTime
let waiting = false

function startTest() {
    const box = document.getElementById('box')
    box.style.backgroundColor = 'red'
    box.textContent = '기다리세요...'

    // 랜덤 시간 후 초록색으로
    const delay = Math.random() * 3000 + 1000
    setTimeout(() => {
        box.style.backgroundColor = 'green'
        box.textContent = '클릭!'
        startTime = Date.now()
        waiting = true
    }, delay)
}

function handleClick() {
    if (!waiting) return

    const reactionTime = Date.now() - startTime
    document.getElementById('result').textContent =
        `반응 속도: ${reactionTime}ms`
    waiting = false
}
```

### 개선하기

```
> 평균 반응 속도 계산해줘 (5회 테스트)
```
통계 집계 및 평균 계산

```
> 너무 빨리 클릭하면 "속임수!" 표시해줘
```
입력 검증과 예외 처리

```
> 순위표 기능 추가해줘
```
데이터 정렬과 리더보드 UI

---

## 게임 5: 메모리 카드 게임

카드 짝 맞추기 게임입니다.

### 만들기

```
> 메모리 카드 게임 만들어줘.
> 8쌍(16장)의 카드.
> 같은 그림 찾으면 사라지게.
> 시도 횟수 표시.
```

### 핵심 개념

```javascript
const emojis = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
let cards = [...emojis, ...emojis]  // 2쌍씩

function shuffle(array) {
    for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]]
    }
    return array
}

function createBoard() {
    const shuffled = shuffle(cards)
    // 카드 UI 생성...
}
```

### 개선하기

```
> 카드 뒤집는 애니메이션 추가해줘
```
CSS transform 기반 3D 애니메이션

```
> 타이머 추가해줘
```
`setInterval` 기반 타이머 구현

```
> 다 맞추면 축하 메시지 보여줘
```
게임 완료 조건 감지와 모달 UI

---

## 게임 만들기 팁

### 1. 작게 시작하기

```
# 나쁜 예
> RPG 게임 만들어줘. 캐릭터 성장, 던전, 보스전 있는.

# 좋은 예
> 간단한 클릭 게임 먼저 만들어줘.
> 클릭하면 점수 올라가는.
```

### 2. 한 기능씩 추가

```
# 1단계: 기본
> 점프하는 캐릭터 만들어줘

# 2단계: 장애물
> 장애물 추가해줘

# 3단계: 충돌
> 장애물이랑 부딪히면 게임 오버

# 4단계: 점수
> 넘은 장애물 개수 = 점수
```

### 3. 피드백으로 개선

```
> 점프가 너무 느려. 더 빠르게.

> 장애물 간격이 너무 좁아.

> 배경 색이 눈 아파. 바꿔줘.
```

---

## 실습: 나만의 게임

### 기본 과제

위 5개 게임 중 하나를 골라 만들어보세요.

### 추가 도전

```
> 게임에 효과음 추가해줘
```
Web Audio API로 사운드 피드백 구현

```
> 최고 점수 저장해줘 (localStorage)
```
브라우저 스토리지 기반 데이터 영속성

```
> 모바일에서도 작동하게 해줘
```
터치 이벤트와 반응형 레이아웃

### 아이디어

다른 게임도 만들어보세요:

- 두더지 잡기
- 스네이크 게임
- 퐁 (테니스)
- 틱택토
- 퀴즈 게임

```
> [게임 이름] 게임 만들어줘
```

---

## 게임을 웹에 공개하기

만든 게임을 배포해서 친구들과 공유하세요.

```
# GitHub에 올리기
> 이 게임 GitHub에 올려줘

# Vercel로 배포
# (Chapter 12 참고)
```

배포 후 링크를 친구들에게 공유하세요.

---

## 정리

이번 챕터에서 배운 것:
- [x] 간단한 게임 로직
- [x] 사용자 입력 처리
- [x] 점수와 상태 관리
- [x] 게임 개선하기

축하합니다. Part 3 (실전 프로젝트 I)을 완료했습니다.

다음 Part에서는 AI 도구와 API 연동 같은 더 실용적인 프로젝트를 만들어봅니다.

[Chapter 15: AI 도구 만들기](../Chapter15/README.ko.md)로 넘어가세요.
