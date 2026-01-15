# Chapter 15: CLI 도구 만들기

[English](./README.md) | **한국어**

## 이 챕터에서 배우는 것

- Node.js CLI 도구의 구조
- 사용자 입력 처리
- 파일 시스템 조작
- npm 패키지로 배포하기

---

## 왜 필요합니까?

**CLI 도구가 빛나는 실제 상황들:**

- **다운로드 폴더가 난장판입니다** - 수백 개의 파일이 뒤섞여 있습니다. CLI 도구가 몇 초 만에 자동으로 정리해줍니다
- **똑같은 프로젝트 구조를 반복해서 만듭니다** - 새 프로젝트마다 같은 폴더와 파일이 필요합니다. 자동화하시기 바랍니다
- **많은 파일을 한꺼번에 처리해야 합니다** - 파일 100개 이름 바꾸기, 이미지 리사이즈, 포맷 변환 등 CLI 도구는 일괄 작업을 쉽게 처리합니다
- **자동화 도구를 다른 사람과 공유하고 싶습니다** - npm에 배포하면 누구나 한 줄 명령어로 설치할 수 있습니다

CLI 도구를 **반복 작업에 지치지 않는 나만의 로봇 비서**라고 생각하시기 바랍니다.

---

## 쉬운 비유: CLI 도구는 주방 가전과 같습니다

주스를 만든다고 상상해 보시기 바랍니다:
- **믹서기 없이**: 과일을 하나하나 손으로 짜야 하며, 시간이 많이 걸립니다
- **믹서기로**: 과일 넣고 버튼 누르면 끝입니다

CLI 도구도 마찬가지입니다:
- **CLI 도구 없이**: 파일을 수동으로 옮기고, 폴더 만들고, 명령어 반복 입력
- **CLI 도구로**: 명령어 한 번 실행하면 모든 게 자동으로 처리

우리가 매일 쓰는 `git`, `npm`, `npx`도 모두 누군가가 시간을 아끼려고 만든 CLI 도구입니다.

---

## 왜 CLI 도구입니까?

CLI(Command Line Interface) 도구는 개발자의 생산성을 높여주는 핵심 도구입니다:
- 반복 작업 자동화
- 프로젝트 초기 설정
- 파일 일괄 처리
- 개발 워크플로우 개선

우리가 매일 쓰는 `git`, `npm`, `npx`도 모두 CLI 도구입니다.

**CLI 도구 요청 팁:**

```
> 프로젝트 폴더 구조를 자동으로 생성하는 CLI 도구 만들어줘.
> 사용자가 프로젝트 이름을 입력하면 기본 폴더들(src, tests, docs)을 만들고
> package.json도 초기화해줘.
```

입력(인자, 옵션)과 출력(어떤 동작을 할지)을 명확히 설명하시기 바랍니다.

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
├── package.json
├── bin/
│   └── organize.js    # CLI 진입점
└── src/
    └── organizer.js   # 핵심 로직
```

### Step 2: CLI 진입점 만들기

```javascript
#!/usr/bin/env node
// bin/organize.js

const { program } = require('commander')
const { organizeFiles } = require('../src/organizer')

program
  .name('organize')
  .description('파일을 종류별로 자동 정리합니다')
  .argument('<directory>', '정리할 폴더 경로')
  .option('-d, --dry-run', '실제로 이동하지 않고 미리보기만')
  .option('-v, --verbose', '상세 로그 출력')
  .action((directory, options) => {
    organizeFiles(directory, options)
  })

program.parse()
```

**핵심 개념: Shebang**

`#!/usr/bin/env node`는 이 파일을 Node.js로 실행하라는 의미입니다. 이게 있어야 `./organize.js`로 직접 실행할 수 있습니다.

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

const CATEGORIES = {
  images: ['.jpg', '.jpeg', '.png', '.gif', '.webp', '.svg'],
  docs: ['.pdf', '.doc', '.docx', '.txt', '.md', '.xlsx'],
  videos: ['.mp4', '.mov', '.avi', '.mkv'],
  audio: ['.mp3', '.wav', '.flac'],
  archives: ['.zip', '.rar', '.7z', '.tar', '.gz']
}

function getCategory(filename) {
  const ext = path.extname(filename).toLowerCase()
  for (const [category, extensions] of Object.entries(CATEGORIES)) {
    if (extensions.includes(ext)) return category
  }
  return 'others'
}

function organizeFiles(directory, options) {
  const fullPath = path.resolve(directory)

  if (!fs.existsSync(fullPath)) {
    console.log(chalk.red(`❌ 폴더를 찾을 수 없습니다: ${fullPath}`))
    process.exit(1)
  }

  const files = fs.readdirSync(fullPath)
  const results = { moved: 0, skipped: 0 }

  files.forEach(file => {
    const filePath = path.join(fullPath, file)

    // 디렉토리는 건너뛰기
    if (fs.statSync(filePath).isDirectory()) {
      results.skipped++
      return
    }

    const category = getCategory(file)
    const targetDir = path.join(fullPath, category)
    const targetPath = path.join(targetDir, file)

    if (options.dryRun) {
      console.log(chalk.yellow(`[미리보기] ${file} → ${category}/`))
    } else {
      if (!fs.existsSync(targetDir)) {
        fs.mkdirSync(targetDir, { recursive: true })
      }
      fs.renameSync(filePath, targetPath)
      if (options.verbose) {
        console.log(chalk.green(`✓ ${file} → ${category}/`))
      }
      results.moved++
    }
  })

  console.log(chalk.cyan(`\n정리 완료: ${results.moved}개 이동, ${results.skipped}개 건너뜀`))
}

