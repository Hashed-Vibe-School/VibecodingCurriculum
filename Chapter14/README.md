# Chapter 14: Mini Games

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Making games with Claude
- JavaScript game logic
- Completing fun projects

---

## Why Do You Need This?

Games aren't just for fun (though they ARE fun). Making games teaches you programming concepts in the most engaging way possible.

**Real-world scenarios where game-building skills help:**

- **Learning programming**: Variables, loops, conditions, functions - games use them ALL
- **Portfolio wow-factor**: A playable game impresses way more than static pages
- **User engagement**: Interactive elements keep visitors on your site longer
- **Problem-solving**: Game logic sharpens your coding brain
- **Interviews**: "I built a game" is a great conversation starter

> Every game mechanic is a programming concept in disguise. Score tracking? That's state management. Hit detection? That's conditional logic. Game loops? That's event handling.

### Simple Analogy: Games as the Gym for Coders

Learning to code by building utilities is like exercising by doing housework - effective, but boring.

Making games is like going to the gym - you're still exercising (learning), but it's actually enjoyable. And just like the gym, you get stronger (better at coding) while having fun.

---

## Try It Yourself: The Simplest Game

Before building complex games, let's make sure the basics work. Here's the simplest possible game:

```
> Create a button that shows a counter.
> Every click increases the counter by 1.
> That's the whole game.
```

Click it a few times. Congratulations, you just made a "clicker game" - the same genre as Cookie Clicker that has millions of players! Everything else is just adding features to this foundation.

---

## Why Games?

Making games is one of the most engaging ways to learn programming.

Games incorporate all aspects of development:
- Rendering on screen (HTML/CSS)
- Handling user input (keyboard, mouse)
- Processing logic (JavaScript)
- Managing state (score, level)

**Game request tips:**

```
> Make a number guessing game. Range 1-100.
> Show attempt count, and display a congratulations message if solved within 10 tries.
```

Describe game rules and desired features specifically for more polished results.

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

## If It Doesn't Work?

Game development can be tricky. Here are common issues and fixes:

### Nothing happens when I click
- Check if the click handler is attached
- Look for errors in browser console (F12)
```
> The button doesn't respond to clicks. Check the event listener.
```

### Game is too fast or too slow
Timing issues are common in games:
```
> The game runs too fast. Slow it down.
```
or
```
> The timer doesn't work correctly. Check setInterval usage.
```

### Score doesn't update on screen
The variable might update, but the display doesn't:
```
> Score increases but the screen doesn't show the new value.
> Check the display update logic.
```

### Game state gets messed up
Multiple clicks can cause race conditions:
```
> I can click the button multiple times when I shouldn't be able to.
> Add a check to prevent this.
```

### Animation is choppy
```
> The animation is not smooth. Help me optimize it.
```

### Works on desktop but not mobile
Touch events are different from click events:
```
> Make this game work on mobile devices with touch input.
```

---

## Common Mistakes

Avoid these game development pitfalls!

### Mistake 1: Starting too big
**Wrong approach:**
```
> Make a multiplayer battle royale game with 100 players.
```

**Better approach:**
```
> Make a simple game where I click to increase a score.
```
Start tiny, add features one by one.

### Mistake 2: Forgetting to reset game state
After game over, clicking "Play Again" should reset everything:
```javascript
// Forgot to reset
function playAgain() {
    showGame()  // Oops, score is still from last game!
}

// Correct
function playAgain() {
    score = 0
    timeLeft = 30
    updateDisplay()
    showGame()
}
```

### Mistake 3: Using setInterval without clearing it
This causes multiple timers running at once:
```javascript
// Wrong - new timer each click!
function startGame() {
    setInterval(tick, 1000)
}

// Correct - clear old timer first
let timerId
function startGame() {
    if (timerId) clearInterval(timerId)
    timerId = setInterval(tick, 1000)
}
```

### Mistake 4: Not handling edge cases
What happens when:
- User clicks before game starts?
- User clicks after game ends?
- User refreshes mid-game?

```
> Add checks to prevent clicking when the game isn't active.
```

### Mistake 5: Hardcoding everything
Makes it hard to adjust difficulty:
```javascript
// Hard to adjust
if (score > 100) levelUp()

// Better - use variables
const LEVEL_UP_THRESHOLD = 100
if (score > LEVEL_UP_THRESHOLD) levelUp()
```

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
