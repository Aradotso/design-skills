---
name: vibefigma-figma-to-react
description: Convert Figma designs to production-ready React components with Tailwind CSS using VibeFigma
triggers:
  - convert figma design to react
  - import from figma
  - figma to react component
  - generate react from figma url
  - transform figma frame to code
  - extract figma design as tailwind
  - create component from figma
  - figma design conversion
---

# VibeFigma - Figma to React Converter

> Skill by [ara.so](https://ara.so) — Design Skills collection.

VibeFigma transforms Figma designs into production-ready React components with Tailwind CSS automatically. It uses the official Figma API to extract designs and generates TypeScript React components with proper styling.

## Installation

VibeFigma can be used directly via npx without installation:

```bash
npx vibefigma --interactive
```

Or install globally:

```bash
npm install -g vibefigma
```

For use as a library in a Node.js project:

```bash
npm install vibefigma
```

## Getting a Figma Access Token

Before using VibeFigma, you need a Figma personal access token:

1. Go to https://www.figma.com/settings
2. Scroll to **Personal Access Tokens**
3. Click **Generate new token**
4. Give it a name and click **Generate**
5. Copy the token immediately (it won't be shown again)
6. Store it in environment variables or pass via `--token` flag

Set the token as an environment variable:

```bash
export FIGMA_TOKEN=your_figma_access_token
```

Or create a `.env` file:

```env
FIGMA_TOKEN=your_figma_access_token
```

## CLI Usage

### Interactive Mode (Recommended for First-Time Users)

The interactive mode guides you through the conversion process:

```bash
npx vibefigma --interactive
```

The CLI will prompt you for:
- Figma URL
- Access token (if not set via environment variable)
- Output paths for components and assets

### Direct Command Mode

Convert a Figma design with a single command:

```bash
npx vibefigma "https://www.figma.com/design/4i8Tp5btFPRqtkYXplnfT6/example?node-id=26-2944" --token YOUR_TOKEN
```

Or using environment variable for token:

```bash
export FIGMA_TOKEN=your_token
npx vibefigma "https://www.figma.com/design/4i8Tp5btFPRqtkYXplnfT6/example?node-id=26-2944"
```

### Custom Output Paths

Specify where to save components and assets:

```bash
# Save to specific directory
npx vibefigma "FIGMA_URL" -c ./src/components -a ./public/assets

# Save to specific file
npx vibefigma "FIGMA_URL" -c ./src/components/Hero.tsx

# Save with custom asset path
npx vibefigma "FIGMA_URL" -c ./src/components/Hero.tsx -a ./public/images
```

### Overwrite Existing Files

Use `--force` to skip confirmation prompts when overwriting files:

```bash
npx vibefigma "FIGMA_URL" --force

# Useful in automated workflows
npx vibefigma "FIGMA_URL" -c ./src/components/Hero.tsx --force
```

### Disable Tailwind CSS

Generate standard CSS instead of Tailwind classes:

```bash
npx vibefigma "FIGMA_URL" --no-tailwind
```

### Advanced Options

```bash
npx vibefigma "FIGMA_URL" \
  --token YOUR_TOKEN \
  --component ./src/components \
  --assets ./public/assets \
  --optimize \
  --clean \
  --force
```

## CLI Options Reference

```
Options:
  -V, --version                 Output the version number
  -t, --token <token>           Figma access token (overrides FIGMA_TOKEN env var)
  -u, --url <url>               Figma file/node URL
  -c, --component <path>        Component output path (default: ./src/components/[ComponentName].tsx)
  -a, --assets <dir>            Assets directory (default: ./public)
  --no-tailwind                 Disable Tailwind CSS (enabled by default)
  --optimize                    Optimize components
  --clean                       Use AI code cleaner (requires GOOGLE_GENERATIVE_AI_API_KEY)
  --no-classes                  Don't generate CSS classes
  --no-absolute                 Don't use absolute positioning
  --no-responsive               Disable responsive design
  --no-fonts                    Don't include fonts
  --interactive                 Force interactive mode
  -f, --force                   Overwrite existing files without confirmation
  -h, --help                    Display help for command
```

## Programmatic Usage (TypeScript/JavaScript)

### Basic Conversion

```typescript
import { convertFigmaToReact } from 'vibefigma';

const result = await convertFigmaToReact({
  figmaUrl: 'https://www.figma.com/design/4i8Tp5btFPRqtkYXplnfT6/example?node-id=26-2944',
  figmaToken: process.env.FIGMA_TOKEN,
  outputPath: './src/components',
  assetsPath: './public/assets'
});

console.log('Component saved to:', result.componentPath);
console.log('Assets saved to:', result.assetsPaths);
```

### With Custom Options

```typescript
import { convertFigmaToReact } from 'vibefigma';

const result = await convertFigmaToReact({
  figmaUrl: 'https://www.figma.com/design/4i8Tp5btFPRqtkYXplnfT6/example?node-id=26-2944',
  figmaToken: process.env.FIGMA_TOKEN,
  outputPath: './src/components/Hero.tsx',
  assetsPath: './public/images',
  options: {
    tailwind: true,
    optimize: true,
    responsive: true,
    includeFonts: true,
    useAbsolutePositioning: false,
    generateClasses: true,
    force: true // Skip overwrite confirmations
  }
});
```

### Disable Tailwind Programmatically

```typescript
const result = await convertFigmaToReact({
  figmaUrl: 'FIGMA_URL',
  figmaToken: process.env.FIGMA_TOKEN,
  outputPath: './src/components',
  options: {
    tailwind: false // Generate standard CSS
  }
});
```

### Extract Figma Data Only

```typescript
import { extractFigmaData } from 'vibefigma';

const figmaData = await extractFigmaData({
  figmaUrl: 'https://www.figma.com/design/4i8Tp5btFPRqtkYXplnfT6/example?node-id=26-2944',
  figmaToken: process.env.FIGMA_TOKEN
});

console.log('Design nodes:', figmaData.nodes);
console.log('Images:', figmaData.images);
```

## API Server Usage

VibeFigma includes a REST API server for remote conversions.

### Starting the Server

```bash
# Install dependencies
bun install

# Set environment variables
export FIGMA_TOKEN=your_token
export GOOGLE_GENERATIVE_AI_API_KEY=your_google_ai_key # Optional, for AI optimization
export PORT=3000
export HOST=0.0.0.0
export CORS_ORIGIN=*

# Run server
bun run dev
```

### Environment Variables for Server

```env
FIGMA_TOKEN=your_figma_token
GOOGLE_GENERATIVE_AI_API_KEY=your_google_ai_key
PORT=3000
HOST=0.0.0.0
CORS_ORIGIN=*
```

### API Request

```bash
curl -X POST http://localhost:3000/v1/api/vibe-figma \
  -H "Content-Type: application/json" \
  -d '{
    "figmaUrl": "https://www.figma.com/design/4i8Tp5btFPRqtkYXplnfT6/example?node-id=26-2944",
    "figmaToken": "your_token",
    "options": {
      "tailwind": true,
      "optimize": true
    }
  }'
```

### API Response

```json
{
  "success": true,
  "component": "import React from 'react';\n\nexport default function Component() {\n  return (\n    <div className=\"w-full h-screen\">\n      ...\n    </div>\n  );\n}",
  "assets": [
    {
      "name": "logo.png",
      "url": "https://...",
      "path": "./public/assets/logo.png"
    }
  ]
}
```

### JavaScript API Client

```typescript
async function convertFigmaDesign(figmaUrl: string) {
  const response = await fetch('http://localhost:3000/v1/api/vibe-figma', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      figmaUrl: figmaUrl,
      figmaToken: process.env.FIGMA_TOKEN,
      options: {
        tailwind: true,
        optimize: true,
        responsive: true
      }
    })
  });

  const result = await response.json();
  
  if (result.success) {
    // Save component to file
    fs.writeFileSync('./src/components/Component.tsx', result.component);
    
    // Download and save assets
    for (const asset of result.assets) {
      const assetData = await fetch(asset.url);
      const buffer = await assetData.arrayBuffer();
      fs.writeFileSync(asset.path, Buffer.from(buffer));
    }
  }
  
  return result;
}
```

## Common Patterns

### Converting Multiple Figma Frames

```typescript
const figmaUrls = [
  'https://www.figma.com/design/example?node-id=1-1',
  'https://www.figma.com/design/example?node-id=1-2',
  'https://www.figma.com/design/example?node-id=1-3'
];

for (const url of figmaUrls) {
  await convertFigmaToReact({
    figmaUrl: url,
    figmaToken: process.env.FIGMA_TOKEN,
    outputPath: './src/components',
    options: { force: true }
  });
}
```

### Integration with Build Pipeline

```typescript
// scripts/import-designs.ts
import { convertFigmaToReact } from 'vibefigma';

async function importDesigns() {
  const designs = [
    { url: 'FIGMA_URL_1', name: 'Hero' },
    { url: 'FIGMA_URL_2', name: 'Footer' }
  ];

  for (const design of designs) {
    console.log(`Importing ${design.name}...`);
    
    await convertFigmaToReact({
      figmaUrl: design.url,
      figmaToken: process.env.FIGMA_TOKEN,
      outputPath: `./src/components/${design.name}.tsx`,
      assetsPath: './public/assets',
      options: {
        tailwind: true,
        optimize: true,
        force: true
      }
    });
    
    console.log(`✓ ${design.name} imported`);
  }
}

importDesigns();
```

### Custom Post-Processing

```typescript
import { convertFigmaToReact } from 'vibefigma';
import { formatCode } from './utils/formatter';

async function convertAndFormat(figmaUrl: string) {
  const result = await convertFigmaToReact({
    figmaUrl,
    figmaToken: process.env.FIGMA_TOKEN,
    outputPath: './temp/component.tsx'
  });

  // Read generated component
  const componentCode = fs.readFileSync(result.componentPath, 'utf-8');
  
  // Apply custom formatting
  const formatted = await formatCode(componentCode);
  
  // Save to final location
  fs.writeFileSync('./src/components/Component.tsx', formatted);
  
  return result;
}
```

## Configuration

### Tailwind CSS Configuration

Ensure your Tailwind config includes the generated components:

```javascript
// tailwind.config.js
module.exports = {
  content: [
    './src/components/**/*.{js,ts,jsx,tsx}',
    './src/pages/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### TypeScript Configuration

```json
{
  "compilerOptions": {
    "jsx": "react-jsx",
    "module": "esnext",
    "moduleResolution": "node",
    "esModuleInterop": true,
    "strict": true
  },
  "include": ["src/**/*"]
}
```

## Troubleshooting

### Invalid Figma Token

**Error:** `Figma API authentication failed`

**Solution:** 
- Verify your token is correct
- Check token hasn't expired
- Ensure token is set via `--token` flag or `FIGMA_TOKEN` environment variable
- Generate a new token if needed

```bash
export FIGMA_TOKEN=your_new_token
```

### Cannot Access Figma File

**Error:** `Figma file not found` or `Access denied`

**Solution:**
- Ensure the Figma file is accessible with your account
- Check the file URL is correct and includes `node-id` parameter
- Verify file permissions if it's a team file

### Invalid Figma URL

**Error:** `Invalid Figma URL format`

**Solution:**
- Use the full Figma URL including `node-id`
- Format: `https://www.figma.com/design/FILE_ID/NAME?node-id=NODE_ID`
- Get the URL by right-clicking a frame in Figma > "Copy link"

### File Already Exists

**Error:** `File already exists: ./src/components/Component.tsx`

**Solution:**
- Use `--force` flag to overwrite: `npx vibefigma "URL" --force`
- Choose a different output path: `npx vibefigma "URL" -c ./src/components/NewComponent.tsx`

### Missing Dependencies

**Error:** `Cannot find module 'vibefigma'`

**Solution:**
```bash
npm install vibefigma
# or
bun install vibefigma
```

### AI Optimization Fails

**Error:** `AI optimization failed`

**Solution:**
- Ensure `GOOGLE_GENERATIVE_AI_API_KEY` is set when using `--clean` flag
- Remove `--clean` flag if you don't need AI optimization
- Check API key is valid and has sufficient quota

```bash
export GOOGLE_GENERATIVE_AI_API_KEY=your_google_api_key
```

### Asset Download Failures

**Error:** `Failed to download asset`

**Solution:**
- Check internet connection
- Verify Figma token has access to file assets
- Ensure assets directory exists and is writable
- Try running with `--force` to retry downloads

### Generated Component Not Rendering

**Issue:** Component displays incorrectly or not at all

**Solution:**
- Ensure Tailwind CSS is properly configured
- Check all imported assets are accessible
- Verify component imports in your app
- Try regenerating without optimization: `npx vibefigma "URL" --no-optimize`

### Tailwind Classes Not Applied

**Issue:** Styles not appearing in generated component

**Solution:**
- Run Tailwind build process: `npm run build:css`
- Verify Tailwind config includes generated component paths
- Clear build cache and rebuild
- Check for conflicting global styles

### Using Without Tailwind

If you prefer standard CSS:

```bash
npx vibefigma "FIGMA_URL" --no-tailwind
```

This generates inline styles or CSS modules instead of Tailwind classes.

## Best Practices

1. **Always use environment variables for tokens** - Never commit tokens to version control
2. **Use `--force` in CI/CD pipelines** - Avoid interactive prompts in automated workflows
3. **Organize output paths** - Keep components and assets in consistent directories
4. **Version control generated code** - Track changes to converted components
5. **Test generated components** - Always review and test converted designs
6. **Use specific node IDs** - Target specific frames rather than entire files for better results
7. **Optimize selectively** - Use `--optimize` and `--clean` only when needed to save API costs
