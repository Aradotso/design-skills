```markdown
---
name: huashu-design-html-prototyping
description: HTML-native design skill for Claude Code — ship high-fidelity prototypes, slides, animations, and infographics in 3-30 minutes with AI-assisted design workflows
triggers:
  - "create a product launch animation"
  - "build an interactive iOS prototype"
  - "make a slide deck presentation"
  - "design an infographic with data visualization"
  - "generate design variations with tweaks"
  - "run a 5-dimension design review"
  - "export this animation to MP4"
  - "create a clickable app prototype"
---

# Huashu Design — HTML-Native Design Prototyping Skill

> Skill by [ara.so](https://ara.so) — Design Skills collection.

**Huashu Design** is an agent-agnostic design skill that transforms natural language prompts into production-ready HTML design artifacts: interactive prototypes, animated presentations, slide decks (with editable PPTX export), infographics, and motion graphics (MP4/GIF). Built for Claude Code, Cursor, and other AI coding agents.

**Philosophy**: Great high-fidelity design doesn't start from a blank page — it grows from existing design context (logos, product shots, UI screenshots, brand colors). This skill enforces a **Core Asset Protocol** to gather and verify real brand assets before generating designs.

## Installation

```bash
npx skills add alchaincyf/huashu-design
```

The skill auto-loads into your agent's context. No configuration files needed — all workflows are conversation-driven.

## What It Does

| Capability | Output Format | Typical Duration |
|------------|---------------|------------------|
| **Interactive Prototypes** | Single-file HTML with state management, real device bezels (iPhone/iPad), Playwright-verified clicks | 10-15 min |
| **Slide Decks** | HTML presentation + editable PPTX (real text frames, not images) | 15-25 min |
| **Motion Design** | MP4 (25fps/60fps), GIF (palette-optimized), optional BGM | 8-12 min |
| **Design Variations** | Side-by-side HTML with live parameter tweaks (colors, typography, density) | 10 min |
| **Infographics** | Print-quality HTML → PDF/PNG/SVG exports (300dpi) | 10 min |
| **Design Direction Advisor** | 3 differentiated visual directions from 5 design schools × 20 philosophies | 5 min |
| **Expert Critique** | 5-dimension radar chart + actionable punch list (Keep/Fix/Quick Wins) | 3 min |

## Core Workflows

### 1. Core Asset Protocol (Mandatory for Brand Work)

**When to trigger**: Any task involving a specific brand (Stripe, Linear, DJI, etc.).

**5-step hard process**:

```markdown
STEP 1: ASK
- Logo files (SVG preferred)
- Product renders/photos
- UI screenshots (App Store, marketing site)
- Color palette (hex codes)
- Fonts (names/weights)
- Brand guidelines URL

STEP 2: SEARCH OFFICIAL CHANNELS
- <brand>.com/brand
- <brand>.com/press
- brand.<brand>.com
- Product pages
- Launch videos

STEP 3: DOWNLOAD BY ASSET TYPE
Logo: SVG → inline in HTML → social avatar fallback
Product shots: Hero image → press kit → video frames → AI-generated from reference
UI: App Store screenshots → official video frames

STEP 4: VERIFY + EXTRACT
- Check logo fidelity (not distorted)
- Verify product image resolution (min 1200px wide)
- Extract color hex values from actual images (use eyedropper, not memory)
- Check UI screenshot freshness (current version)

STEP 5: FREEZE TO SPEC
Create `brand-spec.md`:
```

**Example `brand-spec.md`**:

```markdown
# Linear Brand Specification

## Logo
- Path: `assets/linear-logo.svg`
- Inline SVG: `<svg viewBox="0 0 32 32">...</svg>`
- Colors: `#5E6AD2` (primary), `#000000` (wordmark)

## Product UI Screenshots
- Main app: `assets/linear-ui-main.png` (1920×1080)
- Issue detail: `assets/linear-ui-detail.png` (1920×1080)

## Color Palette
```css
:root {
  --linear-primary: #5E6AD2;
  --linear-bg: #FFFFFF;
  --linear-text: #000000;
  --linear-border: #E5E5E5;
}
```

