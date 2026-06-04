---
name: figma-console-mcp-skills
description: Use Figma Console MCP Skills to export/import design tokens, analyze components, run WCAG audits, and manage Figma variables via the native Figma MCP server
triggers:
  - export design tokens from Figma to CSS variables
  - import DTCG tokens into Figma as variables
  - analyze Figma component variants and states
  - run WCAG accessibility audit on Figma design
  - generate changelog from Figma version history
  - extract design system inventory from Figma
  - convert Figma variables to Tailwind config
  - audit Figma components for accessibility
---

# Figma Console MCP Skills

> Skill by [ara.so](https://ara.so) — Design Skills collection.

A collection of 22 self-contained Markdown skills that bring powerful Figma design-systems capabilities to AI agents. Each skill packages proven workflows for the native Figma MCP server: design token export/import (DTCG, CSS, Tailwind), variable management, component analysis, WCAG linting, accessibility audits, version history, and more.

## What This Project Does

Figma Console MCP Skills provides ready-to-use playbooks and scripts that work with the **native Figma MCP server**. Instead of requiring you to install a separate MCP server, these skills leverage the official Figma MCP's `use_figma` tool (which runs Figma Plugin API JavaScript) plus a few REST API calls for features the Plugin API can't reach.

**Key capabilities:**
- **Tokens & Variables**: Export to DTCG/CSS/Tailwind/SCSS/TypeScript, import tokens, bootstrap token systems
- **Components**: Analyze variant state machines, arrange component sets, manage properties
- **Quality & A11y**: WCAG 2.2 linting, per-component accessibility scorecards, axe-core scanning
- **Versioning**: Version history diffs, changelog generation, blame tracking (requires REST API)
- **Documentation**: Generate component docs, annotations, FigJam & Slides authoring

Each skill is **self-contained** in its own folder with a `SKILL.md` playbook and `scripts/` directory.

## Installation

### Option A: Claude Desktop / claude.ai (No Terminal)

1. Navigate to any skill folder (e.g., `figma-export-tokens/`)
2. Compress the folder into a `.zip` (ensure `SKILL.md` is at the root of the zip)
3. In Claude Desktop or claude.ai, go to **Create / upload a skill**
4. Upload the zip and toggle it on
5. Repeat for each skill you want

**Note**: This method works for all `use_figma` skills but **cannot run** the 4 REST API skills (version history, changelog, blame, comments) which require a terminal.

### Option B: Claude Code / Cursor / Terminal-Based Agents

```bash
git clone https://github.com/PercentProduction/figma-console-mcp-skills-347.git
cd figma-console-mcp-skills-347

# Copy all skills to your agent's skills folder
cp -R skills/* ~/.claude/skills/

# Or copy individual skills
cp -R skills/figma-export-tokens ~/.claude/skills/
cp -R skills/figma-lint-design ~/.claude/skills/
```

For other agents (Codex, Gemini CLI), check their documentation for the skills directory location.

### Prerequisites

**For most skills (18 of 22):**
- Native Figma MCP server connected via OAuth
- No additional tokens needed

**For 4 REST API skills only** (`figma-version-history`, `figma-generate-changelog`, `figma-blame-node`, `figma-comments`):
- Terminal-capable agent (Claude Code, Cursor, etc.)
- Figma personal access token set as environment variable:

```bash
export FIGMA_TOKEN="figd_your_token_here"
```

Create a token at: https://www.figma.com/developers/api#access-tokens
Required scopes: `file:read`, `file:write` (for comments)

## Core Skills Overview

### Tokens & Variables

#### figma-export-tokens

Export Figma variables to multiple formats with alias resolution and multi-mode support.

**Natural trigger**: "Export design tokens from this Figma file to CSS variables"

**Example use_figma script** (`scripts/export-tokens-dtcg.js`):

```javascript
const collections = await figma.variables.getLocalVariableCollectionsAsync();
const output = { tokens: {} };

for (const collection of collections) {
  const variables = await Promise.all(
    collection.variableIds.map(id => figma.variables.getVariableByIdAsync(id))
  );
  
  for (const variable of variables) {
    const modes = collection.modes;
    for (const mode of modes) {
      const value = variable.valuesByMode[mode.modeId];
      const resolvedValue = await resolveValue(value, mode.modeId);
      
      output.tokens[variable.name] = {
        $type: mapTypeToDTCG(variable.resolvedType),
        $value: resolvedValue,
        $description: variable.description || undefined
      };
    }
  }
}

return JSON.stringify(output, null, 2);

async function resolveValue(value, modeId) {
  if (typeof value === 'object' && value.type === 'VARIABLE_ALIAS') {
    const aliasedVar = await figma.variables.getVariableByIdAsync(value.id);
    return `{${aliasedVar.name}}`;
  }
  return value;
}

function mapTypeToDTCG(type) {
  const map = {
    COLOR: 'color',
    FLOAT: 'number',
    STRING: 'string',
    BOOLEAN: 'boolean'
  };
  return map[type] || 'string';
}
```

**Supported formats**: DTCG (JSON), CSS Custom Properties, Tailwind v4/v3, SCSS variables, TypeScript, JSON flat

#### figma-import-tokens

Import tokens (DTCG or other formats) into Figma as variables.

**Natural trigger**: "Import these DTCG tokens into Figma as variables"

**Example use_figma script** (`scripts/import-dtcg.js`):

```javascript
const tokens = INPUT_TOKENS; // Injected by agent
const collectionName = INPUT_COLLECTION_NAME || 'Imported Tokens';

let collection = figma.variables.getLocalVariableCollections()
  .find(c => c.name === collectionName);

if (!collection) {
  collection = figma.variables.createVariableCollection(collectionName);
}

const defaultMode = collection.modes[0];

for (const [tokenName, tokenData] of Object.entries(tokens)) {
  let variable = collection.variableIds
    .map(id => figma.variables.getVariableByIdAsync(id))
    .find(v => v.name === tokenName);
  
  if (!variable) {
    const type = mapDTCGToFigmaType(tokenData.$type);
    variable = figma.variables.createVariable(tokenName, collection, type);
  }
  
  if (tokenData.$description) {
    variable.description = tokenData.$description;
  }
  
  const value = parseValue(tokenData.$value, tokenData.$type);
  variable.setValueForMode(defaultMode.modeId, value);
}

return `Imported ${Object.keys(tokens).length} tokens into ${collectionName}`;

function mapDTCGToFigmaType(dtcgType) {
  const map = {
    color: 'COLOR',
    number: 'FLOAT',
    string: 'STRING',
    boolean: 'BOOLEAN'
  };
  return map[dtcgType] || 'STRING';
}

function parseValue(value, type) {
  if (type === 'color') {
    // Parse hex to RGB
    const hex = value.replace('#', '');
    return {
      r: parseInt(hex.substr(0, 2), 16) / 255,
      g: parseInt(hex.substr(2, 2), 16) / 255,
      b: parseInt(hex.substr(4, 2), 16) / 255
    };
  }
  return value;
}
```

### Component Analysis

#### figma-analyze-component-set

Extract component variant state machines and visual diffs.

**Natural trigger**: "Analyze this component set and show me the variant states"

**Example use_figma script** (`scripts/analyze-variants.js`):

```javascript
const componentSet = figma.currentPage.selection[0];

if (componentSet.type !== 'COMPONENT_SET') {
  return 'Please select a component set';
}

const variants = componentSet.children.filter(n => n.type === 'COMPONENT');
const properties = {};

// Extract variant properties
for (const variant of variants) {
  const props = variant.variantProperties;
  for (const [key, value] of Object.entries(props)) {
    if (!properties[key]) properties[key] = new Set();
    properties[key].add(value);
  }
}

// Build state machine
const stateMachine = {};
for (const [prop, values] of Object.entries(properties)) {
  stateMachine[prop] = {
    values: Array.from(values),
    defaultValue: variants[0].variantProperties[prop]
  };
}

// Map to CSS pseudo-classes
const cssMapping = {};
if (stateMachine.State) {
  const stateMap = {
    'hover': ':hover',
    'active': ':active',
    'focus': ':focus',
    'disabled': ':disabled'
  };
  for (const state of stateMachine.State.values) {
    cssMapping[state] = stateMap[state.toLowerCase()] || `.${state}`;
  }
}

return JSON.stringify({
  componentSetName: componentSet.name,
  variantCount: variants.length,
  properties: stateMachine,
  cssMapping: cssMapping
}, null, 2);
```

### Accessibility & Quality

#### figma-lint-design

Run WCAG 2.2 and design system quality checks.

**Natural trigger**: "Run accessibility lint on this Figma design"

**Example use_figma script** (`scripts/wcag-lint.js`):

```javascript
const node = figma.currentPage.selection[0];
const issues = [];

async function lintNode(n, depth = 0) {
  // WCAG 1.4.3 - Contrast minimum (AA)
  if (n.type === 'TEXT' && n.fills && n.fills.length > 0) {
    const textFill = n.fills[0];
    const bgFill = findBackgroundFill(n);
    
    if (textFill.type === 'SOLID' && bgFill && bgFill.type === 'SOLID') {
      const contrast = calculateContrast(textFill.color, bgFill.color);
      const fontSize = n.fontSize;
      const minContrast = fontSize >= 18 ? 3 : 4.5;
      
      if (contrast < minContrast) {
        issues.push({
          node: n.name,
          wcagCriterion: '1.4.3 Contrast (Minimum)',
          level: 'AA',
          severity: 'error',
          message: `Contrast ratio ${contrast.toFixed(2)}:1 is below ${minContrast}:1 minimum`,
          recommendation: `Increase contrast to at least ${minContrast}:1`
        });
      }
    }
  }
  
  // WCAG 2.5.5 - Target Size (Level AAA)
  if (n.type === 'FRAME' && isInteractive(n)) {
    const minSize = 44;
    if (n.width < minSize || n.height < minSize) {
      issues.push({
        node: n.name,
        wcagCriterion: '2.5.5 Target Size',
        level: 'AAA',
        severity: 'warning',
        message: `Interactive element ${n.width}×${n.height}px is below 44×44px minimum`,
        recommendation: 'Increase touch target size to 44×44px minimum'
      });
    }
  }
  
  // Recurse
  if ('children' in n) {
    for (const child of n.children) {
      await lintNode(child, depth + 1);
    }
  }
}

function calculateContrast(fg, bg) {
  const l1 = relativeLuminance(fg);
  const l2 = relativeLuminance(bg);
  const lighter = Math.max(l1, l2);
  const darker = Math.min(l1, l2);
  return (lighter + 0.05) / (darker + 0.05);
}

function relativeLuminance(color) {
  const rsRGB = color.r;
  const gsRGB = color.g;
  const bsRGB = color.b;
  const r = rsRGB <= 0.03928 ? rsRGB / 12.92 : Math.pow((rsRGB + 0.055) / 1.055, 2.4);
  const g = gsRGB <= 0.03928 ? gsRGB / 12.92 : Math.pow((gsRGB + 0.055) / 1.055, 2.4);
  const b = bsRGB <= 0.03928 ? bsRGB / 12.92 : Math.pow((bsRGB + 0.055) / 1.055, 2.4);
  return 0.2126 * r + 0.7152 * g + 0.0722 * b;
}

function findBackgroundFill(node) {
  let current = node.parent;
  while (current) {
    if (current.fills && current.fills.length > 0) {
      return current.fills[0];
    }
    current = current.parent;
  }
  return null;
}

function isInteractive(node) {
  return node.name.toLowerCase().includes('button') ||
         node.name.toLowerCase().includes('link') ||
         node.reactions && node.reactions.length > 0;
}

await lintNode(node);

return JSON.stringify({
  totalIssues: issues.length,
  errors: issues.filter(i => i.severity === 'error').length,
  warnings: issues.filter(i => i.severity === 'warning').length,
  issues: issues
}, null, 2);
```

### Version History (REST API)

#### figma-version-history

List and diff Figma file versions.

**Natural trigger**: "Show me version history for this Figma file"

**Example script** (`scripts/list-versions.mjs`):

```javascript
import https from 'https';

const fileKey = process.argv[2];
const token = process.env.FIGMA_TOKEN;

if (!token) {
  console.error('FIGMA_TOKEN environment variable not set');
  process.exit(1);
}

function figmaRequest(path) {
  return new Promise((resolve, reject) => {
    const options = {
      hostname: 'api.figma.com',
      path: path,
      method: 'GET',
      headers: {
        'X-Figma-Token': token
      }
    };
    
    https.get(options, (res) => {
      let data = '';
      res.on('data', chunk => data += chunk);
      res.on('end', () => resolve(JSON.parse(data)));
    }).on('error', reject);
  });
}

const versions = await figmaRequest(`/v1/files/${fileKey}/versions`);

console.log('Recent Versions:');
for (const version of versions.versions.slice(0, 10)) {
  console.log(`- ${version.label || 'Unnamed'} (${version.id})`);
  console.log(`  Created: ${new Date(version.created_at).toLocaleString()}`);
  console.log(`  By: ${version.user.handle}`);
  console.log(`  Description: ${version.description || 'No description'}\n`);
}
```

**Usage**:
```bash
node scripts/list-versions.mjs abc123filekey
```

## Configuration

### Environment Variables

Most skills require no configuration. For the 4 REST API skills:

```bash
# Required for version history, changelog, blame, comments
export FIGMA_TOKEN="figd_..."
```

### Skill Invocation

**Natural language**: Just ask naturally, e.g.:
- "Export design tokens to Tailwind config"
- "Analyze this component's variant states"
- "Run WCAG audit on the selected frame"

**Explicit invocation**: Use skill name with slash:
- `/figma-export-tokens`
- `/figma-analyze-component-set`
- `/figma-lint-design`

## Common Patterns

### Pattern 1: Export Tokens in Multiple Formats

```javascript
// 1. Export to DTCG
// Use figma-export-tokens with format=dtcg

// 2. Convert to CSS variables
// The skill will provide CSS output:
:root {
  --color-primary: #3b82f6;
  --color-secondary: #8b5cf6;
  --spacing-sm: 8px;
  --spacing-md: 16px;
}

// 3. Convert to Tailwind v4
// The skill will provide Tailwind config:
@theme {
  --color-primary: #3b82f6;
  --color-secondary: #8b5cf6;
  --spacing-sm: 8px;
  --spacing-md: 16px;
}
```

### Pattern 2: Complete Component Documentation

```javascript
// Use figma-generate-component-doc
// It combines:
// 1. Component anatomy (layers, structure)
// 2. Bound tokens/variables
// 3. Variant states
// 4. Accessibility properties
// 5. CSS mapping

// Output is comprehensive markdown:
# Button Component

## Anatomy
- Container (Frame)
  - Label (Text)
  - Icon (optional)

## Tokens
- Background: {color.primary}
- Text: {color.on-primary}
- Padding: {spacing.md}

## States
- Default
- Hover (:hover)
- Active (:active)
- Disabled (:disabled)

## Accessibility
- Min touch target: 44×44px ✓
- Focus indicator: Present ✓
- ARIA role: button
```

### Pattern 3: Token Roundtrip (Export → Modify → Import)

```javascript
// 1. Export current tokens
// Use figma-export-tokens format=json

// 2. Modify in code
const tokens = { /* exported tokens */ };
tokens['color.primary'] = { $type: 'color', $value: '#ef4444' };

// 3. Import back
// Use figma-import-tokens with modified JSON
// Non-destructive: matches by id/key/name
```

### Pattern 4: Version Diff for Changelogs

```bash
# 1. List versions
node scripts/list-versions.mjs abc123filekey

# 2. Generate changelog between two versions
node scripts/diff-versions.mjs abc123filekey version1 version2

# Output:
## Changed Components
- Button: Updated hover state color
- Input: Added focus ring

## Added Components
- Toast notification

## Removed Components
- Legacy alert
```

## Troubleshooting

### "Cannot read property 'type' of undefined"

**Cause**: No node selected in Figma when script expects one.

**Fix**: Select a node/frame before running the skill, or modify the script to use `figma.currentPage` instead of `figma.currentPage.selection[0]`.

### "This operation requires a parent node"

**Cause**: Script tried to create/modify a node without a valid parent.

**Fix**: Ensure you're operating within a page or frame:
```javascript
const parent = figma.currentPage; // or specific frame
const newNode = parent.appendChild(figma.createRectangle());
```

### Token export returns empty object

**Cause**: No variable collections in the file, or collections are from libraries (not local).

**Fix**: 
- Check if variables exist: `figma.variables.getLocalVariableCollections()`
- For library variables, use `figma-library-variables` skill instead

### REST API skills return 401 Unauthorized

**Cause**: Missing or invalid `FIGMA_TOKEN`.

**Fix**:
```bash
# Check token is set
echo $FIGMA_TOKEN

# If empty, set it
export FIGMA_TOKEN="figd_your_token_here"

# Verify token has correct scopes at:
# https://www.figma.com/developers/api#access-tokens
```

### Script timeout on large files

**Cause**: Processing too many nodes synchronously.

**Fix**: Add pagination or limit depth:
```javascript
async function processNodes(nodes, maxDepth = 3, currentDepth = 0) {
  if (currentDepth >= maxDepth) return;
  
  for (const node of nodes) {
    await processNode(node);
    if ('children' in node) {
      await processNodes(node.children, maxDepth, currentDepth + 1);
    }
  }
}
```

### Color values not matching between export/import

**Cause**: Figma uses 0-1 RGB float values, but exports often use 0-255 or hex.

**Fix**: Normalize in both directions:
```javascript
// Export: Figma → Hex
function rgbToHex(r, g, b) {
  return '#' + [r, g, b]
    .map(x => Math.round(x * 255).toString(16).padStart(2, '0'))
    .join('');
}

// Import: Hex → Figma
function hexToRgb(hex) {
  const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
  return {
    r: parseInt(result[1], 16) / 255,
    g: parseInt(result[2], 16) / 255,
    b: parseInt(result[3], 16) / 255
  };
}
```

## Key Files Reference

- `SKILL.md` (each skill folder): Playbook with workflow steps
- `scripts/*.js`: use_figma snippets (Plugin API)
- `scripts/*.mjs`: Node.js scripts for REST API
- `scripts/*.sh`: Shell wrappers for REST operations
- `references/`: Optional background docs (not required by skills)

## Additional Resources

- Official Figma Plugin API: https://www.figma.com/plugin-docs/
- Figma REST API: https://www.figma.com/developers/api
- DTCG Spec: https://design-tokens.github.io/community-group/format/
- WCAG 2.2: https://www.w3.org/WAI/WCAG22/quickref/

## Skill Dependencies

**Always load alongside these skills:**
- `figma-use`: Official Figma Plugin API reference (prerequisite for all use_figma skills)

**No other dependencies** — each skill is self-contained with its own scripts and references.
