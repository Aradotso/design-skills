---
name: vibefigma-figma-to-react
description: Convert Figma designs to production-ready React components with Tailwind CSS using VibeFigma
triggers:
  - convert figma design to react
  - import figma into react component
  - generate react from figma url
  - create tailwind component from figma
  - export figma frame to typescript
  - turn figma design into code
  - figma to react conversion
  - extract react component from figma
---

# VibeFigma - Figma to React Converter

> Skill by [ara.so](https://ara.so) — Design Skills collection.

VibeFigma transforms Figma designs into production-ready React components with Tailwind CSS automatically. It uses the official Figma API to extract designs and generate optimized TypeScript/React code with proper styling.

## Installation

### As a Claude Code Skill

```bash
npx skills add vibeflowing-inc/vibe_figma --skill vibefigma
```

### As a CLI Tool

No installation needed - use with `npx`:

```bash
npx vibefigma --help
```

### As an API Server

```bash
# Clone and setup
git clone https://github.com/vibeflowing-inc/vibe_figma.git
cd vibe_figma
bun install

# Run server
bun run dev
```

## Getting a Figma Access Token

Before using VibeFigma, you need a Figma Personal Access Token:

1. Go to https://www.figma.com/settings
2. Scroll to **Personal Access Tokens**
3. Click **Generate new token**
4. Give it a name and click **Generate**
5. Copy the token immediately (it won't be shown again)
6. Store in environment variable:

```bash
export FIGMA_TOKEN=your_figma_access_token_here
```

Or use `.env` file:

```env
FIGMA_TOKEN=your_figma_access_token_here
GOOGLE_GENERATIVE_AI_API_KEY=your_google_ai_key  # Optional, for AI optimization
```

## CLI Usage

### Interactive Mode (Recommended for First-Time Users)

```bash
npx vibefigma --interactive
```

The CLI will prompt you for:
- Figma URL
- Access token (if not in env)
- Output paths

### Direct Conversion

```bash
# Basic usage with env token
npx vibefigma "https://www.figma.com/design/FILE_ID/Design-Name?node-id=NODE_ID"

# With explicit token
npx vibefigma "https://www.figma.com/design/FILE_ID/Design-Name?node-id=NODE_ID" --token YOUR_TOKEN

# Overwrite without confirmation
npx vibefigma "https://www.figma.com/design/FILE_ID/Design-Name?node-id=NODE_ID" --force
```

### Custom Output Paths

```bash
# Save to specific directory
npx vibefigma [figma_url] -c ./src/components -a ./public/assets

# Save to specific file
npx vibefigma [figma_url] -c ./src/components/Hero.tsx

# With force overwrite
npx vibefigma [figma_url] -c ./src/components/Hero.tsx --force
```

### Styling Options

```bash
# Default: Tailwind CSS enabled
npx vibefigma [figma_url]

# Disable Tailwind, use regular CSS
npx vibefigma [figma_url] --no-tailwind

# Without CSS classes
npx vibefigma [figma_url] --no-classes
```

### Advanced Options

```bash
# Full customization
npx vibefigma [figma_url] \
  --token YOUR_TOKEN \
  --component ./src/components/MyComponent.tsx \
  --assets ./public/images \
  --optimize \
  --clean \
  --force

# Disable features
npx vibefigma [figma_url] \
  --no-absolute \
  --no-responsive \
  --no-fonts
```

## CLI Options Reference

| Option | Alias | Description | Default |
|--------|-------|-------------|---------|
| `--version` | `-V` | Show version | - |
| `--token <token>` | `-t` | Figma access token | `$FIGMA_TOKEN` |
| `--url <url>` | `-u` | Figma file/node URL | - |
| `--component <path>` | `-c` | Component output path | `./src/components/[Name].tsx` |
| `--assets <dir>` | `-a` | Assets directory | `./public` |
| `--no-tailwind` | - | Disable Tailwind CSS | Tailwind enabled |
| `--optimize` | - | Optimize components | false |
| `--clean` | - | Use AI code cleaner | false |
| `--no-classes` | - | Don't generate CSS classes | Classes enabled |
| `--no-absolute` | - | Don't use absolute positioning | Absolute enabled |
| `--no-responsive` | - | Disable responsive design | Responsive enabled |
| `--no-fonts` | - | Don't include fonts | Fonts enabled |
| `--interactive` | - | Force interactive mode | false |
| `--force` | `-f` | Overwrite without confirmation | false |
| `--help` | `-h` | Show help | - |

## API Usage

### Start the Server

```bash
# Development
bun run dev

# Production
bun run start
```

Server configuration (`.env`):

```env
PORT=3000
HOST=0.0.0.0
CORS_ORIGIN=*
FIGMA_TOKEN=your_token_here
GOOGLE_GENERATIVE_AI_API_KEY=your_google_ai_key
```

### Convert Figma to React

```typescript
// POST /v1/api/vibe-figma
const response = await fetch('http://localhost:3000/v1/api/vibe-figma', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    url: 'https://www.figma.com/design/FILE_ID/Design-Name?node-id=NODE_ID',
    token: 'your_figma_token',
    options: {
      tailwind: true,
      optimize: false,
      clean: false,
      includeClasses: true,
      absolutePosition: true,
      responsive: true,
      includeFonts: true
    }
  })
});

const data = await response.json();
console.log(data.component); // React component code
console.log(data.assets);    // Array of image assets
```

### API Response Format

```typescript
{
  "component": "import React from 'react';\n\nexport default function Component() {...}",
  "assets": [
    {
      "name": "image-1.png",
      "url": "https://...",
      "data": "base64_encoded_image_data"
    }
  ]
}
```

## Common Patterns

### Pattern 1: Batch Conversion Script

```typescript
import { exec } from 'child_process';
import { promisify } from 'util';

const execAsync = promisify(exec);

const figmaUrls = [
  'https://www.figma.com/design/...?node-id=1-100',
  'https://www.figma.com/design/...?node-id=1-200',
  'https://www.figma.com/design/...?node-id=1-300',
];

async function convertAll() {
  for (const url of figmaUrls) {
    try {
      const { stdout, stderr } = await execAsync(
        `npx vibefigma "${url}" --force`
      );
      console.log(`✓ Converted: ${url}`);
      console.log(stdout);
    } catch (error) {
      console.error(`✗ Failed: ${url}`, error);
    }
  }
}

convertAll();
```

### Pattern 2: Custom Build Pipeline

```typescript
import { exec } from 'child_process';
import path from 'path';
import fs from 'fs/promises';

async function buildComponent(
  figmaUrl: string,
  componentName: string
) {
  const outputPath = path.join('./src/components', `${componentName}.tsx`);
  const assetsPath = path.join('./public/assets', componentName);
  
  // Ensure directories exist
  await fs.mkdir(path.dirname(outputPath), { recursive: true });
  await fs.mkdir(assetsPath, { recursive: true });
  
  // Convert Figma to React
  const cmd = `npx vibefigma "${figmaUrl}" \
    --component ${outputPath} \
    --assets ${assetsPath} \
    --force \
    --optimize`;
  
  return new Promise((resolve, reject) => {
    exec(cmd, (error, stdout, stderr) => {
      if (error) reject(error);
      else resolve({ stdout, stderr });
    });
  });
}

// Usage
await buildComponent(
  'https://www.figma.com/design/...?node-id=1-100',
  'Hero'
);
```

### Pattern 3: API Integration with Error Handling

```typescript
async function convertFigmaDesign(
  figmaUrl: string,
  options = {}
) {
  const API_URL = process.env.VIBEFIGMA_API_URL || 'http://localhost:3000';
  
  try {
    const response = await fetch(`${API_URL}/v1/api/vibe-figma`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        url: figmaUrl,
        token: process.env.FIGMA_TOKEN,
        options: {
          tailwind: true,
          optimize: false,
          ...options
        }
      })
    });
    
    if (!response.ok) {
      const error = await response.json();
      throw new Error(`API Error: ${error.message || response.statusText}`);
    }
    
    const { component, assets } = await response.json();
    
    // Save component
    await fs.writeFile('./src/Component.tsx', component);
    
    // Save assets
    for (const asset of assets) {
      const buffer = Buffer.from(asset.data, 'base64');
      await fs.writeFile(`./public/${asset.name}`, buffer);
    }
    
    return { component, assets };
  } catch (error) {
    console.error('Conversion failed:', error);
    throw error;
  }
}
```

### Pattern 4: Watch Figma for Changes

```typescript
import { exec } from 'child_process';
import { promisify } from 'util';

const execAsync = promisify(exec);

async function getFigmaFileVersion(fileId: string, token: string) {
  const response = await fetch(
    `https://api.figma.com/v1/files/${fileId}`,
    { headers: { 'X-Figma-Token': token } }
  );
  const data = await response.json();
  return data.version;
}

