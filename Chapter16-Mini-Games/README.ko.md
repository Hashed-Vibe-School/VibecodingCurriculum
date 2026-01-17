# Chapter 16: 미니 게임

[English](./README.md) | **한국어**

---

## 💬 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이 챕터에서 배우는 것

- Claude로 게임 만들기
- JavaScript 게임 로직의 핵심 개념
- 사용자 입력 처리와 상태 관리
- 재미있는 프로젝트 완성하기
- 게임 개발을 통한 프로그래밍 실력 향상

---

## 이전 챕터와의 연결

Chapter 15에서는 API를 연동하는 방법을 배웠습니다. 이제 그동안 배운 HTML, CSS, JavaScript 지식을 총동원해서 **직접 플레이할 수 있는 게임**을 만들어봅니다. 게임은 모든 프로그래밍 개념이 융합되는 최고의 학습 프로젝트입니다.

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

## 게임 개발의 핵심 개념

게임을 만들기 전에 알아야 할 핵심 개념들이 있습니다. 이것들을 이해하면 어떤 게임이든 만들 수 있습니다.

### 1. 게임 상태 (Game State)

게임의 "현재 상황"을 저장하는 변수들입니다.

```javascript
// 게임 상태 예시
let score = 0           // 현재 점수
let lives = 3           // 남은 목숨
let level = 1           // 현재 레벨
let isGameOver = false  // 게임 종료 여부
let isPaused = false    // 일시정지 여부
```

> 💡 **비전공자 팁**: 게임 상태를 "게임의 기억"이라고 생각하세요. 게임을 저장하고 불러올 때 필요한 모든 정보가 상태입니다.

### 2. 게임 루프 (Game Loop)

게임이 계속 실행되게 하는 반복 구조입니다.

```javascript
// 기본 게임 루프
function gameLoop() {
    if (isGameOver) return  // 게임 끝나면 중지

    update()    // 상태 업데이트 (캐릭터 이동, 충돌 검사 등)
    render()    // 화면 그리기

    requestAnimationFrame(gameLoop)  // 다음 프레임 요청
}
```

> 💡 **비전공자 팁**: 게임 루프는 영화와 비슷합니다. 영화가 1초에 24장의 사진을 보여주듯이, 게임도 1초에 60번 정도 화면을 다시 그립니다. 그래서 움직이는 것처럼 보이는 거예요.

### 3. 이벤트 처리 (Event Handling)

사용자의 입력(클릭, 키보드)에 반응하는 것입니다.

```javascript
// 키보드 입력 처리
document.addEventListener('keydown', (e) => {
    if (e.key === 'ArrowLeft') player.moveLeft()
    if (e.key === 'ArrowRight') player.moveRight()
    if (e.key === ' ') player.jump()  // 스페이스바
})

// 마우스 클릭 처리
canvas.addEventListener('click', (e) => {
    const x = e.clientX
    const y = e.clientY
    handleClick(x, y)
})
```

> 🔥 **프로 팁**: `keydown`은 키를 누르는 순간, `keyup`은 키를 떼는 순간 발생합니다. 캐릭터가 키를 누르고 있는 동안 계속 움직이게 하려면 별도의 상태 변수가 필요합니다.

### 4. 충돌 감지 (Collision Detection)

두 객체가 닿았는지 확인하는 것입니다.

```javascript
// 사각형 충돌 감지 (가장 간단한 방법)
function isColliding(rect1, rect2) {
    return rect1.x < rect2.x + rect2.width &&
           rect1.x + rect1.width > rect2.x &&
           rect1.y < rect2.y + rect2.height &&
           rect1.y + rect1.height > rect2.y
}

// 사용 예시
if (isColliding(player, enemy)) {
    player.takeDamage()
}
```

> ⚠️ **주의사항**: 원형 객체의 충돌 감지는 다른 공식을 사용합니다. 두 원의 중심 거리가 반지름의 합보다 작으면 충돌입니다.

---

## 따라해보세요: 가장 간단한 게임

복잡한 게임을 만들기 전에, 기본이 작동하는지 확인합시다. 가장 간단한 게임입니다:

```
> 카운터를 보여주는 버튼 만들어줘.
> 클릭할 때마다 카운터가 1씩 올라가게.
> 그게 전부야.
```

몇 번 클릭해 보시기 바랍니다. 방금 "클리커 게임"을 만들었습니다. 수백만 플레이어가 있는 쿠키 클리커와 같은 장르입니다. 나머지는 전부 이 기초에 기능을 추가하는 것입니다.

### 클리커 게임 확장 예시

```
> 클릭하면 점수가 올라가는 게임 만들어줘.
> - 점수가 10점마다 레벨업
> - 레벨이 높을수록 한 번 클릭에 더 많은 점수
> - 예쁜 애니메이션과 효과음
> - 최고 점수 저장
```

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

> 💡 **비전공자 팁**: 처음에는 간단한 게임부터 시작하세요. "숫자 맞추기" → "가위바위보" → "타이핑 게임" 순서로 난이도를 높여가면 좋습니다.

---

## 게임 1: 숫자 맞추기

가장 간단한 게임부터 시작합니다.

### 게임 설명

- 컴퓨터가 1~100 사이 숫자를 정함
- 플레이어가 맞출 때까지 추측
- "더 크게" / "더 작게" 힌트 제공

### 이 게임에서 배우는 것

| 개념 | 설명 | 게임에서의 적용 |
|------|------|----------------|
| 변수 | 데이터를 저장하는 공간 | 정답, 시도 횟수 |
| 조건문 | 상황에 따라 다른 동작 | 정답 비교 |
| 함수 | 재사용 가능한 코드 블록 | 추측 확인 로직 |
| 이벤트 | 사용자 동작에 반응 | 버튼 클릭 |
| DOM 조작 | 화면 내용 변경 | 결과 표시 |

### 만들기

```
> 숫자 맞추기 게임 만들어줘.
> 1부터 100 사이 숫자를 맞추는 게임이야.
> 힌트도 보여줘.
```