## Typography
- Display: Inter Display (700/800)
- Body: Inter (400/500/600)
```

**Asset Priority Ranking**:
1. Logo (mandatory)
2. Product renders (mandatory for physical products)
3. UI screenshots (mandatory for digital products)
4. Colors (auxiliary)
5. Fonts (auxiliary)

### 2. Junior Designer Workflow (Default Mode)

**Process**:

```
1. BATCH QUESTIONS → Wait for all answers
2. ASSUMPTIONS + PLACEHOLDERS → Show early (gray blocks OK)
3. REAL CONTENT → Fill in verified data
4. VARIATIONS → Generate 2-3 options
5. TWEAKS → Add live parameter controls
6. PLAYWRIGHT VERIFICATION → Manual eyeball test
7. DELIVERY
```

**Example Conversation Flow**:

```
User: "Build a Pomodoro timer app prototype for iOS"

Agent:
QUESTIONS (answer all at once):
- Target device: iPhone 15 Pro, iPhone SE, or iPad?
- How many screens: 4-6 typical (timer, settings, stats, history)?
- Visual style: Minimalist / Playful / Professional?
- Color preference: Tomato red / Custom?
- Timer presets: 25/5/15 min or custom?
- Do you have existing brand assets (logo/colors)?

[User answers]

Agent:
ASSUMPTIONS (showing early):
- iPhone 15 Pro bezel (Dynamic Island)
- 4 screens: Timer / Settings / Stats / History
- Minimalist style, tomato red (#FF6347)
- No brand assets → using design philosophy: Swiss Modernism
- Placeholder: Timer shows "25:00", gray Start button

[Generates HTML with gray blocks + reasoning comments]

Agent:
REAL CONTENT (filling in):
- Timer: Circular progress ring, "25:00" display
- Settings: Toggle switches, preset buttons
- Stats: Chart.js bar chart for daily focus time
- History: List of completed sessions

[Shows updated HTML]

Agent:
VARIATIONS:
A) High contrast (black bg, white text)
B) Soft gradient (peach to coral)
C) Neumorphic (raised buttons)

[User picks A]

Agent:
TWEAKS ADDED:
- Color: Red intensity slider
- Typography: Display font size (18-32px)
- Density: Spacing multiplier (0.8x-1.2x)

[Adds side panel controls]

Agent:
PLAYWRIGHT VERIFICATION:
✓ Timer button clickable
✓ Settings save to localStorage
✓ Navigation between screens works
✓ Responsive to iPhone 15 Pro viewport

DELIVERY: `pomodoro-prototype.html` (self-contained, 1 file)
```

### 3. Design Direction Advisor (Fallback for Vague Briefs)

**Trigger**: When brief is too vague to execute directly.

**Output**: 3 differentiated directions from 5 schools × 20 philosophies:

```
SCHOOLS:
1. Modernist (Swiss, Bauhaus, International Typographic)
2. Digital Native (Flat 2.0, Neumorphic, Glassmorphic)
3. Editorial (Magazine, Brutalist, Neo-Grotesque)
4. Organic (Memphis, Art Deco, Retro Futurism)
5. Data-Driven (Infographic, Dashboard, Scientific)

PROCESS:
1. Recommend 3 directions (each from DIFFERENT school)
2. Each includes:
   - Flagship works (e.g., "Braun product manuals, NYC Subway map")
   - Gestalt keywords (e.g., "grid, white space, sans-serif hierarchy")
   - Representative designer (e.g., "Massimo Vignelli")
3. Generate 3 visual demos IN PARALLEL
4. User picks one → continue Junior Designer flow
```

**Example Output**:

```markdown
DIRECTION A: Swiss Modernism (Modernist school)
- Flagship: Helvetica posters, Swiss railway timetables
- Keywords: Grid, asymmetry, negative space, sans-serif
- Designer: Josef Müller-Brockmann
- Demo: `direction-a-swiss.html`

DIRECTION B: Glassmorphic (Digital Native school)
- Flagship: macOS Big Sur UI, iOS 14 widgets
- Keywords: Frosted glass, depth, translucency, subtle shadows
- Designer: Apple Design Team
- Demo: `direction-b-glass.html`

DIRECTION C: Brutalist Editorial (Editorial school)
- Flagship: Bloomberg Businessweek covers, Craigslist
- Keywords: Raw HTML, monospace, high contrast, anti-polish
- Designer: Tracy Ma
- Demo: `direction-c-brutalist.html`
```

### 4. Motion Design Engine

**Architecture**: Stage + Sprite time-slice model.

**Core APIs**:

```javascript
// TIME MANAGEMENT
const t = useTime(); // Returns current time in seconds (0-10 for 10s animation)