async function watchFigma(figmaUrl: string, interval = 60000) {
  const fileId = figmaUrl.match(/design\/([^\/]+)/)?.[1];
  const token = process.env.FIGMA_TOKEN!;
  let lastVersion = await getFigmaFileVersion(fileId!, token);
  
  console.log(`Watching Figma file: ${fileId}`);
  
  setInterval(async () => {
    const currentVersion = await getFigmaFileVersion(fileId!, token);
    
    if (currentVersion !== lastVersion) {
      console.log('Changes detected, regenerating...');
      await execAsync(`npx vibefigma "${figmaUrl}" --force`);
      lastVersion = currentVersion;
      console.log('✓ Components updated');
    }
  }, interval);
}
```

## Generated Component Example

When you run VibeFigma, it generates React components like this:

```tsx
import React from 'react';

export default function Hero() {
  return (
    <div className="relative w-full h-screen bg-gradient-to-r from-blue-500 to-purple-600">
      <div className="absolute inset-0 flex flex-col items-center justify-center">
        <h1 className="text-6xl font-bold text-white mb-4">
          Welcome to Our App
        </h1>
        <p className="text-xl text-white opacity-90 mb-8">
          Transform your designs into reality
        </p>
        <button className="px-8 py-4 bg-white text-blue-600 rounded-lg font-semibold hover:bg-gray-100 transition-colors">
          Get Started
        </button>
      </div>
      <img 
        src="/assets/hero-bg.png" 
        alt="Hero background"
        className="absolute inset-0 w-full h-full object-cover opacity-20"
      />
    </div>
  );
}
```

## Troubleshooting

### Invalid Figma Token

```
Error: Invalid Figma access token
```

**Solution**: Regenerate your token at https://www.figma.com/settings and update your environment variable:

```bash
export FIGMA_TOKEN=new_token_here
```

### Node Not Found

```
Error: Node ID not found in Figma file
```

**Solution**: Ensure your URL includes the correct `node-id` parameter. Right-click on a frame in Figma and select "Copy link" to get the correct URL.

### Missing Dependencies

```
Error: Cannot find module 'X'
```

**Solution**: Install dependencies:

```bash
bun install
# or
npm install
```

### Permission Denied on File Write

```
Error: EACCES: permission denied
```

**Solution**: Use `--force` flag or ensure output directory is writable:

```bash
chmod -R 755 ./src/components
npx vibefigma [url] --force
```

### API Server Not Starting

```
Error: Address already in use
```

**Solution**: Change port in `.env`:

```env
PORT=3001
```

Or kill existing process:

```bash
lsof -ti:3000 | xargs kill -9
```

### Tailwind Classes Not Working

If generated components have Tailwind classes but they're not being applied:

1. Ensure Tailwind is configured in your project
2. Add VibeFigma output paths to `tailwind.config.js`:

```javascript
module.exports = {
  content: [
    './src/**/*.{js,ts,jsx,tsx}',
    './src/components/**/*.{js,ts,jsx,tsx}', // VibeFigma output
  ],
  // ...
}
```

### Large Files Timeout

For complex designs, increase timeout:

```typescript
// In API usage
const controller = new AbortController();
const timeoutId = setTimeout(() => controller.abort(), 60000); // 60s

