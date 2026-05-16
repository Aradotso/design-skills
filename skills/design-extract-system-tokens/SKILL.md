```markdown
---
name: design-extract-system-tokens
description: Extract complete design systems from any website into DTCG tokens, Tailwind configs, Figma variables, and multi-platform code with one command.
triggers:
  - extract design tokens from a website
  - generate a design system from live site
  - convert website to DTCG tokens
  - create Tailwind config from existing site
  - audit design system quality
  - extract figma variables from URL
  - clone a website's design language
  - compare design systems between sites
---

# Design Extract System Tokens

> Skill by [ara.so](https://ara.so) — Design Skills collection.

**designlang** (design-extract) is a headless browser-based design system extraction tool that reads any live website and outputs 17+ artifacts: W3C DTCG tokens, Tailwind configs, Figma variables, shadcn/ui themes, iOS/Android/Flutter code, component anatomy, brand voice, WCAG audits, and AI-ready prompts. It goes beyond simple token extraction to capture layout patterns, responsive behavior, interaction states, motion language, and design quality scoring.

## Installation

```bash
# Global install
npm install -g designlang

# One-off usage
npx designlang <url>

# As an agent skill
npx skills add Manavarya09/design-extract
```

**Requirements:** Node.js 20+, Playwright (auto-installed)

## Core Commands

### Basic Extraction

```bash
# Extract everything (17+ files)
npx designlang https://stripe.com

# Extract to custom directory
npx designlang https://stripe.com -o ./design-system

# Enable all features (screenshots + responsive + interactions)
npx designlang https://stripe.com --full

# Extract with dark mode
npx designlang https://stripe.com --dark

# Multi-page extraction (shared tokens + per-route variants)
npx designlang https://stripe.com --depth 5
```

### Design Grading & Analysis

```bash
# Generate shareable design report card (HTML + letter grade)
npx designlang grade https://stripe.com

# Generate SVG badge for README
npx designlang grade https://stripe.com --badge

# Head-to-head design battle
npx designlang battle stripe.com vercel.com

# Quality score with breakdown
npx designlang score https://stripe.com
```

### Design Transformation

```bash
# Restyle in another vocabulary
npx designlang remix https://stripe.com --as cyberpunk
# Options: brutalist, swiss, art-deco, cyberpunk, soft-ui, editorial

# Emit all 6 vocabularies at once
npx designlang remix https://stripe.com --all

# Recolor around new brand primary (OKLCH hue rotation)
npx designlang theme-swap https://stripe.com --primary "#ff4800"

# Fuse two designs (visuals from A, voice from B)
npx designlang pair stripe.com linear.app

# Generate full brand guidelines book (13 chapters)
npx designlang brand https://stripe.com
```

### Application & Cloning

```bash
# Clone as working Next.js starter
npx designlang clone https://stripe.com

# Auto-detect framework and inject tokens
npx designlang apply https://stripe.com -d ./my-app

# Pack everything into polished design-system directory
npx designlang pack https://stripe.com
```

### Comparison & Monitoring

```bash
# Compare multiple brands
npx designlang brands stripe.com vercel.com linear.app

# Diff two sites
npx designlang diff https://stripe.com https://vercel.com

# Visual diff (HTML report)
npx designlang visual-diff https://staging.app https://app.com

# Drift detection (CI-ready, exits non-zero on failure)
npx designlang drift https://yourapp.com --tokens ./src/tokens.json

# Lint tokens
npx designlang lint ./design-tokens.json

# Watch for changes
npx designlang watch https://yourapp.com --interval 3600

# Sync local tokens from live site
npx designlang sync https://yourapp.com

# Track design history
npx designlang history https://yourapp.com
```

### MCP Server (for Claude Code / Cursor / Windsurf)

```bash
# Start stdio MCP server
npx designlang mcp

