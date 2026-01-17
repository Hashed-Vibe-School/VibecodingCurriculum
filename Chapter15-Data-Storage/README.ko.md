# Chapter 15: 데이터 저장

[English](./README.md) | **한국어**

---

## 챕터 연결

| 이전 | 현재 | 다음 |
|------|------|------|
| [Chapter 14: 배포](../Chapter14-Deployment/README.ko.md) | **Chapter 15: 데이터 저장** | [Chapter 16: 미니 게임](../Chapter16-Mini-Games/README.ko.md) |

**선수 지식**: Chapter 13(웹사이트 만들기)의 JavaScript 기초를 이해하고 있어야 합니다.

---

## 💬 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이 챕터에서 배우는 것

- 데이터 저장이 왜 필요한지
- localStorage로 브라우저에 데이터 저장하기
- CRUD 개념 이해하기
- 실제 앱에서 데이터 관리하기
- JSON으로 복잡한 데이터 다루기
- 데이터 저장의 한계와 대안

---

## 왜 필요합니까?

온라인에서 폼을 작성하다가 실수로 탭을 닫아본 적이 있습니까? 열심히 쓴 내용이 다 사라졌을 때의 그 공포를 느껴본 적이 있을 것입니다. 이것이 바로 데이터 저장 없이 일어나는 일입니다.

**데이터 저장이 필요한 실제 상황:**

- **할 일 앱**: 새로고침해도 할 일 목록이 유지되어야 함
- **사용자 설정**: 다크 모드 설정이 매번 초기화되면 안 됨
- **장바구니**: 쇼핑하는 동안 담은 물건이 남아있어야 함
- **게임 진행**: 최고 점수와 해금한 레벨이 저장되어야 함
- **임시 저장**: 반쯤 쓴 글이 사라지면 안 됨

> 데이터 저장 없이는, 당신의 앱은 금붕어 같은 기억력을 갖습니다 - 눈 돌리는 순간 모든 걸 잊어버립니다.

### 쉬운 비유: 공책 vs 화이트보드

화이트보드는 빠르게 메모하기 좋지만, 지우거나 방을 나가면 모든 게 사라집니다. 데이터 저장 없는 앱이 이렇습니다.

공책은 덮어도 메모가 남아있습니다. 데이터 저장이 있는 앱이 이렇습니다. localStorage는 앱의 공책입니다.

### 데이터의 생명주기 이해하기

```
┌─────────────────────────────────────────────────────────────┐
│                    데이터의 생명주기                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   [사용자 입력] → [메모리(RAM)] → [저장소(localStorage)]     │
│        ↓              ↓                  ↓                  │
│     키보드        휘발성           영구 저장                 │
│     마우스      (새로고침→소멸)   (브라우저 닫아도 유지)      │
│                                                             │
│   💡 메모리는 빠르지만 임시적, 저장소는 느리지만 영구적      │
└─────────────────────────────────────────────────────────────┘
```

💡 **비전공자 팁**: 컴퓨터의 메모리(RAM)는 작업대입니다. 넓고 빠르지만 전원이 꺼지면 치워집니다. 저장소(하드디스크, localStorage)는 서랍입니다. 꺼내는 데 시간이 걸리지만 오래 보관됩니다.

---

## 따라해보세요: 한 가지만 저장하고 불러오기

완전한 앱을 만들기 전에, 가장 간단한 예제로 데이터 저장이 어떻게 동작하는지 봅시다.

```
> HTML 페이지 만들어줘. 입력 필드 하나랑 "저장" 버튼만 있게.
> 저장 버튼 누르면 입력값을 localStorage에 저장해.
> 페이지 로드될 때 저장된 값을 입력 필드에 보여줘.
```

뭔가 입력하고, 저장 누르고, 페이지 새로고침하세요. 텍스트가 그대로 있는 것을 확인할 수 있습니다. 이것이 바로 localStorage의 기능입니다.

### 실습: 직접 해보기

브라우저 개발자 도구(F12)를 열고 Console 탭에서 직접 테스트해보세요:

```javascript
// 1. 저장하기
localStorage.setItem('myName', '홍길동')

// 2. 확인하기
console.log(localStorage.getItem('myName'))  // '홍길동' 출력

// 3. 새로고침 후 다시 확인
// (페이지를 새로고침해도 데이터가 남아있습니다)

// 4. 삭제하기
localStorage.removeItem('myName')

// 5. 모두 삭제하기
localStorage.clear()
```

🔥 **프로 팁**: 개발자 도구의 Application 탭에서 localStorage를 시각적으로 확인하고 직접 수정할 수 있습니다. 디버깅할 때 매우 유용합니다.

---

## 왜 데이터 저장이 필요합니까?

지금까지 만든 웹사이트는 "정적"입니다. 페이지를 새로고침하면 입력한 내용이 사라집니다.

하지만 실제 앱은 데이터가 유지되어야 합니다:
- 할 일 목록 → 새로고침해도 유지
- 메모 앱 → 다음에 다시 열어도 내용 있음
- 설정 → 사용자가 바꾼 설정 기억

이런 걸 하려면 **데이터 저장**이 필요합니다.

### 실제 서비스에서의 데이터 저장

