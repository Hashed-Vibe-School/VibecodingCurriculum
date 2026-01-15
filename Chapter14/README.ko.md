# Chapter 14: 미니 게임

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- Claude로 게임 만들기
- JavaScript 게임 로직
- 재미있는 프로젝트 완성하기

---

## 왜 필요합니까?

게임은 단순히 재미만 있는 것이 아닙니다. 게임 만들기는 프로그래밍 개념을 가장 흥미롭게 배우는 방법입니다.

**게임 제작 스킬이 도움되는 실제 상황:**

- **프로그래밍 학습**: 변수, 반복문, 조건문, 함수 - 게임은 이 모든 걸 사용합니다
- **포트폴리오 임팩트**: 플레이 가능한 게임은 정적 페이지보다 훨씬 인상적입니다
- **사용자 참여**: 인터랙티브한 요소는 방문자를 더 오래 머물게 합니다
- **문제 해결**: 게임 로직은 코딩 두뇌를 날카롭게 합니다
- **면접**: "게임 만들어봤어요"는 훌륭한 대화 시작점입니다

> 모든 게임 메카닉은 변장한 프로그래밍 개념입니다. 점수 추적? 그건 상태 관리입니다. 충돌 감지? 그건 조건문입니다. 게임 루프? 그건 이벤트 처리입니다.

### 쉬운 비유: 게임은 코더를 위한 헬스장

유틸리티를 만들면서 코딩을 배우는 건 집안일 하면서 운동하는 것과 같습니다 - 효과는 있지만 지루합니다.

게임 만들기는 헬스장 가는 것과 같습니다 - 여전히 운동(학습)하지만, 실제로 즐겁습니다. 그리고 헬스장처럼, 재미있게 하면서 더 강해집니다 (코딩 실력 향상).

---

## 따라해보세요: 가장 간단한 게임

복잡한 게임을 만들기 전에, 기본이 작동하는지 확인합시다. 가장 간단한 게임입니다:

```
> 카운터를 보여주는 버튼 만들어줘.
> 클릭할 때마다 카운터가 1씩 올라가게.
> 그게 전부야.
```

몇 번 클릭해 보시기 바랍니다. 방금 "클리커 게임"을 만들었습니다. 수백만 플레이어가 있는 쿠키 클리커와 같은 장르입니다. 나머지는 전부 이 기초에 기능을 추가하는 것입니다.

---

## 왜 게임입니까?

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

## 안 되면?

게임 개발은 까다로울 수 있습니다. 자주 발생하는 문제와 해결법입니다.

### 클릭해도 아무 반응이 없습니다
- 클릭 핸들러가 연결되어 있는지 확인하시기 바랍니다
- 브라우저 콘솔에서 에러를 확인하시기 바랍니다 (F12)
```
> 버튼이 클릭에 반응 안 해. 이벤트 리스너 확인해줘.
```

### 게임이 너무 빠르거나 느립니다
타이밍 문제는 게임에서 흔합니다:
```
> 게임이 너무 빨라. 느리게 해줘.
```
또는
```
> 타이머가 제대로 안 동작해. setInterval 사용법 확인해줘.
```

### 점수가 화면에 업데이트되지 않습니다
변수는 바뀌는데 화면은 안 바뀔 수 있습니다:
```
> 점수는 올라가는데 화면에 새 값이 안 보여.
> 화면 업데이트 로직 확인해줘.
```

### 게임 상태가 엉망이 됩니다
여러 번 클릭하면 경쟁 상태가 발생할 수 있습니다:
```
> 클릭하면 안 될 때 여러 번 클릭할 수 있어.
> 이거 막는 체크 추가해줘.
```

### 애니메이션이 끊깁니다
```
> 애니메이션이 부드럽지 않아. 최적화 도와줘.
```

### 데스크톱에서는 되는데 모바일에서 안 됩니다
터치 이벤트는 클릭 이벤트와 다릅니다:
```
> 이 게임 모바일에서도 터치 입력으로 작동하게 해줘.
```

---

## 자주 하는 실수

이런 게임 개발 함정을 피하시기 바랍니다.

### 실수 1: 너무 크게 시작하기
**나쁜 접근:**
```
> 100명 플레이어가 있는 멀티플레이어 배틀로얄 게임 만들어줘.
```

**좋은 접근:**
```
> 클릭하면 점수가 올라가는 간단한 게임 만들어줘.
```
아주 작게 시작하고, 기능을 하나씩 추가하시기 바랍니다.

### 실수 2: 게임 상태 초기화 잊기
게임 오버 후 "다시 하기" 누르면 모든 게 초기화되어야 합니다:
```javascript
// 초기화 깜빡
function playAgain() {
    showGame()  // 이런, 점수가 지난 게임 것 그대로!
}

// 올바름
function playAgain() {
    score = 0
    timeLeft = 30
    updateDisplay()
    showGame()
}
```

### 실수 3: setInterval 정리 안 하고 쓰기
여러 타이머가 동시에 돌아가게 됩니다:
```javascript
// 잘못됨 - 클릭할 때마다 새 타이머!
function startGame() {
    setInterval(tick, 1000)
}

// 올바름 - 기존 타이머 먼저 정리
let timerId
function startGame() {
    if (timerId) clearInterval(timerId)
    timerId = setInterval(tick, 1000)
}
```

### 실수 4: 예외 상황 처리 안 하기
이런 경우 어떻게 되나요:
- 게임 시작 전에 클릭?
- 게임 끝난 후에 클릭?
- 게임 중간에 새로고침?

```
> 게임이 활성화되지 않았을 때 클릭 못하게 체크 추가해줘.
```

### 실수 5: 모든 것 하드코딩
난이도 조절이 어려워집니다:
```javascript
// 조절 어려움
if (score > 100) levelUp()

// 더 나음 - 변수 사용
const LEVEL_UP_THRESHOLD = 100
if (score > LEVEL_UP_THRESHOLD) levelUp()
```

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