### 결과 예시

```html
<!-- HTML 구조 -->
<div id="game">
    <h1>숫자 맞추기</h1>
    <p>1부터 100 사이의 숫자를 맞춰보세요!</p>

    <input type="number" id="guess" placeholder="숫자 입력" min="1" max="100">
    <button onclick="checkGuess()">확인</button>

    <p id="result"></p>
    <p>시도 횟수: <span id="attempts">0</span></p>

    <!-- 힌트 영역 -->
    <div id="hint-area">
        <p>범위: <span id="range">1 ~ 100</span></p>
    </div>
</div>
```

```javascript
// 게임 상태 변수들
let answer = Math.floor(Math.random() * 100) + 1  // 1~100 사이 랜덤 숫자
let attempts = 0  // 시도 횟수
let minRange = 1  // 최소 범위 (힌트용)
let maxRange = 100  // 최대 범위 (힌트용)

function checkGuess() {
    // 입력값 가져오기
    const guessInput = document.getElementById('guess')
    const guess = parseInt(guessInput.value)

    // 유효성 검사
    if (isNaN(guess) || guess < 1 || guess > 100) {
        document.getElementById('result').textContent = '1부터 100 사이 숫자를 입력하세요!'
        document.getElementById('result').style.color = 'orange'
        return
    }

    // 시도 횟수 증가
    attempts++
    document.getElementById('attempts').textContent = attempts

    const resultEl = document.getElementById('result')

    if (guess === answer) {
        // 정답!
        resultEl.textContent = `🎉 정답입니다! ${attempts}번 만에 맞췄습니다!`
        resultEl.style.color = 'green'

        // 축하 효과
        if (attempts <= 5) {
            resultEl.textContent += ' 천재시네요! 🏆'
        } else if (attempts <= 7) {
            resultEl.textContent += ' 훌륭합니다! ⭐'
        }

        // 다시 시작 버튼 보이기
        showRestartButton()

    } else if (guess < answer) {
        // 더 크게
        resultEl.textContent = '⬆️ 더 크게!'
        resultEl.style.color = '#3498db'
        minRange = Math.max(minRange, guess + 1)
        updateRange()

    } else {
        // 더 작게
        resultEl.textContent = '⬇️ 더 작게!'
        resultEl.style.color = '#e74c3c'
        maxRange = Math.min(maxRange, guess - 1)
        updateRange()
    }

    // 입력 필드 초기화 및 포커스
    guessInput.value = ''
    guessInput.focus()
}

function updateRange() {
    document.getElementById('range').textContent = `${minRange} ~ ${maxRange}`
}

function showRestartButton() {
    const btn = document.createElement('button')
    btn.textContent = '다시 하기'
    btn.onclick = restartGame
    document.getElementById('game').appendChild(btn)
}

function restartGame() {
    // 모든 상태 초기화
    answer = Math.floor(Math.random() * 100) + 1
    attempts = 0
    minRange = 1
    maxRange = 100

    // 화면 업데이트
    document.getElementById('attempts').textContent = '0'
    document.getElementById('result').textContent = ''
    document.getElementById('range').textContent = '1 ~ 100'
    document.getElementById('guess').value = ''
    document.getElementById('guess').focus()

    // 다시 하기 버튼 제거
    const restartBtn = document.querySelector('button:last-child')
    if (restartBtn.textContent === '다시 하기') {
        restartBtn.remove()
    }
}

// 엔터 키로도 확인 가능하게
document.getElementById('guess').addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
        checkGuess()
    }
})
```

> 💡 **비전공자 팁**: `Math.floor(Math.random() * 100) + 1`이 복잡해 보이죠? 분해해봅시다:
> - `Math.random()` → 0~0.999... 사이 소수
> - `* 100` → 0~99.999... 사이 숫자
> - `Math.floor()` → 소수점 버림 (0~99)
> - `+ 1` → 1~100으로 조정

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

```
> 난이도 선택 기능 추가해줘 (쉬움: 1-50, 보통: 1-100, 어려움: 1-500)
```
설정 기반 게임 로직

### 🎯 미니 퀴즈

1. `Math.random()`은 어떤 범위의 숫자를 반환하나요?
2. 왜 `parseInt()`를 사용하나요?
3. `attempts++`는 무엇을 의미하나요?

<details>
<summary>정답 보기</summary>

1. 0 이상 1 미만의 소수 (예: 0.7342...)
2. 입력값이 문자열이므로 숫자로 변환하기 위해
3. `attempts = attempts + 1`과 같은 의미, 시도 횟수를 1 증가

</details>

---

## 게임 2: 가위바위보

클래식 게임을 만들어봅니다.

### 이 게임에서 배우는 것

| 개념 | 설명 | 게임에서의 적용 |
|------|------|----------------|
| 배열 | 여러 값을 순서대로 저장 | 선택지 목록 |
| 랜덤 선택 | 배열에서 무작위로 고르기 | 컴퓨터 선택 |
| 복잡한 조건문 | 여러 조건 조합 | 승패 판정 |
| 통계 계산 | 데이터 집계 | 승률 표시 |

### 만들기

```
> 가위바위보 게임 만들어줘.
> 컴퓨터랑 대결하는 게임이야.
> 점수도 표시해줘.
> 이모지로 가위바위보를 표시해줘.
```

### 핵심 로직 (상세 주석 포함)