| 서비스 | 저장하는 데이터 | 저장 위치 |
|--------|----------------|-----------|
| Gmail | 이메일, 첨부파일 | 서버 (데이터베이스) |
| 카카오톡 | 메시지, 사진 | 서버 + 로컬 |
| 메모장 | 텍스트 파일 | 로컬 (하드디스크) |
| 웹 게임 | 점수, 레벨 | localStorage |
| YouTube | 시청 기록 | 서버 + 쿠키 |

💡 **비전공자 팁**: 대부분의 앱은 여러 저장 방식을 조합합니다. 예를 들어 카카오톡은 최근 메시지는 로컬에, 전체 기록은 서버에 저장합니다. 이렇게 하면 오프라인에서도 최근 메시지를 볼 수 있습니다.

---

## 데이터 저장 방법들

데이터를 저장하는 방법은 여러 가지가 있습니다:

| 방법 | 특징 | 용도 | 난이도 |
|------|------|------|--------|
| **localStorage** | 브라우저에 저장, 간단함 | 개인용 앱, 학습용 | 쉬움 |
| **sessionStorage** | 탭 닫으면 삭제 | 임시 데이터 | 쉬움 |
| **IndexedDB** | 대용량, 복잡함 | 오프라인 앱 | 어려움 |
| **Supabase/Firebase** | 서버에 저장 | 여러 사람이 쓰는 앱 | 중간 |
| **파일** | 직접 파일로 저장 | 데스크톱 앱 | 중간 |

### 저장 방식 비교 도표

```
┌─────────────────────────────────────────────────────────────┐
│                    저장 방식 선택 가이드                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  질문 1: 다른 사람도 데이터를 봐야 합니까?                    │
│     ├─ YES → 서버 저장 필요 (Supabase, Firebase)            │
│     └─ NO → 로컬 저장 가능                                  │
│                                                             │
│  질문 2: 다른 기기에서도 접근해야 합니까?                     │
│     ├─ YES → 서버 저장 필요                                 │
│     └─ NO → localStorage 가능                              │
│                                                             │
│  질문 3: 5MB 이상 저장해야 합니까?                           │
│     ├─ YES → IndexedDB 또는 서버                           │
│     └─ NO → localStorage로 충분                            │
│                                                             │
│  질문 4: 브라우저 닫으면 삭제되어야 합니까?                   │
│     ├─ YES → sessionStorage                                │
│     └─ NO → localStorage                                   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

이번 챕터에서는 **localStorage**를 사용합니다. 가입이 필요 없고, 바로 사용할 수 있습니다.

---

## localStorage란?

localStorage는 브라우저가 제공하는 저장 공간입니다.

### 특징

- **무료**: 별도 서비스 가입 불필요
- **간단**: JavaScript 몇 줄로 사용
- **영구적**: 브라우저를 닫아도 데이터 유지
- **용량**: 약 5MB (대부분의 앱에 충분)
- **동기적**: 저장/읽기가 즉시 완료됨
- **문자열 전용**: 객체는 JSON으로 변환 필요

### localStorage vs sessionStorage vs 쿠키

| 특성 | localStorage | sessionStorage | 쿠키 |
|------|-------------|----------------|------|
| 용량 | 5-10MB | 5-10MB | 4KB |
| 만료 | 영구 | 탭 닫으면 삭제 | 설정 가능 |
| 서버 전송 | X | X | O (매 요청) |
| 접근 | JavaScript | JavaScript | JavaScript + 서버 |

### 어떻게 작동합니까?

```javascript
// 저장하기 - 키와 값을 짝으로 저장
localStorage.setItem('이름', '값')

// 읽어오기 - 키로 값을 찾음
localStorage.getItem('이름')

// 삭제하기 - 특정 키 삭제
localStorage.removeItem('이름')

// 모두 삭제하기 - 전체 초기화
localStorage.clear()

// 저장된 항목 개수
localStorage.length

// n번째 키 이름 가져오기
localStorage.key(0)
```

💡 **비전공자 팁**: localStorage는 사물함과 같습니다. 사물함 번호(키)로 물건(값)을 넣고 빼는 것입니다. 모든 사물함에는 이름표(키)가 있어야 합니다.

### 더 다양한 대화 예시

**기본 저장 요청:**

```
> 사용자 이름을 localStorage에 저장해줘
```

**조건부 저장 요청:**

```
> 입력 필드가 비어있지 않을 때만 저장하게 해줘
```

**여러 데이터 저장:**

```
> 사용자 설정(테마, 언어, 폰트 크기)을 한 번에 저장하고 불러오게 해줘
```

**저장 확인 요청:**

```
> 저장 성공하면 "저장되었습니다" 알림 보여줘
```

**자동 저장 요청:**

```
> 텍스트 입력할 때마다 자동으로 localStorage에 저장해줘.
> 1초 딜레이 주고 (디바운스 적용).
```

어떤 데이터를 저장하고 싶은지 명확히 하면 적절한 코드가 나옵니다.

---

## JSON: 복잡한 데이터 저장하기

localStorage는 문자열만 저장할 수 있습니다. 그런데 우리는 보통 배열이나 객체를 저장해야 합니다.

### 문제 상황

```javascript
// 객체를 그냥 저장하면?
const user = { name: '홍길동', age: 25 }
localStorage.setItem('user', user)

// 읽어보면...
console.log(localStorage.getItem('user'))
// 출력: "[object Object]"  <- 원하는 결과가 아님!
```

### 해결책: JSON

```javascript
// JSON.stringify: 객체 → 문자열
const user = { name: '홍길동', age: 25 }
const jsonString = JSON.stringify(user)
// 결과: '{"name":"홍길동","age":25}'

