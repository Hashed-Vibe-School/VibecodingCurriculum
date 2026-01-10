# Chapter 11: Website Development

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Building a website from scratch with Claude
- Completing a portfolio site
- Understanding web development basics

---

## Time for Real Projects

You have learned how to use Claude Code. Now let's build something real.

In this chapter, we will create a **personal portfolio website** from start to finish.

### What We Will Build

- About me page
- Project gallery
- Contact form
- Responsive design (works well on mobile)

---

## Preparation

### 1. Create Project Folder

```
> Create a my-portfolio folder
```

### 2. Set Up CLAUDE.md

```
> Create a CLAUDE.md with:
> - Using HTML, CSS, JavaScript
> - Styling with Tailwind CSS
> - Mobile responsive required
> - Comments in English
```

### 3. Basic File Structure

```
> Create the basic file structure:
> - index.html (main page)
> - style.css (styles)
> - script.js (behavior)
```

---

## Step-by-Step Development

### Step 1: Create the Basic HTML

```
> Create the basic structure in index.html.
> Divide it into header, main, and footer sections.
```

Claude creates a structure like this:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Portfolio</title>
    <link href="style.css" rel="stylesheet">
</head>
<body>
    <header>
        <!-- Navigation -->
    </header>

    <main>
        <!-- Main content -->
    </main>

    <footer>
        <!-- Footer -->
    </footer>

    <script src="script.js"></script>
</body>
</html>
```

### Step 2: About Me Section

```
> Add an about me section.
> - Profile photo placeholder
> - Name
> - One-line intro
> - Short bio paragraph
```

### Step 3: Project Gallery

```
> Add a projects section.
> - 3 projects in card format
> - Each card has thumbnail, title, description
> - Slight lift effect on hover
```

### Step 4: Contact Form

```
> Add a contact section.
> - Name input
> - Email input
> - Message input
> - Send button
```

### Step 5: Styling

```
> Apply Tailwind CSS.
> Clean and modern feel.
> Blue color scheme.
```

### Step 6: Make It Responsive

```
> Make it look good on mobile too.
> Menu should become hamburger on small screens.
```

---

## Improving with Feedback

The first result may not be perfect. Provide feedback to refine it.

### Examples

```
> The header is too big. Make it shorter.

> Don't like the button color. Make it darker blue.

> The text is too small. Make it bigger.

> Add more space between project cards.
```

After 2-3 rounds of feedback, you will get the design you want.

---

## Adding Real Data

Replace the dummy data with your actual content.

```
> Change the profile name to "John Doe".
> Bio should be "A student learning web development".
```

```
> Change project 1 to:
> - Title: "Todo List App"
> - Description: "My first project built with React"
```

---

## Checking the Result

### View in Browser

```
> Open index.html in browser
```

You can also double-click the file to open it directly.

### Using a Dev Server (Recommended)

```
> Start a dev server
```

Claude will run a simple server. Typically accessible at `http://localhost:3000`.

---

## Common Requests

### Layout Related

```
> Change to 2-column layout. Sidebar on left, main content on right.

> Arrange cards in 3 columns.

> Center align it.
```

### Style Related

```
> Add shadow effect.

> Make the corners rounded.

> Add hover effect.
```

### Behavior Related

```
> Make the header stick when scrolling.

> Make scrolling smooth.

> Make images enlarge when clicked.
```

---

## Practice: Complete Your Portfolio

### Basic Tasks

1. Follow the steps above to create the basic portfolio structure
2. Fill in with your own information
3. Modify colors and design to your liking

### Extra Challenges

```
> Add a dark mode toggle button.

> Show tags (HTML, CSS, JS) on project cards.

> Add a skills section. Show proficiency with progress bars.
```

---

## Troubleshooting

### When Styles Do Not Apply

```
> CSS isn't being applied. What's wrong?
```

Claude will check and fix it.

### When Layout Breaks

```
> Layout is broken on mobile. Fix it.
```

### When JavaScript Does Not Work

```
> Button click does nothing. Why?
```

---

## Post-Completion Checklist

- [ ] Do all links work?
- [ ] Does it look good on mobile?
- [ ] Do all images show?
- [ ] Does form input work?
- [ ] Any typos?

---

## Mini Project: Personal Blog

After completing the portfolio, try building a blog.

### Goals

- Build a multi-page site
- Understand content management

### Create the Blog

```
> Create a personal blog.
> - Home: latest posts list
> - Post page: post content
> - About page
> - Category sorting
```

### Add Features

```
> Add search functionality

> Add a tag system

> Support dark mode
```

### Advanced Challenges (For Experts)

```
> Make it possible to write posts in Markdown

> Build it like a static site generator

> Add SEO optimization (meta tags, Open Graph)
```

---

## Summary

What you learned in this chapter:
- [x] Creating project structure
- [x] Building pages with HTML/CSS
- [x] Improving design with feedback
- [x] Making responsive websites

The next chapter covers deploying this website to make it accessible to anyone on the internet.

Proceed to [Chapter 12: Deployment](../Chapter12/README.md).
