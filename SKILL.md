---
name: dope-skills-amp-web-dev
description: Build valid Google AMP pages with modern design aesthetics -- frosted glass, neon glows, gradients, shimmer animations, responsive layouts. Use when asked to create a landing page, marketing page, product page, or any web page that must be valid AMP. Covers AMP boilerplate, allowed CSS techniques, AMP components, responsive design, and deployment to GitHub Pages with custom domains. Triggers on "AMP page", "AMP landing page", "AMP website", "Google AMP", "valid AMP", "amp web dev".
---

# Dope Skills - AMP Web Dev

Build production-ready, valid Google AMP pages with modern premium design.

## What is AMP

AMP (Accelerated Mobile Pages) is a web framework that enforces performance constraints:
- No custom JavaScript
- CSS limited to 75KB in a single `<style amp-custom>` block
- Images use `<amp-img>` instead of `<img>`
- Interactivity via AMP components only
- Boilerplate code must match the official spec exactly

The tradeoff: you lose JavaScript flexibility but gain guaranteed fast load times and eligibility for Google's AMP cache/search features.

## AMP Boilerplate (MUST BE EXACT)

This boilerplate must appear in the `<head>` of every AMP page. It must match character-for-character. Do not modify it. Do not reformat it. Copy-paste it exactly.

```html
<style amp-boilerplate>body{-webkit-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-moz-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-ms-animation:-amp-start 8s steps(1,end) 0s 1 normal both;animation:-amp-start 8s steps(1,end) 0s 1 normal both}@-webkit-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-moz-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-ms-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-o-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}</style><noscript><style amp-boilerplate>body{-webkit-animation:none;-moz-animation:none;-ms-animation:none;animation:none}</style></noscript>
```

**Critical details:**
- The `<noscript>` tag must be on the SAME LINE as the closing `</style>` tag -- no whitespace between them
- ALL vendor prefixes must be present: `-webkit-`, `-moz-`, `-ms-`, `-o-`
- The noscript style must include `-ms-animation:none`
- Missing ANY prefix = invalid AMP

## Required Head Structure

```html
<!doctype html>
<html amp lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1">
  <title>Page Title</title>
  <meta name="description" content="Page description">
  <link rel="canonical" href="https://yourdomain.com/">
  <!-- Google Fonts: use <link>, NEVER @import -->
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&display=swap">
  <!-- AMP boilerplate (exact copy from above) -->
  <style amp-boilerplate>...</style><noscript><style amp-boilerplate>...</style></noscript>
  <!-- AMP JS -->
  <script async src="https://cdn.ampproject.org/v0.js"></script>
  <!-- AMP components (only include what you use) -->
  <script async custom-element="amp-accordion" src="https://cdn.ampproject.org/v0/amp-accordion-0.1.js"></script>
  <!-- All custom CSS in one block -->
  <style amp-custom>
    /* Your CSS here -- max 75KB */
  </style>
</head>
```

## CSS Rules for AMP

### Allowed
- All standard CSS properties
- CSS Grid and Flexbox
- CSS custom properties (variables)
- `@keyframes` animations
- `@media` queries
- Pseudo-elements (`::before`, `::after`)
- `calc()`, `min()`, `max()`, `clamp()`
- `linear-gradient`, `radial-gradient`
- `backdrop-filter: blur()`
- `box-shadow`, `text-shadow`
- `transition` properties
- Google Fonts via `<link>` tag

### NOT Allowed
- `@import` inside `<style amp-custom>` (use `<link>` tag instead)
- `!important` qualifier
- More than 75,000 bytes of CSS
- Multiple `<style amp-custom>` blocks
- Inline styles on elements ARE allowed but count toward the limit
- `behavior:` or `-moz-binding:` properties
- `filter:` on the `<html>` or `<body>` elements

### Common Gotcha
`@import url('https://fonts.googleapis.com/...')` inside `<style amp-custom>` is the #1 AMP validation error. It causes a "CSS syntax error" that can cascade and also flag the boilerplate as malformed.

### Best Practice: Inline Font Declarations

Do NOT use an external `<link>` tag for Google Fonts -- it creates a render-blocking request (Lighthouse flags ~790ms penalty).

Instead, inline `@font-face` declarations directly in `<style amp-custom>`. This is allowed -- it's `@import` that's banned, not `@font-face`.

```css
@font-face{
  font-family:'Inter';
  font-style:normal;
  font-weight:700;
  font-display:swap;
  src:url(https://fonts.gstatic.com/s/inter/v20/UcC73FwrK3iLTeHuS_nVMrMxCp50SjIa1ZL7W0Q5nw.woff2) format('woff2');
  unicode-range:U+0000-00FF,U+0131,U+0152-0153,U+02BB-02BC,U+02C6,U+02DA,U+02DC,U+0304,U+0308,U+0329,U+2000-206F,U+20AC,U+2122,U+2191,U+2193,U+2212,U+2215,U+FEFF,U+FFFD
}
```

