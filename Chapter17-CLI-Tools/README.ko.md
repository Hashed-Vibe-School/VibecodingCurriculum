# Chapter 17: CLI 도구 만들기

[English](./README.md) | **한국어**

---

## 💬 질문하기

학습 중 궁금한 점이 있으면 디스코드에서 질문하세요!

[![Discord](https://img.shields.io/badge/Discord-질문하기-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## 이 챕터에서 배우는 것

- Node.js CLI 도구의 구조와 작동 원리
- 사용자 입력(인자, 옵션) 처리
- 파일 시스템 조작
- 인터랙티브 프롬프트 만들기
- npm 패키지로 배포하기
- 실제로 사용하는 자동화 도구 만들기

---

## 이전 챕터와의 연결

Chapter 16에서는 브라우저에서 동작하는 게임을 만들었습니다. 이제 **터미널에서 실행되는 도구**를 만들어봅니다. 웹 브라우저 없이 명령어 한 줄로 작업을 자동화하는 것이 목표입니다.

---

## 왜 필요합니까?

**CLI 도구가 빛나는 실제 상황들:**

- **다운로드 폴더가 난장판입니다** - 수백 개의 파일이 뒤섞여 있습니다. CLI 도구가 몇 초 만에 자동으로 정리해줍니다
- **똑같은 프로젝트 구조를 반복해서 만듭니다** - 새 프로젝트마다 같은 폴더와 파일이 필요합니다. 자동화하시기 바랍니다
- **많은 파일을 한꺼번에 처리해야 합니다** - 파일 100개 이름 바꾸기, 이미지 리사이즈, 포맷 변환 등 CLI 도구는 일괄 작업을 쉽게 처리합니다
- **자동화 도구를 다른 사람과 공유하고 싶습니다** - npm에 배포하면 누구나 한 줄 명령어로 설치할 수 있습니다

CLI 도구를 **반복 작업에 지치지 않는 나만의 로봇 비서**라고 생각하시기 바랍니다.

> 💡 **비전공자 팁**: 우리가 매일 쓰는 `git`, `npm`, `npx`도 모두 누군가가 시간을 아끼려고 만든 CLI 도구입니다. 여러분도 만들 수 있어요!

---

## 쉬운 비유: CLI 도구는 주방 가전과 같습니다

주스를 만든다고 상상해 보시기 바랍니다:
- **믹서기 없이**: 과일을 하나하나 손으로 짜야 하며, 시간이 많이 걸립니다
- **믹서기로**: 과일 넣고 버튼 누르면 끝입니다

CLI 도구도 마찬가지입니다:
- **CLI 도구 없이**: 파일을 수동으로 옮기고, 폴더 만들고, 명령어 반복 입력
- **CLI 도구로**: 명령어 한 번 실행하면 모든 게 자동으로 처리

---

## CLI란 무엇인가요?

### CLI vs GUI 비교

| CLI (Command Line Interface) | GUI (Graphical User Interface) |
|------------------------------|-------------------------------|
| 텍스트 명령어로 조작 | 마우스 클릭으로 조작 |
| 터미널/명령 프롬프트에서 실행 | 창과 버튼이 있는 프로그램 |
| 자동화에 적합 | 직관적이지만 반복 작업에 불리 |
| 예: `git commit -m "메시지"` | 예: GitHub Desktop 앱 |

### CLI의 장점

```
1. 속도: 명령어 한 줄 > 여러 번 클릭
2. 자동화: 스크립트로 반복 작업 처리
3. 원격 작업: SSH로 서버에서도 사용 가능
4. 재현성: 같은 명령어 = 같은 결과
5. 조합: 파이프(|)로 여러 도구 연결
```

> 🔥 **프로 팁**: 개발자가 되면 터미널에서 보내는 시간이 많아집니다. CLI 도구를 만들 줄 알면 자신만의 워크플로우를 만들 수 있어요.

---

## 따라해보세요: 최소 동작 예제

전체 파일 정리 도구를 만들기 전에, 기본을 이해하기 위해 아주 간단한 CLI 도구부터 만들어봅시다:

**1. `hello-cli.js`라는 파일 하나를 만드세요:**

```javascript
#!/usr/bin/env node
// ↑ Shebang: 이 파일을 Node.js로 실행하라는 의미

// 명령줄 인자 가져오기
// process.argv는 배열: [node 경로, 스크립트 경로, 인자1, 인자2, ...]
// 처음 두 개는 건너뜀
const args = process.argv.slice(2)

// 첫 번째 인자를 이름으로 사용 (없으면 'World')
const name = args[0] || 'World'

console.log(`안녕하세요, ${name}님!`)
console.log('첫 번째 CLI 도구를 만들었어요!')

// 인자가 여러 개면 모두 표시
if (args.length > 1) {
    console.log(`전달받은 모든 인자: ${args.join(', ')}`)
}
```

**2. 실행 권한을 주고 실행:**

```bash
# Windows에서는 실행 권한 설정이 필요 없습니다
# Mac/Linux에서는 실행 가능하게 만들기
chmod +x hello-cli.js

# 실행!
node hello-cli.js
# 출력: 안녕하세요, World님!

node hello-cli.js 철수
# 출력: 안녕하세요, 철수님!

node hello-cli.js 철수 영희 민수
# 출력: 안녕하세요, 철수님!
#       전달받은 모든 인자: 철수, 영희, 민수
```

**3. package.json에 추가해서 명령어로 사용:**

```json
{
  "name": "my-first-cli",
  "version": "1.0.0",
  "bin": {
    "greet": "./hello-cli.js"
  }
}
```

```bash
# 현재 프로젝트를 전역으로 링크
npm link

# 이제 어디서든 사용 가능!
greet 영희
# 출력: 안녕하세요, 영희님!
```

> 💡 **비전공자 팁**: `npm link`는 현재 폴더의 CLI 도구를 시스템 전체에서 사용할 수 있게 해줍니다. 개발 중에 테스트할 때 유용해요.

---

## CLI 도구의 핵심 개념

### 1. Shebang (#!/usr/bin/env node)

```javascript
#!/usr/bin/env node
// ↑ 이 줄이 "Shebang"입니다
```

| 운영체제 | Shebang 필요 여부 | 설명 |
|---------|-----------------|------|
| Mac/Linux | 필수 | 파일을 직접 실행할 때 어떤 프로그램을 사용할지 알려줌 |
| Windows | 선택 | npm이 알아서 처리하지만, 호환성을 위해 넣는 것이 좋음 |

### 2. process.argv - 명령줄 인자

```javascript
// node myscript.js hello world 123 을 실행하면:
console.log(process.argv)
// [
//   '/usr/local/bin/node',     // [0] Node.js 실행 파일 경로
//   '/path/to/myscript.js',    // [1] 스크립트 경로
//   'hello',                   // [2] 첫 번째 인자
//   'world',                   // [3] 두 번째 인자
//   '123'                      // [4] 세 번째 인자
// ]

// 실제 인자만 가져오기
const args = process.argv.slice(2)  // ['hello', 'world', '123']
```

### 3. process.exit() - 종료 코드

```javascript
// 성공적으로 종료
process.exit(0)

// 에러로 종료 (다른 스크립트에서 이 결과를 확인할 수 있음)
process.exit(1)

// 실제 사용 예시
if (!inputFile) {
    console.error('입력 파일을 지정해주세요')
    process.exit(1)
}
```

> ⚠️ **주의사항**: 종료 코드 0은 "성공", 1 이상은 "에러"를 의미합니다. 이 규칙을 지켜야 다른 스크립트와 조합할 때 문제가 없습니다.

---

## 프로젝트: 파일 정리 도구 만들기

다운로드 폴더가 지저분하지 않습니까? 파일을 자동으로 정리하는 CLI 도구를 만들어봅시다.

### 목표

```bash
# 이렇게 실행하면
$ organize ./downloads

# 파일이 자동으로 분류됨
# downloads/
#   images/  → .jpg, .png, .gif
#   docs/    → .pdf, .docx, .txt
#   videos/  → .mp4, .mov
#   others/  → 나머지
```

### 왜 이 프로젝트인가요?

| 배우는 것 | 실제 활용 |
|----------|---------|
| 파일 시스템 조작 | 모든 CLI 도구의 기본 |
| 인자 처리 | 사용자 입력 받기 |
| 에러 핸들링 | 안정적인 도구 만들기 |
| 옵션 처리 | 유연한 동작 설정 |

### Step 1: 프로젝트 시작

```
> Node.js CLI 프로젝트를 만들어줘.
> 이름은 file-organizer.
> commander 라이브러리를 사용해서 인자를 파싱하고,
> chalk로 컬러 출력을 지원해줘.
```

Claude가 만드는 구조:

```
file-organizer/
├── package.json           # 프로젝트 설정
├── bin/
│   └── organize.js        # CLI 진입점 (사용자가 실행하는 파일)
└── src/
    ├── organizer.js       # 핵심 로직 (파일 정리)
    ├── categories.js      # 파일 분류 규칙
    └── utils.js           # 유틸리티 함수
```

> 💡 **비전공자 팁**: `bin/` 폴더에는 실행 파일, `src/` 폴더에는 실제 로직을 넣는 것이 일반적인 구조입니다. 관심사를 분리하면 유지보수가 쉬워져요.

### Step 2: CLI 진입점 만들기

```javascript
#!/usr/bin/env node
// bin/organize.js

const { program } = require('commander')
const { organizeFiles } = require('../src/organizer')
const chalk = require('chalk')
const packageJson = require('../package.json')

// 프로그램 기본 정보
program
  .name('organize')
  .description('파일을 종류별로 자동 정리합니다')
  .version(packageJson.version)  // package.json에서 버전 읽기

// 메인 명령어 정의
program
  .argument('<directory>', '정리할 폴더 경로')
  .option('-d, --dry-run', '실제로 이동하지 않고 미리보기만')
  .option('-v, --verbose', '상세 로그 출력')
  .option('-c, --config <path>', '설정 파일 경로')
  .option('--undo', '마지막 정리 되돌리기')
  .action((directory, options) => {
    console.log(chalk.cyan('📁 파일 정리를 시작합니다...\n'))

    try {
      organizeFiles(directory, options)
    } catch (error) {
      console.error(chalk.red(`❌ 오류: ${error.message}`))
      process.exit(1)
    }
  })

// 추가 명령어: 통계 보기
program
  .command('stats <directory>')
  .description('폴더 내 파일 통계 보기')
  .action((directory) => {
    // 통계 표시 로직
  })

// 인자 파싱 시작
program.parse()
```

**핵심 개념: Commander 라이브러리**

| 메서드 | 역할 | 예시 |
|-------|------|------|
| `.argument()` | 필수 인자 정의 | `<directory>` |
| `.option()` | 선택적 옵션 정의 | `-d, --dry-run` |
| `.action()` | 명령어 실행 시 동작 | 함수 전달 |
| `.command()` | 하위 명령어 추가 | `stats`, `undo` |

### Step 3: 핵심 로직 구현

```
> organizer.js에 파일 정리 로직을 구현해줘.
> 확장자별로 카테고리를 나누고,
> 폴더가 없으면 생성하고,
> 파일을 이동시켜줘.
> dry-run 옵션이 있으면 실제로 이동하지 말고 뭘 할지만 보여줘.
```

```javascript
// src/organizer.js
const fs = require('fs')
const path = require('path')
const chalk = require('chalk')

// 파일 분류 규칙 정의
const CATEGORIES = {
  images: {
    extensions: ['.jpg', '.jpeg', '.png', '.gif', '.webp', '.svg', '.ico', '.bmp'],
    icon: '🖼️'
  },
  docs: {
    extensions: ['.pdf', '.doc', '.docx', '.txt', '.md', '.xlsx', '.xls', '.pptx', '.ppt'],
    icon: '📄'
  },
  videos: {
    extensions: ['.mp4', '.mov', '.avi', '.mkv', '.webm', '.flv'],
    icon: '🎬'
  },
  audio: {
    extensions: ['.mp3', '.wav', '.flac', '.aac', '.ogg', '.m4a'],
    icon: '🎵'
  },
  archives: {
    extensions: ['.zip', '.rar', '.7z', '.tar', '.gz', '.bz2'],
    icon: '📦'
  },
  code: {
    extensions: ['.js', '.ts', '.py', '.java', '.cpp', '.c', '.html', '.css', '.json'],
    icon: '💻'
  }
}

/**
 * 파일 확장자로 카테고리 찾기
 * @param {string} filename - 파일 이름
 * @returns {string} - 카테고리 이름
 */
function getCategory(filename) {
  const ext = path.extname(filename).toLowerCase()

  for (const [category, config] of Object.entries(CATEGORIES)) {
    if (config.extensions.includes(ext)) {
      return category
    }
  }

  return 'others'  // 분류되지 않는 파일
}

/**
 * 메인 정리 함수
 * @param {string} directory - 정리할 디렉토리 경로
 * @param {object} options - 옵션 (dryRun, verbose 등)
 */
function organizeFiles(directory, options = {}) {
  const fullPath = path.resolve(directory)

  // 1. 디렉토리 존재 확인
  if (!fs.existsSync(fullPath)) {
    throw new Error(`폴더를 찾을 수 없습니다: ${fullPath}`)
  }

  // 2. 디렉토리인지 확인
  const stats = fs.statSync(fullPath)
  if (!stats.isDirectory()) {
    throw new Error(`디렉토리가 아닙니다: ${fullPath}`)
  }

  // 3. 파일 목록 읽기
  const files = fs.readdirSync(fullPath)
  const results = { moved: 0, skipped: 0, errors: [] }
  const history = []  // 되돌리기용 기록

  console.log(chalk.gray(`총 ${files.length}개 항목을 확인합니다...\n`))

  // 4. 각 파일 처리
  files.forEach(file => {
    const filePath = path.join(fullPath, file)
    const fileStats = fs.statSync(filePath)

    // 디렉토리는 건너뛰기
    if (fileStats.isDirectory()) {
      if (options.verbose) {
        console.log(chalk.gray(`⏭️  건너뜀 (폴더): ${file}`))
      }
      results.skipped++
      return
    }

    // 숨김 파일 건너뛰기 (선택적)
    if (file.startsWith('.')) {
      if (options.verbose) {
        console.log(chalk.gray(`⏭️  건너뜀 (숨김): ${file}`))
      }
      results.skipped++
      return
    }

    // 카테고리 결정
    const category = getCategory(file)
    const categoryConfig = CATEGORIES[category] || { icon: '📁' }
    const targetDir = path.join(fullPath, category)
    const targetPath = path.join(targetDir, file)

    // 5. dry-run 모드: 미리보기만
    if (options.dryRun) {
      console.log(chalk.yellow(
        `${categoryConfig.icon} [미리보기] ${file} → ${category}/`
      ))
      return
    }

    // 6. 실제 이동
    try {
      // 대상 폴더가 없으면 생성
      if (!fs.existsSync(targetDir)) {
        fs.mkdirSync(targetDir, { recursive: true })
        if (options.verbose) {
          console.log(chalk.blue(`📁 폴더 생성: ${category}/`))
        }
      }

      // 같은 이름 파일이 있으면 이름 변경
      let finalPath = targetPath
      if (fs.existsSync(targetPath)) {
        finalPath = getUniqueFilename(targetPath)
        console.log(chalk.yellow(`⚠️  이름 충돌: ${file} → ${path.basename(finalPath)}`))
      }

      // 파일 이동
      fs.renameSync(filePath, finalPath)

      // 기록 저장 (되돌리기용)
      history.push({ from: filePath, to: finalPath })

      if (options.verbose) {
        console.log(chalk.green(
          `${categoryConfig.icon} ${file} → ${category}/`
        ))
      }

      results.moved++

    } catch (error) {
      console.log(chalk.red(`❌ 실패: ${file} - ${error.message}`))
      results.errors.push({ file, error: error.message })
    }
  })

  // 7. 기록 저장
  if (!options.dryRun && history.length > 0) {
    saveHistory(fullPath, history)
  }

  // 8. 결과 요약
  console.log('')
  console.log(chalk.cyan('─'.repeat(40)))
  console.log(chalk.cyan(`정리 완료!`))
  console.log(chalk.green(`  ✓ 이동: ${results.moved}개`))
  console.log(chalk.gray(`  ⏭️  건너뜀: ${results.skipped}개`))
  if (results.errors.length > 0) {
    console.log(chalk.red(`  ❌ 실패: ${results.errors.length}개`))
  }

  if (options.dryRun) {
    console.log(chalk.yellow('\n💡 미리보기 모드입니다. 실제로 이동하려면 --dry-run 옵션을 제거하세요.'))
  }

  return results
}

/**
 * 파일명 중복 시 고유한 이름 생성
 * example.txt → example (1).txt → example (2).txt
 */
function getUniqueFilename(filePath) {
  const dir = path.dirname(filePath)
  const ext = path.extname(filePath)
  const base = path.basename(filePath, ext)

  let counter = 1
  let newPath = filePath

  while (fs.existsSync(newPath)) {
    newPath = path.join(dir, `${base} (${counter})${ext}`)
    counter++
  }

  return newPath
}

/**
 * 되돌리기용 히스토리 저장
 */
function saveHistory(directory, history) {
  const historyPath = path.join(directory, '.organize-history.json')
  fs.writeFileSync(historyPath, JSON.stringify(history, null, 2))
}

/**
 * 마지막 정리 되돌리기
 */
function undoLastOrganize(directory) {
  const historyPath = path.join(directory, '.organize-history.json')

  if (!fs.existsSync(historyPath)) {
    throw new Error('되돌릴 기록이 없습니다')
  }

  const history = JSON.parse(fs.readFileSync(historyPath, 'utf-8'))
  let restored = 0

  history.forEach(({ from, to }) => {
    if (fs.existsSync(to)) {
      fs.renameSync(to, from)
      restored++
      console.log(chalk.yellow(`↩️  ${path.basename(to)} → 원래 위치`))
    }
  })

  // 히스토리 파일 삭제
  fs.unlinkSync(historyPath)

  console.log(chalk.green(`\n✓ ${restored}개 파일 복원 완료`))
}

module.exports = { organizeFiles, undoLastOrganize, getCategory }
```

> 🔥 **프로 팁**: 항상 `--dry-run` 옵션을 먼저 만드세요. 파일을 다루는 도구는 실수하면 되돌리기 어렵습니다.

### Step 4: 로컬에서 테스트

```bash
# 현재 프로젝트를 전역으로 링크
npm link

# 미리보기 모드로 테스트 (안전!)
organize ./downloads --dry-run

# 상세 로그와 함께 실행
organize ./downloads --verbose

# 실제 정리 실행
organize ./downloads

# 되돌리기
organize ./downloads --undo
```

### 🎯 미니 퀴즈

1. `path.resolve()`와 `path.join()`의 차이는?
2. `fs.existsSync()` vs `fs.exists()`의 차이는?
3. `--dry-run` 옵션이 중요한 이유는?

<details>
<summary>정답 보기</summary>

1. `path.resolve()`는 절대 경로를 반환, `path.join()`은 경로를 연결만 함
2. `existsSync`는 동기, `exists`는 비동기 (deprecated)
3. 실제 파일 변경 전에 미리보기로 확인 가능 - 실수 방지!

</details>

---

## 기능 확장하기

### 설정 파일 지원

```
> .organizerc 설정 파일을 읽어서 커스텀 규칙을 지원해줘.
> 사용자가 자기만의 카테고리와 확장자 매핑을 정의할 수 있게.
```

```json
// .organizerc (프로젝트 폴더에 생성)
{
  "categories": {
    "code": [".js", ".ts", ".py", ".go", ".rs"],
    "design": [".psd", ".ai", ".sketch", ".figma", ".xd"],
    "3d": [".blend", ".obj", ".fbx", ".stl"]
  },
  "ignore": ["node_modules", ".git", "*.tmp"],
  "defaultCategory": "misc"
}
```

```javascript
// 설정 파일 로드 함수
function loadConfig(directory) {
  const configPaths = [
    path.join(directory, '.organizerc'),
    path.join(directory, '.organizerc.json'),
    path.join(process.env.HOME, '.organizerc')  // 전역 설정
  ]

  for (const configPath of configPaths) {
    if (fs.existsSync(configPath)) {
      console.log(chalk.gray(`📋 설정 로드: ${configPath}`))
      return JSON.parse(fs.readFileSync(configPath, 'utf-8'))
    }
  }

  return null  // 기본 설정 사용
}
```

### 인터랙티브 모드

```
> inquirer를 사용해서 인터랙티브 모드를 추가해줘.
> 파일별로 어디로 이동할지 물어보는 옵션.
```

```javascript
const inquirer = require('inquirer')

async function interactiveMode(files, fullPath) {
  console.log(chalk.cyan('\n🔄 인터랙티브 모드\n'))

  for (const file of files) {
    const filePath = path.join(fullPath, file)

    // 파일 정보 표시
    const stats = fs.statSync(filePath)
    const size = formatFileSize(stats.size)

    const { action } = await inquirer.prompt([
      {
        type: 'list',
        name: 'action',
        message: `${file} (${size})`,
        choices: [
          { name: '🖼️  images/', value: 'images' },
          { name: '📄 docs/', value: 'docs' },
          { name: '🎬 videos/', value: 'videos' },
          { name: '🎵 audio/', value: 'audio' },
          { name: '📦 archives/', value: 'archives' },
          { name: '⏭️  건너뛰기', value: 'skip' },
          { name: '🗑️  삭제', value: 'delete' }
        ]
      }
    ])

    // 선택에 따라 처리
    if (action === 'skip') continue
    if (action === 'delete') {
      const { confirm } = await inquirer.prompt([
        {
          type: 'confirm',
          name: 'confirm',
          message: `정말 ${file}을 삭제할까요?`,
          default: false
        }
      ])
      if (confirm) fs.unlinkSync(filePath)
    } else {
      // 파일 이동
      moveToCategory(filePath, action)
    }
  }
}

function formatFileSize(bytes) {
  if (bytes < 1024) return bytes + ' B'
  if (bytes < 1024 * 1024) return (bytes / 1024).toFixed(1) + ' KB'
  return (bytes / (1024 * 1024)).toFixed(1) + ' MB'
}
```

> 💡 **비전공자 팁**: inquirer를 사용하면 화살표 키로 선택하는 멋진 메뉴를 만들 수 있어요. 사용자 경험이 훨씬 좋아집니다!

---

## 두 번째 프로젝트: 프로젝트 생성기

create-react-app처럼 프로젝트를 자동 생성하는 도구를 만들어봅시다.

### 목표

```bash
$ create-my-app my-project
? 프로젝트 타입을 선택하세요: (React / Node API / CLI Tool)
? TypeScript를 사용할까요? (Y/n)
? 테스트 프레임워크: (Jest / Vitest / None)

✓ 프로젝트 생성 완료!

다음 단계:
  cd my-project
  npm install
  npm run dev
```

### 왜 이런 도구가 필요한가요?

```
매번 새 프로젝트 시작할 때:
1. 폴더 생성
2. npm init
3. 필요한 패키지 설치
4. 설정 파일 생성 (tsconfig, eslint, prettier...)
5. 폴더 구조 만들기
6. 기본 파일 작성

→ 30분~1시간 소요

CLI 도구로:
1. 명령어 실행
2. 질문에 답하기

→ 1분 소요!
```

### 구현

```
> create-my-app이라는 프로젝트 생성 CLI를 만들어줘.
> inquirer로 사용자 입력을 받고,
> 선택에 따라 템플릿 파일들을 복사하고,
> 필요한 패키지를 package.json에 추가해줘.
```

```javascript
#!/usr/bin/env node
// bin/create-my-app.js

const inquirer = require('inquirer')
const fs = require('fs-extra')
const path = require('path')
const chalk = require('chalk')
const { execSync } = require('child_process')
const ora = require('ora')  // 로딩 스피너

// 프로젝트 타입별 템플릿 정의
const TEMPLATES = {
  react: {
    name: 'React 앱',
    dependencies: ['react', 'react-dom'],
    devDependencies: ['vite', '@vitejs/plugin-react']
  },
  'node-api': {
    name: 'Node.js API',
    dependencies: ['express', 'cors'],
    devDependencies: ['nodemon']
  },
  'cli-tool': {
    name: 'CLI 도구',
    dependencies: ['commander', 'chalk', 'inquirer'],
    devDependencies: []
  }
}

async function main() {
  // 1. 환영 메시지
  console.log(chalk.cyan(`
╔════════════════════════════════════════╗
║                                        ║
║   🚀 Create My App                     ║
║   프로젝트를 빠르게 시작하세요!        ║
║                                        ║
╚════════════════════════════════════════╝
`))

  // 2. 사용자 입력 받기
  const answers = await inquirer.prompt([
    {
      type: 'input',
      name: 'name',
      message: '프로젝트 이름:',
      default: 'my-app',
      validate: (input) => {
        if (/^[a-z0-9-]+$/.test(input)) return true
        return '소문자, 숫자, 하이픈만 사용할 수 있습니다'
      }
    },
    {
      type: 'list',
      name: 'template',
      message: '프로젝트 타입:',
      choices: Object.entries(TEMPLATES).map(([key, val]) => ({
        name: val.name,
        value: key
      }))
    },
    {
      type: 'confirm',
      name: 'typescript',
      message: 'TypeScript를 사용할까요?',
      default: true
    },
    {
      type: 'list',
      name: 'testFramework',
      message: '테스트 프레임워크:',
      choices: [
        { name: 'Jest', value: 'jest' },
        { name: 'Vitest', value: 'vitest' },
        { name: '없음', value: 'none' }
      ]
    },
    {
      type: 'confirm',
      name: 'git',
      message: 'Git 저장소를 초기화할까요?',
      default: true
    }
  ])

  const projectPath = path.join(process.cwd(), answers.name)

  // 3. 폴더 존재 확인
  if (fs.existsSync(projectPath)) {
    const { overwrite } = await inquirer.prompt([{
      type: 'confirm',
      name: 'overwrite',
      message: `${answers.name} 폴더가 이미 존재합니다. 덮어쓸까요?`,
      default: false
    }])

    if (!overwrite) {
      console.log(chalk.yellow('취소되었습니다.'))
      process.exit(0)
    }

    fs.removeSync(projectPath)
  }

  // 4. 프로젝트 생성 시작
  console.log(chalk.cyan('\n📁 프로젝트 생성 중...\n'))
  const spinner = ora()

  try {
    // 4-1. 폴더 생성
    spinner.start('폴더 구조 생성 중...')
    fs.ensureDirSync(projectPath)
    fs.ensureDirSync(path.join(projectPath, 'src'))

    if (answers.template === 'react') {
      fs.ensureDirSync(path.join(projectPath, 'src', 'components'))
      fs.ensureDirSync(path.join(projectPath, 'public'))
    } else if (answers.template === 'node-api') {
      fs.ensureDirSync(path.join(projectPath, 'src', 'routes'))
      fs.ensureDirSync(path.join(projectPath, 'src', 'middleware'))
    } else if (answers.template === 'cli-tool') {
      fs.ensureDirSync(path.join(projectPath, 'bin'))
    }
    spinner.succeed('폴더 구조 생성 완료')

    // 4-2. package.json 생성
    spinner.start('package.json 생성 중...')
    const pkg = createPackageJson(answers)
    fs.writeJsonSync(path.join(projectPath, 'package.json'), pkg, { spaces: 2 })
    spinner.succeed('package.json 생성 완료')

    // 4-3. 템플릿 파일 생성
    spinner.start('템플릿 파일 생성 중...')
    createTemplateFiles(projectPath, answers)
    spinner.succeed('템플릿 파일 생성 완료')

    // 4-4. TypeScript 설정
    if (answers.typescript) {
      spinner.start('TypeScript 설정 중...')
      createTsConfig(projectPath, answers.template)
      spinner.succeed('TypeScript 설정 완료')
    }

    // 4-5. Git 초기화
    if (answers.git) {
      spinner.start('Git 초기화 중...')
      execSync('git init', { cwd: projectPath, stdio: 'ignore' })
      createGitignore(projectPath)
      spinner.succeed('Git 초기화 완료')
    }

    // 4-6. 의존성 설치
    spinner.start('패키지 설치 중... (잠시 기다려주세요)')
    execSync('npm install', { cwd: projectPath, stdio: 'ignore' })
    spinner.succeed('패키지 설치 완료')

    // 5. 완료 메시지
    console.log(chalk.green(`
╔════════════════════════════════════════╗
║                                        ║
║   ✅ ${answers.name} 프로젝트가 생성되었습니다!
║                                        ║
╚════════════════════════════════════════╝

다음 단계:

  ${chalk.cyan(`cd ${answers.name}`)}
  ${chalk.cyan('npm run dev')}

행복한 코딩 되세요! 🎉
`))

  } catch (error) {
    spinner.fail('오류 발생')
    console.error(chalk.red(error.message))
    process.exit(1)
  }
}

function createPackageJson(answers) {
  const template = TEMPLATES[answers.template]
  const ext = answers.typescript ? 'ts' : 'js'

  const pkg = {
    name: answers.name,
    version: '1.0.0',
    description: '',
    main: answers.template === 'cli-tool' ? `bin/index.${ext}` : `src/index.${ext}`,
    scripts: {},
    dependencies: {},
    devDependencies: {}
  }

  // 템플릿별 스크립트
  if (answers.template === 'react') {
    pkg.scripts = {
      dev: 'vite',
      build: 'vite build',
      preview: 'vite preview'
    }
  } else if (answers.template === 'node-api') {
    pkg.scripts = {
      dev: 'nodemon src/index.js',
      start: 'node src/index.js'
    }
  } else if (answers.template === 'cli-tool') {
    pkg.scripts = {
      dev: 'node bin/index.js',
      link: 'npm link'
    }
    pkg.bin = {
      [answers.name]: `./bin/index.${ext}`
    }
  }

  // 테스트 스크립트
  if (answers.testFramework !== 'none') {
    pkg.scripts.test = answers.testFramework
  }

  return pkg
}

function createTemplateFiles(projectPath, answers) {
  // 템플릿별 기본 파일 생성
  // (실제로는 templates/ 폴더에서 복사하는 것이 좋음)
}

function createTsConfig(projectPath, template) {
  const config = {
    compilerOptions: {
      target: 'ES2020',
      module: 'commonjs',
      strict: true,
      esModuleInterop: true,
      skipLibCheck: true,
      outDir: './dist'
    },
    include: ['src/**/*']
  }

  fs.writeJsonSync(
    path.join(projectPath, 'tsconfig.json'),
    config,
    { spaces: 2 }
  )
}

function createGitignore(projectPath) {
  const content = `
node_modules/
dist/
.env
.DS_Store
*.log
`
  fs.writeFileSync(path.join(projectPath, '.gitignore'), content.trim())
}

main().catch(console.error)
```

> 💡 **비전공자 팁**: `ora` 라이브러리를 사용하면 로딩 스피너를 표시할 수 있어요. 사용자가 "뭔가 진행 중이구나"를 알 수 있게 해줍니다.

---

## npm에 배포하기

만든 CLI 도구를 전 세계와 공유할 수 있습니다.

### package.json 설정

```json
{
  "name": "file-organizer-cli",
  "version": "1.0.0",
  "description": "파일을 자동으로 정리하는 CLI 도구",
  "bin": {
    "organize": "./bin/organize.js"
  },
  "files": [
    "bin/",
    "src/"
  ],
  "keywords": ["cli", "file", "organizer", "automation"],
  "author": "your-name",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/your-name/file-organizer-cli"
  }
}
```

**bin 필드**가 핵심입니다. `organize`라는 명령어가 `./bin/organize.js`를 실행합니다.

### 배포 전 체크리스트

```markdown
- [ ] README.md 작성 (사용법, 예시)
- [ ] package.json 정보 확인 (name, version, description)
- [ ] .npmignore 또는 files 필드 설정
- [ ] 테스트 실행 확인
- [ ] npm whoami로 로그인 확인
```

### 배포

```bash
# npm 로그인
npm login

# 패키지 이름 사용 가능한지 확인
npm search file-organizer-cli

# 배포
npm publish

# 버전 업데이트 후 재배포
npm version patch  # 1.0.0 → 1.0.1
npm publish

# 이제 누구나 설치 가능!
npm install -g file-organizer-cli
```

> ⚠️ **주의사항**: npm 패키지 이름은 고유해야 합니다. 이미 있는 이름은 사용할 수 없어요. `@username/package-name` 형식으로 스코프를 붙이면 중복을 피할 수 있습니다.

---

## CLI 도구 설계 원칙

### 1. 명확한 에러 메시지

```javascript
// ❌ 나쁨
console.log('Error')
process.exit(1)

// ✅ 좋음
console.log(chalk.red('❌ 폴더를 찾을 수 없습니다: ' + path))
console.log(chalk.gray('힌트: 상대 경로나 절대 경로를 확인해주세요'))
console.log(chalk.gray('예시: organize ./downloads'))
process.exit(1)
```

### 2. --help 자동 생성

commander가 알아서 해줍니다:

```bash
$ organize --help
Usage: organize [options] <directory>

파일을 종류별로 자동 정리합니다

Options:
  -V, --version       output the version number
  -d, --dry-run       실제로 이동하지 않고 미리보기만
  -v, --verbose       상세 로그 출력
  -c, --config <path> 설정 파일 경로
  --undo              마지막 정리 되돌리기
  -h, --help          display help for command
```

### 3. 진행 상황 표시

```
> 파일이 많을 때 진행률 바를 보여줘. cli-progress 사용.
```

```javascript
const cliProgress = require('cli-progress')

function processFilesWithProgress(files) {
  const bar = new cliProgress.SingleBar({
    format: '진행 |{bar}| {percentage}% | {value}/{total} 파일',
    barCompleteChar: '█',
    barIncompleteChar: '░'
  }, cliProgress.Presets.shades_classic)

  bar.start(files.length, 0)

  files.forEach((file, index) => {
    // 파일 처리...
    bar.update(index + 1)
  })

  bar.stop()
}
```

### 4. 색상 있는 출력

```javascript
const chalk = require('chalk')

console.log(chalk.green('✓ 성공'))
console.log(chalk.red('✗ 실패'))
console.log(chalk.yellow('⚠ 경고'))
console.log(chalk.blue('ℹ 정보'))
console.log(chalk.gray('  세부사항...'))
console.log(chalk.bold('중요한 내용'))
console.log(chalk.cyan.underline('https://링크'))
```

---

## 유용한 라이브러리

| 라이브러리 | 용도 | 예시 |
|-----------|------|------|
| `commander` | 인자/옵션 파싱 | `program.option('-v, --verbose')` |
| `inquirer` | 인터랙티브 프롬프트 | 선택 메뉴, 확인 질문 |
| `chalk` | 컬러 출력 | `chalk.red('에러')` |
| `ora` | 스피너 애니메이션 | 로딩 표시 |
| `cli-progress` | 진행률 바 | 파일 처리 진행률 |
| `fs-extra` | 향상된 파일 시스템 | `fs.copySync`, `fs.ensureDir` |
| `glob` | 파일 패턴 매칭 | `**/*.js` |
| `boxen` | 박스 UI | 환영 메시지, 결과 표시 |
| `figlet` | ASCII 아트 텍스트 | 큰 제목 표시 |
| `update-notifier` | 업데이트 알림 | 새 버전 알림 |

---

## 📝 실습 과제

### 레벨 1: 기본 (초보자)

파일 정리 도구를 그대로 따라 만들어보세요.

**체크리스트:**
- [ ] `organize ./폴더명` 으로 실행되는가?
- [ ] `--dry-run` 옵션이 작동하는가?
- [ ] 에러 메시지가 명확한가?

### 레벨 2: 응용 (중급자)

```
> git 커밋 메시지를 분석해서 통계를 보여주는 CLI 도구 만들어줘.
> - 커밋 타입별 개수 (feat, fix, docs 등)
> - 가장 활발한 요일/시간
> - 기여자별 커밋 수
```

**체크리스트:**
- [ ] git 로그를 읽어오는가?
- [ ] 통계가 표나 그래프로 표시되는가?
- [ ] 날짜 범위 필터가 있는가?

### 레벨 3: 도전 (심화)

```
> 마크다운 파일들을 합쳐서 PDF나 HTML 문서로 만드는 도구

> 여러 프로젝트의 package.json을 분석해서
> 공통 의존성과 버전 충돌을 찾아주는 도구

> 이미지 파일들을 일괄 리사이즈/압축하는 도구
```

---

## 🏆 도전 과제

### 도전 1: 플러그인 시스템

사용자가 자신만의 분류 규칙을 추가할 수 있게:

```javascript
// ~/.organize/plugins/my-rule.js
module.exports = {
  name: 'screenshots',
  match: (filename) => filename.startsWith('Screenshot'),
  category: 'screenshots'
}
```

### 도전 2: 크로스 플랫폼

Windows, Mac, Linux 모두에서 동작하도록:

```javascript
const os = require('os')
const homeDir = os.homedir()
const pathSep = path.sep  // Windows: \, Unix: /
```

### 도전 3: 자동 완성

Bash/Zsh 자동 완성 스크립트 생성:

```bash
# organize <TAB>으로 폴더 목록 표시
```

---

## 안 되면? 문제 해결 팁

### npm link 후 "command not found" 오류

```bash
# npm bin이 PATH에 있는지 확인
npm bin -g
# 출력된 경로가 $PATH에 있어야 함

# npx로 실행해보기
npx organize ./downloads

# 또는 절대 경로로 실행
$(npm bin -g)/organize ./downloads
```

### "Permission denied" 실행 권한 오류

```bash
# 파일에 실행 권한이 있는지 확인
ls -la bin/organize.js
# -rwxr-xr-x 이어야 함

# 권한 부여
chmod +x bin/organize.js

# shebang 라인이 올바른지 확인 (첫 줄이 이래야 함):
#!/usr/bin/env node
```

### 파일이 안 움직이거나 아무 일도 안 일어날 때

```bash
# verbose 플래그로 무슨 일이 일어나는지 확인
organize ./downloads --verbose

# 디렉토리가 존재하는지 확인
ls ./downloads

# 절대 경로로 시도
organize /Users/yourname/downloads

# 권한 확인
ls -la ./downloads
```

### "Cannot find module 'commander'" 오류

```bash
# 의존성을 설치했는지 확인
cd your-project
npm install

# 또는 직접 설치
npm install commander chalk

# package.json의 dependencies 확인
cat package.json | grep -A 10 "dependencies"
```

### Windows에서 한글이 깨지는 경우

```javascript
// 파일 읽을 때 인코딩 명시
fs.readFileSync(path, { encoding: 'utf8' })

// 또는 터미널 인코딩 설정
// PowerShell: chcp 65001
```

---

## 자주 하는 실수

### 1. Shebang 라인을 빼먹음

```javascript
// ❌ 틀림 - 직접 실행 안 됨
const { program } = require('commander')

// ✅ 맞음 - 첫 줄에 shebang 필요
#!/usr/bin/env node
const { program } = require('commander')
```

### 2. 인자가 없을 때 처리를 안 함

```javascript
// ❌ 틀림 - 디렉토리를 안 주면 크래시
const dir = process.argv[2]
fs.readdirSync(dir)  // dir이 undefined면 에러!

// ✅ 맞음 - 먼저 검증
const dir = process.argv[2]
if (!dir) {
  console.log('디렉토리 경로를 입력해주세요')
  console.log('사용법: organize <directory>')
  process.exit(1)
}
```

### 3. 경로를 하드코딩함

```javascript
// ❌ 틀림 - 내 컴퓨터에서만 동작
const targetDir = '/Users/yourname/downloads'

// ✅ 맞음 - 상대 경로나 설정 가능한 경로 사용
const targetDir = process.argv[2] || './downloads'

// 또는 홈 디렉토리 기준
const os = require('os')
const defaultDir = path.join(os.homedir(), 'Downloads')
```

### 4. 테스트할 때 --dry-run을 안 씀

파일을 수정하는 CLI에는 항상 dry-run 옵션을 추가하시기 바랍니다. 테스트하다가 중요한 파일을 실수로 삭제하거나 옮기는 것을 방지할 수 있습니다.

```bash
# 항상 먼저 dry-run으로 테스트!
organize ./important-folder --dry-run

# 결과 확인 후 실제 실행
organize ./important-folder
```

### 5. 옵션 파싱을 잘못함

```javascript
// 주의 - commander가 kebab-case를 camelCase로 변환함
.option('-d, --dry-run')

// --dry-run은 options.dryRun이 됨 (하이픈 제거, 카멜케이스)
if (options.dryRun) {  // options['dry-run'] 아님!
  // ...
}
```

### 6. 비동기 처리를 잘못함

```javascript
// ❌ 틀림 - 파일 읽기가 끝나기 전에 종료될 수 있음
files.forEach(async (file) => {
  await processFile(file)
})
console.log('완료!')  // 실제로는 아직 완료 안 됨

// ✅ 맞음 - 모든 작업이 끝날 때까지 대기
for (const file of files) {
  await processFile(file)
}
console.log('완료!')

// 또는 Promise.all 사용 (병렬 처리)
await Promise.all(files.map(file => processFile(file)))
console.log('완료!')
```

---

## 용어 정리

| 용어 | 의미 |
|------|------|
| CLI | Command Line Interface, 명령줄 인터페이스 |
| Shebang | `#!/usr/bin/env node` - 스크립트 실행기 지정 |
| process.argv | 명령줄 인자 배열 |
| bin 필드 | package.json에서 실행 파일 지정 |
| npm link | 로컬 패키지를 전역으로 설치 (개발용) |
| 옵션/플래그 | `-v`, `--verbose` 같은 설정 스위치 |
| 인자 | 명령어에 전달되는 값 |
| stdout/stderr | 표준 출력/표준 에러 |

---

## 다음 챕터 미리보기

CLI 도구를 만들었으니, 이제 **챗봇**을 만들어봅니다!

- **Chapter 18**: Discord/Slack 봇 만들기
  - 메시지에 자동 응답
  - 명령어로 기능 실행
  - 24시간 동작하는 자동화

CLI와 챗봇의 공통점: 둘 다 **텍스트 기반 인터페이스**입니다. CLI에서 배운 명령어 처리, 인자 파싱이 챗봇에서도 그대로 적용됩니다!

---

## 정리

이번 챕터에서 배운 것:
- [x] CLI 도구의 기본 구조와 작동 원리
- [x] process.argv로 인자 처리
- [x] commander로 옵션 파싱
- [x] 파일 시스템(fs) 조작
- [x] inquirer로 인터랙티브 프롬프트
- [x] chalk, ora로 멋진 출력
- [x] npm 배포

CLI 도구는 개발자로서의 생산성을 크게 높여줍니다. 반복되는 작업이 있다면 CLI로 자동화해보세요!

[Chapter 18: 챗봇 만들기](../Chapter18-Chatbots/README.ko.md)로 넘어가세요.

---

## 📖 더 알아보기

### 추천 자료

**공식 문서:**
- [Node.js 공식 문서](https://nodejs.org/docs/latest/api/) - Node.js API 레퍼런스
- [Commander.js 문서](https://github.com/tj/commander.js) - CLI 인자 파싱 라이브러리
- [npm 패키지 배포 가이드](https://docs.npmjs.com/packages-and-modules/contributing-packages-to-the-registry)

**영상 자료:**
- [Node.js CLI 도구 만들기 (YouTube)](https://www.youtube.com/results?search_query=nodejs+cli+tool+tutorial)
- [npm 패키지 배포하기 (YouTube)](https://www.youtube.com/results?search_query=publish+npm+package+tutorial)
- [Inquirer.js 사용법 (YouTube)](https://www.youtube.com/results?search_query=inquirer.js+tutorial)

**읽을거리:**
- [CLI 디자인 가이드라인](https://clig.dev/) - 좋은 CLI 도구 설계 원칙
- [12 Factor CLI Apps](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46) - CLI 앱 설계 패턴

**관련 도구:**
- [Chalk](https://github.com/chalk/chalk) - 터미널 컬러 출력
- [Ora](https://github.com/sindresorhus/ora) - 로딩 스피너
- [Inquirer.js](https://github.com/SBoudrias/Inquirer.js) - 인터랙티브 프롬프트
- [cli-progress](https://github.com/npkgz/cli-progress) - 진행률 바

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
