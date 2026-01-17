# Chapter 16: Mini Games

**English** | [한국어](./README.ko.md)

---

## Ask Questions

If you have any questions during your studies, feel free to ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask%20Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## What You'll Learn in This Chapter

- Creating games with Claude
- Core concepts of JavaScript game logic
- User input handling and state management
- Completing fun projects
- Improving programming skills through game development

---

## Connection to Previous Chapters

In Chapter 15, you learned how to integrate APIs. Now we'll combine all the HTML, CSS, and JavaScript knowledge you've learned to create **games you can actually play**. Games are the ultimate learning projects where all programming concepts converge.

---

## Why Is This Necessary?

Games aren't just about fun. Making games is the most engaging way to learn programming concepts.

**Real situations where game development skills help:**

- **Learning to program**: Variables, loops, conditionals, functions - games use all of them
- **Portfolio impact**: Playable games are much more impressive than static pages
- **User engagement**: Interactive elements keep visitors engaged longer
- **Problem solving**: Game logic sharpens your coding brain
- **Interviews**: "I built a game" is a great conversation starter

> Every game mechanic is a programming concept in disguise. Score tracking? That's state management. Collision detection? That's conditionals. Game loop? That's event handling.

### Simple Analogy: Games Are a Gym for Coders

Learning to code by building utilities is like exercising while doing housework - it works, but it's boring.

Making games is like going to the gym - you're still exercising (learning), but it's actually fun. And like the gym, you get stronger (better at coding) while enjoying yourself.

---

## Core Concepts of Game Development

There are key concepts you need to understand before making games. Once you understand these, you can make any game.

### 1. Game State

Variables that store the "current situation" of the game.

```javascript
// Game state examples
let score = 0           // Current score
let lives = 3           // Remaining lives
let level = 1           // Current level
let isGameOver = false  // Whether the game is over
let isPaused = false    // Whether the game is paused
```

> **Beginner Tip**: Think of game state as the "memory" of the game. It's all the information needed when you save and load a game.

### 2. Game Loop

The repeating structure that keeps the game running.

```javascript
// Basic game loop
function gameLoop() {
    if (isGameOver) return  // Stop if game is over

    update()    // Update state (character movement, collision detection, etc.)
    render()    // Draw the screen

    requestAnimationFrame(gameLoop)  // Request next frame
}
```

> **Beginner Tip**: The game loop is similar to a movie. Just like a movie shows 24 images per second, games redraw the screen about 60 times per second. That's why it looks like it's moving.

### 3. Event Handling

Responding to user input (clicks, keyboard).

```javascript
// Keyboard input handling
document.addEventListener('keydown', (e) => {
    if (e.key === 'ArrowLeft') player.moveLeft()
    if (e.key === 'ArrowRight') player.moveRight()
    if (e.key === ' ') player.jump()  // Spacebar
})

// Mouse click handling
canvas.addEventListener('click', (e) => {
    const x = e.clientX
    const y = e.clientY
    handleClick(x, y)
})
```

> **Pro Tip**: `keydown` fires the moment a key is pressed, `keyup` fires when it's released. To have a character move continuously while a key is held, you need a separate state variable.

### 4. Collision Detection

Checking if two objects are touching.

```javascript
// Rectangle collision detection (the simplest method)
function isColliding(rect1, rect2) {
    return rect1.x < rect2.x + rect2.width &&
           rect1.x + rect1.width > rect2.x &&
           rect1.y < rect2.y + rect2.height &&
           rect1.y + rect1.height > rect2.y
}

// Usage example
if (isColliding(player, enemy)) {
    player.takeDamage()
}
```

> **Caution**: Circular object collision detection uses a different formula. Two circles collide when the distance between their centers is less than the sum of their radii.

---

## Try It Yourself: The Simplest Game

Before making complex games, let's verify the basics work. Here's the simplest game:

```
> Make a button that shows a counter.
> The counter goes up by 1 each time you click.
> That's it.
```

Try clicking it a few times. You just made a "clicker game." The same genre as Cookie Clicker, which has millions of players. Everything else is just adding features to this foundation.

### Extended Clicker Game Example

```
> Make a game where clicking increases the score.
> - Level up every 10 points
> - Higher levels mean more points per click
> - Nice animations and sound effects
> - Save the high score
```

---

## Why Games?

Making games is the most fun way to learn programming.

Games have everything:
- Drawing on screen (HTML/CSS)
- Receiving user input (keyboard, mouse)
- Processing logic (JavaScript)
- Managing state (score, level)

**Game Request Tips:**

```
> Make a number guessing game. Range 1~100.
> Show the number of attempts, and display a congratulations message if guessed within 10 tries.
```

Describing game rules and desired features specifically leads to more complete games.

> **Beginner Tip**: Start with simple games first. Increase difficulty in order: "Number Guessing" -> "Rock Paper Scissors" -> "Typing Game".

