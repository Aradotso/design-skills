---
name: rocket-design-manufacturing-automation-shenzhen22
description: Automated rocket design and manufacturing program built by students using TypeScript
triggers:
  - how do I use the rocket design automation program
  - help me design a rocket with the Shenzhen22 tool
  - automate rocket manufacturing process
  - use the rocket design and manufacturing system
  - calculate rocket parameters automatically
  - generate rocket manufacturing specs
  - design and simulate a rocket flight
  - automate rocket component design
---

# Rocket Design and Manufacturing Automation Program

> Skill by [ara.so](https://ara.so) — Design Skills collection.

This is a student-built TypeScript program from Shenzhen 22nd High School that automates rocket design calculations, component specifications, and manufacturing workflows. It provides tools for trajectory simulation, structural analysis, propulsion system design, and automated CAD file generation.

## Installation

```bash
# Clone the repository
git clone https://github.com/Kevin100202/Rocket-Design-and-Manufacturing-Automation-Program-from-Shenzhen22highschool.git
cd Rocket-Design-and-Manufacturing-Automation-Program-from-Shenzhen22highschool

# Install dependencies
npm install

# Build the project
npm run build

# Run the program
npm start
```

## Project Structure

```
├── src/
│   ├── design/          # Rocket design modules
│   ├── simulation/      # Flight simulation
│   ├── manufacturing/   # Manufacturing automation
│   ├── calculations/    # Physics and engineering calculations
│   └── utils/          # Helper utilities
├── config/             # Configuration files
├── output/             # Generated designs and specs
└── tests/              # Test suites
```

## Core Modules

### 1. Rocket Design Module

```typescript
import { RocketDesigner } from './src/design/RocketDesigner';

// Initialize rocket designer
const designer = new RocketDesigner({
  targetAltitude: 1000, // meters
  payloadMass: 0.5,     // kg
  diameter: 0.1,        // meters
  safetyFactor: 1.5
});

// Generate design
const design = await designer.generateDesign();

console.log(design);
// {
//   totalLength: 1.2,
//   noseConeLength: 0.3,
//   bodyTubeLength: 0.7,
//   finSpan: 0.15,
//   estimatedMass: 2.3,
//   centerOfGravity: 0.65,
//   centerOfPressure: 0.55,
//   stabilityMargin: 1.8
// }
```

### 2. Propulsion System Calculator

```typescript
import { PropulsionCalculator } from './src/calculations/PropulsionCalculator';

const propulsion = new PropulsionCalculator({
  motorClass: 'E',
  totalImpulse: 40,      // Newton-seconds
  burnTime: 2.5,         // seconds
  propellantMass: 0.025  // kg
});

// Calculate motor parameters
const motorSpecs = propulsion.calculateMotorSpecs();

console.log(motorSpecs);
// {
//   averageThrust: 16,
//   specificImpulse: 160,
//   exhaustVelocity: 1568,
//   massFlowRate: 0.01,
//   chamberPressure: 7000000
// }

// Estimate performance
const performance = propulsion.estimatePerformance({
  rocketMass: 2.3,
  dragCoefficient: 0.45,
  referenceArea: 0.00785
});

console.log(performance);
// {
//   maxAltitude: 987,
//   maxVelocity: 95,
//   acceleration: 58,
//   flightTime: 24.5
// }
```

### 3. Trajectory Simulator

```typescript
import { TrajectorySimulator } from './src/simulation/TrajectorySimulator';

const simulator = new TrajectorySimulator({
  initialMass: 2.3,
  thrust: 16,
  burnTime: 2.5,
  dragCoefficient: 0.45,
  referenceArea: 0.00785,
  windSpeed: 5,         // m/s
  windDirection: 90,    // degrees
  timeStep: 0.01        // seconds
});

// Run simulation
const trajectory = await simulator.simulate();

// Export results
await simulator.exportCSV('./output/trajectory.csv');
await simulator.exportJSON('./output/trajectory.json');

// Get key metrics
console.log(trajectory.metrics);
// {
//   apogee: 987,
//   maxVelocity: 95,
//   maxAcceleration: 58,
//   flightTime: 24.5,
//   landingDistance: 125,
//   driftAngle: 88
// }
```

### 4. Structural Analysis

```typescript
import { StructuralAnalyzer } from './src/calculations/StructuralAnalyzer';

const analyzer = new StructuralAnalyzer({
  material: 'fiberglass',
  thickness: 0.002,      // meters
  diameter: 0.1,
  length: 0.7,
  safetyFactor: 1.5
});

// Analyze structural integrity
const analysis = analyzer.analyze({
  maxThrust: 16,
  maxVelocity: 95,
  maxAcceleration: 58
});

console.log(analysis);
// {
//   maxStress: 45000000,    // Pa
//   yieldStrength: 90000000,
//   stressRatio: 0.5,
//   bucklingLoad: 350,
//   bucklingRatio: 0.65,
//   passed: true,
//   recommendations: []
// }
```

### 5. Fin Design

```typescript
import { FinDesigner } from './src/design/FinDesigner';

const finDesigner = new FinDesigner({
  numberOfFins: 3,
  bodyDiameter: 0.1,
  rootChord: 0.12,
  tipChord: 0.06,
  span: 0.08,
  sweepAngle: 30,       // degrees
  thickness: 0.003      // meters
});

// Calculate fin parameters
const fins = finDesigner.calculate();

console.log(fins);
// {
//   area: 0.0072,
//   aspectRatio: 0.89,
//   centerOfPressure: 0.045,
//   normalForceCoefficient: 1.2,
//   mass: 0.015,
//   cantAngle: 0
// }

// Generate cutting template
await finDesigner.generateTemplate('./output/fin_template.svg');
```

### 6. Manufacturing Automation

```typescript
import { ManufacturingPlanner } from './src/manufacturing/ManufacturingPlanner';

const planner = new ManufacturingPlanner({
  design: design,
  materials: {
    bodyTube: 'fiberglass',
    fins: 'plywood',
    noseCone: '3d-printed-pla'
  },
  processes: ['cutting', 'bonding', 'finishing', 'assembly']
});

// Generate manufacturing plan
const plan = await planner.generatePlan();

console.log(plan);
// {
//   steps: [
//     {
//       step: 1,
//       process: 'cutting',
//       component: 'body-tube',
//       length: 700,
//       instructions: 'Cut fiberglass tube to 700mm',
//       tools: ['tube cutter', 'measuring tape'],
//       duration: 10
//     },
//     // ... more steps
//   ],
//   totalDuration: 180,    // minutes
//   materialList: [...],
//   toolList: [...],
//   qualityChecks: [...]
// }

// Export manufacturing documents
await planner.exportBOM('./output/bill_of_materials.csv');
await planner.exportInstructions('./output/assembly_guide.pdf');
```

### 7. CAD File Generation

```typescript
import { CADGenerator } from './src/manufacturing/CADGenerator';

const cadGen = new CADGenerator({
  design: design,
  format: 'step',        // or 'stl', 'iges'
  resolution: 'high'
});

// Generate 3D models
await cadGen.generateNoseCone('./output/nose_cone.step');
await cadGen.generateBodyTube('./output/body_tube.step');
await cadGen.generateFins('./output/fins.step');
await cadGen.generateAssembly('./output/full_rocket.step');

// Generate 2D drawings
await cadGen.generate2DDrawings({
  scale: '1:2',
  units: 'mm',
  views: ['front', 'side', 'top'],
  dimensions: true,
  tolerances: true,
  outputPath: './output/technical_drawings.pdf'
});
```

## Configuration

Create a `config/rocket.config.json` file:

```json
{
  "project": {
    "name": "Model Rocket Alpha",
    "version": "1.0",
    "designer": "Engineering Team"
  },
  "requirements": {
    "targetAltitude": 1000,
    "targetApogeeAccuracy": 50,
    "maxDiameter": 0.15,
    "maxLength": 1.5,
    "maxMass": 3.0,
    "safetyFactor": 1.5
  },
  "materials": {
    "bodyTube": "fiberglass",
    "fins": "plywood",
    "noseCone": "3d-printed-pla",
    "recovery": "nylon-parachute"
  },
  "simulation": {
    "timeStep": 0.01,
    "atmosphericModel": "standard",
    "windModel": "linear",
    "turbulenceEnabled": false
  },
  "manufacturing": {
    "tolerances": {
      "length": 1,
      "diameter": 0.5,
      "angle": 2
    },
    "processes": ["cutting", "bonding", "finishing", "assembly"],
    "qualityControlEnabled": true
  },
  "output": {
    "cadFormat": "step",
    "drawingFormat": "pdf",
    "simulationFormat": "csv",
    "reportFormat": "markdown"
  }
}
```

Load configuration:

```typescript
import { ConfigLoader } from './src/utils/ConfigLoader';

const config = ConfigLoader.load('./config/rocket.config.json');

const designer = new RocketDesigner(config.requirements);
```

## Complete Workflow Example

```typescript
import { RocketDesignWorkflow } from './src/RocketDesignWorkflow';

async function designRocket() {
  // Initialize workflow
  const workflow = new RocketDesignWorkflow({
    configPath: './config/rocket.config.json',
    outputDir: './output'
  });

  // Step 1: Design rocket
  console.log('Designing rocket...');
  const design = await workflow.design();

  // Step 2: Validate design
  console.log('Validating design...');
  const validation = await workflow.validate(design);
  
  if (!validation.passed) {
    console.error('Design validation failed:', validation.issues);
    return;
  }

  // Step 3: Simulate flight
  console.log('Simulating flight...');
  const simulation = await workflow.simulate(design);

  // Step 4: Generate manufacturing plan
  console.log('Generating manufacturing plan...');
  const manufacturing = await workflow.planManufacturing(design);

  // Step 5: Generate CAD files
  console.log('Generating CAD files...');
  await workflow.generateCAD(design);

  // Step 6: Export complete documentation
  console.log('Exporting documentation...');
  await workflow.exportDocumentation({
    design,
    simulation,
    manufacturing,
    outputPath: './output/rocket_documentation.pdf'
  });

  console.log('Rocket design complete!');
  console.log(`Predicted apogee: ${simulation.metrics.apogee}m`);
  console.log(`Manufacturing time: ${manufacturing.totalDuration} minutes`);
}

designRocket().catch(console.error);
```

## Common Patterns

### Custom Motor Configuration

```typescript
import { CustomMotor } from './src/calculations/CustomMotor';

const motor = new CustomMotor({
  propellantType: 'KNSB',
  grainGeometry: 'BATES',
  numberOfGrains: 4,
  grainDiameter: 0.024,
  coreDiameter: 0.008,
  grainLength: 0.07
});

const thrustCurve = motor.generateThrustCurve();
propulsion.setCustomMotor(motor);
```

### Stability Analysis

```typescript
import { StabilityAnalyzer } from './src/calculations/StabilityAnalyzer';

const stability = new StabilityAnalyzer({
  centerOfGravity: design.centerOfGravity,
  centerOfPressure: design.centerOfPressure,
  bodyDiameter: design.diameter
});

const analysis = stability.analyze();

if (analysis.stabilityMargin < 1.0) {
  console.warn('Rocket is unstable! Adjust fin size or weight distribution.');
}
```

### Recovery System Design

```typescript
import { RecoverySystem } from './src/design/RecoverySystem';

const recovery = new RecoverySystem({
  rocketMass: design.estimatedMass,
  descentRate: 5,        // m/s
  deploymentAltitude: 300 // meters
});

const parachute = recovery.calculateParachute();

console.log(parachute);
// {
//   diameter: 0.6,
//   area: 0.28,
//   dragCoefficient: 1.5,
//   shroudLineLength: 0.6,
//   numberOfLines: 8
// }
```

## Troubleshooting

### Design Validation Failures

```typescript
// Check for common issues
if (design.stabilityMargin < 1.0) {
  // Increase fin size or move CG forward
  finDesigner.setSpan(finDesigner.span * 1.2);
}

if (design.estimatedMass > config.requirements.maxMass) {
  // Reduce wall thickness or use lighter materials
  bodyTubeThickness *= 0.9;
}
```

### Simulation Convergence Issues

```typescript
// Reduce time step for better accuracy
simulator.setTimeStep(0.001);

// Enable adaptive time stepping
simulator.setAdaptiveTimeStep(true, {
  minStep: 0.0001,
  maxStep: 0.01,
  tolerance: 0.001
});
```

### CAD Generation Errors

```typescript
try {
  await cadGen.generateAssembly('./output/assembly.step');
} catch (error) {
  if (error.code === 'GEOMETRY_INVALID') {
    // Simplify geometry
    cadGen.setResolution('medium');
    await cadGen.generateAssembly('./output/assembly.step');
  }
}
```

### Material Property Lookup

```typescript
import { MaterialDatabase } from './src/utils/MaterialDatabase';

const materials = MaterialDatabase.load();

// Get material properties
const fiberglass = materials.get('fiberglass');
console.log(fiberglass);
// {
//   density: 1800,
//   yieldStrength: 90000000,
//   elasticModulus: 25000000000,
//   poissonRatio: 0.25
// }
```

## Testing

```bash
# Run all tests
npm test

# Run specific test suite
npm test -- --grep "PropulsionCalculator"

# Run with coverage
npm run test:coverage
```

## Environment Variables

```bash
# Optional: Set output directory
export ROCKET_OUTPUT_DIR=./output

# Optional: Set log level
export ROCKET_LOG_LEVEL=debug

# Optional: Enable performance profiling
export ROCKET_PROFILE=true
```

This skill provides comprehensive automation for rocket design, simulation, and manufacturing planning using TypeScript-based engineering calculations and CAD generation tools.
