---
name: figma-design-md-generator
description: Generate DESIGN.md and SKILL.md files from Figma local styles for AI-driven development with consistent design systems
triggers:
  - "extract design tokens from Figma file"
  - "generate DESIGN.md from Figma styles"
  - "create design system documentation for AI tools"
  - "export Figma local styles to markdown"
  - "build TypeUI DESIGN.md from Figma"
  - "generate SKILL.md for design system"
  - "document Figma component families"
  - "export Figma variables and tokens"
---

# Figma DESIGN.md Generator (TypeUI)

> Skill by [ara.so](https://ara.so) — Design Skills collection.

## Overview

The `bergside/design-md-figma` plugin extracts local styles, variables, and component families from Figma files and generates structured `DESIGN.md` and `SKILL.md` files. These outputs follow the TypeUI DESIGN.md format and enable AI coding tools (Claude Code, Cursor, Codex) to build interfaces with consistent design-system blueprints.

**Key capabilities:**
- Auto-extract colors, typography, spacing, radius, effects, grids
- Parse Figma variable collections and local styles
- Generate editable DESIGN.md (human-readable guidelines)
- Generate SKILL.md (agent-ready instructions)
- Download outputs for project integration

## Installation

### As a Figma Plugin (Development)

1. **Clone and install dependencies:**

```bash
git clone https://github.com/bergside/design-md-figma.git
cd design-md-figma
npm install
```

2. **Build the plugin:**

```bash
npm run build
```

3. **Load in Figma Desktop:**
   - Open Figma Desktop
   - Navigate to **Plugins → Development → Import plugin from manifest...**
   - Select `manifest.json` from the project directory
   - Run **Design MD Skill Generator** from the plugins menu

### As a Published Plugin

Install from Figma Community:
https://www.figma.com/community/plugin/1612814320994608244/design-md-skills

## Development Commands

```bash
# Build plugin once
npm run build

# Watch mode for development
npm run watch

# Type checking
npm run typecheck
```

## Usage Workflow

### 1. Extract Design Tokens

Open a Figma file with local styles and variables, then run the plugin. The auto-extract process reads:

- **Color Tokens**: Paint styles and color variables
- **Typography Tokens**: Text styles and type scales
- **Spacing/Radius/Motion**: Layout tokens from variable collections
- **Effect Styles**: Shadows, blurs, layer effects
- **Grid Styles**: Layout grid definitions
- **Component Families**: Component-set family names

### 2. Generate DESIGN.md

The plugin produces a structured markdown file with sections:

```markdown
# Design System - [File Name]

## Source
File: [Figma File Name]
Page: [Current Page]
Extracted: [Timestamp]

## Variable Collections
- Collection Name (Mode: default)

## Color Tokens
| Token | Value | Type |
|-------|-------|------|
| primary-500 | #3B82F6 | Solid |

## Typography Tokens
| Style | Family | Size | Weight | Line Height |
|-------|--------|------|--------|-------------|
| heading-1 | Inter | 32px | 700 | 1.2 |

## Component Families
- Button
- Input
- Card
```

### 3. Generate SKILL.md

The SKILL.md output includes agent-ready instructions:

```markdown
# Design System Skill

## Mission
Implement interfaces using extracted design tokens from [File Name].

## Style Foundations
- Color palette: 8 tokens (primary, secondary, neutral, semantic)
- Typography: 6 styles (heading-1 through body-small)
- Spacing scale: 8-point grid (4px, 8px, 16px, 24px, 32px, 48px, 64px)

## Rules: Do
✓ Use only tokens defined in Variable Collections
✓ Apply text styles exactly as specified
✓ Maintain spacing consistency with 8px grid

## Rules: Don't
✗ Create arbitrary color values outside token palette
✗ Override typography line-height without justification
✗ Ignore semantic color tokens for state/feedback
```

### 4. Toggle and Download

- **Toggle View**: Switch between DESIGN.md and SKILL.md in the editor
- **Refresh**: Re-run extraction after updating Figma file
- **Download**: Save `.md` files to your project repository

## Code Integration Examples

### Using Generated DESIGN.md in CSS Variables

Convert extracted tokens to CSS custom properties:

```css
/* From DESIGN.md Color Tokens */
:root {
  --color-primary-500: #3B82F6;
  --color-primary-600: #2563EB;
  --color-neutral-100: #F3F4F6;
  --color-semantic-error: #EF4444;
  
  /* From Typography Tokens */
  --font-heading-1: 700 32px/1.2 Inter;
  --font-body: 400 16px/1.5 Inter;
  
  /* From Spacing Tokens */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 16px;
  --space-lg: 24px;
}

.button-primary {
  background: var(--color-primary-500);
  padding: var(--space-sm) var(--space-md);
  font: var(--font-body);
}
```

### Using SKILL.md with Claude Code

Place `SKILL.md` in your project root or `.ai/` directory:

```bash
# Project structure
my-app/
├── .ai/
│   └── SKILL.md          # Agent-ready design system
├── src/
│   └── components/
└── DESIGN.md             # Human-readable reference
```

Prompt Claude Code:

```
"Build a login form using the design tokens from SKILL.md. 
Include primary button, input fields, and error states."
```

Claude will reference spacing scales, color tokens, and component rules from the generated file.

### TypeScript Token Type Generation

Parse DESIGN.md programmatically to generate types:

```typescript
// scripts/generate-tokens.ts
interface ColorToken {
  name: string;
  value: string;
  type: 'Solid' | 'Gradient';
}

interface TypographyToken {
  name: string;
  family: string;
  size: string;
  weight: number;
  lineHeight: number;
}

// Read DESIGN.md and parse tables
import fs from 'fs';

const designMd = fs.readFileSync('./DESIGN.md', 'utf-8');

// Extract color tokens (parse markdown table)
const colorRegex = /\| (\S+) \| (#[A-F0-9]{6}) \| (\w+) \|/gi;
const colors: ColorToken[] = [];
let match;

while ((match = colorRegex.exec(designMd)) !== null) {
  colors.push({
    name: match[1],
    value: match[2],
    type: match[3] as 'Solid' | 'Gradient'
  });
}

// Generate TypeScript definitions
const colorTypes = colors.map(c => 
  `export const ${c.name.replace(/-/g, '_').toUpperCase()} = '${c.value}';`
).join('\n');

fs.writeFileSync('./src/tokens.ts', colorTypes);
```

### React Component with Design Tokens

```tsx
// src/components/Button.tsx
import React from 'react';
import './tokens.css'; // Generated from DESIGN.md

interface ButtonProps {
  variant: 'primary' | 'secondary';
  size: 'sm' | 'md' | 'lg';
  children: React.ReactNode;
}

export const Button: React.FC<ButtonProps> = ({ 
  variant, 
  size, 
  children 
}) => {
  // Follows spacing and typography from DESIGN.md
  const sizeMap = {
    sm: 'var(--space-xs) var(--space-sm)',
    md: 'var(--space-sm) var(--space-md)',
    lg: 'var(--space-md) var(--space-lg)'
  };

  return (
    <button
      className={`button button--${variant}`}
      style={{ padding: sizeMap[size] }}
    >
      {children}
    </button>
  );
};
```

## Configuration

### Customizing Extraction

The plugin auto-extracts all local styles. To focus on specific collections, manually edit generated files after extraction.

### TypeUI Format Customization

Generated files follow the TypeUI DESIGN.md schema. To extend sections:

1. Run plugin to generate base files
2. Edit downloaded `.md` files
3. Add custom sections (e.g., "Animation Tokens", "Iconography")
4. Commit to repository for AI tool consumption

### Environment Variables for Automation

If integrating with CI/CD:

```bash
# .env
FIGMA_FILE_KEY=your-figma-file-key
FIGMA_ACCESS_TOKEN=${FIGMA_ACCESS_TOKEN}  # Use env var, not hardcoded
```

Use Figma REST API to automate extraction outside the plugin:

```typescript
// scripts/fetch-figma-styles.ts
const response = await fetch(
  `https://api.figma.com/v1/files/${process.env.FIGMA_FILE_KEY}/styles`,
  {
    headers: {
      'X-Figma-Token': process.env.FIGMA_ACCESS_TOKEN!
    }
  }
);