module.exports = { organizeFiles }
```

### Step 4: 로컬에서 테스트

```bash
# 현재 프로젝트를 전역으로 링크
npm link

# 이제 어디서든 사용 가능
organize ./downloads --dry-run
organize ./downloads --verbose
```

---

## 따라해보세요: 최소 동작 예제

전체 파일 정리 도구를 만들기 전에, 기본을 이해하기 위해 아주 간단한 CLI 도구부터 만들어봅시다:

**1. `hello-cli.js`라는 파일 하나를 만드세요:**

```javascript
#!/usr/bin/env node

// 명령줄 인자 가져오기 (처음 두 개는 건너뜀: node 경로와 스크립트 경로)
const args = process.argv.slice(2)
const name = args[0] || 'World'

console.log(`안녕하세요, ${name}님!`)
console.log('첫 번째 CLI 도구를 만들었어요!')
```

**2. 실행 권한을 주고 실행:**

```bash
# 실행 가능하게 만들기
chmod +x hello-cli.js

# 실행!
./hello-cli.js
# 출력: 안녕하세요, World님!

./hello-cli.js 철수
# 출력: 안녕하세요, 철수님!
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
npm link
greet 영희
# 출력: 안녕하세요, 영희님!
```

이것이 전부입니다. 이제 핵심 개념을 이해했습니다. 나머지는 모두 이 기초 위에 쌓아가는 것입니다.

---

## 기능 확장하기

### 설정 파일 지원

```
> .organizerc 설정 파일을 읽어서 커스텀 규칙을 지원해줘.
> 사용자가 자기만의 카테고리와 확장자 매핑을 정의할 수 있게.
```

```json
// .organizerc
{
  "categories": {
    "code": [".js", ".ts", ".py", ".go"],
    "design": [".psd", ".ai", ".sketch", ".figma"]
  },
  "ignore": ["node_modules", ".git"]
}
```

### 실행 취소 기능

```
> 파일 이동 전에 원래 위치를 기록해두고,
> organize --undo로 되돌릴 수 있게 해줘.
```

```javascript
// 이동 기록 저장
const history = []
history.push({ from: filePath, to: targetPath })
fs.writeFileSync('.organize-history.json', JSON.stringify(history))

// 되돌리기
function undoLastOrganize() {
  const history = JSON.parse(fs.readFileSync('.organize-history.json'))
  history.forEach(({ from, to }) => {
    fs.renameSync(to, from)
  })
}
```

### 인터랙티브 모드

```
> inquirer를 사용해서 인터랙티브 모드를 추가해줘.
> 파일별로 어디로 이동할지 물어보는 옵션.
```

```javascript
const inquirer = require('inquirer')

async function interactiveMode(files) {
  for (const file of files) {
    const { action } = await inquirer.prompt([
      {
        type: 'list',
        name: 'action',
        message: `${file} 파일을 어디로 이동할까요?`,
        choices: ['images', 'docs', 'videos', 'skip', 'delete']
      }
    ])
    // 선택에 따라 처리
  }
}
```

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

const inquirer = require('inquirer')
const fs = require('fs-extra')
const path = require('path')
const chalk = require('chalk')
const { execSync } = require('child_process')

async function main() {
  const answers = await inquirer.prompt([
    {
      type: 'input',
      name: 'name',
      message: '프로젝트 이름:',
      default: 'my-app'
    },
    {
      type: 'list',
      name: 'template',
      message: '프로젝트 타입:',
      choices: ['React', 'Node API', 'CLI Tool']
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
      choices: ['Jest', 'Vitest', 'None']
    }
  ])

  const projectPath = path.join(process.cwd(), answers.name)

  console.log(chalk.cyan('\n📁 프로젝트 생성 중...\n'))

  // 1. 폴더 생성
  fs.ensureDirSync(projectPath)

  // 2. 템플릿 복사
  const templatePath = path.join(__dirname, '../templates', answers.template.toLowerCase())
  fs.copySync(templatePath, projectPath)

  // 3. package.json 수정
  const pkgPath = path.join(projectPath, 'package.json')
  const pkg = require(pkgPath)
  pkg.name = answers.name

  if (answers.typescript) {
    pkg.devDependencies.typescript = '^5.0.0'
  }

  fs.writeJsonSync(pkgPath, pkg, { spaces: 2 })

  // 4. 의존성 설치
  console.log(chalk.yellow('📦 패키지 설치 중...'))
  execSync('npm install', { cwd: projectPath, stdio: 'inherit' })

  console.log(chalk.green(`\n✅ ${answers.name} 프로젝트가 생성되었습니다!`))
  console.log(chalk.gray(`\n  cd ${answers.name}\n  npm run dev\n`))
}

main().catch(console.error)
```

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
  "keywords": ["cli", "file", "organizer", "automation"],
  "author": "your-name",
  "license": "MIT"
}
```

**bin 필드**가 핵심입니다. `organize`라는 명령어가 `./bin/organize.js`를 실행합니다.

### 배포

```bash
# npm 로그인
npm login