localStorage.setItem('user', jsonString)

// JSON.parse: 문자열 → 객체
const savedString = localStorage.getItem('user')
const savedUser = JSON.parse(savedString)
// 결과: { name: '홍길동', age: 25 }
```

### JSON이란?

**J**ava**S**cript **O**bject **N**otation의 약자입니다.

```
┌─────────────────────────────────────────────────────────────┐
│                    JSON 변환 과정                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   JavaScript 객체         JSON 문자열          localStorage │
│   { name: "홍길동" }  ─→  '{"name":"홍길동"}'  ─→  저장!     │
│                    stringify                                │
│                                                             │
│   JavaScript 객체         JSON 문자열          localStorage │
│   { name: "홍길동" }  ←─  '{"name":"홍길동"}'  ←─  읽기!     │
│                    parse                                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 다양한 데이터 타입의 JSON 변환

```javascript
// 배열
const fruits = ['사과', '바나나', '오렌지']
localStorage.setItem('fruits', JSON.stringify(fruits))
// 저장: '["사과","바나나","오렌지"]'

// 복잡한 객체
const settings = {
  theme: 'dark',
  language: 'ko',
  notifications: {
    email: true,
    push: false
  },
  favorites: [1, 5, 12]
}
localStorage.setItem('settings', JSON.stringify(settings))

// 읽을 때
const loadedSettings = JSON.parse(localStorage.getItem('settings'))
console.log(loadedSettings.theme)  // 'dark'
console.log(loadedSettings.notifications.email)  // true
```

⚠️ **주의사항**: JSON으로 변환할 수 없는 것들이 있습니다.
- 함수
- undefined
- Symbol
- 순환 참조가 있는 객체

🔥 **프로 팁**: 날짜(Date) 객체는 문자열로 변환됩니다. 읽을 때 `new Date(string)`으로 다시 Date 객체로 만들어야 합니다.

---

## CRUD: 데이터의 4가지 기본 작업

CRUD는 데이터를 다루는 기본 패턴입니다:

- **C**reate: 만들기 (새 데이터 추가)
- **R**ead: 읽기 (저장된 데이터 가져오기)
- **U**pdate: 수정하기 (기존 데이터 변경)
- **D**elete: 삭제하기 (데이터 제거)

거의 모든 앱이 이 4가지 작업의 조합입니다. 할 일 앱도, 메모 앱도, SNS도 결국 CRUD입니다.

### CRUD의 실생활 예시

| 앱 | Create | Read | Update | Delete |
|----|--------|------|--------|--------|
| 메모 | 새 메모 작성 | 메모 목록 보기 | 메모 수정 | 메모 삭제 |
| 연락처 | 연락처 추가 | 연락처 검색 | 전화번호 수정 | 연락처 삭제 |
| SNS | 게시물 작성 | 피드 보기 | 게시물 수정 | 게시물 삭제 |
| 이메일 | 메일 작성 | 받은편지함 | 임시저장 편집 | 메일 삭제 |

### CRUD와 localStorage 메서드 매핑

```javascript
// Create (만들기)
localStorage.setItem('newItem', 'value')

// Read (읽기)
const data = localStorage.getItem('newItem')

// Update (수정하기) - 같은 키로 덮어쓰기
localStorage.setItem('newItem', 'new value')

// Delete (삭제하기)
localStorage.removeItem('newItem')
```

💡 **비전공자 팁**: CRUD는 마법의 단어입니다. 어떤 앱을 만들든 "이 앱의 CRUD는 뭐지?"라고 생각하면 기능 설계가 쉬워집니다.

---

## 🎯 미니 퀴즈

### 퀴즈 1: localStorage 기초
localStorage.setItem('count', 5)를 실행한 후 localStorage.getItem('count')의 결과는?

A) 5 (숫자)
B) "5" (문자열)
C) undefined
D) null

<details>
<summary>정답 보기</summary>

**정답: B) "5" (문자열)**

localStorage는 모든 것을 문자열로 저장합니다. 숫자 5를 저장해도 "5"로 변환됩니다.
숫자로 사용하려면 `parseInt(localStorage.getItem('count'))` 또는 `Number(localStorage.getItem('count'))`로 변환해야 합니다.

</details>

### 퀴즈 2: JSON 변환
다음 코드의 문제점은?

```javascript
const todos = localStorage.getItem('todos')
todos.push({ text: '새 할 일' })
```

A) localStorage에 'todos' 키가 없을 수 있음
B) todos가 문자열이라 push를 사용할 수 없음
C) 객체를 직접 push할 수 없음
D) A와 B 모두

<details>
<summary>정답 보기</summary>

**정답: D) A와 B 모두**

1. `localStorage.getItem('todos')`는 문자열을 반환합니다. 배열 메서드 `push()`를 사용하려면 먼저 `JSON.parse()`로 변환해야 합니다.
2. 키가 없으면 `null`을 반환하고, `null.push()`는 에러가 납니다.

올바른 코드:
```javascript
const todos = JSON.parse(localStorage.getItem('todos')) || []
todos.push({ text: '새 할 일' })
localStorage.setItem('todos', JSON.stringify(todos))
```

</details>

### 퀴즈 3: CRUD 작업 식별
"사용자가 프로필 사진을 변경한다"는 CRUD 중 어떤 작업입니까?