---

## Game 1: Number Guessing

Let's start with the simplest game.

### Game Description

- Computer picks a number between 1-100
- Player guesses until correct
- Provides "Higher" / "Lower" hints

### What You'll Learn in This Game

| Concept | Description | Application in Game |
|---------|-------------|---------------------|
| Variables | Space to store data | Answer, attempt count |
| Conditionals | Different actions based on situation | Answer comparison |
| Functions | Reusable code blocks | Guess checking logic |
| Events | Responding to user actions | Button clicks |
| DOM Manipulation | Changing screen content | Displaying results |

### Creating It

```
> Make a number guessing game.
> It's a game where you guess a number between 1 and 100.
> Show hints too.
```

### Example Result

```html
<!-- HTML structure -->
<div id="game">
    <h1>Guess the Number</h1>
    <p>Guess a number between 1 and 100!</p>

    <input type="number" id="guess" placeholder="Enter a number" min="1" max="100">
    <button onclick="checkGuess()">Check</button>

    <p id="result"></p>
    <p>Attempts: <span id="attempts">0</span></p>

    <!-- Hint area -->
    <div id="hint-area">
        <p>Range: <span id="range">1 ~ 100</span></p>
    </div>
</div>
```

```javascript
// Game state variables
let answer = Math.floor(Math.random() * 100) + 1  // Random number between 1-100
let attempts = 0  // Number of attempts
let minRange = 1  // Minimum range (for hints)
let maxRange = 100  // Maximum range (for hints)

function checkGuess() {
    // Get input value
    const guessInput = document.getElementById('guess')
    const guess = parseInt(guessInput.value)

    // Validation
    if (isNaN(guess) || guess < 1 || guess > 100) {
        document.getElementById('result').textContent = 'Please enter a number between 1 and 100!'
        document.getElementById('result').style.color = 'orange'
        return
    }

    // Increment attempt count
    attempts++
    document.getElementById('attempts').textContent = attempts

    const resultEl = document.getElementById('result')

    if (guess === answer) {
        // Correct!
        resultEl.textContent = `Correct! You got it in ${attempts} attempts!`
        resultEl.style.color = 'green'

        // Celebration effect
        if (attempts <= 5) {
            resultEl.textContent += ' You\'re a genius!'
        } else if (attempts <= 7) {
            resultEl.textContent += ' Excellent!'
        }

        // Show restart button
        showRestartButton()

    } else if (guess < answer) {
        // Go higher
        resultEl.textContent = 'Go higher!'
        resultEl.style.color = '#3498db'
        minRange = Math.max(minRange, guess + 1)
        updateRange()

    } else {
        // Go lower
        resultEl.textContent = 'Go lower!'
        resultEl.style.color = '#e74c3c'
        maxRange = Math.min(maxRange, guess - 1)
        updateRange()
    }

    // Clear and focus input field
    guessInput.value = ''
    guessInput.focus()
}

function updateRange() {
    document.getElementById('range').textContent = `${minRange} ~ ${maxRange}`
}

function showRestartButton() {
    const btn = document.createElement('button')
    btn.textContent = 'Play Again'
    btn.onclick = restartGame
    document.getElementById('game').appendChild(btn)
}

function restartGame() {
    // Reset all state
    answer = Math.floor(Math.random() * 100) + 1
    attempts = 0
    minRange = 1
    maxRange = 100

    // Update display
    document.getElementById('attempts').textContent = '0'
    document.getElementById('result').textContent = ''
    document.getElementById('range').textContent = '1 ~ 100'
    document.getElementById('guess').value = ''
    document.getElementById('guess').focus()

    // Remove restart button
    const restartBtn = document.querySelector('button:last-child')
    if (restartBtn.textContent === 'Play Again') {
        restartBtn.remove()
    }
}

// Allow checking with Enter key
document.getElementById('guess').addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
        checkGuess()
    }
})
```

> **Beginner Tip**: `Math.floor(Math.random() * 100) + 1` looks complex, right? Let's break it down:
> - `Math.random()` -> Decimal between 0~0.999...
> - `* 100` -> Number between 0~99.999...
> - `Math.floor()` -> Remove decimal (0~99)
> - `+ 1` -> Adjust to 1~100

### Improving It

```
> Add a restart button
```
State reset pattern - the basics of game loops

```
> Save the high score
```
`localStorage`-based data persistence

```
> Add nice styling
```
Game UI/UX design basics

```
> Add difficulty selection (Easy: 1-50, Normal: 1-100, Hard: 1-500)
```
Configuration-based game logic

### Mini Quiz

1. What range of numbers does `Math.random()` return?
2. Why do we use `parseInt()`?
3. What does `attempts++` mean?

<details>
<summary>View Answers</summary>

