# Chapter 14: Mini Games

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Making games with Claude
- JavaScript game logic
- Completing fun projects

---

## Why Games?

Making games is one of the most engaging ways to learn programming.

Games incorporate all aspects of development:
- Rendering on screen (HTML/CSS)
- Handling user input (keyboard, mouse)
- Processing logic (JavaScript)
- Managing state (score, level)

---

## Game 1: Number Guessing

Start with the simplest game.

### Game Description

- Computer picks a number between 1-100
- Player guesses until correct
- Hints: "Higher" / "Lower"

### Building

```
> Make a number guessing game.
> Guess a number between 1 and 100.
> Show hints too.
```

### Example Result

```html
<div id="game">
    <h1>Number Guessing</h1>
    <p>Guess a number between 1 and 100!</p>

    <input type="number" id="guess" placeholder="Enter number">
    <button onclick="checkGuess()">Check</button>

    <p id="result"></p>
    <p>Attempts: <span id="attempts">0</span></p>
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
            `Correct! You got it in ${attempts} tries!`
    } else if (guess < answer) {
        document.getElementById('result').textContent = 'Higher!'
    } else {
        document.getElementById('result').textContent = 'Lower!'
    }
}
```

### Improvements

```
> Add a restart button
```
State reset pattern - core of any game loop

```
> Save high score
```
`localStorage` based data persistence

```
> Make it look nice with styling
```
Game UI/UX design fundamentals

---

## Game 2: Rock Paper Scissors

Create this classic game.

### Building

```
> Make a rock paper scissors game.
> Play against the computer.
> Show the score too.
```

### Core Logic

```javascript
function play(playerChoice) {
    const choices = ['rock', 'paper', 'scissors']
    const computer = choices[Math.floor(Math.random() * 3)]

    if (playerChoice === computer) {
        return 'Tie!'
    }

    if (
        (playerChoice === 'scissors' && computer === 'paper') ||
        (playerChoice === 'rock' && computer === 'scissors') ||
        (playerChoice === 'paper' && computer === 'rock')
    ) {
        return 'You win!'
    }

    return 'You lose...'
}
```

### Improvements

```
> Show rock paper scissors with emojis
```
Unicode/emoji rendering

```
> Make it best of 5
```
Round-based game logic design

```
> Show win rate statistics
```
Game stats and data visualization

---

## Game 3: Typing Game

A keyboard practice game.

### Building

```
> Make a typing game.
> Type words as fast as they appear.
> 30 second time limit.
> Show the score.
```

### Core Logic

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

### Improvements

```
> Use longer words
```
Content management and arrays

```
> Different word lengths for different difficulty levels
```
Difficulty curve design

```
> Calculate typing speed (WPM)
```
Time-based performance metrics

---

## Game 4: Reaction Time Test

A game that measures reaction speed.

### Building

```
> Make a reaction time test game.
> Click when the screen turns green.
> Show reaction time in milliseconds.
```

### Core Logic

```javascript
let startTime
let waiting = false

function startTest() {
    const box = document.getElementById('box')
    box.style.backgroundColor = 'red'
    box.textContent = 'Wait...'

    // Turn green after random time
    const delay = Math.random() * 3000 + 1000
    setTimeout(() => {
        box.style.backgroundColor = 'green'
        box.textContent = 'Click!'
        startTime = Date.now()
        waiting = true
    }, delay)
}

function handleClick() {
    if (!waiting) return

    const reactionTime = Date.now() - startTime
    document.getElementById('result').textContent =
        `Reaction time: ${reactionTime}ms`
    waiting = false
}
```

### Improvements

```
> Calculate average reaction time (5 tests)
```
Statistical aggregation

```
> Show "Too early!" if clicked before green
```
Input validation and edge cases

```
> Add a leaderboard feature
```
Data sorting and leaderboard UI

---

## Game 5: Memory Card Game

A card matching game.

### Building

```
> Make a memory card game.
> 8 pairs (16 cards).
> Matched pairs disappear.
> Show attempt count.
```

### Core Concept

```javascript
const emojis = ['🎮', '🎯', '🎨', '🎭', '🎪', '🎫', '🎬', '🎤']
let cards = [...emojis, ...emojis]  // 2 of each

function shuffle(array) {
    for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]]
    }
    return array
}

function createBoard() {
    const shuffled = shuffle(cards)
    // Create card UI...
}
```

### Improvements

```
> Add card flip animation
```
CSS transform-based 3D animation

```
> Add a timer
```
`setInterval` timer implementation

```
> Show congratulations message when all matched
```
Win condition detection and modal UI

---

## Game Building Tips

### 1. Start Small

```
# Bad example
> Make an RPG game. With character growth, dungeons, and boss fights.

# Good example
> Make a simple click game first.
> Score goes up when you click.
```

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
```

### 3. Improve with Feedback

```
> Jump is too slow. Make it faster.

> Obstacle spacing is too narrow.

> Background color hurts my eyes. Change it.
```

---

## Practice: Your Own Game

### Basic Task

Pick one of the 5 games above and build it.

### Extra Challenges

```
> Add sound effects to the game
```
Web Audio API for sound feedback

```
> Save high score (localStorage)
```
Browser storage for data persistence

```
> Make it work on mobile too
```
Touch events and responsive layout

### Ideas

Try making other games too:

- Whack-a-mole
- Snake game
- Pong (tennis)
- Tic-tac-toe
- Quiz game

```
> Make a [game name] game
```

---

## Publish Your Game

Deploy your game and share it with others.

```
# Upload to GitHub
> Upload this game to GitHub

# Deploy with Vercel
# (See Chapter 12)
```

Share the link after deployment.

---

## Summary

What you learned in this chapter:
- [x] Simple game logic
- [x] Handling user input
- [x] Managing score and state
- [x] Improving games

Congratulations on completing Part 3 (Practical Projects I).

The next Part covers more practical projects including AI tools and API integration.

Proceed to [Chapter 15: Building AI Tools](../Chapter15/README.md).
