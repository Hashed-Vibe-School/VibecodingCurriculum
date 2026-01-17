# Chapter 30: Base 스마트 컨트랙트

## 💬 질문하기
[Discord](https://discord.gg/your-invite-link) - 막히면 여기서 질문하세요!

## 🎯 이 챕터의 목표

이 챕터를 완료하면:
- 스마트 컨트랙트가 무엇이고 어떻게 작동하는지 이해할 수 있습니다
- Claude와 함께 첫 번째 Solidity 스마트 컨트랙트를 작성할 수 있습니다!
- Base Sepolia 테스트넷에 컨트랙트를 배포할 수 있습니다
- 배포된 컨트랙트와 상호작용할 수 있습니다
- 컨트랙트용 프론트엔드를 만들 수 있습니다
- **최종 프로젝트 완료: NFT 기반 커뮤니티**
- Web3 스마트 컨트랙트 바이브코딩 베스트 프랙티스를 배웁니다

## ⏱️ 예상 소요 시간
**3-4시간**

## 📋 필요한 것
- **Chapters 28-29 완료** - 두 챕터의 모든 스킬 사용
- Chapter 28에서 설정한 MetaMask와 Base Sepolia 테스트넷
- faucet에서 받은 테스트넷 ETH
- Node.js 설치

## 🔗 Web3 여정의 정점

드디어 - 전체 커리큘럼의 **마지막 챕터**! 여기까지 오느라 수고하셨습니다:

```
┌─────────────────────────────────────────────────────────────┐
│              이 챕터에 가져오는 스킬                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Chapter 28 (Web3 기초)에서:                                 │
│  ✅ MetaMask 설정 및 지갑 관리                               │
│  ✅ 트랜잭션 이해 (가스, wei, 서명)                           │
│  ✅ 탈중앙화 스토리지용 IPFS                                  │
│  ✅ 프론트엔드-블록체인 연결용 wagmi/viem                     │
│  ✅ 기존 컨트랙트에 연결하는 dapp 빌드                        │
│                                                             │
│  Chapter 29 (Farcaster Frames)에서:                         │
│  ✅ 인터랙티브 Web3 경험 빌드                                │
│  ✅ 민팅과 트랜잭션에 기존 컨트랙트 사용                       │
│  ✅ 사용자가 온체인 액션과 상호작용하는 방법 이해              │
│  ✅ Web3 앱을 프로덕션에 배포                                 │
│                                                             │
│  Chapter 30에서 새로운 것:                                   │
│  🆕 Solidity로 나만의 스마트 컨트랙트 작성                    │
│  🆕 직접 만든 컨트랙트 배포                                   │
│  🆕 커스텀 로직으로 완전한 dapp 빌드                          │
│  🆕 최종 프로젝트: 풀스택 Web3 애플리케이션                   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 최종 진화

| 챕터 | 역할 | 핵심 스킬 |
|------|------|-----------|
| 28 | 소비자 | 기존 dapp 사용 |
| 29 | 빌더 | 기존 컨트랙트 위에 빌드 |
| **30** | **크리에이터** | **나만의 컨트랙트 생성** |

> 💡 **큰 그림**: Ch28에서 Web3 사용법을 배웠어요. Ch29에서 그 위에 빌드했어요. 이제 Ch30에서 기반 레이어 자체를 만들어요 - 다른 사람들이 위에 빌드할 수 있는 스마트 컨트랙트!

---

## ⚠️ 중요 경고: 스마트 컨트랙트 보안

> 🚨 **전체 WEB3 커리큘럼에서 가장 중요한 섹션**

### 스마트 컨트랙트가 다른 이유

| 일반 코드 | 스마트 컨트랙트 코드 |
|----------|---------------------|
| 버그 = 고치고 재배포 | 버그 = **영구적** (보통 고칠 수 없음) |
| 해킹 = 비밀번호 재설정 | 해킹 = **자금 영원히 도난** |
| 로컬에서 테스트 | **실제 블록체인**에서 완벽하게 작동해야 함 |
| 빠르게 반복 가능 | 모든 배포가 **실제 돈** 비용 |
| 실수는 복구 가능 | 실수는 **치명적**일 수 있음 |

### 스마트 컨트랙트 바이브코딩 딜레마

```
You: 지금까지 모든 걸 Claude와 바이브코딩해왔어. 스마트 컨트랙트도 똑같이 해도 돼?

Claude: 솔직한 진실을 말해줄게요:

        바이브코딩이 좋은 경우:
        ✅ Solidity 문법 배우기
        ✅ 개념 이해하기
        ✅ 테스트넷에서 빌드하기
        ✅ 아이디어 프로토타이핑
        ✅ 기본 컨트랙트 만들기

        바이브코딩이 위험한 경우:
        ❌ 실제 돈이 있는 프로덕션 컨트랙트
        ❌ 복잡한 DeFi 로직
        ❌ 상당한 가치를 처리하는 모든 것

        왜?:
        1. AI가 취약한 코드를 작성할 수 있음
        2. 이해하지 못하는 것을 완전히 검토할 수 없음
        3. 블록체인은 실수를 영구적으로 만듦
        4. 해커들이 AI가 생성한 컨트랙트를 특별히 찾아다님
```

### 안전한 접근법

```
학습용 (이 챕터):
1. Claude와 테스트넷에서만 빌드
2. 작은 테스트 금액 사용
3. 이해에 집중

프로덕션용 (실제 돈):
1. 전문 감사자 고용
2. 검증된 템플릿 사용 (OpenZeppelin)
3. 아주 작은 금액으로 시작
4. 여러 번 검토 받기
5. 보험 고려
```

---

## 스마트 컨트랙트란?

### 블록체인에 사는 코드

```
You: 5살에게 스마트 컨트랙트를 설명하듯 해줘

Claude: 🎰 마법의 자판기를 상상해보세요:

        일반 자판기:
        - 천 원 넣기
        - B4 누르기
        - 과자 받기
        - 하지만 주인이... 가격 바꾸거나, 돈 훔치거나, 공기만 줄 수 있음

        스마트 컨트랙트 "자판기":
        - 천 원 넣기
        - B4 누르기
        - 과자 받기
        - 보장됨. 창시자도 속일 수 없음.

        왜 "스마트"?
        - 규칙이 코드로 작성됨
        - 코드가 자동으로 실행됨
        - 모든 사람이 규칙을 볼 수 있음
        - 규칙은 절대 바뀔 수 없음 (불변)

        수학으로 작동하는 다이아몬드로 만든 자판기 같아요.
        아무도 침입할 수 없어요. 아무도 바꿀 수 없어요.
        그냥... 영원히 작동해요.
```

### "로봇 직원" 비유

```
🤖 스마트 컨트랙트를 이런 직원이라고 생각해보세요:

   ✅ 절대 휴가를 안 감
   ✅ 절대 실수를 안 함
   ✅ 절대 편애를 안 함
   ✅ 절대 월급 인상을 요구 안 함
   ✅ 절대 뇌물을 안 받음
   ✅ 24/7/365 일함

   ❌ 하지만... 잘못된 지시를 썼으면 절대 수정 불가
   ❌ 버그가 있는 코드를 배포하면 영원히 나쁜 규칙을 따름
   ❌ "이번만 예외"를 만들 수 없음

   그래서 스마트 컨트랙트 개발에는 특별한 주의가 필요해요!
```

### 핵심 스마트 컨트랙트 개념

| 개념 | 설명 | 비유 |
|------|------|------|
| **컨트랙트** | 블록체인의 프로그램 | 자판기 |
| **상태** | 컨트랙트에 저장된 데이터 | 기계 안의 물품 |
| **함수** | 컨트랙트가 할 수 있는 액션 | 기계의 버튼 |
| **이벤트** | 일어난 일의 로그 | 영수증 |
| **가스** | 코드 실행 수수료 | 기계의 전기료 |
| **불변** | 배포 후 변경 불가 | 용접으로 봉인됨 |

### 왜 Solidity인가?

```
You: 왜 특별한 언어가 필요해? JavaScript는 왜 안 돼?

Claude: Solidity는 블록체인의 고유한 필요를 위해 설계됐어요:

        1. 결정론적
           - 모든 컴퓨터에서 정확히 같게 실행되어야 함
           - JavaScript는 변동이 너무 많음

        2. 가스 인식
           - 모든 작업이 돈이 듦
           - 비용 최적화 필요

        3. 보안 우선
           - 안전한 돈 처리를 위한 내장 패턴
           - 무엇이 돈이 드는지 명시적

        4. 상태 관리
           - 데이터가 블록체인에 영원히 유지됨
           - 임시 앱 데이터와 다름

        좋은 소식:
        - 문법이 JavaScript와 비슷함
        - Claude가 대신 써줄 수 있음
        - 무엇을 하는지만 이해하면 됨
```

---

## Step 1: 개발 환경

### 1.1 도구 선택

```
You: 스마트 컨트랙트를 쓰려면 뭐가 필요해?

Claude: 여러 옵션이 있어요:

        옵션 1: Remix IDE (가장 쉬움 - 처음에 이걸 사용할 거예요)
        - 브라우저 기반, 설치 필요 없음
        - 학습에 좋음
        - remix.ethereum.org

        옵션 2: Foundry (프로들에게 가장 인기)
        - 빠르고 강력함
        - 커맨드라인 기반
        - 나중에 시도할 거예요

        옵션 3: Hardhat (역시 인기)
        - JavaScript 기반
        - 좋은 생태계

        학습용으로, Remix로 시작합시다!
```

### 1.2 Remix IDE 열기

```
You: Remix 설정하는 거 안내해줘

Claude: 시작해봅시다:

        1. https://remix.ethereum.org 열기

        2. 보이는 것:
           - 파일 탐색기 (왼쪽)
           - 코드 에디터 (가운데)
           - 터미널 (아래)

        3. 새 파일 만들기:
           - "contracts" 폴더 우클릭
           - "New File"
           - "MyFirstContract.sol"로 이름 짓기

        4. MetaMask 연결:
           - "Deploy" 탭 클릭 (왼쪽 사이드바)
           - Environment: "Injected Provider - MetaMask"
           - MetaMask가 Base Sepolia를 보여주는지 확인!
```

---

## Step 2: 첫 번째 스마트 컨트랙트

### 2.1 가장 간단한 컨트랙트

```
You: 뭔가 하는 가장 간단한 스마트 컨트랙트 만들어줘

Claude: 숫자를 저장하고 가져오는 "Storage" 컨트랙트예요:
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleStorage {
    // 상태 변수 - 블록체인에 영원히 저장!
    uint256 public storedNumber;

    // 숫자를 저장하는 함수
    function store(uint256 _number) public {
        storedNumber = _number;
    }

    // 가져오는 함수 (view = 호출에 가스 안 듦)
    function retrieve() public view returns (uint256) {
        return storedNumber;
    }
}
```

```
You: 그 코드의 모든 줄을 설명해줘

Claude: 분석해볼게요:

        // SPDX-License-Identifier: MIT
        → 법적인 것, 다른 사람들이 당신 코드를 어떻게 사용할 수 있는지 설명

        pragma solidity ^0.8.20;
        → 어떤 Solidity 버전을 사용할지 ("Node v18 사용"처럼)

        contract SimpleStorage {
        → 컨트랙트 시작 (다른 언어의 "class"처럼)

        uint256 public storedNumber;
        → uint256 = 양수 정수 (0부터 매우 큰 수)
        → public = 누구나 읽을 수 있음
        → 블록체인에 영원히 살아있음

        function store(uint256 _number) public {
        → function = 컨트랙트가 할 수 있는 것
        → _number = 입력 파라미터
        → public = 누구나 호출 가능

        storedNumber = _number;
        → 입력을 블록체인에 저장 (가스 듦!)

        function retrieve() public view returns (uint256) {
        → view = 읽기만, 쓰기 안 함 (호출 무료!)
        → returns (uint256) = 돌려주는 타입
```

### 2.2 컴파일 및 배포

```
You: 이걸 Base Sepolia에 어떻게 배포해?

Claude: 단계별로 배포합시다:

        컴파일:
        1. Remix에 코드 있는지 확인
        2. "Solidity Compiler" 클릭 (왼쪽 탭, 'S' 모양)
        3. "Compile SimpleStorage.sol" 클릭
        4. 초록 체크마크 = 성공!

        배포:
        1. "Deploy & Run" 탭 클릭 (ETH 심볼 모양)
        2. Environment: "Injected Provider - MetaMask"
        3. MetaMask가 Base Sepolia를 보여주는지 확인!
        4. "Deploy" 클릭
        5. MetaMask 팝업 → Confirm

        배포 후:
        - "Deployed Contracts" 아래에 컨트랙트가 보임
        - 주소 복사 - 이제 컨트랙트가 사는 곳!
```

### 2.3 컨트랙트와 상호작용

```
You: 배포된 컨트랙트를 어떻게 사용해?

Claude: Remix에서, "Deployed Contracts" 아래:

        읽기 (무료):
        1. "storedNumber" 또는 "retrieve" 클릭
        2. 현재 값 확인 (0으로 시작)

        쓰기 (가스 듦):
        1. "store" 옆에 숫자 입력
        2. "store" 클릭
        3. MetaMask에서 확인
        4. 트랜잭션 대기
        5. "retrieve" 클릭 - 당신의 숫자 확인!

        무슨 일이 일어났냐면:
        - 블록체인에 데이터를 썼어요
        - 그 숫자가 이제 영원히 존재해요
        - 누구나 볼 수 있어요
        - 오직 store 함수만 바꿀 수 있어요
```

---

## Step 3: 팁 항아리 컨트랙트 만들기

유용한 걸 만들어봅시다 - 사람들이 ETH를 보낼 수 있는 팁 항아리!

### 3.1 컨트랙트 설계

```
You: 팁 항아리를 만들고 싶어:
     - 누구나 팁으로 ETH를 보낼 수 있음
     - 나(소유자)만 출금 가능
     - 총 팁과 누가 팁을 줬는지 추적
     - 팁이 발생하면 이벤트 발생

Claude: 모든 기능이 있는 TipJar 컨트랙트를 만들게요.

        사용할 핵심 개념:
        - payable: ETH를 받을 수 있음
        - msg.sender: 함수를 호출한 사람
        - msg.value: 얼마나 ETH를 보냈는지
        - mapping: 딕셔너리처럼
        - events: 추적용 로그
        - require: 실패하면 되돌리는 체크
```

### 3.2 TipJar 컨트랙트

```
You: 모든 것을 설명하는 주석과 함께 TipJar 컨트랙트 작성해줘

Claude: 완전한 TipJar입니다:
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract TipJar {
    // ============ 상태 변수 ============

    // 이 컨트랙트를 배포한 사람 (출금 가능)
    address public owner;

    // 지금까지 받은 모든 팁의 누적 합계
    uint256 public totalTips;

    // 각 주소가 얼마나 팁을 줬는지 추적
    // mapping은 이런 것: { address => amount }
    mapping(address => uint256) public tipsFrom;

    // ============ 이벤트 ============
    // 이벤트는 로그 - 무슨 일이 일어났는지 추적하는 저렴한 방법

    event TipReceived(
        address indexed from,    // 누가 팁을 줬는지 (indexed = 검색 가능)
        uint256 amount,          // 얼마나
        uint256 newTotal         // 누적 합계
    );

    event Withdrawn(
        address indexed to,      // 누가 출금했는지
        uint256 amount           // 얼마나
    );

    // ============ 생성자 ============
    // 컨트랙트가 배포될 때 한 번만 실행

    constructor() {
        // 배포자를 소유자로 설정
        owner = msg.sender;
    }

    // ============ 함수 ============

    // 팁 받기
    // "payable" = ETH를 받을 수 있음
    function tip() public payable {
        // 실제로 뭔가를 보냈는지 체크
        require(msg.value > 0, "Tip must be greater than 0");

        // 합계 업데이트
        totalTips += msg.value;
        tipsFrom[msg.sender] += msg.value;

        // 이벤트 발생 (프론트엔드가 리슨할 수 있음)
        emit TipReceived(msg.sender, msg.value, totalTips);
    }

    // 직접 ETH 받기 허용 (누군가 ETH만 보낼 때)
    receive() external payable {
        tip();  // 직접 전송을 팁으로 처리
    }

    // 소유자가 모든 팁 출금
    function withdraw() public {
        // 소유자만 출금 가능
        require(msg.sender == owner, "Only owner can withdraw");

        // 현재 잔액 가져오기
        uint256 balance = address(this).balance;
        require(balance > 0, "Nothing to withdraw");

        // 소유자에게 ETH 보내기
        // call 사용이 ETH 보내는 권장 방법
        (bool success, ) = owner.call{value: balance}("");
        require(success, "Withdrawal failed");

        // 이벤트 발생
        emit Withdrawn(owner, balance);
    }

    // 컨트랙트 잔액 확인
    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }

    // 특정 주소가 얼마나 팁을 줬는지 확인
    function getTipsFrom(address _tipper) public view returns (uint256) {
        return tipsFrom[_tipper];
    }
}
```

### 3.3 코드 이해하기

```
You: 일부를 이해 못하겠어. 설명해줘:
     1. msg.sender와 msg.value가 뭐야?
     2. mapping이 뭐야?
     3. require가 뭐야?
     4. 왜 (bool success, )에 쉼표가 있어?

Claude: 좋은 질문들!

        1. msg.sender와 msg.value:
           - msg는 트랜잭션 정보가 있는 전역 객체
           - msg.sender = 이 함수를 호출한 주소
           - msg.value = 얼마나 ETH를 보냈는지 (wei 단위)

        2. mapping:
           - 딕셔너리나 해시맵 같은 것
           - mapping(address => uint256)의 의미:
             "주소가 주어지면, 숫자를 반환"
           - tipsFrom[0x123...] = 5000000000000000
           - 사용자별 잔액 추적에 완벽

        3. require:
           - "아니면, 모든 것 중단" 같은 것
           - require(조건, "에러 메시지")
           - 조건이 false면, 트랜잭션이 되돌아감
           - 모든 변경이 취소되고, 가스 환불

        4. (bool success, ):
           - call()이 두 가지를 반환: (bool, bytes)
           - bool만 필요함
           - 쉼표가 두 번째 반환값을 무시
           - JavaScript의 구조 분해와 비슷:
             const [success, ] = [true, "data"]
```

### 3.4 배포 및 테스트

```
You: 이거 배포하고 테스트하는 거 안내해줘

Claude: 전체 플로우를 테스트해봅시다:

        배포:
        1. TipJar 코드를 Remix에 복사
        2. 컴파일 (compile 버튼 클릭)
        3. Base Sepolia에 배포
        4. 컨트랙트 주소 저장!

        테스트 - 팁:
        1. "VALUE" 필드에 입력: 0.001
        2. "Ether" 선택 (Wei 아님)
        3. "tip" 클릭
        4. MetaMask에서 확인
        5. "totalTips" 확인 - 1000000000000000 (wei) 표시

        테스트 - 잔액 확인:
        1. "getBalance" 클릭
        2. 팁 금액이 wei로 표시되어야 함

        테스트 - 출금:
        1. "withdraw" 클릭
        2. MetaMask에서 확인
        3. 지갑 확인 - ETH가 돌아왔어야 함!

        테스트 - 보안:
        1. MetaMask에서 다른 계정으로 전환
        2. 출금 시도
        3. "Only owner can withdraw"로 실패해야 함
```

---

## Step 4: 컨트랙트용 프론트엔드 만들기

### 4.1 배포된 컨트랙트에 연결

```
You: 이제 TipJar용 멋진 웹사이트를 만들고 싶어.
     방금 배포한 컨트랙트와 상호작용하게 해줘.

Claude: TipJar에 연결하는 Next.js 프론트엔드를 만들게요.

        필요한 것:
        1. 컨트랙트 주소 (배포에서)
        2. ABI (컴파일 후 Remix에서)

        ABI는 "사용 설명서" - 프론트엔드에게
        어떤 함수가 있고 어떻게 호출하는지 알려줘요.
```

### 4.2 프론트엔드 생성

```
You: 이런 완전한 TipJar 프론트엔드 만들어줘:
     - 지갑 연결 버튼
     - 총 팁 표시
     - 팁 폼 (금액 선택)
     - 내 총 팁 표시
     - 출금 버튼 (소유자용)
     - 최근 팁 목록 (이벤트에서)

     wagmi 사용하고 Tailwind로 멋지게 만들어줘.

Claude: 완전한 프론트엔드를 만들게요...
        [Next.js 프로젝트 생성]
        [wagmi config 설정]
        [모든 기능이 있는 TipJar 컴포넌트 생성]
        [실시간 업데이트를 위한 이벤트 리스닝 추가]
```

### 4.3 프론트엔드-컨트랙트 연결 이해하기

```
You: 프론트엔드가 컨트랙트와 어떻게 통신하는지 설명해줘

Claude: 연결 방법:

        1. 컨트랙트 주소
           - 당신의 컨트랙트가 블록체인 어디에 있는지
           - URL 같지만 블록체인용

        2. ABI (Application Binary Interface)
           - 함수와 파라미터 목록
           - API 스펙 같은 것
           - 컴파일할 때 생성됨

        3. 프로바이더
           - 블록체인 연결 (MetaMask를 통해)
           - 데이터 읽기

        4. 서명자
           - 당신의 지갑 (MetaMask를 통해)
           - 트랜잭션 서명 및 전송

        읽기 (무료):
        프론트엔드 → 프로바이더 → 블록체인 → 데이터 반환

        쓰기 (가스 듦):
        프론트엔드 → 서명자 → MetaMask 팝업 → 블록체인

        wagmi에서:
        - useReadContract() 읽기용
        - useWriteContract() 쓰기용
        - useWatchContractEvent() 이벤트 리스닝용
```

---

## Step 5: 고급 컨트랙트 기능

### 5.1 TipJar에 NFT 추가

```
You: 팁을 주면 "감사합니다" NFT를 받으면 어때?

Claude: 좋은 아이디어! TipJar에 NFT 민팅을 추가합시다.

        OpenZeppelin의 ERC721 (NFT 표준)을 사용할 거예요.
        OpenZeppelin은 안전한 컨트랙트 코드의 황금 표준이에요.
```

```
You: TipJar에 NFT 리워드 추가해줘:
     - 0.01 ETH 이상 팁 = 감사 NFT 받기
     - 각 NFT는 고유 ID
     - 총 민팅된 NFT 추적

Claude: NFT 민팅하도록 TipJar를 수정할게요...
        [OpenZeppelin imports 추가]
        [NFT 민팅 로직 추가]
        [threshold 체크하도록 tip 함수 업데이트]
```

### 5.2 일시 정지 기능 추가

```
You: 뭔가 잘못되면? 컨트랙트를 일시 정지할 수 있어?

Claude: 네! 이건 "pausable" 패턴이라고 해요.
        프로덕션 컨트랙트에 매우 중요해요.

        컨트랙트에 추가:
        - paused 상태 변수
        - pause() 함수 (소유자만)
        - unpause() 함수 (소유자만)
        - tip() 함수에서 paused 체크

        버그를 발견하거나 공격받을 때 컨트랙트를 멈출 수 있어요.
```

### 5.3 업그레이드 고려사항

```
You: 나중에 기능을 추가하고 싶으면?

Claude: 이건 까다로워요! 스마트 컨트랙트는 기본적으로 불변이에요.

        옵션들:

        1. 받아들이기
           - 새 기능으로 새 컨트랙트 배포
           - 사용자 마이그레이션 (전환해야 함)

        2. 프록시 패턴 (고급)
           - 호출을 전달하는 "프록시" 컨트랙트 배포
           - 뒤의 로직을 업그레이드 가능
           - 더 복잡, 더 많은 것이 잘못될 수 있음

        3. 모듈러 디자인
           - 여러 컨트랙트로 분리
           - 개별 조각 업그레이드

        학습용: 그냥 새 컨트랙트 배포!
        프로덕션용: 프록시 패턴 고려하되, 감사 받기.
```

---

## Step 6: 안전한 스마트 컨트랙트 바이브코딩

### 6.1 안전한 프롬프팅 패턴

```
You: 스마트 컨트랙트 코드를 위해 Claude에게 어떻게 프롬프트해야 해?

Claude: 안전한 스마트 컨트랙트 프롬프팅:

        1. 항상 명시:
           - "Solidity 0.8.20 이상 사용"
           - "표준 기능은 OpenZeppelin 포함"
           - "모든 입력에 require 체크 추가"
           - "모든 상태 변경에 이벤트 발생"
           - "테스트넷용만"

        2. 설명 요청:
           - "각 줄이 뭘 하는지 설명해"
           - "이 코드에서 뭐가 잘못될 수 있어?"
           - "어떤 보안 체크가 빠졌어?"

        3. 테스트 요청:
           - "엣지 케이스 테스트 작성해"
           - "어떤 입력이 이걸 깨뜨릴 수 있어?"
           - "공격자가 이걸 어떻게 exploit할 수 있어?"

        좋은 프롬프트:
        "OpenZeppelin ERC20을 사용해서 간단한 토큰 컨트랙트 만들어줘.
         Solidity 0.8.20 사용해. 소유자만 호출할 수 있는 mint 함수 추가해.
         모든 상태 변경에 이벤트 포함해. 각 보안 결정을 설명해.
         테스트넷 학습용이야."

        나쁜 프롬프트:
        "메인넷에 배포할 수 있는 토큰 만들어"
```

### 6.2 Claude 코드 검토하기

```
You: Claude가 주는 스마트 컨트랙트 코드를 어떻게 검토해?

Claude: 스마트 컨트랙트 코드 리뷰 체크리스트:

        나에게 이 질문들을 해보세요:

        1. "이 컨트랙트가 자금을 잃을 수 있는 모든 방법은?"

        2. "[함수]를 이런 것들로 호출하면 무슨 일이 일어나:
            - 0 값
            - 최대 uint256
            - 자기 주소
            - 0 주소
            - 컨트랙트 주소"

        3. "소유자가 사용자를 rug pull할 수 있어? 어떻게?"

        4. "공격자가 할 수 있는 최악의 일은?"

        5. "재진입 위험이 있어?"

        6. "가스 비용이 바뀌면?"

        솔직하게 취약점을 알려줄게요!
```

### 6.3 확인할 흔한 취약점

```
You: 가장 흔한 스마트 컨트랙트 취약점은?

Claude: 주의할 최상위 취약점:

        1. 재진입 (REENTRANCY)
           - 공격자가 실행 중에 컨트랙트를 다시 호출
           - 예: 외부 컨트랙트를 호출하는 withdraw()
           - 해결: OpenZeppelin의 ReentrancyGuard 사용

        2. 정수 오버플로우/언더플로우
           - 숫자가 순환 (255 + 1 = 0)
           - 해결: Solidity 0.8+ 사용 (내장 체크)

        3. 체크 안 된 외부 호출
           - 외부 호출이 조용히 실패
           - 해결: 항상 반환값 체크

        4. 접근 제어
           - 빠진 또는 잘못된 권한 체크
           - 해결: OpenZeppelin의 Ownable, AccessControl 사용

        5. 프론트러닝
           - 공격자가 당신의 트랜잭션을 보고, 자기 것을 먼저 제출
           - 해결: Commit-reveal 스킴, 프라이빗 mempool

        6. 오라클 조작
           - 잘못된 가격 데이터가 잘못된 계산으로 이어짐
           - 해결: 여러 오라클 소스, 시간 가중 평균

        실제 가치를 다루는 모든 컨트랙트:
        → 전문 감사 받기!
```

---

## Step 7: Foundry로 배포 (전문 도구)

### 7.1 왜 Foundry?

```
You: Remix가 쉬웠어. 왜 Foundry를 배워야 해?

Claude: Foundry의 장점:

        | Remix | Foundry |
        |-------|---------|
        | 브라우저 기반 | 로컬, 버전 관리 |
        | 학습에 좋음 | 프로덕션에 좋음 |
        | 수동 테스트 | 자동화된 테스트 |
        | 단일 파일 | 전체 프로젝트 구조 |
        | 제한된 스크립팅 | 강력한 배포 스크립트 |

        간단한 학습 이상의 것에는 Foundry가 필요해요.
```

### 7.2 Foundry 설정

```
You: Foundry 설치하고 설정하는 거 도와줘

Claude: Foundry를 설치합시다:

        1. 설치 (Mac/Linux):
           curl -L https://foundry.paradigm.xyz | bash
           foundryup

        2. 설치 (Windows):
           WSL (Windows Subsystem for Linux) 사용
           그 다음 같은 명령어 실행

        3. 새 프로젝트 생성:
           forge init my-contracts
           cd my-contracts

        4. 프로젝트 구조:
           my-contracts/
           ├── src/           ← 컨트랙트가 여기에
           ├── script/        ← 배포 스크립트
           ├── test/          ← 테스트
           ├── lib/           ← 의존성
           └── foundry.toml   ← 설정
```

### 7.3 Foundry로 배포

```
You: Foundry를 사용해서 TipJar 배포하는 거 도와줘

Claude: 배포 스크립트를 만들게요:

        1. src/TipJar.sol에 컨트랙트 추가

        2. script/Deploy.s.sol 생성

        3. .env 파일 생성 (절대 커밋하지 마세요!):
           PRIVATE_KEY=당신의_개인_키
           BASE_SEPOLIA_RPC=https://sepolia.base.org

        4. 배포:
           forge script script/Deploy.s.sol \
             --rpc-url base_sepolia \
             --broadcast \
             --verify

        --verify 플래그가 블록 탐색기에서 자동으로 검증해줘요!
```

---

## 🔨 연습: 나만의 컨트랙트 만들기

### 프로젝트 선택

```
You: 내 스마트 컨트랙트 프로젝트를 만들고 싶어.
     학습에 좋은 아이디어가 뭐야?

Claude: 학습을 위한 좋은 프로젝트 아이디어 (난이도 증가):

        초급:
        1. 메시지 보드
           - 사용자가 메시지 게시 가능
           - 메시지가 온체인에 저장됨
           - 간단, 돈 관련 없음

        2. 투표 컨트랙트
           - 제안 생성
           - 사용자가 한 번씩 투표
           - 투표 집계

        중급:
        3. 스테이킹이 있는 토큰
           - 나만의 토큰 생성
           - 사용자가 스테이킹해서 더 얻을 수 있음
           - 시간 잠금 리워드

        4. 허용 목록이 있는 NFT
           - 승인된 주소만 민팅 가능
           - 제한된 공급
           - 공개 메커니즘

        고급 (나중에):
        5. 간단한 DEX
           - 토큰 간 스왑
           - 유동성 풀
           - 가격 계산

        하나 선택하고 같이 만들어봐요!
```

### 만들어보기!

```
You: 메시지 보드 컨트랙트를 만들고 싶어:
     - 누구나 메시지 게시 가능 (가스 듦)
     - 메시지가 영구 저장됨
     - 모든 메시지 읽기 가능
     - 작성자별 메시지 읽기 가능

     이거 만드는 거 안내해줘.

Claude: 단계별로 메시지 보드를 만들어봅시다...
        [컨트랙트 설계]
        [Solidity 코드 작성]
        [각 부분 설명]
        [테스트넷에 배포]
        [프론트엔드 빌드]
```

---

## Step 8: 최종 프로젝트 - NFT 기반 커뮤니티

🎓 **전체 커리큘럼의 최종 프로젝트!**

```
🏆 최종 보스: NFT 기반 커뮤니티

지금까지 클리어한 것:
✅ Part 1-2: 튜토리얼 던전 (Claude Code 기초)
✅ Part 3-4: 프로젝트 아레나 (실제 앱)
✅ Part 5: 고급 성 (hooks, MCP, CI/CD)
✅ Part 6 Ch28-29: Web3 관문 (지갑, frames)

이제 최종 보스와 대결:
🐉 완전한 Web3 멤버십 커뮤니티 빌드!

승리 보상:
🎖️ "AI 네이티브 풀스택 Web3 개발자" 타이틀
```

Part 1부터 Part 6까지 배운 모든 것을 사용해서 **NFT 보유자만 접근할 수 있는 멤버십 커뮤니티 사이트**를 만들어봅시다 - 나만의 미니 Bored Ape Yacht Club처럼!

### 🔄 이 프로젝트에서 사용되는 모든 스킬

```
┌─────────────────────────────────────────────────────────────┐
│           최종 프로젝트: 모든 스킬 결합!                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Part 1-2 (기초):                                           │
│  └── Claude Code 사용, 프로젝트 구조, Git 관리                │
│                                                             │
│  Part 3-4 (프로젝트):                                        │
│  └── Next.js, 배포 (Vercel), 데이터베이스 기초                │
│                                                             │
│  Chapter 28 (Web3 기초):                                     │
│  └── 지갑 연결, 트랜잭션, IPFS, wagmi/viem                   │
│                                                             │
│  Chapter 29 (Frames):                                       │
│  └── 사용자 경험, 온체인 액션, 배포                           │
│                                                             │
│  Chapter 30 (이 챕터):                                       │
│  └── Solidity 컨트랙트, 배포, 프론트엔드 연결                 │
│                                                             │
│  ═══════════════════════════════════════════════════════    │
│  최종 프로젝트 = 모든 것이 결합!                              │
│  └── 컨트랙트 작성 + 프론트엔드 빌드 + 배포 + 토큰 게이팅     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 8.1 프로젝트 개요

```
You: 특정 NFT를 가진 사람만 접근할 수 있는 커뮤니티 사이트를 만들고 싶어.
     - 지갑 연결
     - 사용자가 NFT를 소유하는지 확인
     - NFT가 있으면 비밀 콘텐츠 표시
     - 없으면 NFT 민팅/구매 안내

Claude: 완벽한 졸업 프로젝트! 이건 "토큰 게이팅"이라고 해요.
        많은 실제 Web3 커뮤니티가 이 접근법을 사용해요.

        만들 것:
        1. NFT 컨트랙트 (멤버십 패스)
        2. NFT 민팅 페이지
        3. 게이티드 커뮤니티 페이지
        4. NFT 소유권 검증 로직

        배포하면, 친구들과 공유할 수 있어요!
```

### 8.2 아키텍처 이해하기

```
┌─────────────────────────────────────────────────────────────┐
│                  NFT 기반 커뮤니티 아키텍처                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  [사용자] → 지갑 연결                                         │
│      ↓                                                      │
│  [프론트엔드] → NFT 소유권 확인 요청                           │
│      ↓                                                      │
│  [스마트 컨트랙트] → balanceOf(user) > 0 ?                   │
│      ↓                                                      │
│  ┌─────────────┐       ┌─────────────┐                      │
│  │ NFT 있음 ✅  │       │ NFT 없음 ❌  │                      │
│  │ 비밀 콘텐츠  │       │ 민팅 페이지  │                      │
│  │ 표시        │       │ 표시        │                      │
│  └─────────────┘       └─────────────┘                      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 8.3 Step 1: 멤버십 NFT 컨트랙트

```
You: 커뮤니티 멤버십 NFT 컨트랙트 만들어줘.
     - 이름: "VibeCoder Pass"
     - 심볼: "VIBE"
     - 누구나 민팅 가능 (테스트용)
     - 최대 100개 공급
     - 주소당 1개 민팅

Claude: OpenZeppelin ERC721 기반으로 만들게요...
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract VibeCoderPass is ERC721, Ownable {
    uint256 public constant MAX_SUPPLY = 100;
    uint256 public totalMinted;

    // 이미 민팅한 주소 추적
    mapping(address => bool) public hasMinted;

    // 메타데이터용 Base URI
    string private _baseTokenURI;

    event PassMinted(address indexed to, uint256 tokenId);

    constructor() ERC721("VibeCoder Pass", "VIBE") Ownable(msg.sender) {
        _baseTokenURI = "ipfs://YOUR_METADATA_CID/";
    }

    // 누구나 민팅 가능 (주소당 1개 제한)
    function mint() public {
        require(totalMinted < MAX_SUPPLY, "All passes minted");
        require(!hasMinted[msg.sender], "Already minted");

        uint256 tokenId = totalMinted + 1;
        totalMinted++;
        hasMinted[msg.sender] = true;

        _safeMint(msg.sender, tokenId);
        emit PassMinted(msg.sender, tokenId);
    }

    // 남은 공급량 확인
    function remainingSupply() public view returns (uint256) {
        return MAX_SUPPLY - totalMinted;
    }

    // 주소가 패스를 가지고 있는지 확인
    function hasPass(address user) public view returns (bool) {
        return balanceOf(user) > 0;
    }

    function _baseURI() internal view override returns (string memory) {
        return _baseTokenURI;
    }

    function setBaseURI(string memory newBaseURI) public onlyOwner {
        _baseTokenURI = newBaseURI;
    }
}
```

### 8.4 Step 2: 컨트랙트 배포

```
You: Remix를 사용해서 이 컨트랙트를 Base Sepolia에 배포해줘

Claude: 단계별로 해봅시다:

        1. Remix에서 새 파일 생성: VibeCoderPass.sol
        2. 코드 붙여넣기
        3. OpenZeppelin imports 설정:
           - Remix가 자동으로 처리
        4. 컴파일
        5. 배포 (Injected Provider - MetaMask)
        6. 컨트랙트 주소 저장!

        배포 후:
        - mint() 호출해서 나만의 NFT 민팅
        - hasPass(당신의_주소)로 검증
```

### 8.5 Step 3: NFT 메타데이터 설정

```
You: NFT 이미지와 메타데이터를 어떻게 설정해?

Claude: NFT 메타데이터 구조:

        1. 이미지 만들기 (Figma, Canva, AI 생성)

        2. IPFS에 이미지 업로드

        3. 메타데이터 JSON 생성:
           {
             "name": "VibeCoder Pass #1",
             "description": "VibeCoder 커뮤니티의 공식 멤버십 패스",
             "image": "ipfs://YOUR_IMAGE_CID",
             "attributes": [
               { "trait_type": "Tier", "value": "Founding Member" },
               { "trait_type": "Access", "value": "Full" }
             ]
           }

        4. IPFS에 메타데이터 JSON 업로드

        5. 컨트랙트에서 setBaseURI 호출
```

### 8.6 Step 4: 프론트엔드 - 민팅 페이지

```
You: NFT 민팅 페이지 만들어줘.
     - 지갑 연결 버튼
     - 남은 공급량 표시
     - 민팅 버튼
     - 민팅 성공 시 축하 메시지
     - 이미 민팅했으면 "Already owned" 표시

Claude: MintPage 컴포넌트를 만들게요...
        [wagmi hooks가 있는 Next.js 페이지 생성]
        [컨트랙트 상호작용 추가]
        [모든 시나리오에 대한 UI 상태 추가]
```

### 8.7 Step 5: 프론트엔드 - 게이티드 커뮤니티 페이지

```
You: NFT 보유자만 볼 수 있는 커뮤니티 페이지 만들어줘.
     - NFT 없으면 민팅 페이지로 리다이렉트
     - NFT 있으면 비밀 콘텐츠 표시
     - 비밀 콘텐츠: 멤버 전용 게시물, 링크, 리소스

Claude: GatedCommunity 컴포넌트를 만들게요...

        핵심 로직:
        1. 지갑 연결 확인
        2. hasPass(address) 호출
        3. true → 콘텐츠 표시
        4. false → 민팅 안내 표시
```

```javascript
// 게이트 로직 예시 (의사 코드)
const { address, isConnected } = useAccount();
const { data: hasPass } = useReadContract({
  address: CONTRACT_ADDRESS,
  abi: ABI,
  functionName: 'hasPass',
  args: [address],
});

if (!isConnected) return <ConnectWalletPrompt />;
if (!hasPass) return <MintPrompt />;
return <SecretCommunityContent />;
```

### 8.8 Step 6: 비밀 콘텐츠 아이디어

```
You: 비밀 콘텐츠로 뭘 넣을 수 있어?

Claude: 아이디어들:

        1. 멤버 전용 Discord 초대 링크
        2. 프리미엄 리소스 다운로드
        3. 멤버 전용 포럼/채팅
        4. 미래 에어드롭 자격
        5. 멤버 전용 이벤트 공지
        6. 개발자 Q&A 세션 접근

        테스트용:
        - "🎉 축하합니다! VibeCoder 커뮤니티에 오신 걸 환영합니다!"
        - 멤버 전용 학습 리소스
        - 다음 단계 가이드
```

### 8.9 Step 7: 배포

```
You: 완성된 사이트를 Vercel에 배포해줘

Claude: 배포 단계:

        1. GitHub에 푸시
           git add .
           git commit -m "NFT 기반 커뮤니티 완료"
           git push

        2. Vercel에서 임포트

        3. 환경 변수 설정:
           NEXT_PUBLIC_CONTRACT_ADDRESS=0x...
           NEXT_PUBLIC_CHAIN_ID=84532

        4. 배포 완료!

        이제 URL을 가진 누구나:
        - NFT 민팅 가능
        - 민팅 후 커뮤니티 접근 가능!
```

### 8.10 완료! 🎉

```
You: 완성된 프로젝트를 요약해줘

Claude: 축하합니다! 만든 것:

        📦 스마트 컨트랙트
        - VibeCoder Pass NFT (ERC721)
        - Base Sepolia에 배포됨
        - 최대 100개 공급, 주소당 1개 제한

        🖥️ 프론트엔드
        - 민팅 페이지 (/mint)
        - 게이티드 커뮤니티 (/community)
        - 반응형 디자인

        🔐 토큰 게이팅
        - 실시간 NFT 소유권 검증
        - 비보유자 자동 리다이렉트

        🚀 배포
        - Vercel에 라이브
        - 누구나 접근 가능

        포트폴리오에 이 프로젝트를 추가하세요!
        Farcaster에 공유해서 피드백 받으세요!
```

---

## ❌ 흔한 실수

| 실수 | Claude에게 물어볼 것 |
|------|---------------------|
| 컴파일 에러 | "이 에러가 나: [에러 붙여넣기]. 뭐가 잘못됐어?" |
| 배포 실패 | "배포할 때 트랜잭션이 되돌아갔어. 내 코드: [붙여넣기]" |
| 함수 호출 불가 | "이 함수를 호출할 수 없어, [에러]라고 해. 뭐가 잘못됐어?" |
| 가스가 너무 높음 | "이 함수가 가스가 너무 많이 들어. 어떻게 최적화해?" |
| 컨트랙트 검증 안 됨 | "블록 탐색기에서 컨트랙트 검증하는 거 도와줘" |
| 잘못된 네트워크 | "잘못된 네트워크에 배포했어! Base Sepolia에 다시 배포할 수 있어?" |

---

## 🆘 막힐 때

### 에러 이해하기

```
You: "execution reverted: Only owner can withdraw"가 나와

Claude: 이 에러의 의미:
        - withdraw()를 호출했어
        - 하지만 컨트랙트를 배포한 지갑을 사용하지 않음
        - require() 체크가 실패했어

        해결:
        - MetaMask에서 배포한 계정으로 전환
        - 또는 다른 계정에서 테스트 중 (예상된 동작!)
```

### 트랜잭션 디버깅

```
You: 트랜잭션이 10분째 "pending"으로 멈춰있어

Claude: 흔한 원인들:

        1. 가스 가격이 너무 낮음
           - 취소하고 더 높은 가스로 재전송

        2. Nonce 이슈
           - MetaMask: 설정 → 고급 → 계정 재설정

        3. 네트워크 혼잡
           - 그냥 기다리거나, 트랜잭션 속도 높이기

        4. MetaMask 이슈
           - 지갑 연결 해제하고 다시 연결
```

---

## ✅ 체크리스트

커리큘럼 완료 전에 확인하세요:

- [ ] 스마트 컨트랙트가 무엇인지 이해
- [ ] 컨트랙트 바이브코딩의 보안 위험 알기
- [ ] Remix IDE 설정
- [ ] SimpleStorage 컨트랙트 작성 및 배포
- [ ] 기본 Solidity 이해: 변수, 함수, 수정자
- [ ] TipJar 컨트랙트 빌드 및 배포
- [ ] Remix를 통해 컨트랙트와 상호작용
- [ ] Claude와 함께 컨트랙트용 프론트엔드 빌드!
- [ ] ABI와 프론트엔드가 컨트랙트에 연결하는 방법 이해
- [ ] 흔한 취약점 알기
- [ ] 전문 감사가 필요한 시기 이해
- [ ] (선택) 로컬 개발용 Foundry 설정
- [ ] 나만의 컨트랙트 프로젝트 빌드

---

## 📖 더 배우기

### 공식 문서
- [Solidity Docs](https://docs.soliditylang.org/)
- [Foundry Book](https://book.getfoundry.sh/)
- [Base Documentation](https://docs.base.org/)
- [OpenZeppelin Docs](https://docs.openzeppelin.com/)

### 인터랙티브 학습
- [CryptoZombies](https://cryptozombies.io/) - 무료 Solidity 게임
- [Speedrun Ethereum](https://speedrunethereum.com/) - 실습 챌린지
- [Ethernaut](https://ethernaut.openzeppelin.com/) - 보안 워게임

### 보안 리소스
- [SWC Registry](https://swcregistry.io/) - 스마트 컨트랙트 약점 분류
- [Consensys Best Practices](https://consensys.github.io/smart-contract-best-practices/)
- [Damn Vulnerable DeFi](https://www.damnvulnerabledefi.xyz/) - 보안 챌린지

### 템플릿 & 라이브러리
- [OpenZeppelin Contracts](https://github.com/OpenZeppelin/openzeppelin-contracts)
- [Solmate](https://github.com/transmissions11/solmate) - 가스 최적화 컨트랙트

---

## 🎉 축하합니다!

**바이브코딩 커리큘럼** 전체를 완료하셨습니다!

### 달성한 것

```
┌─────────────────────────────────────────────────────────────┐
│                  30챕터 여정                                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Part 1-2 (Ch.01-11): Claude Code 마스터                    │
│  └── "코딩이 뭐야?" → 효과적인 프롬프팅, Git, 코드 탐색       │
│                                                             │
│  Part 3-4 (Ch.12-20): 실제 프로젝트                          │
│  └── 포트폴리오 사이트, 게임, 챗봇, 풀스택 앱                 │
│                                                             │
│  Part 5 (Ch.21-27): 고급 기능                                │
│  └── Hooks, MCP, Skills, CI/CD, 팀 협업                     │
│                                                             │
│  Part 6 (Ch.28-30): Web3 개발                               │
│  └── Ch.28: 지갑, 트랜잭션 - Web3 소비자                     │
│  └── Ch.29: Farcaster Frames - 소셜 Web3 빌더               │
│  └── Ch.30: 스마트 컨트랙트 - Web3 크리에이터                │
│                                                             │
│  ═══════════════════════════════════════════════════════    │
│                                                             │
│  🎓 최종 프로젝트: NFT 기반 커뮤니티                          │
│  └── 모든 스킬을 하나의 프로젝트로 결합!                      │
│                                                             │
│  당신은 이제 AI 네이티브 풀스택 Web3 개발자입니다!            │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Web3 진화

```
Chapter 28          Chapter 29          Chapter 30
    ↓                   ↓                   ↓
소비자            →   빌더             →   크리에이터
"Web3 앱 사용"       "Web3 위에 빌드"      "Web3 생성"
    │                   │                   │
지갑 연결             Frame 개발          컨트랙트 작성
트랜잭션 이해         기존 컨트랙트 사용    직접 배포
IPFS 기초             소셜 Web3           토큰 게이팅
```

### 바이브코딩 + Web3 마인드셋

```
배운 것:
- 의도 표현 → AI가 코드 작성 → 당신이 다듬기
- 프론트엔드, 백엔드, 그리고 스마트 컨트랙트에 적용됨
- 하지만 스마트 컨트랙트는 특별한 주의 필요

기억하세요:
- 항상 테스트넷 먼저
- 실제 돈은 전문 감사
- 계속 배우세요 - Web3는 빠르게 진화해요!
```

### 다음은?

1. **최종 프로젝트 공유** - Farcaster에 포스트, 피드백 받기!
2. **나만의 아이디어 빌드** - 이제 모든 스킬이 있어요
3. **오픈 소스 기여** - 다른 사람 돕고, 코드에서 배우기
4. **커뮤니티 참여** - Farcaster, Base Discord, 로컬 밋업
5. **업데이트 유지** - AI 도구와 Web3는 빠르게 진화
6. **다른 사람 가르치기** - 초보자를 도우며 지식 굳히기

---

<p align="center">
  <a href="../Chapter29-Farcaster-Frames/README.ko.md">← 이전: Farcaster Frames</a>
</p>

<p align="center">
  <strong>함께 배워주셔서 감사합니다!</strong><br>
  <a href="https://warpcast.com">Farcaster</a>에 여정을 공유하세요! 🎉
</p>

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
