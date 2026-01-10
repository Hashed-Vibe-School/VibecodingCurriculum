# Chapter 15: AI 도구 만들기

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- AI API 사용하기
- 챗봇 만들기
- 이미지 생성 도구 만들기

---

## AI API란?

AI API는 인공지능 기능을 가져다 쓸 수 있게 해주는 서비스입니다.

직접 AI를 만들 필요 없이, API를 호출하면 AI가 대신 일해줍니다:
- 텍스트 생성
- 이미지 생성
- 번역
- 요약

---

## OpenAI API 시작하기

가장 유명한 AI API인 OpenAI를 사용해봅니다.

### 1단계: API 키 발급

1. [platform.openai.com](https://platform.openai.com) 접속
2. 회원가입 (카드 등록 필요)
3. API Keys 메뉴에서 키 생성
4. 키를 안전한 곳에 저장

### 2단계: 프로젝트 설정

```
> AI 챗봇 프로젝트 만들어줘.
> OpenAI API 사용할 거야.
> 환경 변수로 API 키 관리하게.
```

### 환경 변수 설정

`.env` 파일:
```
OPENAI_API_KEY=sk-your-api-key-here
```

**주의**: `.env` 파일은 절대 GitHub에 올리면 안 됩니다!

---

## 프로젝트 1: 간단한 챗봇

### 목표

- 사용자가 질문 입력
- AI가 대답
- 대화 내용 표시

### 만들기

```
> 간단한 AI 챗봇 만들어줘.
> 사용자가 질문하면 OpenAI API로 답변 받아서 보여주는.
> 대화 히스토리도 보여줘.
```

### 핵심 코드 (Node.js)

```javascript
import OpenAI from 'openai'

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY
})

async function chat(message) {
  const response = await openai.chat.completions.create({
    model: "gpt-3.5-turbo",
    messages: [
      { role: "user", content: message }
    ]
  })

  return response.choices[0].message.content
}

// 사용
const answer = await chat("안녕하세요!")
console.log(answer)
```

### 개선하기

```
> 대화 맥락을 기억하게 해줘
```
→ 메시지 히스토리 관리와 컨텍스트 전달

```
> 시스템 프롬프트 추가해줘.
> "당신은 친절한 한국어 비서입니다"
```
→ 시스템 프롬프트로 AI 행동 제어

```
> 답변 중에는 로딩 표시해줘
```
→ 비동기 상태 관리와 UX 개선

---

## 프로젝트 2: 글 요약 도구

### 목표

- 긴 글을 붙여넣기
- AI가 3줄로 요약
- 핵심 키워드 추출

### 만들기

```
> 글 요약 도구 만들어줘.
> 긴 텍스트를 넣으면 3줄로 요약.
> 핵심 키워드도 뽑아줘.
```

### 핵심 로직

```javascript
async function summarize(text) {
  const response = await openai.chat.completions.create({
    model: "gpt-3.5-turbo",
    messages: [
      {
        role: "system",
        content: "당신은 텍스트 요약 전문가입니다. 핵심만 간단히 정리해주세요."
      },
      {
        role: "user",
        content: `다음 글을 3줄로 요약하고, 핵심 키워드 5개를 뽑아주세요:\n\n${text}`
      }
    ]
  })

  return response.choices[0].message.content
}
```

### 개선하기

```
> 요약 길이를 선택할 수 있게 해줘 (짧게/보통/길게)
```
→ 동적 프롬프트 생성 패턴

```
> 영어 글도 한글로 요약해줘
```
→ 다국어 처리와 번역 로직

```
> 복사 버튼 추가해줘
```
→ Clipboard API 활용

---

## 프로젝트 3: 이미지 생성기

### DALL-E API 사용

OpenAI의 DALL-E로 이미지를 생성할 수 있습니다.

### 만들기

```
> 이미지 생성 도구 만들어줘.
> 설명을 입력하면 DALL-E로 이미지 생성.
> 생성된 이미지 화면에 표시.
```

### 핵심 코드

```javascript
async function generateImage(prompt) {
  const response = await openai.images.generate({
    model: "dall-e-3",
    prompt: prompt,
    n: 1,
    size: "1024x1024"
  })

  return response.data[0].url
}

// 사용
const imageUrl = await generateImage("우주에서 피자 먹는 고양이")
```

### 개선하기

```
> 이미지 크기 선택할 수 있게 해줘
```
→ API 파라미터 설정 패턴

```
> 생성 중 로딩 애니메이션 보여줘
```
→ 장시간 요청의 로딩 상태 처리

```
> 이미지 다운로드 버튼 추가해줘
```
→ Blob과 다운로드 링크 생성

---

## 프로젝트 4: 번역기

### 목표

- 텍스트 입력
- 언어 자동 감지
- 원하는 언어로 번역

### 만들기

```
> AI 번역기 만들어줘.
> 입력 언어는 자동 감지.
> 영어, 한국어, 일본어, 중국어 번역 지원.
```

### 핵심 로직

```javascript
async function translate(text, targetLanguage) {
  const response = await openai.chat.completions.create({
    model: "gpt-3.5-turbo",
    messages: [
      {
        role: "system",
        content: `You are a translator. Translate the given text to ${targetLanguage}. Only output the translation, nothing else.`
      },
      {
        role: "user",
        content: text
      }
    ]
  })

  return response.choices[0].message.content
}
```

---

## API 비용 관리

### 요금 확인

OpenAI API는 사용량에 따라 요금이 부과됩니다.

| 모델 | 가격 (1K 토큰당) |
|------|------------------|
| GPT-3.5 Turbo | ~$0.001 |
| GPT-4 | ~$0.03 |
| DALL-E 3 | ~$0.04/이미지 |

### 비용 절약 팁

```
> API 호출 횟수 제한 기능 추가해줘.
> 하루 10번까지만.

> 응답 캐싱 기능 추가해줘.
> 같은 질문은 저장된 답변 사용.
```

### 사용량 모니터링

OpenAI 대시보드에서 사용량을 확인하세요.
예산 한도를 설정해두면 안전합니다.

---

## 무료 대안

OpenAI가 부담되면 무료 옵션도 있습니다:

| 서비스 | 특징 |
|--------|------|
| Claude API | Anthropic의 AI (이 커리큘럼의 주인공!) |
| Hugging Face | 무료 오픈소스 모델 |
| Ollama | 로컬에서 AI 실행 |

```
> Ollama로 로컬 AI 챗봇 만드는 법 알려줘
```

---

## 보안 주의사항

### API 키 보호

```
# 절대 하면 안 되는 것
const apiKey = "sk-1234567890"  // 코드에 직접 넣기

# 해야 하는 것
const apiKey = process.env.OPENAI_API_KEY  // 환경 변수 사용
```

### .gitignore 설정

```
.env
.env.local
*.env
```

### 프론트엔드에서 API 호출하지 않기

API 키가 노출될 수 있습니다. 항상 백엔드 서버를 통해 호출하세요.

---

## 실습: AI 도구 만들기

### 기본 과제

위 프로젝트 중 하나를 완성하세요.

### 추가 도전

```
> 코드 설명 도구 만들어줘.
> 코드를 붙여넣으면 AI가 설명해주는.

> 이메일 작성 도우미 만들어줘.
> 핵심 내용 입력하면 정중한 이메일로 바꿔주는.

> 퀴즈 생성기 만들어줘.
> 주제 입력하면 AI가 퀴즈 문제 만들어주는.
```

---

## 미니 프로젝트: AI 멀티툴

여러 AI 기능을 하나로 합쳐보세요!

### 목표

- 여러 AI 기능 통합
- 사용자 친화적 UI

### 만들기

```
> AI 멀티툴 만들어줘.
> 하나의 앱에서 다음 기능 지원:
> - 챗봇
> - 텍스트 요약
> - 번역
> - 이미지 생성
> 탭이나 사이드바로 기능 전환
```

### 심화 과제 (숙련자용)

```
> 대화 내역 저장/불러오기 기능 추가해줘

> 스트리밍 응답 구현해줘 (글자가 하나씩 나오게)

> 프롬프트 템플릿 관리 기능 추가해줘

> 여러 AI 모델 선택할 수 있게 해줘 (GPT-4, Claude, 등)
```

---

## 정리

이번 챕터에서 배운 것:
- [x] AI API 사용법
- [x] 챗봇 만들기
- [x] 이미지 생성
- [x] 번역/요약 도구
- [x] API 비용 관리

다음 챕터에서는 데이터를 처리하고 분석하는 도구를 만들어봅니다.

[Chapter 16: 데이터 처리](../Chapter16/README.ko.md)로 넘어가세요.
