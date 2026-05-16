---
name: keychron-hardware-design
description: Access and work with Keychron's production-grade CAD files for keyboards and mice to design compatible accessories and modifications
triggers:
  - "open keychron cad files"
  - "design a keychron compatible case"
  - "modify keychron keyboard plate"
  - "work with keychron step files"
  - "create keychron accessory"
  - "remix keychron hardware design"
  - "3d print keychron parts"
  - "check keychron model dimensions"
---

# Keychron Hardware Design Skill

> Skill by [ara.so](https://ara.so) — Design Skills collection.

This skill enables AI agents to help developers work with Keychron's production-grade hardware design files for 135+ keyboard and mouse models. The repository contains CAD assets in STEP, DXF, DWG, and PDF formats covering C Pro 8K, Q, Q Pro, Q HE, Q Max, K Pro, K Max, K HE, V Max, P HE series keyboards and M/G series mice.

## Project Overview

**Repository:** `Keychron/Keychron-Keyboards-Hardware-Design`  
**Stars:** 3,376 (80 stars/day)  
**License:** Source-available (personal/educational use; original compatible accessories exempt from commercial restrictions)  
**File Types:** STEP (.stp), DXF, DWG, PDF  
**Component Categories:** Cases, plates, encoders, stabilizers, keycaps, full models, shells

## Installation

Clone the repository to access the CAD files:

```bash
git clone https://github.com/Keychron/Keychron-Keyboards-Hardware-Design.git
cd Keychron-Keyboards-Hardware-Design
```

Files are organized by series and model in dedicated folders. For example:

```
Q-Series/Q1/
K-Pro-Series/K8-Pro/
Q-Max-Series/Q6-Max/
Mice/M1/
```

## Repository Structure

### Major Series Categories

```
C-Pro-8K-Series/          # Wired keyboards (C1, C2, C3 Pro 8K)
Q-Series/                 # Flagship mechanical (Q0-Q12, Q60, Q65)
Q-Pro-Series/             # Wireless Q variants (Q1-Q14 Pro)
Q-HE-Series/              # Hall Effect models (Q0-Q12 HE)
Q-HE-8K-Series/           # 8K polling Hall Effect (Q1-Q16 HE 8K)
Q-Max-Series/             # Premium Q series (Q0-Q15 Max)
Q-Ultra-8K-Series/        # Ultra series (Q1, Q3, Q5, Q6, Q13 Ultra 8K)
K-Pro-Series/             # K Pro keyboards (K1-K17 Pro)
K-Max-Series/             # K Max keyboards (K0-K17 Max)
K-HE-Series/              # K Hall Effect (K2, K4, K6, K8, K10 HE)
K-QMK-Series/             # QMK firmware models (K1-K10 QMK)
L-Series/                 # Aluminum keyboards (L1, L3)
V-8K-Series/              # V series 8K polling (V1, V3, V5, V6)
V-Ultra-8K-Series/        # V Ultra series (V0, V1, V3, V5, V6, V10)
V-Max-Series/             # V Max series (V1-V10 Max)
P-HE-Series/              # Lemokey Hall Effect (P1-P3 HE)
Mice/                     # Mouse models (M1-M7, G1-G2)
Keycap-Profiles/          # Cherry, KSA, LSA, MDA, OEM, OSA
```

## File Format Guide

### STEP Files (.stp)

Primary 3D CAD format for mechanical design. Compatible with:

- **FreeCAD** (Open source)
- **Fusion 360** (Autodesk)
- **SolidWorks**
- **Onshape**
- **Rhino**

Opening a STEP file in FreeCAD:

```bash
# Install FreeCAD
# Ubuntu/Debian
sudo apt install freecad

# macOS
brew install --cask freecad

# Open file
freecad K-Pro-Series/K8-Pro/K8-Pro-Case.stp
```

### DXF/DWG Files

2D CAD formats for plate designs and dimensional drawings. Compatible with:

- **LibreCAD** (Open source)
- **QCAD**
- **AutoCAD**
- **DraftSight**

Opening DXF in LibreCAD:

```bash
# Install LibreCAD
sudo apt install librecad

# Open file
librecad Q-Series/Q1/Q1-Plate.dxf
```

### PDF Files

Reference documentation and dimensional drawings viewable in any PDF reader.

## Common Workflows

### 1. Inspecting Keyboard Dimensions

To check dimensions for a Q1 Pro case:

```bash
# Navigate to model folder
cd Q-Pro-Series/Q1-Pro/

# List available files
ls -lh
# Expected: Q1-Pro-Case.stp, Q1-Pro-Plate.stp, Q1-Pro-Full-Model.stp

# Open in FreeCAD to measure
freecad Q1-Pro-Case.stp
```

In FreeCAD:
1. Select **Measure** → **Distance**
2. Click two points to measure
3. View dimensions in property panel

### 2. Designing a Compatible Plate Modification

Example: Adding extra mounting holes to a K8 Pro plate.

```python
# Python script using FreeCAD API
import FreeCAD
import Part

# Load original plate
doc = FreeCAD.open("K-Pro-Series/K8-Pro/K8-Pro-Plate.stp")
plate = doc.Objects[0]

# Add mounting holes at specific coordinates
hole_diameter = 3.0  # mm
hole_positions = [
    (10, 10, 0),
    (350, 10, 0),
    (10, 120, 0),
    (350, 120, 0)
]

for pos in hole_positions:
    cylinder = Part.makeCylinder(hole_diameter / 2, 5, FreeCAD.Vector(*pos))
    plate.Shape = plate.Shape.cut(cylinder)

# Export modified plate
plate.Shape.exportStep("K8-Pro-Plate-Modified.stp")
doc.saveAs("K8-Pro-Plate-Modified.FCStd")
```

### 3. Creating a Custom Case Add-on

Example: Designing a magnetic feet attachment for Q6 Max.

```python
# FreeCAD Python console script
import FreeCAD
import Part

# Load Q6 Max case
doc = FreeCAD.open("Q-Max-Series/Q6-Max/Q6-Max-Case.stp")
case = doc.Objects[0]

# Get case bottom dimensions
bbox = case.Shape.BoundBox
width = bbox.XLength
depth = bbox.YLength

# Create magnetic feet holder (4 corners)
feet_diameter = 10  # mm
feet_height = 3  # mm
inset = 15  # mm from edges

feet_positions = [
    (inset, inset),
    (width - inset, inset),
    (inset, depth - inset),
    (width - inset, depth - inset)
]

feet_parts = []
for x, y in feet_positions:
    foot = Part.makeCylinder(feet_diameter / 2, feet_height, FreeCAD.Vector(x, y, -feet_height))
    feet_parts.append(foot)

# Combine all feet
feet_holder = feet_parts[0]
for foot in feet_parts[1:]:
    feet_holder = feet_holder.fuse(foot)

# Export for 3D printing
feet_holder.exportStl("Q6-Max-Magnetic-Feet.stl")
```

### 4. Extracting Plate DXF for Laser Cutting

```bash
# Navigate to desired model
cd Q-Series/Q3/

# Verify DXF exists
ls *.dxf
# Q3-Plate.dxf

# Open in LibreCAD to verify dimensions
librecad Q3-Plate.dxf

# Export with correct units for laser cutting service
# In LibreCAD: File > Export > DXF
# Set units to millimeters
# Set version to R12/LT2 for maximum compatibility
```

### 5. Converting STEP to STL for 3D Printing

Using FreeCAD command line:

```bash
# Convert K2 HE case to STL
freecad -c << EOF
import FreeCAD
import Mesh

doc = FreeCAD.open("K-HE-Series/K2-HE/K2-HE-Case.stp")
obj = doc.Objects[0]

# Export with reasonable resolution
Mesh.export([obj], "K2-HE-Case.stl")
EOF
```

Or using Python script:

```python
import FreeCAD
import Mesh
import sys

input_file = sys.argv[1]  # "K2-HE-Case.stp"
output_file = sys.argv[2]  # "K2-HE-Case.stl"

doc = FreeCAD.open(input_file)
shape = doc.Objects[0]

# Higher mesh deviation = smoother surface
mesh = shape.Shape.tessellate(0.01)
Mesh.Mesh(mesh[0], mesh[1]).write(output_file)
```

### 6. Batch Processing Multiple Models

Extract all plate files from Q Series:

```bash
#!/bin/bash

# Find all STEP plate files in Q-Series
find Q-Series -name "*Plate.stp" -type f > plate_files.txt

# Convert each to STL
while IFS= read -r file; do
    base=$(basename "$file" .stp)
    dir=$(dirname "$file")
    
    echo "Converting $file..."
    freecad -c << EOF
import FreeCAD
import Mesh

doc = FreeCAD.open("$file")
obj = doc.Objects[0]
Mesh.export([obj], "$dir/${base}.stl")
EOF
done < plate_files.txt
```

## Working with Keycap Profiles

The repository includes reference keycap profiles:

```bash
cd Keycap-Profiles

# Available profiles
ls -1
# Cherry-Profile/
# KSA-Profile/
# LSA-Profile/
# MDA-Profile/
# OEM-Profile/
# OSA-Profile/
```

Load a keycap profile for custom keycap design:

```python
import FreeCAD
import Part

# Load OSA profile reference
doc = FreeCAD.open("Keycap-Profiles/OSA-Profile/OSA-R1-1u.stp")
keycap = doc.Objects[0]

# Clone and modify for custom legend
custom = keycap.Shape.copy()

# Add custom text engraving (simplified example)
text_height = 1.0  # mm deep
# ... perform boolean cut operation for legend
```

## Configuration & Best Practices

### File Organization

```bash
# Recommended local structure
keychron-mods/
├── original/              # Clone of official repo
├── modified/              # Your modifications
│   ├── plates/
│   ├── cases/
│   └── accessories/
├── exports/               # STL/DXF exports for production
└── scripts/               # Automation scripts
```

### CAD Software Settings

**FreeCAD Preferences:**
- Units: Millimeters (default for Keychron files)
- Grid spacing: 1mm
- Snap to grid: Enabled for precision

**Fusion 360 Import:**
- File format: STEP AP214
- Unit: mm (auto-detected)
- Stitching tolerance: 0.001 mm

### 3D Printing Guidelines

For printing keyboard parts from this repository:

```yaml
# Recommended PLA/PETG settings
layer_height: 0.2mm
wall_thickness: 1.2mm (3 walls)
infill: 20-30%
supports: Usually required for cases
bed_adhesion: Brim for large flat parts
orientation: Mounting face down for plates
```

Example Cura profile snippet:

```python
# cura_keychron_profile.py
settings = {
    'layer_height': 0.2,
    'wall_thickness': 1.2,
    'top_bottom_thickness': 1.0,
    'infill_sparse_density': 25,
    'support_enable': True,
    'support_type': 'buildplate',
    'adhesion_type': 'brim',
    'brim_width': 5
}
```

## Measurement & Validation

### Validating Plate Tolerances

```python
import FreeCAD
import Part

# Load plate and switch reference
plate = FreeCAD.open("Q-Series/Q1/Q1-Plate.stp").Objects[0]

# Cherry MX switch cutout should be 14mm x 14mm
# Measure actual cutout dimensions
def measure_switch_cutout(plate_shape):
    # Find rectangular faces on top surface
    faces = [f for f in plate_shape.Faces if f.Area > 180 and f.Area < 200]
    
    for face in faces:
        bbox = face.BoundBox
        width = bbox.XLength
        height = bbox.YLength
        
        if 13.8 <= width <= 14.2 and 13.8 <= height <= 14.2:
            print(f"Switch cutout: {width:.2f}mm x {height:.2f}mm")
            return True
    
    print("Warning: Standard switch cutout not found")
    return False

measure_switch_cutout(plate.Shape)
```

## Common Issues & Troubleshooting

### Issue: STEP file won't open

```bash
# Verify file integrity
file K-Pro-Series/K8-Pro/K8-Pro-Case.stp
# Expected: "K8-Pro-Case.stp: ISO-10303 STEP data"

# Try converting with Open CASCADE
STEPFILE="K8-Pro-Case.stp"
python3 << EOF
import OCC.Core.STEPControl as STEP
reader = STEP.STEPControl_Reader()
status = reader.ReadFile("$STEPFILE")
if status == 1:
    print("File valid")
else:
    print("File corrupted or invalid")
EOF
```

### Issue: Units appear incorrect

All Keychron files are in millimeters. If dimensions appear wrong:

```python
# FreeCAD unit conversion
import FreeCAD

doc = FreeCAD.open("Q1-Case.stp")
obj = doc.Objects[0]

# Check bounding box in mm
bbox = obj.Shape.BoundBox
print(f"Width: {bbox.XLength}mm")
print(f"Depth: {bbox.YLength}mm")
print(f"Height: {bbox.ZLength}mm")

# Expected for Q1: ~360mm x ~140mm x ~30mm
```

### Issue: Missing files for specific model

Check the model's README for download links:

```bash
# Each model folder contains a README with download links
cat Q-Max-Series/Q6-Max/README.md
# Look for "Downloads" section
```

Many models have placeholder folders with README/product pages but CAD files are uploaded incrementally. Check repository updates.

### Issue: Exported STL has gaps or errors

Increase mesh resolution:

```python
import FreeCAD
import Mesh

doc = FreeCAD.open("source.stp")
obj = doc.Objects[0]

# Lower deviation = smoother mesh (0.01 is high quality)
mesh = obj.Shape.tessellate(0.01)
Mesh.Mesh(mesh[0], mesh[1]).write("output-high-res.stl")
```

## License Compliance

**Allowed:**
- Personal use and modifications
- Educational projects
- Creating original compatible accessories
- Studying and remixing designs
- 3D printing parts for personal keyboards

**Not Allowed:**
- Copying and selling Keychron keyboards/mice
- Using Keychron trademarks as your own brand
- Mass manufacturing clones for commercial sale

**Gray Area (Original Accessories ARE Allowed):**
The license explicitly states: "Original compatible accessories and add-ons are not subject to the commercial-use restriction."

This means you CAN sell:
- Custom plates that fit Keychron cases
- Aftermarket cases for Keychron PCBs
- Mounting brackets and stands
- Cable management solutions
- Foam inserts and dampeners

## Contributing Modifications

If you improve dimensions or fix issues:

```bash
# Fork the repository
gh repo fork Keychron/Keychron-Keyboards-Hardware-Design

# Create feature branch
git checkout -b fix/q1-plate-tolerance

# Make changes to STEP files
# Use FreeCAD or other CAD software

# Export corrected version
# Include both STEP and source format (.FCStd for FreeCAD)

# Commit with descriptive message
git add Q-Series/Q1/Q1-Plate.stp
git add Q-Series/Q1/Q1-Plate.FCStd
git commit -m "Fix Q1 plate switch cutout tolerance to 14.0mm ±0.05mm"

# Push and create PR
git push origin fix/q1-plate-tolerance
gh pr create --title "Fix Q1 plate switch cutout tolerance"
```

## Additional Resources

- **Documentation:** `docs/file-format-guide.md`, `docs/getting-started.md`
- **3D Printing Guide:** `docs/3d-printing-guide.md`
- **Contributing:** `docs/CONTRIBUTING.md`
- **Discord:** https://discord.com/invite/HAYbRrTsjN
- **Keychron Homepage:** https://www.keychron.com/

This skill enables comprehensive support for users working with Keychron's hardware design files for modification, accessory creation, and education.
