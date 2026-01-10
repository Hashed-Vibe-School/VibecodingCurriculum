# Chapter 17: API Integration

**English** | [한국어](./README.ko.md)

## What You Will Learn

- What an API is
- Using external APIs
- Real API projects

---

## What is an API?

An API is how programs talk to each other.

Think of it like a restaurant:
- **You** = Client (the one ordering)
- **Waiter** = API (delivers requests)
- **Kitchen** = Server (processes requests)

### Example

How a weather app shows weather data:
1. App requests "Give me Seoul weather" to weather API
2. API delivers to weather server
3. Server responds with data
4. App displays on screen

---

## Making API Calls

### Using fetch

```javascript
// Basic GET request
const response = await fetch('https://api.example.com/data')
const data = await response.json()
console.log(data)
```

### POST Request (Sending Data)

```javascript
const response = await fetch('https://api.example.com/users', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    name: 'John Doe',
    email: 'john@email.com'
  })
})
```

---

## Free Public APIs

Good free APIs for practice:

| API | Description | Auth Required |
|-----|-------------|---------------|
| JSONPlaceholder | Fake data for testing | No |
| OpenWeatherMap | Weather info | Yes (free key) |
| The Cat API | Cat photos | No |
| Pokemon API | Pokemon data | No |
| NASA API | Space data | Yes (free) |

---

## Project 1: Weather App

### Preparation

1. Sign up at [OpenWeatherMap](https://openweathermap.org/api)
2. Get free API key

### Building It

```
> Make a weather app.
> - Enter city name
> - Get weather from OpenWeatherMap API
> - Display temperature, condition, icon
```

### Core Code

```javascript
async function getWeather(city) {
  const API_KEY = 'your-api-key'
  const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}&units=metric`

  const response = await fetch(url)
  const data = await response.json()

  return {
    temp: data.main.temp,
    description: data.weather[0].description,
    icon: data.weather[0].icon
  }
}
```

### Improving It

```
> Show weather based on current location

> Show 5-day forecast too

> Change background color based on weather
```

---

## Project 2: Random Quote App

### Building It

```
> Make a quote app.
> - Show random quote on page load
> - "New Quote" button shows different quote
> - Add copy feature
```

### Using the API

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

## Project 3: Pokemon Pokedex

### Building It

```
> Make a Pokemon Pokedex.
> - Search by number or name
> - Use PokeAPI
> - Show image, types, stats
```

### Using the API

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

### Improving It

```
> Allow search by localized names too

> Show evolution info

> Add favorites feature
```

---

## Project 4: Currency Converter

### Building It

```
> Make a currency converter.
> - Enter amount
> - Convert USD ↔ EUR/JPY/GBP
> - Use real-time rates
```

### Using the API

```javascript
async function getExchangeRate(from, to) {
  const response = await fetch(
    `https://api.exchangerate-api.com/v4/latest/${from}`
  )
  const data = await response.json()

  return data.rates[to]
}

// Usage
const rate = await getExchangeRate('USD', 'EUR')
console.log(`1 USD = ${rate} EUR`)
```

---

## API Error Handling

### Basic Error Handling

```javascript
async function fetchData(url) {
  try {
    const response = await fetch(url)

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }

    return await response.json()
  } catch (error) {
    console.error('API error:', error)
    return null
  }
}
```

### Notifying Users

```
> Show friendly messages for API errors.
> - Network error: "Please check your internet connection"
> - 404: "Data not found"
> - 500: "Server is having issues"
```

---

## Loading State Management

### Showing Loading

```javascript
async function fetchWithLoading() {
  showLoading()  // Start loading

  try {
    const data = await fetchData(url)
    displayData(data)
  } catch (error) {
    showError(error)
  } finally {
    hideLoading()  // End loading
  }
}
```

### Skeleton Loading

```
> Show skeleton UI while data is loading
```

---

## API Key Security

### Be Careful in Frontend

```
// Bad - API key exposed!
const API_KEY = 'sk-1234567890'
fetch(`https://api.example.com?key=${API_KEY}`)
```

### Solutions

1. **Go Through Backend Server**
```
> Create a backend that makes API calls.
> Frontend only calls the backend.
```

2. **Use Serverless Functions**
```
> Create an API proxy with Vercel Edge Function
```

---

## Optimization with Caching

### Prevent Repeated Requests

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

### Using localStorage

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

## Practice: Build an API App

### Basic Task

Complete one of the projects above.

### Extra Challenges

```
> Make an app that shows my GitHub profile using GitHub API

> Make a movie search app (TMDB API)

> Make a news headlines app (News API)
```

---

## Mini Project: API Mashup

Combine multiple APIs to build a comprehensive app.

### Goals

- Integrate multiple APIs
- Use complex data

### Build It: Travel Helper

```
> Create a travel helper app.
> - City search
> - Weather API for current weather
> - Exchange API for local currency
> - Map API for location display
```

### Other Ideas

```
> Create a developer dashboard.
> - GitHub API for my repos
> - Stack Overflow API for popular questions
> - Hacker News API for latest news

> Create a health tracker.
> - Weather API for good exercise days
> - Recipe API for healthy meals
> - Exercise API for workout recommendations
```

### Advanced Challenges (For Experts)

```
> Implement real-time data streaming with WebSocket

> Add rate limiting handling logic

> Create an API response caching layer

> Handle multiple API requests in parallel (Promise.all)
```

---

## Summary

What you learned in this chapter:
- [x] Understanding API concepts
- [x] API calls with fetch
- [x] Various API projects
- [x] Error handling
- [x] Security and optimization

This completes Part 4 (Practical Projects II).

In the next Part, we cover advanced Claude Code features.

[Chapter 18: Advanced Features](../Chapter18/README.md)
