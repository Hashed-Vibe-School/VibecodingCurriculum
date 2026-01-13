# Chapter 02: Claude Code 설치하기

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- 터미널의 개념
- Claude Code 설치 방법
- 로그인 및 첫 실행

---

## 터미널이란?

터미널은 컴퓨터와 텍스트로 소통하는 창입니다.

평소에는 마우스로 폴더를 클릭하고, 앱 아이콘을 눌러서 실행합니다. 터미널은 같은 작업을 텍스트로 수행합니다.

```
# 예시
cd Documents        # Documents 폴더로 이동
ls                  # 현재 폴더의 파일 목록 보기
```

어려워 보이지만 걱정하지 마세요. Claude는 터미널 명령어도 수많이 학습했기 때문에, 여러분이 "이 폴더의 파일 목록 보여줘"라고 말하면 적절한 명령어(`ls`)를 알아서 실행합니다. 터미널 여는 방법만 알면 됩니다.

### 터미널 여는 법

**Mac:**
1. `Command + Space`로 Spotlight 열기
2. "터미널" 또는 "Terminal" 입력
3. Enter

**Windows:**
1. `Windows 키` 누르기
2. "PowerShell" 입력
3. Enter

---

## Claude Code 설치

### 방법 1: 설치 스크립트 (권장)

가장 간단한 방법입니다. 터미널을 열고 아래 명령어를 복사해서 붙여넣으세요.

**Mac / Linux:**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Windows (PowerShell):**
```powershell
irm https://claude.ai/install.ps1 | iex
```

설치가 완료되면 성공입니다.

### 방법 2: Homebrew (Mac 사용자)

Homebrew를 사용한다면:
```bash
brew install --cask claude-code
```

### 방법 3: npm (Node.js 사용자)

Node.js가 설치되어 있다면:
```bash
npm install -g @anthropic-ai/claude-code
```

### 설치 확인

설치가 정상적으로 완료되었는지 확인:
```bash
claude --version
```

버전 번호가 출력되면 성공입니다. (예: `2.1.3`)

---

## 로그인

Claude Code를 처음 실행하면 로그인이 필요합니다.

### 1. Claude Code 시작

터미널에서:
```bash
claude
```

### 2. 로그인 화면

브라우저 창이 열립니다. Claude 계정으로 로그인하세요.

계정이 없으면 [claude.ai](https://claude.ai)에서 생성할 수 있습니다.

### 3. 로그인 완료

로그인하면 터미널에 Claude Code가 실행됩니다.

```
╭────────────────────────────────────────╮
│ Welcome to Claude Code!                │
│                                        │
│ Type your message below.               │
╰────────────────────────────────────────╯

>
```

`>` 표시가 나오면 대화할 준비가 된 것입니다.

---

## 요금제 안내

Claude Code는 유료 서비스입니다.

### 사용 방법

1. **Claude Pro/Max 구독**: 월 $20부터. 일반 사용량에 충분합니다.
2. **API 키 사용**: 사용한 만큼 결제. 개발자용.

처음이라면 Claude Pro 구독을 권장합니다. [claude.ai/pricing](https://claude.ai/pricing)에서 확인하세요.

### API 키로 사용하기 (선택)

API 키가 있다면:
```bash
export ANTHROPIC_API_KEY="your-api-key"
claude
```

---

## 첫 대화

로그인했으면 간단히 테스트해보세요.

```
> 안녕! 넌 뭘 할 수 있어?
```

Claude가 자기소개를 합니다.

### 간단한 테스트

```
> 이 폴더에 뭐가 있어?
```

Claude는 "파일 목록 보기" 요청을 이해하고, 해당하는 터미널 명령어(`ls` 또는 `dir`)를 실행한 뒤 결과를 보여줍니다.

```
> hello.txt 파일 만들어줘. 안에 "안녕하세요"라고 써줘.
```

파일이 생성됩니다. 직접 폴더를 열어서 확인해보세요.

---

## 종료

Claude Code를 종료하려면:

- `/exit` 입력
- 또는 `Ctrl + C` 두 번

---

## 문제 해결

### "command not found" 에러

터미널을 닫고 다시 열어보세요. 그래도 안 되면:
```bash
# Mac/Linux
source ~/.bashrc
# 또는
source ~/.zshrc
```

### 로그인이 안 됨

1. 브라우저에서 [claude.ai](https://claude.ai)에 로그인되어 있는지 확인
2. 다른 브라우저로 시도
3. VPN이 켜져 있으면 끄고 시도

### 그래도 안 되면

```bash
claude /doctor
```
이 명령어가 문제를 진단합니다.

---

## 정리

이번 챕터에서 완료한 것:
- [x] 터미널 여는 법
- [x] Claude Code 설치
- [x] 로그인
- [x] 첫 대화

다음 챕터에서는 Claude Code와 본격적으로 대화하는 방법을 학습합니다.

[Chapter 03: 첫 대화 시작하기](../Chapter03/README.ko.md)로 진행하세요.
