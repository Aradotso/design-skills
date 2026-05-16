---
name: nothing-design-system-skill
description: Generate UI in Nothing's design language — monochrome, typographic, industrial with Swiss hierarchy and OLED blacks
triggers:
  - "use nothing design style"
  - "generate UI in nothing design language"
  - "create a nothing style component"
  - "apply nothing design system"
  - "design this with nothing aesthetic"
  - "make this look like nothing phone UI"
  - "use monochrome industrial design"
  - "apply swiss typography and segmented UI"
---

# Nothing Design System Skill

> Skill by [ara.so](https://ara.so) — Design Skills collection.

A reusable design system for Claude Code that generates UI following Nothing's visual language: monochrome palettes, Swiss typography hierarchy, industrial widgets, and OLED-optimized blacks.

## What This Does

This skill teaches AI agents to generate UI components following Nothing's design principles without repeatedly describing the same rules. It provides:

- **Three-layer visual hierarchy**: Display, body, metadata typography scales
- **Monochrome token system**: True blacks (#000000), grays, and full dark/light modes
- **Industrial components**: Segmented progress bars, mechanical toggles, dot-matrix patterns
- **Multi-platform output**: HTML/CSS, SwiftUI, React/Tailwind with consistent design tokens

## Installation

```sh
# Clone the repository
git clone https://github.com/dominikmartn/nothing-design-skill.git

# Copy to Claude Code skills directory
cp -r nothing-design-skill/nothing-design ~/.claude/skills/

# Or create skills directory if it doesn't exist
mkdir -p ~/.claude/skills
cp -r nothing-design-skill/nothing-design ~/.claude/skills/
```

Restart Claude Code to activate the skill.

## Core Design Principles

### Typography Hierarchy

**Three layers only:**

1. **Display**: Space Grotesk, 32-48px, 700 weight — Headlines, hero text
2. **Body**: Space Grotesk, 14-18px, 400-500 weight — Primary content
3. **Metadata**: Space Mono, 11-13px, 400 weight — Labels, captions, timestamps

**Dot Matrix**: Doto font for special callouts and dot-matrix aesthetic

### Color Tokens

**Dark Mode (Default)**
```css
--nothing-bg-primary: #000000;      /* True OLED black */
--nothing-bg-secondary: #0A0A0A;    /* Elevated surfaces */
--nothing-bg-tertiary: #141414;     /* Cards, panels */

--nothing-text-primary: #FFFFFF;    /* Headlines, body */
--nothing-text-secondary: #A0A0A0;  /* Supporting text */
--nothing-text-tertiary: #606060;   /* Metadata, disabled */

--nothing-border: #1F1F1F;          /* Dividers, outlines */
--nothing-accent: #FF0000;          /* Rare accent color */
```

**Light Mode**
```css
--nothing-bg-primary: #FFFFFF;
--nothing-bg-secondary: #F5F5F5;
--nothing-bg-tertiary: #EBEBEB;

--nothing-text-primary: #000000;
--nothing-text-secondary: #606060;
--nothing-text-tertiary: #A0A0A0;

--nothing-border: #E0E0E0;
--nothing-accent: #FF0000;
```

### Spacing Scale

```css
--nothing-space-xs: 4px;   /* Tight inline gaps */
--nothing-space-s: 8px;    /* Component padding */
--nothing-space-m: 16px;   /* Standard spacing */
--nothing-space-l: 24px;   /* Section gaps */
--nothing-space-xl: 40px;  /* Major sections */
```

## Component Patterns

### HTML/CSS Example: Segmented Progress Bar

```html
<div class="nothing-progress">
  <div class="nothing-progress-track">
    <div class="nothing-progress-segment"></div>
    <div class="nothing-progress-segment"></div>
    <div class="nothing-progress-segment"></div>
    <div class="nothing-progress-segment active"></div>
    <div class="nothing-progress-segment active"></div>
    <div class="nothing-progress-segment active"></div>
  </div>
  <span class="nothing-progress-label">60%</span>
</div>

<style>
.nothing-progress {
  display: flex;
  align-items: center;
  gap: var(--nothing-space-s);
}

.nothing-progress-track {
  flex: 1;
  display: grid;
  grid-auto-flow: column;
  gap: 2px;
  height: 4px;
}

.nothing-progress-segment {
  background: var(--nothing-bg-tertiary);
  border-radius: 0; /* Hard edges */
}

.nothing-progress-segment.active {
  background: var(--nothing-text-primary);
}

.nothing-progress-label {
  font-family: 'Space Mono', monospace;
  font-size: 11px;
  color: var(--nothing-text-tertiary);
  letter-spacing: 0.05em;
}
</style>
```

### React/Tailwind Example: Nothing Button

```jsx
// NothingButton.jsx
export default function NothingButton({ 
  children, 
  variant = 'primary', 
  size = 'md',
  onClick 
}) {
  const baseClasses = "uppercase tracking-wider font-medium transition-all duration-150";
  
  const variants = {
    primary: "bg-white text-black hover:bg-gray-200 active:bg-gray-300",
    secondary: "bg-transparent border border-white/20 text-white hover:bg-white/5",
    ghost: "bg-transparent text-white hover:bg-white/10"
  };
  
  const sizes = {
    sm: "px-4 py-2 text-xs",
    md: "px-6 py-3 text-sm",
    lg: "px-8 py-4 text-base"
  };
  
  return (
    <button
      className={`${baseClasses} ${variants[variant]} ${sizes[size]}`}
      onClick={onClick}
    >
      <span className="font-['Space_Grotesk']">{children}</span>
    </button>
  );
}

// Usage
<NothingButton variant="primary" size="md">
  Start Now
</NothingButton>
```

### SwiftUI Example: Mechanical Toggle

```swift
struct NothingToggle: View {
    @Binding var isOn: Bool
    
    var body: some View {
        HStack(spacing: 2) {
            Rectangle()
                .fill(isOn ? Color.white : Color(hex: "141414"))
                .frame(width: 24, height: 24)
                .overlay(
                    Text(isOn ? "I" : "")
                        .font(.custom("Space Mono", size: 11))
                        .foregroundColor(.black)
                )
            
            Rectangle()
                .fill(isOn ? Color(hex: "141414") : Color.white)
                .frame(width: 24, height: 24)
                .overlay(
                    Text(!isOn ? "O" : "")
                        .font(.custom("Space Mono", size: 11))
                        .foregroundColor(.black)
                )
        }
        .onTapGesture {
            withAnimation(.linear(duration: 0.15)) {
                isOn.toggle()
            }
        }
    }
}

// Usage
@State private var enabled = false

NothingToggle(isOn: $enabled)
```

### HTML/CSS Example: Dot Matrix Card

```html
<article class="nothing-card">
  <div class="nothing-card-header">
    <h3 class="nothing-display">Battery Status</h3>
    <span class="nothing-metadata">SYSTEM</span>
  </div>
  
  <div class="nothing-card-body">
    <div class="nothing-dotmatrix">
      <span class="nothing-dotmatrix-value">87%</span>
    </div>
    <p class="nothing-body">Estimated 4h 23m remaining</p>
  </div>
</article>

<style>
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=Space+Mono:wght@400&display=swap');

.nothing-card {
  background: var(--nothing-bg-secondary);
  border: 1px solid var(--nothing-border);
  padding: var(--nothing-space-l);
}

.nothing-card-header {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  margin-bottom: var(--nothing-space-m);
}

.nothing-display {
  font-family: 'Space Grotesk', sans-serif;
  font-size: 24px;
  font-weight: 700;
  color: var(--nothing-text-primary);
  margin: 0;
}

.nothing-metadata {
  font-family: 'Space Mono', monospace;
  font-size: 11px;
  color: var(--nothing-text-tertiary);
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

.nothing-body {
  font-family: 'Space Grotesk', sans-serif;
  font-size: 14px;
  color: var(--nothing-text-secondary);
  margin: var(--nothing-space-s) 0 0;
}

.nothing-dotmatrix {
  background: repeating-linear-gradient(
    0deg,
    transparent,
    transparent 2px,
    var(--nothing-border) 2px,
    var(--nothing-border) 3px
  );
  padding: var(--nothing-space-s);
  margin-bottom: var(--nothing-space-s);
}

.nothing-dotmatrix-value {
  font-family: 'Space Mono', monospace;
  font-size: 32px;
  font-weight: 700;
  color: var(--nothing-text-primary);
  display: block;
  text-align: center;
}
</style>
```

## Common Usage Patterns

### When to Use This Skill

1. **Dashboard UIs**: System monitoring, analytics, admin panels
2. **Device Interfaces**: Mobile apps with hardware aesthetic
3. **Portfolio Sites**: Tech-focused, minimal personal sites
4. **Documentation**: Technical docs with industrial feel
5. **CLI Visualizations**: Terminal-inspired web interfaces

### Design Workflow

1. **Start with hierarchy**: Define display, body, metadata layers first
2. **Apply tokens**: Use CSS variables or platform equivalents
3. **Add mechanical elements**: Segmented bars, toggles, hard edges
4. **Optimize for OLED**: True blacks, high contrast in dark mode
5. **Test both modes**: Ensure light mode has proper inverse tokens

## Configuration

### Font Loading (Web)

```html
<!-- In <head> -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
```

### Theme Switching

```javascript
// theme-switcher.js
const toggleTheme = () => {
  const root = document.documentElement;
  const currentTheme = root.getAttribute('data-theme');
  const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
  
  root.setAttribute('data-theme', newTheme);
  localStorage.setItem('nothing-theme', newTheme);
};

// Apply saved theme on load
const savedTheme = localStorage.getItem('nothing-theme') || 'dark';
document.documentElement.setAttribute('data-theme', savedTheme);
```

```css
:root {
  --nothing-bg-primary: #000000;
  --nothing-text-primary: #FFFFFF;
  /* ... dark mode tokens ... */
}

[data-theme="light"] {
  --nothing-bg-primary: #FFFFFF;
  --nothing-text-primary: #000000;
  /* ... light mode tokens ... */
}
```

### SwiftUI Theme Environment

```swift
// NothingTheme.swift
struct NothingTheme: EnvironmentKey {
    static let defaultValue = NothingColorScheme.dark
}

extension EnvironmentValues {
    var nothingTheme: NothingColorScheme {
        get { self[NothingTheme.self] }
        set { self[NothingTheme.self] = newValue }
    }
}

struct NothingColorScheme {
    let bgPrimary: Color
    let textPrimary: Color
    // ... other tokens ...
    
    static let dark = NothingColorScheme(
        bgPrimary: Color(hex: "000000"),
        textPrimary: Color.white
    )
    
    static let light = NothingColorScheme(
        bgPrimary: Color.white,
        textPrimary: Color(hex: "000000")
    )
}
```

## Troubleshooting

### Fonts not loading

**Problem**: Space Grotesk/Space Mono not rendering

**Solution**: Verify font URLs or use fallback stack:
```css
font-family: 'Space Grotesk', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
```

### OLED black not working

**Problem**: Background appears gray instead of true black

**Solution**: Ensure no transparency or overlays:
```css
body {
  background: #000000; /* Not #000 or rgb(0,0,0) if using color profiles */
  color: #FFFFFF;
}
```

### Segmented progress bar misaligned

**Problem**: Segments have uneven spacing

**Solution**: Use CSS Grid with fixed gap:
```css
.nothing-progress-track {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(0, 1fr));
  gap: 2px; /* Fixed gap, not margin */
}
```

### Light mode contrast too low

**Problem**: Text not readable in light mode

**Solution**: Ensure proper token mapping:
```css
[data-theme="light"] {
  --nothing-text-primary: #000000; /* Not gray */
  --nothing-text-secondary: #606060; /* Minimum WCAG AA */
}
```

## Advanced Patterns

### Animated Segment Fill

```css
@keyframes nothing-fill {
  from { transform: scaleX(0); }
  to { transform: scaleX(1); }
}

.nothing-progress-segment.active {
  animation: nothing-fill 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  transform-origin: left;
}
```

### Dot Matrix Effect (CSS)

```css
.nothing-dotmatrix-bg {
  background-image: 
    radial-gradient(circle, var(--nothing-text-tertiary) 1px, transparent 1px);
  background-size: 4px 4px;
  background-position: 0 0;
}
```

### Mechanical Counter (React)

```jsx
function NothingCounter({ value, digits = 4 }) {
  const formatted = String(value).padStart(digits, '0');
  
  return (
    <div className="flex gap-0.5">
      {formatted.split('').map((digit, i) => (
        <div 
          key={i}
          className="bg-[#141414] border border-[#1F1F1F] w-8 h-12 flex items-center justify-center"
        >
          <span className="font-['Space_Mono'] text-xl text-white">
            {digit}
          </span>
        </div>
      ))}
    </div>
  );
}
```

## Output Examples

When a user requests Nothing-style UI, generate complete, copy-paste-ready code with:

1. **Design tokens** defined as CSS variables or platform constants
2. **Typography hierarchy** using the three-layer system
3. **Monochrome palette** with proper dark/light mode support
4. **Industrial components** (segmented bars, mechanical toggles)
5. **Hard edges** — no border-radius unless specified
6. **OLED optimization** — true #000000 blacks in dark mode

Always provide working code, not pseudocode. Include necessary imports, font loading, and theme setup.
