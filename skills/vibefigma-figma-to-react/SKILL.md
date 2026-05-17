---
name: vibefigma-figma-to-react
description: Convert Figma designs to production-ready React components with Tailwind CSS using VibeFigma CLI and API
triggers:
  - convert figma design to react
  - transform figma to tailwind components
  - export figma frames as react code
  - generate react from figma url
  - import figma design into project
  - create components from figma file
  - parse figma api and build jsx
  - figma to code conversion setup
---

# VibeFigma - Figma to React Converter

> Skill by [ara.so](https://ara.so) — Design Skills collection.

VibeFigma transforms Figma designs into production-ready React components with automatic Tailwind CSS generation. It uses the official Figma API to extract design data and generates TypeScript/React components with proper styling, assets, and responsive design patterns.

## Installation

VibeFigma can be used via npx (no installation needed) or installed globally:

```bash
# Use directly with npx (recommended)
npx vibefigma --help

# Or install globally
npm install -g vibefigma

# Or install as dev dependency
npm install --save-dev vibefigma
```

## Figma Access Token Setup

Before using VibeFigma, you need a Figma Personal Access Token:

1. Go to https://www.figma.com/settings
2. Scroll to "Personal Access Tokens"
3. Click "Generate new token"
4. Copy the token and store it securely

Set the token as an environment variable:

```bash
# Add to .env file
FIGMA_TOKEN=figd_your_figma_access_token_here

# Or export in shell
export FIGMA_TOKEN=figd_your_figma_access_token_here
```

## CLI Usage

### Interactive Mode (Easiest)

```bash
npx vibefigma --interactive
```

The CLI will prompt for:
- Figma URL
- Access token (if not in env)
- Output paths for components and assets

### Direct Command

```bash
# Basic usage with environment variable
npx vibefigma "https://www.figma.com/design/FILE_ID/DESIGN_NAME?node-id=NODE_ID"

# With explicit token
npx vibefigma "FIGMA_URL" --token YOUR_TOKEN

# Custom output paths
npx vibefigma "FIGMA_URL" \
  --component ./src/components/Hero.tsx \
  --assets ./public/images

# Force overwrite without confirmation
npx vibefigma "FIGMA_URL" --force

# Disable Tailwind CSS (use regular CSS)
npx vibefigma "FIGMA_URL" --no-tailwind

# With AI optimization
npx vibefigma "FIGMA_URL" --optimize --clean
```

### Complete CLI Options

```bash
npx vibefigma [options] [url]

Options:
  -t, --token <token>       Figma access token
  -u, --url <url>           Figma file/node URL
  -c, --component <path>    Component output path (default: ./src/components/[ComponentName].tsx)
  -a, --assets <dir>        Assets directory (default: ./public)
  --no-tailwind            Disable Tailwind CSS
  --optimize               Optimize components
  --clean                  Use AI code cleaner (requires GOOGLE_GENERATIVE_AI_API_KEY)
  --no-classes             Don't generate CSS classes
  --no-absolute            Don't use absolute positioning
  --no-responsive          Disable responsive design
  --no-fonts               Don't include fonts
  --interactive            Force interactive mode
  -f, --force              Overwrite existing files without confirmation
```

## Programmatic Usage (TypeScript/JavaScript)

```typescript
import { convertFigmaToReact } from 'vibefigma';

// Basic conversion
const result = await convertFigmaToReact({
  figmaUrl: 'https://www.figma.com/design/FILE_ID/DESIGN_NAME?node-id=NODE_ID',
  figmaToken: process.env.FIGMA_TOKEN!,
  outputDir: './src/components',
  assetsDir: './public/assets'
});

console.log('Generated component:', result.componentPath);
console.log('Assets:', result.assets);

// With custom options
const customResult = await convertFigmaToReact({
  figmaUrl: 'FIGMA_URL',
  figmaToken: process.env.FIGMA_TOKEN!,
  outputDir: './src/components',
  assetsDir: './public/images',
  options: {
    tailwind: true,           // Use Tailwind CSS
    optimize: true,           // Optimize code
    clean: false,             // AI code cleaning
    generateClasses: true,    // Generate CSS classes
    absolutePositioning: true,// Use absolute positioning
    responsive: true,         // Responsive design
    includeFonts: true,       // Include font imports
    force: false              // Overwrite without confirmation
  }
});
```

## API Server

VibeFigma includes a REST API server for backend integration:

### Starting the Server

```bash
# Install dependencies
bun install  # or npm install

# Set up environment
cat > .env << EOF
GOOGLE_GENERATIVE_AI_API_KEY=your_google_ai_key
PORT=3000
HOST=0.0.0.0
CORS_ORIGIN=*
FIGMA_TOKEN=figd_your_token_here
EOF

# Run server
bun run dev  # or npm run dev
```

### API Endpoint

```http
POST /v1/api/vibe-figma
Content-Type: application/json

{
  "figmaUrl": "https://www.figma.com/design/FILE_ID/DESIGN_NAME?node-id=NODE_ID",
  "figmaToken": "figd_your_token_here",
  "options": {
    "tailwind": true,
    "optimize": false,
    "clean": false
  }
}
```

**Response:**

```json
{
  "success": true,
  "component": "import React from 'react';\n\nexport default function Component() {\n  return (\n    <div className=\"flex flex-col items-center justify-center\">\n      ...\n    </div>\n  );\n}",
  "assets": [
    {
      "name": "logo.png",
      "url": "https://...",
      "path": "/assets/logo.png"
    }
  ],
  "metadata": {
    "componentName": "Hero",
    "hasImages": true,
    "tailwindUsed": true
  }
}
```

### Using the API with curl

```bash
curl -X POST http://localhost:3000/v1/api/vibe-figma \
  -H "Content-Type: application/json" \
  -d '{
    "figmaUrl": "YOUR_FIGMA_URL",
    "figmaToken": "'$FIGMA_TOKEN'",
    "options": {
      "tailwind": true
    }
  }'
```

### Using the API with TypeScript

```typescript
interface VibeFigmaRequest {
  figmaUrl: string;
  figmaToken: string;
  options?: {
    tailwind?: boolean;
    optimize?: boolean;
    clean?: boolean;
    generateClasses?: boolean;
    absolutePositioning?: boolean;
    responsive?: boolean;
    includeFonts?: boolean;
  };
}

async function convertFigmaDesign(request: VibeFigmaRequest) {
  const response = await fetch('http://localhost:3000/v1/api/vibe-figma', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(request)
  });
  
  if (!response.ok) {
    throw new Error(`API error: ${response.statusText}`);
  }
  
  return await response.json();
}

// Usage
const result = await convertFigmaDesign({
  figmaUrl: 'https://www.figma.com/design/...',
  figmaToken: process.env.FIGMA_TOKEN!,
  options: {
    tailwind: true,
    responsive: true
  }
});

// Save component to file
import fs from 'fs/promises';
await fs.writeFile('./src/components/Hero.tsx', result.component);
```

## Common Patterns

### Converting Multiple Figma Frames

```typescript
const figmaUrls = [
  'https://www.figma.com/design/FILE_ID?node-id=NODE_1',
  'https://www.figma.com/design/FILE_ID?node-id=NODE_2',
  'https://www.figma.com/design/FILE_ID?node-id=NODE_3'
];

for (const url of figmaUrls) {
  await convertFigmaToReact({
    figmaUrl: url,
    figmaToken: process.env.FIGMA_TOKEN!,
    outputDir: './src/components',
    assetsDir: './public/assets',
    options: { force: true }  // Auto-overwrite
  });
}
```

### Batch Processing with Custom Names

```bash
#!/bin/bash
# batch-convert.sh

declare -A components=(
  ["Hero"]="https://www.figma.com/design/FILE_ID?node-id=NODE_1"
  ["Footer"]="https://www.figma.com/design/FILE_ID?node-id=NODE_2"
  ["Navbar"]="https://www.figma.com/design/FILE_ID?node-id=NODE_3"
)

for name in "${!components[@]}"; do
  echo "Converting $name..."
  npx vibefigma "${components[$name]}" \
    --component "./src/components/${name}.tsx" \
    --assets ./public/assets \
    --force
done
```

### Integrating with Build Pipeline

```typescript
// scripts/sync-figma-designs.ts
import { convertFigmaToReact } from 'vibefigma';
import { config } from 'dotenv';

config();

const designs = [
  { name: 'Hero', nodeId: '26-2944' },
  { name: 'Features', nodeId: '27-3001' },
  { name: 'Pricing', nodeId: '28-3150' }
];

const FILE_ID = '4i8Tp5btFPRqtkYXplnfT6';
const BASE_URL = `https://www.figma.com/design/${FILE_ID}/Design-System`;

async function syncDesigns() {
  for (const design of designs) {
    console.log(`🎨 Converting ${design.name}...`);
    
    const figmaUrl = `${BASE_URL}?node-id=${design.nodeId}`;
    
    await convertFigmaToReact({
      figmaUrl,
      figmaToken: process.env.FIGMA_TOKEN!,
      outputDir: `./src/components/${design.name}`,
      assetsDir: './public/assets',
      options: {
        tailwind: true,
        optimize: true,
        force: true
      }
    });
    
    console.log(`✅ ${design.name} converted`);
  }
}

syncDesigns().catch(console.error);
```

### Custom Post-Processing

```typescript
import { convertFigmaToReact } from 'vibefigma';
import fs from 'fs/promises';
import path from 'path';

async function convertWithPostProcessing(figmaUrl: string) {
  const result = await convertFigmaToReact({
    figmaUrl,
    figmaToken: process.env.FIGMA_TOKEN!,
    outputDir: './src/components',
    assetsDir: './public/assets'
  });
  
  // Read generated component
  let componentCode = await fs.readFile(result.componentPath, 'utf-8');
  
  // Add custom imports
  componentCode = `import { cn } from '@/lib/utils';\n${componentCode}`;
  
  // Replace className with cn() helper
  componentCode = componentCode.replace(
    /className="([^"]+)"/g,
    'className={cn("$1")}'
  );
  
  // Write back
  await fs.writeFile(result.componentPath, componentCode);
  
  // Generate index file
  const componentName = path.basename(result.componentPath, '.tsx');
  const indexPath = path.join(path.dirname(result.componentPath), 'index.ts');
  await fs.writeFile(indexPath, `export { default } from './${componentName}';\n`);
  
  return result;
}
```

## Configuration

### Environment Variables

```bash
# Required for CLI and API
FIGMA_TOKEN=figd_your_figma_access_token

