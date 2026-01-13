# Chapter 12: Deployment

**English** | [한국어](./README.ko.md)

## What You Will Learn

- What deployment is
- Free deployment with Vercel
- Backend deployment with Railway

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