A) Create
B) Read
C) Update
D) Delete

<details>
<summary>정답 보기</summary>

**정답: C) Update**

기존 프로필 사진을 새 사진으로 "수정"하는 것이므로 Update 작업입니다.
만약 프로필 사진을 처음 등록하는 것이라면 Create, 삭제하는 것이라면 Delete가 됩니다.

</details>

---

## 실습: 할 일 목록 앱 만들기

localStorage를 사용해서 실제 할 일 목록 앱을 만들어봅시다.

### 전체 구조 미리보기

```
┌─────────────────────────────────────────────────────────────┐
│                     할 일 목록 앱 구조                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   [HTML]              [JavaScript]           [localStorage] │
│   - 입력 필드         - addTodo()            - 'todos' 키   │
│   - 추가 버튼         - getTodos()           - JSON 배열    │
│   - 할 일 목록        - toggleTodo()                        │
│   - 삭제 버튼         - deleteTodo()                        │
│                       - displayTodos()                      │
│                                                             │
│   사용자 입력 → JavaScript 처리 → localStorage 저장         │
│                       ↓                                     │
│              화면에 목록 표시 ← localStorage에서 읽기        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 1단계: 기본 구조 만들기

```
> 할 일 목록 앱 만들어줘.
> HTML, CSS, JavaScript로.
> 할 일 추가, 목록 보기, 삭제 기능 포함.
> localStorage로 데이터 저장.
```

### 2단계: Create - 할 일 추가

```
> 할 일 추가 기능 구현해줘
```

Claude가 만드는 코드:

```javascript
function addTodo(text) {
  // 빈 입력 체크
  if (!text.trim()) {
    alert('할 일을 입력해주세요!')
    return
  }

  // 기존 할 일 목록 가져오기
  // || [] 는 저장된 게 없을 때 빈 배열을 사용한다는 뜻
  const todos = JSON.parse(localStorage.getItem('todos')) || []

  // 새 할 일 객체 만들기
  const newTodo = {
    id: Date.now(),       // 현재 시간을 ID로 사용 (고유값 보장)
    text: text,           // 할 일 내용
    done: false,          // 완료 여부
    createdAt: new Date().toISOString()  // 생성 시간
  }

  // 배열에 추가
  todos.push(newTodo)

  // localStorage에 저장
  localStorage.setItem('todos', JSON.stringify(todos))

  // 화면 갱신
  displayTodos()
}
```

💡 **비전공자 팁**: `Date.now()`는 1970년 1월 1일부터 지금까지의 밀리초를 반환합니다. 매 순간 다른 숫자가 나오므로 고유 ID로 쓰기 좋습니다.

### 3단계: Read - 할 일 불러오기

```
> 저장된 할 일 목록 불러오는 기능 만들어줘
```

```javascript
// 할 일 목록 가져오기
function getTodos() {
  // JSON.parse가 null에서 에러나지 않도록 처리
  const saved = localStorage.getItem('todos')
  if (!saved) return []

  try {
    return JSON.parse(saved)
  } catch (error) {
    console.error('할 일 목록 파싱 에러:', error)
    return []  // 에러나면 빈 배열 반환
  }
}

// 화면에 표시하기
function displayTodos() {
  const todos = getTodos()
  const container = document.getElementById('todo-list')

  // 할 일이 없을 때 메시지
  if (todos.length === 0) {
    container.innerHTML = '<p class="empty-message">할 일이 없습니다. 새로운 할 일을 추가해보세요!</p>'
    return
  }

  // 할 일 목록 HTML 생성
  container.innerHTML = todos.map(todo => `
    <div class="todo-item ${todo.done ? 'completed' : ''}" data-id="${todo.id}">
      <input type="checkbox" ${todo.done ? 'checked' : ''}
             onchange="toggleTodo(${todo.id})">
      <span class="todo-text">${todo.text}</span>
      <button onclick="deleteTodo(${todo.id})" class="delete-btn">삭제</button>
    </div>
  `).join('')
}

// 페이지 로드 시 실행
window.onload = function() {
  displayTodos()
}
```

⚠️ **주의사항**: `JSON.parse()`는 잘못된 JSON 문자열에서 에러를 발생시킵니다. try-catch로 감싸는 것이 안전합니다.

### 4단계: Update - 완료 표시

```
> 할 일 완료 체크 기능 추가해줘
```

```javascript
function toggleTodo(id) {
  const todos = getTodos()

  // 해당 ID의 할 일 찾기
  const todoIndex = todos.findIndex(t => t.id === id)

  if (todoIndex === -1) {
    console.error('할 일을 찾을 수 없습니다:', id)
    return
  }

  // 완료 상태 토글 (true ↔ false)
  todos[todoIndex].done = !todos[todoIndex].done

  // 완료 시간 기록
  if (todos[todoIndex].done) {
    todos[todoIndex].completedAt = new Date().toISOString()
  } else {
    delete todos[todoIndex].completedAt
  }

  // 저장
  localStorage.setItem('todos', JSON.stringify(todos))

  // 화면 갱신
  displayTodos()
}
```

🔥 **프로 팁**: `find()`는 조건에 맞는 첫 번째 요소를 반환하고, `findIndex()`는 그 요소의 인덱스를 반환합니다. 수정할 때는 인덱스가 필요합니다.

### 5단계: Delete - 삭제

```
> 할 일 삭제 기능 추가해줘
```

```javascript
function deleteTodo(id) {
  // 삭제 확인
  if (!confirm('정말 삭제하시겠습니까?')) {
    return
  }

  const todos = getTodos()

  // 해당 ID를 제외한 새 배열 만들기
  const filtered = todos.filter(t => t.id !== id)

  // 저장
  localStorage.setItem('todos', JSON.stringify(filtered))

  // 화면 갱신
  displayTodos()
}