# Optional for API server
PORT=3000
HOST=0.0.0.0
CORS_ORIGIN=*

# Optional for AI optimization
GOOGLE_GENERATIVE_AI_API_KEY=your_google_ai_key
```

### Project Structure After Conversion

```
your-project/
├── src/
│   └── components/
│       ├── Hero.tsx          # Generated component
│       ├── Features.tsx
│       └── Pricing.tsx
├── public/
│   └── assets/
│       ├── logo.png          # Extracted images
│       ├── hero-bg.jpg
│       └── icon-feature.svg
└── .env                      # Tokens
```

## Troubleshooting

### "Invalid Figma URL"

Ensure URL format is correct:
```
https://www.figma.com/design/{FILE_ID}/{DESIGN_NAME}?node-id={NODE_ID}
```

Extract from Figma:
1. Open design in Figma
2. Right-click frame → "Copy link"
3. Use that URL

### "Unauthorized" or 403 Errors

- Verify Figma token is valid and not expired
- Ensure token has access to the file (file must be shared with you)
- Check token starts with `figd_`

```bash
# Test token
curl -H "X-Figma-Token: $FIGMA_TOKEN" \
  https://api.figma.com/v1/me
```

### No Component Generated

- Ensure node-id points to a Frame, not a Group
- Check that the frame has visible content
- Try with `--no-absolute` flag if positioning issues occur

### Tailwind Classes Not Working

Ensure Tailwind is configured in your project:

```javascript
// tailwind.config.js
module.exports = {
  content: [
    './src/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### Missing Assets

Assets are downloaded to the specified directory. Ensure:
- Directory exists or is creatable
- You have write permissions
- Images in Figma are not hidden or locked

```bash
# Create assets directory if missing
mkdir -p public/assets
```

### "File exists" Errors

Use `--force` flag to auto-overwrite:

```bash
npx vibefigma "FIGMA_URL" --force
```

Or delete existing files:

```bash
rm -rf src/components/Hero.tsx
npx vibefigma "FIGMA_URL"
```

## Advanced Usage

### Disable Specific Features

```bash
# No absolute positioning (use flexbox/grid)
npx vibefigma "FIGMA_URL" --no-absolute

# No responsive design
npx vibefigma "FIGMA_URL" --no-responsive

# No fonts
npx vibefigma "FIGMA_URL" --no-fonts

# Combine multiple flags
npx vibefigma "FIGMA_URL" \
  --no-absolute \
  --no-responsive \
  --no-fonts \
  --no-tailwind
```

### AI-Powered Optimization

Requires Google Generative AI API key:

```bash
export GOOGLE_GENERATIVE_AI_API_KEY=your_key_here

npx vibefigma "FIGMA_URL" --optimize --clean
```

This will:
- Optimize component structure
- Clean up redundant code
- Improve accessibility
- Add semantic HTML

### Custom Component Template

```typescript
import { convertFigmaToReact } from 'vibefigma';
import fs from 'fs/promises';

const result = await convertFigmaToReact({
  figmaUrl: 'FIGMA_URL',
  figmaToken: process.env.FIGMA_TOKEN!,
  outputDir: './src/components'
});

// Wrap in custom template
const template = `
import React from 'react';
import { ComponentProps } from '@/types';

interface Props extends ComponentProps {
  variant?: 'default' | 'dark';
}

export default function Component({ variant = 'default', ...props }: Props) {
  return (
    <div data-variant={variant} {...props}>
      ${result.component}
    </div>
  );
}
`;

await fs.writeFile(result.componentPath, template);
```

## Integration Examples

### Next.js App Router

```typescript
// app/actions/sync-design.ts
'use server';

import { convertFigmaToReact } from 'vibefigma';
import { revalidatePath } from 'next/cache';

export async function syncFigmaDesign(figmaUrl: string) {
  await convertFigmaToReact({
    figmaUrl,
    figmaToken: process.env.FIGMA_TOKEN!,
    outputDir: './src/components',
    assetsDir: './public/assets',
    options: { force: true }
  });
  
  revalidatePath('/');
  return { success: true };
}
```

### Vite/React Project

```typescript
// vite.config.ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [
    react(),
    {
      name: 'figma-sync',
      buildStart: async () => {
        if (process.env.SYNC_FIGMA === 'true') {
          const { convertFigmaToReact } = await import('vibefigma');
          await convertFigmaToReact({
            figmaUrl: process.env.FIGMA_URL!,
            figmaToken: process.env.FIGMA_TOKEN!,
            outputDir: './src/components',
            assetsDir: './public/assets'
          });
        }
      }
    }
  ]
});
```

This skill enables AI coding agents to help developers convert Figma designs to React components efficiently using VibeFigma's CLI, API, or programmatic interface.
