---
name: rocket-design-manufacturing-automation-shenzhen22
description: Automated rocket design and manufacturing program built by students using TypeScript for aerospace engineering workflows
triggers:
  - how do I design a rocket using the Shenzhen22 automation program
  - use the rocket design and manufacturing automation tool
  - automate rocket component manufacturing with TypeScript
  - configure rocket design parameters for automated manufacturing
  - help me with the rocket design automation from Shenzhen22highschool
  - implement automated aerospace manufacturing workflows
  - set up the rocket design automation program
  - calculate rocket parameters using the automation system
---

# Rocket Design and Manufacturing Automation Program

> Skill by [ara.so](https://ara.so) — Design Skills collection.

This is a TypeScript-based automation program developed by students from Shenzhen 22 High School for designing and manufacturing rockets. The system automates aerospace engineering calculations, component design, and manufacturing workflows.

## Installation

Clone and install the project:

```bash
git clone https://github.com/Kevin100202/Rocket-Design-and-Manufacturing-Automation-Program-from-Shenzhen22highschool.git
cd Rocket-Design-and-Manufacturing-Automation-Program-from-Shenzhen22highschool
npm install
```

Build the TypeScript project:

```bash
npm run build
```

Run the program:

```bash
npm start
```

## Project Structure

The project typically follows this structure:

```
src/
  ├── design/          # Rocket design modules
  ├── manufacturing/   # Manufacturing automation
  ├── calculations/    # Aerospace calculations
  ├── models/          # Data models and types
  └── utils/           # Utility functions
```

## Core Modules

### Rocket Design Module

Design rocket components with automated parameter calculation:

```typescript
import { RocketDesigner } from './design/RocketDesigner';
import { RocketSpecs } from './models/RocketSpecs';

// Initialize rocket designer
const designer = new RocketDesigner();

// Define rocket specifications
const specs: RocketSpecs = {
  targetAltitude: 10000, // meters
  payloadMass: 5, // kg
  desiredThrust: 2000, // Newtons
  burnTime: 15, // seconds
  stability: 2.0 // caliber stability margin
};

// Generate rocket design
const rocketDesign = designer.createDesign(specs);
console.log(rocketDesign);
```

### Manufacturing Automation

Automate manufacturing workflows:

```typescript
import { ManufacturingController } from './manufacturing/ManufacturingController';
import { ComponentType } from './models/ComponentType';

const controller = new ManufacturingController();

// Define component to manufacture
const noseConeParams = {
  type: ComponentType.NOSE_CONE,
  material: 'ABS Plastic',
  length: 300, // mm
  diameter: 100, // mm
  wallThickness: 3 // mm
};

// Generate manufacturing instructions
const instructions = controller.generateInstructions(noseConeParams);

// Export to CAD/CAM format
controller.exportToGCode(instructions, './output/nosecone.gcode');
```

### Aerospace Calculations

Perform critical rocket calculations:

```typescript
import { AeroCalculator } from './calculations/AeroCalculator';
import { PropulsionCalculator } from './calculations/PropulsionCalculator';

const aeroCalc = new AeroCalculator();
const propCalc = new PropulsionCalculator();

// Calculate center of pressure
const centerOfPressure = aeroCalc.calculateCenterOfPressure({
  noseConeLength: 300,
  bodyTubeLength: 1000,
  finSpan: 150,
  finRootChord: 200,
  bodyDiameter: 100
});

// Calculate thrust requirements
const thrustProfile = propCalc.calculateThrustProfile({
  totalMass: 25, // kg
  targetVelocity: 300, // m/s
  burnTime: 15, // seconds
  atmosphericDrag: 0.75
});

console.log(`Center of Pressure: ${centerOfPressure.position}mm`);
console.log(`Required Average Thrust: ${thrustProfile.averageThrust}N`);
```

### Stability Analysis

Ensure rocket stability:

```typescript
import { StabilityAnalyzer } from './design/StabilityAnalyzer';

const stabilityAnalyzer = new StabilityAnalyzer();

const stabilityData = {
  centerOfGravity: 600, // mm from nose tip
  centerOfPressure: 850, // mm from nose tip
  bodyDiameter: 100 // mm
};

const stability = stabilityAnalyzer.analyze(stabilityData);

if (stability.isStable) {
  console.log(`Rocket is stable with ${stability.calibers} caliber margin`);
} else {
  console.log(`Warning: Unstable configuration. Adjust fin size or weight distribution.`);
}
```

## Configuration

Create a `rocket.config.ts` file for project settings:

```typescript
export const RocketConfig = {
  units: {
    length: 'mm',
    mass: 'kg',
    force: 'N'
  },
  materials: {
    bodyTube: 'Fiberglass',
    fins: 'Plywood',
    noseCone: 'ABS Plastic'
  },
  manufacturing: {
    tolerance: 0.1, // mm
    layer_height: 0.2, // mm for 3D printing
    infill: 20 // percentage
  },
  safety: {
    minStabilityMargin: 1.5,
    maxOperatingPressure: 800, // PSI
    safetyFactor: 1.5
  }
};
```

Use configuration in your design:

```typescript
import { RocketConfig } from './rocket.config';

const designer = new RocketDesigner(RocketConfig);
```

## Common Design Patterns

### Complete Rocket Design Workflow

```typescript
import { RocketDesigner } from './design/RocketDesigner';
import { StabilityAnalyzer } from './design/StabilityAnalyzer';
import { ManufacturingController } from './manufacturing/ManufacturingController';
import { ReportGenerator } from './utils/ReportGenerator';

async function designRocket() {
  const designer = new RocketDesigner();
  const stabilityAnalyzer = new StabilityAnalyzer();
  const manufacturing = new ManufacturingController();
  const reporter = new ReportGenerator();

  // Step 1: Create initial design
  const design = designer.createDesign({
    targetAltitude: 5000,
    payloadMass: 3,
    desiredThrust: 1500,
    burnTime: 12
  });

  // Step 2: Analyze stability
  const stability = stabilityAnalyzer.analyze({
    centerOfGravity: design.cg,
    centerOfPressure: design.cp,
    bodyDiameter: design.bodyDiameter
  });

  // Step 3: Optimize if needed
  if (!stability.isStable) {
    design = designer.optimizeForStability(design, stability);
  }

  // Step 4: Generate manufacturing files
  for (const component of design.components) {
    const instructions = manufacturing.generateInstructions(component);
    manufacturing.exportToGCode(instructions, `./output/${component.name}.gcode`);
  }

  // Step 5: Create documentation
  const report = reporter.generate(design, stability);
  reporter.exportPDF(report, './output/rocket-design-report.pdf');

  return design;
}

designRocket().then(design => {
  console.log('Rocket design complete!');
});
```

### Fin Design and Optimization

```typescript
import { FinDesigner } from './design/FinDesigner';

const finDesigner = new FinDesigner();

const finConfig = {
  numberOfFins: 4,
  rootChord: 200, // mm
  tipChord: 80, // mm
  span: 150, // mm
  sweepAngle: 30, // degrees
  thickness: 6 // mm
};

const fins = finDesigner.design(finConfig);

// Optimize for stability
const optimizedFins = finDesigner.optimizeForCP(fins, {
  targetCP: 850, // mm from nose
  currentCP: 780
});

console.log(`Optimized fin span: ${optimizedFins.span}mm`);
```

### Motor Selection and Integration

```typescript
import { MotorDatabase } from './propulsion/MotorDatabase';
import { MotorSelector } from './propulsion/MotorSelector';

const motorDB = new MotorDatabase();
const selector = new MotorSelector(motorDB);

// Find suitable motor
const requirements = {
  minTotalImpulse: 2500, // Ns
  maxDiameter: 38, // mm
  maxLength: 400, // mm
  preferredPropellant: 'APCP'
};

const suitableMotors = selector.findMotors(requirements);
const selectedMotor = suitableMotors[0];

console.log(`Selected motor: ${selectedMotor.designation}`);
console.log(`Total Impulse: ${selectedMotor.totalImpulse}Ns`);
console.log(`Average Thrust: ${selectedMotor.averageThrust}N`);
```

## Simulation and Testing

Run flight simulations:

```typescript
import { FlightSimulator } from './simulation/FlightSimulator';

const simulator = new FlightSimulator();

const flightData = simulator.simulate({
  rocket: rocketDesign,
  motor: selectedMotor,
  launchAngle: 90, // degrees
  windSpeed: 5, // m/s
  temperature: 20, // celsius
  pressure: 101325 // Pa
});

console.log(`Predicted Apogee: ${flightData.maxAltitude}m`);
console.log(`Max Velocity: ${flightData.maxVelocity}m/s`);
console.log(`Flight Time: ${flightData.totalTime}s`);
```

## Export and Integration

### Export to CAD Formats

```typescript
import { CADExporter } from './export/CADExporter';

const exporter = new CADExporter();

// Export to STEP format
exporter.exportSTEP(rocketDesign, './output/rocket.step');

// Export to STL for 3D printing
exporter.exportSTL(rocketDesign.noseCone, './output/nosecone.stl');

// Export technical drawings
exporter.exportDXF(rocketDesign, './output/rocket-drawings.dxf');
```

## Troubleshooting

### Design Not Stable

If stability analysis fails:

```typescript
// Increase fin size
finConfig.span += 20;
finConfig.rootChord += 10;

// Or move weight forward
design.adjustCG(-50); // Move CG 50mm forward
```

### Manufacturing Tolerances

Handle precision requirements:

```typescript
const manufacturing = new ManufacturingController({
  tolerance: 0.05, // Tighter tolerance
  qualityCheck: true
});
```

### Unit Conversion Issues

Ensure consistent units:

```typescript
import { UnitConverter } from './utils/UnitConverter';

const converter = new UnitConverter();

const lengthInMeters = converter.convert(1000, 'mm', 'm'); // 1
const forceInPounds = converter.convert(2000, 'N', 'lbf'); // ~449.6
```

## Environment Variables

Configure environment-specific settings in `.env`:

```
MANUFACTURING_PRECISION=0.1
SAFETY_FACTOR=1.5
EXPORT_PATH=./output
SIMULATION_ITERATIONS=1000
```

Access in code:

```typescript
const precision = parseFloat(process.env.MANUFACTURING_PRECISION || '0.1');
const safetyFactor = parseFloat(process.env.SAFETY_FACTOR || '1.5');
```