```javascript
// 게임 상태
let playerWins = 0
let computerWins = 0
let draws = 0
let history = []  // 게임 기록

// 선택지 정의 (이모지와 함께)
const CHOICES = {
    rock: { name: '바위', emoji: '✊', beats: 'scissors' },
    scissors: { name: '가위', emoji: '✌️', beats: 'paper' },
    paper: { name: '보', emoji: '✋', beats: 'rock' }
}

function play(playerChoice) {
    // 컴퓨터의 랜덤 선택
    const choices = Object.keys(CHOICES)  // ['rock', 'scissors', 'paper']
    const randomIndex = Math.floor(Math.random() * choices.length)
    const computerChoice = choices[randomIndex]

    // 결과 판정
    let result
    if (playerChoice === computerChoice) {
        result = 'draw'
        draws++
    } else if (CHOICES[playerChoice].beats === computerChoice) {
        result = 'win'
        playerWins++
    } else {
        result = 'lose'
        computerWins++
    }

    // 기록 저장
    history.push({
        player: playerChoice,
        computer: computerChoice,
        result: result,
        timestamp: new Date()
    })

    // 화면 업데이트
    updateDisplay(playerChoice, computerChoice, result)
    updateStats()

    return result
}

function updateDisplay(playerChoice, computerChoice, result) {
    const player = CHOICES[playerChoice]
    const computer = CHOICES[computerChoice]

    // 선택 표시
    document.getElementById('player-choice').textContent = player.emoji
    document.getElementById('computer-choice').textContent = computer.emoji

    // 결과 메시지
    const resultEl = document.getElementById('result')
    const messages = {
        win: '🎉 이겼습니다!',
        lose: '😢 졌습니다...',
        draw: '🤝 비겼습니다!'
    }
    resultEl.textContent = messages[result]
    resultEl.className = `result-${result}`  // CSS 스타일링용
}

function updateStats() {
    const total = playerWins + computerWins + draws
    const winRate = total > 0 ? ((playerWins / total) * 100).toFixed(1) : 0

    document.getElementById('player-wins').textContent = playerWins
    document.getElementById('computer-wins').textContent = computerWins
    document.getElementById('draws').textContent = draws
    document.getElementById('win-rate').textContent = `${winRate}%`
}

// 통계 분석 함수
function analyzeHistory() {
    if (history.length === 0) return null

    // 가장 많이 선택한 것
    const playerChoices = history.map(h => h.player)
    const mostUsed = getMostFrequent(playerChoices)

    // 최근 5게임 승률
    const recent = history.slice(-5)
    const recentWins = recent.filter(h => h.result === 'win').length

    return {
        totalGames: history.length,
        mostUsedChoice: CHOICES[mostUsed].name,
        recentWinRate: (recentWins / recent.length * 100).toFixed(0)
    }
}

function getMostFrequent(arr) {
    const counts = {}
    arr.forEach(item => counts[item] = (counts[item] || 0) + 1)
    return Object.entries(counts).sort((a, b) => b[1] - a[1])[0][0]
}
```

> 🔥 **프로 팁**: 객체(`CHOICES`)를 사용하면 조건문을 줄일 수 있습니다. 각 선택지가 무엇을 이기는지 데이터로 정의해두면 코드가 훨씬 깔끔해집니다.

### 개선하기

```
> 5전 3선승제로 바꿔줘
```
라운드 기반 게임 로직 설계

```
> 승률 통계 보여줘
```
게임 통계 및 데이터 시각화

```
> 애니메이션 추가해줘 (선택 후 잠깐 로딩)
```
타이밍과 서스펜스 효과

```
> 컴퓨터가 조금 더 똑똑하게 (플레이어 패턴 분석)
```
간단한 AI 로직

### 💡 비전공자 팁: 왜 객체를 사용하나요?

```javascript
// 이렇게 하면 조건문이 너무 많아집니다
if (player === '가위' && computer === '보') return 'win'
if (player === '가위' && computer === '바위') return 'lose'
if (player === '바위' && computer === '가위') return 'win'
// ... 계속 ...

// 객체를 사용하면 한 줄로 해결!
if (CHOICES[player].beats === computer) return 'win'
```

데이터 구조를 잘 설계하면 코드가 간단해집니다.

---

## 게임 3: 타이핑 게임

키보드 연습 게임입니다.

### 이 게임에서 배우는 것

| 개념 | 설명 | 게임에서의 적용 |
|------|------|----------------|
| 타이머 | 시간 기반 로직 | 제한 시간 |
| 문자열 비교 | 텍스트 일치 확인 | 타이핑 검증 |
| 실시간 피드백 | 즉각적인 반응 | 타이핑 확인 |
| 성능 측정 | 속도/정확도 계산 | WPM 측정 |

### 만들기

```
> 타이핑 게임 만들어줘.
> 단어가 나오면 빨리 타이핑하는 게임.
> 시간 제한 30초.
> 점수와 WPM도 표시해.
```

### 핵심 로직 (상세 주석 포함)

