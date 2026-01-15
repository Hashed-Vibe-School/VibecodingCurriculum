# Chapter 13: 데이터 저장

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- 데이터 저장이 왜 필요한지
- localStorage로 브라우저에 데이터 저장하기
- CRUD 개념 이해하기

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

---

## 따라해보세요: 한 가지만 저장하고 불러오기

완전한 앱을 만들기 전에, 가장 간단한 예제로 데이터 저장이 어떻게 동작하는지 봅시다.

```
> HTML 페이지 만들어줘. 입력 필드 하나랑 "저장" 버튼만 있게.
> 저장 버튼 누르면 입력값을 localStorage에 저장해.
> 페이지 로드될 때 저장된 값을 입력 필드에 보여줘.
```

뭔가 입력하고, 저장 누르고, 페이지 새로고침하세요. 텍스트가 그대로 있는 것을 확인할 수 있습니다. 이것이 바로 localStorage의 기능입니다.

---

## 왜 데이터 저장이 필요합니까?

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

### 어떻게 작동합니까?

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

**JSON.stringify/parse는 무엇입니까?**

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

## 언제 localStorage를 쓰고, 언제 데이터베이스를 사용합니까?

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

## 안 되면?

데이터 저장은 까다로울 수 있습니다. 자주 발생하는 문제와 해결법입니다.

### 새로고침하면 데이터가 사라집니다
- `localStorage.setItem()`을 실제로 호출했습니까?
- 브라우저 콘솔에서 에러를 확인하시기 바랍니다 (F12 > Console)
```
> 데이터가 저장 안 돼. localStorage 코드 확인해줘.
```

### JSON.parse 에러
저장된 데이터가 유효한 JSON이 아닐 때 발생합니다:
```
> localStorage 데이터 파싱할 때 "Unexpected token" 에러 나와.
> 어떻게 고쳐?
```
빠른 해결: localStorage 지우고 새로 시작:
```javascript
localStorage.clear()
```

### 데이터가 "[object Object]"로 보입니다
저장하기 전에 stringify를 깜빡한 경우입니다:
```javascript
// 잘못됨
localStorage.setItem('data', myObject)

// 올바름
localStorage.setItem('data', JSON.stringify(myObject))
```

### localStorage가 꽉 찼습니다
localStorage는 5MB 제한이 있습니다. 많이 저장했다면:
```
> localStorage가 꽉 찼어. 뭐가 공간 차지하는지 확인하고
> 불필요한 데이터 정리하는 것 도와줘.
```

### 로컬에서는 되는데 배포 후에는 안 됩니다
localStorage는 브라우저별로 저장됩니다. 본인 컴퓨터의 데이터가 다른 사람 브라우저에 나타나지 않는 것은 정상입니다.

### 개발자 도구에서 localStorage가 안 보입니다
- 올바른 도메인에 있는지 확인하시기 바랍니다
- Application 탭 > Local Storage > 사이트 URL을 선택하시기 바랍니다

---

## 자주 하는 실수

이런 함정을 피하시기 바랍니다.

### 실수 1: 읽을 때 parse 깜빡하기
```javascript
// 잘못됨 - 이건 문자열이지 배열이 아닙니다!
const todos = localStorage.getItem('todos')
todos.push(newTodo)  // 에러!

// 올바름
const todos = JSON.parse(localStorage.getItem('todos')) || []
todos.push(newTodo)
```

### 실수 2: null/빈 값 처리 안 하기
처음 사용하는 유저는 데이터가 없습니다:
```javascript
// 저장된 게 없으면 크래시
const todos = JSON.parse(localStorage.getItem('todos'))

// 안전한 방법 - 빈 배열을 기본값으로
const todos = JSON.parse(localStorage.getItem('todos')) || []
```

### 실수 3: 키 이름 잘못 쓰기
키는 대소문자 구분하고 정확해야 합니다:
```javascript
localStorage.setItem('todos', data)
localStorage.getItem('Todos')  // null 반환! (대문자 T)
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

### 실수 5: 민감한 데이터 저장하기
localStorage는 보안이 안 됩니다. 절대 저장하면 안 되는 것:
- 비밀번호
- API 키
- 개인정보

누구나 개발자 도구 열고 localStorage를 볼 수 있습니다!

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
