---
name: vibefigma-figma-to-react
description: Convert Figma designs to production-ready React components with Tailwind CSS using VibeFigma
triggers:
  - convert this Figma design to React
  - import Figma component into our codebase
  - generate React code from Figma
  - turn this Figma frame into a component
  - extract this design from Figma
  - create component from Figma URL
  - convert Figma to Tailwind React
  - import Figma design as React component
---

# VibeFigma - Figma to React Converter

> Skill by [ara.so](https://ara.so) — Design Skills collection.

VibeFigma transforms Figma designs into production-ready React components with Tailwind CSS. It uses the official Figma API to extract designs and generates clean, optimized React/TypeScript code with proper styling.

## Installation

VibeFigma works best via `npx` (no installation required):

```bash
npx vibefigma --interactive
```

For programmatic use or API server:

```bash
npm install vibefigma
# or
bun install vibefigma
```

## Figma Access Token Setup

Before using VibeFigma, you need a Figma personal access token:

1. Go to [Figma Account Settings](https://www.figma.com/settings)
2. Scroll to **Personal Access Tokens**
3. Click **Generate new token**
4. Copy the token

Set it as an environment variable:

```bash
export FIGMA_TOKEN=your_figma_access_token
```

Or create a `.env` file:

```env
FIGMA_TOKEN=your_figma_access_token
```

## CLI Usage

### Interactive Mode (Recommended)

```bash
npx vibefigma --interactive
```

This guides you through:
- Entering the Figma URL
- Providing your access token (if not set)
- Choosing output paths

### Direct Conversion

```bash
npx vibefigma "https://www.figma.com/design/FILE_KEY/NAME?node-id=NODE_ID"
```

### With Options

```bash
# Custom output paths
npx vibefigma "FIGMA_URL" \
  -c ./src/components/Hero.tsx \
  -a ./public/assets

# Disable Tailwind CSS (use regular CSS)
npx vibefigma "FIGMA_URL" --no-tailwind

# Force overwrite without prompts
npx vibefigma "FIGMA_URL" --force

# With optimization and AI cleaning
npx vibefigma "FIGMA_URL" --optimize --clean
```

### Complete Options

```bash
npx vibefigma "FIGMA_URL" \
  --token YOUR_TOKEN \
  --component ./src/components \
  --assets ./public/assets \
  --optimize \
  --clean \
  --force \
  --no-tailwind \
  --no-classes \
  --no-absolute \
  --no-responsive \
  --no-fonts
```

## CLI Options Reference

| Option | Alias | Description | Default |
|--------|-------|-------------|---------|
| `--token <token>` | `-t` | Figma access token | `$FIGMA_TOKEN` |
| `--url <url>` | `-u` | Figma file/node URL | positional arg |
| `--component <path>` | `-c` | Component output path | `./src/components/[Name].tsx` |
| `--assets <dir>` | `-a` | Assets directory | `./public` |
| `--no-tailwind` | | Disable Tailwind CSS | enabled |
| `--optimize` | | Optimize components | disabled |
| `--clean` | | Use AI code cleaner | disabled |
| `--force` | `-f` | Overwrite without confirmation | disabled |
| `--no-classes` | | Don't generate CSS classes | enabled |
| `--no-absolute` | | Don't use absolute positioning | enabled |
| `--no-responsive` | | Disable responsive design | enabled |
| `--no-fonts` | | Don't include fonts | enabled |
| `--interactive` | | Force interactive mode | auto |

## Programmatic Usage (TypeScript/JavaScript)

```typescript
import { VibeFigma } from 'vibefigma';

const converter = new VibeFigma({
  figmaToken: process.env.FIGMA_TOKEN!,
  useTailwind: true,
  optimize: false,
  generateClasses: true,
  useAbsolutePositioning: true,
  responsiveDesign: true,
  includeFonts: true
});

// Convert a Figma design
const result = await converter.convert({
  figmaUrl: 'https://www.figma.com/design/FILE_KEY/NAME?node-id=NODE_ID',
  componentPath: './src/components/Hero.tsx',
  assetsDir: './public/assets'
});

console.log('Generated:', result.componentPath);
console.log('Assets:', result.assets);
```

### Conversion Result Structure

```typescript
interface ConversionResult {
  componentPath: string;      // Path to generated component
  assets: string[];           // Array of generated asset paths
  code: string;               // Generated React code
  warnings?: string[];        // Any conversion warnings
}
```

## API Server Usage

VibeFigma includes a REST API server for integration into other services.

### Starting the Server

```bash
# Development
bun run dev

# Production
bun run build
bun start
```

### Environment Variables

```env
GOOGLE_GENERATIVE_AI_API_KEY=your_google_ai_key
PORT=3000
HOST=0.0.0.0
CORS_ORIGIN=*
FIGMA_TOKEN=your_figma_token
```

### API Endpoint

**POST** `/v1/api/vibe-figma`

Request body:

```typescript
interface ConvertRequest {
  figmaUrl: string;
  token?: string;              // Optional if FIGMA_TOKEN env set
  useTailwind?: boolean;       // default: true
  optimize?: boolean;          // default: false
  clean?: boolean;             // default: false
  generateClasses?: boolean;   // default: true
  useAbsolutePositioning?: boolean; // default: true
  responsiveDesign?: boolean;  // default: true
  includeFonts?: boolean;      // default: true
}
```

Example request:

```typescript
const response = await fetch('http://localhost:3000/v1/api/vibe-figma', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    figmaUrl: 'https://www.figma.com/design/FILE_KEY/NAME?node-id=NODE_ID',
    useTailwind: true,
    optimize: true
  })
});

const result = await response.json();
console.log(result.code);      // React component code
console.log(result.assets);    // Array of asset URLs
```

## Common Patterns

### Converting Multiple Frames

```bash
# Convert each frame separately
npx vibefigma "URL?node-id=FRAME1" -c ./src/components/Header.tsx
npx vibefigma "URL?node-id=FRAME2" -c ./src/components/Footer.tsx
npx vibefigma "URL?node-id=FRAME3" -c ./src/components/Hero.tsx
```

### Batch Conversion Script

```typescript
import { VibeFigma } from 'vibefigma';

const frames = [
  { nodeId: '26-2944', name: 'Hero' },
  { nodeId: '26-2945', name: 'Features' },
  { nodeId: '26-2946', name: 'Footer' }
];

const converter = new VibeFigma({
  figmaToken: process.env.FIGMA_TOKEN!,
  useTailwind: true
});

for (const frame of frames) {
  const url = `https://www.figma.com/design/FILE_KEY/NAME?node-id=${frame.nodeId}`;
  await converter.convert({
    figmaUrl: url,
    componentPath: `./src/components/${frame.name}.tsx`,
    assetsDir: './public/assets'
  });
  console.log(`✓ Generated ${frame.name}`);
}
```

### Integration with Build Pipeline

```typescript
// scripts/import-designs.ts
import { VibeFigma } from 'vibefigma';
import { config } from 'dotenv';

config();

const converter = new VibeFigma({
  figmaToken: process.env.FIGMA_TOKEN!,
  useTailwind: true,
  optimize: true
});

async function importDesigns() {
  const designs = [
    { url: process.env.FIGMA_HERO_URL!, path: './src/components/Hero.tsx' },
    { url: process.env.FIGMA_FOOTER_URL!, path: './src/components/Footer.tsx' }
  ];

  for (const design of designs) {
    try {
      await converter.convert({
        figmaUrl: design.url,
        componentPath: design.path,
        assetsDir: './public/assets'
      });
      console.log(`✓ ${design.path}`);
    } catch (error) {
      console.error(`✗ ${design.path}:`, error);
    }
  }
}

importDesigns();
```

### Without Tailwind (Custom CSS)

```bash
# Generate component with regular CSS classes
npx vibefigma "FIGMA_URL" --no-tailwind -c ./src/components/Hero.tsx
```

This generates:

```tsx
// Hero.tsx
export const Hero = () => {
  return (
    <div className="hero-container">
      <h1 className="hero-title">Welcome</h1>
    </div>
  );
};

// Hero.css (separate file)
.hero-container {
  display: flex;
  align-items: center;
  padding: 2rem;
}

.hero-title {
  font-size: 3rem;
  font-weight: bold;
}
```

## Generated Component Structure

With Tailwind (default):

```tsx
import React from 'react';

export const ComponentName: React.FC = () => {
  return (
    <div className="relative w-full h-screen bg-white">
      <div className="absolute top-0 left-0 w-full">
        <h1 className="text-4xl font-bold text-gray-900">
          Heading
        </h1>
      </div>
      <img 
        src="/assets/image-123.png" 
        alt="Description"
        className="absolute w-64 h-64 object-cover"
      />
    </div>
  );
};
```

## Troubleshooting

### "Invalid Figma token"

**Solution**: Verify your token is correct and hasn't expired:

```bash
# Test your token
curl -H "X-Figma-Token: $FIGMA_TOKEN" \
  https://api.figma.com/v1/me
```

### "Node not found"

**Solution**: Ensure the `node-id` is in your URL:

```bash
# Correct format
https://www.figma.com/design/FILE_KEY/NAME?node-id=26-2944

# Get node ID: Right-click frame in Figma > Copy link
```

### "Permission denied"

**Solution**: Make sure you have access to the Figma file:
- File must be shared with you
- Or you must be in the same team/organization
- Or file must be public (Community file)

### AI Cleaner Requires API Key

If using `--clean`:

```bash
export GOOGLE_GENERATIVE_AI_API_KEY=your_api_key
npx vibefigma "FIGMA_URL" --clean
```

### Generated Code Has Absolute Positioning Issues

```bash
# Disable absolute positioning
npx vibefigma "FIGMA_URL" --no-absolute

# Or disable responsive design
npx vibefigma "FIGMA_URL" --no-responsive
```

### Assets Not Loading

**Solution**: Check asset paths match your public directory:

```bash
# Default: ./public
npx vibefigma "FIGMA_URL"

# Custom assets directory
npx vibefigma "FIGMA_URL" -a ./src/assets
```

Update your bundler config to serve the assets directory.

### TypeScript Errors in Generated Code

**Solution**: Ensure you have React types installed:

```bash
npm install --save-dev @types/react @types/react-dom
```

### Component Overwrites Existing File

**Solution**: Use `--force` to skip confirmation, or choose a different path:

```bash
# Skip confirmation
npx vibefigma "FIGMA_URL" --force

# Different path
npx vibefigma "FIGMA_URL" -c ./src/components/HeroV2.tsx
```

## Advanced Configuration

### Custom Component Template

```typescript
import { VibeFigma } from 'vibefigma';

const converter = new VibeFigma({
  figmaToken: process.env.FIGMA_TOKEN!,
  useTailwind: true,
  optimize: true
});

// Post-process generated code
const result = await converter.convert({
  figmaUrl: 'FIGMA_URL',
  componentPath: './src/components/Hero.tsx',
  assetsDir: './public/assets'
});

// Add custom imports or wrappers
const enhancedCode = `import { motion } from 'framer-motion';
${result.code.replace('export const', 'export const AnimatedHero = motion(').replace(/}\s*$/, ')}')}`;
```

### Integration with Storybook

```typescript
// scripts/generate-stories.ts
import { VibeFigma } from 'vibefigma';
import fs from 'fs/promises';

const converter = new VibeFigma({
  figmaToken: process.env.FIGMA_TOKEN!,
  useTailwind: true
});

const result = await converter.convert({
  figmaUrl: 'FIGMA_URL',
  componentPath: './src/components/Hero.tsx',
  assetsDir: './public/assets'
});

const story = `
import type { Meta, StoryObj } from '@storybook/react';
import { Hero } from './Hero';

const meta: Meta<typeof Hero> = {
  component: Hero,
};

export default meta;
type Story = StoryObj<typeof Hero>;

export const Default: Story = {};
`;

await fs.writeFile('./src/components/Hero.stories.tsx', story);
```

## Best Practices

1. **Use environment variables** for tokens, never commit them
2. **Use `--force`** in CI/CD pipelines to avoid prompts
3. **Version control** generated components to track design changes
4. **Review generated code** before using in production
5. **Use `--optimize`** for cleaner output
6. **Organize by feature** when setting output paths
7. **Keep Figma frames simple** for better conversion
8. **Name Figma layers** descriptively for better component names

## License

AGPL-3.0 - See LICENSE file for details.