```javascript
// 한글 단어 목록 (난이도별)
const WORDS = {
    easy: ['사과', '바나나', '딸기', '포도', '수박', '참외', '배', '감'],
    medium: ['프로그래밍', '자바스크립트', '컴퓨터', '키보드', '모니터', '인터넷'],
    hard: ['알고리즘', '데이터베이스', '인공지능', '머신러닝', '블록체인', '메타버스']
}

// 게임 상태
let score = 0
let timeLeft = 30
let totalTyped = 0      // 총 타이핑한 글자 수
let correctTyped = 0    // 정확히 타이핑한 글자 수
let currentWord = ''
let timerId = null
let difficulty = 'easy'
let gameStartTime = null

function startGame() {
    // 이전 타이머가 있으면 정리
    if (timerId) clearInterval(timerId)

    // 상태 초기화
    score = 0
    timeLeft = 30
    totalTyped = 0
    correctTyped = 0
    gameStartTime = Date.now()

    // 화면 초기화
    updateDisplay()
    showRandomWord()

    // 입력 필드 활성화 및 포커스
    const input = document.getElementById('input')
    input.disabled = false
    input.value = ''
    input.focus()

    // 타이머 시작
    timerId = setInterval(() => {
        timeLeft--
        document.getElementById('timer').textContent = timeLeft

        // 시간이 10초 이하면 빨간색으로
        if (timeLeft <= 10) {
            document.getElementById('timer').classList.add('warning')
        }

        if (timeLeft <= 0) {
            endGame()
        }
    }, 1000)  // 1초마다 실행
}

function showRandomWord() {
    const wordList = WORDS[difficulty]
    const randomIndex = Math.floor(Math.random() * wordList.length)
    currentWord = wordList[randomIndex]

    document.getElementById('word').textContent = currentWord
    document.getElementById('word').classList.remove('correct', 'incorrect')
}

function checkInput(event) {
    const input = event.target.value
    totalTyped += 1  // 키 입력 횟수

    // 실시간 피드백: 현재까지 맞는지 확인
    const wordEl = document.getElementById('word')
    if (currentWord.startsWith(input)) {
        wordEl.classList.remove('incorrect')
        wordEl.classList.add('typing')
    } else {
        wordEl.classList.add('incorrect')
        wordEl.classList.remove('typing')
    }

    // 완전히 일치하면
    if (input === currentWord) {
        score++
        correctTyped += currentWord.length

        // 성공 효과
        wordEl.classList.add('correct')

        // 콤보 보너스
        if (score > 0 && score % 5 === 0) {
            timeLeft += 2  // 5개마다 2초 추가
            showBonus('+2초!')
        }

        updateDisplay()

        // 다음 단어
        setTimeout(() => {
            document.getElementById('input').value = ''
            showRandomWord()
        }, 100)
    }
}

function updateDisplay() {
    document.getElementById('score').textContent = score
    document.getElementById('timer').textContent = timeLeft
}

function endGame() {
    clearInterval(timerId)
    timerId = null

    // 입력 비활성화
    document.getElementById('input').disabled = true

    // WPM 계산 (Words Per Minute)
    const elapsedMinutes = (Date.now() - gameStartTime) / 60000
    const wpm = Math.round(score / elapsedMinutes)

    // 정확도 계산
    const accuracy = totalTyped > 0
        ? Math.round((correctTyped / totalTyped) * 100)
        : 0

    // 결과 표시
    showResults(wpm, accuracy)

    // 최고 기록 저장
    saveHighScore(score, wpm)
}

function showResults(wpm, accuracy) {
    const resultDiv = document.getElementById('results')
    resultDiv.innerHTML = `
        <h2>게임 종료!</h2>
        <p>점수: ${score}개</p>
        <p>타이핑 속도: ${wpm} WPM</p>
        <p>정확도: ${accuracy}%</p>
        <p>등급: ${getGrade(wpm)}</p>
        <button onclick="startGame()">다시 하기</button>
    `
    resultDiv.style.display = 'block'
}

function getGrade(wpm) {
    if (wpm >= 80) return '🏆 전문가'
    if (wpm >= 60) return '⭐ 고급'
    if (wpm >= 40) return '👍 중급'
    if (wpm >= 20) return '📝 초급'
    return '🌱 입문'
}

function saveHighScore(score, wpm) {
    const highScore = localStorage.getItem('typingHighScore') || 0
    if (score > highScore) {
        localStorage.setItem('typingHighScore', score)
        showBonus('🎉 새 기록!')
    }
}

function showBonus(text) {
    const bonus = document.createElement('div')
    bonus.className = 'bonus-popup'
    bonus.textContent = text
    document.body.appendChild(bonus)
    setTimeout(() => bonus.remove(), 1000)
}
```

> ⚠️ **주의사항**: `setInterval`을 사용할 때는 반드시 `clearInterval`로 정리해야 합니다. 안 그러면 게임을 여러 번 시작할 때 타이머가 중복 실행됩니다.

### 개선하기

```
> 난이도별로 단어 길이 다르게 해줘
```
난이도 곡선(difficulty curve) 설계

```
> 틀린 글자는 빨간색으로 표시해줘
```
실시간 피드백 UI

```
> 콤보 시스템 추가해줘 (연속 정답 시 보너스)
```
동기 부여 메카닉

---

## 게임 4: 반응 속도 테스트

반응 속도를 측정하는 게임입니다.

### 이 게임에서 배우는 것

| 개념 | 설명 | 게임에서의 적용 |
|------|------|----------------|
| setTimeout | 지연 실행 | 랜덤 대기 시간 |
| Date.now() | 현재 시각 밀리초 | 시간 측정 |
| 배열 메서드 | reduce, sort 등 | 평균/최고 기록 계산 |
| 상태 머신 | 게임 상태 전환 | 대기/준비/측정 |

### 만들기

```
> 반응 속도 테스트 게임 만들어줘.
> 화면이 초록색으로 바뀌면 클릭.
> 반응 시간을 밀리초로 보여줘.
> 5회 테스트 후 평균도 보여줘.
```

### 핵심 로직 (상세 주석 포함)