**Key details:**
- Only include the `latin` unicode-range subset (skip cyrillic, greek, vietnamese) to save CSS bytes
- Add one `@font-face` per weight you use (400, 500, 600, 700, 800, 900)
- Fetch the CSS from Google Fonts to get the correct woff2 URLs: `curl -s -A 'Mozilla/5.0' 'https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap'`

### Font Preloading

Add a `<link rel="preload">` in the `<head>` to eliminate the critical request chain (font download waiting on CSS parse):

```html
<link rel="preload" href="https://fonts.gstatic.com/s/inter/v20/UcC73FwrK3iLTeHuS_nVMrMxCp50SjIa1ZL7W0Q5nw.woff2" as="font" type="font/woff2" crossorigin>
```

The browser starts downloading the font immediately, in parallel with parsing. By the time the `@font-face` declaration is reached, the font is already downloaded.

## AMP Components Quick Reference

Include the script tag in `<head>` for each component you use.

| Component | Use Case | Script |
|-----------|----------|--------|
| amp-img | All images (required) | Built-in |
| amp-accordion | FAQ sections, expandable content | amp-accordion-0.1.js |
| amp-carousel | Image/card carousels | amp-carousel-0.2.js |
| amp-form | Email capture, contact forms | amp-form-0.1.js |
| amp-fit-text | Auto-sizing text | amp-fit-text-0.1.js |
| amp-video | Self-hosted video | Built-in |
| amp-youtube | YouTube embeds | amp-youtube-0.1.js |
| amp-analytics | Google Analytics | amp-analytics-0.1.js |
| amp-sidebar | Mobile navigation drawer | amp-sidebar-0.1.js |

### amp-img (replaces all `<img>` tags)
```html
<!-- Fixed size -->
<amp-img src="image.webp" width="200" height="182" alt="Description" layout="fixed"></amp-img>

<!-- Responsive (fills container width) -->
<amp-img src="image.jpg" width="720" height="1280" alt="Description" layout="responsive"></amp-img>

<!-- Fill parent container -->
<amp-img src="bg.jpg" layout="fill" alt="Background"></amp-img>
```

### amp-accordion (FAQ sections)

**Important:** CSS `::after` pseudo-elements on `<h4>` do NOT work reliably inside amp-accordion. AMP's internal component structure overrides the header layout. Use inline `<span>` elements instead.

```html
<amp-accordion animate>
  <section>
    <h4><span class="faq-q"><span>Question text</span><span class="faq-caret">&#9662;</span></span></h4>
    <div class="answer">Answer text</div>
  </section>
</amp-accordion>
```

**FAQ caret styling (using inline spans, NOT pseudo-elements):**
```css
.faq-q {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}
.faq-caret {
  color: #999;
  font-size: 18px;
  flex-shrink: 0;
  margin-left: 16px;
  transition: transform 0.2s;
}
amp-accordion section[expanded] .faq-caret {
  transform: rotate(180deg);
}
```

## Design Patterns (AMP-Safe)

### Frosted Glass Cards
```css
.card {
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 16px;
  padding: 32px;
  backdrop-filter: blur(10px);
}
```

### Neon Glow Buttons
```css
.btn {
  background: linear-gradient(135deg, #00b894, #00cec9);
  color: white;
  border-radius: 12px;
  padding: 16px 36px;
  box-shadow: 0 0 30px rgba(0,184,148,0.3);
  text-decoration: none;
  font-weight: 700;
}
.btn:hover {
  box-shadow: 0 0 50px rgba(0,184,148,0.5);
  transform: translateY(-2px);
}
```

### Shimmer Animation on Buttons
```css
.btn {
  position: relative;
  overflow: hidden;
}
.btn::after {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 60%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.25), transparent);
  animation: shimmer 6s ease-in-out infinite;
  animation-delay: 1s;
}
@keyframes shimmer {
  0% { left: -100%; }
  16% { left: 150%; }
  100% { left: 150%; }
}
```
This creates a single sweep in the first ~1 second, then idles for ~5 seconds before repeating. Subtle and premium.

### Gradient Text
```css
.highlight {
  background: linear-gradient(135deg, #4d7cff, #00d4ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}
```

### CSS Variables for Theming
```css
:root {
  --midnight: #0a0e1a;
  --card-bg: rgba(255,255,255,0.04);
  --card-border: rgba(255,255,255,0.08);
  --silver: #c0c7d6;
  --white: #ffffff;
  --neon-blue: #4d7cff;
  --neon-cyan: #00d4ff;
}
```