// 완료된 할 일 일괄 삭제
function deleteCompletedTodos() {
  const todos = getTodos()
  const completedCount = todos.filter(t => t.done).length

  if (completedCount === 0) {
    alert('완료된 할 일이 없습니다.')
    return
  }

  if (!confirm(`완료된 ${completedCount}개의 할 일을 삭제하시겠습니까?`)) {
    return
  }

  const remaining = todos.filter(t => !t.done)
  localStorage.setItem('todos', JSON.stringify(remaining))
  displayTodos()
}
```

💡 **비전공자 팁**: `filter()`는 조건에 맞는 요소만 모아 새 배열을 만듭니다. 원본 배열은 변하지 않습니다.

---

## 더 다양한 대화 예시

### 기능 추가 요청

```
> 할 일에 우선순위(높음/중간/낮음) 추가해줘
```

**Claude의 응답 예시:**
```javascript
const newTodo = {
  id: Date.now(),
  text: text,
  done: false,
  priority: 'medium'  // 'high', 'medium', 'low'
}
```

### 정렬 기능 요청

```
> 우선순위별로 정렬해줘. 높음이 맨 위에 오게.
```

**Claude의 응답 예시:**
```javascript
function sortByPriority(todos) {
  const order = { high: 0, medium: 1, low: 2 }
  return todos.sort((a, b) => order[a.priority] - order[b.priority])
}
```

### 검색 기능 요청

```
> 할 일 검색 기능 추가해줘.
> 입력할 때마다 실시간으로 필터링되게.
```

### 마감일 추가 요청

```
> 각 할 일에 마감일을 추가해줘.
> 마감일이 지난 항목은 빨간색으로 표시.
> 오늘까지인 항목은 주황색으로.
```

### 통계 기능 요청

```
> 할 일 통계 보여줘.
> - 전체 개수
> - 완료된 개수
> - 미완료 개수
> - 완료율
```

### 에러 상황 해결 대화

```
> 할 일 저장이 안 돼. 콘솔에 에러 없는데.
```

**Claude의 응답:** "몇 가지 확인해봅시다..."

```
> 새로고침하면 추가한 할 일이 다 사라져
```

**Claude의 응답:** "localStorage.setItem()이 호출되는지 확인해봅시다..."

---

## 피드백으로 개선하기

기본 기능이 되면 피드백으로 개선합니다:

### 시각적 개선

```
> 완료된 할 일은 회색으로 표시하고 취소선 그어줘

> 우선순위별로 색깔 다르게 해줘 (높음: 빨강, 중간: 노랑, 낮음: 초록)

> 드래그 앤 드롭으로 순서 바꿀 수 있게 해줘
```

### UX 개선

```
> 할 일이 없을 때 "할 일이 없습니다" 메시지 보여줘

> 엔터키 누르면 할 일 추가되게 해줘

> 할 일 추가 후 입력 필드 자동 비우기

> 삭제 전에 확인 물어보게 해줘
```

### 고급 기능

```
> 완료된 것만 한 번에 삭제하는 버튼 추가해줘

> 할 일을 카테고리별로 분류할 수 있게 해줘

> 다크 모드 지원해줘

> 데이터 내보내기/가져오기 기능 추가해줘
```

---

## 개발자 도구로 확인하기

localStorage에 저장된 데이터를 확인하려면:

1. 브라우저에서 F12 (개발자 도구)
2. Application 탭 선택 (Chrome) / Storage 탭 (Firefox)
3. 왼쪽 메뉴에서 Local Storage 클릭
4. 사이트 URL 선택

저장된 데이터를 직접 볼 수 있습니다.

### 개발자 도구에서 할 수 있는 것

| 작업 | 방법 |
|------|------|
| 값 보기 | 키 클릭 |
| 값 수정 | 값 더블클릭 후 편집 |
| 항목 삭제 | 항목 우클릭 > Delete |
| 전체 삭제 | 우클릭 > Clear |
| 새 항목 추가 | 빈 공간 더블클릭 |

🔥 **프로 팁**: 개발 중에 localStorage가 꼬이면 `localStorage.clear()`로 깨끗이 지우고 다시 시작하세요.

### Console에서 직접 조작하기

```javascript
// 저장된 데이터 보기 (예쁘게 출력)
console.log(JSON.parse(localStorage.getItem('todos')))

// 특정 할 일만 보기
const todos = JSON.parse(localStorage.getItem('todos'))
console.table(todos)  // 표 형태로 보기