1. Decimals from 0 up to (but not including) 1 (e.g., 0.7342...)
2. To convert the string input value to a number
3. Same as `attempts = attempts + 1`, incrementing the attempt count by 1

</details>

---

## Game 2: Rock Paper Scissors

Let's make this classic game.

### What You'll Learn in This Game

| Concept | Description | Application in Game |
|---------|-------------|---------------------|
| Arrays | Store multiple values in order | List of choices |
| Random selection | Pick randomly from array | Computer's choice |
| Complex conditionals | Combining multiple conditions | Win/lose determination |
| Statistics calculation | Data aggregation | Displaying win rate |

### Creating It

```
> Make a Rock Paper Scissors game.
> It's a game where you compete against the computer.
> Show the score too.
> Display rock paper scissors with emojis.
```

### Core Logic (with detailed comments)

```javascript
// Game state
let playerWins = 0
let computerWins = 0
let draws = 0
let history = []  // Game history

// Define choices (with emojis)
const CHOICES = {
    rock: { name: 'Rock', emoji: '&#9994;', beats: 'scissors' },
    scissors: { name: 'Scissors', emoji: '&#9996;', beats: 'paper' },
    paper: { name: 'Paper', emoji: '&#9995;', beats: 'rock' }
}

function play(playerChoice) {
    // Computer's random selection
    const choices = Object.keys(CHOICES)  // ['rock', 'scissors', 'paper']
    const randomIndex = Math.floor(Math.random() * choices.length)
    const computerChoice = choices[randomIndex]

    // Determine result
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

    // Save history
    history.push({
        player: playerChoice,
        computer: computerChoice,
        result: result,
        timestamp: new Date()
    })

    // Update display
    updateDisplay(playerChoice, computerChoice, result)
    updateStats()

    return result
}

function updateDisplay(playerChoice, computerChoice, result) {
    const player = CHOICES[playerChoice]
    const computer = CHOICES[computerChoice]

    // Display choices
    document.getElementById('player-choice').textContent = player.emoji
    document.getElementById('computer-choice').textContent = computer.emoji

    // Result message
    const resultEl = document.getElementById('result')
    const messages = {
        win: 'You win!',
        lose: 'You lose...',
        draw: 'It\'s a tie!'
    }
    resultEl.textContent = messages[result]
    resultEl.className = `result-${result}`  // For CSS styling
}

function updateStats() {
    const total = playerWins + computerWins + draws
    const winRate = total > 0 ? ((playerWins / total) * 100).toFixed(1) : 0

    document.getElementById('player-wins').textContent = playerWins
    document.getElementById('computer-wins').textContent = computerWins
    document.getElementById('draws').textContent = draws
    document.getElementById('win-rate').textContent = `${winRate}%`
}

// Statistics analysis function
function analyzeHistory() {
    if (history.length === 0) return null

    // Most frequently selected
    const playerChoices = history.map(h => h.player)
    const mostUsed = getMostFrequent(playerChoices)

    // Last 5 games win rate
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

> **Pro Tip**: Using objects (`CHOICES`) can reduce conditionals. Defining what each choice beats as data makes the code much cleaner.

### Improving It

```
> Change to best of 5 format
```
Round-based game logic design

```
> Show win rate statistics
```
Game statistics and data visualization

```
> Add animation (brief loading after selection)
```
Timing and suspense effects

```
> Make the computer smarter (analyze player patterns)
```
Simple AI logic

### Beginner Tip: Why Use Objects?

```javascript
// This way has too many conditionals
if (player === 'scissors' && computer === 'paper') return 'win'
if (player === 'scissors' && computer === 'rock') return 'lose'
if (player === 'rock' && computer === 'scissors') return 'win'
// ... continues ...

// With objects, one line solves it!
if (CHOICES[player].beats === computer) return 'win'
```

Well-designed data structures make code simpler.

---

## Game 3: Typing Game

A keyboard practice game.

### What You'll Learn in This Game

| Concept | Description | Application in Game |
|---------|-------------|---------------------|
| Timer | Time-based logic | Time limit |
| String comparison | Text matching verification | Typing validation |
| Real-time feedback | Immediate response | Typing confirmation |
| Performance measurement | Speed/accuracy calculation | WPM measurement |

### Creating It

```
> Make a typing game.
> A game where you type words quickly when they appear.
> 30 second time limit.
> Show score and WPM too.
```

### Core Logic (with detailed comments)

```javascript
// Word list (by difficulty)
const WORDS = {
    easy: ['apple', 'banana', 'cherry', 'grape', 'melon', 'peach', 'pear', 'plum'],
    medium: ['programming', 'javascript', 'computer', 'keyboard', 'monitor', 'internet'],
    hard: ['algorithm', 'database', 'artificial', 'machine', 'blockchain', 'metaverse']
}

// Game state
let score = 0
let timeLeft = 30
let totalTyped = 0      // Total characters typed
let correctTyped = 0    // Correctly typed characters
let currentWord = ''
let timerId = null
let difficulty = 'easy'
let gameStartTime = null