## Responsive Design in AMP

### Mobile-First Grid Pattern
```css
/* Works on all screens without overflow */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(320px, 100%), 1fr));
  gap: 24px;
}
```

**Critical:** Always use `min(Npx, 100%)` inside `minmax()`. Without `min()`, a `minmax(320px, 1fr)` grid will overflow on screens narrower than 320px + padding.

### Preventing Horizontal Scroll
```css
html { overflow-x: hidden; }
body { overflow-x: hidden; max-width: 100%; width: 100%; }

/* Every section */
.section { overflow: hidden; }
```

**Do NOT use decorative elements with absolute positioning and negative offsets** (e.g., `right: -200px` for glow effects). They extend the document width and cause horizontal scroll on mobile. Either clip them with `overflow: hidden` on the parent or remove them entirely on mobile.

### Two-Column Desktop, One-Column Mobile
```css
.hero-inner {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 48px;
  align-items: center;
}

@media (max-width: 768px) {
  .hero-inner {
    grid-template-columns: 1fr;
    gap: 0;
  }
  .hero-media { order: 1; margin-bottom: 12px; }
  .hero-content { order: 2; text-align: center; }
}
```

Use CSS `order` to resequence elements on mobile without changing HTML structure.

## Deployment to GitHub Pages

### Setup
```bash
# Create repo
gh repo create username/site-name --public

# Initialize and push
cd project-dir
git init
git remote add origin https://github.com/username/site-name.git
git add -A
git commit -m "Initial deploy"
git branch -M main
git push -u origin main

# Enable GitHub Pages
gh api repos/username/site-name/pages -X POST -f "source[branch]=main" -f "source[path]=/"
```

### Custom Domain
1. Add DNS records at your registrar:
   - A records: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - CNAME: `www` -> `username.github.io`

2. Add CNAME file to repo:
```bash
echo "yourdomain.com" > CNAME
git add CNAME && git commit -m "Add custom domain" && git push
```

3. Configure in GitHub:
```bash
gh api repos/username/site-name/pages -X PUT -f "cname=yourdomain.com" -f "source[branch]=main" -f "source[path]=/"
```

## Validation

Always validate before considering the page done:
- **Google AMP Test:** https://search.google.com/test/amp
- **AMP Validator browser extension:** Available for Chrome
- **In-browser:** Append `#development=1` to the URL and check browser console

Common validation errors and fixes:

| Error | Cause | Fix |
|-------|-------|-----|
| Boilerplate tags malformed | Missing vendor prefixes (-ms-, -o-) | Use exact official boilerplate |
| CSS syntax error | `@import` in style amp-custom | Move to `<link>` tag |
| Disallowed tag `<img>` | Used HTML img tag | Replace with `<amp-img>` |
| Disallowed tag `<script>` | Custom JavaScript | Remove; use AMP components |
| CSS too long | Over 75KB | Reduce CSS, remove unused rules |
| Missing required attribute | amp-img without width/height | Add explicit width and height |

## File Organization

```
project/
  index.html          -- single AMP HTML file
  logo.webp           -- logo (WebP for transparency + compression)
  images/             -- any other images
  CNAME               -- custom domain (if using GitHub Pages)
```

AMP pages are ideally single-file. All CSS is inline in `<style amp-custom>`. No external stylesheets. Google Fonts should be inlined as `@font-face` declarations, not loaded via external `<link>` tags.

## Accessibility

### Main Landmark (Required)

Wrap all content between nav and footer in a `<main>` element. Lighthouse flags this for screen reader navigation.

```html
<nav>...</nav>
<main>
  <!-- all page sections -->
</main>
<footer>...</footer>
```

### Image Alt Text

All `<amp-img>` elements must have descriptive `alt` attributes.

## Image Optimization

Resize images to match their displayed dimensions exactly. If an `<amp-img>` displays at 200x182, the source file should be 200x182 (or 2x for retina: 400x364). Oversized images waste bandwidth and Lighthouse flags them.

Prefer WebP format for transparency + compression. AMP supports WebP natively.

## Lighthouse Warnings to Ignore

### Forced Reflows
Lighthouse flags "forced reflow" warnings from the AMP runtime (`v0.js`). This is Google's own code doing internal layout calculations. Can't fix, unscored, ignore.

### GitHub Pages Cache TTL
GitHub Pages sets a 10-minute cache TTL on all assets. Not configurable. Unscored in Lighthouse. To fix, put Cloudflare in front or move to Cloudflare Pages.

## References

- AMP official docs: https://amp.dev/documentation/
- AMP component catalog: https://amp.dev/documentation/components/
- AMP validator: https://validator.ampproject.org/
- AMP playground: https://playground.amp.dev/