# 배포
npm publish

# 이제 누구나 설치 가능
npm install -g file-organizer-cli
```

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
process.exit(1)
```

### 2. --help 자동 생성

commander가 알아서 해줍니다:

```bash
$ organize --help
Usage: organize [options] <directory>

파일을 종류별로 자동 정리합니다

Options:
  -d, --dry-run  실제로 이동하지 않고 미리보기만
  -v, --verbose  상세 로그 출력
  -h, --help     display help for command
```

### 3. 진행 상황 표시

```
> 파일이 많을 때 진행률 바를 보여줘. cli-progress 사용.
```

```javascript
const cliProgress = require('cli-progress')

const bar = new cliProgress.SingleBar({}, cliProgress.Presets.shades_classic)
bar.start(files.length, 0)

files.forEach((file, index) => {
  // 처리...
  bar.update(index + 1)
})

bar.stop()
```

---

## 유용한 라이브러리

| 라이브러리 | 용도 |
|-----------|------|
| `commander` | 인자/옵션 파싱 |
| `inquirer` | 인터랙티브 프롬프트 |
| `chalk` | 컬러 출력 |
| `ora` | 스피너 애니메이션 |
| `cli-progress` | 진행률 바 |
| `fs-extra` | 향상된 파일 시스템 |
| `glob` | 파일 패턴 매칭 |
| `boxen` | 박스 UI |

---

## 실습 과제

### 기본 과제

```
> git 커밋 메시지를 분석해서 통계를 보여주는 CLI 도구 만들어줘.
> - 커밋 타입별 개수 (feat, fix, docs 등)
> - 가장 활발한 요일/시간
> - 기여자별 커밋 수
```

### 심화 과제

```
> 마크다운 파일들을 합쳐서 PDF나 HTML 문서로 만드는 도구

> 여러 프로젝트의 package.json을 분석해서
> 공통 의존성과 버전 충돌을 찾아주는 도구

> 이미지 파일들을 일괄 리사이즈/압축하는 도구
```

---

## 안 되면? 문제 해결 팁

### npm link 후 "command not found" 오류

```bash
# npm bin이 PATH에 있는지 확인
npm bin -g

# npx로 실행해보기
npx organize ./downloads
```

### "Permission denied" 실행 권한 오류

```bash
# 파일에 실행 권한이 있는지 확인
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
```

### "Cannot find module 'commander'" 오류

```bash
# 의존성을 설치했는지 확인
cd your-project
npm install

# 또는 직접 설치
npm install commander chalk
```

---

## 자주 하는 실수

### 1. Shebang 라인을 빼먹음

```javascript
// 틀림 - 직접 실행 안 됨
const { program } = require('commander')

// 맞음 - 첫 줄에 shebang 필요
#!/usr/bin/env node
const { program } = require('commander')
```

### 2. 인자가 없을 때 처리를 안 함

```javascript
// 틀림 - 디렉토리를 안 주면 크래시
const dir = process.argv[2]
fs.readdirSync(dir)  // dir이 undefined면 에러!

// 맞음 - 먼저 검증
const dir = process.argv[2]
if (!dir) {
  console.log('디렉토리 경로를 입력해주세요')
  process.exit(1)
}
```

### 3. 경로를 하드코딩함

```javascript
// 틀림 - 내 컴퓨터에서만 동작
const targetDir = '/Users/yourname/downloads'

// 맞음 - 상대 경로나 설정 가능한 경로 사용
const targetDir = process.argv[2] || './downloads'
```

### 4. 테스트할 때 --dry-run을 안 씀

파일을 수정하는 CLI에는 항상 dry-run 옵션을 추가하시기 바랍니다. 테스트하다가 중요한 파일을 실수로 삭제하거나 옮기는 것을 방지할 수 있습니다.

### 5. 옵션 파싱을 잘못함

```javascript
// 주의 - options.dryRun으로 접근해야 함
.option('-d, --dry-run')

// commander가 camelCase로 변환함
// --dry-run은 options.dryRun이 됨
```

---

## 정리

이번 챕터에서 배운 것:
- [x] CLI 도구의 기본 구조
- [x] 인자와 옵션 처리
- [x] 파일 시스템 조작
- [x] 인터랙티브 프롬프트
- [x] npm 배포

CLI 도구는 개발자로서의 생산성을 크게 높여줍니다. 반복되는 작업이 있다면 CLI로 자동화해보세요.

다음 챕터에서는 Discord/Slack 봇을 만들어봅니다.

[Chapter 16: 챗봇 만들기](../Chapter16/README.ko.md)로 넘어가세요.