function startGame() {
    // Clear previous timer if exists
    if (timerId) clearInterval(timerId)

    // Reset state
    score = 0
    timeLeft = 30
    totalTyped = 0
    correctTyped = 0
    gameStartTime = Date.now()

    // Reset display
    updateDisplay()
    showRandomWord()

    // Enable and focus input field
    const input = document.getElementById('input')
    input.disabled = false
    input.value = ''
    input.focus()

    // Start timer
    timerId = setInterval(() => {
        timeLeft--
        document.getElementById('timer').textContent = timeLeft

        // Turn red when 10 seconds or less remain
        if (timeLeft <= 10) {
            document.getElementById('timer').classList.add('warning')
        }

        if (timeLeft <= 0) {
            endGame()
        }
    }, 1000)  // Run every 1 second
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
    totalTyped += 1  // Key input count

    // Real-time feedback: Check if correct so far
    const wordEl = document.getElementById('word')
    if (currentWord.startsWith(input)) {
        wordEl.classList.remove('incorrect')
        wordEl.classList.add('typing')
    } else {
        wordEl.classList.add('incorrect')
        wordEl.classList.remove('typing')
    }

    // If completely matching
    if (input === currentWord) {
        score++
        correctTyped += currentWord.length

        // Success effect
        wordEl.classList.add('correct')

        // Combo bonus
        if (score > 0 && score % 5 === 0) {
            timeLeft += 2  // Add 2 seconds every 5 words
            showBonus('+2 seconds!')
        }

        updateDisplay()

        // Next word
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

    // Disable input
    document.getElementById('input').disabled = true

    // Calculate WPM (Words Per Minute)
    const elapsedMinutes = (Date.now() - gameStartTime) / 60000
    const wpm = Math.round(score / elapsedMinutes)

    // Calculate accuracy
    const accuracy = totalTyped > 0
        ? Math.round((correctTyped / totalTyped) * 100)
        : 0

    // Display results
    showResults(wpm, accuracy)

    // Save high score
    saveHighScore(score, wpm)
}

function showResults(wpm, accuracy) {
    const resultDiv = document.getElementById('results')
    resultDiv.innerHTML = `
        <h2>Game Over!</h2>
        <p>Score: ${score} words</p>
        <p>Typing Speed: ${wpm} WPM</p>
        <p>Accuracy: ${accuracy}%</p>
        <p>Grade: ${getGrade(wpm)}</p>
        <button onclick="startGame()">Play Again</button>
    `
    resultDiv.style.display = 'block'
}

function getGrade(wpm) {
    if (wpm >= 80) return 'Expert'
    if (wpm >= 60) return 'Advanced'
    if (wpm >= 40) return 'Intermediate'
    if (wpm >= 20) return 'Beginner'
    return 'Novice'
}

function saveHighScore(score, wpm) {
    const highScore = localStorage.getItem('typingHighScore') || 0
    if (score > highScore) {
        localStorage.setItem('typingHighScore', score)
        showBonus('New Record!')
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

> **Caution**: When using `setInterval`, you must clean it up with `clearInterval`. Otherwise, timers will run multiple times when starting the game repeatedly.

### Improving It

```
> Make word length vary by difficulty
```
Difficulty curve design

```
> Highlight incorrect letters in red
```
Real-time feedback UI

```
> Add a combo system (bonus for consecutive correct answers)
```
Motivation mechanics

---

## Game 4: Reaction Speed Test

A game that measures reaction speed.

### What You'll Learn in This Game

| Concept | Description | Application in Game |
|---------|-------------|---------------------|
| setTimeout | Delayed execution | Random wait time |
| Date.now() | Current time in milliseconds | Time measurement |
| Array methods | reduce, sort, etc. | Average/best record calculation |
| State machine | Game state transitions | Waiting/Ready/Measuring |

### Creating It

```
> Make a reaction speed test game.
> Click when the screen turns green.
> Show reaction time in milliseconds.
> Show the average after 5 tests.
```

### Core Logic (with detailed comments)

```javascript
// Define game states
const STATE = {
    IDLE: 'idle',       // Before start
    WAITING: 'waiting', // Waiting (red)
    READY: 'ready',     // Ready (green)
    RESULT: 'result'    // Showing result
}

// Game variables
let state = STATE.IDLE
let startTime = null
let timeoutId = null
let results = []
const MAX_ROUNDS = 5

function startTest() {
    // Clear previous timeout
    if (timeoutId) clearTimeout(timeoutId)

    // State transition: Waiting
    state = STATE.WAITING
    const box = document.getElementById('box')
    box.style.backgroundColor = '#e74c3c'  // Red
    box.textContent = 'Wait for green...'
    box.className = 'waiting'

    // Turn green after random time between 1-4 seconds
    const delay = Math.random() * 3000 + 1000
    timeoutId = setTimeout(() => {
        state = STATE.READY
        box.style.backgroundColor = '#2ecc71'  // Green
        box.textContent = 'Click!'
        box.className = 'ready'
        startTime = Date.now()  // Start timing
    }, delay)
}

function handleClick() {
    const box = document.getElementById('box')

    switch (state) {
        case STATE.IDLE:
            // First click: Start game
            startTest()
            break

        case STATE.WAITING:
            // Clicked too early!
            clearTimeout(timeoutId)
            state = STATE.RESULT
            box.style.backgroundColor = '#f39c12'  // Orange
            box.textContent = 'Too early! Wait until it turns green'
            box.className = 'too-early'

            // Can start again after 2 seconds
            setTimeout(() => {
                state = STATE.IDLE
                box.textContent = 'Click to try again'
                box.style.backgroundColor = '#3498db'
            }, 2000)
            break

        case STATE.READY:
            // Normal click: Measure reaction time
            const reactionTime = Date.now() - startTime
            state = STATE.RESULT
            results.push(reactionTime)

            box.style.backgroundColor = '#9b59b6'  // Purple
            box.textContent = `${reactionTime}ms`
            box.className = 'result'

            // Analyze results
            updateResults(reactionTime)

            // Check if 5 rounds completed
            if (results.length >= MAX_ROUNDS) {
                showFinalResults()
            } else {
                // Next round after 1 second
                setTimeout(() => {
                    state = STATE.IDLE
                    box.textContent = `Round ${results.length + 1}/${MAX_ROUNDS} - Click to start`
                    box.style.backgroundColor = '#3498db'
                }, 1000)
            }
            break

        case STATE.RESULT:
            // Ignore while showing result
            break
    }
}

function updateResults(latestTime) {
    const resultsList = document.getElementById('results-list')

    // Rate the result
    let rating = ''
    if (latestTime < 200) rating = 'Lightning!'
    else if (latestTime < 250) rating = 'Fast!'
    else if (latestTime < 350) rating = 'Average'
    else rating = 'Slow'

    // Add result
    const li = document.createElement('li')
    li.textContent = `Round ${results.length}: ${latestTime}ms ${rating}`
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

    // Calculate statistics
    const sum = results.reduce((a, b) => a + b, 0)
    const average = Math.round(sum / results.length)
    const best = Math.min(...results)
    const worst = Math.max(...results)

    // Grade based on average
    let grade = ''
    if (average < 200) grade = 'Pro gamer level!'
    else if (average < 250) grade = 'Excellent reflexes!'
    else if (average < 300) grade = 'Above average!'
    else if (average < 400) grade = 'Normal'
    else grade = 'Needs practice'

    box.innerHTML = `
        <h2>Final Results</h2>
        <p><strong>Average:</strong> ${average}ms</p>
        <p><strong>Best:</strong> ${best}ms</p>
        <p><strong>Worst:</strong> ${worst}ms</p>
        <p><strong>Grade:</strong> ${grade}</p>
        <button onclick="resetGame()">Play Again</button>
    `
    box.className = 'final-result'

    // Save best record
    const storedBest = localStorage.getItem('reactionBest') || Infinity
    if (best < storedBest) {
        localStorage.setItem('reactionBest', best)
        box.innerHTML += '<p class="new-record">New Best Record!</p>'
    }
}

function resetGame() {
    results = []
    state = STATE.IDLE
    document.getElementById('results-list').innerHTML = ''
    const box = document.getElementById('box')
    box.textContent = 'Click to Start'
    box.style.backgroundColor = '#3498db'
    box.className = ''
}
```

> **Pro Tip**: Using the State Machine pattern allows you to manage complex game logic cleanly. Clearly define what actions are possible in each state.

### Mini Quiz

1. Why do we use `clearTimeout(timeoutId)`?
2. What does `Date.now()` return?
3. What does the spread operator `...results` do?

<details>
<summary>View Answers</summary>

1. To cancel an already set timeout (when clicking too early)
2. Milliseconds since January 1, 1970 (timestamp)
3. Spreads the array into individual arguments (`Math.min(...[1,2,3])` is the same as `Math.min(1,2,3)`)

</details>

---

## Game 5: Memory Card Game

A card matching game.

### What You'll Learn in This Game

| Concept | Description | Application in Game |
|---------|-------------|---------------------|
| Array shuffling | Fisher-Yates algorithm | Random card placement |
| DOM creation | Dynamic element generation | Card grid |
| CSS animation | transform, transition | Card flipping |
| Asynchronous handling | setTimeout combinations | Match checking delay |

### Creating It

```
> Make a memory card game.
> 8 pairs (16 cards).
> Make matched cards disappear.
> Show attempt count.
> Include card flipping animation.
```

### Core Concepts (with detailed comments)

```javascript
// Card emojis (8 pairs)
const EMOJIS = ['dog', 'cat', 'mouse', 'hamster', 'rabbit', 'fox', 'bear', 'panda']

// Game state
let cards = []
let flippedCards = []     // Currently flipped cards
let matchedPairs = 0      // Number of matched pairs
let attempts = 0          // Number of attempts
let isLocked = false      // Click lock (while checking match)
let startTime = null
let timerId = null

function initGame() {
    // Reset state
    matchedPairs = 0
    attempts = 0
    flippedCards = []
    isLocked = false
    startTime = null
    if (timerId) clearInterval(timerId)

    // Create card array (each emoji twice)
    cards = [...EMOJIS, ...EMOJIS]

    // Shuffle cards (Fisher-Yates algorithm)
    shuffle(cards)

    // Create board
    createBoard()

    // Update display
    updateDisplay()
}

// Fisher-Yates shuffle algorithm
// The most efficient way to randomly shuffle an array
function shuffle(array) {
    for (let i = array.length - 1; i > 0; i--) {
        // Random index from 0 to i
        const j = Math.floor(Math.random() * (i + 1));
        // Swap two elements (destructuring assignment)
        [array[i], array[j]] = [array[j], array[i]]
    }
    return array
}

function createBoard() {
    const board = document.getElementById('board')
    board.innerHTML = ''  // Remove existing cards

    cards.forEach((emoji, index) => {
        // Card container
        const card = document.createElement('div')
        card.className = 'card'
        card.dataset.index = index
        card.dataset.emoji = emoji

        // Card front (emoji)
        const front = document.createElement('div')
        front.className = 'card-front'
        front.textContent = emoji

        // Card back
        const back = document.createElement('div')
        back.className = 'card-back'
        back.textContent = '?'

        card.appendChild(front)
        card.appendChild(back)

        // Click event
        card.addEventListener('click', () => flipCard(card))

        board.appendChild(card)
    })
}

function flipCard(card) {
    // Conditions where clicking is not allowed
    if (isLocked) return                           // Checking match
    if (card.classList.contains('flipped')) return // Already flipped
    if (card.classList.contains('matched')) return // Already matched
    if (flippedCards.length >= 2) return          // 2 or more already flipped

    // Start timer on first click
    if (!startTime) {
        startTime = Date.now()
        timerId = setInterval(updateTimer, 1000)
    }

    // Flip card
    card.classList.add('flipped')
    flippedCards.push(card)

    // Check for match if 2 cards flipped
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
        // Match!
        handleMatch(card1, card2)
    } else {
        // No match
        handleMismatch(card1, card2)
    }
}

function handleMatch(card1, card2) {
    // Mark matched cards
    card1.classList.add('matched')
    card2.classList.add('matched')

    matchedPairs++
    flippedCards = []

    // Sound effect or animation
    card1.classList.add('success-animation')
    card2.classList.add('success-animation')

    // All matched?
    if (matchedPairs === EMOJIS.length) {
        endGame()
    }
}

function handleMismatch(card1, card2) {
    isLocked = true  // Lock clicks

    // Show briefly then flip back
    setTimeout(() => {
        card1.classList.remove('flipped')
        card2.classList.remove('flipped')

        // Wrong effect
        card1.classList.add('shake')
        card2.classList.add('shake')
        setTimeout(() => {
            card1.classList.remove('shake')
            card2.classList.remove('shake')
        }, 300)

        flippedCards = []
        isLocked = false  // Unlock clicks
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

    // Display result modal
    const modal = document.getElementById('result-modal')
    modal.innerHTML = `
        <div class="modal-content">
            <h2>Congratulations!</h2>
            <p>Time: ${formatTime(elapsed)}</p>
            <p>Attempts: ${attempts}</p>
            <p>Efficiency: ${efficiency}%</p>
            <p>Grade: ${getEfficiencyGrade(efficiency)}</p>
            <button onclick="initGame(); closeModal()">Play Again</button>
        </div>
    `
    modal.style.display = 'flex'
}

function formatTime(seconds) {
    const m = Math.floor(seconds / 60)
    const s = seconds % 60
    return `${m}m ${s}s`
}

function getEfficiencyGrade(efficiency) {
    if (efficiency >= 90) return 'Perfect!'
    if (efficiency >= 70) return 'Excellent!'
    if (efficiency >= 50) return 'Good!'
    return 'Practicing'
}
```

### CSS Animation (Card Flipping)

```css
/* Card base style */
.card {
    width: 80px;
    height: 80px;
    position: relative;
    cursor: pointer;
    transform-style: preserve-3d;  /* Enable 3D transformation */
    transition: transform 0.5s;    /* Smooth animation */
}

/* Flipped state */
.card.flipped {
    transform: rotateY(180deg);
}

/* Common style for card faces */
.card-front, .card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;  /* Hide back side */
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2rem;
    border-radius: 8px;
}

/* Front (emoji) */
.card-front {
    background: #3498db;
    transform: rotateY(180deg);  /* Initially flipped */
}

/* Back (question mark) */
.card-back {
    background: #2c3e50;
    color: white;
}

/* Matched cards */
.card.matched {
    opacity: 0.6;
    cursor: default;
}

/* Success animation */
@keyframes success {
    0%, 100% { transform: rotateY(180deg) scale(1); }
    50% { transform: rotateY(180deg) scale(1.1); }
}

.card.success-animation {
    animation: success 0.3s ease;
}

/* Failure animation (shake) */
@keyframes shake {
    0%, 100% { transform: rotateY(180deg) translateX(0); }
    25% { transform: rotateY(180deg) translateX(-5px); }
    75% { transform: rotateY(180deg) translateX(5px); }
}

.card.shake {
    animation: shake 0.3s ease;
}
```

> **Beginner Tip**: `transform-style: preserve-3d` and `backface-visibility: hidden` are the keys to 3D card flipping. Without these properties, both sides would be visible when flipping.

---

## Game Development Tips

### 1. Start Small

```
# Bad example - requesting too much at once
> Make an RPG game. With character growth, dungeons, boss battles, inventory,
> skill system, quests, NPC dialogue.

# Good example - start with the core
> Make a game where clicking increases the score.
```

> **Caution**: Requesting complex games all at once confuses Claude, and the results are likely to be incomplete.

### 2. Add One Feature at a Time

```
# Step 1: Basic
> Make a jumping character

# Step 2: Obstacles
> Add obstacles

# Step 3: Collision
> Game over when hitting obstacles

# Step 4: Score
> Number of obstacles passed = score

# Step 5: Difficulty
> Obstacles get faster as score increases
```

### 3. Improve with Feedback

```
> The jump is too slow. Make it faster.

> The obstacle spacing is too narrow.

> The background color hurts my eyes. Change it.

> The score text is too small. Make it bigger.
```

### 4. Debugging Conversations

```
> It seems like I can jump twice. Check why.

> I get this error in the console: [paste error message]

> It doesn't work on mobile. Check touch events.
```

---

## Practice Exercises

### Level 1: Basic (Beginner)

Choose one of the 5 games above and make it. Just following along is great!

**Checklist:**
- [ ] Does the game start and end?
- [ ] Is the score displayed?
- [ ] Can you restart?

### Level 2: Application (Intermediate)

Add these features to the basic game:

```
> Add sound effects to the game
```
Sound feedback with Web Audio API

```
> Save the high score (localStorage)
```
Browser storage-based data persistence

```
> Make it work on mobile too
```
Touch events and responsive layout

**Checklist:**
- [ ] Do sound effects play?
- [ ] Does the high score persist after refresh?
- [ ] Can you play on mobile?

### Level 3: Challenge (Advanced)

Create a completely new game:

**Ideas:**
- Whac-A-Mole: Moles appear at random positions, click for points
- Snake Game: Control snake with arrow keys, eating food makes it longer
- Pong: Bounce ball with paddle
- Tic-Tac-Toe: Compete against computer
- Quiz Game: Multiple choice questions
- 2048: Number merging puzzle
- Minesweeper: Classic puzzle game

```
> Make a [game name] game.
> [Brief rules explanation]
```

---

## Challenge Tasks

### Challenge 1: Game Combo

Combine two or more game elements.

```
> Make a typing + reaction speed game.
> Type quickly when a word appears after a random time.
> Measure both reaction time and typing accuracy.
```

### Challenge 2: Multiplayer (Local)

2-player game on the same screen:

```
> Make a 2-player Pong game.
> Left player uses W/S keys, right player uses arrow keys.
> First to 5 points wins.
```

### Challenge 3: AI Battle

Simple computer AI:

```
> Add AI to the tic-tac-toe game.
> Easy: Random moves
> Hard: Always plays optimal moves (minimax algorithm)
```

---

## Publishing Games to the Web

Deploy your games and share with friends.

### Deploy with GitHub Pages (Free)

```bash
# 1. Upload game files to GitHub repository
git add .
git commit -m "Add my awesome game"
git push origin main

# 2. Repository Settings > Pages > Source: main branch
# 3. A few minutes later, access at https://username.github.io/repo-name!
```

### Deploy with Vercel (Free)

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel
```

> **Beginner Tip**: After deploying, share the link with friends. Watching others actually play your game is motivating!

---

## Troubleshooting

Game development can be tricky. Here are common problems and solutions.

### Clicking doesn't do anything

**Possible causes:**
1. Event listener not attached
2. Typo in function name
3. Script loaded before DOM

**Solution:**
```javascript
// Check in browser console (F12)
console.log('Button:', document.getElementById('myButton'))

// Execute after DOM is loaded
document.addEventListener('DOMContentLoaded', () => {
    // Code here
})
```

```
> The button doesn't respond to clicks. Check the event listener.
```

### Game is too fast or too slow

Timing issues are common in games:

```javascript
// Adjust setInterval time (milliseconds)
setInterval(gameLoop, 16)  // About 60fps
setInterval(gameLoop, 33)  // About 30fps

// Or use requestAnimationFrame (recommended)
function gameLoop() {
    update()
    render()
    requestAnimationFrame(gameLoop)
}
```

### Score doesn't update on screen

Variables change but screen doesn't:

```javascript
// Wrong example
score++
// Forgot to update display!

// Correct example
score++
document.getElementById('score').textContent = score
```

### Game state gets messed up

Multiple clicks can cause race conditions:

```javascript
// Use a lock variable
let isProcessing = false

function handleClick() {
    if (isProcessing) return  // Ignore if already processing
    isProcessing = true

    // Processing...

    setTimeout(() => {
        isProcessing = false  // Unlock after completion
    }, 500)
}
```

### Animation is choppy

```javascript
// Use requestAnimationFrame instead of setInterval
function animate() {
    // Update logic
    requestAnimationFrame(animate)
}
```

> **Pro Tip**: `requestAnimationFrame` is called right before the browser draws the next frame, so it's smoother.

### Works on desktop but not on mobile

Touch events are different from click events:

```javascript
// Support both click and touch
element.addEventListener('click', handleInput)
element.addEventListener('touchstart', handleInput)

// Or use pointer events (handles both)
element.addEventListener('pointerdown', handleInput)
```

---

## Common Mistakes

Avoid these game development pitfalls.

### Mistake 1: Starting too big

**Bad approach:**
```
> Make a multiplayer battle royale game with 100 players.
```

**Good approach:**
```
> Make a simple game where clicking increases the score.
```

Start very small and add features one at a time.

### Mistake 2: Forgetting to reset game state

After game over, pressing "Play Again" should reset everything:

```javascript
// Forgot to reset
function playAgain() {
    showGame()  // Oops, score is from the last game!
}

// Correct
function playAgain() {
    score = 0
    timeLeft = 30
    lives = 3
    isGameOver = false
    updateDisplay()
    showGame()
}
```

### Mistake 3: Using setInterval without cleanup

Multiple timers end up running simultaneously:

```javascript
// Wrong - new timer every click!
function startGame() {
    setInterval(tick, 1000)
}

// Correct - clean up existing timer first
let timerId = null
function startGame() {
    if (timerId) clearInterval(timerId)
    timerId = setInterval(tick, 1000)
}
```

### Mistake 4: Not handling edge cases

What happens in these cases:
- Click before game starts?
- Click after game ends?
- Refresh during game?

```javascript
function handleAction() {
    // Check if game is active
    if (!isGameActive) return
    if (isGameOver) return

    // Actual logic
}
```

### Mistake 5: Hardcoding everything

Difficulty adjustment becomes hard:

```javascript
// Hard to adjust
if (score > 100) levelUp()
setTimeout(spawn, 1000)

// Better - use variables/constants
const LEVEL_UP_THRESHOLD = 100
const SPAWN_INTERVAL = 1000

if (score > LEVEL_UP_THRESHOLD) levelUp()
setTimeout(spawn, SPAWN_INTERVAL)
```

> **Beginner Tip**: Defining game "magic numbers" (100, 1000, etc.) as constants makes balance adjustments much easier later.

---

## Terminology

| Term | Meaning |
|------|---------|
| Game Loop | Repeating structure that keeps the game running |
| State | Variables that store the game's current situation |
| Event Listener | Function that detects user input |
| Collision Detection | Logic to check if two objects are touching |
| FPS | Frames Per Second, screen refresh rate |
| requestAnimationFrame | Browser-optimized animation function |
| localStorage | Browser space for permanent data storage |
| Fisher-Yates | Algorithm for randomly shuffling arrays |
| State Machine | Design pattern for managing state transitions |

---

## Next Chapter Preview

Congratulations! You've completed Part 3 (Practical Projects I).

In the next Part, you'll create more practical tools:
- **Chapter 17**: Making CLI Tools - Automation tools that run in the terminal
- **Chapter 18**: Making Chatbots - Discord/Slack bots
- **Chapter 19**: Making Fullstack Apps - Frontend + Backend + Database

The state management, event handling, and asynchronous logic you learned in game development are the foundation for all projects!

---

## Summary

What you learned in this chapter:
- [x] Core game concepts (state, loop, events, collision)
- [x] Making 5 different types of games
- [x] Handling user input
- [x] Managing scores and state
- [x] CSS animation basics
- [x] Game improvement and debugging

[Continue to Chapter 17: Building CLI Tools](../Chapter17-CLI-Tools/README.md)

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
