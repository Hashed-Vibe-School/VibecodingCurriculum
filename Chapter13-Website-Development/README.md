# Chapter 13: Website Development

**English** | [한국어](./README.ko.md)

---

## Ask Questions

If you have questions while learning, ask on Discord!

[![Discord](https://img.shields.io/badge/Discord-Ask_Questions-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/your-invite-link)

---

## What You Will Learn

- Building a website from scratch with Claude
- HTML, CSS, JavaScript basics
- Completing a personal portfolio site
- Principles of responsive web design
- Feedback-based iterative development

---

## Connection to Previous Chapters

In Chapter 11, you learned how to manage code with Git, and in Chapter 12, you learned how to set project rules with CLAUDE.md.

Now the theory is done. **It's time to actually build a website!**

In this chapter, we'll use everything we've learned to create a **personal portfolio website** from start to finish.

---

## Why Do You Need This?

Think about it - every company, freelancer, and professional has a website. When someone searches for you online, what will they find?

### Real-World Scenarios Where You Need a Portfolio Website

| Scenario | Without Portfolio | With Portfolio |
|----------|-------------------|----------------|
| **Job hunting** | Just submit a resume | "See my work" with attached link |
| **Freelancing** | "I'm good" in words only | Demonstrate actual work results |
| **Personal branding** | Just one of thousands | Memorable first impression |
| **Networking** | Exchange contact info | "Visit my site" |

> **Beginner Tip**: A portfolio is like a digital business card, but much more powerful. It shows what you CAN DO, not just what you SAY you can do.

### Simple Analogy: The Portfolio as Your Showcase Window

Imagine you're a baker:

```
Option A: A business card that says "I make delicious cakes"
Option B: A shop window filled with beautiful cakes
```

Which one makes people want to buy?

Your portfolio website is that showcase window - it displays your best work for the world to see.

---

## Web Development Basics

Before diving into practice, let's briefly understand how websites are built.

### The Roles of HTML, CSS, JavaScript

Websites are built with three technologies:

```
🏗️ HTML  = Skeleton (structure)
🎨 CSS   = Clothes (style)
⚡ JavaScript = Movement (behavior)
```

**Comparing to building a house:**

| Technology | Role | House Analogy |
|------------|------|---------------|
| **HTML** | Content and structure | Bricks, rooms, doors |
| **CSS** | Design and layout | Paint, furniture, interior |
| **JavaScript** | Interaction and behavior | Electricity, plumbing, switches |

### Actual Code Examples

```html
<!-- HTML: Structure -->
<button>Click me</button>
```

```css
/* CSS: Style */
button {
  background-color: blue;
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
}
```

```javascript
// JavaScript: Behavior
button.addEventListener('click', function() {
  alert('Button was clicked!');
});
```

> **Note**: You don't need to memorize the code! Claude will write it for you. You just need to understand the concepts here.

---

## Try It Yourself: The Simplest Portfolio

Before diving into the full project, let's make sure everything works with a minimal example.

```
> Create an HTML file with just "John Doe" as the heading
> and one sentence about what I do. Nothing else.
```

**Code Claude creates:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>John Doe</title>
</head>
<body>
    <h1>John Doe</h1>
    <p>A college student studying web development.</p>
</body>
</html>
```

**How to run it:**
1. Double-click the file
2. It opens in a browser
3. If you see the name, success!

> **Beginner Tip**: You just made your first webpage! Everything else is just building on this foundation.

---

## Time for Real Projects

In this chapter, we'll create a **personal portfolio website** from start to finish.

### What We Will Build

```
┌─────────────────────────────────────┐
│          Header (Navigation)        │
├─────────────────────────────────────┤
│                                     │
│           About Section             │
│    (Profile photo, name, intro)     │
│                                     │
├─────────────────────────────────────┤
│                                     │
│         Project Gallery             │
│    [Card 1] [Card 2] [Card 3]       │
│                                     │
├─────────────────────────────────────┤
│                                     │
│          Contact Form               │
│    Name / Email / Message / Send    │
│                                     │
├─────────────────────────────────────┤
│              Footer                 │
└─────────────────────────────────────┘
```

**What you'll have when complete:**
- About me page
- Project gallery
- Contact form
- Responsive design (looks good on mobile)

---

## Preparation

### 1. Create Project Folder

```
> Create a my-portfolio folder
```

**Claude's response:**
```
Created the my-portfolio folder.
```

### 2. Set Up CLAUDE.md

As we learned in Chapter 12, first set up the project rules.

```
> Create CLAUDE.md with:
> - Using HTML, CSS, JavaScript
> - Styling with Tailwind CSS (CDN)
> - Mobile responsive required
> - Comments in English
> - Lowercase file names
```

**Actual conversation example:**

```
User: Create CLAUDE.md. It's an HTML, CSS, JS project.
      Using Tailwind CSS, mobile responsive required.

Claude: Created CLAUDE.md:

# Portfolio Project

## Tech Stack
- HTML5
- CSS3 (Tailwind CSS CDN)
- JavaScript (Vanilla)

## Rules
- Mobile-first responsive design
- Use semantic HTML tags
- Write English comments
- Use lowercase and hyphens for file names

## Forbidden
- No inline styles (use Tailwind classes)
- No jQuery
```

### 3. Basic File Structure

```
> Create the basic file structure:
> - index.html (main page)
> - style.css (custom styles)
> - script.js (behavior)
> - images/ folder (for images)
```

**Created structure:**
```
my-portfolio/
├── CLAUDE.md
├── index.html
├── style.css
├── script.js
└── images/
```

> **Pro tip**: Setting up the structure from the start makes finding files much easier later.

---

## Step-by-Step Development

### Step 1: Create the Basic HTML

```
> Create the basic structure in index.html.
> Divide it into header, main, and footer sections.
> Connect Tailwind CSS CDN.
```

**Code Claude creates:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Portfolio</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="style.css" rel="stylesheet">
</head>
<body class="bg-gray-50">
    <!-- Header: Navigation area -->
    <header class="bg-white shadow">
        <!-- Navigation content -->
    </header>

    <!-- Main: Primary content area -->
    <main>
        <!-- About section -->
        <section id="about">
        </section>

        <!-- Projects section -->
        <section id="projects">
        </section>

        <!-- Contact section -->
        <section id="contact">
        </section>
    </main>

    <!-- Footer: Bottom information -->
    <footer class="bg-gray-800 text-white">
        <!-- Footer content -->
    </footer>

    <script src="script.js"></script>
</body>
</html>
```

> **Beginner Tip**: Tags like `<header>`, `<main>`, `<footer>` are called "semantic tags." They have meaning, so search engines and screen readers can better understand the page.

### Step 2: About Section

```
> Add an about section.
> - Profile photo placeholder (circular)
> - Name (large)
> - One-line intro (small)
> - Bio paragraph (2-3 lines)
> - Center aligned
```

**Actual conversation example:**

```
User: Create an about section. Include profile photo, name, intro.

Claude: Added the about section:

<section id="about" class="py-20 px-4">
    <div class="max-w-4xl mx-auto text-center">
        <!-- Profile photo -->
        <div class="mb-6">
            <img
                src="images/profile.jpg"
                alt="Profile photo"
                class="w-32 h-32 rounded-full mx-auto object-cover border-4 border-blue-500"
            >
        </div>

        <!-- Name -->
        <h1 class="text-4xl font-bold text-gray-800 mb-2">
            John Doe
        </h1>

        <!-- One-line intro -->
        <p class="text-xl text-blue-600 mb-6">
            Web Developer | Problem Solver | Lifelong Learner
        </p>

        <!-- Bio paragraph -->
        <p class="text-gray-600 max-w-2xl mx-auto leading-relaxed">
            Hello! I'm a developer who loves building web applications
            that improve user experience. I'm passionate about learning
            new technologies and solving real problems.
        </p>
    </div>
</section>

Put your profile photo in images/profile.jpg.
If you don't have a photo yet, would you like me to use a placeholder image?
```

### Step 3: Project Gallery

```
> Add a projects section.
> - 3 projects in card format
> - Each card has thumbnail, title, description, tags
> - Slight lift effect on hover
> - 3-column grid (1 column on mobile)
```

**Code Claude creates:**

```html
<section id="projects" class="py-20 px-4 bg-white">
    <div class="max-w-6xl mx-auto">
        <!-- Section title -->
        <h2 class="text-3xl font-bold text-center text-gray-800 mb-12">
            Projects
        </h2>

        <!-- Project card grid -->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">

            <!-- Project card 1 -->
            <div class="bg-gray-50 rounded-lg overflow-hidden shadow-md
                        hover:shadow-xl hover:-translate-y-2
                        transition-all duration-300">
                <!-- Thumbnail -->
                <img
                    src="images/project1.jpg"
                    alt="Project 1"
                    class="w-full h-48 object-cover"
                >

                <!-- Card content -->
                <div class="p-6">
                    <h3 class="text-xl font-semibold mb-2">
                        Todo List App
                    </h3>
                    <p class="text-gray-600 mb-4">
                        A task management application built with React.
                        Saves data using local storage.
                    </p>

                    <!-- Tags -->
                    <div class="flex flex-wrap gap-2">
                        <span class="px-3 py-1 bg-blue-100 text-blue-600
                                     text-sm rounded-full">
                            React
                        </span>
                        <span class="px-3 py-1 bg-green-100 text-green-600
                                     text-sm rounded-full">
                            JavaScript
                        </span>
                    </div>
                </div>
            </div>

            <!-- Project cards 2, 3 have similar structure... -->

        </div>
    </div>
</section>
```

> **Pro tip**: `hover:shadow-xl hover:-translate-y-2` are Tailwind CSS hover effects. On mouse hover, the shadow gets bigger and the card lifts slightly.

### Step 4: Contact Form

```
> Add a contact section.
> - Name input (required)
> - Email input (required, email format)
> - Message input (text area)
> - Send button
> - Form validation not needed for now
```

**Code Claude creates:**

```html
<section id="contact" class="py-20 px-4 bg-gray-100">
    <div class="max-w-2xl mx-auto">
        <h2 class="text-3xl font-bold text-center text-gray-800 mb-12">
            Contact
        </h2>

        <form class="bg-white rounded-lg shadow-md p-8">
            <!-- Name input -->
            <div class="mb-6">
                <label
                    for="name"
                    class="block text-gray-700 font-medium mb-2"
                >
                    Name
                </label>
                <input
                    type="text"
                    id="name"
                    name="name"
                    required
                    class="w-full px-4 py-2 border border-gray-300
                           rounded-lg focus:ring-2 focus:ring-blue-500
                           focus:border-transparent outline-none"
                    placeholder="John Doe"
                >
            </div>

            <!-- Email input -->
            <div class="mb-6">
                <label
                    for="email"
                    class="block text-gray-700 font-medium mb-2"
                >
                    Email
                </label>
                <input
                    type="email"
                    id="email"
                    name="email"
                    required
                    class="w-full px-4 py-2 border border-gray-300
                           rounded-lg focus:ring-2 focus:ring-blue-500
                           focus:border-transparent outline-none"
                    placeholder="example@email.com"
                >
            </div>

            <!-- Message input -->
            <div class="mb-6">
                <label
                    for="message"
                    class="block text-gray-700 font-medium mb-2"
                >
                    Message
                </label>
                <textarea
                    id="message"
                    name="message"
                    rows="5"
                    required
                    class="w-full px-4 py-2 border border-gray-300
                           rounded-lg focus:ring-2 focus:ring-blue-500
                           focus:border-transparent outline-none resize-none"
                    placeholder="Enter your message..."
                ></textarea>
            </div>

            <!-- Send button -->
            <button
                type="submit"
                class="w-full bg-blue-600 text-white py-3 rounded-lg
                       font-medium hover:bg-blue-700
                       transition-colors duration-200"
            >
                Send Message
            </button>
        </form>
    </div>
</section>
```

### Step 5: Complete the Navigation

```
> Add navigation to the header.
> - Logo on the left (text "JD")
> - Menu on the right (About, Projects, Contact)
> - Clicking scrolls to that section
> - Hamburger menu on mobile
```

### Step 6: Make It Responsive

```
> Check that it looks good on mobile.
> Specifically:
> - Does navigation become hamburger?
> - Do project cards become 1 column?
> - Is text size appropriate?
```

**Actual conversation example:**

```
User: Check the mobile layout

Claude: Let me check the current responsive settings.

✅ What's working well:
- Project grid: lg:grid-cols-3 → md:grid-cols-2 → grid-cols-1
- Padding and margins are set responsively
- Images maintain aspect ratio with object-cover

⚠️ What needs improvement:
- Navigation doesn't have mobile hamburger menu
- Title size is a bit large on mobile

Should I fix these?

User: Yes, fix them

Claude: Applied mobile optimization:

1. Added hamburger menu (only shows on mobile)
2. Adjusted title size: text-4xl → text-2xl md:text-4xl
3. Adjusted section padding: py-20 → py-12 md:py-20

Check with browser developer tools (F12) in mobile view.
```

---

## Improving with Feedback

It's okay if the first result isn't perfect. Give feedback!

### Layout Feedback

```
> The header is too big. Make it shorter.
> Add more space between project cards.
> The contact form is too narrow. Make it wider.
```

### Color/Style Feedback

```
> Don't like the button color. Make it darker blue.
> The background is too plain. Add a gradient.
> Text is too small. Make the body text bigger.
```

### Effect Feedback

```
> Add fade-in effect on page load.
> Elements should appear sequentially on scroll.
> Add click effect to buttons.
```

**Actual conversation example (feedback iteration):**

```
User: The header is too plain

Claude: What style would you like?
1. Gradient background
2. Transparent background + white on scroll
3. Dark color background

User: Option 2

Claude: Changed to header that shows background on scroll.

[JavaScript added]
window.addEventListener('scroll', function() {
    const header = document.querySelector('header');
    if (window.scrollY > 50) {
        header.classList.add('bg-white', 'shadow');
    } else {
        header.classList.remove('bg-white', 'shadow');
    }
});

Try scrolling to check!

User: Nice! But add a transition effect. It changes too abruptly.

Claude: Added transition.

header {
    transition: all 0.3s ease;
}

Now it changes smoothly.
```

> **Beginner Tip**: After 2-3 rounds of feedback, you'll get the design you want. You don't need it to be perfect from the start!

---

## Adding Real Data

Replace the dummy data with your actual content.

### Modifying Profile Information

```
> Change the profile name to "Jane Smith".
> Bio should be "A college student studying frontend development".
> One-line intro should be "React Lover | UI/UX Enthusiast".
```

### Modifying Project Information

```
> Change project 1 to:
> - Title: "Movie Search App"
> - Description: "A movie information search service using the TMDB API"
> - Tags: React, API, Tailwind
```

### How to Add Images

**Option 1: Use placeholder images**
```
> Replace with placeholder if I don't have a profile photo
```

**Option 2: Use actual images**
```
Put files in the images/ folder:
- profile.jpg (profile photo)
- project1.jpg (project 1 thumbnail)
- project2.jpg (project 2 thumbnail)
```

> **Note**: Be careful with image file name case sensitivity! `Profile.jpg` and `profile.jpg` are different.

---

## Checking the Result

### View in Browser

```
> Open index.html in browser
```

Or double-click the file to open it directly.

### Using a Dev Server (Recommended)

```
> Start a dev server
```

Claude will run a simple server:

```
User: Start a dev server

Claude: I'll run a simple Python server.

[Running: python -m http.server 3000]

Server started!
Open http://localhost:3000 in your browser.

Press Ctrl+C to stop.
```

> **Pro tip**: Using a dev server means you just need to refresh whenever you modify files. More convenient than double-clicking.

---

## Common Requests

### Layout Related

```
> Change to 2-column layout. Sidebar on left, main content on right.
> Arrange cards in 3 columns.
> Center align it.
> Add more/less space between elements.
```

### Style Related

```
> Add shadow effect.
> Make the corners rounded.
> Add hover effect.
> Unify color palette to blue scheme.
> Change font to Pretendard.
```

### Behavior Related

```
> Make the header stick when scrolling.
> Make scrolling smooth.
> Make images enlarge when clicked.
> Add a back-to-top button.
> Add a dark mode toggle.
```

---

## Mini Quiz

### Quiz 1
Q: What are the roles of HTML, CSS, and JavaScript?

<details>
<summary>Show Answer</summary>

- **HTML**: Structure and content (skeleton)
- **CSS**: Design and style (clothes)
- **JavaScript**: Interaction and behavior (movement)
</details>

### Quiz 2
Q: What does `hover:shadow-xl` mean in Tailwind CSS?

<details>
<summary>Show Answer</summary>

A large shadow (shadow-xl) is applied on mouse hover.
</details>

### Quiz 3
Q: What does `grid-cols-1 md:grid-cols-3` mean?

<details>
<summary>Show Answer</summary>

By default (mobile) it shows 1 column, and on medium screens (md) and above it shows 3 columns. This is responsive design.
</details>

---

## Practice Exercises

### Difficulty 1: Basic Portfolio (Beginner)

Follow the steps above to create the basic structure:

1. Create project folder
2. Set up CLAUDE.md
3. Basic HTML structure
4. About section
5. 3 project cards
6. Contact form

**Completion checklist:**
- [ ] All sections exist
- [ ] Basic styles applied
- [ ] Viewable in browser

### Difficulty 2: Style Customization (Intermediate)

Add personality to the basic portfolio:

```
> Change color theme to mint green
> Add hover effects to all buttons
> Add gradient borders to cards
> Change font to Noto Sans
```

**Completion checklist:**
- [ ] Unique color theme
- [ ] Animation effects
- [ ] Custom font applied

### Difficulty 3: Feature Addition (Advanced)

```
> Add a dark mode toggle button
> Add project card filtering (by tags)
> Add scroll progress bar
> Display intro text with typing animation
```

**Completion checklist:**
- [ ] Dark/light mode toggle
- [ ] Project filtering works
- [ ] Advanced animations

---

## Challenge Exercises

### Challenge 1: Multi-page
```
> Create a separate about.html page.
> Include more detailed experience and skills list.
> Link it from the navigation.
```

### Challenge 2: Animation Master
```
> Make elements appear sequentially on scroll.
> Use Intersection Observer.
```

### Challenge 3: Add Blog
```
> Add a blog section.
> - Post list page
> - Individual post pages
> - Markdown format support
```

---

## Troubleshooting

### Nothing Shows When I Open the HTML File
- Make sure the file is saved with `.html` extension (not `.txt`)
- Try right-click > "Open with" > select your browser
- Ask Claude to check for errors in the code

```
> Check if there are any syntax errors in index.html
```

### Page Looks Plain / No Styles

```
> Check if Tailwind CSS CDN is properly connected
```

Common causes:
- CDN script not inside `<head>`
- Need internet connection (CDN requires online)
- Typo in class names

### Images Don't Appear

```
> Check image paths
```

Things to check:
- Image file exists in project folder
- File name case is correct (`Photo.jpg` vs `photo.jpg`)
- Path is correct (`images/photo.jpg` vs `./images/photo.jpg`)

### Changes Don't Show Up

1. Hard refresh browser: `Ctrl+Shift+R` (Windows) or `Cmd+Shift+R` (Mac)
2. Make sure you saved the file
3. Make sure you're editing the right file

### Layout Looks Wrong on Mobile

```
> Check the mobile responsive classes
```

Common problems:
- Missing `viewport` meta tag
- Responsive classes (md:, lg:) not applied
- Using fixed width (use % or rem instead of px)

---

## Common Mistakes

### Mistake 1: Starting Too Complex

```
# ❌ Bad approach
> Create a portfolio with animations, 3D effects, video backgrounds,
> particle effects, and a chatbot.

# ✅ Good approach
> Create a simple portfolio with about me, projects, and contact sections.
```

**Why is this a problem?**
- Advanced features won't work if basics aren't done
- Debugging becomes difficult
- You'll give up without finishing

> **Beginner Tip**: Start simple, add features later. A "completed simple thing" is better than an "incomplete complex thing."

### Mistake 2: Not Testing on Mobile

```
# ❌ Bad approach
(Only checking on big monitor)

# ✅ Good approach
> Check how this looks on mobile
```

**Why is this a problem?**
Most people view websites on their phones. If it breaks on mobile, more than half your visitors will leave.

### Mistake 3: Forgetting to Replace Placeholder Content

Claude uses dummy text like "Lorem ipsum" and example images.

```
# Pre-deployment checklist
- [ ] Has all "Lorem ipsum" text been replaced with real content?
- [ ] Have placeholder images been replaced with real images?
- [ ] Has example contact info been replaced with real info?
```

### Mistake 4: Too Many Fonts and Colors

```
# ❌ Bad example
5 different fonts, 10 colors

# ✅ Good example
1-2 fonts, 2-3 main colors
```

**Why is this a problem?**
- Looks messy and unprofessional
- Inconsistent design
- Hard for visitors to focus

### Mistake 5: Links That Go Nowhere

```
> Check all links in this project and tell me which ones are broken
```

Every link should lead somewhere:
- `#` links go to page top (confirm this is intended)
- External links actually work
- Internal anchor IDs exist

---

## Post-Completion Checklist

Verify everything before deployment:

**Content:**
- [ ] All dummy text replaced with real content?
- [ ] No typos?
- [ ] All images showing?

**Functionality:**
- [ ] All links working?
- [ ] Form input working?
- [ ] Navigation working?

**Responsive:**
- [ ] Looks good on mobile?
- [ ] Looks good on tablet?
- [ ] Doesn't break on large screens?

**Performance:**
- [ ] Images optimized? (not too large)
- [ ] Page load speed acceptable?

---

## Mini Project: Personal Blog

Now that you've made a portfolio, try building a blog.

### Goals
- Build a multi-page site
- Understand content management

### Create the Blog

```
> Create a personal blog.
> - Home: latest posts list (card format)
> - Post page: individual post content
> - About page
> - Category sorting
```

### Add Features

```
> Add search functionality
> Add a tag system
> Support dark mode
> Show estimated reading time
```

### Advanced Challenges (For Experts)

```
> Make it possible to write posts in Markdown
> Build it like a static site generator
> Add SEO optimization (meta tags, Open Graph)
```

---

## Glossary

| Term | Description |
|------|-------------|
| **HTML** | Markup language that defines webpage structure |
| **CSS** | Stylesheet language that defines webpage styles |
| **JavaScript** | Programming language that adds dynamic features to webpages |
| **Tailwind CSS** | Utility-based CSS framework |
| **Responsive design** | Design where layout adjusts based on screen size |
| **Semantic tags** | HTML tags with meaning (header, main, footer, etc.) |
| **CDN** | Content Delivery Network, loads external libraries quickly |
| **Hover effect** | Visual effect that appears on mouse hover |
| **Viewport** | Visible area in the browser |

---

## Summary

What you learned in this chapter:
- [x] Web development basics (HTML, CSS, JavaScript)
- [x] Creating project structure
- [x] Step-by-step development with Claude
- [x] Improving design with feedback
- [x] Making responsive websites

> **Key point**: Don't expect a perfect first result. Giving feedback, making revisions, and iterating is the real development process. With Claude, this process is much faster.

---

## Next Chapter Preview

In Chapter 14, you'll learn about **deployment**.

The website you made now only exists on your computer. Others can't see it. In the next chapter:

- Free deployment with Vercel
- Get your own URL (yourname.vercel.app)
- Set up automatic deployment
- Connect custom domains

It's finally time to share your website with the world!

Proceed to [Chapter 14: Deployment](../Chapter14-Deployment/README.md).

---

## Learn More

### Recommended Resources

**Official Documentation:**
- [MDN Web Docs](https://developer.mozilla.org/en-US/) - Complete web development guide
- [Tailwind CSS Official Docs](https://tailwindcss.com/docs) - Complete Tailwind guide
- [HTML Living Standard](https://html.spec.whatwg.org/) - HTML standard documentation

**Video Resources:**
- [HTML/CSS Tutorial (YouTube)](https://www.youtube.com/results?search_query=html+css+tutorial+beginners) - HTML/CSS basics course
- [Tailwind CSS Tutorial (YouTube)](https://www.youtube.com/results?search_query=tailwind+css+tutorial) - Tailwind usage
- [Responsive Web Design (YouTube)](https://www.youtube.com/results?search_query=responsive+web+design+tutorial) - Responsive design techniques

**Reading:**
- [Web Accessibility Guide](https://www.w3.org/WAI/fundamentals/accessibility-intro/) - Web accessibility basics
- [CSS Tricks](https://css-tricks.com/) - CSS tips and techniques

**Related Tools:**
- [Can I Use](https://caniuse.com/) - Browser compatibility checker
- [Coolors](https://coolors.co/) - Color palette generator
- [Google Fonts](https://fonts.google.com/) - Free web fonts
- [Font Awesome](https://fontawesome.com/) - Icon library

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://hashed.com">Hashed</a></sub>
</p>