// 데이터 강제 수정 (테스트용)
const todos = JSON.parse(localStorage.getItem('todos'))
todos[0].text = '수정된 할 일'
localStorage.setItem('todos', JSON.stringify(todos))
```

---

## 언제 localStorage를 쓰고, 언제 데이터베이스를 사용합니까?

### localStorage가 좋을 때

- 개인용 앱 (나만 쓸 때)
- 설정 저장 (테마, 언어, 폰트 크기 등)
- 간단한 메모, 할 일 목록
- 학습용 프로젝트
- 오프라인에서도 동작해야 할 때
- 캐시용 데이터 (API 응답 임시 저장)

### 데이터베이스가 필요할 때

- 여러 사람이 함께 쓸 때 (협업 앱)
- 다른 기기에서도 접근해야 할 때 (동기화)
- 데이터가 많을 때 (5MB 이상)
- 사용자 인증이 필요할 때
- 데이터 분석, 통계가 필요할 때
- 백업/복구가 중요할 때

### 단계별 성장 경로

```
┌─────────────────────────────────────────────────────────────┐
│                    데이터 저장 성장 경로                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  [초급]           [중급]              [고급]                 │
│  localStorage  →  Supabase/Firebase  →  직접 서버 구축      │
│                                                             │
│  - 개인 프로젝트   - 사용자 인증        - 완전한 제어권       │
│  - 학습용         - 실시간 동기화       - 복잡한 쿼리         │
│  - 프로토타입     - 여러 기기 지원      - 대용량 데이터       │
│                   - 팀 협업            - 맞춤형 최적화        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

데이터베이스가 필요해지면 Supabase, Firebase 같은 서비스를 사용하면 됩니다. CRUD 개념은 똑같이 적용됩니다.

```
> Supabase로 할 일 앱 데이터를 저장하게 바꿔줘
```

💡 **비전공자 팁**: 처음엔 localStorage로 시작하고, 앱이 성장하면 데이터베이스로 마이그레이션하면 됩니다. CRUD 개념을 알면 전환이 쉽습니다.

---

## 📝 실습 과제

### 난이도 1: 메모 앱

```
> 메모 앱 만들어줘.
> - 메모 작성, 수정, 삭제
> - localStorage 저장
> - 제목으로 검색
```

**성공 기준:**
- [ ] 새 메모를 작성할 수 있다
- [ ] 기존 메모를 수정할 수 있다
- [ ] 메모를 삭제할 수 있다
- [ ] 새로고침해도 메모가 유지된다
- [ ] 제목으로 메모를 검색할 수 있다

### 난이도 2: 다크 모드 설정 저장

```
> 다크 모드 토글 만들어줘.
> 사용자가 선택한 테마를 localStorage에 저장해서
> 다음에 열 때도 유지되게.
> 시스템 설정도 감지해줘.
```

**성공 기준:**
- [ ] 다크/라이트 모드 전환 버튼이 있다
- [ ] 선택한 테마가 localStorage에 저장된다
- [ ] 페이지 새로고침해도 선택한 테마가 유지된다
- [ ] 시스템 테마를 기본값으로 사용한다
- [ ] 부드러운 전환 애니메이션이 있다

### 난이도 3: 습관 트래커

```
> 일일 습관 체크 앱 만들어줘.
> - 습관 목록 관리 (추가, 수정, 삭제)
> - 오늘 완료한 습관 체크
> - 날짜별 완료 기록
> - 주간/월간 통계 보여주기
> - 연속 달성일(스트릭) 표시
```

**성공 기준:**
- [ ] 습관을 추가, 수정, 삭제할 수 있다
- [ ] 각 습관의 오늘 완료 여부를 체크할 수 있다
- [ ] 과거 날짜의 기록도 볼 수 있다
- [ ] 주간 완료율을 그래프로 볼 수 있다
- [ ] 연속 달성일이 표시된다
- [ ] 새로고침해도 모든 데이터가 유지된다

---

## 🏆 도전 과제

### 도전 1: 데이터 백업/복원

```
> localStorage 데이터를 JSON 파일로 내보내기/가져오기 기능 추가해줘.
> 백업과 복원 용도로.
```

**힌트:**
- 파일 다운로드: Blob과 URL.createObjectURL 사용
- 파일 업로드: input type="file"과 FileReader 사용

### 도전 2: 데이터 동기화 시뮬레이션

```
> 두 개의 브라우저 탭에서 같은 앱을 열었을 때,
> 한쪽에서 데이터를 변경하면 다른 쪽에도 반영되게 해줘.
> (localStorage의 storage 이벤트 사용)
```

### 도전 3: 용량 관리

```
> localStorage 사용량을 보여주고,
> 80% 이상 사용하면 경고 메시지 표시.
> 오래된 데이터 자동 정리 기능도 추가.
```

---

## 안 되면?

데이터 저장은 까다로울 수 있습니다. 자주 발생하는 문제와 해결법입니다.

### 새로고침하면 데이터가 사라집니다

**체크리스트:**
1. `localStorage.setItem()`을 실제로 호출했습니까?
2. 브라우저 콘솔에서 에러를 확인했습니까? (F12 > Console)
3. 저장 후 `localStorage.getItem()`으로 확인했습니까?

```
> 데이터가 저장 안 돼. localStorage 코드 확인해줘.
```

**디버깅 방법:**
```javascript
// 저장 전후에 로그 추가
console.log('저장 전:', localStorage.getItem('todos'))
localStorage.setItem('todos', JSON.stringify(todos))
console.log('저장 후:', localStorage.getItem('todos'))
```

### JSON.parse 에러

저장된 데이터가 유효한 JSON이 아닐 때 발생합니다:

```
Uncaught SyntaxError: Unexpected token 'u' at position 0
```

이 에러는 보통 `undefined`를 파싱하려 할 때 발생합니다.

