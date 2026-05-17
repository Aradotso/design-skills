---
name: figma-design-md-generator
description: Extract design system tokens and styles from Figma files to generate DESIGN.md and SKILL.md files for AI coding agents
triggers:
  - generate design.md from figma
  - extract figma design tokens
  - create design system documentation from figma
  - export figma styles to markdown
  - build design.md skill file
  - extract figma local styles and variables
  - convert figma design system to markdown
  - create ai-ready design guidelines from figma
---

# Figma DESIGN.md Generator

> Skill by [ara.so](https://ara.so) — Design Skills collection.

This Figma plugin extracts local styles, variables, and component families from Figma files and generates standardized `DESIGN.md` and `SKILL.md` files. These files provide AI coding agents with structured design system context based on the TypeUI DESIGN.md format.

## What It Does

The plugin automatically:

- Extracts local color styles and color variables from Figma files
- Captures typography styles and type scales
- Reads spacing, radius, and motion tokens
- Documents effect styles (shadows, blur)
- Lists grid styles and layout definitions
- Identifies component families and component sets
- Generates editable `DESIGN.md` (human-readable design guidelines)
- Generates editable `SKILL.md` (AI agent-ready instructions)

## Installation & Setup

### As a Figma Plugin Developer

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
- Select the `manifest.json` file from the project root
- Run **Design MD Skill Generator** from the plugins menu

### Development Commands

```bash
# Build plugin once
npm run build

# Watch mode for development
npm run watch

# Type checking
npm run typecheck
```

## Plugin Architecture

The plugin consists of two main parts:

### UI Component (`ui.tsx`)

React-based interface built with TypeScript and Tailwind CSS:

```typescript
import React, { useState, useEffect } from 'react';
import { createRoot } from 'react-dom/client';

function App() {
  const [activeTab, setActiveTab] = useState<'design' | 'skill'>('design');
  const [designMd, setDesignMd] = useState('');
  const [skillMd, setSkillMd] = useState('');

  useEffect(() => {
    window.onmessage = (event) => {
      const msg = event.data.pluginMessage;
      if (msg.type === 'generation-complete') {
        setDesignMd(msg.designMd);
        setSkillMd(msg.skillMd);
      }
    };
  }, []);

  const handleGenerate = () => {
    parent.postMessage({ pluginMessage: { type: 'generate' } }, '*');
  };

  return (
    <div className="p-4">
      <button onClick={handleGenerate}>Auto-extract & Generate</button>
      {/* Tab switching and markdown display */}
    </div>
  );
}

const root = createRoot(document.getElementById('react-page')!);
root.render(<App />);
```

### Plugin Code (`code.ts`)

Figma API interactions for extracting design tokens:

```typescript
figma.showUI(__html__, { width: 800, height: 600 });

figma.ui.onmessage = async (msg) => {
  if (msg.type === 'generate') {
    const extraction = await extractDesignTokens();
    const designMd = generateDesignMd(extraction);
    const skillMd = generateSkillMd(extraction);
    
    figma.ui.postMessage({
      type: 'generation-complete',
      designMd,
      skillMd
    });
  }
};
```

## Extracting Design Tokens

### Color Styles & Variables

```typescript
interface ColorToken {
  name: string;
  value: string;
  type: 'solid' | 'gradient';
  opacity?: number;
}

function extractColorStyles(): ColorToken[] {
  const localPaintStyles = figma.getLocalPaintStyles();
  
  return localPaintStyles.map(style => {
    const paint = style.paints[0] as SolidPaint;
    const { r, g, b } = paint.color;
    
    return {
      name: style.name,
      value: `rgba(${Math.round(r * 255)}, ${Math.round(g * 255)}, ${Math.round(b * 255)}, ${paint.opacity || 1})`,
      type: paint.type === 'SOLID' ? 'solid' : 'gradient',
      opacity: paint.opacity
    };
  });
}

function extractColorVariables(): ColorToken[] {
  const collections = figma.variables.getLocalVariableCollections();
  const tokens: ColorToken[] = [];
  
  collections.forEach(collection => {
    const variables = collection.variableIds.map(id => 
      figma.variables.getVariableById(id)
    );
    
    variables.forEach(variable => {
      if (variable?.resolvedType === 'COLOR') {
        const value = Object.values(variable.valuesByMode)[0] as RGB;
        tokens.push({
          name: variable.name,
          value: `rgb(${Math.round(value.r * 255)}, ${Math.round(value.g * 255)}, ${Math.round(value.b * 255)})`,
          type: 'solid'
        });
      }
    });
  });
  
  return tokens;
}
```

### Typography Styles

```typescript
interface TypographyToken {
  name: string;
  fontFamily: string;
  fontSize: number;
  fontWeight: number;
  lineHeight: number | 'AUTO';
  letterSpacing: number;
}

function extractTypographyStyles(): TypographyToken[] {
  const localTextStyles = figma.getLocalTextStyles();
  
  return localTextStyles.map(style => ({
    name: style.name,
    fontFamily: style.fontName.family,
    fontSize: style.fontSize as number,
    fontWeight: style.fontName.style.includes('Bold') ? 700 : 400,
    lineHeight: style.lineHeight,
    letterSpacing: style.letterSpacing as number
  }));
}
```

### Spacing & Radius Tokens

```typescript
function extractSpacingTokens(): Record<string, number> {
  const collections = figma.variables.getLocalVariableCollections();
  const spacing: Record<string, number> = {};
  
  collections.forEach(collection => {
    const variables = collection.variableIds
      .map(id => figma.variables.getVariableById(id))
      .filter(v => v?.resolvedType === 'FLOAT' && v.name.toLowerCase().includes('spacing'));
    
    variables.forEach(variable => {
      if (variable) {
        const value = Object.values(variable.valuesByMode)[0] as number;
        spacing[variable.name] = value;
      }
    });
  });
  
  return spacing;
}
```

### Effect Styles (Shadows, Blur)

```typescript
interface EffectToken {
  name: string;
  type: 'DROP_SHADOW' | 'INNER_SHADOW' | 'LAYER_BLUR';
  x?: number;
  y?: number;
  blur: number;
  spread?: number;
  color?: string;
}

function extractEffectStyles(): EffectToken[] {
  const localEffectStyles = figma.getLocalEffectStyles();
  
  return localEffectStyles.map(style => {
    const effect = style.effects[0];
    
    if (effect.type === 'DROP_SHADOW' || effect.type === 'INNER_SHADOW') {
      const { r, g, b, a } = effect.color;
      return {
        name: style.name,
        type: effect.type,
        x: effect.offset.x,
        y: effect.offset.y,
        blur: effect.radius,
        spread: effect.spread || 0,
        color: `rgba(${Math.round(r * 255)}, ${Math.round(g * 255)}, ${Math.round(b * 255)}, ${a})`
      };
    }
    
    return {
      name: style.name,
      type: 'LAYER_BLUR',
      blur: effect.radius
    };
  });
}
```

### Component Families

```typescript
function extractComponentFamilies(): string[] {
  const componentSets = figma.root.findAllWithCriteria({
    types: ['COMPONENT_SET']
  });
  
  return componentSets.map(set => set.name).sort();
}
```

## Generating DESIGN.md

```typescript
interface DesignExtraction {
  fileName: string;
  pageNames: string[];
  timestamp: string;
  variableCollections: string[];
  colors: ColorToken[];
  typography: TypographyToken[];
  spacing: Record<string, number>;
  effects: EffectToken[];
  componentFamilies: string[];
}

function generateDesignMd(data: DesignExtraction): string {
  return `# DESIGN.md

## Source
- **File**: ${data.fileName}
- **Pages**: ${data.pageNames.join(', ')}
- **Extracted**: ${data.timestamp}

## Variable Collections
${data.variableCollections.map(name => `- ${name}`).join('\n')}

## Color Tokens
${data.colors.map(c => `- **${c.name}**: \`${c.value}\``).join('\n')}

## Typography Tokens
${data.typography.map(t => `- **${t.name}**: ${t.fontFamily} ${t.fontSize}px / ${t.lineHeight}`).join('\n')}

## Spacing Tokens
${Object.entries(data.spacing).map(([name, val]) => `- **${name}**: ${val}px`).join('\n')}

## Effect Styles
${data.effects.map(e => `- **${e.name}**: ${e.type} blur:${e.blur}px`).join('\n')}

## Component Families
${data.componentFamilies.map(name => `- ${name}`).join('\n')}
`;
}
```

## Generating SKILL.md

```typescript
function generateSkillMd(data: DesignExtraction): string {
  return `# Design System Skill

## Mission
Implement UI components using the design tokens extracted from **${data.fileName}**.

## Brand
- **Product**: ${data.fileName}
- **Audience**: End users
- **Surface**: Web/Mobile interface

## Style Foundations

### Colors
${data.colors.slice(0, 5).map(c => `- ${c.name}: ${c.value}`).join('\n')}

### Typography
${data.typography.slice(0, 3).map(t => `- ${t.name}: ${t.fontFamily} ${t.fontSize}px`).join('\n')}

### Spacing Scale
${Object.entries(data.spacing).slice(0, 5).map(([k, v]) => `- ${k}: ${v}px`).join('\n')}

## Accessibility
- Follow WCAG 2.2 AA standards
- Ensure 4.5:1 contrast for text
- Provide keyboard navigation
- Use semantic HTML

## Rules: Do
- Use extracted color tokens exactly as defined
- Follow typography scale for all text
- Apply spacing tokens for layout consistency
- Reference component families for naming

## Rules: Don't
- Don't create arbitrary color values
- Don't use pixel values outside spacing scale
- Don't ignore effect styles
- Don't rename component patterns

## Component Rule Expectations
- Document all interactive states (hover, active, disabled)
- Include accessibility attributes
- Provide responsive breakpoints
- List required props and variants

## Quality Gates
- All colors must match extracted tokens
- Typography must use defined text styles
- Spacing must use spacing scale
- Effects must reference effect styles
`;
}
```

## Common Usage Patterns

### Full Extraction Workflow

```typescript
async function performFullExtraction() {
  const extraction: DesignExtraction = {
    fileName: figma.root.name,
    pageNames: figma.root.children.map(page => page.name),
    timestamp: new Date().toISOString(),
    variableCollections: figma.variables.getLocalVariableCollections()
      .map(c => c.name),
    colors: [
      ...extractColorStyles(),
      ...extractColorVariables()
    ],
    typography: extractTypographyStyles(),
    spacing: extractSpacingTokens(),
    effects: extractEffectStyles(),
    componentFamilies: extractComponentFamilies()
  };
  
  return extraction;
}

figma.ui.onmessage = async (msg) => {
  if (msg.type === 'generate') {
    const data = await performFullExtraction();
    
    figma.ui.postMessage({
      type: 'generation-complete',
      designMd: generateDesignMd(data),
      skillMd: generateSkillMd(data)
    });
  }
  
  if (msg.type === 'download') {
    // Trigger download in UI
    figma.ui.postMessage({
      type: 'trigger-download',
      filename: msg.filename,
      content: msg.content
    });
  }
};
```

### Download Handler (UI Side)

```typescript
function downloadMarkdown(filename: string, content: string) {
  const blob = new Blob([content], { type: 'text/markdown' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = filename;
  a.click();
  URL.revokeObjectURL(url);
}

window.onmessage = (event) => {
  const msg = event.data.pluginMessage;
  
  if (msg.type === 'trigger-download') {
    downloadMarkdown(msg.filename, msg.content);
  }
};
```

## Troubleshooting

### Plugin Not Loading

**Issue**: Manifest not recognized in Figma Desktop

**Solution**: Ensure `manifest.json` is valid and run `npm run build` first:

```bash
npm run build
# Then import manifest.json in Figma Desktop
```

### Empty Extraction

**Issue**: No styles or variables extracted

**Cause**: File has no local styles (only uses library styles)

**Solution**: The plugin only extracts **local** styles. Either:
- Create local styles in your file
- Publish library styles as local overrides
- Check that you're running the plugin in the correct file

### TypeScript Errors

**Issue**: Type errors during build

**Solution**: Ensure Figma plugin typings are installed:

```bash
npm install --save-dev @figma/plugin-typings
```

Add to `tsconfig.json`:

```json
{
  "compilerOptions": {
    "types": ["@figma/plugin-typings"]
  }
}
```

### Missing Color Variables

**Issue**: Color variables not appearing in output

**Check**: Variables must be in a **local** collection:

```typescript
// Only local collections are extracted
const collections = figma.variables.getLocalVariableCollections();
// Not: figma.variables.getVariableCollections() (includes libraries)
```

### UI Not Updating

**Issue**: Changes to `ui.tsx` not reflected

**Solution**: Use watch mode during development:

```bash
npm run watch
# Then reload plugin in Figma (Plugins → Development → Reload)
```

## Best Practices

1. **Extract Before Major Changes**: Run extraction before and after design updates to track token changes

2. **Version Control Generated Files**: Commit `DESIGN.md` and `SKILL.md` to track design system evolution

3. **Use with AI Agents**: Add generated `SKILL.md` to your project's `.claude` or `.cursor` skills directory for context-aware code generation

4. **Combine with TypeUI**: Reference [typeui.sh/design-skills](https://www.typeui.sh/design-skills) for standardized design system patterns

5. **Automate Extraction**: Set up GitHub Actions to extract on Figma file webhooks (requires Figma API)

## Advanced: Programmatic API Access

While primarily a Figma plugin, you can access Figma's REST API for automated extraction:

```typescript
// Requires FIGMA_ACCESS_TOKEN environment variable
const FIGMA_TOKEN = process.env.FIGMA_ACCESS_TOKEN;
const FILE_KEY = 'your-file-key';

async function fetchFigmaStyles() {
  const response = await fetch(
    `https://api.figma.com/v1/files/${FILE_KEY}/styles`,
    {
      headers: {
        'X-Figma-Token': FIGMA_TOKEN!
      }
    }
  );
  
  const data = await response.json();
  return data.meta.styles;
}
```

Use this for CI/CD pipelines that auto-generate `DESIGN.md` on design updates.
