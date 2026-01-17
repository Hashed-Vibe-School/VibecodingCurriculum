# Chapter 29: Farcaster Frames

## 💬 Ask Questions
[Discord](https://discord.gg/your-invite-link) - When you're stuck, ask here!

## 🎯 Goals for This Chapter

By the end of this chapter, you will:
- Understand what Farcaster and Frames are
- Understand how Frames work technically (meta tags, API routes, state management)
- Build an interactive Frame from scratch (with Claude!)
- Deploy your Frame and share it on Farcaster
- Add on-chain actions to your Frame (tips, mints, transactions)
- Build a multi-step Frame with persistent state
- Complete two hands-on exercises (Poll Frame + NFT Mint Frame)

## ⏱️ Estimated Time
**3-4 hours**

## 📋 What You Need
- **Completed Chapter 28** (Web3 Basics) - wallet setup, transactions, IPFS basics
- A Farcaster account (Warpcast app)
- Vercel account (for deployment)
- Node.js 18+ installed

## 🔗 Building on Chapter 28

In Chapter 28, you learned Web3 fundamentals as a **consumer**. Now you'll use those skills to build something that reaches **thousands of users** through social media!

```
What You Learned in Ch28        →    How It's Used in Ch29
──────────────────────────────────────────────────────────
Wallet connection               →    Frame users sign transactions
Sending transactions            →    Mint NFTs, send tips in Frames
IPFS upload                     →    Store Frame images
wagmi/viem basics               →    Same libraries in Frame backend
```

> 💡 **Key Insight**: Frames are like building a mini-dapp that lives inside a social media post. All the Web3 concepts from Chapter 28 apply here!

---

## What is Farcaster?

### Decentralized Social Media

Imagine if Twitter and your crypto wallet had a baby:

```
Traditional Social (Twitter/X):         Farcaster:
🏢 "Renting an apartment"               🏠 "Owning your house"

└── Company owns your data              └── YOU own your identity
└── Can ban you anytime                 └── Can't be deplatformed
└── Algorithm controls you              └── Choose your own feed
└── Followers stuck on platform         └── Take followers anywhere
└── "Their rules, their property"       └── "Your keys, your content"
```

### The Technical Side (Simplified)

```
┌─────────────────────────────────────────────────────────────┐
│                  Farcaster Architecture                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  HUBS (Decentralized Servers)                               │
│  ┌─────┐  ┌─────┐  ┌─────┐                                 │
│  │Hub 1│──│Hub 2│──│Hub 3│  ← Anyone can run a hub         │
│  └─────┘  └─────┘  └─────┘                                 │
│      │        │        │                                    │
│      └────────┼────────┘                                    │
│               │                                             │
│               ▼                                             │
│  ┌─────────────────────────────┐                           │
│  │    Apps (Clients)           │                           │
│  │  • Warpcast (most popular)  │                           │
│  │  • Supercast                │                           │
│  │  • Farcord                  │                           │
│  └─────────────────────────────┘                           │
│               │                                             │
│               ▼                                             │
│  ┌─────────────────────────────┐                           │
│  │    Your Identity            │                           │
│  │  • FID (Farcaster ID)       │  ← Stored on blockchain   │
│  │  • Recovery address         │                           │
│  │  • Connected wallets        │                           │
│  └─────────────────────────────┘                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Why Developers Love Farcaster

```
🚀 For Vibecoding, Farcaster is PERFECT because:

1. BUILT-IN AUDIENCE
   - 500k+ crypto-native users
   - They WANT to try new things
   - Instant feedback on your creations

2. NO APP STORE NEEDED
   - Share a link → Your app works
   - No Apple/Google approval
   - Deploy in minutes, not weeks

3. COMPOSABLE
   - Your Frame can use any smart contract
   - Other people can build on YOUR Frame
   - LEGO blocks for social apps

4. DEVELOPER FRIENDLY
   - Open protocol, open APIs
   - Rich ecosystem of tools
   - Active developer community
```

### Key Terms

| Term | Explanation | "Explain Like I'm 5" |
|------|-------------|---------------------|
| **Farcaster** | Decentralized social protocol | Twitter that nobody owns |
| **Warpcast** | Most popular app to use Farcaster | Like using Gmail to check email |
| **Cast** | A post on Farcaster | A tweet, but on Farcaster |
| **Frame** | Interactive mini-app in a post | Instagram Story that can DO stuff |
| **FID** | Your Farcaster ID | Your player number (permanent!) |
| **Hub** | Server that stores Farcaster data | A library that keeps copies of all posts |
| **Signer** | Key that lets apps post on your behalf | A stamp that says "this is really me" |

> 💡 **Mind Blown Moment**: A Frame is like putting a whole app INSIDE a social media post. Imagine if an Instagram Story could mint NFTs, run polls, or play games - that's what Frames do!

---

## What are Frames?

### Interactive Posts

A Frame turns a boring link into an **interactive experience**:

```
🥱 Regular Cast:                    🎮 Frame:
┌────────────────────────┐          ┌────────────────────────┐
│ "Check out my project" │          │ ┌────────────────────┐ │
│ [link]                 │    →     │ │  🎯 Web3 Quiz!     │ │
│                        │          │ │  Score: 0/5        │ │
└────────────────────────┘          │ └────────────────────┘ │
                                    │ [A] [B] [C] [D]        │
 "Click to open website"            │ Click → Magic happens! │
 (user has to leave)                └────────────────────────┘
                                     (user stays in feed!)
```

### The "Pop-up Shop" Analogy

```
Think of social media as a shopping mall:

📱 Regular post = Handing out flyers
   - "Visit our website!"
   - People have to leave the mall
   - Most people don't bother

🏪 Frame = A pop-up shop IN the mall
   - People can interact right there
   - No leaving required
   - Instant engagement
   - Can even take payments!
```

### How Frames Work Technically

```
┌─────────────────────────────────────────────────────────────┐
│                    Frame Request/Response Cycle               │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. USER SEES FRAME                                         │
│     Warpcast fetches your URL → reads meta tags → displays  │
│                                                             │
│  2. USER CLICKS BUTTON                                      │
│     ┌──────────────┐                                        │
│     │ [Button 1]   │  ← User clicks                         │
│     └──────────────┘                                        │
│            │                                                │
│            ▼                                                │
│     POST request to your server with:                       │
│     • buttonIndex (which button: 1, 2, 3, or 4)            │
│     • fid (user's Farcaster ID)                            │
│     • inputText (if you have a text input)                 │
│     • castId (which cast this is from)                     │
│                                                             │
│  3. YOUR SERVER RESPONDS                                    │
│     Return new HTML with updated:                           │
│     • fc:frame:image (new image to show)                   │
│     • fc:frame:button:1 (new button labels)                │
│     • fc:frame:post_url (where next click goes)            │
│                                                             │
│  4. FRAME UPDATES                                           │
│     Warpcast shows the new image and buttons                │
│                                                             │
│  This cycle repeats for each interaction!                   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Frame Meta Tags Explained

When Warpcast visits your URL, it looks for special HTML meta tags:

```html
<!-- Required: Tells Warpcast "this is a Frame" -->
<meta property="fc:frame" content="vNext" />

<!-- Required: The image to display (1.91:1 or 1:1 aspect ratio) -->
<meta property="fc:frame:image" content="https://your-site.com/image.png" />

<!-- Optional: Buttons (up to 4) -->
<meta property="fc:frame:button:1" content="Vote Yes" />
<meta property="fc:frame:button:2" content="Vote No" />
<meta property="fc:frame:button:3" content="See Results" />

<!-- Optional: Button actions -->
<meta property="fc:frame:button:1:action" content="post" />      <!-- Default: sends to your server -->
<meta property="fc:frame:button:2:action" content="link" />      <!-- Opens external URL -->
<meta property="fc:frame:button:3:action" content="tx" />        <!-- Triggers blockchain transaction -->
<meta property="fc:frame:button:4:action" content="mint" />      <!-- Mints an NFT -->

<!-- Optional: Where button clicks go -->
<meta property="fc:frame:post_url" content="https://your-site.com/api/frame" />

<!-- Optional: Text input field -->
<meta property="fc:frame:input:text" content="Enter your answer..." />

<!-- Optional: Image aspect ratio -->
<meta property="fc:frame:image:aspect_ratio" content="1.91:1" />
```

> 💡 **Vibecoding Tip**: You don't need to memorize these! Just tell Claude what you want, and it generates the correct meta tags.

### Button Action Types

| Action | What It Does | Example Use Case |
|--------|--------------|------------------|
| `post` | Sends data to your server | Voting, navigation, quiz answers |
| `post_redirect` | Posts then redirects to URL | "Continue on website" |
| `link` | Opens URL in browser | External links, documentation |
| `tx` | Triggers a blockchain transaction | Tipping, buying, swapping |
| `mint` | Special action for minting NFTs | One-click NFT mints |

### What You Can Build

Your imagination is the limit, but here are proven hits:

| Frame Type | Example | Why It Works | Difficulty |
|------------|---------|--------------|------------|
| **Polls** | "Tabs vs Spaces?" | Easy engagement, everyone has opinions | ⭐ Easy |
| **Quizzes** | "How Web3 are you?" | Viral potential + educational | ⭐⭐ Medium |
| **Games** | Rock-paper-scissors | Fun + competitive + shareable | ⭐⭐ Medium |
| **NFT Mints** | One-click collect | Frictionless for users | ⭐⭐ Medium |
| **Tip Jars** | Support creators | Direct crypto payments | ⭐⭐ Medium |
| **Galleries** | Swipe through art | Visual + interactive | ⭐⭐ Medium |
| **Raffles** | "Enter to win!" | Excitement + engagement | ⭐⭐⭐ Hard |
| **Token-gated** | "NFT holders only" | Exclusivity + community | ⭐⭐⭐ Hard |

> 💡 **Pro Tip**: The best Frames are SIMPLE. If you can't explain it in 5 seconds, simplify it.

---

## Step 1: Set Up Farcaster Account

### 1.1 Download Warpcast

```
You: Help me set up a Farcaster account for sharing Frames

Claude: I'll guide you through setting up Warpcast...
```

Claude will help you:
1. Download [Warpcast](https://warpcast.com/) on iOS/Android
2. Create an account (requires a small fee for spam prevention)
3. Set up your profile
4. Connect your wallet (important for Web3 features!)

### 1.2 Explore Some Frames

Before building, experience some Frames as a user:

```
You: Show me some cool Farcaster Frames I can try

Claude: Here are some popular Frames to explore:
        1. Polls - Find any poll cast and vote
        2. @base's mint frames - One-click NFT mints
        3. Quiz games - Test your Web3 knowledge
        4. Tip frames - Send tips to creators
```

**How to find Frames:**
1. Open Warpcast
2. Search for "frame" or browse trending casts
3. Look for posts with interactive buttons
4. Click and interact!

### 1.3 Get Frame Developer Tools

```
You: What tools do I need to build Frames?

Claude: For Frame development, you'll want:
        1. Node.js 18+ (you should have this from earlier chapters)
        2. A code editor (VS Code)
        3. ngrok or similar for local testing
        4. Warpcast Frame Validator: https://warpcast.com/~/developers/frames
```

---

## Step 2: Understanding Frame Project Structure

### 2.1 The Anatomy of a Frame Project

```
my-frame/
├── app/
│   ├── page.tsx              # Home page with initial Frame meta tags
│   ├── api/
│   │   └── frame/
│   │       └── route.ts      # Handles button clicks (POST requests)
│   └── opengraph-image.tsx   # Dynamic image generation
├── public/
│   └── images/               # Static images for your Frame
├── lib/
│   └── frame-utils.ts        # Helper functions
├── .env.local                # Environment variables
├── next.config.js
└── package.json
```

### 2.2 Create the Project

```
You: Create a Farcaster Frame project using Next.js.
     Set it up with TypeScript and the latest Frame specifications.
     Include a basic "Hello World" frame.

Claude: I'll set up a complete Frame project...
        [Creates project structure]
        [Installs dependencies: @coinbase/onchainkit, viem]
        [Sets up initial Frame with proper meta tags]
```

### 2.3 Key Files Explained

**`app/page.tsx`** - The entry point of your Frame:

```tsx
// This file returns the HTML that Warpcast reads
// The meta tags tell Warpcast what to display

export default function Home() {
  return (
    <>
      {/* Frame meta tags go in the head */}
      <head>
        <meta property="fc:frame" content="vNext" />
        <meta property="fc:frame:image" content={`${process.env.NEXT_PUBLIC_URL}/api/og`} />
        <meta property="fc:frame:button:1" content="Click Me!" />
        <meta property="fc:frame:post_url" content={`${process.env.NEXT_PUBLIC_URL}/api/frame`} />
      </head>
      <body>
        <h1>My First Frame</h1>
        {/* This HTML is for browsers, not Warpcast */}
      </body>
    </>
  );
}
```

**`app/api/frame/route.ts`** - Handles button clicks:

```tsx
// This runs when someone clicks a button in your Frame
import { NextRequest, NextResponse } from 'next/server';

export async function POST(req: NextRequest) {
  // Parse the incoming request
  const body = await req.json();

  // Extract useful information
  const { untrustedData } = body;
  const buttonIndex = untrustedData.buttonIndex;  // Which button (1-4)
  const fid = untrustedData.fid;                  // User's Farcaster ID
  const inputText = untrustedData.inputText;      // Text they typed (if any)

  // Decide what to show next based on which button was clicked
  let newImage = '';
  let newButtons = [];

  if (buttonIndex === 1) {
    newImage = 'https://your-site.com/image-after-button-1.png';
    newButtons = ['Continue', 'Go Back'];
  }

  // Return new Frame HTML
  return new NextResponse(
    `<!DOCTYPE html>
    <html>
      <head>
        <meta property="fc:frame" content="vNext" />
        <meta property="fc:frame:image" content="${newImage}" />
        <meta property="fc:frame:button:1" content="${newButtons[0]}" />
        <meta property="fc:frame:button:2" content="${newButtons[1]}" />
      </head>
    </html>`
  );
}
```

> 💡 **Key Insight**: Your Frame is basically a conversation. User clicks → you respond with new image/buttons → user clicks again → repeat!

---

## Step 3: Build Your First Frame (Quiz)

### 3.1 Design the Flow

Before coding, plan your Frame's flow:

```
┌─────────────────────────────────────────────────────────────┐
│                    Quiz Frame Flow                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  SCREEN 1: Welcome                                          │
│  ┌────────────────────┐                                     │
│  │   🎯 Web3 Quiz!    │                                     │
│  │   Test your        │                                     │
│  │   knowledge!       │                                     │
│  └────────────────────┘                                     │
│  [Start Quiz]                                               │
│       │                                                     │
│       ▼                                                     │
│  SCREEN 2: Question                                         │
│  ┌────────────────────┐                                     │
│  │  Q1: What does     │                                     │
│  │  "HODL" mean?      │                                     │
│  └────────────────────┘                                     │
│  [A] [B] [C] [D]                                            │
│       │                                                     │
│       ▼                                                     │
│  SCREEN 3: Result                                           │
│  ┌────────────────────┐                                     │
│  │  ✅ Correct!       │  OR  │  ❌ Wrong!    │              │
│  │  +1 point          │      │  Answer was B │              │
│  └────────────────────┘                                     │
│  [Next Question]                                            │
│       │                                                     │
│       ▼                                                     │
│  SCREEN 4: Final Score                                      │
│  ┌────────────────────┐                                     │
│  │  🏆 Your Score:    │                                     │
│  │     4/5            │                                     │
│  │  Great job!        │                                     │
│  └────────────────────┘                                     │
│  [Play Again] [Share]                                       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 Create the Project

```
You: Create a quiz Frame with these features:
     1. Welcome screen with "Start Quiz" button
     2. 5 questions about Web3/crypto
     3. 4 answer options per question (A, B, C, D)
     4. Show correct/incorrect after each answer
     5. Final score screen
     6. "Play Again" button

     Use Next.js with TypeScript. Make the images dynamic using @vercel/og.

Claude: I'll create the complete quiz Frame...
        [Creates project structure]
        [Sets up questions array with correct answers]
        [Creates dynamic image generation]
        [Implements state management with URL params]
```

### 3.3 Managing State in Frames

Frames are **stateless** - each request is independent. How do we track progress?

**Option 1: URL Parameters (Simple)**
```
/api/frame?question=2&score=1
```

**Option 2: Encoded State (More Secure)**
```tsx
// Encode state into a single string
const state = Buffer.from(JSON.stringify({
  question: 2,
  score: 1,
  answers: [1, 3]
})).toString('base64');

// Pass it through the frame
<meta property="fc:frame:state" content={state} />
```

**Option 3: Database (For Persistence)**
```tsx
// Store in database with FID as key
await db.set(`quiz:${fid}`, {
  question: 2,
  score: 1,
  startedAt: Date.now()
});
```

### 3.4 Dynamic Image Generation

Create beautiful images on-the-fly:

```tsx
// app/api/og/route.tsx
import { ImageResponse } from '@vercel/og';

export async function GET(request: Request) {
  const { searchParams } = new URL(request.url);
  const question = searchParams.get('q') || '1';
  const score = searchParams.get('score') || '0';

  return new ImageResponse(
    (
      <div
        style={{
          display: 'flex',
          flexDirection: 'column',
          alignItems: 'center',
          justifyContent: 'center',
          width: '100%',
          height: '100%',
          backgroundColor: '#1a1a2e',
          color: 'white',
          fontSize: 48,
        }}
      >
        <div style={{ marginBottom: 20 }}>🎯 Web3 Quiz</div>
        <div style={{ fontSize: 32 }}>Question {question} of 5</div>
        <div style={{ fontSize: 24, marginTop: 10 }}>Score: {score}</div>
      </div>
    ),
    {
      width: 1200,
      height: 628,
    }
  );
}
```

---

## Step 4: Test Your Frame

### 4.1 Local Testing with ngrok

Since Warpcast needs a public URL, use ngrok for local development:

```bash
# Terminal 1: Run your app
npm run dev

# Terminal 2: Expose it publicly
ngrok http 3000
```

```
You: Help me set up ngrok for Frame testing

Claude: Let's get ngrok running:
        1. Install: npm install -g ngrok (or download from ngrok.com)
        2. Start your app: npm run dev
        3. In another terminal: ngrok http 3000
        4. Copy the https URL (like https://abc123.ngrok.io)
        5. Update your .env.local: NEXT_PUBLIC_URL=https://abc123.ngrok.io
        6. Restart your dev server
```

### 4.2 Use Frame Validator

The official validator shows exactly what users will see:

1. Go to https://warpcast.com/~/developers/frames
2. Paste your ngrok URL
3. See your Frame render
4. Click buttons to test the flow
5. Check for errors in the debug panel

```
You: The Frame validator shows an error. Here's what I see: [paste error]

Claude: Let me debug this...
        Common issues:
        1. Image URL not accessible (check CORS)
        2. Missing required meta tags
        3. Image wrong size (should be 1.91:1 or 1:1)
        4. post_url returning wrong content type
```

### 4.3 Debug Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| "Invalid frame" | Missing meta tags | Check `fc:frame` tag exists |
| Image not loading | URL not public | Use ngrok or deployed URL |
| Buttons don't work | Wrong post_url | Verify API route path |
| State not persisting | Not passing state | Use URL params or `fc:frame:state` |
| "Untrusted data" | Signature not verified | Use OnchainKit to verify |

---

## Step 5: Deploy to Vercel

### 5.1 Prepare for Deployment

```
You: Deploy my Frame to Vercel

Claude: Let's deploy step by step:

        1. Push to GitHub:
           git add .
           git commit -m "Quiz frame complete"
           git push

        2. Connect to Vercel:
           - Go to vercel.com
           - Import your GitHub repo
           - Vercel auto-detects Next.js

        3. Set environment variables:
           NEXT_PUBLIC_URL = https://your-project.vercel.app

        4. Deploy!
```

### 5.2 Environment Variables

In Vercel dashboard → Settings → Environment Variables:

| Variable | Value | Purpose |
|----------|-------|---------|
| `NEXT_PUBLIC_URL` | `https://your-app.vercel.app` | For generating absolute URLs |
| `NEYNAR_API_KEY` | Your Neynar key (optional) | For verifying Frame signatures |

### 5.3 Share Your Frame!

1. Copy your Vercel URL
2. Open Warpcast
3. Create a new cast
4. Paste ONLY the URL (no other text needed)
5. The Frame renders automatically!

```
You: How do I make my Frame go viral?

Claude: Tips for Frame distribution:
        1. POST AT THE RIGHT TIME - When your audience is active
        2. ADD CONTEXT - Explain what your Frame does in the cast text
        3. TAG RELEVANT CHANNELS - /frames, /base, etc.
        4. ENGAGE - Reply to people who use your Frame
        5. ITERATE - Check analytics and improve
```

---

## Step 6: Add On-Chain Actions

### 6.1 Add a Tip Button

Let users send you ETH directly from the Frame:

```
You: Add a tip button that lets users send me 0.001 ETH

Claude: I'll add a transaction button:
```

```html
<!-- Transaction button -->
<meta property="fc:frame:button:3" content="💰 Tip 0.001 ETH" />
<meta property="fc:frame:button:3:action" content="tx" />
<meta property="fc:frame:button:3:target" content="https://your-site.com/api/tip-tx" />
```

```tsx
// app/api/tip-tx/route.ts
export async function POST(req: NextRequest) {
  const YOUR_WALLET = '0x1234...'; // Your wallet address

  // Return transaction data
  return NextResponse.json({
    chainId: 'eip155:8453', // Base mainnet
    method: 'eth_sendTransaction',
    params: {
      to: YOUR_WALLET,
      value: '1000000000000000', // 0.001 ETH in wei
    },
  });
}
```

### 6.2 Add an NFT Mint Button

One-click NFT minting:

```
You: Add a button that mints an NFT from my contract

Claude: I'll create the mint transaction:
```

```html
<meta property="fc:frame:button:4" content="🖼️ Mint NFT" />
<meta property="fc:frame:button:4:action" content="tx" />
<meta property="fc:frame:button:4:target" content="https://your-site.com/api/mint-tx" />
```

```tsx
// app/api/mint-tx/route.ts
import { encodeFunctionData } from 'viem';

export async function POST(req: NextRequest) {
  const body = await req.json();
  const userAddress = body.untrustedData.address;

  const NFT_CONTRACT = '0xYourContract...';

  // Encode the mint function call
  const data = encodeFunctionData({
    abi: [{
      name: 'mint',
      type: 'function',
      inputs: [{ name: 'to', type: 'address' }],
      outputs: [],
    }],
    functionName: 'mint',
    args: [userAddress],
  });

  return NextResponse.json({
    chainId: 'eip155:84532', // Base Sepolia
    method: 'eth_sendTransaction',
    params: {
      to: NFT_CONTRACT,
      data: data,
    },
  });
}
```

### 6.3 Transaction Flow

```
┌─────────────────────────────────────────────────────────────┐
│                 Transaction Button Flow                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. User clicks "Mint NFT" button                           │
│                                                             │
│  2. Warpcast calls your /api/mint-tx endpoint               │
│                                                             │
│  3. Your server returns transaction data:                   │
│     {                                                       │
│       chainId: "eip155:84532",                             │
│       method: "eth_sendTransaction",                        │
│       params: { to: "0x...", data: "0x..." }               │
│     }                                                       │
│                                                             │
│  4. Warpcast shows transaction preview to user              │
│     ┌────────────────────────┐                              │
│     │ Mint NFT               │                              │
│     │ Contract: 0x1234...    │                              │
│     │ Network: Base Sepolia  │                              │
│     │                        │                              │
│     │ [Confirm] [Cancel]     │                              │
│     └────────────────────────┘                              │
│                                                             │
│  5. User confirms → Transaction sent → Success screen!      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 6.4 Track User Actions with Database

For features like vote counting, use a database:

```
You: I want to track votes in my poll Frame. How do I store the data?

Claude: I'll set up Vercel KV (Redis) for vote storage:
```

```tsx
// Using Vercel KV
import { kv } from '@vercel/kv';

export async function POST(req: NextRequest) {
  const body = await req.json();
  const fid = body.untrustedData.fid;
  const buttonIndex = body.untrustedData.buttonIndex;

  // Check if user already voted
  const hasVoted = await kv.sismember('voters', fid.toString());
  if (hasVoted) {
    return renderFrame({ message: "You already voted!" });
  }

  // Record vote
  await kv.sadd('voters', fid.toString());
  await kv.hincrby('votes', `option${buttonIndex}`, 1);

  // Get current results
  const results = await kv.hgetall('votes');

  return renderFrame({
    message: "Thanks for voting!",
    results: results
  });
}
```

---

## 🔨 Exercise 1: Build a Poll Frame (Beginner)

Ask Claude to help you build an interactive poll:

```
You: Help me build a Farcaster poll Frame with these features:
     1. Question: "What's your favorite blockchain?"
     2. Options: Ethereum, Base, Solana, Other
     3. Track votes using Vercel KV
     4. Show results after voting (with percentages)
     5. Prevent double voting (check by FID)
     6. Display total vote count

     Make it look clean and modern with a dark theme.

Claude: Let's build this poll Frame step by step...
```

**Expected outcome:**
- Working poll with 4 options
- Vote tracking that persists
- Results visualization
- Double-vote prevention

> 💡 **Vibecoding Approach**: Don't worry about the technical details. Describe what you want, test it, and refine!

---

## 🔨 Exercise 2: Build an NFT Mint Frame with Allowlist (Advanced)

This is a more challenging project that combines multiple concepts:

```
You: Help me build an advanced NFT mint Frame with these features:

     BASIC FEATURES:
     1. Welcome screen showing NFT preview image
     2. "Check Eligibility" button
     3. Show mint price (0.001 ETH on Base Sepolia)

     ADVANCED FEATURES:
     4. Allowlist check - only certain FIDs can mint
     5. "Already Minted" check - prevent double mints
     6. Dynamic image showing remaining supply (e.g., "42/100 minted")
     7. After successful mint, show "View on OpenSea" link button
     8. Track total mints in database

     BONUS:
     9. If not on allowlist, show a "Join Waitlist" button that adds their FID
     10. Show different messages based on wallet connection status

     Use Base Sepolia testnet. Include proper error handling.

Claude: This is a comprehensive Frame! Let me build it with all features...
```

### Expected Architecture:

```
┌─────────────────────────────────────────────────────────────┐
│               NFT Mint Frame Architecture                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  SCREEN 1: Welcome                                          │
│  ┌────────────────────┐                                     │
│  │  [NFT Preview]     │                                     │
│  │  "Exclusive NFT"   │                                     │
│  │  42/100 remaining  │                                     │
│  └────────────────────┘                                     │
│  [Check Eligibility]                                        │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────┬─────────┬─────────┐                           │
│  │ On List │Not List │ Already │                           │
│  │         │         │ Minted  │                           │
│  └────┬────┴────┬────┴────┬────┘                           │
│       │         │         │                                 │
│       ▼         ▼         ▼                                 │
│  SCREEN 2a   SCREEN 2b   SCREEN 2c                          │
│  "Eligible!" "Not on    "You already                        │
│  [Mint Now]  allowlist" own this NFT"                       │
│              [Waitlist] [View on OS]                        │
│       │         │                                           │
│       ▼         │                                           │
│  SCREEN 3      │                                            │
│  "Minting..."  │                                            │
│  [tx pending]  │                                            │
│       │        │                                            │
│       ▼        │                                            │
│  SCREEN 4     SCREEN 5                                      │
│  "Success!"   "Added to                                     │
│  [View OS]    waitlist!"                                    │
│  [Share]                                                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Key Challenges to Solve:

1. **Allowlist Management**
   ```tsx
   // Store allowlist FIDs
   const ALLOWLIST = new Set([12345, 67890, 11111]);

   // Or use database for dynamic allowlist
   const isAllowed = await kv.sismember('allowlist', fid.toString());
   ```

2. **Mint Tracking**
   ```tsx
   // Track who minted
   await kv.sadd('minted', fid.toString());
   await kv.incr('totalMinted');
   ```

3. **Dynamic Supply Image**
   ```tsx
   // Generate image with current supply
   const minted = await kv.get('totalMinted') || 0;
   const remaining = 100 - minted;
   // Pass to OG image generator
   ```

4. **Transaction + Callback**
   ```tsx
   // After tx succeeds, update database
   // Use fc:frame:button:post_url to handle callback
   ```

This exercise tests:
- State management across multiple screens
- Database operations
- Transaction handling
- Conditional rendering
- Error states

---

## ❌ Common Mistakes

| Mistake | Symptom | What to Ask Claude |
|---------|---------|-------------------|
| Frame not rendering | Shows as regular link | "My Frame shows as a regular link. Here's my URL: [url]. Debug the meta tags." |
| Buttons not working | Click does nothing | "Clicking buttons does nothing. Check my postUrl setup and API route." |
| Images not loading | Broken image icon | "Frame images aren't showing. Here's my image URL: [url]. Check CORS and size." |
| "Invalid frame" error | Validator shows error | "Frame validator says invalid. Here's my full HTML response: [code]" |
| State not persisting | Data lost between clicks | "My quiz score resets. Help me pass state between frames." |
| Transaction failing | Error on tx button | "Transaction button fails with: [error]. Here's my tx endpoint code." |
| Double actions | User can vote twice | "Users can vote multiple times. Add FID-based deduplication." |

---

## 🆘 When You're Stuck

### Understanding Frame Concepts

```
You: I don't understand how Frame buttons work. Explain it simply.

Claude: When a user clicks a button:
        1. Their click goes to YOUR server
        2. Your server decides what to show next
        3. You send back new image + buttons

        It's like a conversation where the user clicks instead of types.

        Think of it as a "choose your own adventure" book:
        - You show a page (image + buttons)
        - User picks an option (clicks button)
        - You show the next page based on their choice
```

### Debugging Deployments

```
You: My Frame works locally but not on Vercel. Help!

Claude: Let's debug:
        1. Check NEXT_PUBLIC_URL is set correctly
        2. Verify environment variables in Vercel dashboard
        3. Check Vercel function logs for errors
        4. Ensure all API routes are exported correctly
        5. Test the deployed URL in Frame validator
```

### Verifying Frame Data

```
You: How do I know the button click is legitimate?

Claude: Use signature verification with Neynar or OnchainKit:

        import { validateFrameMessage } from '@coinbase/onchainkit';

        const { isValid, message } = await validateFrameMessage(body);
        if (!isValid) {
          return new Response('Invalid', { status: 400 });
        }

        // Now you can trust message.data
```

---

## ✅ Checklist

Before moving on, confirm:

**Setup:**
- [ ] Farcaster account created (Warpcast)
- [ ] Development environment ready (Node.js, ngrok)
- [ ] Understand Frame meta tags
- [ ] Understand request/response cycle

**Building:**
- [ ] Built a working Frame locally (with Claude!)
- [ ] Created dynamic images
- [ ] Handled button clicks correctly
- [ ] Managed state across frames

**Testing:**
- [ ] Tested with Frame validator
- [ ] Debugged common issues
- [ ] Tested on actual Warpcast app

**Deployment:**
- [ ] Deployed to Vercel
- [ ] Set environment variables
- [ ] Shared and tested on Warpcast

**Exercises:**
- [ ] Completed Exercise 1: Poll Frame
- [ ] Completed Exercise 2: NFT Mint Frame with Allowlist

**Understanding:**
- [ ] Can explain how Frames work to someone else
- [ ] Understand transaction buttons
- [ ] Know how to persist state

---

## 📖 Learn More

### Official Documentation
- [Farcaster Frames Docs](https://docs.farcaster.xyz/developers/frames/)
- [Frames.js Documentation](https://framesjs.org/)
- [OnchainKit Frame Tools](https://onchainkit.xyz/frame)
- [Warpcast Developer Docs](https://docs.farcaster.xyz/)

### Tools
- [Frame Validator](https://warpcast.com/~/developers/frames) - Test your Frames
- [Neynar](https://neynar.com/) - Frame infrastructure
- [Pinata](https://pinata.cloud/) - Frame hosting

### Examples & Inspiration
- [Awesome Frames](https://github.com/farcasterxyz/awesome-frames)
- [Frame.js Examples](https://github.com/framesjs/frames.js/tree/main/examples)

### Videos
- [Building Farcaster Frames](https://www.youtube.com/results?search_query=farcaster+frames+tutorial)
- [Frame Development Deep Dive](https://www.youtube.com/results?search_query=farcaster+frame+development)

---

## What's Next?

In **Chapter 30**, you'll become a **Smart Contract Creator** - writing and deploying your own on-chain code!

### The Evolution of Your Web3 Skills

```
┌─────────────────────────────────────────────────────────────┐
│              Your Web3 Developer Journey                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Chapter 28: CONSUMER                                       │
│  "I can use Web3 apps and understand how they work"         │
│  └── Wallet, transactions, claiming tokens/NFTs             │
│                                                             │
│  Chapter 29: BUILDER (You are here!)                        │
│  "I can build interactive Web3 experiences"                 │
│  └── Frames that reach users through social media           │
│  └── On-chain actions (tips, mints, transactions)           │
│  └── State management and user tracking                     │
│                                                             │
│  Chapter 30: CREATOR                                        │
│  "I can create my own smart contracts"                      │
│  └── Write Solidity, deploy contracts, build dapps          │
│                                                             │
│  🎯 Final Project: NFT-Gated Community                      │
│  └── Combines EVERYTHING: contract + frontend + token gating│
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Why Smart Contracts After Frames?

| In Frames (Ch29) | In Smart Contracts (Ch30) |
|------------------|---------------------------|
| You used **existing** contracts | You **create** your own contracts |
| Limited to what contracts allow | **Full control** over logic |
| Quick to build, viral distribution | More powerful, more responsibility |
| Called contract functions | Define contract functions |

### What You'll Build in Chapter 30

> 💡 **Preview**: In Chapter 30, you'll write your own NFT contract and build a complete membership site where only NFT holders can access exclusive content. Imagine combining:
> - Your own smart contract (like TipJar or NFT)
> - A Frame that mints from YOUR contract
> - A gated community website
>
> This is the culmination of everything you've learned!

---

## 🌉 Bridge to Chapter 30: Smart Contracts Preview

Before moving on, let's peek at what's coming! In Chapter 30, you'll transition from using **existing contracts** to **creating your own**.

### Why Write Your Own Smart Contracts?

In this chapter, you used `encodeFunctionData` to call existing contracts:

```tsx
// What you did in Chapter 29 - calling someone else's contract
const data = encodeFunctionData({
  abi: [...],
  functionName: 'mint',
  args: [userAddress],
});
```

But what if you want to:
- Create your own NFT with custom rules?
- Build a tip jar where you control the logic?
- Set up a membership system with your own requirements?

You need to write **Solidity** - the language of smart contracts!

### Your First Look at Solidity

Here's a sneak peek at what you'll learn. This is a simple smart contract:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MyFirstContract {
    // A variable stored on blockchain forever!
    string public message;

    // This runs when someone deploys the contract
    constructor() {
        message = "Hello, Web3!";
    }

    // Anyone can call this to change the message
    function setMessage(string memory newMessage) public {
        message = newMessage;
    }
}
```

```
You: This looks like JavaScript!

Claude: It is similar! Key differences:
        - Runs on blockchain, not a server
        - Every change costs "gas" (tiny fee)
        - Once deployed, code can't be changed
        - Everything is public and permanent

        Don't worry - in Chapter 30, I'll write it WITH you!
```

### How Frames + Smart Contracts Work Together

Here's the powerful combination you'll master:

```
┌─────────────────────────────────────────────────────────────┐
│           The Complete Web3 Builder Stack                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  CHAPTER 30: Your Smart Contract                            │
│  ┌─────────────────────────────┐                           │
│  │ contract MembershipNFT {    │                           │
│  │   function mint() public {} │  ← You write this!        │
│  │   function hasPass() ...    │                           │
│  │ }                           │                           │
│  └──────────────┬──────────────┘                           │
│                 │                                           │
│                 ▼                                           │
│  CHAPTER 29: Your Frame                                     │
│  ┌─────────────────────────────┐                           │
│  │ [NFT Preview Image]         │                           │
│  │ "Join our community!"       │                           │
│  │ [Mint Membership NFT]       │  ← Calls YOUR contract!   │
│  └──────────────┬──────────────┘                           │
│                 │                                           │
│                 ▼                                           │
│  YOUR WEBSITE: Token-Gated Content                          │
│  ┌─────────────────────────────┐                           │
│  │ if (hasNFT) {               │                           │
│  │   show secret content       │  ← Uses YOUR contract!    │
│  │ }                           │                           │
│  └─────────────────────────────┘                           │
│                                                             │
│  This is the FINAL PROJECT in Chapter 30!                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Safety First: The One Thing You Must Know

> ⚠️ **Important Heads-Up for Chapter 30**

Smart contracts handle real money, and mistakes can be permanent. In Chapter 30, you'll learn:

```
Vibecoding Smart Contracts Safely:

✅ SAFE to vibecode:
- Learning and experimenting on testnet
- Understanding concepts
- Building prototypes
- Small, simple contracts

❌ DANGEROUS to vibecode:
- Production contracts with real money
- Complex DeFi logic
- Anything handling significant value

The rule: TESTNET FIRST, ALWAYS!
```

Don't let this scare you - we'll practice safely on testnet where mistakes cost nothing. But this awareness is why you're ready for Chapter 30!

### What's Coming

| Chapter 30 Section | What You'll Learn |
|-------------------|-------------------|
| Smart Contract Basics | What contracts are, how they work |
| Your First Contract | Deploy "Hello World" to blockchain |
| TipJar Contract | Build something useful with ETH |
| Frontend Connection | Connect your Next.js app to your contract |
| NFT Contract | Create your own ERC721 NFT |
| **Final Project** | NFT-Gated Community (combines everything!) |

> 🚀 **You're Ready!** You've mastered wallet connections (Ch28), built interactive Frames (Ch29), and now you're ready to become a smart contract creator. Let's go!

---

<p align="center">
  <a href="../Chapter28-Web3-Basics/README.md">← Previous: Web3 Basics</a> |
  <a href="../Chapter30-Base-Smart-Contracts/README.md">Next: Base Smart Contracts →</a>
</p>

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