fetch(url, { signal: controller.signal });
```

## Configuration Files

### `.env` Example

```env
# Required
FIGMA_TOKEN=figd_your_token_here

# Optional - API Server
PORT=3000
HOST=0.0.0.0
CORS_ORIGIN=*

# Optional - AI Optimization
GOOGLE_GENERATIVE_AI_API_KEY=your_google_ai_api_key
```

### `package.json` Integration

```json
{
  "scripts": {
    "figma:import": "vibefigma",
    "figma:hero": "vibefigma https://www.figma.com/design/...?node-id=1-100 -c ./src/components/Hero.tsx --force",
    "figma:watch": "node scripts/watch-figma.js"
  },
  "devDependencies": {
    "vibefigma": "latest"
  }
}
```

## Best Practices

1. **Use Environment Variables**: Never commit tokens to version control
2. **Organize Output**: Use consistent directory structure for components and assets
3. **Version Control Assets**: Commit generated components, consider gitignoring large assets
4. **Component Naming**: Use descriptive names that match Figma frame names
5. **Incremental Updates**: Use `--force` in CI/CD but review changes locally first
6. **Optimize Images**: Post-process exported assets with image optimization tools
7. **Review Generated Code**: AI-generated code should be reviewed and adjusted as needed
8. **Type Safety**: Generated TypeScript components include proper types - preserve them

## License

AGPL-3.0 - See LICENSE file for details. VibeFigma is built by Vibeflow (https://vibeflow.ai).
