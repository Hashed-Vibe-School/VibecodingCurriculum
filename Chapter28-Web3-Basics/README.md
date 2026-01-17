# Chapter 28: Web3 Basics

## 💬 Ask Questions
[Discord](https://discord.gg/your-invite-link) - When you're stuck, ask here!

## 🎯 Goals for This Chapter

By the end of this chapter, you will:
- Understand what Web3 and blockchain mean
- Set up a crypto wallet (MetaMask)
- Connect a wallet to a web app (with Claude's help!)
- Make your first on-chain transaction on testnet
- Learn Web3-specific vibecoding tips and pitfalls

## ⏱️ Estimated Time
**2-3 hours**

## 📋 What You Need
- Completed through Chapter 20 (Full-Stack Apps)
- Chrome/Brave browser
- No real cryptocurrency needed (we use testnet)

## 🗺️ Web3 Learning Journey (Part 6)

```
┌─────────────────────────────────────────────────────────────┐
│                    Web3 Learning Path                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Chapter 28 (YOU ARE HERE) - "Web3 Consumer"                │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ 🎯 Goal: Understand Web3 from USER perspective       │   │
│  │ • Set up wallet, understand transactions             │   │
│  │ • Connect to dapps, claim tokens/NFTs               │   │
│  │ • Build a simple dapp that connects wallets          │   │
│  │                                                      │   │
│  │ 📚 You'll learn: wallet, gas, testnet, IPFS         │   │
│  └─────────────────────────────────────────────────────┘   │
│                        ↓                                    │
│  Chapter 29 - "Social Web3 Builder"                         │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ 🎯 Goal: Build interactive Web3 social experiences   │   │
│  │ • Uses wallet connection from Ch28                   │   │
│  │ • Build Farcaster Frames (polls, quizzes, mints)    │   │
│  │ • Add on-chain actions to social posts              │   │
│  │                                                      │   │
│  │ 📚 You'll learn: Frames API, social miniapps        │   │
│  └─────────────────────────────────────────────────────┘   │
│                        ↓                                    │
│  Chapter 30 - "Smart Contract Creator"                      │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ 🎯 Goal: Create your OWN smart contracts             │   │
│  │ • Write Solidity code (with Claude!)                │   │
│  │ • Deploy contracts to Base                          │   │
│  │ • Final Project: NFT-Gated Community                │   │
│  │                                                      │   │
│  │ 📚 You'll learn: Solidity, deployment, security     │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  🎓 After Part 6: Full-stack Web3 Developer!                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Skill Progression

| Chapter | Role | What You Build | Key Skills |
|---------|------|----------------|------------|
| **28** | Consumer | Wallet app, Claim site | wallet, tx, IPFS |
| **29** | Builder | Interactive Frames | social Web3, Frames API |
| **30** | Creator | Smart contracts, NFT community | Solidity, deployment |

> 💡 **Learning Flow**: The wallet connection and transaction skills you learn in Ch28 will be used continuously in Ch29 and Ch30. Each chapter builds upon the knowledge from the previous one.

---

## ⚠️ Web3 Vibecoding: Critical Warnings

Before we start, there are **important differences** between regular vibecoding and Web3 vibecoding:

### Why Web3 Requires Extra Caution

| Regular App | Web3 App |
|-------------|----------|
| Bug = fix and redeploy | Bug in smart contract = **permanent** |
| User loses data = restore from backup | User loses crypto = **gone forever** |
| Security breach = reset passwords | Security breach = **funds drained** |
| Test with fake data | Test with **real testnet** first |

> 🚨 **Critical Rule**: NEVER deploy Claude-generated smart contracts with real money until thoroughly audited by professionals.

### Web3 Vibecoding Golden Rules

1. **Always use testnet first** - Never test with real crypto
2. **Double-check addresses** - One wrong character = lost funds
3. **Understand before signing** - Ask Claude to explain what you're approving
4. **Never share private keys** - Not even with Claude in the chat
5. **Small amounts first** - Even on mainnet, test with tiny amounts

```
You: What are the biggest security mistakes in Web3 development?

Claude: Here are the critical ones to avoid:
        1. Exposing private keys in code/git
        2. Not validating user input in smart contracts
        3. Using untested libraries
        4. Skipping testnet entirely
        5. Approving unlimited token spending
```

---

## What is Web3?

### The Evolution of the Web

Think of it like this - the internet grew up:

```
Web1 (1990s): READ-ONLY 📰
└── Like a library - you can only read
└── Static websites, you consume content
└── Example: News sites, Wikipedia
└── "Look but don't touch!"

Web2 (2000s-now): READ-WRITE 📱
└── Like a shopping mall - you can buy and sell
└── Social media, you create content
└── BUT the mall owns everything (your posts, followers, data)
└── Example: Facebook, YouTube, Twitter
└── "Play in our sandbox, but we make the rules"

Web3 (emerging): READ-WRITE-OWN 🔑
└── Like owning your own house - it's actually YOURS
└── Blockchain-based, you own your data and assets
└── No landlord who can evict you
└── Example: DeFi, NFTs, DAOs
└── "Your keys, your coins, your rules"
```

> 💡 **Real Talk**: Ever had Instagram delete your post? Or Spotify remove your playlist? In Web3, that can't happen. It's like the difference between renting an apartment vs owning your home.

### Key Concepts

| Term | Simple Explanation | Real-World Analogy |
|------|-------------------|-------------------|
| **Blockchain** | Shared database no one controls | Google Doc everyone can see, but no one can delete history |
| **Wallet** | Your identity + bank account | Passport + bank account in one app |
| **Transaction** | Any action on blockchain | Making an entry in the shared Google Doc |
| **Gas** | Fee to use blockchain | Postage stamp - pay to send your entry |
| **Testnet** | Practice blockchain (free!) | Monopoly money - looks real but worthless |
| **Mainnet** | Real blockchain (real money) | Your actual bank account - be careful! |
| **Smart Contract** | Code that runs on blockchain | Vending machine nobody can hack |
| **ABI** | Contract's instruction manual | "How to use" guide for the vending machine |

> 💡 **Beginner Tip**: Imagine blockchain as a public notebook where:
> - Everyone can read it
> - Anyone can write in it (if they pay the "ink fee")
> - Once written, it can NEVER be erased
> - No single person owns the notebook

### The "Coffee Shop" Analogy for Everything

```
Let's understand Web3 by building a mental model with a coffee shop:

☕ WALLET = Your coffee shop membership app
   - Has your ID (public address)
   - Has your secret PIN (private key)
   - Shows your reward points (token balance)

💰 TRANSACTION = Ordering coffee
   - You tap the app (sign with wallet)
   - Pay for your order (gas fee)
   - Get a receipt (transaction hash)
   - Receipt is saved forever (on blockchain)

📜 SMART CONTRACT = The coffee machine
   - Same input always = same output
   - Nobody can cheat it (even the shop owner)
   - Rules are visible to everyone
   - "Insert $5 → Get coffee" ALWAYS works

🎨 NFT = Your limited edition coffee mug
   - Only you own THIS specific mug
   - Numbered (Token ID #42)
   - Can prove you own it
   - Can sell or trade it
```

### Why Base?

We'll use **Base** blockchain in this curriculum:

| Feature | Base | Why It Matters |
|---------|------|----------------|
| **Built by** | Coinbase | Trusted, well-funded |
| **Type** | Layer 2 (L2) | Faster, cheaper than Ethereum mainnet |
| **Speed** | ~2 second blocks | Quick feedback when developing |
| **Cost** | Very low fees | Won't drain your testnet ETH |
| **Security** | Secured by Ethereum | Inherits Ethereum's security |
| **Ecosystem** | Growing rapidly | Good for finding examples/help |

---

## Step 1: Set Up MetaMask Wallet

### 1.1 Install MetaMask

```
You: Help me install MetaMask browser extension. Guide me step by step.

Claude: I'll walk you through installing MetaMask...
        1. Go to metamask.io
        2. Click Download
        3. Select your browser (Chrome recommended)
        4. Add the extension
        5. Click "Create a new wallet"
        6. Create a strong password
        7. IMPORTANT: Write down your Secret Recovery Phrase on paper
```

### 1.2 Understand Your Wallet

```
You: Explain MetaMask to me like I'm 5 years old

Claude: Your MetaMask wallet is like a magical keychain:
        - The "address" (0x...) is like your mailbox number - anyone can send you stuff
        - The "private key" is like your house key - NEVER share it
        - The "recovery phrase" is like a spare key hidden under a rock - keep it super safe
        - "Networks" are like different post offices - some are for practice (testnet)
```

### 1.3 Security Essentials

> ⚠️ **Critical Security - Read This Carefully**

**Your Secret Recovery Phrase:**
- Is the ONLY way to recover your wallet
- Should be written on **paper** (not digital)
- Should be stored in a **safe place**
- Should **NEVER** be shared with anyone
- Not even "support" asking for it (they're scammers)

```
You: What's a Secret Recovery Phrase and why is it so important?

Claude: Your Secret Recovery Phrase (also called seed phrase) is 12-24 words like:
        "apple banana cat dog elephant fish..."

        Anyone with these words can:
        - Access ALL your funds
        - Send everything to their own wallet
        - And you can NEVER get it back

        There's no "forgot password" in crypto. Lose the phrase = lose everything.
```

**Common Scams to Avoid:**

| Scam Type | How It Works | How to Avoid |
|-----------|--------------|--------------|
| Fake support | "We need your seed phrase to help" | Real support NEVER asks for seed |
| Phishing sites | metamask.com (fake) vs metamask.io (real) | Always check URL |
| Malicious tokens | Airdrop tokens that drain wallet when you try to sell | Never interact with unknown tokens |
| Discord DMs | "You won! Click this link" | Disable DMs from servers |

### 1.4 Add Base Sepolia Testnet

```
You: Help me add Base Sepolia testnet to MetaMask

Claude: I'll help you configure MetaMask for Base Sepolia.

        Method 1 (Automatic):
        Go to chainlist.org, search "Base Sepolia", click "Add to MetaMask"

        Method 2 (Manual):
        Settings → Networks → Add Network:
        - Network Name: Base Sepolia
        - RPC URL: https://sepolia.base.org
        - Chain ID: 84532
        - Currency Symbol: ETH
        - Block Explorer: https://sepolia.basescan.org
```

### 1.5 Verify Your Setup

```
You: How do I know MetaMask is set up correctly?

Claude: Let's verify:
        1. Click the MetaMask extension
        2. At the top, you should see "Base Sepolia" (not "Ethereum Mainnet")
        3. Your address starts with 0x and has 42 characters
        4. Balance shows 0 ETH (we'll get test ETH next)
```

---

## Step 2: Get Testnet ETH

### 2.1 What are Faucets?

```
You: What's a faucet in Web3?

Claude: A faucet is a website that gives you FREE test cryptocurrency.
        It's called a "faucet" because it drips small amounts of test tokens.

        Why it exists:
        - Developers need test ETH to build apps
        - Test ETH has no real value
        - It's how everyone practices before using real money
```

### 2.2 Get Test ETH

```
You: Help me get testnet ETH for Base Sepolia

Claude: Here are the faucets you can try:

        1. Alchemy Faucet (https://www.alchemy.com/faucets/base-sepolia)
           - Requires Alchemy account (free)
           - Gives 0.1 ETH per day

        2. Coinbase Faucet (https://portal.cdp.coinbase.com/products/faucet)
           - Requires Coinbase developer account
           - Good amount of test ETH

        3. QuickNode Faucet (https://faucet.quicknode.com/base/sepolia)
           - May require social verification

        Let me know which one you're trying and I'll help you through it.
```

> 💡 **Tip**: Faucets often have requirements:
> - Social media verification
> - Waiting period between requests
> - Small amount of ETH on mainnet
> If one doesn't work, try another!

### 2.3 Check Your Balance

```
You: How do I check if I received the testnet ETH?

Claude: Check in MetaMask:
        1. Open MetaMask
        2. Make sure "Base Sepolia" is selected at top
        3. Your balance should show something like "0.1 ETH"

        Or check on block explorer:
        1. Go to sepolia.basescan.org
        2. Paste your wallet address
        3. See your balance and transaction history
```

---

## Step 3: Build a Wallet Connect App

Let's build a simple web app that connects to MetaMask - **using vibecoding**!

### 3.1 Understanding the Project

```
You: Explain what we're building and why

Claude: We're building a "dapp" (decentralized app) that:
        1. Shows a "Connect Wallet" button
        2. When clicked, opens MetaMask
        3. After connecting, shows your address
        4. Shows your ETH balance
        5. Lets you send ETH to another address

        This is the foundation of ALL Web3 apps!
```

### 3.2 Create the Project

```
You: Create a Next.js project with Web3 wallet connection.
     Use wagmi and viem (they're the modern standard).
     Set it up for Base Sepolia testnet.

Claude: I'll create the project with best practices...
        [Creates project structure]
        [Installs wagmi, viem, @tanstack/react-query]
        [Sets up providers and config]
        [Creates WalletButton component]
```

> 💡 **Vibecoding Tip**: You don't need to know wagmi, viem, or any Web3 library syntax. Just describe what you want, and Claude will write the code!

### 3.3 Web3 Library Choices

```
You: Why wagmi and viem instead of ethers.js or web3.js?

Claude: Good question! Here's the landscape:

        | Library | Pros | Cons |
        |---------|------|------|
        | wagmi + viem | Modern, React hooks, TypeScript | Newer, less legacy docs |
        | ethers.js v6 | Popular, good docs | Heavier, not React-native |
        | web3.js | OG library, lots of examples | Older patterns, larger bundle |

        For React apps in 2024+, wagmi + viem is the recommended choice.
        But the concepts transfer to any library!
```

### 3.4 Add Wallet Connection

```
You: Add a button that connects to MetaMask when clicked,
     shows the connected address, and has a disconnect button

Claude: I'll create a WalletConnect component...
        [Writes the component using wagmi hooks]
```

### 3.5 Understanding the Code

> 💡 **Vibecoding + Understanding**: Even when Claude writes the code, ask it to explain! You should understand WHAT is happening, even if you don't memorize the syntax.

```
You: Explain what the code you just wrote does.
     Especially useAccount, useConnect, and useDisconnect.

Claude: Let me break it down:

        useAccount():
        - Like asking "who's logged in?"
        - Returns { address, isConnected }
        - address is the user's wallet (0x...)
        - isConnected is true/false

        useConnect():
        - Triggers the "login" flow
        - Opens MetaMask popup
        - Waits for user approval

        useDisconnect():
        - Logs out the user
        - Clears the connection

        The flow:
        1. User clicks "Connect"
        2. MetaMask popup opens
        3. User approves
        4. useAccount now has their address
        5. UI updates to show address
```

### 3.6 Run and Test

```
You: Run the app and help me test the wallet connection

Claude: Starting the development server...
        npm run dev

        Now:
        1. Open http://localhost:3000
        2. Click "Connect Wallet"
        3. MetaMask popup should appear
        4. Click "Connect" in MetaMask
        5. Your address should show on the page!
```

---

## Step 4: Read Blockchain Data

### 4.1 Display ETH Balance

```
You: Add a component that shows the connected wallet's ETH balance.
     Make it update automatically when the balance changes.

Claude: I'll use the useBalance hook from wagmi...
        [Writes balance display component]

        This will:
        - Show balance in ETH (not wei)
        - Auto-update every 5 seconds
        - Show loading state while fetching
```

### 4.2 Understand Wei vs ETH

```
You: What's "wei" and why do I keep seeing it in Web3 code?

Claude: ETH has 18 decimal places, which is too many for computers to handle precisely.

        So internally, everything is stored in "wei" (the smallest unit):
        - 1 ETH = 1,000,000,000,000,000,000 wei (18 zeros)
        - Like how $1 = 100 cents, but with WAY more decimals

        Common conversions:
        - 1 ETH = 10^18 wei
        - 1 gwei = 10^9 wei (used for gas prices)
        - 0.001 ETH = 10^15 wei

        Libraries handle this:
        - parseEther("1.5") → converts "1.5 ETH" to wei
        - formatEther(wei) → converts wei back to readable ETH
```

### 4.3 Display Current Block

```
You: Also show the current block number.
     Explain what a block number means.

Claude: Adding block number display...

        A block number is like a page number in blockchain's history book:
        - Every ~2 seconds on Base, a new "page" (block) is added
        - Each block contains transactions from that moment
        - Block 1000 happened before block 2000
        - Used for: tracking when things happened, coordinating actions
```

### 4.4 Read Contract Data (Preview)

```
You: Can I read data from any smart contract?

Claude: Yes! You need:
        1. Contract address (where it lives)
        2. ABI (what functions it has)

        Example - reading an ERC20 token balance:
        [Shows example with useReadContract hook]

        We'll do more of this in Chapter 30!
```

---

## Step 5: Send a Transaction

### 5.1 Understanding Transactions

```
You: Explain what happens when I send ETH on the blockchain

Claude: Here's the full journey:

        1. YOU: Click "Send"
        2. YOUR APP: Creates transaction data
        3. METAMASK: Shows you what you're approving
        4. YOU: Click "Confirm" (or reject)
        5. METAMASK: Signs with your private key
        6. NETWORK: Broadcasts to blockchain nodes
        7. MINERS/VALIDATORS: Include in next block
        8. BLOCKCHAIN: Transaction is now permanent!

        Important concepts:
        - Gas: Fee paid to validators (varies with network congestion)
        - Nonce: Transaction counter (prevents replay attacks)
        - Hash: Unique ID for your transaction (proof it happened)
```

### 5.2 Add a Send Form

```
You: Add a form where I can enter an address and amount to send ETH.
     Include validation to make sure the address is valid.
     Show the gas estimate before sending.

Claude: I'll create a send form with validation...
        [Creates SendETH component]

        This includes:
        - Address validation (must be valid Ethereum address)
        - Amount validation (can't send more than balance)
        - Gas estimation (shows approximate cost)
        - Transaction status (pending, success, failed)
```

### 5.3 Test the Transaction

```
You: Help me send 0.001 ETH to test the form

Claude: Let's do a test transaction:

        1. For "To Address", you can use:
           - Another wallet you own, OR
           - A "burn" address like 0x000...dead (just for testing)

        2. Enter 0.001 in the amount field

        3. Click "Send" - MetaMask will popup

        4. Check the details in MetaMask:
           - Sending to: [correct address?]
           - Amount: 0.001 ETH
           - Gas fee: ~$0.01 or less

        5. Click "Confirm"

        6. Wait for confirmation (usually ~2 seconds on Base)

        7. You'll get a transaction hash - save it!
```

### 5.4 View Transaction on Explorer

```
You: How do I see my transaction details?

Claude: Go to the block explorer:
        1. Open https://sepolia.basescan.org
        2. Paste your transaction hash (0x...)
        3. You'll see:
           - Status: Success/Failed
           - Block: Which block included it
           - From/To: Addresses involved
           - Value: How much was sent
           - Gas: How much you paid

        Or just click the hash in your app - I can make it a link!
```

---

## Step 6: Vibecoding Web3 Best Practices

### 6.1 Prompting Tips for Web3

```
You: How should I prompt Claude for Web3 code?

Claude: Web3 prompting tips:

        BE SPECIFIC about:
        1. Which chain (Base, Ethereum, Polygon?)
        2. Testnet or mainnet
        3. Which libraries (wagmi, ethers, web3.js?)
        4. React or vanilla JS

        ALWAYS mention:
        - "Use TypeScript"
        - "Handle errors properly"
        - "Show loading states"
        - "Make it work on testnet first"

        Good prompt:
        "Create a component that reads the balance of an ERC20 token
         on Base Sepolia using wagmi. Show loading and error states.
         Use TypeScript and handle the case where wallet isn't connected."

        Bad prompt:
        "Show token balance"
```

### 6.2 Testing Strategy

```
You: What's the right way to test Web3 code?

Claude: Web3 Testing Pyramid:

        Level 1: Local Testing
        - Use hardhat/foundry local node
        - Instant, free, can time-travel

        Level 2: Testnet Testing (Base Sepolia)
        - Real network behavior
        - Free test tokens
        - Good for integration testing

        Level 3: Mainnet Testing
        - ONLY after thorough testnet testing
        - Start with tiny amounts
        - Have recovery plan ready

        For vibecoding:
        "Help me set up local testing with Hardhat so I can test
         without using testnet ETH every time"
```

### 6.3 Common Pitfalls

```
You: What mistakes do beginners make in Web3 development?

Claude: Top Web3 Beginner Mistakes:

        1. WRONG NETWORK
           - Sending to mainnet when you meant testnet
           - Fix: Always check network in MetaMask before confirming

        2. WRONG ADDRESS FORMAT
           - Using wrong chain address (ETH address on Bitcoin)
           - Fix: Always use checksummed addresses

        3. NOT HANDLING REJECTIONS
           - User cancels MetaMask = unhandled error
           - Fix: Always catch user rejection

        4. TRUSTING AI-GENERATED CONTRACT CODE
           - Claude can write Solidity, but security bugs = lost funds
           - Fix: ALWAYS get professional audit for anything with real money

        5. STORING PRIVATE KEYS IN CODE
           - .env file committed to git = hacked wallet
           - Fix: Use hardware wallet, never hardcode keys

        6. NOT UNDERSTANDING GAS
           - Transaction stuck because gas too low
           - Fix: Learn about gas price vs gas limit
```

---

## Step 7: Build a Token/NFT Claim Site

Let's build something exciting - a website where users can fill out a form and receive a **token or NFT** directly to their wallet!

### 7.1 Project Overview

```
You: I want to build a site where:
     1. User connects wallet
     2. User fills out a form (name, email, uploads an image)
     3. User clicks "Claim"
     4. They receive either a token OR an NFT (made from their image)

     Can we build this on Base Sepolia?

Claude: Absolutely! This is called a "claim" or "airdrop" flow.

        For this to work, we need:
        1. Frontend: Form + wallet connection
        2. Backend: API to trigger minting
        3. Smart Contract: Pre-deployed contract that can mint
        4. Storage: IPFS for the NFT image

        Let's build it step by step!
```

### 7.2 Understanding Tokens vs NFTs

```
You: Explain the difference between tokens and NFTs for someone new

Claude: Think of it like this:

        TOKEN (ERC20):
        - Like coins or points
        - Every token is identical (1 TOKEN = 1 TOKEN)
        - Divisible (you can have 0.5 tokens)
        - Examples: USDC, DEGEN, Points in games

        NFT (ERC721):
        - Like trading cards or art
        - Every NFT is unique (NFT #1 ≠ NFT #2)
        - Not divisible (can't have half an NFT)
        - Examples: Art, PFPs, Tickets, Certificates

        For our project:
        - Token claim = give user 100 REWARD tokens
        - NFT claim = mint unique NFT from their uploaded image
```

### 7.3 Option A: Build a Token Claim Site

```
You: Let's start with a simple token claim.
     Create a site where:
     1. User connects wallet
     2. User enters their name and email
     3. User clicks "Claim 100 REWARD tokens"
     4. Tokens are sent to their wallet

Claude: I'll set up this project.

        We'll use a pre-deployed ERC20 contract on Base Sepolia that
        allows minting. Here's the plan:

        1. Create the form UI
        2. Connect to the token contract
        3. Call the mint function when user claims
        4. Show success with transaction link
```

#### Setting Up the Token Contract

```
You: How do we get a token contract that can mint?

Claude: For learning purposes, we have options:

        Option 1: Use an existing test token
        - Many test tokens allow free minting
        - Good for learning, no setup needed

        Option 2: Deploy your own (Chapter 30 preview)
        - Create a simple ERC20 with a mint function
        - You control everything

        For now, let's use the Zora testnet's practice token,
        or I can help you deploy a simple one using Remix IDE.
```

#### Building the Claim Form

```
You: Create the token claim form with:
     - Name field
     - Email field (we'll just store locally for now)
     - Claim button
     - Show pending/success states

Claude: I'll create the ClaimToken component...
        [Creates form with validation]
        [Adds wallet connection check]
        [Adds contract interaction]
        [Shows transaction status]
```

### 7.4 Option B: Build an NFT Mint Site (with User's Image!)

This is more exciting - users upload an image, and it becomes their unique NFT!

```
You: Now let's do the NFT version.
     User uploads an image, and we mint it as an NFT to their wallet.
     Walk me through how this works.

Claude: Great! Here's the NFT minting flow:

        1. User uploads image
        2. Image → IPFS (decentralized storage)
        3. Metadata (name, description, image URL) → IPFS
        4. Mint NFT with metadata URL → Blockchain
        5. User now owns the NFT!

        We need:
        - IPFS upload (we'll use NFT.Storage or Pinata)
        - NFT contract that allows minting
        - Frontend form
```

#### Understanding IPFS

```
You: What is IPFS and why do we need it for NFTs?

Claude: IPFS = InterPlanetary File System

        The Problem:
        - Blockchain is expensive (storing 1MB = lots of $$$)
        - Images are big (100KB to several MB)
        - If you store on normal server, server dies = image gone

        The Solution (IPFS):
        - Decentralized file storage
        - Files get a unique "hash" as their address
        - Anyone can host the file
        - As long as ONE person has it, it exists

        NFT on blockchain stores: "ipfs://Qm123abc..."
        IPFS has the actual image at that address!
```

#### Setting Up IPFS Upload

```
You: Set up IPFS upload for our NFT images.
     Use a free service that's easy to set up.

Claude: I recommend NFT.Storage (free, run by Protocol Labs):

        1. Go to nft.storage
        2. Create free account
        3. Get API key
        4. Add to .env.local

        I'll create an upload function that:
        - Takes the user's image file
        - Uploads to IPFS via NFT.Storage
        - Returns the IPFS URL (ipfs://...)
```

#### Building the NFT Mint Form

```
You: Create the NFT mint form with:
     - Image upload with preview
     - Name field for the NFT
     - Description field
     - Mint button
     - Show the minted NFT at the end

Claude: I'll create the MintNFT component...
        [Creates image upload with drag-and-drop]
        [Adds image preview]
        [Adds name/description fields]
        [Handles IPFS upload]
        [Calls NFT contract mint]
        [Shows minted NFT with OpenSea link]
```

### 7.5 Complete NFT Mint Flow

Here's the full user experience:

```
┌─────────────────────────────────────────────────────────────┐
│                     NFT Mint Flow                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Step 1: Connect Wallet                                     │
│  [Connect Wallet Button]                                    │
│                                                             │
│  Step 2: Upload Your Image                                  │
│  ┌─────────────────────┐                                    │
│  │  Drag & drop image  │  ← User's image becomes the NFT   │
│  │  or click to upload │                                    │
│  └─────────────────────┘                                    │
│                                                             │
│  Step 3: Add Details                                        │
│  Name: [My Awesome NFT_______]                              │
│  Description: [My first NFT!_]                              │
│                                                             │
│  Step 4: Mint!                                              │
│  [🎨 Mint NFT] ← Triggers IPFS upload + blockchain mint     │
│                                                             │
│  Step 5: Success!                                           │
│  ┌─────────────────────┐                                    │
│  │   [Your NFT Image]  │                                    │
│  │   My Awesome NFT    │                                    │
│  │   Token ID: #42     │                                    │
│  │   [View on OpenSea] │                                    │
│  └─────────────────────┘                                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 7.6 Important Considerations

```
You: What should I know before deploying this to real users?

Claude: Critical points for production:

        1. GAS COSTS
           - Minting costs gas
           - Option A: Users pay (they need ETH)
           - Option B: You pay (gasless/sponsored mints)

        2. SPAM PREVENTION
           - Anyone can mint = someone will abuse it
           - Add rate limiting, captcha, or allowlist

        3. IMAGE SIZE LIMITS
           - Large images = slow upload
           - Add max file size (e.g., 5MB)

        4. CONTRACT OWNERSHIP
           - Who owns the contract?
           - Can pause if something goes wrong?

        5. IPFS PINNING
           - Free tier has limits
           - For production, use paid pinning or Arweave

        For testnet learning, don't worry about these yet!
```

### 7.7 Testing Your Mint Site

```
You: Help me test the complete mint flow

Claude: Let's test step by step:

        1. Start the app: npm run dev

        2. Open http://localhost:3000

        3. Connect wallet (Base Sepolia!)

        4. Upload a test image (use any photo)

        5. Fill in name: "My First NFT"

        6. Fill in description: "Minted with vibecoding!"

        7. Click "Mint NFT"

        8. Watch the progress:
           - "Uploading to IPFS..." (few seconds)
           - "Minting NFT..." (MetaMask popup)
           - "Confirm in MetaMask"
           - "Waiting for confirmation..."
           - "Success! NFT #X minted!"

        9. Click the OpenSea testnet link to see it!
```

---

## 🔨 Exercise: Complete Dapp

Build a complete dapp by asking Claude to add these features:

```
You: Help me build a complete dapp with:
     1. Wallet connect/disconnect button with connection status
     2. Display connected address (shortened format)
     3. Show ETH balance that updates every 10 seconds
     4. Send ETH form with:
        - Address validation
        - Amount validation (can't exceed balance)
        - Gas estimate
        - Transaction status tracking
     5. Transaction history (last 5 transactions from this session)
     6. Error handling for all edge cases
     7. Loading states for all async operations

     Use wagmi, viem, and TypeScript. Target Base Sepolia.

Claude: Let's build this step by step...
        [Creates complete dapp with all features]
```

> 💡 **Vibecoding Approach**: Start with the prompt above, then refine. "The connect button is too small." "Add a copy address button." "Make the transaction history show timestamps."

---

## ❌ Common Mistakes

| Mistake | What to Ask Claude |
|---------|-------------------|
| "MetaMask not detected" | "MetaMask isn't connecting, help me debug. Here's my error: [error]" |
| Transaction stuck | "My transaction is pending for 5 minutes, what's wrong?" |
| Wrong network | "I'm on mainnet but should be on testnet. Help me add network switching." |
| "Insufficient funds" | "I'm getting insufficient funds error but I have 0.1 ETH. What's wrong?" |
| Gas estimation failed | "Transaction gas estimation failed. Error: [error]. What does this mean?" |
| Address invalid | "My address validation says invalid but it looks correct. Help debug." |

---

## 🆘 When You're Stuck

### Debugging with Claude

```
You: [Copy paste the FULL error message]
     This error appeared when I tried to connect the wallet.
     I'm using wagmi v2 on Base Sepolia.

Claude: This error means... Here's how to fix it...
```

> 💡 **Debugging Tip**: Always include:
> - Full error message
> - What you were trying to do
> - Which libraries/versions
> - Which network

### Understanding Concepts

```
You: I keep hearing about "signing" vs "sending" transactions. What's the difference?

Claude: Great question!

        SIGNING (free):
        - Uses your private key to prove it's you
        - Doesn't cost gas
        - Used for: login, proving ownership, off-chain actions
        - Example: Sign in with Ethereum

        SENDING (costs gas):
        - Actually changes blockchain state
        - Costs gas (real fee)
        - Used for: transfers, contract interactions
        - Example: Send ETH, mint NFT

        Both require MetaMask confirmation, but only sending costs money!
```

---

## ✅ Checklist

Before moving on, confirm:

- [ ] MetaMask installed and secured
- [ ] Understand seed phrase importance (and stored it safely)
- [ ] Base Sepolia testnet configured
- [ ] Got testnet ETH from faucet
- [ ] Built wallet connect app (with Claude!)
- [ ] Can connect and disconnect wallet
- [ ] Can see ETH balance
- [ ] Successfully sent a test transaction
- [ ] Viewed transaction on block explorer
- [ ] Built token or NFT claim site (with Claude!)
- [ ] Understand IPFS and why NFTs use it
- [ ] Successfully minted an NFT with your own image
- [ ] Understand the Web3 vibecoding warnings
- [ ] Know the difference between testnet and mainnet
- [ ] Understand basic concepts (wallet, transaction, gas, wei, token, NFT)

---

## 📖 Learn More

### Official Documentation
- [MetaMask Docs](https://docs.metamask.io/)
- [Base Documentation](https://docs.base.org/)
- [wagmi Documentation](https://wagmi.sh/)
- [viem Documentation](https://viem.sh/)

### Security Resources
- [Smart Contract Security Best Practices](https://consensys.github.io/smart-contract-best-practices/)
- [Common Web3 Scams](https://www.youtube.com/results?search_query=web3+scam+awareness)

### Videos
- [What is Web3?](https://www.youtube.com/results?search_query=what+is+web3+explained)
- [MetaMask Tutorial](https://www.youtube.com/results?search_query=metamask+tutorial+beginners)
- [wagmi Tutorial](https://www.youtube.com/results?search_query=wagmi+tutorial)

### Tools
- [Base Sepolia Faucet](https://www.alchemy.com/faucets/base-sepolia)
- [Base Sepolia Block Explorer](https://sepolia.basescan.org)
- [Chainlist](https://chainlist.org/) - Easy network adding

---

## What's Next?

In **Chapter 29**, you'll build **Farcaster Frames** - interactive social media posts that can do Web3 actions!

### Skills You'll Carry Forward

Everything you learned here is **foundational** for the next chapters:

| What You Learned | How It's Used in Ch29 | How It's Used in Ch30 |
|------------------|----------------------|----------------------|
| **Wallet connection** | Frame users connect wallets | Frontend connects to your contracts |
| **Sending transactions** | Frames can trigger mints/tips | Call your contract functions |
| **Reading blockchain** | Check if user owns NFT | Read contract state |
| **IPFS basics** | Store Frame images | Store NFT metadata |
| **wagmi/viem** | Same libraries used | Same libraries used |

### What's Different in Chapter 29

```
Chapter 28: Build standalone dapps
           ↓
Chapter 29: Build dapps INSIDE social media posts!
           - No app store needed
           - Viral distribution through Farcaster
           - Users interact without leaving their feed
```

> 💡 **Preview**: In Chapter 29, you'll create an interactive poll Frame, a quiz game, and even a Frame that mints NFTs - all embedded in a social media post!

---

<p align="center">
  <a href="../Chapter27-Team-Collaboration/README.md">← Previous: Team Collaboration</a> |
  <a href="../Chapter29-Farcaster-Frames/README.md">Next: Farcaster Frames →</a>
</p>

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