```javascript
// 게임 상태 정의
const STATE = {
    IDLE: 'idle',       // 시작 전
    WAITING: 'waiting', // 대기 중 (빨간색)
    READY: 'ready',     // 준비됨 (초록색)
    RESULT: 'result'    // 결과 표시 중
}

// 게임 변수
let state = STATE.IDLE
let startTime = null
let timeoutId = null
let results = []
const MAX_ROUNDS = 5

function startTest() {
    // 이전 타임아웃 정리
    if (timeoutId) clearTimeout(timeoutId)

    // 상태 전환: 대기 중
    state = STATE.WAITING
    const box = document.getElementById('box')
    box.style.backgroundColor = '#e74c3c'  // 빨간색
    box.textContent = '초록색이 되면 클릭하세요...'
    box.className = 'waiting'

    // 1~4초 사이 랜덤 시간 후 초록색으로
    const delay = Math.random() * 3000 + 1000
    timeoutId = setTimeout(() => {
        state = STATE.READY
        box.style.backgroundColor = '#2ecc71'  // 초록색
        box.textContent = '클릭!'
        box.className = 'ready'
        startTime = Date.now()  // 시간 측정 시작
    }, delay)
}

function handleClick() {
    const box = document.getElementById('box')

    switch (state) {
        case STATE.IDLE:
            // 첫 클릭: 게임 시작
            startTest()
            break

        case STATE.WAITING:
            // 너무 빨리 클릭함!
            clearTimeout(timeoutId)
            state = STATE.RESULT
            box.style.backgroundColor = '#f39c12'  // 주황색
            box.textContent = '🚫 너무 빨라요! 초록색이 될 때까지 기다리세요'
            box.className = 'too-early'

            // 2초 후 다시 시작 가능
            setTimeout(() => {
                state = STATE.IDLE
                box.textContent = '클릭하여 다시 시작'
                box.style.backgroundColor = '#3498db'
            }, 2000)
            break

        case STATE.READY:
            // 정상 클릭: 반응 시간 측정
            const reactionTime = Date.now() - startTime
            state = STATE.RESULT
            results.push(reactionTime)

            box.style.backgroundColor = '#9b59b6'  // 보라색
            box.textContent = `${reactionTime}ms`
            box.className = 'result'

            // 결과 분석
            updateResults(reactionTime)

            // 5회 완료 체크
            if (results.length >= MAX_ROUNDS) {
                showFinalResults()
            } else {
                // 1초 후 다음 라운드
                setTimeout(() => {
                    state = STATE.IDLE
                    box.textContent = `${results.length + 1}/${MAX_ROUNDS} 라운드 - 클릭하여 시작`
                    box.style.backgroundColor = '#3498db'
                }, 1000)
            }
            break

        case STATE.RESULT:
            // 결과 표시 중에는 무시
            break
    }
}

function updateResults(latestTime) {
    const resultsList = document.getElementById('results-list')

    // 결과 평가
    let rating = ''
    if (latestTime < 200) rating = '⚡ 번개!'
    else if (latestTime < 250) rating = '🔥 빠름!'
    else if (latestTime < 350) rating = '👍 평균'
    else rating = '🐢 느림'

    // 결과 추가
    const li = document.createElement('li')
    li.textContent = `${results.length}회: ${latestTime}ms ${rating}`
    li.className = getTimeClass(latestTime)
    resultsList.appendChild(li)
}

function getTimeClass(time) {
    if (time < 200) return 'excellent'
    if (time < 250) return 'good'
    if (time < 350) return 'average'
    return 'slow'
}

function showFinalResults() {
    const box = document.getElementById('box')

    // 통계 계산
    const sum = results.reduce((a, b) => a + b, 0)
    const average = Math.round(sum / results.length)
    const best = Math.min(...results)
    const worst = Math.max(...results)

    // 평균 기준 등급
    let grade = ''
    if (average < 200) grade = '🏆 프로게이머급!'
    else if (average < 250) grade = '⭐ 우수한 반응속도!'
    else if (average < 300) grade = '👍 평균 이상!'
    else if (average < 400) grade = '📝 보통입니다'
    else grade = '🌱 연습이 필요해요'

    box.innerHTML = `
        <h2>최종 결과</h2>
        <p><strong>평균:</strong> ${average}ms</p>
        <p><strong>최고:</strong> ${best}ms</p>
        <p><strong>최저:</strong> ${worst}ms</p>
        <p><strong>등급:</strong> ${grade}</p>
        <button onclick="resetGame()">다시 하기</button>
    `
    box.className = 'final-result'

    // 최고 기록 저장
    const storedBest = localStorage.getItem('reactionBest') || Infinity
    if (best < storedBest) {
        localStorage.setItem('reactionBest', best)
        box.innerHTML += '<p class="new-record">🎉 새 최고 기록!</p>'
    }
}

function resetGame() {
    results = []
    state = STATE.IDLE
    document.getElementById('results-list').innerHTML = ''
    const box = document.getElementById('box')
    box.textContent = '클릭하여 시작'
    box.style.backgroundColor = '#3498db'
    box.className = ''
}
```

> 🔥 **프로 팁**: 상태 머신(State Machine) 패턴을 사용하면 복잡한 게임 로직도 깔끔하게 관리할 수 있습니다. 각 상태에서 어떤 동작이 가능한지 명확히 정의하세요.

### 🎯 미니 퀴즈

1. 왜 `clearTimeout(timeoutId)`를 사용하나요?
2. `Date.now()`는 무엇을 반환하나요?
3. 스프레드 연산자 `...results`는 무엇을 하나요?

<details>
<summary>정답 보기</summary>

1. 이미 설정된 타임아웃을 취소하기 위해 (너무 빨리 클릭했을 때)
2. 1970년 1월 1일부터 현재까지의 밀리초 (타임스탬프)
3. 배열을 개별 인수로 펼침 (`Math.min(...[1,2,3])`은 `Math.min(1,2,3)`과 같음)

</details>

---

## 게임 5: 메모리 카드 게임

카드 짝 맞추기 게임입니다.

### 이 게임에서 배우는 것

| 개념 | 설명 | 게임에서의 적용 |
|------|------|----------------|
| 배열 섞기 | Fisher-Yates 알고리즘 | 카드 랜덤 배치 |
| DOM 생성 | 동적 요소 생성 | 카드 그리드 |
| CSS 애니메이션 | transform, transition | 카드 뒤집기 |
| 비동기 처리 | setTimeout 조합 | 짝 확인 딜레이 |

### 만들기

```
> 메모리 카드 게임 만들어줘.
> 8쌍(16장)의 카드.
> 같은 그림 찾으면 사라지게.
> 시도 횟수 표시.
> 카드 뒤집는 애니메이션도.
```

### 핵심 개념 (상세 주석 포함)

