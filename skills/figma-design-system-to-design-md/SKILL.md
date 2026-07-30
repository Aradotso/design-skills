---
name: figma-design-system-to-design-md
description: Convert Figma design tokens into structured design.md for AI-assisted coding with Cursor, Claude Code, and Copilot
triggers:
  - convert my design tokens to design.md
  - generate design system documentation from Figma
  - create design.md from tokens
  - document my design system automatically
  - extract design tokens to markdown
  - sync Figma design system to docs
  - build design.md from CSS variables
  - parse design tokens for AI coding
---

# figma-design-system-to-design-md

> Skill by [ara.so](https://ara.so) — Design Skills collection.

A Claude Code skill plugin that automatically converts Figma design tokens into a structured `design.md` file. It extracts CSS variables, Tailwind config, theme files, and optionally pulls data from Figma MCP to create comprehensive design system documentation that AI coding tools can understand.

## What It Does

**The Problem**: Design systems live in Figma and code, but documentation is outdated or nonexistent. Developers guess at color roles, spacing scales, and typography.

**The Solution**: One command generates a complete `design.md` that stays in sync with your actual token files.

The tool:
- Detects token sources automatically (CSS variables, Tailwind config, theme files)
- Parses and classifies tokens into categories (colors, typography, spacing, etc.)
- Optionally enriches data via Figma MCP
- Generates structured design.md with semantic roles
- Works framework-agnostic (React, Vue, Svelte, etc.)

## Installation

### As Claude Code Plugin

```bash
# Via marketplace
claude plugin install figma-design-system-to-design-md

# Via GitHub
claude /plugin install https://github.com/albertzhangz10/figma-design-system-to-design-md
```

### Local Development

```bash
git clone https://github.com/albertzhangz10/figma-design-system-to-design-md.git
cd figma-design-system-to-design-md
claude --plugin-dir /path/to/figma-design-system-to-design-md
```

### Web Version (No Installation)

For non-technical users: [https://figmadesignmd.com/](https://figmadesignmd.com/)
Just paste a Figma URL and get your design.md.

## Key Commands

```bash
# Generate design.md in project root
/figma-design-system-to-design-md

# Custom output path
/figma-design-system-to-design-md ./docs/design.md

# Specify token file directory
/figma-design-system-to-design-md --tokens-dir ./src/styles
```

## Token Source Detection

The skill automatically searches for these patterns:

**CSS Tokens**:
- `**/tokens.css`
- `**/variables.css`
- `**/theme.css`
- `**/globals.css`

**JSON/JS Tokens**:
- `**/tokens.json`
- `**/tokens.ts`
- `**/tokens.js`
- `**/theme.ts`

**Framework Config**:
- `tailwind.config.js`
- `tailwind.config.ts`
- `uno.config.ts`

## What Gets Generated

| Section | Source | Auto-Generated |
|---------|--------|:--------------:|
| Colors (base + semantic roles) | Token files | ✅ |
| Typography (families, scale, weights) | Token files | ✅ |
| Spacing (scale with values) | Token files | ✅ |
| Border Radius | Token files | ✅ |
| Border Width | Token files | ✅ |
| Elevation (shadows) | Tailwind / Figma MCP | ✅ |
| Responsive (breakpoints) | Config files | ✅ |
| Components (variants, states) | Figma MCP | 🔶 Optional |
| Overview (design intent) | Manual | ✍️ |
| Do's and Don'ts | Manual | ✍️ |

## Configuration

### Enabling Figma MCP (Optional)

For enhanced component extraction and effect styles:

1. Open Figma desktop app (latest version)
2. Menu → **Preferences** → Enable **Dev Mode MCP Server**
3. Restart Claude Code
4. Open your design system file in Figma

### Project Requirements

- **Required**: Token files in your project (any format)
- **Optional**: Figma MCP Server for component data
- **Recommended**: Claude Code latest version

## Code Examples

### Example 1: CSS Variables Token File

Input (`src/styles/tokens.css`):
```css
:root {
  /* Colors - Base */
  --color-blue-50: #eff6ff;
  --color-blue-500: #3b82f6;
  --color-blue-900: #1e3a8a;
  
  /* Colors - Semantic */
  --color-text-primary: var(--color-blue-900);
  --color-surface-primary: #ffffff;
  --color-border-default: #e5e7eb;
  
  /* Typography */
  --font-sans: 'Inter', system-ui, sans-serif;
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  
  /* Spacing */
  --spacing-1: 0.25rem;
  --spacing-2: 0.5rem;
  --spacing-4: 1rem;
  --spacing-8: 2rem;
  
  /* Border Radius */
  --radius-sm: 0.125rem;
  --radius-md: 0.375rem;
  --radius-lg: 0.5rem;
}
```

Generated `design.md` (excerpt):
```markdown
## Colors

### Base Palette
- `--color-blue-50`: #eff6ff (Light accent)
- `--color-blue-500`: #3b82f6 (Primary brand)
- `--color-blue-900`: #1e3a8a (Dark text)

### Semantic Roles
- **Text Primary**: `--color-text-primary` → var(--color-blue-900)
- **Surface Primary**: `--color-surface-primary` → #ffffff
- **Border Default**: `--color-border-default` → #e5e7eb

## Typography

### Font Families
- Sans: `'Inter', system-ui, sans-serif` (`--font-sans`)

### Size Scale
- Small: 0.875rem (`--font-size-sm`)
- Base: 1rem (`--font-size-base`)
- Large: 1.125rem (`--font-size-lg`)

## Spacing Scale
- 1: 0.25rem (4px) — `--spacing-1`
- 2: 0.5rem (8px) — `--spacing-2`
- 4: 1rem (16px) — `--spacing-4`
- 8: 2rem (32px) — `--spacing-8`
```

### Example 2: Tailwind Config

Input (`tailwind.config.ts`):
```typescript
import type { Config } from 'tailwindcss'

export default {
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#eff6ff',
          500: '#3b82f6',
          900: '#1e3a8a',
        },
        semantic: {
          text: '#1e3a8a',
          surface: '#ffffff',
          border: '#e5e7eb',
        },
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', 'sans-serif'],
      },
      spacing: {
        '1': '0.25rem',
        '2': '0.5rem',
        '4': '1rem',
        '8': '2rem',
      },
      borderRadius: {
        'sm': '0.125rem',
        'md': '0.375rem',
        'lg': '0.5rem',
      },
    },
  },
} satisfies Config
```

### Example 3: TypeScript Theme Object

Input (`src/theme/tokens.ts`):
```typescript
export const tokens = {
  colors: {
    base: {
      blue50: '#eff6ff',
      blue500: '#3b82f6',
      blue900: '#1e3a8a',
    },
    semantic: {
      textPrimary: '#1e3a8a',
      surfacePrimary: '#ffffff',
      borderDefault: '#e5e7eb',
    },
  },
  typography: {
    fontFamily: {
      sans: "'Inter', system-ui, sans-serif",
    },
    fontSize: {
      sm: '0.875rem',
      base: '1rem',
      lg: '1.125rem',
    },
  },
  spacing: {
    1: '0.25rem',
    2: '0.5rem',
    4: '1rem',
    8: '2rem',
  },
  borderRadius: {
    sm: '0.125rem',
    md: '0.375rem',
    lg: '0.5rem',
  },
} as const;

export type Tokens = typeof tokens;
```

## Common Patterns

### Pattern 1: Multi-Source Token Detection

The skill automatically merges tokens from multiple sources:

```
project/
├── src/
│   ├── styles/
│   │   ├── tokens.css        ← CSS variables
│   │   └── globals.css       ← Additional variables
│   └── theme/
│       └── config.ts         ← TypeScript tokens
└── tailwind.config.js        ← Tailwind tokens
```

All sources are parsed and consolidated into one `design.md`.

### Pattern 2: Framework-Specific Usage

**React with CSS Modules**:
```tsx
import styles from './Button.module.css';

export function Button({ children }: { children: React.ReactNode }) {
  return <button className={styles.button}>{children}</button>;
}

// Button.module.css
.button {
  color: var(--color-text-primary);
  padding: var(--spacing-2) var(--spacing-4);
  border-radius: var(--radius-md);
}
```

**Vue with Tailwind**:
```vue
<template>
  <button class="bg-primary-500 text-white px-4 py-2 rounded-md">
    {{ label }}
  </button>
</template>

<script setup lang="ts">
defineProps<{ label: string }>();
</script>
```

**Svelte with CSS Variables**:
```svelte
<script lang="ts">
  export let variant: 'primary' | 'secondary' = 'primary';
</script>

<button class="btn {variant}">
  <slot />
</button>

<style>
  .btn {
    padding: var(--spacing-2) var(--spacing-4);
    border-radius: var(--radius-md);
  }
  .primary {
    background: var(--color-blue-500);
    color: white;
  }
</style>
```

### Pattern 3: Semantic Color Roles

The skill automatically classifies color tokens into semantic roles:

```css
/* Detected as "text" role */
--color-text-primary
--color-text-secondary
--text-default

/* Detected as "surface" role */
--color-surface-primary
--color-bg-default
--background-primary

/* Detected as "border" role */
--color-border-default
--border-subtle
--divider-color

/* Detected as "icon" role */
--color-icon-default
--icon-primary
```

### Pattern 4: Responsive Breakpoints

Automatically extracts breakpoint config:

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    screens: {
      sm: '640px',
      md: '768px',
      lg: '1024px',
      xl: '1280px',
    },
  },
};
```

Generated in `design.md`:
```markdown
## Responsive

