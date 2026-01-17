# Chapter 14: Deployment

**English** | [한국어](./README.ko.md)

---

## Ask Questions

If you have questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask_Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## What You Will Learn

- What deployment is and why it matters
- Free deployment with Vercel (frontend)
- Backend deployment with Railway
- Automatic deployment and CI/CD concepts
- Connecting custom domains
- Deployment troubleshooting

---

## Connection to Previous Chapters

In Chapter 13, you created a beautiful portfolio website. But right now, **only you can see it**.

```
Current state:
👤 Me → 💻 My Computer → 🌐 localhost:3000
          ↑
       (Only works here)
```

It's like cooking a delicious meal and eating it alone in a locked room. Now it's time to open the door!

---

## Why Do You Need This?

### Real-World Scenarios Where You Need Deployment

| Scenario | Without Deployment | After Deployment |
|----------|-------------------|------------------|
| **Job application** | "My portfolio... I'll show you later" | "Here's my portfolio: portfolio.vercel.app" |
| **Client meeting** | Need to schedule screen-share meeting | Instant verification with one link |
| **Team collaboration** | "Come to my computer to see it" | "Test it at this URL" |
| **Collecting feedback** | Send screenshots and explain | "Try it yourself and give feedback" |

> Until you deploy, your project **"doesn't exist"** to the outside world.

### Simple Analogy: From Recipe to Restaurant

```
Recipe development (localhost)
→ Practicing cooking in home kitchen
→ Only I can eat
→ Others can't see my cooking

Restaurant opening (deployment)
→ Store opens!
→ Anyone can come taste the food
→ Get real customer feedback
```

`localhost:3000` = Your kitchen (only you can eat)
`yoursite.vercel.app` = Your restaurant (anyone can visit)

> **Beginner Tip**: Deployment is "putting my work on the internet." Just like uploading a video to YouTube, you upload your website.

---

## Try It Yourself: Deploy in 5 Minutes

Before diving deep, let's prove how easy this is.

### Prerequisites