```javascript
// 카드 이모지 (8쌍)
const EMOJIS = ['🐶', '🐱', '🐭', '🐹', '🐰', '🦊', '🐻', '🐼']

// 게임 상태
let cards = []
let flippedCards = []     // 현재 뒤집힌 카드들
let matchedPairs = 0      // 맞춘 쌍 개수
let attempts = 0          // 시도 횟수
let isLocked = false      // 클릭 잠금 (짝 확인 중)
let startTime = null
let timerId = null

function initGame() {
    // 상태 초기화
    matchedPairs = 0
    attempts = 0
    flippedCards = []
    isLocked = false
    startTime = null
    if (timerId) clearInterval(timerId)

    // 카드 배열 생성 (각 이모지를 2번씩)
    cards = [...EMOJIS, ...EMOJIS]

    // 카드 섞기 (Fisher-Yates 알고리즘)
    shuffle(cards)

    // 보드 생성
    createBoard()

    // 화면 업데이트
    updateDisplay()
}

// Fisher-Yates 셔플 알고리즘
// 배열을 무작위로 섞는 가장 효율적인 방법
function shuffle(array) {
    for (let i = array.length - 1; i > 0; i--) {
        // 0부터 i까지 랜덤 인덱스
        const j = Math.floor(Math.random() * (i + 1));
        // 두 요소 교환 (구조 분해 할당)
        [array[i], array[j]] = [array[j], array[i]]
    }
    return array
}

function createBoard() {
    const board = document.getElementById('board')
    board.innerHTML = ''  // 기존 카드 제거

    cards.forEach((emoji, index) => {
        // 카드 컨테이너
        const card = document.createElement('div')
        card.className = 'card'
        card.dataset.index = index
        card.dataset.emoji = emoji

        // 카드 앞면 (이모지)
        const front = document.createElement('div')
        front.className = 'card-front'
        front.textContent = emoji

        // 카드 뒷면
        const back = document.createElement('div')
        back.className = 'card-back'
        back.textContent = '?'

        card.appendChild(front)
        card.appendChild(back)

        // 클릭 이벤트
        card.addEventListener('click', () => flipCard(card))

        board.appendChild(card)
    })
}

function flipCard(card) {
    // 클릭 불가 조건들
    if (isLocked) return                           // 짝 확인 중
    if (card.classList.contains('flipped')) return // 이미 뒤집힘
    if (card.classList.contains('matched')) return // 이미 맞춤
    if (flippedCards.length >= 2) return          // 2장 이상 뒤집힘

    // 첫 클릭 시 타이머 시작
    if (!startTime) {
        startTime = Date.now()
        timerId = setInterval(updateTimer, 1000)
    }

    // 카드 뒤집기
    card.classList.add('flipped')
    flippedCards.push(card)

    // 2장 뒤집었으면 짝 확인
    if (flippedCards.length === 2) {
        attempts++
        updateDisplay()
        checkMatch()
    }
}

function checkMatch() {
    const [card1, card2] = flippedCards
    const emoji1 = card1.dataset.emoji
    const emoji2 = card2.dataset.emoji

    if (emoji1 === emoji2) {
        // 짝 맞음!
        handleMatch(card1, card2)
    } else {
        // 짝 안 맞음
        handleMismatch(card1, card2)
    }
}

function handleMatch(card1, card2) {
    // 맞은 카드 표시
    card1.classList.add('matched')
    card2.classList.add('matched')

    matchedPairs++
    flippedCards = []

    // 효과음 또는 애니메이션
    card1.classList.add('success-animation')
    card2.classList.add('success-animation')

    // 모두 맞춤?
    if (matchedPairs === EMOJIS.length) {
        endGame()
    }
}

function handleMismatch(card1, card2) {
    isLocked = true  // 클릭 잠금

    // 잠깐 보여주고 다시 뒤집기
    setTimeout(() => {
        card1.classList.remove('flipped')
        card2.classList.remove('flipped')

        // 틀렸을 때 효과
        card1.classList.add('shake')
        card2.classList.add('shake')
        setTimeout(() => {
            card1.classList.remove('shake')
            card2.classList.remove('shake')
        }, 300)

        flippedCards = []
        isLocked = false  // 클릭 잠금 해제
    }, 1000)
}

function updateDisplay() {
    document.getElementById('attempts').textContent = attempts
    document.getElementById('pairs').textContent = `${matchedPairs}/${EMOJIS.length}`
}

function updateTimer() {
    const elapsed = Math.floor((Date.now() - startTime) / 1000)
    const minutes = Math.floor(elapsed / 60)
    const seconds = elapsed % 60
    document.getElementById('timer').textContent =
        `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`
}

function endGame() {
    clearInterval(timerId)

    const elapsed = Math.floor((Date.now() - startTime) / 1000)
    const efficiency = ((EMOJIS.length / attempts) * 100).toFixed(0)

    // 결과 모달 표시
    const modal = document.getElementById('result-modal')
    modal.innerHTML = `
        <div class="modal-content">
            <h2>🎉 축하합니다!</h2>
            <p>시간: ${formatTime(elapsed)}</p>
            <p>시도 횟수: ${attempts}회</p>
            <p>효율: ${efficiency}%</p>
            <p>등급: ${getEfficiencyGrade(efficiency)}</p>
            <button onclick="initGame(); closeModal()">다시 하기</button>
        </div>
    `
    modal.style.display = 'flex'
}

function formatTime(seconds) {
    const m = Math.floor(seconds / 60)
    const s = seconds % 60
    return `${m}분 ${s}초`
}

function getEfficiencyGrade(efficiency) {
    if (efficiency >= 90) return '🏆 완벽!'
    if (efficiency >= 70) return '⭐ 훌륭!'
    if (efficiency >= 50) return '👍 좋아요!'
    return '📝 연습 중'
}
```

### CSS 애니메이션 (카드 뒤집기)

```css
/* 카드 기본 스타일 */
.card {
    width: 80px;
    height: 80px;
    position: relative;
    cursor: pointer;
    transform-style: preserve-3d;  /* 3D 변환 활성화 */
    transition: transform 0.5s;    /* 부드러운 애니메이션 */
}

/* 뒤집힌 상태 */
.card.flipped {
    transform: rotateY(180deg);
}

/* 카드 면 공통 스타일 */
.card-front, .card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;  /* 뒷면 숨기기 */
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2rem;
    border-radius: 8px;
}

/* 앞면 (이모지) */
.card-front {
    background: #3498db;
    transform: rotateY(180deg);  /* 처음에는 뒤집힌 상태 */
}

/* 뒷면 (물음표) */
.card-back {
    background: #2c3e50;
    color: white;
}

/* 맞춘 카드 */
.card.matched {
    opacity: 0.6;
    cursor: default;
}

/* 성공 애니메이션 */
@keyframes success {
    0%, 100% { transform: rotateY(180deg) scale(1); }
    50% { transform: rotateY(180deg) scale(1.1); }
}

.card.success-animation {
    animation: success 0.3s ease;
}

/* 실패 애니메이션 (흔들기) */
@keyframes shake {
    0%, 100% { transform: rotateY(180deg) translateX(0); }
    25% { transform: rotateY(180deg) translateX(-5px); }
    75% { transform: rotateY(180deg) translateX(5px); }
}

.card.shake {
    animation: shake 0.3s ease;
}
```

