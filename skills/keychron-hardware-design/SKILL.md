---
name: keychron-hardware-design
description: Access and work with Keychron keyboard and mouse CAD files (STEP, DXF, DWG, PDF) for hardware design, remixing, and compatible accessories
triggers:
  - open keychron cad files
  - work with keychron hardware designs
  - modify keychron keyboard case
  - design keychron compatible accessories
  - remix keychron plate design
  - extract keychron dimensions
  - create keychron keyboard mod
  - 3d print keychron parts
---

# Keychron Hardware Design Skill

> Skill by [ara.so](https://ara.so) — Design Skills collection

This skill enables AI agents to work with Keychron's industrial design files for 135+ keyboard and mouse models. Access production-grade CAD assets in STEP, DXF, DWG, and PDF formats to study hardware design, create remixes, design compatible accessories, or extract dimensions for custom builds.

## What This Project Provides

- **Production CAD files**: Case, plate, stabilizer, encoder, and full assembly models
- **Multiple formats**: STEP (3D), DXF/DWG (2D), PDF (reference drawings)
- **135+ models**: Q, Q Pro, Q HE, Q Max, K Pro, K Max, K HE, V Max, C Pro, mice (M/G series)
- **Component libraries**: Keycap profiles (Cherry, OSA, KSA, LSA, MDA, OEM)
- **Source-available license**: Personal/educational use allowed; original accessories exempt from commercial restrictions

## Repository Structure

```
keychron-keyboards-hardware-design/
├── Q-Series/              # Q0–Q12, Q60, Q65
├── Q-Pro-Series/          # Q1 Pro–Q14 Pro
├── Q-HE-Series/           # Hall Effect models
├── Q-Max-Series/          # Q0 Max, Q1 Max, Q2 Max, etc.
├── K-Pro-Series/          # K1 Pro–K17 Pro
├── K-Max-Series/          # K0 Max–K17 Max
├── K-HE-Series/           # K2 HE, K4 HE, K6 HE, K8 HE, K10 HE
├── V-Max-Series/          # V1 Max–V10 Max
├── C-Pro-8K-Series/       # C1, C2, C3 Pro 8K
├── P-HE-Series/           # Lemokey P1 HE, P2 HE, P3 HE
├── L-Series/              # L1, L3
├── Mice/                  # M1–M7, G1, G2
├── Keycap Profiles/       # Cherry, OSA, KSA, LSA, MDA, OEM
└── docs/                  # Guides and reference
```

## Installation

```bash
# Clone the repository
git clone https://github.com/Keychron/Keychron-Keyboards-Hardware-Design.git
cd Keychron-Keyboards-Hardware-Design
```

## File Format Guide

### STEP Files (.stp, .step)
- **Use for**: 3D modeling, assembly inspection, CAD remixing
- **Compatible with**: FreeCAD, Fusion 360, SolidWorks, Rhino, Blender (with plugins)
- **Typical files**: `*-Case.stp`, `*-Plate.stp`, `*-Full-Model.stp`

```python
# Example: Load STEP file in FreeCAD (Python console)
import FreeCAD
import Part

# Open a plate design
doc = FreeCAD.newDocument("KeychronPlate")
Part.insert("Q-Series/Q1/Q1-Plate.stp", doc.Name)
FreeCAD.ActiveDocument.recompute()
```

### DXF/DWG Files (.dxf, .dwg)
- **Use for**: 2D laser cutting, CNC machining, dimension extraction
- **Compatible with**: AutoCAD, LibreCAD, QCAD, Inkscape, LaserWeb

```python
# Example: Parse DXF dimensions with ezdxf (Python)
import ezdxf

# Load plate DXF
doc = ezdxf.readfile("Q-Pro-Series/Q1-Pro/Q1-Pro-Plate.dxf")
msp = doc.modelspace()

# Extract all LINE entities (plate outline)
for entity in msp.query('LINE'):
    print(f"Line from {entity.dxf.start} to {entity.dxf.end}")

# Get bounding box
bbox = ezdxf.bbox.extents(msp)
print(f"Plate dimensions: {bbox.size}")
```

### PDF Files (.pdf)
- **Use for**: Quick reference, dimension verification
- **Extract data**: Use `pdfplumber` or manual measurement

```python
# Example: Extract text/tables from PDF documentation
import pdfplumber

with pdfplumber.open("docs/Q1-Pro-Specs.pdf") as pdf:
    first_page = pdf.pages[0]
    text = first_page.extract_text()
    tables = first_page.extract_tables()
    print(text)
```

## Common Workflows

### 1. Extract Keyboard Dimensions

```python
# Using Blender Python API to measure STEP import
import bpy
import bmesh

# Import STEP (requires CAD Sketcher add-on or similar)
bpy.ops.import_scene.step(filepath="K-Pro-Series/K8-Pro/K8-Pro-Case.stp")

# Get bounding box
obj = bpy.context.selected_objects[0]
bbox_corners = [obj.matrix_world @ Vector(corner) for corner in obj.bound_box]

# Calculate dimensions
from mathutils import Vector
dims = obj.dimensions
print(f"Case dimensions: {dims.x:.2f} × {dims.y:.2f} × {dims.z:.2f} mm")
```

### 2. Modify Plate for Custom Layout

```python
# Using CadQuery to load and modify STEP plate
import cadquery as cq

# Load existing plate
plate = cq.importers.importStep("Q-Series/Q3/Q3-Plate.stp")

# Add custom switch cutout (14mm × 14mm Cherry MX)
result = (plate
    .faces(">Z")  # Top face
    .workplane()
    .center(19.05, 0)  # 1U offset (standard 19.05mm spacing)
    .rect(14, 14)
    .cutThruAll()
)

# Export modified plate
cq.exporters.export(result, "Q3-Plate-Custom.step")
```

### 3. Design Compatible Wrist Rest

```python
# Using FreeCAD scripting
import FreeCAD
import Part
import Draft

# Load keyboard case for reference
doc = FreeCAD.newDocument("WristRest")
Part.insert("K-Max-Series/K8-Max/K8-Max-Case.stp", doc.Name)

# Get front edge position
case = doc.Objects[0]
bbox = case.Shape.BoundBox
front_y = bbox.YMin

# Create wrist rest profile
points = [
    FreeCAD.Vector(0, front_y - 10, 0),
    FreeCAD.Vector(0, front_y - 80, 15),
    FreeCAD.Vector(0, front_y - 90, 10),
    FreeCAD.Vector(0, front_y - 100, 0)
]
spline = Draft.makeBSpline(points, closed=False)

# Extrude along keyboard width
wrist_rest = Part.makeSweep(spline.Shape, 
                            Part.makeLine((0, 0, 0), (bbox.XLength, 0, 0)))

# Save as STEP
Part.export([wrist_rest], "K8-Max-WristRest.step")
```

### 4. Generate Laser Cutting Files from DXF

```bash
# Using QCAD CLI to convert DXF to SVG for laser cutter
qcad -command "open Q1-Pro-Plate.dxf; export Q1-Pro-Plate.svg; quit"

# Or use Inkscape for conversion
inkscape Q1-Pro-Plate.dxf --export-filename=Q1-Pro-Plate.svg
```

### 5. 3D Print Custom Knob

```python
# Using OpenSCAD via Python (using SolidPython2)
from solid2 import *
from solid2.extensions.bosl2 import *

# Load reference knob STEP (for dimensions)
# Then create parametric replacement
knob_diameter = 30  # mm
knob_height = 15

knob = cylinder(d=knob_diameter, h=knob_height)
shaft_hole = cylinder(d=6, h=knob_height + 2)  # D-shaft encoder
knob_with_hole = knob - shaft_hole

# Add grip texture
for i in range(20):
    angle = i * 360 / 20
    grip = (rotate([0, 0, angle]) 
            * translate([knob_diameter/2 - 1, 0, 0])
            * cylinder(d=2, h=knob_height))
    knob_with_hole -= grip

# Export for 3D printing
scad_render_to_file(knob_with_hole, "custom-knob.scad")
```

## Analyzing Stabilizer Mounting

```python
# Extract stabilizer hole positions from plate DXF
import ezdxf
from collections import defaultdict

doc = ezdxf.readfile("K-Pro-Series/K2-Pro/K2-Pro-Plate.dxf")
msp = doc.modelspace()

# Find circular holes (likely stabilizer mounts)
holes = defaultdict(list)
for circle in msp.query('CIRCLE'):
    diameter = circle.dxf.radius * 2
    center = circle.dxf.center
    holes[round(diameter, 1)].append((center.x, center.y))

# Typical stabilizer wire hole: ~4mm
stab_holes = holes.get(4.0, [])
print(f"Found {len(stab_holes)} stabilizer mounting holes")

# Calculate spacing for 6.25U spacebar (standard)
if len(stab_holes) >= 2:
    spacing = abs(stab_holes[0][0] - stab_holes[1][0])
    print(f"Stabilizer spacing: {spacing:.2f}mm")
```

## Creating Assembly Documentation

```python
# Generate exploded view using FreeCAD
import FreeCAD
import FreeCADGui

doc = FreeCAD.newDocument("Q1Assembly")

# Load components
plate = Part.insert("Q-Series/Q1/Q1-Plate.stp", doc.Name)
case = Part.insert("Q-Series/Q1/Q1-Case.stp", doc.Name)
pcb = Part.insert("Q-Series/Q1/Q1-PCB.stp", doc.Name)

# Create exploded view (manual positioning or use Assembly4 workbench)
plate_obj = doc.Objects[0]
case_obj = doc.Objects[1]
pcb_obj = doc.Objects[2]

# Offset for explosion
plate_obj.Placement.Base.z += 30  # 30mm up
case_obj.Placement.Base.z -= 20   # 20mm down

# Export render
FreeCADGui.activeDocument().activeView().saveImage("Q1-Exploded.png", 1920, 1080, "White")
```

## Batch Conversion Script

```python
#!/usr/bin/env python3
"""Convert all STEP files in a series to STL for 3D printing"""
import os
import sys
from pathlib import Path

try:
    import cadquery as cq
except ImportError:
    print("Install CadQuery: pip install cadquery")
    sys.exit(1)

def convert_step_to_stl(step_path, output_dir):
    """Convert STEP file to STL"""
    step_path = Path(step_path)
    output_path = output_dir / f"{step_path.stem}.stl"
    
    # Import and export
    shape = cq.importers.importStep(str(step_path))
    cq.exporters.export(shape, str(output_path))
    print(f"Converted: {step_path.name} -> {output_path.name}")

def batch_convert(series_dir):
    """Convert all STEP files in a series directory"""
    series_path = Path(series_dir)
    output_dir = series_path / "STL_Export"
    output_dir.mkdir(exist_ok=True)
    
    for step_file in series_path.rglob("*.stp"):
        if "STL_Export" not in str(step_file):
            convert_step_to_stl(step_file, output_dir)

if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("Usage: python convert_batch.py <series_directory>")
        sys.exit(1)
    
    batch_convert(sys.argv[1])
```

## Configuration & Environment

```bash
# Recommended CAD software environment variables
export FREECAD_USER_HOME=~/.config/FreeCAD
export PYTHONPATH=/usr/lib/freecad/lib:$PYTHONPATH

# For CadQuery
export CADQUERY_CACHE_DIR=~/.cache/cadquery

# OCE/OpenCASCADE (needed for STEP import)
export LD_LIBRARY_PATH=/usr/lib/opencascade:$LD_LIBRARY_PATH
```

## Troubleshooting

### STEP File Won't Import
```python
# Try alternative importers or repair
import cadquery as cq
from OCP.STEPControl import STEPControl_Reader

reader = STEPControl_Reader()
status = reader.ReadFile("problematic-file.stp")

if status == 1:  # IFSelect_RetDone
    reader.TransferRoots()
    shape = reader.OneShape()
    # Now manually create CadQuery object
    result = cq.Workplane("XY").add(cq.Shape.cast(shape))
else:
    print("STEP import failed, try opening in FreeCAD first")
```

### DXF Units Are Wrong
```python
# DXF files may be in different units
import ezdxf

doc = ezdxf.readfile("plate.dxf")
units = doc.units  # Check document units

# If units are inches but you need mm:
scale_factor = 25.4 if units == ezdxf.units.IN else 1.0

for entity in doc.modelspace():
    if entity.dxftype() == 'LINE':
        entity.dxf.start = (entity.dxf.start[0] * scale_factor,
                           entity.dxf.start[1] * scale_factor)
```

### Missing Dimension References
```bash
# Generate measurement report from STEP file using FreeCAD CLI
freecadcmd -c "
import FreeCAD
import Part
doc = FreeCAD.newDocument()
Part.insert('Q1-Case.stp', doc.Name)
obj = doc.Objects[0]
bbox = obj.Shape.BoundBox
print(f'X: {bbox.XLength}, Y: {bbox.YLength}, Z: {bbox.ZLength}')
"
```

### 3D Printer Compatibility
```python
# Repair and optimize mesh for 3D printing
import trimesh

# Load STL converted from STEP
mesh = trimesh.load("K8-Pro-Case.stl")

# Fill holes and fix normals
mesh.fill_holes()
mesh.fix_normals()

# Check if watertight
if mesh.is_watertight:
    print("Mesh is printable")
else:
    print("Mesh has holes, attempting repair...")
    mesh = mesh.fill_holes()

# Export repaired
mesh.export("K8-Pro-Case-Repaired.stl")
```

## License Compliance

**Source-available license**: Personal and educational use allowed. Original compatible accessories are NOT subject to commercial restrictions, but you cannot copy/sell Keychron keyboards themselves or use Keychron trademarks.

```python
# Good: Creating custom accessory
def design_keycap_puller():
    """Custom tool compatible with Keychron keyboards - OK"""
    pass

# Good: Personal remix
def modify_case_for_custom_build():
    """Personal modification for own use - OK"""
    pass

# Bad: Manufacturing clone
def replicate_entire_keyboard_for_sale():
    """Violates license - NOT ALLOWED"""
    raise LicenseViolation("Cannot manufacture/sell Keychron clones")
```

## Quick Reference

| Task | Files Needed | Tool |
|------|-------------|------|
| Extract dimensions | `*-Case.stp` or `*.dxf` | FreeCAD, ezdxf |
| Modify plate layout | `*-Plate.stp` | CadQuery, FreeCAD |
| 3D print parts | `*-Case.stp` → STL | CadQuery, Blender |
| Laser cut custom plate | `*-Plate.dxf` | Inkscape, QCAD |
| Design accessories | `*-Full-Model.stp` | Fusion 360, FreeCAD |
| Study assembly | `*-Full-Model.stp` | Any CAD viewer |

## Resources

- **Documentation**: `docs/` directory in repository
- **File formats**: `docs/file-format-guide.md`
- **Getting started**: `docs/getting-started.md`
- **3D printing**: `docs/3d-printing-guide.md`
- **License FAQ**: `docs/license-faq.md`
- **Community**: [Keychron Discord](https://discord.com/invite/HAYbRrTsjN)