1. **GitHub account**: Sign up at [github.com](https://github.com)
2. **Vercel account**: [vercel.com](https://vercel.com) (sign up with GitHub)
3. **Project from Chapter 13**: Portfolio website

### 5-Minute Deployment

```
# Step 1: Upload to GitHub
> Initialize git, create a GitHub repository called "my-first-deploy",
> and push all files. Make it public.
```

```
# Step 2: Connect Vercel
1. Go to vercel.com
2. Click "New Project"
3. Select GitHub repository (my-first-deploy)
4. Click "Deploy"

# Step 3: Wait (1-2 minutes)

# Step 4: Done!
🎉 https://my-first-deploy-abc123.vercel.app
```

If it worked, you now have a live website! **Share the link with a friend right now.**

> **Pro tip**: It's okay if it fails. This chapter covers troubleshooting in detail.

---

## What is Deployment?

Deployment is **putting code from your computer onto an internet server so anyone can access it**.

### Before vs After Deployment

```
Before deployment:
┌──────────────┐
│  My Computer │  → localhost:3000
│  (Local)     │     ↑ Only I can access
└──────────────┘

After deployment:
┌──────────────┐     ┌──────────────┐
│  My Computer │  →  │ Cloud Server │  → mysite.vercel.app
│  (Local)     │     │  (Vercel)    │     ↑ Anyone in the world can access
└──────────────┘     └──────────────┘
```

| State | Address | Who Can See It |
|-------|---------|----------------|
| Before | `http://localhost:3000` | Only me |
| After | `https://my-site.vercel.app` | Anyone in the world |

### What You Get After Deployment

- **Public URL**: A link you can share with anyone
- **HTTPS**: Secure connection (lock icon)
- **24/7 Access**: Site works even when your computer is off
- **Global Access**: Fast loading from anywhere in the world

---

## Comparing Deployment Services

### Static Sites (Frontend)

| Service | Free Tier | Features | Recommended For |
|---------|-----------|----------|-----------------|
| **Vercel** | Unlimited | Easiest, Next.js optimized | Beginners, React |
| **Netlify** | Unlimited | Similar to Vercel, form features | Vercel alternative |
| **GitHub Pages** | Unlimited | GitHub integration, static only | Simple sites |
| **Cloudflare Pages** | Unlimited | Fast, edge computing | Performance focus |

### Backend/Full-Stack

| Service | Free Tier | Features | Recommended For |
|---------|-----------|----------|-----------------|
| **Railway** | $5 credit/month | Easy, DB support | Node.js, Python |
| **Render** | 750 hours/month | Various languages | General backends |
| **Fly.io** | Free tier | Docker-based, global | Advanced users |
| **Supabase** | Free tier | PostgreSQL + API | BaaS needs |

### Which Should You Choose?

```
                    What did you build?
                         │
           ┌─────────────┴─────────────┐
           ▼                           ▼
    HTML/CSS/JS only              Need server/API
    (Static site)                 (Has backend)
           │                           │
           ▼                           ▼
    ┌─────────────┐           ┌─────────────┐
    │   Vercel    │           │   Railway   │
    │ (Recommend!)│           │ (Recommend!)│
    └─────────────┘           └─────────────┘
```

> **Beginner Tip**: Start with Vercel for everything. Add Railway later when you need a backend.

---

## Vercel: The Easiest Deployment

Vercel is the most popular service for frontend deployment.

### Why Vercel?

- **Free**: Completely free for personal projects
- **Easy**: Just a few clicks
- **Fast**: Global CDN for fast loading
- **Auto HTTPS**: Automatic security certificate
- **Auto Deploy**: Updates automatically when you git push
- **Preview**: Creates test URLs for each PR

### Prerequisites

1. GitHub account
2. Vercel account (sign up with GitHub recommended)
3. Project to deploy

---

## Detailed Vercel Deployment Guide

### Step 1: Upload Code to GitHub

```
> Upload this project to GitHub.
> Repository name: my-portfolio.
> Make it public.
```

**Actual conversation example:**

```
User: Upload this project to GitHub. Repository name is my-portfolio

Claude: I'll upload to GitHub.

[Running: git init]
[Running: git add .]
[Running: git commit -m "Initial commit"]
[Running: gh repo create my-portfolio --public]
[Running: git push -u origin main]

✅ GitHub repository created!
URL: https://github.com/yourusername/my-portfolio

Now connect this repository in Vercel.
```

### Step 2: Connect Vercel

**On the website:**

1. Go to [vercel.com](https://vercel.com)
2. "Sign Up" → "Continue with GitHub"
3. Approve GitHub integration
4. Click "New Project"
5. Find `my-portfolio` in repository list
6. Click "Import"
7. Check settings (usually defaults are OK)
8. Click "Deploy"

**Ask Claude to verify:**

```
> Check if my Vercel deployment settings are correct.
> This project is pure HTML/CSS/JS.
```

```
Claude: Vercel settings for pure HTML/CSS/JS project:

Framework Preset: Other
Build Command: (leave empty)
Output Directory: (leave empty or . )
Install Command: (leave empty)

Deploy with these settings.
If index.html is in root, it will be recognized automatically.
```

### Step 3: Deployment Complete

When deployment finishes, you receive addresses:

```
✅ Deployment complete!

Production: https://my-portfolio-abc123.vercel.app
Preview: https://my-portfolio-git-main-yourusername.vercel.app

Anyone can view my website at these addresses.
```

> **Note**: First deployment may take 1-3 minutes. If you see "Building...", just wait.

---

## Automatic Deployment (CI/CD)

Vercel's most powerful feature: **when you modify code, it automatically redeploys**.

### Auto Deployment Flow

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ Modify Code │ →  │ Git Push    │ →  │ Auto Deploy │
│ (Local)     │    │ (GitHub)    │    │ (Vercel)    │
└─────────────┘    └─────────────┘    └─────────────┘
                                            │
                                            ▼
                                    ┌─────────────┐
                                    │ 1-2 min     │
                                    │ later       │
                                    │ New version │
                                    │ live        │
                                    └─────────────┘
```

### Actual Workflow

```
# 1. Modify code
> Change the title from "Hello" to "Welcome"

# 2. Commit and push
> Commit and push the changes

# 3. Auto deployed!
# Vercel dashboard shows "Building..."
# 1-2 minutes later, new version is live!

# 4. Verify
Refresh browser to see changes reflected
```

### Check Deployment Status

```
> Check Vercel deployment status
```

Or check directly in Vercel dashboard:
- **Production**: Currently live version
- **Preview**: Test deployment
- **Building**: Build in progress
- **Error**: Problem occurred (check logs)

> **Pro tip**: When you create a PR (Pull Request), Vercel automatically creates a preview URL. Very useful for team reviews!

---

## Connecting Custom Domains

Instead of the default address (`xxx.vercel.app`), you can use your own domain.

### Domain Purchase Options

| Service | Features | .com Price (Annual) |
|---------|----------|---------------------|
| [Namecheap](https://www.namecheap.com) | Global, cheap | ~$10 |
| [Google Domains](https://domains.google) | Simple, clean | ~$12 |
| [Cloudflare](https://www.cloudflare.com) | Free DNS, security | At cost |
| [GoDaddy](https://www.godaddy.com) | Well-known | ~$12 |

### Connecting Domain to Vercel

**1. Add domain in Vercel:**
```
1. Vercel dashboard → Select project
2. Settings → Domains
3. Enter domain (e.g., myname.com)
4. Click "Add"
```

**2. DNS settings:**

Add the DNS records Vercel provides to your domain registrar:

```
Type: A
Name: @
Value: 76.76.21.21

Type: CNAME
Name: www
Value: cname.vercel-dns.com
```

**3. Verify:**
```
> Check if custom domain is connected
```

> **Note**: DNS propagation can take up to 48 hours. Usually it's done within minutes to hours.

---

## Railway: Backend Deployment

If you need a server (API, database) rather than just a static website, use Railway.

### When Do You Need Railway?

```
✅ Vercel alone is sufficient:
- Portfolio site
- Blog
- Static landing page
- Client-side only apps

🔄 Railway is needed:
- Storing user data (DB)
- Login/signup features
- Running API server
- Server-side logic
```

### Railway Example: Simple API Server

```
> Create an Express server.
> Returns { message: "Hello" } when accessing /api/hello.
```

**Code Claude creates:**

```javascript
// server.js
const express = require('express');
const app = express();

// Port setting (Railway uses PORT environment variable)
const PORT = process.env.PORT || 3000;

// API endpoint
app.get('/api/hello', (req, res) => {
    res.json({ message: 'Hello' });
});

// Start server
app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
```

```json
// package.json
{
  "name": "my-api",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.0"
  }
}
```

### Railway Deployment Steps

**1. Sign up for Railway:**
- Go to [railway.app](https://railway.app)
- Log in with GitHub

**2. Create project:**
```
1. Click "New Project"
2. Select "Deploy from GitHub repo"
3. Select repository
4. Deployment starts automatically!
```

**3. Set environment variables (if needed):**
```
1. Project → Variables tab
2. Click "New Variable"
3. Name: DATABASE_URL, Value: (connection string)
```

**4. Check domain:**
```
After deployment, Railway provides URL:
https://my-api-production-abc123.up.railway.app

Test:
https://my-api-production-abc123.up.railway.app/api/hello
→ { "message": "Hello" }
```

> **Beginner Tip**: Railway gives $5 free credits per month. Enough for simple learning/side projects.

---

## Mini Quiz

### Quiz 1
Q: What's the difference between localhost:3000 and yoursite.vercel.app?

<details>
<summary>Show Answer</summary>

- `localhost:3000`: Only accessible from my computer (for local development)
- `yoursite.vercel.app`: Published on the internet, accessible by anyone in the world (deployed)
</details>

### Quiz 2
Q: Why does Vercel automatically deploy when you git push?

<details>
<summary>Show Answer</summary>

Because Vercel is "watching" your GitHub repository. When a new commit is pushed, it automatically detects it, builds, and deploys. This is called CI/CD (Continuous Integration/Continuous Deployment).
</details>

### Quiz 3
Q: Which should you use, Vercel or Railway?

<details>
<summary>Show Answer</summary>

- **Vercel**: Static sites (HTML/CSS/JS, React and other frontend)
- **Railway**: When you need server/API (Node.js, Python backend, database)

In most cases, start with Vercel and add Railway when you need a backend.
</details>

---

## Practice Exercises

### Difficulty 1: First Deployment (Beginner)

Deploy the portfolio from Chapter 13.

```
# 1. Upload to GitHub
> Upload this project to GitHub. Repository name is my-portfolio.

# 2. Connect Vercel
Go to vercel.com → New Project → Select repository → Deploy

# 3. Check URL
Open deployed URL in browser

# 4. Share with friend
Send link to friend and ask them to verify
```

**Completion checklist:**
- [ ] GitHub repository created
- [ ] Vercel deployment successful
- [ ] Accessible via public URL
- [ ] Verified someone else can see it

### Difficulty 2: Auto Deployment Test (Intermediate)

```
# 1. Modify code
> Add "Made with Claude Code" to the footer

# 2. Commit and push
> Commit and push

# 3. Check Vercel dashboard
Verify build status changes from "Building" to "Ready"

# 4. Check site
Refresh to verify changes are reflected
```

**Completion checklist:**
- [ ] Local changes committed
- [ ] Pushed to GitHub
- [ ] Vercel started auto build
- [ ] New version reflected live

### Difficulty 3: Custom Domain (Advanced)

Purchase a domain and connect it.

```
# 1. Purchase domain (optional)
Buy desired domain from Namecheap, etc.

# 2. Add domain in Vercel
Settings → Domains → Enter domain

# 3. DNS settings
Add DNS records Vercel provides at domain registrar

# 4. Verify
Test access at https://yourdomain.com
```

**Completion checklist:**
- [ ] Domain purchased/owned
- [ ] DNS records configured
- [ ] HTTPS working
- [ ] Both www and non-www work

---

## Challenge Exercises

### Challenge 1: Deploy API Server
```
> Create a simple API server.
> Returns project list JSON on GET /api/projects.
> Make it deployable to Railway.
```

### Challenge 2: Connect Frontend + Backend
```
> Fetch and display project list from Railway API in the portfolio.
> Use fetch for API calls.
```

### Challenge 3: Environment Variables
```
> Manage API URL with environment variables.
> Use localhost locally, real URL when deployed.
```

---

## Deployment Troubleshooting

### "Git command not found" Error

Git isn't installed.

```
> Tell me how to install git on my computer
```

### GitHub Push Failed / Authentication Error

```
> Help me set up GitHub authentication. I'm getting a push error.
```

Common solutions:
1. Check GitHub login status
2. Generate and use Personal Access Token
3. Set up SSH keys

**Actual conversation example:**

```
User: git push isn't working. Getting permission denied error.

Claude: This is a GitHub authentication issue. Solutions:

1. Personal Access Token method (recommended):
   - GitHub → Settings → Developer settings
   - Personal access tokens → Tokens (classic)
   - Generate new token
   - Check "repo" permission
   - Copy token

2. Try push again:
   Username: (GitHub ID)
   Password: (paste copied token)

Or you can set up SSH keys. Which method would you like?
```

### Vercel Build Failed

```
> Vercel build error: [paste error message]
> What's wrong and how do I fix it?
```

**Common build errors:**

| Error | Cause | Solution |
|-------|-------|----------|
| `Module not found` | Package missing | `npm install` then push |
| `Build command failed` | Build script error | Check package.json |
| `Output directory not found` | Output folder setting wrong | Fix in Vercel settings |

### 404 Error After Deployment

```
> Deployed site shows 404. Check the file structure.
```

**Common causes:**
- Main file not named `index.html`
- File is in subfolder (not root)
- Case mismatch (`Index.html` vs `index.html`)

### Styles Look Broken

```
> Deployed site has no styles. Check CSS links.
```

**Common causes:**
- CSS file path is wrong (relative vs absolute path)
- File name case mismatch
- CDN connection issue (Tailwind, etc.)

### Environment Variables Not Working

```
> API key isn't working. Tell me how to check environment variables.
```

**Checklist:**
1. Did you add environment variables in Vercel dashboard?
2. Is the variable name exact? (case-sensitive)
3. Did you redeploy after adding?
4. For frontend, need `VITE_` or `NEXT_PUBLIC_` prefix

---

## Common Mistakes

### Mistake 1: Committing Sensitive Information

**Never commit these to GitHub:**
- API keys
- Passwords
- Database connection strings
- `.env` files

```
> Check if there's any sensitive information in my code before pushing
```

**Prevention:**
```
# Add to .gitignore file
.env
.env.local
.env.production
```

> **Note**: If you accidentally committed secrets, **change the keys immediately**. They remain visible in git history even after deletion!

### Mistake 2: Not Adding node_modules to .gitignore

```
# ❌ node_modules gets committed
(Thousands of files uploaded to GitHub)
(Repository becomes huge)
(Push/pull becomes very slow)

# ✅ Add to .gitignore
> Check if node_modules is in .gitignore
```

### Mistake 3: Using Local Absolute Paths

```html
<!-- ❌ Wrong - Only works on my computer -->
<img src="C:/Users/John/project/images/photo.png">
<link href="D:/projects/style.css">

<!-- ✅ Correct - Use relative paths -->
<img src="./images/photo.png">
<link href="./style.css">
```

### Mistake 4: Not Waiting for Deployment to Finish

```
❌ Check site immediately after push → See old version
✅ Verify "Ready" in Vercel dashboard → See new version
```

Deployments take 1-3 minutes. Be patient.

### Mistake 5: Wrong Build Settings

```
> What are the correct Vercel build settings for my project?
```

**Settings by framework:**

| Project Type | Framework Preset | Build Command | Output Directory |
|--------------|------------------|---------------|------------------|
| Pure HTML | Other | (empty) | (empty) |
| React (CRA) | Create React App | `npm run build` | `build` |
| React (Vite) | Vite | `npm run build` | `dist` |
| Next.js | Next.js | (auto) | (auto) |
| Vue | Vue.js | `npm run build` | `dist` |

---

## Deployment Checklist

Verify before deploying:

**Code:**
- [ ] No errors in code (works locally)
- [ ] No sensitive information (API keys) in code
- [ ] .gitignore includes node_modules, .env

**Files:**
- [ ] Main file is index.html (or follows framework rules)
- [ ] All image paths are relative
- [ ] No Korean/special characters in file names

**Settings:**
- [ ] package.json scripts are correct (if needed)
- [ ] Environment variables set in Vercel (if needed)
- [ ] Build settings match framework

**Testing:**
- [ ] Final check locally
- [ ] Test all page access after deployment
- [ ] Check on mobile too

---

## Glossary

| Term | Description |
|------|-------------|
| **Deployment** | Publishing code to a server |
| **CI/CD** | Continuous Integration/Continuous Deployment - automated build and deploy |
| **Build** | Process of converting code to runnable form |
| **Static site** | Site made of only HTML/CSS/JS without server logic |
| **CDN** | Content Delivery Network, serves content quickly via global servers |
| **Environment variables** | Values set outside code (API keys, etc.) |
| **Domain** | Website address (e.g., google.com) |
| **DNS** | System that converts domain names to IP addresses |
| **HTTPS** | Secure HTTP connection (lock icon) |
| **SSL/TLS** | Security certificate for HTTPS |

---

## Summary

What you learned in this chapter:
- [x] What deployment is and why it's needed
- [x] Deploying frontend with Vercel
- [x] Deploying backend with Railway
- [x] Automatic deployment (CI/CD) concepts
- [x] Connecting custom domains
- [x] Deployment troubleshooting

> **Key point**: You can now share your projects with the world. Deployment isn't the end of development - it's **the beginning of meeting real users**!

---

## Next Chapter Preview

In Chapter 15, you'll learn about **data storage**.

The websites we've made so far are "static." When you refresh, entered content disappears.

In the next chapter:
- Storing data in browser with localStorage
- CRUD (Create, Read, Update, Delete) concepts
- Building a todo list app
- Database integration basics

Let's build a "real" app that remembers user data!

Proceed to [Chapter 15: Data Storage](../Chapter15-Data-Storage/README.md).

---

## Learn More

### Recommended Resources

**Official Documentation:**
- [Vercel Official Docs](https://vercel.com/docs) - Complete Vercel guide
- [Netlify Official Docs](https://docs.netlify.com/) - Netlify deployment guide
- [Railway Official Docs](https://docs.railway.app/) - Railway backend deployment

**Video Resources:**
- [Vercel Deployment Tutorial (YouTube)](https://www.youtube.com/results?search_query=vercel+deployment+tutorial) - Vercel deployment methods
- [CI/CD Explained (YouTube)](https://www.youtube.com/results?search_query=ci+cd+explained+beginners) - Automatic deployment concepts

**Reading:**
- [GitHub Pages Guide](https://pages.github.com/) - Free static hosting
- [Cloudflare Pages](https://pages.cloudflare.com/) - Fast static hosting

**Related Tools:**
- [Vercel](https://vercel.com/) - Frontend deployment platform
- [Netlify](https://www.netlify.com/) - Static site hosting
- [Railway](https://railway.app/) - Backend deployment platform
- [Namecheap](https://www.namecheap.com/) - Domain purchase

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