> 💡 **비전공자 팁**: `transform-style: preserve-3d`와 `backface-visibility: hidden`이 3D 카드 뒤집기의 핵심입니다. 이 두 속성이 없으면 뒤집을 때 양면이 동시에 보여요.

---

## 게임 만들기 팁

### 1. 작게 시작하기

```
# 나쁜 예 - 한 번에 너무 많은 것을 요청
> RPG 게임 만들어줘. 캐릭터 성장, 던전, 보스전, 인벤토리,
> 스킬 시스템, 퀘스트, NPC 대화 있는.

# 좋은 예 - 핵심부터 시작
> 클릭하면 점수 올라가는 게임 만들어줘.
```

> ⚠️ **주의사항**: 복잡한 게임을 한 번에 요청하면 Claude도 혼란스럽고, 결과물도 불완전할 가능성이 높습니다.

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

# 5단계: 난이도
> 점수 높아질수록 장애물 빨라지게
```

### 3. 피드백으로 개선

```
> 점프가 너무 느려. 더 빠르게.

> 장애물 간격이 너무 좁아.

> 배경 색이 눈 아파. 바꿔줘.

> 점수판 글씨가 작아. 키워줘.
```

### 4. 디버깅 대화

```
> 점프가 두 번 되는 것 같아. 왜 그런지 확인해줘.

> 콘솔에 이런 에러가 나와: [에러 메시지 붙여넣기]

> 모바일에서는 안 돼. 터치 이벤트 확인해줘.
```

---

## 📝 실습 과제

### 레벨 1: 기본 (초보자)

위 5개 게임 중 하나를 골라 만들어보세요. 그대로 따라하는 것만으로도 훌륭합니다!

**체크리스트:**
- [ ] 게임이 시작되고 끝나는가?
- [ ] 점수가 표시되는가?
- [ ] 다시 시작할 수 있는가?

### 레벨 2: 응용 (중급자)

기본 게임에 다음 기능을 추가해보세요:

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

**체크리스트:**
- [ ] 효과음이 나오는가?
- [ ] 새로고침해도 최고 점수가 유지되는가?
- [ ] 휴대폰에서도 플레이할 수 있는가?

### 레벨 3: 도전 (심화)

완전히 새로운 게임을 만들어보세요:

**아이디어:**
- 🔨 두더지 잡기: 랜덤 위치에 두더지가 나타나고 클릭하면 점수
- 🐍 스네이크 게임: 방향키로 뱀 조종, 먹이 먹으면 길어짐
- 🏓 퐁 (테니스): 패들로 공 튕기기
- ⭕ 틱택토: 컴퓨터와 대결
- ❓ 퀴즈 게임: 객관식 문제 풀기
- 🧩 2048: 숫자 합치기 퍼즐
- 💣 지뢰찾기: 클래식 퍼즐 게임

```
> [게임 이름] 게임 만들어줘.
> [간단한 규칙 설명]
```

---

## 🏆 도전 과제

### 도전 1: 게임 콤보

두 개 이상의 게임 요소를 조합해보세요.

```
> 타이핑 + 반응속도 게임 만들어줘.
> 랜덤 시간 후 단어가 나타나면 빨리 타이핑하는 게임.
> 반응 시간과 타이핑 정확도 둘 다 측정.
```

### 도전 2: 멀티플레이어 (로컬)

같은 화면에서 2인용 게임:

```
> 2인용 퐁 게임 만들어줘.
> 왼쪽 플레이어는 W/S 키, 오른쪽 플레이어는 화살표 키.
> 5점 먼저 내면 승리.
```

### 도전 3: AI 대전

간단한 컴퓨터 AI:

```
> 틱택토 게임에 AI 추가해줘.
> 쉬움: 랜덤으로 두기
> 어려움: 항상 최선의 수 두기 (미니맥스 알고리즘)
```

---

## 게임을 웹에 공개하기

만든 게임을 배포해서 친구들과 공유하세요.

### GitHub Pages로 배포 (무료)

```bash
# 1. GitHub 저장소에 게임 파일 올리기
git add .
git commit -m "Add my awesome game"
git push origin main

# 2. GitHub 저장소 설정 > Pages > Source: main branch
# 3. 몇 분 후 https://username.github.io/repo-name 에서 접속!
```

### Vercel로 배포 (무료)

```bash
# Vercel CLI 설치
npm install -g vercel

# 배포
vercel
```

> 💡 **비전공자 팁**: 배포 후 링크를 친구들에게 공유하세요. 실제로 다른 사람이 플레이하는 것을 보면 동기부여가 됩니다!

---

## 안 되면?

게임 개발은 까다로울 수 있습니다. 자주 발생하는 문제와 해결법입니다.

### 클릭해도 아무 반응이 없습니다

**가능한 원인:**
1. 이벤트 리스너가 연결되지 않음
2. 함수 이름 오타
3. 스크립트가 DOM보다 먼저 로드됨

**해결 방법:**
```javascript
// 브라우저 콘솔(F12)에서 확인
console.log('버튼:', document.getElementById('myButton'))

// DOM이 로드된 후 실행하기
document.addEventListener('DOMContentLoaded', () => {
    // 여기에 코드
})
```

```
> 버튼이 클릭에 반응 안 해. 이벤트 리스너 확인해줘.
```

### 게임이 너무 빠르거나 느립니다

타이밍 문제는 게임에서 흔합니다:

```javascript
// setInterval 시간 조정 (밀리초)
setInterval(gameLoop, 16)  // 약 60fps
setInterval(gameLoop, 33)  // 약 30fps

