# Chapter 13: 데이터 저장

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- 데이터베이스가 무엇인지
- Supabase 사용하기
- 데이터 저장하고 불러오기

---

## 왜 데이터 저장이 필요한가요?

지금까지 만든 웹사이트는 "정적"입니다. 내용이 고정되어 있습니다.

하지만 실제 앱은 데이터가 계속 바뀝니다:
- 회원 가입하면 사용자 정보 저장
- 글을 쓰면 글 내용 저장
- 좋아요 누르면 숫자 증가

이런 걸 하려면 **데이터베이스**가 필요합니다.

---

## 데이터베이스란?

데이터베이스는 엑셀 같은 것입니다. 표 형태로 데이터를 저장합니다.

### 예시: 사용자 테이블

| id | 이름 | 이메일 | 가입일 |
|----|------|--------|--------|
| 1 | 홍길동 | hong@email.com | 2024-01-15 |
| 2 | 김철수 | kim@email.com | 2024-01-16 |

### 예시: 게시글 테이블

| id | 제목 | 내용 | 작성자_id | 작성일 |
|----|------|------|-----------|--------|
| 1 | 첫 글 | 안녕하세요 | 1 | 2024-01-15 |
| 2 | 두 번째 | 반갑습니다 | 2 | 2024-01-16 |

---

## Supabase: 가장 쉬운 데이터베이스

Supabase는 무료로 쓸 수 있는 데이터베이스 서비스입니다.

### 왜 Supabase인가요?

- **무료**: 개인 프로젝트에 충분
- **쉬움**: 엑셀처럼 테이블 만들기
- **빠름**: 클릭 몇 번으로 설정
- **기능 많음**: 로그인, 파일 저장도 가능

### 다른 선택지

| 서비스 | 특징 | 난이도 |
|--------|------|--------|
| **Supabase** | PostgreSQL 기반, UI 좋음 | 쉬움 |
| Firebase | Google 제품, 실시간 DB | 보통 |
| PlanetScale | MySQL 기반 | 보통 |

초보자는 Supabase를 추천합니다.

---

## Supabase 시작하기

### 1단계: 가입하기