### Breakpoints
- Mobile: < 640px
- Tablet: 640px (`sm`)
- Desktop: 1024px (`lg`)
- Wide: 1280px (`xl`)

### Approach
Mobile-first with `min-width` breakpoints
```

## Troubleshooting

### Issue: No tokens detected

**Symptom**: Plugin says "No token files found"

**Solutions**:
1. Verify token files exist in common paths
2. Use explicit path: `/figma-design-system-to-design-md --tokens-dir ./src/styles`
3. Check file patterns match (e.g., rename `variables.css` to `tokens.css`)

### Issue: CSS variables not parsed correctly

**Symptom**: Colors or spacing values are missing/incorrect

**Solutions**:
1. Ensure CSS variables use standard format:
   ```css
   /* ✅ Good */
   --color-blue-500: #3b82f6;
   --spacing-4: 1rem;
   
   /* ❌ Bad (calc not supported yet) */
   --spacing-4: calc(0.25rem * 4);
   ```

2. Use clear semantic naming:
   ```css
   /* ✅ Detected as semantic role */
   --color-text-primary: #000;
   
   /* ❌ Not detected as semantic */
   --black: #000;
   ```

### Issue: Figma MCP not working

**Symptom**: Component data not appearing in design.md

**Solutions**:
1. Verify Figma desktop app is latest version
2. Enable MCP Server: **Preferences → Dev Mode → Enable MCP Server**
3. Restart Claude Code after enabling MCP
4. Open the correct Figma design file
5. Check Figma file has components (not just frames)

### Issue: Tailwind config not detected

**Symptom**: Tailwind tokens missing from design.md

**Solutions**:
1. Ensure config is in project root or `config/` directory
2. Check file extension (`.js`, `.ts`, `.mjs` all supported)
3. Verify config exports `theme.extend`:
   ```typescript
   // ✅ Good
   export default {
     theme: {
       extend: { colors: {...} }
     }
   }
   
   // ❌ Bad (missing extend)
   export default {
     colors: {...}
   }
   ```

### Issue: Generated design.md is incomplete

**Symptom**: Some sections are missing

**Solutions**:
1. Some sections require manual input (Overview, Do's and Don'ts)
2. Component data requires Figma MCP (optional)
3. Elevation/shadows may need Tailwind config or MCP
4. Run with `--verbose` to see what was detected:
   ```bash
   /figma-design-system-to-design-md --verbose
   ```

## Best Practices

### Token Naming Conventions

Use clear, semantic names for better detection:

```css
/* ✅ Recommended */
--color-text-primary
--color-surface-primary
--color-border-default
--spacing-4
--font-size-base
--radius-md