```
> localStorage 데이터 파싱할 때 "Unexpected token" 에러 나와.
> 어떻게 고쳐?
```

**해결책:**
```javascript
// 안전한 파싱 함수
function safeGetItem(key, defaultValue = null) {
  try {
    const item = localStorage.getItem(key)
    return item ? JSON.parse(item) : defaultValue
  } catch (error) {
    console.error(`${key} 파싱 에러:`, error)
    return defaultValue
  }
}
```

빠른 해결: localStorage 지우고 새로 시작:
```javascript
localStorage.clear()
```

### 데이터가 "[object Object]"로 보입니다

저장하기 전에 stringify를 깜빡한 경우입니다:

```javascript
// 잘못됨 - 객체가 문자열로 변환될 때 [object Object]가 됨
localStorage.setItem('data', myObject)

// 올바름 - JSON 문자열로 변환 후 저장
localStorage.setItem('data', JSON.stringify(myObject))
```

### localStorage가 꽉 찼습니다

localStorage는 5MB 제한이 있습니다.

```
> localStorage가 꽉 찼어. 뭐가 공간 차지하는지 확인하고
> 불필요한 데이터 정리하는 것 도와줘.
```

**용량 확인 방법:**
```javascript
// 전체 사용량 확인
function getLocalStorageSize() {
  let total = 0
  for (let key in localStorage) {
    if (localStorage.hasOwnProperty(key)) {
      total += localStorage[key].length * 2  // UTF-16이므로 2배
    }
  }
  return (total / 1024 / 1024).toFixed(2) + ' MB'
}

console.log('localStorage 사용량:', getLocalStorageSize())

// 키별 용량 확인
for (let key in localStorage) {
  if (localStorage.hasOwnProperty(key)) {
    const size = (localStorage[key].length * 2 / 1024).toFixed(2)
    console.log(`${key}: ${size} KB`)
  }
}
```

### 로컬에서는 되는데 배포 후에는 안 됩니다

localStorage는 브라우저별, 도메인별로 분리됩니다.

- localhost의 localStorage ≠ mysite.com의 localStorage
- Chrome의 localStorage ≠ Firefox의 localStorage
- 본인 컴퓨터의 데이터가 다른 사람 브라우저에 나타나지 않는 것은 정상입니다

### 개발자 도구에서 localStorage가 안 보입니다

- 올바른 도메인에 있는지 확인하시기 바랍니다
- Application 탭 > Local Storage > 사이트 URL을 선택하시기 바랍니다
- "file://" 프로토콜에서는 localStorage가 작동하지 않을 수 있습니다 (Live Server 사용)

### 시크릿 모드에서 안 됩니다

시크릿(프라이빗) 브라우징 모드에서는 localStorage가 제한될 수 있습니다:
- 저장은 되지만 창을 닫으면 삭제됨
- 일부 브라우저에서는 완전히 차단됨

---

## 자주 하는 실수

이런 함정을 피하시기 바랍니다.

### 실수 1: 읽을 때 parse 깜빡하기

```javascript
// 잘못됨 - 이건 문자열이지 배열이 아닙니다!
const todos = localStorage.getItem('todos')
todos.push(newTodo)  // 에러! push is not a function

// 올바름
const todos = JSON.parse(localStorage.getItem('todos')) || []
todos.push(newTodo)
```

### 실수 2: null/빈 값 처리 안 하기

처음 사용하는 유저는 데이터가 없습니다:

```javascript
// 저장된 게 없으면 크래시
const todos = JSON.parse(localStorage.getItem('todos'))
// null을 파싱하면 에러!

// 안전한 방법 - 빈 배열을 기본값으로
const todos = JSON.parse(localStorage.getItem('todos')) || []
```

### 실수 3: 키 이름 잘못 쓰기

키는 대소문자 구분하고 정확해야 합니다:

```javascript
localStorage.setItem('todos', data)
localStorage.getItem('Todos')  // null 반환! (대문자 T)
localStorage.getItem('todo')   // null 반환! (s 빠짐)
```

🔥 **프로 팁**: 키 이름은 상수로 정의해서 사용하면 실수를 줄일 수 있습니다.

```javascript
const STORAGE_KEYS = {
  TODOS: 'todos',
  SETTINGS: 'settings',
  USER: 'user'
}

localStorage.setItem(STORAGE_KEYS.TODOS, data)
localStorage.getItem(STORAGE_KEYS.TODOS)
```

### 실수 4: 변경 후 저장 안 하기

데이터 수정 후, 다시 저장해야 합니다:

```javascript
const todos = JSON.parse(localStorage.getItem('todos')) || []
todos.push(newTodo)
// 저장 깜빡함! 새로고침하면 변경 사라짐

// 이거 잊지 마세요:
localStorage.setItem('todos', JSON.stringify(todos))
```

💡 **비전공자 팁**: localStorage에서 가져온 데이터는 "복사본"입니다. 수정해도 원본(localStorage)은 변하지 않습니다. 반드시 다시 저장해야 합니다.

### 실수 5: 민감한 데이터 저장하기

localStorage는 보안이 안 됩니다. 절대 저장하면 안 되는 것:
- 비밀번호
- API 키
- 신용카드 정보
- 주민등록번호

누구나 개발자 도구 열고 localStorage를 볼 수 있습니다!