1. [supabase.com](https://supabase.com) 접속
2. GitHub으로 로그인
3. "New Project" 클릭
4. 프로젝트 이름, 비밀번호 설정
5. 지역은 "Northeast Asia (Seoul)" 선택

### 2단계: 테이블 만들기

대시보드에서:
1. "Table Editor" 클릭
2. "New Table" 클릭
3. 테이블 이름과 컬럼 설정

### Claude에게 시키기

```
> Supabase에 할 일 목록 테이블 만드는 SQL 알려줘
```

Claude가 알려주는 SQL:
```sql
create table todos (
  id bigint primary key generated always as identity,
  title text not null,
  is_done boolean default false,
  created_at timestamp default now()
);
```

이걸 Supabase SQL Editor에 붙여넣고 실행하세요.

---

## 웹사이트에서 Supabase 연결하기

### 1단계: API 키 가져오기

Supabase 대시보드에서:
1. Settings → API
2. `URL`과 `anon key` 복사

### 2단계: 연결 코드 추가

```
> Supabase 연결 코드 만들어줘.
> URL: [복사한 URL]
> API Key: [복사한 키]
```

Claude가 만들어주는 코드:
```javascript
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = '여기에_URL'
const supabaseKey = '여기에_KEY'

const supabase = createClient(supabaseUrl, supabaseKey)
```

---

## 데이터 CRUD

CRUD는 데이터의 기본 작업입니다:
- **C**reate: 만들기
- **R**ead: 읽기
- **U**pdate: 수정하기
- **D**elete: 삭제하기

### Create: 데이터 추가

```
> 할 일 추가하는 함수 만들어줘
```

```javascript
async function addTodo(title) {
  const { data, error } = await supabase
    .from('todos')
    .insert({ title: title })

  if (error) console.error('에러:', error)
  return data
}

// 사용
addTodo('Claude Code 배우기')
```

### Read: 데이터 읽기

```
> 모든 할 일 가져오는 함수 만들어줘
```

```javascript
async function getTodos() {
  const { data, error } = await supabase
    .from('todos')
    .select('*')

  if (error) console.error('에러:', error)
  return data
}

// 사용
const todos = await getTodos()
console.log(todos)
```

### Update: 데이터 수정

```
> 할 일 완료 표시하는 함수 만들어줘
```

```javascript
async function completeTodo(id) {
  const { data, error } = await supabase
    .from('todos')
    .update({ is_done: true })
    .eq('id', id)

  return data
}

// 사용
completeTodo(1)  // id가 1인 할 일 완료
```

### Delete: 데이터 삭제

```
> 할 일 삭제하는 함수 만들어줘
```

```javascript
async function deleteTodo(id) {
  const { data, error } = await supabase
    .from('todos')
    .delete()
    .eq('id', id)

  return data
}

// 사용
deleteTodo(1)  // id가 1인 할 일 삭제
```

---

## 실습: 할 일 목록 앱 만들기

### 목표

- 할 일 추가
- 할 일 목록 보기
- 완료 표시
- 삭제

### 단계별 진행

```
# 1. 프로젝트 설정
> todo-app 프로젝트 만들어줘.
> HTML, CSS, JavaScript 사용.
> Supabase 연결 포함.

# 2. 테이블 만들기
> todos 테이블 만드는 SQL 알려줘

# 3. UI 만들기
> 할 일 입력창이랑 목록 UI 만들어줘

# 4. 기능 연결
> 할 일 추가 기능 연결해줘

# 5. 목록 불러오기
> 페이지 로드할 때 할 일 목록 불러오게 해줘

# 6. 완료 / 삭제
> 체크박스로 완료 표시, 삭제 버튼 추가해줘
```

---

## 보안 주의사항

### API 키 숨기기

API 키를 코드에 직접 넣으면 위험합니다.

```
> 환경 변수로 API 키 관리하는 법 알려줘
```

### Row Level Security (RLS)

Supabase에서 데이터 보안 설정:

1. Table Editor → 테이블 선택
2. RLS 활성화
3. 정책 추가

```
> RLS 정책 설정하는 법 알려줘
```

---

## 자주 쓰는 쿼리

### 조건으로 필터

```javascript
// 완료 안 된 것만
const { data } = await supabase
  .from('todos')
  .select('*')
  .eq('is_done', false)
```

### 정렬

```javascript
// 최신순
const { data } = await supabase
  .from('todos')
  .select('*')
  .order('created_at', { ascending: false })
```

### 개수 제한

```javascript
// 10개만
const { data } = await supabase
  .from('todos')
  .select('*')
  .limit(10)
```

---

## 문제 해결

### 연결 안 될 때

```
> Supabase 연결 에러 나와: [에러 메시지]
> 뭐가 문제야?
```

흔한 원인:
- URL이나 API 키 오타
- 네트워크 문제
- RLS 정책 때문에 접근 차단

### 데이터가 안 보일 때

```
> 데이터 추가는 되는데 읽어오면 빈 배열이야
```

RLS 정책을 확인하세요.

---

## 미니 프로젝트: 데이터베이스 앱들

할 일 앱 외에도 다양한 앱을 만들어보세요.

### 프로젝트 1: 방명록

```
> 방명록 앱 만들어줘.
> - 이름, 메시지 입력
> - 최신 글이 위로 정렬
> - Supabase 연동
```

### 프로젝트 2: 메모 앱

```
> 메모 앱 만들어줘.
> - 메모 작성, 수정, 삭제
> - 검색 기능
> - 카테고리/태그 분류
```

### 프로젝트 3: 간단한 게시판

```
> 간단한 게시판 만들어줘.
> - 글 목록, 상세 페이지
> - 글 작성, 수정, 삭제
> - 페이지네이션
```

### 심화 과제 (숙련자용)

```
> 실시간 채팅 기능 추가해줘. Supabase Realtime 사용해서.

> 파일 업로드 기능 추가해줘. Supabase Storage 사용.

> 사용자 인증 추가해줘. Supabase Auth로.
```

---

## 정리

이번 챕터에서 배운 것:
- [x] 데이터베이스 개념
- [x] Supabase 설정하기
- [x] 테이블 만들기
- [x] CRUD 작업하기
- [x] 웹사이트에 연결하기

이제 데이터를 저장하고 불러오는 진짜 앱을 만들 수 있습니다.

다음 챕터에서는 재미있는 미니 게임을 만들어봅니다.

[Chapter 14: 미니 게임](../Chapter14/README.ko.md)으로 넘어가세요.