# In Claude Code / Cursor MCP config:
# Add server with command: npx designlang mcp
```

## Key Options

```bash
--out, -o <dir>              Output directory (default: ./design-extract-output)
--full                       Enable screenshots + responsive + interactions + deep-interact
--screenshots                Capture component and full-page screenshots
--responsive                 Crawl at 4 breakpoints (mobile/tablet/desktop/wide)
--interactions               Capture hover/focus/active state transitions
--deep-interact              Auto-scroll, open menus/modals, hover CTAs before extraction
--dark                       Extract dark color scheme + light/dark diff
--depth <n>                  Crawl N internal pages for multi-page reconciliation
--platforms <csv>            Emit multi-platform code (web,ios,android,flutter,wordpress,all)
--emit-agent-rules           Generate Cursor/Claude Code/generic agent rule files
--cookie <string>            Cookie header for authenticated pages
--cookie-file <path>         Cookie file (JSON/Playwright storageState/Netscape)
--header <key:value>         Custom HTTP header
--user-agent <ua>            Override User-Agent
--insecure                   Ignore SSL/TLS certificate errors
--tokens-legacy              Use pre-v7 token format instead of DTCG
```

## Output Files Reference

Each extraction generates a timestamped directory with 17+ files:

| File | Purpose |
|------|---------|
| `*-design-language.md` | 19-section markdown — feed any LLM to recreate the design |
| `*-design-tokens.json` | W3C DTCG tokens (primitive + semantic + composite layers) |
| `*-tailwind.config.js` | Drop-in Tailwind theme |
| `*-shadcn-theme.css` | shadcn/ui globals.css variables |
| `*-figma-variables.json` | Figma Variables import (light + dark) |
| `*-variables.css` | CSS custom properties |
| `*-anatomy.tsx` | Typed React component stubs + variants |
| `*-motion-tokens.json` | Durations, easings, springs, feel fingerprint |
| `*-voice.json` | Brand voice (tone, pronouns, CTA verbs) |
| `*-prompts/` | Paste-ready prompts for v0/Lovable/Cursor/Claude |
| `*-mcp.json` | MCP server payload |
| `*-grade.html` | Shareable design report card |
| `*-grade.svg` | Shields.io-style badge |
| `*-wcag-report.json` | Contrast ratios + remediation palette |
| `*-layout-system.json` | Grid patterns, container widths, gaps |
| `*-css-health.json` | Specificity, !important usage, unused CSS |

With `--platforms ios,android,flutter,wordpress`:
- `ios/DesignTokens.swift` + `ios/Colors.swift`
- `android/tokens.kt` + `android/Theme.kt`
- `flutter/design_tokens.dart` + `flutter/theme.dart`
- `wordpress/theme.json` + block theme

## Working with Extracted Tokens

### JavaScript/TypeScript

```javascript
import tokens from './design-extract-output/stripe-com-2026-05-16-design-tokens.json';

// Access primitive tokens
const primaryColor = tokens.color.primary.value; // "#635BFF"
const baseSpacing = tokens.spacing.base.value;   // "4px"

// Semantic tokens
const buttonBg = tokens.component.button.primary.background.value;
const headingFont = tokens.typography.heading.fontFamily.value;

// Composite tokens (component-level)
const cardPadding = tokens.composite.card.padding.value;
const navHeight = tokens.composite.navigation.height.value;
```

### Tailwind Integration

```javascript
// tailwind.config.js
const designTokens = require('./design-extract-output/stripe-com-2026-05-16-tailwind.config.js');

module.exports = {
  ...designTokens,
  content: ['./src/**/*.{js,jsx,ts,tsx}'],
  // Your overrides here
};
```

### CSS Variables

```css
/* Import extracted variables */
@import './design-extract-output/stripe-com-2026-05-16-variables.css';

.my-component {
  background: var(--color-primary);
  padding: var(--spacing-lg);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-md);
}
```

### React Component Anatomy

```typescript
// Generated anatomy file provides typed component interfaces
import { ButtonAnatomy } from './design-extract-output/stripe-com-2026-05-16-anatomy';