const styles = await response.json();
// Process and generate DESIGN.md programmatically
```

## Common Patterns

### Pattern 1: Design-to-Code Workflow

1. Designer updates Figma file with new color tokens
2. Run plugin → Download updated `DESIGN.md`
3. Commit to Git
4. AI agent (Cursor/Claude) references updated tokens
5. Developer implements with consistent values

### Pattern 2: Multi-Brand Design Systems

Extract separate `DESIGN.md` files per brand:

```bash
figma-project/
├── brand-a/
│   ├── DESIGN.md
│   └── SKILL.md
├── brand-b/
│   ├── DESIGN.md
│   └── SKILL.md
```

Tell AI agent: *"Use brand-a/SKILL.md for this component"*

### Pattern 3: Component Library Documentation

After extracting component families, augment with usage rules:

```markdown
## Component Families
- Button

### Button Rules (Manual Addition)
- Use primary variant for main CTAs
- Disabled state reduces opacity to 0.5
- Minimum touch target: 44x44px
```

## Troubleshooting

### No Local Styles Detected

**Problem**: Plugin shows empty color/typography sections.

**Solution**: Ensure the Figma file has local styles (not just variables). Check:
- Right panel → Local styles section
- Variables defined in local collections (not libraries)

### Variable Collections Not Appearing

**Problem**: Variables exist but aren't extracted.

**Solution**: Variables must be in local collections, not published libraries. Clone library variables locally if needed.

### Generated Files Missing Sections

**Problem**: Some token categories are empty.

**Solution**: Plugin only extracts defined local styles. Add missing styles in Figma:
- Effect styles for shadows
- Grid styles for layout
- Component sets for families

### Plugin Crashes on Large Files

**Problem**: Figma file has thousands of styles, plugin times out.

**Solution**: Extract from specific pages or reduce scope:
1. Duplicate file
2. Delete unused pages
3. Re-run extraction

### TypeScript Build Errors

**Problem**: `npm run build` fails with type errors.

**Solution**: Ensure TypeScript version matches project:

```bash
npm install --save-dev typescript@^5.0.0
npm run typecheck  # Verify before build
```

### Downloaded Files Have Encoding Issues

**Problem**: Special characters render incorrectly.

**Solution**: Ensure UTF-8 encoding when saving:

```typescript
// In plugin code (ui.html)
const blob = new Blob([content], { type: 'text/markdown;charset=utf-8' });
```

## Advanced Usage

### Programmatic Parsing of DESIGN.md

```typescript
// Parse generated DESIGN.md for automation
import { readFileSync } from 'fs';

interface ParsedDesign {
  colors: Map<string, string>;
  typography: Map<string, any>;
  spacing: number[];
}

function parseDesignMd(filePath: string): ParsedDesign {
  const content = readFileSync(filePath, 'utf-8');
  
  // Extract color tokens
  const colorSection = content.match(
    /## Color Tokens\n\n\| Token.*?\n([\s\S]*?)(?=\n## )/
  );
  const colors = new Map<string, string>();
  
  if (colorSection) {
    const rows = colorSection[1].trim().split('\n');
    rows.forEach(row => {
      const [, token, value] = row.split('|').map(s => s.trim());
      if (token && value) colors.set(token, value);
    });
  }
  
  return { colors, typography: new Map(), spacing: [] };
}

const design = parseDesignMd('./DESIGN.md');
console.log(design.colors.get('primary-500')); // #3B82F6
```

## Related Resources

- [TypeUI DESIGN.md Specification](https://www.typeui.sh/design-md)
- [Curated Design Skills](https://www.typeui.sh/design-skills)
- [Figma Plugin API](https://www.figma.com/plugin-docs/)