// SPRITE CONTROL
const sprite = useSprite('hero-text', {
  start: 2.0,    // Appear at 2s
  duration: 3.0, // Visible for 3s
  exit: 1.0      // 1s exit animation
});

// INTERPOLATION
const opacity = interpolate(t, [2, 3], [0, 1], Easing.easeOut);
const scale = interpolate(t, [2, 3], [0.8, 1], Easing.spring);

// EASING FUNCTIONS
Easing.linear
Easing.easeIn / easeOut / easeInOut
Easing.spring
Easing.bounce
```

**Example Animation** (Product Launch Hero):

```html
<!DOCTYPE html>
<html>
<head>
<style>
body { margin: 0; background: #000; overflow: hidden; }
#stage { width: 1920px; height: 1080px; position: relative; }
.sprite { position: absolute; }
</style>
</head>
<body>
<div id="stage"></div>
<script>
const DURATION = 10; // 10-second animation
let currentTime = 0;

// Easing functions
const Easing = {
  linear: t => t,
  easeOut: t => 1 - Math.pow(1 - t, 3),
  spring: t => {
    const c4 = (2 * Math.PI) / 3;
    return t === 0 ? 0 : t === 1 ? 1 : Math.pow(2, -10 * t) * Math.sin((t * 10 - 0.75) * c4) + 1;
  }
};

function interpolate(t, domain, range, easing = Easing.linear) {
  const [t0, t1] = domain;
  const [v0, v1] = range;
  if (t <= t0) return v0;
  if (t >= t1) return v1;
  const progress = easing((t - t0) / (t1 - t0));
  return v0 + (v1 - v0) * progress;
}

function useTime() {
  return currentTime;
}

function render() {
  const t = useTime();
  const stage = document.getElementById('stage');
  stage.innerHTML = '';

  // Logo sprite (0-10s)
  if (t >= 0 && t <= 10) {
    const logoOpacity = interpolate(t, [0, 1], [0, 1], Easing.easeOut);
    const logoScale = interpolate(t, [0, 1], [0.5, 1], Easing.spring);
    const logo = document.createElement('div');
    logo.className = 'sprite';
    logo.style.cssText = `
      left: 50%; top: 40%;
      transform: translate(-50%, -50%) scale(${logoScale});
      opacity: ${logoOpacity};
    `;
    logo.innerHTML = '<img src="logo.svg" width="200">';
    stage.appendChild(logo);
  }

  // Product shot sprite (2-8s)
  if (t >= 2 && t <= 8) {
    const productOpacity = interpolate(t, [2, 3], [0, 1], Easing.easeOut);
    const productY = interpolate(t, [2, 3], [60, 50], Easing.easeOut);
    const product = document.createElement('div');
    product.className = 'sprite';
    product.style.cssText = `
      left: 50%; top: ${productY}%;
      transform: translate(-50%, -50%);
      opacity: ${productOpacity};
    `;
    product.innerHTML = '<img src="product.png" width="600">';
    stage.appendChild(product);
  }

  // Headline sprite (4-10s)
  if (t >= 4 && t <= 10) {
    const textOpacity = interpolate(t, [4, 5], [0, 1], Easing.easeOut);
    const text = document.createElement('div');
    text.className = 'sprite';
    text.style.cssText = `
      left: 50%; top: 75%;
      transform: translate(-50%, -50%);
      opacity: ${textOpacity};
      font-family: Inter, sans-serif;
      font-size: 48px;
      font-weight: 700;
      color: #fff;
      text-align: center;
    `;
    text.textContent = 'Introducing the Future';
    stage.appendChild(text);
  }
}

// Animation loop
function animate() {
  render();
  currentTime += 1/60; // 60fps
  if (currentTime < DURATION) {
    requestAnimationFrame(animate);
  }
}

animate();
</script>
</body>
</html>
```

**Export Commands**:

```bash
# Generate MP4 (25fps, default)
npx playwright screenshot animation.html --video animation.mp4

# Generate 60fps interpolated MP4
ffmpeg -i animation-25fps.mp4 -filter:v "minterpolate='fps=60'" animation-60fps.mp4

# Generate GIF (palette-optimized)
ffmpeg -i animation.mp4 -vf "fps=15,scale=800:-1:flags=lanczos,split[s0][s1];[s0]palettegen[p];[s1][p]paletteuse" animation.gif
```

### 5. Slides → Editable PPTX Export

**HTML to PPTX Translation**:

The `html2pptx.js` script reads DOM computed styles and translates each HTML element into PowerPoint objects (text frames, shapes, images).

**Example Slide Deck**:

```html
<!DOCTYPE html>
<html>
<head>
<style>
body { margin: 0; font-family: Inter, sans-serif; }
.slide { width: 1280px; height: 720px; padding: 60px; box-sizing: border-box; }
.slide h1 { font-size: 64px; font-weight: 700; color: #000; margin: 0; }
.slide p { font-size: 32px; color: #666; margin: 20px 0; }
</style>
</head>
<body>
<div class="slide">
  <h1>AI Psychology</h1>
  <p>Understanding how machines think</p>
</div>
<div class="slide">
  <h1>Key Concepts</h1>
  <ul>
    <li>Neural networks mirror brain structures</li>
    <li>Reinforcement learning shapes behavior</li>
    <li>Attention mechanisms mimic human focus</li>
  </ul>
</div>
</body>
</html>
```

**Export to PPTX**:

```javascript
// html2pptx.js (run in Node.js)
const pptxgen = require('pptxgenjs');
const { JSDOM } = require('jsdom');
const fs = require('fs');

const html = fs.readFileSync('slides.html', 'utf8');
const dom = new JSDOM(html);
const slides = dom.window.document.querySelectorAll('.slide');

const pptx = new pptxgen();

slides.forEach(slideEl => {
  const slide = pptx.addSlide();
  const h1 = slideEl.querySelector('h1');
  const p = slideEl.querySelector('p');

  if (h1) {
    const h1Style = dom.window.getComputedStyle(h1);
    slide.addText(h1.textContent, {
      x: 0.5, y: 1, w: 9, h: 1,
      fontSize: parseInt(h1Style.fontSize) / 2, // Convert px to pt
      bold: h1Style.fontWeight >= 700,
      color: h1Style.color.replace(/rgb\((\d+), (\d+), (\d+)\)/, (_, r, g, b) => 
        ((1 << 24) + (parseInt(r) << 16) + (parseInt(g) << 8) + parseInt(b)).toString(16).slice(1)
      )
    });
  }

  if (p) {
    const pStyle = dom.window.getComputedStyle(p);
    slide.addText(p.textContent, {
      x: 0.5, y: 2.5, w: 9, h: 1,
      fontSize: parseInt(pStyle.fontSize) / 2,
      color: pStyle.color.replace(/rgb\((\d+), (\d+), (\d+)\)/, (_, r, g, b) => 
        ((1 << 24) + (parseInt(r) << 16) + (parseInt(g) << 8) + parseInt(b)).toString(16).slice(1)
      )
    });
  }
});

pptx.writeFile('slides-editable.pptx');
```

**Key Point**: Exported PPTX files contain **real text frames**, not flattened images. Users can edit text directly in PowerPoint.

### 6. Tweaks — Live Parameter Controls

**Implementation** (pure frontend, no backend):

```html
<div id="tweaks-panel" style="position: fixed; right: 20px; top: 20px; background: white; padding: 20px; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.1);">
  <h3>Tweaks</h3>
  
  <label>Primary Color</label>
  <input type="color" id="primary-color" value="#5E6AD2">
  
  <label>Font Size (px)</label>
  <input type="range" id="font-size" min="14" max="24" value="16">
  
  <label>Spacing (×)</label>
  <input type="range" id="spacing" min="0.8" max="1.5" step="0.1" value="1">
  
  <button onclick="resetTweaks()">Reset</button>
</div>

<script>
// Load saved tweaks from localStorage
const savedTweaks = JSON.parse(localStorage.getItem('tweaks') || '{}');
document.documentElement.style.setProperty('--primary', savedTweaks.primaryColor || '#5E6AD2');
document.documentElement.style.setProperty('--font-size', (savedTweaks.fontSize || 16) + 'px');
document.documentElement.style.setProperty('--spacing', savedTweaks.spacing || 1);

// Listen for changes
document.getElementById('primary-color').addEventListener('input', e => {
  document.documentElement.style.setProperty('--primary', e.target.value);
  saveTweaks();
});

document.getElementById('font-size').addEventListener('input', e => {
  document.documentElement.style.setProperty('--font-size', e.target.value + 'px');
  saveTweaks();
});

document.getElementById('spacing').addEventListener('input', e => {
  document.documentElement.style.setProperty('--spacing', e.target.value);
  saveTweaks();
});

function saveTweaks() {
  const tweaks = {
    primaryColor: document.getElementById('primary-color').value,
    fontSize: parseInt(document.getElementById('font-size').value),
    spacing: parseFloat(document.getElementById('spacing').value)
  };
  localStorage.setItem('tweaks', JSON.stringify(tweaks));
}

function resetTweaks() {
  localStorage.removeItem('tweaks');
  location.reload();
}
</script>
```

**Persistence**: All tweaks save to `localStorage` → survive page reload.

### 7. Five-Dimension Expert Critique

**Dimensions**:

1. **Philosophical Coherence** (0-10): Does the design follow a consistent design philosophy?
2. **Visual Hierarchy** (0-10): Is information prioritized correctly (size, color, position)?
3. **Execution Craft** (0-10): Typography, spacing, color harmony, attention to detail
4. **Functionality** (0-10): Does it work? Are interactions intuitive?
5. **Innovation** (0-10): Does it push boundaries or follow trends?

**Output Format**:

```markdown
# Expert Critique: Pomodoro Timer Prototype

## Scores (Radar Chart)

│ Dimension               │ Score │
│─────────────────────────│───────│
│ Philosophical Coherence │  8/10 │
│ Visual Hierarchy        │  7/10 │
│ Execution Craft         │  9/10 │
│ Functionality           │  6/10 │
│ Innovation              │  5/10 │

## KEEP (Strengths)
✓ Clean Swiss Modernism execution (grid, white space, Helvetica)
✓ High contrast aids readability (black text on white)
✓ Timer progress ring is intuitive

## FIX (Critical Issues)
✗ Settings don't persist across sessions (localStorage missing)
✗ No visual feedback when timer completes (needs notification)
✗ History screen is text-only (add chart visualization)

## QUICK WINS (30-min improvements)
→ Add browser notification when timer ends
→ Implement localStorage for settings
→ Change history list to bar chart (Chart.js, 10 lines)
```

**Radar Chart** (generated as SVG in HTML):

```html
<svg viewBox="0 0 400 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Pentagon background -->
  <polygon points="200,50 350,150 300,320 100,320 50,150" fill="#f0f0f0" stroke="#ccc"/>
  
  <!-- Score overlay (example: 8,7,9,6,5) -->
  <polygon points="200,70 330,155 280,302 120,302 70,155" fill="rgba(94,106,210,0.3)" stroke="#5E6AD2" stroke-width="2"/>
  
  <!-- Labels -->
  <text x="200" y="40" text-anchor="middle">Philosophical</text>
  <text x="360" y="155" text-anchor="start">Visual Hierarchy</text>
  <text x="310" y="335" text-anchor="middle">Execution</text>
  <text x="90" y="335" text-anchor="middle">Functionality</text>
  <text x="40" y="155" text-anchor="end">Innovation</text>
</svg>
```

## Anti AI-Slop Rules

**Avoid**:
- Purple gradients everywhere
- Emoji icons (🚀 ✨ 💡)
- Rounded-corner + left border accent cards
- SVG placeholder humans
- Inter as display font
- CSS silhouettes standing in for real product shots

**Use Instead**:
- `text-wrap: pretty` for balanced line breaks
- CSS Grid for precise layouts
- Carefully chosen serif display faces (Fraunces, Crimson Pro, EB Garamond)
- `oklch()` color space for perceptually uniform colors
- Real product shots (from Core Asset Protocol)

**Example Anti-Slop CSS**:

```css
:root {
  --color-bg: oklch(98% 0.01 264); /* Neutral white, slight blue tint */
  --color-text: oklch(20% 0.02 264); /* Near-black, slight warmth */
  --color-accent: oklch(55% 0.15 264); /* Muted blue */
}

body {
  font-family: 'Crimson Pro', Georgia, serif; /* Serif body */
  font-size: 18px;
  line-height: 1.6;
  text-wrap: pretty;
  background: var(--color-bg);
  color: var(--color-text);
}

h1 {
  font-family: 'Fraunces', serif; /* Display serif */
  font-weight: 700;
  font-size: 48px;
  line-height: 1.1;
  letter-spacing: -0.02em;
}

.grid {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 24px;
}
```

## Fact Verification (Principle #0)

**Rule**: When the task mentions a specific product/technology/event, the **first action** must be a `WebSearch` to confirm:
- Existence (is "DJI Pocket 4" real or hallucinated?)
- Release status (announced / shipping / rumored?)
- Current version (is "Gemini 3 Pro" the latest or is it "Gemini 2 Ultra"?)
- Specs (resolution, price, features)

**Example**:

```
User: "Create a product launch animation for the new DJI Pocket 4"

Agent: [WebSearch: "DJI Pocket 4 release date specs"]
Result: No official product named "DJI Pocket 4" exists. Latest is "DJI Pocket 3" (released Oct 2023).

Agent: "I couldn't find an official DJI Pocket 4. Did you mean DJI Pocket 3 (latest model), or is this a hypothetical future product?"
```

**Cost of a search**: ~10 seconds  
**Cost of a wrong assumption**: 1-2 hours of rework

## Playwright Verification Before Delivery

**Automated checks**:

```javascript
// verify-prototype.js (run with Playwright)
const { test, expect } = require('@playwright/test');

test('iOS prototype functionality', async ({ page }) => {
  await page.goto('file:///path/to/prototype.html');
  
  // Check device bezel renders
  await expect(page.locator('.iphone-bezel')).toBeVisible();
  
  // Test navigation
  await page.click('button:has-text("Settings")');
  await expect(page.locator('h1:has-text("Settings")')).toBeVisible();
  
  // Test state persistence
  await page.click('input[type="checkbox"]');
  await page.reload();
  await expect(page.locator('input[type="checkbox"]')).toBeChecked();
  
  // Check responsive layout
  await page.setViewportSize({ width: 390, height: 844 }); // iPhone 15 Pro
  const button = await page.locator('button.primary');
  await expect(button).toBeInViewport();
});
```

**Manual eyeball test** (agent's internal checklist before delivery):
- [ ] Logo not distorted
- [ ] Text readable (min 14px body, high contrast)
- [ ] Clickable areas large enough (min 44×44px)
- [ ] Animations smooth (no jank)
- [ ] Real content (no Lorem Ipsum)

## Common Patterns

### iOS App Prototype Template

```html
<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
body {
  margin: 0;
  background: #f0f0f0;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  font-family: -apple-system, BlinkMacSystemFont, sans-serif;
}

.device {
  width: 390px;
  height: 844px;
  background: #000;
  border-radius: 55px;
  padding: 12px;
  box-shadow: 0 20px 60px rgba(0,0,0,0.3);
  position: relative;
}

.dynamic-island {
  position: absolute;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  width: 126px;
  height: 37px;
  background: #000;
  border-radius: 20px;
  z-index: 10;
}

.screen {
  width: 100%;
  height: 100%;
  background: #fff;
  border-radius: 45px;
  overflow: hidden;
  position: relative;
}

.status-bar {
  height: 54px;
  padding: 0 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 15px;
  font-weight: 600;
}

.content {
  height: calc(100% - 54px - 34px);
  overflow-y: auto;
  padding: 20px;
}

.home-indicator {
  position: absolute;
  bottom: 8px;
  left: 50%;
  transform: translateX(-50%);
  width: 134px;
  height: 5px;
  background: #000;
  border-radius: 3px;
  opacity: 0.3;
}

/* App-specific styles */
button {
  width: 100%;
  padding: 16px;
  background: #007AFF;
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 17px;
  font-weight: 600;
  cursor: pointer;
}

button:active {
  background: #0051D5;
}
</style>
</head>
<body>
<div class="device">
  <div class="dynamic-island"></div>
  <div class="screen">
    <div class="status-bar">
      <span>9:41</span>
      <span>100% 🔋</span>
    </div>
    <div class="content" id="app">
      <!-- App content here -->
      <h1>Pomodoro Timer</h1>
      <div style="font-size: 64px; text-align: center; margin: 60px 0;">25:00</div>
      <button>Start Focus</button>
    </div>
    <div class="home-indicator"></div>
  </div>
</div>

<script>
// State management
const state = {
  screen: 'timer',
  timerSeconds: 1500, // 25 min
  isRunning: false
};

function render() {
  const app = document.getElementById('app');
  
  if (state.screen
