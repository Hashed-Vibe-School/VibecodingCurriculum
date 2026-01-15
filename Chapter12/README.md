# Chapter 12: Deployment

**English** | [한국어](./README.ko.md)

## What You Will Learn

- What deployment is
- Free deployment with Vercel
- Backend deployment with Railway

---

## Why Do You Need This?

You've built something amazing. But right now, only YOU can see it. It's like cooking a delicious meal and eating it alone in a locked room.

**Real-world scenarios where you need deployment:**

- **Job applications**: "Here's my portfolio" followed by a real link is 100x better than "I'll show you sometime"
- **Client presentations**: Share your work instantly without scheduling screen-share meetings
- **Collaboration**: Let teammates view and test your project from anywhere
- **Feedback**: Get opinions from friends, mentors, or the internet community

> Until you deploy, your project doesn't really "exist" to the outside world.

### Simple Analogy: From Recipe to Restaurant

Building a website on your computer is like perfecting a recipe in your home kitchen. Deployment is opening a restaurant where anyone can come and taste your food.

`localhost:3000` = Your kitchen (only you can eat)
`yoursite.vercel.app` = Your restaurant (everyone can visit)

---

## Try It Yourself: Deploy in 5 Minutes

Before we dive deep, let's prove how easy this can be. If you already have a project from Chapter 11:

```
> Initialize git, create a GitHub repository called "my-first-deploy",
> and push all files to it. Make it public.
```

