# Color Palette — arehman-dev Portfolio

Professional color system designed for system administration & development portfolio.

---

## 🎨 Brand Colors (Primary)

### Green Suite (System Administration)
Primary accent color representing infrastructure, stability, and growth.

| Shade | Hex | RGB | Usage |
|-------|-----|-----|-------|
| Lightest | `#f0f9f4` | rgb(240, 249, 244) | Backgrounds, light fills |
| Light | `#8ed5ab` | rgb(142, 213, 171) | Accent, hover states, soft UI |
| **Medium** | `#01ac6a` | rgb(1, 172, 106) | **Primary accent, buttons, main branding** |
| Dark | `#00833f` | rgb(0, 131, 63) | Dark hover states |
| Darker | `#1c5c39` | rgb(28, 92, 57) | Deep accents, active states |

### Blue Suite (Python & Development)
Secondary accent color representing technology, trust, and innovation.

| Shade | Hex | RGB | Usage |
|-------|-----|-----|-------|
| Lightest | `#f0f6ff` | rgb(240, 246, 255) | Backgrounds, light fills |
| Light | `#a3c4ef` | rgb(163, 196, 239) | Accent, hover states, soft UI |
| **Medium** | `#397bc9` | rgb(57, 123, 201) | **Secondary accent, links, navigation** |
| Dark | `#1b25ab` | rgb(27, 37, 171) | Dark hover states |
| Darker | `#071e61` | rgb(7, 30, 97) | Deep accents, active states |

---

## 🌈 Gradients

### Primary Gradient
`linear-gradient(135deg, #8ed5ab 0%, #01ac6a 100%)`
- **Used for:** Primary buttons, hero elements, "View Projects" CTA
- **Direction:** Top-left to bottom-right (135°)
- **Effect:** Light green → vibrant green

### Secondary Gradient
`linear-gradient(135deg, #a3c4ef 0%, #397bc9 100%)`
- **Used for:** Secondary buttons, "Get in Touch" CTA
- **Direction:** Top-left to bottom-right (135°)
- **Effect:** Light blue → vibrant blue

### Accent Gradient (Combined)
`linear-gradient(135deg, #01ac6a 0%, #397bc9 100%)`
- **Used for:** Card top borders, hover effects, brand gradient
- **Direction:** Top-left to bottom-right (135°)
- **Effect:** Green → Blue blend

### Hero Background Gradient
`linear-gradient(135deg, rgba(142, 213, 171, 0.05) 0%, rgba(163, 196, 239, 0.05) 50%, rgba(142, 213, 171, 0.03) 100%)`
- **Used for:** Section backgrounds, subtle texturing
- **Effect:** Very subtle green-blue blend with transparency

---

## 🎯 Usage Guidelines

### Buttons
| Type | Background | Text | Border | Hover |
|------|-----------|------|--------|-------|
| Primary (`.btn-primary`) | `linear-gradient(135deg, #8ed5ab, #01ac6a)` | White | None | Shadow boost |
| Secondary (`.btn-secondary`) | `linear-gradient(135deg, #a3c4ef, #397bc9)` | White | None | Shadow boost |
| Outline (`.btn-outline`) | `rgba(1, 172, 106, 0.1)` | #01ac6a | 2px #01ac6a | Solid #01ac6a |
| Nav Outline (`.btn-nav-outline`) | `rgba(255,255,255, 0.05)` | #397bc9 | 2px #397bc9 | #f0f6ff light fill |

### Text & Typography
| Element | Color | Usage |
|---------|-------|-------|
| Headings (h1-h4) | `linear-gradient(text)` | Gradient text with `-webkit-background-clip: text` |
| Primary Text | `#4a5568` (light), `#f7fafc` (dark) | Body text |
| Secondary Text | `#4a5568` (light), `#cbd5e0` (dark) | Descriptions |
| Muted Text | `#718096` (light), `#a0aec0` (dark) | Captions, metadata |
| Accents | `#01ac6a` | Links, emphasis |

### Interactive Elements
| Element | Color | Hover |
|---------|-------|-------|
| Links | `#01ac6a` | Darker shade + underline |
| Social Icons | `#4a5568` | `linear-gradient(135deg, #01ac6a 0%, #397bc9 100%)` |
| Cards | `rgba(255, 255, 255, 0.7)` | Border: #01ac6a, Shadow boost |
| Badges | Green/Blue light backgrounds | Green/Blue solid on hover |

### Shadows & Effects
- **Card Shadow:** `0 2px 4px -1px rgba(142, 213, 171, 0.1), 0 1px 2px -1px rgba(163, 196, 239, 0.1)`
- **Hover Shadow:** `0 10px 15px -3px rgba(1, 172, 106, 0.2)`
- **Top Border (Card Hover):** `gradient-accent` (Green → Blue)

---

## 🌙 Dark Theme Adjustments

### Dark Theme Overrides
| Element | Light Theme | Dark Theme |
|---------|------------|-----------|
| Accent | `#01ac6a` (green) | `#8ed5ab` (light green) |
| Secondary | `#397bc9` (blue) | `#a3c4ef` (light blue) |
| Text | `#4a5568` (dark) | `#f7fafc` (light) |
| Background | `#fdfdfd` (white) | `#0a0a0a` (black) |

---

## 📐 Implementation

### CSS Variables
```css
:root {
    /* Green Suite */
    --green-lightest: #f0f9f4;
    --green-light: #8ed5ab;
    --green-medium: #01ac6a;
    --green-dark: #00833f;
    --green-darker: #1c5c39;

    /* Blue Suite */
    --blue-lightest: #f0f6ff;
    --blue-light: #a3c4ef;
    --blue-medium: #397bc9;
    --blue-dark: #1b25ab;
    --blue-darker: #071e61;

    /* Gradients */
    --gradient-primary: linear-gradient(135deg, var(--green-light) 0%, var(--green-medium) 100%);
    --gradient-secondary: linear-gradient(135deg, var(--blue-light) 0%, var(--blue-medium) 100%);
    --gradient-accent: linear-gradient(135deg, var(--green-medium) 0%, var(--blue-medium) 100%);
}

[data-theme="dark"] {
    --accent-color: var(--green-light);
    --accent-color2: var(--blue-light);
}
```

---

## ✨ Examples

### Landing CTA Button
```css
background: var(--gradient-primary);  /* Green gradient */
color: white;
box-shadow: 0 4px 12px rgba(1, 172, 106, 0.3);
```

### Social Icon Hover
```css
background: var(--gradient-accent);   /* Green → Blue */
border-color: var(--gradient-accent);
```

### Card Top Border (Hover)
```css
background: var(--gradient-accent);
transform: scaleX(0 → 1);             /* Animated reveal */
```

---

**Last Updated:** April 5, 2026  
**Version:** 1.0