/* ❌ Avoid */
--blue
--padding
--size
--round
```

### Project Structure

Organize tokens for easy detection:

```
project/
├── src/
│   └── styles/
│       ├── tokens.css          ← Main tokens
│       ├── theme-light.css     ← Light theme
│       └── theme-dark.css      ← Dark theme
├── tailwind.config.ts          ← Framework config
└── design.md                   ← Generated docs
```

### Keep Tokens DRY

Use variables that reference other variables:

```css
:root {
  /* Base palette */
  --color-blue-500: #3b82f6;
  
  /* Semantic roles */
  --color-text-link: var(--color-blue-500);
  --color-border-focus: var(--color-blue-500);
}
```

The skill will resolve references and document the relationships.

## Environment Variables

If using Figma MCP with authentication:

```bash
# .env
FIGMA_ACCESS_TOKEN=figd_your_token_here
```

Reference in code:
```typescript
const figmaToken = process.env.FIGMA_ACCESS_TOKEN;
```

**Never commit real tokens** — use `.env.local` and add to `.gitignore`.

## Advanced Usage

### Custom Token Parsers

If your tokens use a non-standard format, you can specify a custom parser pattern:

```bash
/figma-design-system-to-design-md --parser-config ./parser-config.json
```

`parser-config.json`:
```json
{
  "colorPattern": "COLOR_([A-Z_]+): '(#[0-9a-f]{6})'",
  "spacingPattern": "SPACE_([A-Z_]+): '([0-9.]+(?:rem|px))'",
  "typographyPattern": "FONT_([A-Z_]+): '([^']+)'"
}
```

### Continuous Integration

Generate design.md automatically on token changes:

```yaml
# .github/workflows/design-md.yml
name: Update design.md
on:
  push:
    paths:
      - 'src/styles/**'
      - 'tailwind.config.*'

jobs:
  update-docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: anthropics/setup-claude@v1
      - run: claude /figma-design-system-to-design-md
      - uses: stefanzweifel/git-auto-commit-action@v4
        with:
          commit_message: 'chore: update design.md'
          file_pattern: 'design.md'
```

---

**Resources**:
- [Web Version](https://figmadesignmd.com/)
- [GitHub Repository](https://github.com/albertzhangz10/figma-design-system-to-design-md)
- [Google Stitch design.md Spec](https://stitch.withgoogle.com/docs/design-md/overview)
- [MIT License](https://github.com/albertzhangz10/figma-design-system-to-design-md/blob/main/LICENSE)