// Use the extracted variant matrix
const MyButton: React.FC<ButtonAnatomy> = ({ variant = 'primary', size = 'md', state = 'default' }) => {
  // variant: 'primary' | 'secondary' | 'ghost'
  // size: 'sm' | 'md' | 'lg'
  // state: 'default' | 'hover' | 'focus' | 'active' | 'disabled'
  
  return <button className={`btn-${variant} btn-${size} btn-${state}`}>Click</button>;
};
```

## Authentication & Protected Pages

```bash
# Cookie string
npx designlang https://app.example.com --cookie "session=abc123; token=xyz"

# Cookie file (JSON format)
npx designlang https://app.example.com --cookie-file ./cookies.json
# cookies.json: [{"name": "session", "value": "abc123", "domain": ".example.com"}]

# Playwright storageState
npx designlang https://app.example.com --cookie-file ./playwright-state.json

# Custom headers
npx designlang https://api.example.com --header "Authorization: Bearer ${API_TOKEN}"

# Self-signed cert (dev environments)
npx designlang https://localhost:3000 --insecure
```

## Multi-Page Extraction Strategy

```bash
# Crawl 10 pages, reconcile shared vs route-specific tokens
npx designlang https://stripe.com --depth 10

# Output includes:
# - stripe-com-tokens-shared.json       (tokens used on ALL pages)
# - stripe-com-tokens-routes/home.json  (home page only)
# - stripe-com-tokens-routes/pricing.json
# - stripe-com-routes-report.md         (consistency analysis)
```

Use shared tokens for global theme, route-specific for page-level overrides.

## CI/CD Integration

### GitHub Actions Example

```yaml
name: Design Drift Check

on:
  schedule:
    - cron: '0 0 * * *' # Daily
  workflow_dispatch:

