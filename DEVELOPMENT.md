# Development Guide — arehman-dev Portfolio

**Complete documentation for developers and content creators**

---

## 📋 Table of Contents

1. [Project Architecture](#project-architecture)
2. [Code Logic & Workflows](#code-logic--workflows)
3. [Component System](#component-system)
4. [Styling & Design](#styling--design)
5. [Adding Content](#adding-content)
6. [Deployment](#deployment)

---

## 🏗️ Project Architecture

### Tech Stack
- **Generator**: Vanilla HTML5 (no framework)
- **Hosting**: GitHub Pages
- **CSS**: Custom (no frameworks), Glassmorphism design
- **JavaScript**: Vanilla (no dependencies)
- **Fonts**: Google Fonts (Inter, JetBrains Mono, Poppins)
- **Icons**: Font Awesome 6.0 (CDN)
- **Animation**: CSS transitions + vanilla JS

### Directory Structure

```
portfolio/
├── index.html               # Single-page portfolio (main entry)
├── js/
│   └── main.js             # Theme toggle, carousel logic
├── css/
│   ├── variables.css       # CSS custom properties (colors, spacing, typography)
│   └── styles.css          # All styling (~1200+ lines)
├── assets/
│   ├── css/                # Additional styles (if needed)
│   ├── images/             # Project/portfolio images
│   │   └── favicon/
│   └── documents/
│       └── resume-abdulrehman.pdf
├── COLOR_PALETTE.md        # Color system documentation
├── README.md               # Quick start & features
├── DEVELOPMENT.md          # This file
└── DEVELOPMENT_CHECKLIST.md # Component tracking
```

---

## 🔄 Code Logic & Workflows

### 1. Theme System (Light/Dark Mode)

**Logic Flow:**
```
User visits page
  ↓
JavaScript checks localStorage for "theme" key
  ↓
If not found: Check OS preference via window.matchMedia('(prefers-color-scheme: dark)')
  ↓
Add data-theme="dark" to <html> root element
  ↓
CSS variables in [data-theme="dark"] override root values
  ↓
When user clicks theme toggle (🌙 button):
   → Toggle data-theme attribute on <html>
   → Save preference to localStorage
   → All colors update instantly via CSS variables
```

**CSS Variables System:**

**Light Mode (:root)**
```css
:root {
    /* Colors - Green Suite */
    --green-lightest: #f0f9f4;
    --green-light: #8ed5ab;
    --green-medium: #01ac6a;
    --green-dark: #00833f;
    --green-darker: #1c5c39;

    /* Colors - Blue Suite */
    --blue-lightest: #f0f6ff;
    --blue-light: #a3c4ef;
    --blue-medium: #397bc9;
    --blue-dark: #1b25ab;
    --blue-darker: #071e61;

    /* Semantic Variables */
    --accent-color: var(--green-medium);
    --accent-color2: var(--blue-medium);
    --text-color: #4a5568;
    --text-secondary: #718096;
    --text-muted: #a0aec0;
    --bg-color: #fdfdfd;
    --bg-secondary: #f7fafc;
    --border-color: rgba(1, 172, 106, 0.1);

    /* Gradients */
    --gradient-primary: linear-gradient(135deg, #8ed5ab 0%, #01ac6a 100%);
    --gradient-secondary: linear-gradient(135deg, #a3c4ef 0%, #397bc9 100%);
    --gradient-accent: linear-gradient(135deg, #01ac6a 0%, #397bc9 100%);
}
```

**Dark Mode ([data-theme="dark"])**
```css
[data-theme="dark"] {
    --accent-color: var(--green-light);
    --accent-color2: var(--blue-light);
    --text-color: #f7fafc;
    --text-secondary: #cbd5e0;
    --text-muted: #a0aec0;
    --bg-color: #0a0a0a;
    --bg-secondary: #111111;
    --border-color: #2d3748;
}
```

**Implementation in HTML:**
```javascript
// js/main.js - Theme Toggle
function toggleTheme() {
  const htmlEl = document.documentElement;
  const currentTheme = htmlEl.getAttribute('data-theme');
  const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
  
  htmlEl.setAttribute('data-theme', newTheme);
  localStorage.setItem('theme', newTheme);
  updateThemeIcon(newTheme);
}

// Initialize theme on page load
const savedTheme = localStorage.getItem('theme');
const systemTheme = window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
const theme = savedTheme || systemTheme;
document.documentElement.setAttribute('data-theme', theme);
```

**All UI components use semantic variables → Theme switching = instant, no re-render**

---

### 2. Navigation & Layout

**Fixed Top Navigation:**
```css
nav {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    background: rgba(253, 253, 253, 0.95);
    z-index: 1000;
    padding: var(--spacing-md);
}

nav .nav-brand {
    font-family: var(--font-mono);
    font-weight: 700;
    color: var(--accent-color);
}

nav .nav-links {
    display: flex;
    gap: var(--spacing-md);
    align-items: center;
}
```

**Responsive Design:**
```css
@media (max-width: 768px) {
    nav .nav-links {
        flex-direction: column;
        gap: var(--spacing-sm);
    }
}
```

---

### 3. Projects Carousel

**Logic Flow:**
```
User clicks next/prev button
  ↓
Update currentProjectIndex
  ↓
Calculate transform translateX value
  ↓
Apply CSS transition to projectsContainer
  ↓
Slide animation (~0.5s cubic-bezier)
  ↓
Auto-repeat after 5 seconds
```

**HTML Structure:**
```html
<div class="projects-container">
  <div class="projects-slider">
    <div class="project-card">
      <h3>Project Name</h3>
      <!-- Content -->
    </div>
    <!-- More projects -->
  </div>
  <button class="carousel-nav prev">❮</button>
  <button class="carousel-nav next">❯</button>
</div>
```

**CSS:**
```css
.projects-slider {
    display: flex;
    gap: var(--spacing-2xl);
    transition: transform 0.5s cubic-bezier(0.16, 1, 0.3, 1);
}

.project-card {
    min-width: 350px;
    flex-shrink: 0;
    background: var(--bg-secondary);
    border-radius: 16px;
    padding: var(--spacing-lg);
    border: 1px solid var(--border-color);
    backdrop-filter: blur(10px);
}
```

**JavaScript:**
```javascript
let currentProjectIndex = 0;
const projects = document.querySelectorAll('.project-card');
const totalProjects = projects.length;

function showProject(index) {
  const offset = -index * (350 + 24); // card-width + gap
  document.querySelector('.projects-slider').style.transform = 
    `translateX(${offset}px)`;
}

function nextProject() {
  currentProjectIndex = (currentProjectIndex + 1) % totalProjects;
  showProject(currentProjectIndex);
}

function prevProject() {
  currentProjectIndex = (currentProjectIndex - 1 + totalProjects) % totalProjects;
  showProject(currentProjectIndex);
}

// Auto-advance every 5 seconds
setInterval(nextProject, 5000);
```

---

### 4. Button Animation System

**Hire Me / Contact Button (.btn-outline):**
```css
.btn-outline {
    background: var(--green-lightest);
    color: var(--accent-color);
    border: 2px solid var(--accent-color);
    padding: var(--spacing-md) var(--spacing-xl);
    border-radius: 10px;
    transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}

.btn-outline:hover {
    background: var(--accent-color);
    color: white;
    transform: translateY(-2px);
    box-shadow: 0 8px 16px -2px rgba(1, 172, 106, 0.4);
}
```

**Resume / Navigation Button (.btn-nav-outline):**
```css
.btn-nav-outline {
    background: var(--green-lightest);
    color: var(--accent-color);
    border: 2px solid var(--accent-color);
    transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}

.btn-nav-outline:hover {
    background: var(--accent-color);
    color: white;
    border-color: var(--accent-color);
    transform: translateY(-2px);
    box-shadow: 0 8px 16px -2px rgba(1, 172, 106, 0.4);
}

/* Dark Theme */
[data-theme="dark"] .btn-nav-outline {
    background: rgba(142, 213, 171, 0.1);
    color: var(--green-light);
    border-color: var(--green-light);
}

[data-theme="dark"] .btn-nav-outline:hover {
    background: var(--green-light);
    color: var(--green-darker);
    border-color: var(--green-light);
    box-shadow: 0 8px 16px -2px rgba(142, 213, 171, 0.4);
}
```

**Animation Details:**
- Button transforms on hover: `translateY(-2px)` = slight lift
- Color shift: transparent background → solid accent color
- Shadow boost: `0 8px 16px -2px` for depth
- Timing: `cubic-bezier(0.16, 1, 0.3, 1)` = snappy, responsive feel

---

### 5. Social Icons Hover Effect

**Static State:**
```css
.social-bar a {
    width: 40px;
    height: 40px;
    background: var(--bg-color);
    border: 1px solid var(--border-color);
    border-radius: 50%;
    transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}
```

**Hover State:**
```css
.social-bar a:hover {
    transform: scale(1.15);
    background: transparent;
    color: white;
    border: 2px solid var(--green-medium);
    box-shadow: var(--shadow-md);
}
```

**Effect:** Icons scale up + green border on hover (no gradient fill to avoid render issues)

---

## 🎨 Component System

### 1. Cards (Project, Skill, Experience)

**Base Card Styles:**
```css
.card {
    background: rgba(255, 255, 255, 0.7);
    border: 1px solid var(--border-color);
    border-radius: 16px;
    padding: var(--spacing-lg);
    backdrop-filter: blur(10px);
    transition: all 0.4s ease;
}

.card:hover {
    border-color: var(--accent-color);
    box-shadow: 0 10px 30px rgba(1, 172, 106, 0.15);
    transform: translateY(-8px);
}

/* Top accent line on hover */
.card::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 3px;
    background: var(--gradient-accent);
    border-radius: 16px 16px 0 0;
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.card:hover::before {
    transform: scaleX(1);
}
```

**Skill Card Variant:**
```css
.skill-item {
    display: flex;
    align-items: center;
    gap: var(--spacing-md);
    padding: var(--spacing-md);
    background: var(--bg-secondary);
    border-radius: 12px;
    border: 1px solid var(--border-color);
}

.skill-badge {
    background: var(--gradient-primary);
    color: white;
    padding: var(--spacing-xs) var(--spacing-sm);
    border-radius: 8px;
    font-size: var(--font-size-sm);
    font-weight: 500;
}
```

### 2. Typography

**Headers - using Poppins font:**
```css
h1 {
    font-family: var(--font-poppins);
    font-size: var(--font-size-4xl);
    font-weight: 700;
    background: var(--gradient-accent);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
}

h2 {
    font-family: var(--font-poppins);
    font-size: var(--font-size-3xl);
    font-weight: 600;
    color: var(--accent-color);
}

h3, h4 {
    font-family: var(--font-poppins);
    font-weight: 500;
}
```

**Body Text:**
```css
p {
    font-family: var(--font-primary);
    font-size: var(--font-size-base);
    line-height: 1.6;
    color: var(--text-secondary);
}
```

### 3. Featured Projects Toggle

**Toggle Button Styling:**
```css
.featured-projects-toggle {
    background: var(--blue-lightest);
    color: var(--blue-medium);
    border: 2px solid var(--blue-medium);
    padding: var(--spacing-sm) var(--spacing-md);
    border-radius: 8px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}

.featured-projects-toggle:hover {
    background: var(--blue-medium);
    color: white;
    transform: translateY(-2px);
    box-shadow: 0 8px 16px -2px rgba(57, 123, 201, 0.4);
}

/* Dark Theme */
[data-theme="dark"] .featured-projects-toggle {
    background: rgba(57, 123, 201, 0.1);
    color: var(--blue-light);
    border-color: var(--blue-light);
}
```

---

## 🎨 Styling & Design

### Color System

See **[COLOR_PALETTE.md](COLOR_PALETTE.md)** for complete details.

**Quick Reference:**
- **Primary (Green):** #01ac6a → used for accent, buttons, text emphasis
- **Secondary (Blue):** #397bc9 → used for secondary actions, links
- **White/Light:** #fdfdfd (light), #f7fafc (bg-secondary)
- **Dark:** #0a0a0a (dark mode bg)

### Layout Grid

```css
.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 var(--spacing-xl);
}

/* Spacing System */
--spacing-xs: 0.25rem;     /* 4px */
--spacing-sm: 0.5rem;      /* 8px */
--spacing-md: 1rem;        /* 16px */
--spacing-lg: 1.5rem;      /* 24px */
--spacing-xl: 2rem;        /* 32px */
--spacing-2xl: 3rem;       /* 48px */
```

### Typography Scale

```css
--font-size-xs: 0.75rem;       /* 12px */
--font-size-sm: 0.825rem;      /* 13px */
--font-size-base: 0.9rem;      /* 14.4px */
--font-size-lg: 1rem;          /* 16px */
--font-size-xl: 1.125rem;      /* 18px */
--font-size-2xl: 1.5rem;       /* 24px */
--font-size-3xl: 2rem;         /* 32px */
--font-size-4xl: 2.5rem;       /* 40px */
```

---

## 📝 Adding Content

### 1. Add Project to Carousel

**Location:** `index.html` - Find `<section id="projects">`

**New Project Template:**
```html
<div class="project-card">
  <div class="project-header">
    <h3>Project Name</h3>
    <div class="project-tags">
      <span class="badge tech-frontend">JavaScript</span>
      <span class="badge tech-design">UI/UX</span>
    </div>
  </div>
  <p class="project-description">
    Brief description of the project and what you learned.
  </p>
  <a href="https://github.com/link" target="_blank" class="btn-primary">
    <i class="fas fa-external-link-alt"></i> View Project
  </a>
</div>
```

**Steps:**
1. Open `index.html`
2. Find `<div class="projects-slider">` section
3. Add new `.project-card` div before closing `</div>`
4. Update carousel logic if adding new badges

---

### 2. Add Skill

**Location:** `index.html` - Find `<section id="skills">`

**New Skill Template:**
```html
<div class="skill-item">
  <span class="skill-name">Skill Name</span>
  <div class="skill-badges">
    <span class="skill-badge skill-proficiency">Proficiency: Advanced</span>
    <span class="skill-badge skill-category">Category: Backend</span>
  </div>
  <span class="skill-level">📊 85%</span>
</div>
```

**Badge Classes:**
- `.tech-frontend` → Blue gradient
- `.tech-backend` → Green gradient
- `.tech-design` → Purple/pink
- `.tech-mobile` → Orange
- `.skill-proficiency` → Green tone
- `.skill-category` → Blue tone

---

### 3. Update Contact Information

**Location:** `index.html` - Find `<section id="contact">`

**Fields to Update:**
- Email (mailto link)
- Form endpoint (if using form service)
- Social links in `.social-bar` section

---

### 4. Update Footer

**Location:** `index.html` - Find `<footer class="footer">`

```html
<footer class="footer">
  <div class="container">
    <p class="footer-text">
      © 2026 Abdul Rehman • <span class="text-accent">Building robust systems & meaningful solutions</span> •
      <a href="https://arehman-dev.github.io" target="_blank" rel="noopener noreferrer" class="footer-link">
        <em>Personal Site</em>
      </a>
    </p>
  </div>
</footer>
```

**To Update:**
- Change year in © symbol
- Update tagline text
- Update personal site link

---

### 5. Update Resume Link

**Location:** Navigation bar

```html
<a href="assets/documents/YOUR_RESUME.pdf" class="btn-nav-outline" target="_blank">
  <i class="fas fa-download"></i> Resume
</a>
```

**File:** Place resume PDF in `assets/documents/` folder with descriptive name

---

## 🚀 Deployment

### Local Development
```bash
# No build step required - open in browser
# Or use a simple HTTP server:
python -m http.server 8000
# Visit http://localhost:8000
```

### GitHub Pages Push

```bash
# From portfolio directory
git add -A
git commit -m "Update: add new projects/skills"
git push origin main
```

**GitHub Pages automatically:**
- Deploys HTML/CSS/JS directly
- Takes ~30-60 seconds to live
- No build process needed

### Environment Setup

1. **Clone repository:**
```bash
git clone https://github.com/arehman-dev/portfolio.git
cd portfolio
```

2. **Create feature branch:**
```bash
git checkout -b feature/add-new-project
```

3. **Make changes** in your editor

4. **Test locally:**
- Open `index.html` in browser
- Or use `python -m http.server`

5. **Commit & Push:**
```bash
git add -A
git commit -m "feat: add new project"
git push origin feature/add-new-project
```

6. **Create Pull Request** on GitHub

7. **Merge to main** → Auto-deployed!

---

## 🔧 Performance Optimization

### Current Setup
- ✅ No build step (instant feedback)
- ✅ Minimal dependencies (Font Awesome CDN only)
- ✅ CSS custom properties (faster theme switching)
- ✅ Lazy `loading="lazy"` on images
- ✅ Hardware-accelerated transforms (GPU)
- ✅ Optimized animations (cubic-bezier timing)

### Further Optimization
- Minify CSS/JS for production
- Compress images (consider WebP format)
- Add service worker for offline support
- Implement critical CSS inline

---

## 📚 Quick Reference

### Update Theme Colors
Edit `css/variables.css`:
```css
:root {
    --accent-color: var(--green-medium);  /* Change this */
}
```

### Add New Badge Type
Edit `css/styles.css`:
```css
.badge.tech-newtype {
    background: linear-gradient(135deg, #color1, #color2);
}
```

### Modify Carousel Speed
Edit `js/main.js`:
```javascript
setInterval(nextProject, 5000);  // Change 5000ms here
```

### Update Typography
Edit `css/variables.css`:
```css
--font-primary: 'New Font', sans-serif;
--font-size-base: 1rem;  /* Adjust base size */
```

---

**Last Updated:** April 5, 2026  
**Version:** 1.0
