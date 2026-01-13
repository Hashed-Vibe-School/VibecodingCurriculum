# Chapter 13: 데이터 저장

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- 데이터 저장이 왜 필요한지
- localStorage로 브라우저에 데이터 저장하기
- CRUD 개념 이해하기

---

## 왜 데이터 저장이 필요한가요?

지금까지 만든 웹사이트는 "정적"입니다. 페이지를 새로고침하면 입력한 내용이 사라집니다.

하지만 실제 앱은 데이터가 유지되어야 합니다:
- 할 일 목록 → 새로고침해도 유지
- 메모 앱 → 다음에 다시 열어도 내용 있음
- 설정 → 사용자가 바꾼 설정 기억

이런 걸 하려면 **데이터 저장**이 필요합니다.

---

## 데이터 저장 방법들

데이터를 저장하는 방법은 여러 가지가 있습니다:

| 방법 | 특징 | 용도 |
|------|------|------|
| **localStorage** | 브라우저에 저장, 간단함 | 개인용 앱, 학습용 |
| Supabase/Firebase | 서버에 저장, 회원가입 필요 | 여러 사람이 쓰는 앱 |
| 파일 | 직접 파일로 저장 | 데스크톱 앱 |

이번 챕터에서는 **localStorage**를 사용합니다. 가입이 필요 없고, 바로 사용할 수 있습니다.

---

## localStorage란?

localStorage는 브라우저가 제공하는 저장 공간입니다.

### 특징

- **무료**: 별도 서비스 가입 불필요
- **간단**: JavaScript 몇 줄로 사용
- **영구적**: 브라우저를 닫아도 데이터 유지
- **용량**: 약 5MB (대부분의 앱에 충분)

### 어떻게 작동하나요?

```javascript
// 저장하기
localStorage.setItem('이름', '값')

// 읽어오기
localStorage.getItem('이름')

// 삭제하기
localStorage.removeItem('이름')
```

**데이터 저장 요청 팁:**

```
> 할 일 목록을 localStorage에 저장해줘
> 새로고침해도 데이터가 유지되게 해줘
```

어떤 데이터를 저장하고 싶은지 명확히 하면 적절한 코드가 나옵니다.

---

## CRUD: 데이터의 4가지 기본 작업

CRUD는 데이터를 다루는 기본 패턴입니다:

- **C**reate: 만들기
- **R**ead: 읽기
- **U**pdate: 수정하기
- **D**elete: 삭제하기

거의 모든 앱이 이 4가지 작업의 조합입니다. 할 일 앱도, 메모 앱도, SNS도 결국 CRUD입니다.

---

## 실습: 할 일 목록 앱 만들기

localStorage를 사용해서 실제 할 일 목록 앱을 만들어봅시다.

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
  // 기존 할 일 목록 가져오기
  const todos = JSON.parse(localStorage.getItem('todos')) || []

  // 새 할 일 추가
  const newTodo = {
    id: Date.now(),
    text: text,
    done: false
  }
  todos.push(newTodo)

  // 저장
  localStorage.setItem('todos', JSON.stringify(todos))
}
```

**JSON.stringify/parse는 뭔가요?**

localStorage는 문자열만 저장할 수 있습니다. 배열이나 객체를 저장하려면 문자열로 변환해야 합니다. `JSON.stringify`는 객체→문자열, `JSON.parse`는 문자열→객체로 변환합니다.

### 3단계: Read - 할 일 불러오기

```
> 저장된 할 일 목록 불러오는 기능 만들어줘
```

```javascript
function getTodos() {
  return JSON.parse(localStorage.getItem('todos')) || []
}

// 페이지 로드 시 실행
window.onload = function() {
  const todos = getTodos()
  todos.forEach(todo => displayTodo(todo))
}
```

### 4단계: Update - 완료 표시

```
> 할 일 완료 체크 기능 추가해줘
```

```javascript
function toggleTodo(id) {
  const todos = getTodos()
  const todo = todos.find(t => t.id === id)
  todo.done = !todo.done
  localStorage.setItem('todos', JSON.stringify(todos))
}
```

### 5단계: Delete - 삭제

```
> 할 일 삭제 기능 추가해줘
```

```javascript
function deleteTodo(id) {
  const todos = getTodos()
  const filtered = todos.filter(t => t.id !== id)
  localStorage.setItem('todos', JSON.stringify(todos))
}
```

---

## 피드백으로 개선하기

기본 기능이 되면 피드백으로 개선합니다:

```
> 완료된 할 일은 회색으로 표시해줘

> 할 일이 없을 때 "할 일이 없습니다" 메시지 보여줘

> 완료된 것만 한 번에 삭제하는 버튼 추가해줘
```

---

## 개발자 도구로 확인하기

localStorage에 저장된 데이터를 확인하려면:

1. 브라우저에서 F12 (개발자 도구)
2. Application 탭 선택
3. 왼쪽 메뉴에서 Local Storage 클릭

저장된 데이터를 직접 볼 수 있습니다.

---

## 언제 localStorage를 쓰고, 언제 데이터베이스를 쓸까?

### localStorage가 좋을 때

- 개인용 앱 (나만 쓸 때)
- 설정 저장
- 간단한 메모, 할 일 목록
- 학습용 프로젝트

### 데이터베이스가 필요할 때

- 여러 사람이 함께 쓸 때
- 다른 기기에서도 접근해야 할 때
- 데이터가 많을 때 (5MB 이상)
- 사용자 인증이 필요할 때

데이터베이스가 필요해지면 Supabase, Firebase 같은 서비스를 사용하면 됩니다. CRUD 개념은 똑같이 적용됩니다.

---

## 미니 프로젝트

### 프로젝트 1: 메모 앱

```
> 메모 앱 만들어줘.
> - 메모 작성, 수정, 삭제
> - localStorage 저장
> - 제목으로 검색
```

### 프로젝트 2: 설정 저장

```
> 다크 모드 토글 만들어줘.
> 사용자가 선택한 테마를 localStorage에 저장해서
> 다음에 열 때도 유지되게.
```

### 프로젝트 3: 습관 트래커

```
> 일일 습관 체크 앱 만들어줘.
> - 습관 목록 관리
> - 오늘 완료한 습관 체크
> - 완료 기록을 localStorage에 저장
```

### 심화 과제 (숙련자용)

```
> localStorage 데이터를 JSON 파일로 내보내기/가져오기 기능 추가해줘.
> 백업과 복원 용도로.
```

---

## 정리

이번 챕터에서 배운 것:
- [x] 데이터 저장이 필요한 이유
- [x] localStorage 사용법
- [x] CRUD 개념
- [x] 할 일 목록 앱 만들기

CRUD는 거의 모든 앱의 기초입니다. localStorage로 개념을 익히면, 나중에 데이터베이스를 배울 때도 같은 패턴을 적용할 수 있습니다.

다음 챕터에서는 재미있는 미니 게임을 만들어봅니다.

[Chapter 14: 미니 게임](../Chapter14/README.ko.md)으로 넘어가세요.
