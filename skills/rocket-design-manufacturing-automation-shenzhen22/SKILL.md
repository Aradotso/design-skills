---
name: rocket-design-manufacturing-automation-shenzhen22
description: Automate rocket design and manufacturing workflows with TypeScript-based simulation, CAD generation, and production planning tools
triggers:
  - how do I design a rocket with automation
  - help me simulate rocket trajectories
  - generate CAD files for rocket components
  - automate rocket manufacturing workflow
  - calculate rocket thrust and performance
  - create rocket design from parameters
  - optimize rocket stage configurations
  - validate rocket structural integrity
---

# Rocket Design and Manufacturing Automation Program

> Skill by [ara.so](https://ara.so) — Design Skills collection.

This project provides a TypeScript-based automation program for designing, simulating, and planning the manufacturing of rockets. Created by students from Shenzhen22highschool, it offers tools for trajectory calculations, structural analysis, CAD generation, and production workflow automation.

## Installation

```bash
# Clone the repository
git clone https://github.com/Kevin100202/Rocket-Design-and-Manufacturing-Automation-Program-from-Shenzhen22highschool.git
cd Rocket-Design-and-Manufacturing-Automation-Program-from-Shenzhen22highschool

# Install dependencies
npm install

# Build the project
npm run build
```

## Project Structure

Typical structure for this automation program:

```
src/
├── design/           # Rocket design modules
├── simulation/       # Physics and trajectory simulation
├── manufacturing/    # Production planning and CAD generation
├── analysis/         # Structural and performance analysis
├── utils/           # Helper functions and constants
└── index.ts         # Main entry point
```

## Core Modules

### Rocket Design

Create rocket designs with customizable parameters:

```typescript
import { RocketDesigner } from './design/RocketDesigner';

// Define rocket specifications
const rocketSpecs = {
  name: "SZ22-Alpha",
  stages: 2,
  payload: 50, // kg
  targetAltitude: 100000, // meters
  diameter: 0.15, // meters
  material: "aluminum-6061"
};

// Initialize designer
const designer = new RocketDesigner(rocketSpecs);

// Generate design
const design = await designer.generateDesign();

console.log(`Total mass: ${design.totalMass} kg`);
console.log(`Estimated delta-v: ${design.deltaV} m/s`);
```

### Stage Configuration

Configure multi-stage rockets:

```typescript
import { StageBuilder } from './design/StageBuilder';

// First stage configuration
const stage1 = new StageBuilder()
  .setName("Booster")
  .setEngine({
    type: "solid",
    thrust: 5000, // Newtons
    burnTime: 15, // seconds
    specificImpulse: 180
  })
  .setStructure({
    length: 1.2, // meters
    diameter: 0.15,
    wallThickness: 0.003
  })
  .setPropellant({
    type: "APCP",
    mass: 8 // kg
  })
  .build();

// Second stage configuration
const stage2 = new StageBuilder()
  .setName("Sustainer")
  .setEngine({
    type: "solid",
    thrust: 2000,
    burnTime: 10,
    specificImpulse: 200
  })
  .setStructure({
    length: 0.8,
    diameter: 0.15,
    wallThickness: 0.002
  })
  .setPropellant({
    type: "APCP",
    mass: 3
  })
  .build();
```

### Trajectory Simulation

Simulate rocket flight paths:

```typescript
import { TrajectorySimulator } from './simulation/TrajectorySimulator';
import { AtmosphereModel } from './simulation/AtmosphereModel';

// Initialize simulator with atmosphere model
const atmosphere = new AtmosphereModel("standard");
const simulator = new TrajectorySimulator(atmosphere);

// Configure simulation parameters
const simConfig = {
  timeStep: 0.01, // seconds
  maxTime: 300,
  launchAngle: 85, // degrees from horizontal
  launchAltitude: 100, // meters ASL
  windSpeed: 5, // m/s
  windDirection: 270 // degrees
};

// Run simulation
const trajectory = await simulator.simulate(design, simConfig);

// Analyze results
console.log(`Max altitude: ${trajectory.maxAltitude.toFixed(2)} m`);
console.log(`Max velocity: ${trajectory.maxVelocity.toFixed(2)} m/s`);
console.log(`Max acceleration: ${trajectory.maxAcceleration.toFixed(2)} m/s²`);
console.log(`Flight time: ${trajectory.flightTime.toFixed(2)} s`);
console.log(`Landing distance: ${trajectory.landingDistance.toFixed(2)} m`);
```

### Performance Analysis

Calculate rocket performance metrics:

```typescript
import { PerformanceAnalyzer } from './analysis/PerformanceAnalyzer';

const analyzer = new PerformanceAnalyzer(design);

// Calculate thrust-to-weight ratio
const twr = analyzer.calculateTWR();
console.log(`Thrust-to-weight ratio: ${twr.toFixed(2)}`);

// Calculate delta-v for each stage
const deltaVs = analyzer.calculateDeltaV();
deltaVs.forEach((dv, index) => {
  console.log(`Stage ${index + 1} delta-v: ${dv.toFixed(2)} m/s`);
});

// Calculate total impulse
const totalImpulse = analyzer.calculateTotalImpulse();
console.log(`Total impulse: ${totalImpulse.toFixed(2)} N·s`);

// Estimate apogee
const apogee = analyzer.estimateApogee();
console.log(`Estimated apogee: ${apogee.toFixed(2)} m`);
```

### Structural Analysis

Validate structural integrity:

```typescript
import { StructuralAnalyzer } from './analysis/StructuralAnalyzer';

const structural = new StructuralAnalyzer(design);

// Calculate maximum stress
const stressAnalysis = structural.analyzeStress({
  maxThrust: 5000,
  maxAcceleration: 150 // m/s²
});

console.log(`Max axial stress: ${stressAnalysis.axialStress.toFixed(2)} MPa`);
console.log(`Max bending stress: ${stressAnalysis.bendingStress.toFixed(2)} MPa`);
console.log(`Safety factor: ${stressAnalysis.safetyFactor.toFixed(2)}`);

// Check buckling
const bucklingAnalysis = structural.analyzeBuckling();
if (bucklingAnalysis.stable) {
  console.log('Structure is stable against buckling');
} else {
  console.warn('Warning: Potential buckling issues detected');
}

// Validate against design criteria
const validation = structural.validate({
  minSafetyFactor: 2.0,
  maxStress: 200 // MPa
});

if (validation.passed) {
  console.log('Structural validation passed');
} else {
  console.error('Validation failed:', validation.issues);
}
```

### CAD Generation

Generate CAD files for manufacturing:

```typescript
import { CADGenerator } from './manufacturing/CADGenerator';
import { ExportFormat } from './manufacturing/types';

const cadGen = new CADGenerator(design);

// Generate body tube CAD
const bodyTube = await cadGen.generateBodyTube({
  stage: 1,
  includeFinSlots: true,
  finCount: 4
});

// Export as STEP file
await cadGen.export(bodyTube, {
  format: ExportFormat.STEP,
  filename: 'body-tube-stage1.step',
  outputDir: './output/cad'
});

// Generate nose cone
const noseCone = await cadGen.generateNoseCone({
  type: "ogive",
  bluntnessRatio: 0.1
});

await cadGen.export(noseCone, {
  format: ExportFormat.STL,
  filename: 'nose-cone.stl',
  outputDir: './output/cad'
});

// Generate fins
const fins = await cadGen.generateFins({
  count: 4,
  profile: "trapezoidal",
  rootChord: 0.15, // meters
  tipChord: 0.05,
  span: 0.1,
  sweepAngle: 30 // degrees
});

await cadGen.export(fins, {
  format: ExportFormat.DXF,
  filename: 'fins.dxf',
  outputDir: './output/cad'
});
```

### Manufacturing Planning

Create manufacturing workflows:

```typescript
import { ManufacturingPlanner } from './manufacturing/ManufacturingPlanner';

const planner = new ManufacturingPlanner(design);

// Generate bill of materials
const bom = planner.generateBOM();
console.log('Bill of Materials:');
bom.items.forEach(item => {
  console.log(`- ${item.name}: ${item.quantity} ${item.unit}`);
  console.log(`  Material: ${item.material}`);
  console.log(`  Process: ${item.manufacturingProcess}`);
});

// Create manufacturing steps
const workflow = planner.generateWorkflow();
workflow.steps.forEach((step, index) => {
  console.log(`Step ${index + 1}: ${step.name}`);
  console.log(`  Duration: ${step.estimatedTime} hours`);
  console.log(`  Tools: ${step.requiredTools.join(', ')}`);
  console.log(`  Instructions: ${step.instructions}`);
});

// Estimate costs
const costEstimate = planner.estimateCost({
  materialCostPerKg: {
    'aluminum-6061': 25,
    'fiberglass': 15,
    'carbon-fiber': 80
  },
  laborRate: 30, // per hour
  overheadFactor: 1.2
});

console.log(`Total material cost: $${costEstimate.materials.toFixed(2)}`);
console.log(`Total labor cost: $${costEstimate.labor.toFixed(2)}`);
console.log(`Total cost: $${costEstimate.total.toFixed(2)}`);
```

### Recovery System Design

Design parachute and recovery systems:

```typescript
import { RecoveryDesigner } from './design/RecoveryDesigner';

const recovery = new RecoveryDesigner({
  rocketMass: design.totalMass,
  targetDescentRate: 5, // m/s
  deploymentAltitude: 500 // meters
});

// Design main parachute
const mainChute = recovery.designParachute({
  type: "main",
  material: "nylon",
  descentRate: 5
});

console.log(`Main parachute diameter: ${mainChute.diameter.toFixed(2)} m`);
console.log(`Drag coefficient: ${mainChute.dragCoefficient}`);

// Design drogue parachute
const drogueChute = recovery.designParachute({
  type: "drogue",
  material: "nylon",
  descentRate: 20
});

console.log(`Drogue parachute diameter: ${drogueChute.diameter.toFixed(2)} m`);

// Calculate deployment shock
const shockLoad = recovery.calculateDeploymentShock({
  velocity: 100, // m/s
  parachute: mainChute
});

console.log(`Deployment shock load: ${shockLoad.toFixed(2)} N`);
```

## Configuration

Create a configuration file `rocket.config.ts`:

```typescript
import { RocketConfig } from './types';

export const config: RocketConfig = {
  design: {
    defaultMaterial: "aluminum-6061",
    safetyFactor: 2.5,
    maxDiameter: 0.20, // meters
    minWallThickness: 0.002 // meters
  },
  simulation: {
    atmosphereModel: "standard",
    defaultTimeStep: 0.01,
    windModel: "logarithmic",
    turbulenceEnabled: true
  },
  manufacturing: {
    outputDirectory: "./output",
    cadFormats: ["STEP", "STL", "DXF"],
    precision: 0.001, // meters
    generateDrawings: true
  },
  analysis: {
    enableStructuralAnalysis: true,
    enableAerodynamicAnalysis: true,
    enableStabilityAnalysis: true
  }
};
```

## Common Workflows

### Complete Rocket Design Workflow

```typescript
import { RocketDesigner } from './design/RocketDesigner';
import { TrajectorySimulator } from './simulation/TrajectorySimulator';
import { StructuralAnalyzer } from './analysis/StructuralAnalyzer';
import { CADGenerator } from './manufacturing/CADGenerator';
import { ManufacturingPlanner } from './manufacturing/ManufacturingPlanner';

async function completeDesignWorkflow() {
  // Step 1: Create initial design
  const designer = new RocketDesigner({
    name: "SZ22-Beta",
    stages: 2,
    payload: 75,
    targetAltitude: 150000,
    diameter: 0.18
  });
  
  const design = await designer.generateDesign();
  
  // Step 2: Simulate trajectory
  const simulator = new TrajectorySimulator();
  const trajectory = await simulator.simulate(design, {
    timeStep: 0.01,
    maxTime: 300,
    launchAngle: 85
  });
  
  if (trajectory.maxAltitude < design.targetAltitude * 0.9) {
    console.warn('Target altitude not achieved, optimizing...');
    await designer.optimize({ targetMetric: 'altitude' });
  }
  
  // Step 3: Structural validation
  const structural = new StructuralAnalyzer(design);
  const validation = structural.validate({
    minSafetyFactor: 2.0,
    maxStress: 200
  });
  
  if (!validation.passed) {
    console.error('Structural validation failed:', validation.issues);
    return;
  }
  
  // Step 4: Generate CAD files
  const cadGen = new CADGenerator(design);
  await cadGen.generateAll({
    outputDir: './output/cad',
    formats: ['STEP', 'STL']
  });
  
  // Step 5: Create manufacturing plan
  const planner = new ManufacturingPlanner(design);
  const bom = planner.generateBOM();
  const workflow = planner.generateWorkflow();
  
  // Export documentation
  await planner.exportDocumentation({
    outputDir: './output/docs',
    includeDrawings: true,
    includeBOM: true,
    includeAssemblyInstructions: true
  });
  
  console.log('Complete design workflow finished successfully');
}
```

### Optimization Loop

```typescript
import { DesignOptimizer } from './design/DesignOptimizer';

async function optimizeRocket() {
  const optimizer = new DesignOptimizer({
    objectives: ['maxAltitude', 'minCost', 'minMass'],
    constraints: {
      maxDiameter: 0.20,
      maxLength: 2.5,
      minSafetyFactor: 2.0,
      budget: 5000 // dollars
    }
  });
  
  // Define parameter ranges
  const parameterSpace = {
    stages: [1, 2, 3],
    diameter: [0.10, 0.20],
    engineThrust: [2000, 8000],
    propellantMass: [5, 15]
  };
  
  // Run optimization
  const results = await optimizer.optimize(parameterSpace, {
    maxIterations: 100,
    algorithm: 'genetic',
    populationSize: 50
  });
  
  console.log('Optimal design found:');
  console.log(`Altitude: ${results.best.altitude.toFixed(2)} m`);
  console.log(`Cost: $${results.best.cost.toFixed(2)}`);
  console.log(`Mass: ${results.best.mass.toFixed(2)} kg`);
  
  return results.best.design;
}
```

## Environment Variables

Configure the program using environment variables:

```bash
# Simulation settings
ATMOSPHERE_MODEL=standard
ENABLE_WIND_SIMULATION=true
DEFAULT_TIME_STEP=0.01

# CAD export settings
CAD_OUTPUT_DIR=./output/cad
CAD_PRECISION=0.001
CAD_DEFAULT_FORMAT=STEP

# Manufacturing settings
BOM_TEMPLATE_PATH=./templates/bom.xlsx
MATERIAL_DATABASE_PATH=./data/materials.json

# Cost estimation
DEFAULT_LABOR_RATE=30
DEFAULT_OVERHEAD_FACTOR=1.2
```

## Troubleshooting

### Simulation Convergence Issues

```typescript
// If simulation doesn't converge, reduce time step
const trajectory = await simulator.simulate(design, {
  timeStep: 0.001, // Smaller time step
  maxTime: 300,
  convergenceTolerance: 1e-6
});
```

### CAD Export Failures

```typescript
try {
  await cadGen.export(component, {
    format: ExportFormat.STEP,
    filename: 'component.step',
    outputDir: './output/cad'
  });
} catch (error) {
  console.error('CAD export failed:', error);
  // Try alternative format
  await cadGen.export(component, {
    format: ExportFormat.STL,
    filename: 'component.stl',
    outputDir: './output/cad'
  });
}
```

### Structural Validation Warnings

```typescript
const validation = structural.validate({
  minSafetyFactor: 2.0,
  maxStress: 200
});

if (!validation.passed) {
  validation.issues.forEach(issue => {
    console.log(`Issue: ${issue.description}`);
    console.log(`Suggested fix: ${issue.recommendation}`);
  });
  
  // Apply automatic fixes if possible
  const fixedDesign = await designer.applyFixes(validation.issues);
}
```

This skill enables AI coding agents to help developers design, simulate, analyze, and plan manufacturing for model rockets using this TypeScript-based automation program.
