# Chapter 30: Base Smart Contracts

## 💬 Ask Questions
[Discord](https://discord.gg/your-invite-link) - When you're stuck, ask here!

## 🎯 Goals for This Chapter

By the end of this chapter, you will:
- Understand what smart contracts are and how they work
- Write your first Solidity smart contract (with Claude!)
- Deploy a contract to Base Sepolia testnet
- Interact with your deployed contract
- Build a frontend for your contract
- **Complete the Final Project: NFT-Gated Community**
- Learn Web3 smart contract vibecoding best practices

## ⏱️ Estimated Time
**3-4 hours**

## 📋 What You Need
- **Completed Chapters 28-29** - You'll use ALL the skills from both chapters
- MetaMask with Base Sepolia testnet configured (from Ch28)
- Testnet ETH (from faucet)
- Node.js installed

## 🔗 Your Web3 Journey Culmination

This is it - the **final chapter** of the entire curriculum! You've come a long way:

```
┌─────────────────────────────────────────────────────────────┐
│              Skills You're Bringing to This Chapter           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  From Chapter 28 (Web3 Basics):                             │
│  ✅ MetaMask setup and wallet management                    │
│  ✅ Transaction understanding (gas, wei, signing)           │
│  ✅ IPFS for decentralized storage                          │
│  ✅ wagmi/viem for frontend-blockchain connection           │
│  ✅ Building dapps that connect to existing contracts       │
│                                                             │
│  From Chapter 29 (Farcaster Frames):                        │
│  ✅ Building interactive Web3 experiences                   │
│  ✅ Using existing contracts for mints and transactions     │
│  ✅ Understanding how users interact with on-chain actions  │
│  ✅ Deploying Web3 apps to production                       │
│                                                             │
│  What's NEW in Chapter 30:                                  │
│  🆕 Writing your OWN smart contracts in Solidity            │
│  🆕 Deploying contracts you created                         │
│  🆕 Building complete dapps with custom logic               │
│  🆕 Final Project: Full-stack Web3 application              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### The Final Evolution

| Chapter | Role | Key Skill |
|---------|------|-----------|
| 28 | Consumer | Use existing dapps |
| 29 | Builder | Build on existing contracts |
| **30** | **Creator** | **Create your own contracts** |

> 💡 **Big Picture**: In Ch28 you learned to use Web3. In Ch29 you built on top of it. Now in Ch30, you create the foundational layer yourself - smart contracts that others can build on!

---

## ⚠️ Critical Warning: Smart Contract Security

> 🚨 **THIS IS THE MOST IMPORTANT SECTION OF THE ENTIRE WEB3 CURRICULUM**

### Why Smart Contracts Are Different

| Regular Code | Smart Contract Code |
|--------------|---------------------|
| Bug = fix and redeploy | Bug = **PERMANENT** (usually can't fix) |
| Hack = reset passwords | Hack = **FUNDS STOLEN FOREVER** |
| Tested locally | Must work perfectly on **real blockchain** |
| Can iterate quickly | Every deployment costs **real money** |
| Mistakes are recoverable | Mistakes can be **catastrophic** |

### The Vibecoding Dilemma with Smart Contracts

```
You: I've been vibecoding everything with Claude. Can I do the same for smart contracts?

Claude: Here's the honest truth:

        Vibecoding is GREAT for:
        ✅ Learning Solidity syntax
        ✅ Understanding concepts
        ✅ Building on testnet
        ✅ Prototyping ideas
        ✅ Creating basic contracts

        Vibecoding is DANGEROUS for:
        ❌ Production contracts with real money
        ❌ Complex DeFi logic
        ❌ Anything handling significant value

        WHY? Because:
        1. AI can write vulnerable code
        2. You can't fully review what you don't understand
        3. Blockchain makes mistakes permanent
        4. Hackers specifically look for AI-generated contracts
```

### The Safe Approach

```
For Learning (this chapter):
1. Build with Claude on TESTNET only
2. Use small test amounts
3. Focus on understanding

For Production (real money):
1. Hire professional auditors
2. Use battle-tested templates (OpenZeppelin)
3. Start with tiny amounts
4. Get multiple reviews
5. Consider insurance
```

---

## What are Smart Contracts?

### Code That Lives on Blockchain

```
You: Explain smart contracts like I'm 5 years old

Claude: 🎰 Imagine a MAGIC vending machine:

        Regular vending machine:
        - You put in $1
        - You press B4
        - You get chips
        - But the owner could... change prices, steal money, give you air

        Smart Contract "vending machine":
        - You put in $1
        - You press B4
        - You get chips
        - GUARANTEED. Even the creator can't cheat.

        Why "smart"?
        - The rules are written in code
        - Code runs automatically
        - Everyone can see the rules
        - Rules can NEVER be changed (immutable)

        It's like a vending machine made of diamond that runs on math.
        Nobody can break into it. Nobody can change it.
        It just... works. Forever.
```

### The "Robot Employee" Analogy

```
🤖 Think of a smart contract as an employee who:

   ✅ NEVER takes a day off
   ✅ NEVER makes mistakes
   ✅ NEVER plays favorites
   ✅ NEVER asks for a raise
   ✅ Can NEVER be bribed
   ✅ Works 24/7/365

   ❌ But also... can NEVER be corrected if you wrote wrong instructions
   ❌ Will follow bad rules forever if you deployed buggy code
   ❌ Can't "just this once" make an exception

   That's why smart contract development requires extra care!
```

### Key Smart Contract Concepts

| Concept | Explanation | Analogy |
|---------|-------------|---------|
| **Contract** | A program on blockchain | A vending machine |
| **State** | Data stored in contract | Items in the machine |
| **Function** | Actions the contract can do | Buttons on the machine |
| **Event** | Logs for things that happened | Receipt |
| **Gas** | Fee to run code | Electricity for the machine |
| **Immutable** | Can't be changed once deployed | Welded shut |

### Why Solidity?

```
You: Why do we need a special language? Why not JavaScript?

Claude: Solidity is designed for blockchain's unique needs:

        1. DETERMINISTIC
           - Must run exactly the same on every computer
           - JavaScript has too much variation

        2. GAS AWARENESS
           - Every operation costs money
           - Need to optimize for cost

        3. SECURITY FIRST
           - Built-in patterns for safe money handling
           - Explicit about what costs money

        4. STATE MANAGEMENT
           - Data persists on blockchain forever
           - Different from temporary app data

        The good news:
        - Syntax is similar to JavaScript
        - Claude can write it for you
        - You just need to understand WHAT it does
```

---

## Step 1: Development Environment

### 1.1 Choose Your Tools

```
You: What do I need to write smart contracts?

Claude: You have several options:

        Option 1: Remix IDE (Easiest - we'll use this first)
        - Browser-based, nothing to install
        - Good for learning
        - remix.ethereum.org

        Option 2: Foundry (Most popular for pros)
        - Fast, powerful
        - Command-line based
        - We'll try this later

        Option 3: Hardhat (Also popular)
        - JavaScript-based
        - Good ecosystem

        For learning, let's start with Remix!
```

### 1.2 Open Remix IDE

```
You: Walk me through setting up Remix

Claude: Let's get started:

        1. Open https://remix.ethereum.org

        2. You'll see:
           - File explorer (left)
           - Code editor (center)
           - Terminal (bottom)

        3. Create new file:
           - Right-click "contracts" folder
           - "New File"
           - Name it "MyFirstContract.sol"

        4. Connect MetaMask:
           - Click "Deploy" tab (left sidebar)
           - Environment: "Injected Provider - MetaMask"
           - Make sure MetaMask shows Base Sepolia!
```

---

## Step 2: Your First Smart Contract

### 2.1 The Simplest Contract

```
You: Create the simplest possible smart contract that does something

Claude: Here's a "Storage" contract - stores and retrieves a number:
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleStorage {
    // State variable - stored on blockchain forever!
    uint256 public storedNumber;

    // Function to store a number
    function store(uint256 _number) public {
        storedNumber = _number;
    }

    // Function to retrieve (view = doesn't cost gas to call)
    function retrieve() public view returns (uint256) {
        return storedNumber;
    }
}
```

```
You: Explain every line of that code

Claude: Let me break it down:

        // SPDX-License-Identifier: MIT
        → Legal stuff, says how others can use your code

        pragma solidity ^0.8.20;
        → Which Solidity version to use (like "use Node v18")

        contract SimpleStorage {
        → Start of the contract (like "class" in other languages)

        uint256 public storedNumber;
        → uint256 = positive whole number (0 to very big)
        → public = anyone can read it
        → This lives on blockchain FOREVER

        function store(uint256 _number) public {
        → function = something the contract can do
        → _number = input parameter
        → public = anyone can call this

        storedNumber = _number;
        → Save the input to blockchain (costs gas!)

        function retrieve() public view returns (uint256) {
        → view = only reads, doesn't write (FREE to call!)
        → returns (uint256) = what type comes back
```

### 2.2 Compile and Deploy

```
You: How do I deploy this to Base Sepolia?

Claude: Let's deploy step by step:

        COMPILE:
        1. Make sure code is in Remix
        2. Click "Solidity Compiler" (left tab, looks like 'S')
        3. Click "Compile SimpleStorage.sol"
        4. Green checkmark = success!

        DEPLOY:
        1. Click "Deploy & Run" tab (looks like ETH symbol)
        2. Environment: "Injected Provider - MetaMask"
        3. Make sure MetaMask shows Base Sepolia!
        4. Click "Deploy"
        5. MetaMask popup → Confirm

        AFTER DEPLOY:
        - You'll see your contract under "Deployed Contracts"
        - Copy the address - that's where it lives now!
```

### 2.3 Interact with Your Contract

```
You: How do I use my deployed contract?

Claude: In Remix, under "Deployed Contracts":

        READ (free):
        1. Click "storedNumber" or "retrieve"
        2. See the current value (starts at 0)

        WRITE (costs gas):
        1. Enter a number next to "store"
        2. Click "store"
        3. Confirm in MetaMask
        4. Wait for transaction
        5. Click "retrieve" - see your number!

        What just happened:
        - You wrote data to the blockchain
        - That number now exists FOREVER
        - Anyone can see it
        - Only the store function can change it
```

---

## Step 3: Build a Tip Jar Contract

Let's build something useful - a tip jar where people can send you ETH!

### 3.1 Design the Contract

```
You: I want to build a tip jar where:
     - Anyone can send ETH as a tip
     - Only I (the owner) can withdraw
     - Track total tips and who tipped
     - Emit events when tips happen

Claude: I'll create a TipJar contract with all those features.

        Key concepts we'll use:
        - payable: can receive ETH
        - msg.sender: who called the function
        - msg.value: how much ETH they sent
        - mapping: like a dictionary
        - events: logs for tracking
        - require: checks that revert if failed
```

### 3.2 The TipJar Contract

```
You: Write the TipJar contract with comments explaining everything

Claude: Here's the complete TipJar:
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract TipJar {
    // ============ STATE VARIABLES ============

    // Who deployed this contract (can withdraw)
    address public owner;

    // Running total of all tips ever received
    uint256 public totalTips;

    // Track how much each address has tipped
    // mapping is like: { address => amount }
    mapping(address => uint256) public tipsFrom;

    // ============ EVENTS ============
    // Events are logs - cheap way to track what happened

    event TipReceived(
        address indexed from,    // who tipped (indexed = searchable)
        uint256 amount,          // how much
        uint256 newTotal         // running total
    );

    event Withdrawn(
        address indexed to,      // who withdrew
        uint256 amount           // how much
    );

    // ============ CONSTRUCTOR ============
    // Runs ONCE when contract is deployed

    constructor() {
        // Set the deployer as owner
        owner = msg.sender;
    }

    // ============ FUNCTIONS ============

    // Receive a tip
    // "payable" = can receive ETH
    function tip() public payable {
        // Check that they actually sent something
        require(msg.value > 0, "Tip must be greater than 0");

        // Update totals
        totalTips += msg.value;
        tipsFrom[msg.sender] += msg.value;

        // Emit event (for frontend to listen)
        emit TipReceived(msg.sender, msg.value, totalTips);
    }

    // Allow receiving ETH directly (when someone just sends ETH)
    receive() external payable {
        tip();  // Treat direct sends as tips
    }

    // Owner withdraws all tips
    function withdraw() public {
        // Only owner can withdraw
        require(msg.sender == owner, "Only owner can withdraw");

        // Get current balance
        uint256 balance = address(this).balance;
        require(balance > 0, "Nothing to withdraw");

        // Send ETH to owner
        // Using call is the recommended way to send ETH
        (bool success, ) = owner.call{value: balance}("");
        require(success, "Withdrawal failed");

        // Emit event
        emit Withdrawn(owner, balance);
    }

    // Check contract balance
    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }

    // Check how much a specific address has tipped
    function getTipsFrom(address _tipper) public view returns (uint256) {
        return tipsFrom[_tipper];
    }
}
```

### 3.3 Understanding the Code

```
You: I don't understand some parts. Explain:
     1. What's msg.sender and msg.value?
     2. What's a mapping?
     3. What's require?
     4. Why (bool success, ) with the comma?

Claude: Great questions!

        1. msg.sender and msg.value:
           - msg is a global object with transaction info
           - msg.sender = address that called this function
           - msg.value = how much ETH they sent (in wei)

        2. mapping:
           - Like a dictionary or hash map
           - mapping(address => uint256) means:
             "given an address, return a number"
           - tipsFrom[0x123...] = 5000000000000000
           - Perfect for tracking balances per user

        3. require:
           - Like an "if not, stop everything"
           - require(condition, "error message")
           - If condition is false, transaction REVERTS
           - All changes are undone, gas is refunded

        4. (bool success, ):
           - call() returns two things: (bool, bytes)
           - We only care about the bool
           - The comma ignores the second return value
           - Like destructuring in JavaScript:
             const [success, ] = [true, "data"]
```

### 3.4 Deploy and Test

```
You: Deploy this and walk me through testing it

Claude: Let's test the complete flow:

        DEPLOY:
        1. Copy the TipJar code to Remix
        2. Compile it (click compile button)
        3. Deploy to Base Sepolia
        4. Save the contract address!

        TEST - TIP:
        1. In "VALUE" field, enter: 0.001
        2. Select "Ether" (not Wei)
        3. Click "tip"
        4. Confirm in MetaMask
        5. Check "totalTips" - should show 1000000000000000 (wei)

        TEST - CHECK BALANCE:
        1. Click "getBalance"
        2. Should show your tip amount in wei

        TEST - WITHDRAW:
        1. Click "withdraw"
        2. Confirm in MetaMask
        3. Check your wallet - ETH returned!

        TEST - SECURITY:
        1. Switch to different account in MetaMask
        2. Try to withdraw
        3. Should FAIL with "Only owner can withdraw"
```

---

## Step 4: Build a Frontend for Your Contract

### 4.1 Connect to Your Deployed Contract

```
You: Now I want to build a nice website for my TipJar.
     Let me interact with the contract I just deployed.

Claude: I'll create a Next.js frontend that connects to your TipJar.

        You'll need:
        1. Contract address (from deployment)
        2. ABI (from Remix after compiling)

        The ABI is the "instruction manual" - it tells the frontend
        what functions exist and how to call them.
```

### 4.2 Create the Frontend

```
You: Create a complete TipJar frontend with:
     - Connect wallet button
     - Show total tips
     - Tip form (choose amount)
     - Show my total tips
     - Withdraw button (only for owner)
     - Recent tips list (from events)

     Use wagmi and make it look nice with Tailwind.

Claude: I'll create the complete frontend...
        [Creates Next.js project]
        [Sets up wagmi config]
        [Creates TipJar component with all features]
        [Adds event listening for real-time updates]
```

### 4.3 Understanding the Frontend-Contract Connection

```
You: Explain how the frontend talks to the contract

Claude: Here's the connection:

        1. CONTRACT ADDRESS
           - Where your contract lives on blockchain
           - Like a URL but for blockchain

        2. ABI (Application Binary Interface)
           - List of functions and their parameters
           - Like an API spec
           - Generated when you compile

        3. PROVIDER
           - Connection to blockchain (via MetaMask)
           - Reads data

        4. SIGNER
           - Your wallet (via MetaMask)
           - Signs and sends transactions

        READING (free):
        Frontend → Provider → Blockchain → Returns data

        WRITING (costs gas):
        Frontend → Signer → MetaMask popup → Blockchain

        With wagmi:
        - useReadContract() for reading
        - useWriteContract() for writing
        - useWatchContractEvent() for listening to events
```

---

## Step 5: Advanced Contract Features

### 5.1 Add an NFT to Your TipJar

```
You: What if tippers get a "Thank You" NFT for tipping?

Claude: Great idea! Let's add NFT minting to the TipJar.

        We'll use ERC721 (the NFT standard) from OpenZeppelin.
        OpenZeppelin is the gold standard for secure contract code.
```

```
You: Add NFT rewards to TipJar:
     - Tip >= 0.01 ETH = get a thank you NFT
     - Each NFT has a unique ID
     - Track total NFTs minted

Claude: I'll modify the TipJar to mint NFTs...
        [Adds OpenZeppelin imports]
        [Adds NFT minting logic]
        [Updates tip function to check threshold]
```

### 5.2 Add a Pause Feature

```
You: What if something goes wrong? Can I pause the contract?

Claude: Yes! This is called the "pausable" pattern.
        Very important for production contracts.

        Add to your contract:
        - paused state variable
        - pause() function (owner only)
        - unpause() function (owner only)
        - Check paused in tip() function

        This lets you stop the contract if you find a bug
        or if it's being attacked.
```

### 5.3 Upgrade Considerations

```
You: What if I want to add features later?

Claude: This is tricky! Smart contracts are immutable by default.

        Options:

        1. ACCEPT IT
           - Deploy a new contract with new features
           - Migrate users (they have to switch)

        2. PROXY PATTERN (Advanced)
           - Deploy a "proxy" contract that forwards calls
           - Can upgrade the logic behind it
           - More complex, more can go wrong

        3. MODULAR DESIGN
           - Split into multiple contracts
           - Upgrade individual pieces

        For learning: Just deploy new contracts!
        For production: Consider proxy patterns, but get audited.
```

---

## Step 6: Vibecoding Smart Contracts Safely

### 6.1 The Safe Prompting Pattern

```
You: How should I prompt Claude for smart contract code?

Claude: Safe smart contract prompting:

        1. ALWAYS SPECIFY:
           - "Use Solidity 0.8.20 or higher"
           - "Include OpenZeppelin for standard functionality"
           - "Add require checks for all inputs"
           - "Emit events for all state changes"
           - "For testnet only"

        2. ASK FOR EXPLANATIONS:
           - "Explain what each line does"
           - "What could go wrong with this code?"
           - "What security checks are missing?"

        3. REQUEST TESTS:
           - "Write tests for edge cases"
           - "What inputs could break this?"
           - "How could an attacker exploit this?"

        GOOD PROMPT:
        "Create a simple token contract using OpenZeppelin ERC20.
         Use Solidity 0.8.20. Add a mint function that only the owner
         can call. Include events for all state changes. Explain each
         security decision. This is for testnet learning only."

        BAD PROMPT:
        "Make a token I can deploy to mainnet"
```

### 6.2 Review Claude's Code

```
You: How do I review smart contract code Claude gives me?

Claude: Smart contract code review checklist:

        ASK ME THESE QUESTIONS:

        1. "What are all the ways this contract can lose funds?"

        2. "What happens if someone calls [function] with:
            - Zero value
            - Max uint256
            - Their own address
            - Zero address
            - Contract address"

        3. "Can the owner rug pull users? How?"

        4. "What's the worst thing an attacker could do?"

        5. "Are there any reentrancy risks?"

        6. "What if gas costs change?"

        I'll honestly tell you the vulnerabilities!
```

### 6.3 Common Vulnerabilities to Check

```
You: What are the most common smart contract vulnerabilities?

Claude: Top vulnerabilities to watch for:

        1. REENTRANCY
           - Attacker calls back into your contract during execution
           - Example: withdraw() that calls external contract
           - Fix: Use ReentrancyGuard from OpenZeppelin

        2. INTEGER OVERFLOW/UNDERFLOW
           - Numbers wrap around (255 + 1 = 0)
           - Fix: Use Solidity 0.8+ (built-in checks)

        3. UNCHECKED EXTERNAL CALLS
           - External call fails silently
           - Fix: Always check return values

        4. ACCESS CONTROL
           - Missing or wrong permission checks
           - Fix: Use Ownable, AccessControl from OpenZeppelin

        5. FRONT-RUNNING
           - Attacker sees your transaction, submits their own first
           - Fix: Commit-reveal schemes, private mempools

        6. ORACLE MANIPULATION
           - Bad price data leads to wrong calculations
           - Fix: Multiple oracle sources, time-weighted averages

        For ANY contract handling real value:
        → Get a professional audit!
```

---

## Step 7: Deploy with Foundry (Professional Tool)

### 7.1 Why Foundry?

```
You: Remix was easy. Why should I learn Foundry?

Claude: Foundry advantages:

        | Remix | Foundry |
        |-------|---------|
        | Browser-based | Local, version controlled |
        | Good for learning | Good for production |
        | Manual testing | Automated testing |
        | Single files | Full project structure |
        | Limited scripting | Powerful deployment scripts |

        For anything beyond simple learning, you'll want Foundry.
```

### 7.2 Set Up Foundry

```
You: Help me install and set up Foundry

Claude: Let's install Foundry:

        1. Install (Mac/Linux):
           curl -L https://foundry.paradigm.xyz | bash
           foundryup

        2. Install (Windows):
           Use WSL (Windows Subsystem for Linux)
           Then run the same commands

        3. Create new project:
           forge init my-contracts
           cd my-contracts

        4. Project structure:
           my-contracts/
           ├── src/           ← Your contracts go here
           ├── script/        ← Deployment scripts
           ├── test/          ← Tests
           ├── lib/           ← Dependencies
           └── foundry.toml   ← Config
```

### 7.3 Deploy with Foundry

```
You: Help me deploy TipJar using Foundry

Claude: I'll create a deployment script:

        1. Add contract to src/TipJar.sol

        2. Create script/Deploy.s.sol

        3. Create .env file (NEVER commit this!):
           PRIVATE_KEY=your_private_key
           BASE_SEPOLIA_RPC=https://sepolia.base.org

        4. Deploy:
           forge script script/Deploy.s.sol \
             --rpc-url base_sepolia \
             --broadcast \
             --verify

        The --verify flag verifies on block explorer automatically!
```

---

## 🔨 Exercise: Build Your Own Contract

### Choose Your Project

```
You: I want to build my own smart contract project.
     What are some good ideas for learning?

Claude: Great project ideas for learning (increasing difficulty):

        BEGINNER:
        1. Message Board
           - Users can post messages
           - Messages stored on chain
           - Simple, no money involved

        2. Voting Contract
           - Create proposals
           - Users vote once each
           - Count votes

        INTERMEDIATE:
        3. Token with Staking
           - Create your own token
           - Users can stake to earn more
           - Time-locked rewards

        4. NFT with Allowlist
           - Only approved addresses can mint
           - Limited supply
           - Reveal mechanism

        ADVANCED (for later):
        5. Simple DEX
           - Swap between tokens
           - Liquidity pools
           - Price calculation

        Pick one and let's build it together!
```

### Build It!

```
You: I want to build a Message Board contract where:
     - Anyone can post a message (costs gas)
     - Messages are stored permanently
     - Can read all messages
     - Can read messages by author

     Walk me through building this.

Claude: Let's build the Message Board step by step...
        [Designs the contract]
        [Writes the Solidity code]
        [Explains each part]
        [Deploys to testnet]
        [Builds frontend]
```

---

## Step 8: Final Project - NFT-Gated Community

🎓 **This is the FINAL PROJECT of the entire curriculum!**

```
🏆 FINAL BOSS: NFT-Gated Community

You've defeated:
✅ Part 1-2: The Tutorial Dungeon (Claude Code basics)
✅ Part 3-4: The Project Arena (real apps)
✅ Part 5: The Advanced Castle (hooks, MCP, CI/CD)
✅ Part 6 Ch28-29: The Web3 Gates (wallet, frames)

Now face the Final Boss:
🐉 Build a complete Web3 membership community!

Victory reward:
🎖️ "AI-Native Full-Stack Web3 Developer" title
```

Let's use everything you've learned from Part 1 to Part 6 to build a **membership community site that only NFT holders can access** - like your own mini Bored Ape Yacht Club!

### 🔄 All Skills Used in This Project

```
┌─────────────────────────────────────────────────────────────┐
│           Final Project: All Skills Combined!                 │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Part 1-2 (Fundamentals):                                   │
│  └── Claude Code usage, project structure, Git management   │
│                                                             │
│  Part 3-4 (Projects):                                       │
│  └── Next.js, deployment (Vercel), database basics          │
│                                                             │
│  Chapter 28 (Web3 Basics):                                  │
│  └── Wallet connection, transactions, IPFS, wagmi/viem      │
│                                                             │
│  Chapter 29 (Frames):                                       │
│  └── User experience, on-chain actions, deployment          │
│                                                             │
│  Chapter 30 (This chapter):                                 │
│  └── Solidity contracts, deployment, frontend connection    │
│                                                             │
│  ═══════════════════════════════════════════════════════    │
│  Final Project = Everything Combined!                       │
│  └── Write contract + Build frontend + Deploy + Token Gating│
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 8.1 Project Overview

```
You: I want to build a community site that only people with a specific NFT can access.
     - Wallet connection
     - Check if user owns NFT
     - If they have NFT, show secret content
     - If not, guide them to mint/buy NFT

Claude: Perfect graduation project! This is called "Token Gating".
        Many real Web3 communities use this approach.

        What we'll build:
        1. NFT contract (membership pass)
        2. NFT minting page
        3. Gated community page
        4. NFT ownership verification logic

        Once deployed, you can share it with your friends!
```

### 8.2 Understanding the Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                  NFT-Gated Community Architecture            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  [User] → Connect Wallet                                    │
│      ↓                                                      │
│  [Frontend] → Request NFT ownership check                   │
│      ↓                                                      │
│  [Smart Contract] → balanceOf(user) > 0 ?                   │
│      ↓                                                      │
│  ┌─────────────┐       ┌─────────────┐                      │
│  │ Has NFT ✅  │       │ No NFT ❌   │                      │
│  │ Show secret │       │ Show mint   │                      │
│  │ content     │       │ page        │                      │
│  └─────────────┘       └─────────────┘                      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 8.3 Step 1: Membership NFT Contract

```
You: Create a community membership NFT contract.
     - Name: "VibeCoder Pass"
     - Symbol: "VIBE"
     - Anyone can mint (for testing)
     - Max 100 supply
     - 1 mint per address

Claude: I'll create this based on OpenZeppelin ERC721...
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract VibeCoderPass is ERC721, Ownable {
    uint256 public constant MAX_SUPPLY = 100;
    uint256 public totalMinted;

    // Track addresses that already minted
    mapping(address => bool) public hasMinted;

    // Base URI for metadata
    string private _baseTokenURI;

    event PassMinted(address indexed to, uint256 tokenId);

    constructor() ERC721("VibeCoder Pass", "VIBE") Ownable(msg.sender) {
        _baseTokenURI = "ipfs://YOUR_METADATA_CID/";
    }

    // Anyone can mint (1 per address limit)
    function mint() public {
        require(totalMinted < MAX_SUPPLY, "All passes minted");
        require(!hasMinted[msg.sender], "Already minted");

        uint256 tokenId = totalMinted + 1;
        totalMinted++;
        hasMinted[msg.sender] = true;

        _safeMint(msg.sender, tokenId);
        emit PassMinted(msg.sender, tokenId);
    }

    // Check remaining supply
    function remainingSupply() public view returns (uint256) {
        return MAX_SUPPLY - totalMinted;
    }

    // Check if an address has a pass
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

### 8.4 Step 2: Deploy the Contract

```
You: Deploy this contract to Base Sepolia using Remix

Claude: Let's do it step by step:

        1. Create new file in Remix: VibeCoderPass.sol
        2. Paste the code
        3. OpenZeppelin imports setup:
           - Remix handles this automatically
        4. Compile
        5. Deploy (Injected Provider - MetaMask)
        6. Save the contract address!

        After deployment:
        - Call mint() to mint your own NFT
        - Verify with hasPass(your_address)
```

### 8.5 Step 3: NFT Metadata Setup

```
You: How do I set up NFT images and metadata?

Claude: NFT metadata structure:

        1. Create your image (Figma, Canva, AI generation)

        2. Upload image to IPFS

        3. Create metadata JSON:
           {
             "name": "VibeCoder Pass #1",
             "description": "Official membership pass for VibeCoder community",
             "image": "ipfs://YOUR_IMAGE_CID",
             "attributes": [
               { "trait_type": "Tier", "value": "Founding Member" },
               { "trait_type": "Access", "value": "Full" }
             ]
           }

        4. Upload metadata JSON to IPFS

        5. Call setBaseURI on your contract
```

### 8.6 Step 4: Frontend - Minting Page

```
You: Create an NFT minting page.
     - Connect wallet button
     - Show remaining supply
     - Mint button
     - Celebration message on successful mint
     - Show "Already owned" if already minted

Claude: I'll create the MintPage component...
        [Creates Next.js page with wagmi hooks]
        [Adds contract interaction]
        [Adds UI states for all scenarios]
```

### 8.7 Step 5: Frontend - Gated Community Page

```
You: Create a community page that only NFT holders can view.
     - Redirect to minting page if no NFT
     - Show secret content if they have NFT
     - Secret content: member-only posts, links, resources

Claude: I'll create the GatedCommunity component...

        Core logic:
        1. Check wallet connection
        2. Call hasPass(address)
        3. true → Show content
        4. false → Show mint prompt
```

```javascript
// Gate logic example (pseudo code)
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

### 8.8 Step 6: Secret Content Ideas

```
You: What can I put as secret content?

Claude: Ideas:

        1. Member-only Discord invite link
        2. Premium resource downloads
        3. Member-only forum/chat
        4. Future airdrop eligibility
        5. Member-only event announcements
        6. Developer Q&A session access

        For testing:
        - "🎉 Congratulations! Welcome to the VibeCoder community!"
        - Member-only learning resources
        - Next steps guide
```

### 8.9 Step 7: Deploy

```
You: Deploy the finished site to Vercel

Claude: Deployment steps:

        1. Push to GitHub
           git add .
           git commit -m "NFT-gated community complete"
           git push

        2. Import in Vercel

        3. Set environment variables:
           NEXT_PUBLIC_CONTRACT_ADDRESS=0x...
           NEXT_PUBLIC_CHAIN_ID=84532

        4. Deployment complete!

        Now anyone with the URL can:
        - Mint the NFT
        - Access the community after minting!
```

### 8.10 Complete! 🎉

```
You: Summarize the completed project

Claude: Congratulations! What you've built:

        📦 Smart Contract
        - VibeCoder Pass NFT (ERC721)
        - Deployed to Base Sepolia
        - 100 max supply, 1 per address limit

        🖥️ Frontend
        - Minting page (/mint)
        - Gated community (/community)
        - Responsive design

        🔐 Token Gating
        - Real-time NFT ownership verification
        - Auto-redirect for non-holders

        🚀 Deployment
        - Live on Vercel
        - Accessible to anyone

        Add this project to your portfolio!
        Share on Farcaster to get feedback!
```

---

## ❌ Common Mistakes

| Mistake | What to Ask Claude |
|---------|-------------------|
| Compilation error | "I'm getting this error: [paste error]. What's wrong?" |
| Deployment fails | "Transaction reverted when deploying. Here's my code: [paste]" |
| Can't call function | "I can't call this function, it says [error]. What's wrong?" |
| Gas too high | "This function costs too much gas. How do I optimize it?" |
| Contract not verified | "Help me verify my contract on block explorer" |
| Wrong network | "I deployed to wrong network! Can I redeploy to Base Sepolia?" |

---

## 🆘 When You're Stuck

### Understanding Errors

```
You: I'm getting "execution reverted: Only owner can withdraw"

Claude: This error means:
        - You called withdraw()
        - But you're not using the wallet that deployed the contract
        - The require() check failed

        Fix:
        - Switch MetaMask to the account that deployed
        - Or you're testing from a different account (expected behavior!)
```

### Debugging Transactions

```
You: My transaction is stuck as "pending" for 10 minutes

Claude: Common causes:

        1. Gas price too low
           - Cancel and resend with higher gas

        2. Nonce issue
           - MetaMask: Settings → Advanced → Reset Account

        3. Network congestion
           - Just wait, or speed up transaction

        4. MetaMask issue
           - Disconnect and reconnect wallet
```

---

## ✅ Checklist

Before completing the curriculum, confirm:

- [ ] Understand what smart contracts are
- [ ] Know the security risks of vibecoding contracts
- [ ] Set up Remix IDE
- [ ] Wrote and deployed SimpleStorage contract
- [ ] Understand basic Solidity: variables, functions, modifiers
- [ ] Built and deployed TipJar contract
- [ ] Interacted with contract via Remix
- [ ] Built frontend for your contract (with Claude!)
- [ ] Understand ABI and how frontend connects to contracts
- [ ] Know about common vulnerabilities
- [ ] Understand when to get professional audits
- [ ] (Optional) Set up Foundry for local development
- [ ] Built your own contract project

---

## 📖 Learn More

### Official Documentation
- [Solidity Docs](https://docs.soliditylang.org/)
- [Foundry Book](https://book.getfoundry.sh/)
- [Base Documentation](https://docs.base.org/)
- [OpenZeppelin Docs](https://docs.openzeppelin.com/)

### Interactive Learning
- [CryptoZombies](https://cryptozombies.io/) - Free Solidity game
- [Speedrun Ethereum](https://speedrunethereum.com/) - Hands-on challenges
- [Ethernaut](https://ethernaut.openzeppelin.com/) - Security wargame

### Security Resources
- [SWC Registry](https://swcregistry.io/) - Smart Contract Weakness Classification
- [Consensys Best Practices](https://consensys.github.io/smart-contract-best-practices/)
- [Damn Vulnerable DeFi](https://www.damnvulnerabledefi.xyz/) - Security challenges

### Templates & Libraries
- [OpenZeppelin Contracts](https://github.com/OpenZeppelin/openzeppelin-contracts)
- [Solmate](https://github.com/transmissions11/solmate) - Gas-optimized contracts

---

## 🎉 Congratulations!

You've completed the entire **Vibecoding Curriculum**!

### What You've Achieved

```
┌─────────────────────────────────────────────────────────────┐
│                  YOUR 30-CHAPTER JOURNEY                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Part 1-2 (Ch.01-11): Claude Code Master                     │
│  └── "What is coding?" → Effective prompting, Git, code nav │
│                                                             │
│  Part 3-4 (Ch.12-20): Real Projects                          │
│  └── Portfolio sites, games, chatbots, full-stack apps      │
│                                                             │
│  Part 5 (Ch.21-27): Advanced Features                        │
│  └── Hooks, MCP, Skills, CI/CD, team collaboration          │
│                                                             │
│  Part 6 (Ch.28-30): Web3 Development                         │
│  └── Ch.28: Wallet, transactions - Web3 Consumer            │
│  └── Ch.29: Farcaster Frames - Social Web3 Builder          │
│  └── Ch.30: Smart Contracts - Web3 Creator                  │
│                                                             │
│  ═══════════════════════════════════════════════════════    │
│                                                             │
│  🎓 Final Project: NFT-Gated Community                      │
│  └── All skills combined into one project!                    │
│                                                             │
│  You are now an AI-Native Full-Stack Web3 Developer!        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Your Web3 Evolution

```
Chapter 28          Chapter 29          Chapter 30
    ↓                   ↓                   ↓
CONSUMER         →   BUILDER          →   CREATOR
"Use Web3 apps"      "Build on Web3"      "Create Web3"
    │                   │                   │
Wallet connect        Frame development    Write contracts
Understand tx         Use existing contracts Deploy yourself
IPFS basics           Social Web3          Token Gating
```

### The Vibecoding + Web3 Mindset

```
What you learned:
- Express intent → AI writes code → You refine
- Works for frontend, backend, AND smart contracts
- But smart contracts need extra caution

Remember:
- Testnet first, always
- Professional audits for real money
- Keep learning - Web3 evolves fast!
```

### What's Next?

1. **Share Your Final Project** - Post on Farcaster, get feedback!
2. **Build Your Own Ideas** - You have all the skills now
3. **Contribute to Open Source** - Help others, learn from code
4. **Join Communities** - Farcaster, Base Discord, local meetups
5. **Stay Updated** - AI tools and Web3 evolve rapidly
6. **Teach Others** - Solidify your knowledge by helping beginners

---

<p align="center">
  <a href="../Chapter29-Farcaster-Frames/README.md">← Previous: Farcaster Frames</a>
</p>

<p align="center">
  <strong>Thank you for learning with us!</strong><br>
  Share your journey on <a href="https://warpcast.com">Farcaster</a>! 🎉
</p>

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