Then go to [vercel.com](https://vercel.com), click "New Project", select your repo, click "Deploy". Done!

If it worked, you now have a live website. Share the link with a friend right now. Feel that? That's the magic of deployment.

---

## What is Deployment?

Deployment is the process of putting your website from your computer onto the internet.

After deployment:
- Anyone can access your website
- You can share the link
- Use it as a portfolio

### Before vs After Deployment

| State | Address | Who Can See It |
|-------|---------|----------------|
| Before | `http://localhost:3000` | Only you |
| After | `https://my-site.vercel.app` | Anyone in the world |

---

## Vercel: The Easiest Deployment

Vercel is a service that enables free website deployment.

### Why Vercel?

- Free
- Simple (only a few clicks required)
- Fast
- Automatic HTTPS

### Prerequisites

1. GitHub account
2. Vercel account (sign up with GitHub)

---

## Deploying with Vercel

### Step 1: Upload Code to GitHub

```
> Upload this project to GitHub.
> Repository name: my-portfolio
```

Claude will:
1. Initialize Git
2. Create repository on GitHub
3. Push code

**Deployment request tips:**

```
> Upload this project to GitHub. Make the repository public.
> Use "Initial commit" as the commit message.
```

Specify repository settings (public/private) or commit messages to deploy exactly as you want.

### Step 2: Connect Vercel

1. Go to [vercel.com](https://vercel.com)
2. Log in with GitHub
3. Click "New Project"
4. Select the repository you just created
5. Click "Deploy"

### Step 3: Complete

When deployment finishes, you receive an address:
```
https://my-portfolio-abc123.vercel.app
```

Anyone can access your website at this address.

---

## Automatic Deployment

A key advantage of Vercel: when you modify code, it automatically redeploys.

### Modify and Auto Deploy Flow

```
1. Modify code
> Change the title to "Hello"

2. Push to GitHub
> Commit and push the changes

3. Automatically deployed (takes 1-2 minutes)

4. New version is live
```

### Check Deployment Status

```
> Check Vercel deployment status
```

Or check directly in the Vercel dashboard.

---

## Connecting a Custom Domain

Instead of the default address (`xxx.vercel.app`), you can use your own domain.

### Purchase a Domain

- [Namecheap](https://www.namecheap.com)
- [Google Domains](https://domains.google)
- [GoDaddy](https://www.godaddy.com)

### Connect to Vercel

1. Vercel dashboard → Settings → Domains
2. Enter domain (e.g., `myname.com`)
3. Follow DNS setup instructions

```
> Tell me how to set up a custom domain
```

---

## Railway: Backend Deployment

If you need a server rather than just a static website, use Railway.

### When to Use Railway

- When you need a database
- When you need an API server
- When you have a Node.js/Python backend

### Example

```
> Create an Express server.
> Returns "Hello" when accessing /api/hello.
```

---

## Deploying with Railway

### Step 1: Sign Up for Railway

Sign up with GitHub at [railway.app](https://railway.app)

### Step 2: Create Project

1. Click "New Project"
2. Select "Deploy from GitHub repo"
3. Select repository
4. Deployment starts automatically

### Step 3: Set Environment Variables (If Needed)

```
> Tell me how to set environment variables in Railway
```

Sensitive information such as passwords or API keys should be stored in environment variables.

---

## Free Plan Comparison

| Service | Free Tier | Best For |
|---------|-----------|----------|
| Vercel | Unlimited static sites | Portfolios, blogs |
| Railway | $5 credit/month | Simple backends |
| Netlify | Unlimited static sites | Vercel alternative |
| Render | 750 hours/month | Backends |

### Start with Vercel

For static websites, Vercel is the simplest and most effective option.
Add Railway when you need a backend.

---

## Troubleshooting Deployment

### Build Errors

```
> Got Vercel build error: [error message]
> What's wrong?
```

### Page Does Not Load

```
> Deployed but getting 404 error. Why?
```

Common causes:
- File is not named `index.html`
- Path is incorrect
- Build settings do not match

### Environment Variable Issues

```
> API key is not working. Check the environment variables.
```

---

## Practice: Deploy Your Portfolio

### Basic Tasks

1. Upload the portfolio from Chapter 11 to GitHub
2. Deploy with Vercel
3. Check the deployed URL

### Step-by-Step Guide

```
# 1. Upload to GitHub
> Upload this project to GitHub

# 2. Verify
> Check git status

# 3. Deploy on Vercel
# (Go to vercel.com in browser)

# 4. Make changes after deployment
> Add "Made with Claude Code" to the footer
> Commit and push

# 5. Check auto deployment
# (New version reflects in 1-2 minutes)
```

---

## Extra Challenges

### Deploy an API Server

```
> Create a simple API server.
> Returns project list on GET /api/projects.
> Make it deployable to Railway.
```

### Frontend + Backend

```
> Fetch and display the project list from the API server in the portfolio
```

---

## Deployment Checklist

Verify before deploying:

- [ ] No errors in code
- [ ] All image paths are correct
- [ ] No sensitive information (API keys) in code
- [ ] README.md is complete
- [ ] node_modules is in .gitignore

---

## If It Doesn't Work?

Deployment can fail for various reasons. Here are quick fixes:

### "Git command not found"
Git isn't installed. Ask Claude:
```
> How do I install git on my computer?
```

### GitHub push failed / authentication error
- Make sure you're logged into GitHub
- If using HTTPS, you might need a personal access token
- Try:
```
> Help me set up GitHub authentication. I'm getting a push error.
```

### Vercel build failed
The most common issue! Check the error message and ask:
```
> Vercel build failed with error: [paste error here]
> What's wrong and how do I fix it?
```

### Page shows 404 after deployment
- Your main file might not be named `index.html`
- The file might be in a subfolder
```
> My deployed site shows 404. Check the file structure.
```

### Site loads but looks broken (no styles)
- CSS file paths might be wrong
- Check for case sensitivity in file names
```
> My deployed site has no styles. Check the CSS links.
```

### Environment variables not working
- Did you add them in Vercel dashboard?
- Did you redeploy after adding them?
- Variable names are case-sensitive

---

## Common Mistakes

Learn from others' errors!

### Mistake 1: Committing sensitive information
**Never commit these to GitHub:**
- API keys
- Passwords
- `.env` files with secrets

```
> Check if there's any sensitive information in my code before pushing
```

If you accidentally committed secrets, change them immediately - the old values might be visible in git history.

### Mistake 2: Forgetting node_modules in .gitignore
This folder can be huge and should never be committed:
```
> Make sure node_modules is in .gitignore
```

### Mistake 3: Using absolute local paths
This works on your computer:
```html
<img src="C:/Users/John/project/image.png">
```
This breaks after deployment! Use relative paths:
```html
<img src="./images/photo.png">
```

### Mistake 4: Not waiting for deployment to finish
Deployments take 1-2 minutes. If you check immediately after pushing, you might see the old version. Be patient!

### Mistake 5: Wrong build settings
If your project uses a framework (React, Vue, etc.), Vercel needs correct build settings. Usually it auto-detects, but if not:
```
> What should be the build settings for my [framework] project on Vercel?
```

---

## Summary

What you learned in this chapter:
- [x] What deployment is
- [x] Deploying static sites with Vercel
- [x] Deploying backends with Railway
- [x] Setting up auto deployment
- [x] Connecting custom domains

You can now share your projects with the world.

The next chapter covers saving and loading data.

Proceed to [Chapter 13: Data Storage](../Chapter13/README.md).