```javascript
// 절대 하지 마세요!
localStorage.setItem('password', '1234')
localStorage.setItem('apiKey', 'sk-xxxxx')
```

### 실수 6: 동기 호출로 인한 성능 문제

localStorage는 동기적으로 동작합니다. 큰 데이터를 자주 읽고 쓰면 앱이 느려질 수 있습니다.

```javascript
// 좋지 않음 - 매번 전체 데이터를 읽고 씀
function updateTodoText(id, newText) {
  const todos = JSON.parse(localStorage.getItem('todos'))
  // 수정
  localStorage.setItem('todos', JSON.stringify(todos))
}

// 개선 - 메모리에 캐시해두고 필요할 때만 저장
let todosCache = null

function getTodosFromCache() {
  if (!todosCache) {
    todosCache = JSON.parse(localStorage.getItem('todos')) || []
  }
  return todosCache
}

function saveTodos() {
  localStorage.setItem('todos', JSON.stringify(todosCache))
}
```

---

## 실전 패턴: 재사용 가능한 저장소 만들기

프로젝트에서 바로 사용할 수 있는 저장소 유틸리티입니다.

```javascript
// storage.js - 재사용 가능한 localStorage 래퍼
const Storage = {
  // 저장하기
  set(key, value) {
    try {
      localStorage.setItem(key, JSON.stringify(value))
      return true
    } catch (error) {
      console.error('저장 실패:', error)
      return false
    }
  },

  // 읽기
  get(key, defaultValue = null) {
    try {
      const item = localStorage.getItem(key)
      return item ? JSON.parse(item) : defaultValue
    } catch (error) {
      console.error('읽기 실패:', error)
      return defaultValue
    }
  },

  // 삭제
  remove(key) {
    localStorage.removeItem(key)
  },

  // 전체 삭제
  clear() {
    localStorage.clear()
  },

  // 키 존재 여부
  has(key) {
    return localStorage.getItem(key) !== null
  },

  // 용량 확인 (KB)
  getSize() {
    let total = 0
    for (let key in localStorage) {
      if (localStorage.hasOwnProperty(key)) {
        total += localStorage[key].length * 2
      }
    }
    return (total / 1024).toFixed(2)
  }
}

// 사용 예시
Storage.set('user', { name: '홍길동', age: 25 })
const user = Storage.get('user', { name: 'Guest' })
console.log(user.name)  // '홍길동'
```

---

## 정리

이번 챕터에서 배운 것:
- [x] 데이터 저장이 필요한 이유
- [x] localStorage 사용법
- [x] JSON으로 복잡한 데이터 저장하기
- [x] CRUD 개념과 실제 구현
- [x] 할 일 목록 앱 만들기
- [x] 흔한 실수와 해결 방법

CRUD는 거의 모든 앱의 기초입니다. localStorage로 개념을 익히면, 나중에 데이터베이스를 배울 때도 같은 패턴을 적용할 수 있습니다.

---

## 용어 정리

| 용어 | 설명 |
|------|------|
| **localStorage** | 브라우저에서 제공하는 키-값 저장소. 영구 저장. |
| **sessionStorage** | localStorage와 유사하지만 탭을 닫으면 삭제됨 |
| **JSON** | JavaScript Object Notation. 데이터 교환 형식. |
| **JSON.stringify()** | JavaScript 객체를 JSON 문자열로 변환 |
| **JSON.parse()** | JSON 문자열을 JavaScript 객체로 변환 |
| **CRUD** | Create, Read, Update, Delete의 약자 |
| **키(Key)** | 데이터를 식별하는 고유한 이름 |
| **값(Value)** | 키에 연결된 실제 데이터 |
| **직렬화** | 객체를 저장/전송 가능한 형태로 변환 (stringify) |
| **역직렬화** | 저장/전송된 데이터를 다시 객체로 변환 (parse) |
| **동기(Synchronous)** | 작업이 순차적으로 실행됨. 완료까지 대기. |
| **캐시(Cache)** | 자주 사용하는 데이터를 임시 저장하여 성능 향상 |

---

## 다음 단계

다음 챕터에서는 재미있는 미니 게임을 만들어봅니다. localStorage로 최고 점수를 저장하는 기능도 추가할 것입니다!

[Chapter 16: 미니 게임](../Chapter16-Mini-Games/README.ko.md)으로 넘어가세요.

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [MDN Web Storage API](https://developer.mozilla.org/ko/docs/Web/API/Web_Storage_API) - localStorage 공식 문서
- [JSON 공식 사이트](https://www.json.org/json-ko.html) - JSON 형식 설명

**영상 자료:**
- [localStorage 튜토리얼 (YouTube)](https://www.youtube.com/results?search_query=javascript+localstorage+tutorial) - localStorage 활용법
- [CRUD 앱 만들기 (YouTube)](https://www.youtube.com/results?search_query=javascript+crud+tutorial) - CRUD 개념 실습

**읽을거리:**
- [JavaScript.info - localStorage](https://ko.javascript.info/localstorage) - localStorage 상세 설명
- [IndexedDB 소개](https://developer.mozilla.org/ko/docs/Web/API/IndexedDB_API) - 대용량 저장소

**관련 도구:**
- [Supabase](https://supabase.com/) - 오픈소스 백엔드 서비스
- [Firebase](https://firebase.google.com/) - Google 백엔드 플랫폼
- [JSON Formatter](https://jsonformatter.org/) - JSON 포맷터/검증기

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