// 또는 requestAnimationFrame 사용 (권장)
function gameLoop() {
    update()
    render()
    requestAnimationFrame(gameLoop)
}
```

### 점수가 화면에 업데이트되지 않습니다

변수는 바뀌는데 화면은 안 바뀔 수 있습니다:

```javascript
// 잘못된 예
score++
// 화면 업데이트를 깜빡함!

// 올바른 예
score++
document.getElementById('score').textContent = score
```

### 게임 상태가 엉망이 됩니다

여러 번 클릭하면 경쟁 상태가 발생할 수 있습니다:

```javascript
// 잠금 변수 사용
let isProcessing = false

function handleClick() {
    if (isProcessing) return  // 이미 처리 중이면 무시
    isProcessing = true

    // 처리...

    setTimeout(() => {
        isProcessing = false  // 완료 후 잠금 해제
    }, 500)
}
```

### 애니메이션이 끊깁니다

```javascript
// setInterval 대신 requestAnimationFrame 사용
function animate() {
    // 업데이트 로직
    requestAnimationFrame(animate)
}
```

> 🔥 **프로 팁**: `requestAnimationFrame`은 브라우저가 다음 화면을 그리기 직전에 호출되므로 더 부드럽습니다.

### 데스크톱에서는 되는데 모바일에서 안 됩니다

터치 이벤트는 클릭 이벤트와 다릅니다:

```javascript
// 클릭과 터치 모두 지원
element.addEventListener('click', handleInput)
element.addEventListener('touchstart', handleInput)

// 또는 pointer 이벤트 사용 (둘 다 처리)
element.addEventListener('pointerdown', handleInput)
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
    lives = 3
    isGameOver = false
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
let timerId = null
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

```javascript
function handleAction() {
    // 게임이 활성 상태인지 확인
    if (!isGameActive) return
    if (isGameOver) return

    // 실제 로직
}
```

### 실수 5: 모든 것 하드코딩

난이도 조절이 어려워집니다:

```javascript
// 조절 어려움
if (score > 100) levelUp()
setTimeout(spawn, 1000)

// 더 나음 - 변수/상수 사용
const LEVEL_UP_THRESHOLD = 100
const SPAWN_INTERVAL = 1000

if (score > LEVEL_UP_THRESHOLD) levelUp()
setTimeout(spawn, SPAWN_INTERVAL)
```

> 💡 **비전공자 팁**: 게임의 "매직 넘버"(100, 1000 등)를 상수로 정의하면 나중에 밸런스 조정이 훨씬 쉬워집니다.

---

## 용어 정리

| 용어 | 의미 |
|------|------|
| 게임 루프 | 게임이 계속 실행되게 하는 반복 구조 |
| 상태(State) | 게임의 현재 상황을 저장하는 변수들 |
| 이벤트 리스너 | 사용자 입력을 감지하는 함수 |
| 충돌 감지 | 두 객체가 닿았는지 확인하는 로직 |
| FPS | Frames Per Second, 초당 화면 갱신 횟수 |
| requestAnimationFrame | 브라우저 최적화된 애니메이션 함수 |
| localStorage | 브라우저에 데이터를 영구 저장하는 공간 |
| Fisher-Yates | 배열을 무작위로 섞는 알고리즘 |
| 상태 머신 | 상태 전환을 관리하는 디자인 패턴 |

---

## 다음 챕터 미리보기

축하합니다! Part 3 (실전 프로젝트 I)을 완료했습니다.

다음 Part에서는 더 실용적인 도구를 만들어봅니다:
- **Chapter 17**: CLI 도구 만들기 - 터미널에서 실행하는 자동화 도구
- **Chapter 18**: 챗봇 만들기 - Discord/Slack 봇
- **Chapter 19**: 풀스택 앱 만들기 - 프론트엔드 + 백엔드 + 데이터베이스

게임 만들기에서 배운 상태 관리, 이벤트 처리, 비동기 로직은 모든 프로젝트의 기초가 됩니다!

---

## 정리

이번 챕터에서 배운 것:
- [x] 게임의 핵심 개념 (상태, 루프, 이벤트, 충돌)
- [x] 5가지 다른 유형의 게임 만들기
- [x] 사용자 입력 처리
- [x] 점수와 상태 관리
- [x] CSS 애니메이션 기초
- [x] 게임 개선과 디버깅

[Chapter 17: CLI 도구 만들기](../Chapter17-CLI-Tools/README.ko.md)로 넘어가세요.

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [MDN Canvas API](https://developer.mozilla.org/ko/docs/Web/API/Canvas_API) - 게임 그래픽을 위한 Canvas 사용법
- [MDN 이벤트 처리](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Building_blocks/Events) - 키보드/마우스 이벤트
- [requestAnimationFrame](https://developer.mozilla.org/ko/docs/Web/API/window/requestAnimationFrame) - 부드러운 게임 루프

**영상 자료:**
- [JavaScript 게임 만들기 튜토리얼 (YouTube)](https://www.youtube.com/results?search_query=javascript+game+tutorial+beginner)
- [HTML5 Canvas 게임 개발 (YouTube)](https://www.youtube.com/results?search_query=html5+canvas+game+development)
- [바닐라 JS로 게임 만들기 (YouTube)](https://www.youtube.com/results?search_query=vanilla+javascript+game+from+scratch)

**읽을거리:**
- [게임 개발 패턴](https://gameprogrammingpatterns.com/) - 게임 개발의 디자인 패턴
- [게임 루프 이해하기](https://gafferongames.com/post/fix_your_timestep/) - 게임 루프 심화

**관련 도구:**
- [Phaser.js](https://phaser.io/) - 인기있는 JavaScript 게임 프레임워크
- [PixiJS](https://pixijs.com/) - 2D 렌더링 엔진
- [Kaboom.js](https://kaboomjs.com/) - 간단한 게임 라이브러리

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