jobs:
  drift-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - uses: actions/setup-node@v3
        with:
          node-version: '20'
      
      - name: Check for design drift
        run: |
          npx designlang drift https://yourapp.com \
            --tokens ./src/design-tokens.json \
            --threshold 0.1
        # Exits non-zero if >10% token drift detected
      
      - name: Lint tokens
        run: npx designlang lint ./src/design-tokens.json
      
      - name: Visual diff (staging vs prod)
        run: |
          npx designlang visual-diff \
            https://staging.yourapp.com \
            https://yourapp.com
      
      - uses: actions/upload-artifact@v3
        if: failure()
        with:
          name: drift-report
          path: ./design-extract-output/*-drift-report.html
```

## MCP Server Integration

**Cursor / Claude Code / Windsurf configuration:**

```json
{
  "mcpServers": {
    "design-extract": {
      "command": "npx",
      "args": ["designlang", "mcp"]
    }
  }
}
```

**Agent can then:**
- `mcp://design-extract/tokens` — Read all extracted tokens
- `mcp://design-extract/components` — Query component anatomy
- `mcp://design-extract/wcag` — Get contrast reports
- `mcp://design-extract/extract` — Trigger new extraction

## Brand Guidelines Generation

```bash
# Full 13-chapter brand book (print-ready HTML)
npx designlang brand https://stripe.com

# Chapters:
# 1. Cover
# 2. About
# 3. Logo Usage
# 4. Color System
# 5. Typography
# 6. Spacing & Layout
# 7. Shape Language
# 8. Iconography
# 9. Motion Principles
# 10. Components
# 11. Voice & Tone
# 12. Accessibility
# 13. Token Reference
# 14. How to Use
```

Output: `stripe-com-brand-book.html` with dark mode toggle, print CSS, table of contents.

## Design Pairing (Fusion)

```bash
# Fuse two designs across 7 axes
npx designlang pair stripe.com linear.app

# Default: visuals from stripe.com, voice + type from linear.app
# Outputs:
# - fused design tokens
# - comparison matrix (what came from where)
# - --brand flag also emits full brand book of fused identity

# Custom fusion weights (advanced)
npx designlang pair stripe.com linear.app \
  --weights color:0.7,typography:0.3,spacing:0.5
```

## Common Patterns

### Extract → Apply → Iterate

```bash
# 1. Extract from reference site
npx designlang https://stripe.com -o ./design-system

# 2. Apply to your Next.js app
npx designlang apply https://stripe.com -d ./my-next-app

# 3. Customize tokens
# Edit ./my-next-app/design-tokens.json

# 4. Re-apply
npx designlang apply https://stripe.com -d ./my-next-app
```

### Multi-Brand Design System

```bash
# Extract primary brand
npx designlang https://brand-a.com -o ./brands/a

# Extract secondary brands
npx designlang https://brand-b.com -o ./brands/b
npx designlang https://brand-c.com -o ./brands/c

# Compare all
npx designlang brands https://brand-a.com https://brand-b.com https://brand-c.com

# Outputs matrix showing:
# - Shared tokens
# - Brand-specific overrides
# - Consistency scores
```

### Design QA Workflow

```bash
# 1. Grade current design
npx designlang grade https://yourapp.com

# 2. Generate badge
npx designlang grade https://yourapp.com --badge

# 3. Add to README
echo '![Design Score](https://designlang.app/badge/yourapp.com.svg)' >> README.md

# 4. Set up monitoring
npx designlang watch https://yourapp.com --interval 86400 # daily
```

## Troubleshooting

### Extraction fails on authenticated pages

**Solution:** Use `--cookie-file` with Playwright storageState:

```bash
# First, manually log in and save state
npx playwright codegen --save-storage=auth.json https://yourapp.com

# Then extract
npx designlang https://yourapp.com --cookie-file ./auth.json
```

### Missing hover/focus states

**Solution:** Enable deep interaction mode:

```bash
npx designlang https://yourapp.com --deep-interact
```

This auto-scrolls, opens menus/modals, and hovers over interactive elements.

### Incomplete component detection

**Solution:** Use multi-page extraction to capture more component variants:

```bash
npx designlang https://yourapp.com --depth 10 --full
```

### SSL certificate errors (localhost/dev)

**Solution:** Use `--insecure` flag:

```bash
npx designlang https://localhost:3000 --insecure
```

### Tokens missing in output

**Solution:** Check `*-routes-report.md` for reconciliation issues. Some tokens may be route-specific. Use `*-tokens-shared.json` for global tokens.

### Large sites timeout

**Solution:** Reduce depth or disable heavy features:

```bash
npx designlang https://large-site.com --depth 3 --no-screenshots
```

### Figma import fails

**Solution:** Ensure Figma Variables plugin is installed. Use the JSON import (Plugins → Variables Importer → Import JSON). If using pre-v7 tokens, add `--tokens-legacy` flag.

## Environment Variables

```bash
# Custom Playwright browser path
PLAYWRIGHT_BROWSERS_PATH=/custom/path

# Disable telemetry (if project adds it)
DESIGNLANG_TELEMETRY=false

# MCP server port (if needed)
DESIGNLANG_MCP_PORT=3000
```

## Use with AI Coding Agents

**Prompt examples:**

> "Extract design tokens from https://stripe.com and apply them to my Next.js app in ./src"

> "Generate a Tailwind config based on linear.app's design system"

> "Compare the design systems of stripe.com and vercel.com, then create a report"

> "Clone https://airbnb.com as a Next.js starter with shadcn/ui components"

> "Restyle my landing page using the cyberpunk vocabulary from designlang remix"

> "Set up design drift monitoring for https://myapp.com against ./tokens.json in CI"

Agent can chain commands:

```bash
# Extract → Grade → Apply → Clone
npx designlang https://reference.com --full && \
npx designlang grade https://reference.com && \
npx designlang apply https://reference.com -d ./my-app && \
npx designlang clone https://reference.com
```

---

**Project:** [Manavarya09/design-extract](https://github.com/Manavarya09/design-extract)  
**License:** MIT  
**Requires:** Node.js 20+, Playwright  
**Homepage:** [designlang.app](https://www.designlang.app)
```
