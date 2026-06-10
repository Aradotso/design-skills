---
name: rocket-design-manufacturing-automation-shenzhen22
description: Automated rocket design and manufacturing program for model rocketry projects developed by Shenzhen 22nd High School students
triggers:
  - "help me design a rocket"
  - "automate rocket manufacturing"
  - "calculate rocket trajectory"
  - "generate rocket CAD files"
  - "simulate rocket flight"
  - "optimize rocket design parameters"
  - "create rocket manufacturing specifications"
  - "design model rocket components"
---

# Rocket Design and Manufacturing Automation

> Skill by [ara.so](https://ara.so) — Design Skills collection.

This TypeScript-based automation program provides tools for designing, simulating, and generating manufacturing specifications for model rockets. Developed by students at Shenzhen 22nd High School, it streamlines the rocket design workflow from initial parameters to production-ready specifications.

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

## Core Concepts

The program handles three primary domains:

1. **Rocket Design**: Parametric design of rocket components (body tube, nose cone, fins, motor mount)
2. **Flight Simulation**: Physics-based trajectory and stability calculations
3. **Manufacturing Automation**: Generate CAD files and cutting specifications

## Basic Usage

### Designing a Rocket

```typescript
import { RocketDesigner, RocketParameters } from './src/design/RocketDesigner';

// Define rocket parameters
const params: RocketParameters = {
  bodyTube: {
    outerDiameter: 50, // mm
    innerDiameter: 48, // mm
    length: 500, // mm
    material: 'cardboard'
  },
  noseCone: {
    type: 'ogive', // 'cone', 'ogive', 'parabolic'
    length: 150, // mm
    baseDiameter: 50, // mm
    material: 'plastic'
  },
  fins: {
    count: 3,
    rootChord: 100, // mm
    tipChord: 50, // mm
    span: 80, // mm
    sweepAngle: 30, // degrees
    thickness: 3, // mm
    material: 'balsa'
  },
  motorMount: {
    diameter: 24, // mm
    length: 70, // mm
    overhang: 10 // mm
  }
};

const designer = new RocketDesigner(params);
const rocket = designer.generateDesign();

console.log(`Rocket mass: ${rocket.totalMass}g`);
console.log(`Center of gravity: ${rocket.centerOfGravity}mm from nose`);
console.log(`Center of pressure: ${rocket.centerOfPressure}mm from nose`);
console.log(`Stability margin: ${rocket.stabilityMargin} calibers`);
```

### Flight Simulation

```typescript
import { FlightSimulator, SimulationConfig } from './src/simulation/FlightSimulator';
import { Motor } from './src/motors/MotorDatabase';

// Configure simulation
const simConfig: SimulationConfig = {
  timestep: 0.01, // seconds
  windSpeed: 5, // m/s
  launchAngle: 90, // degrees (vertical)
  launchAltitude: 0, // meters above sea level
  temperature: 20, // Celsius
  pressure: 101325 // Pa (sea level)
};

// Select motor
const motor: Motor = {
  designation: 'C6-5',
  totalImpulse: 10, // Ns
  averageThrust: 5.5, // N
  burnTime: 1.8, // seconds
  propellantMass: 12.5, // grams
  totalMass: 24, // grams
  diameter: 24, // mm
  length: 70 // mm
};

const simulator = new FlightSimulator(rocket, motor, simConfig);
const trajectory = simulator.run();

console.log(`Maximum altitude: ${trajectory.maxAltitude}m`);
console.log(`Maximum velocity: ${trajectory.maxVelocity}m/s`);
console.log(`Flight time: ${trajectory.totalTime}s`);
console.log(`Landing distance: ${trajectory.landingDistance}m from pad`);
```

### Generating Manufacturing Files

```typescript
import { ManufacturingGenerator } from './src/manufacturing/ManufacturingGenerator';
import { CADExporter } from './src/manufacturing/CADExporter';

const generator = new ManufacturingGenerator(rocket);

// Generate fin cutting template
const finTemplate = generator.generateFinTemplate();
finTemplate.exportDXF('./output/fin_template.dxf');
finTemplate.exportSVG('./output/fin_template.svg');

// Generate body tube cutting pattern
const tubePattern = generator.generateTubePattern();
tubePattern.exportPDF('./output/tube_pattern.pdf');

// Generate complete assembly instructions
const assembly = generator.generateAssemblyInstructions();
assembly.exportPDF('./output/assembly_instructions.pdf');

// Export 3D CAD model
const cadExporter = new CADExporter(rocket);
cadExporter.exportSTEP('./output/rocket_assembly.step');
cadExporter.exportSTL('./output/rocket_assembly.stl');
```

## Advanced Features

### Stability Analysis

```typescript
import { StabilityAnalyzer } from './src/analysis/StabilityAnalyzer';

const analyzer = new StabilityAnalyzer(rocket);

// Calculate stability at different Mach numbers
const stabilityReport = analyzer.analyzeStabilityRange({
  machStart: 0,
  machEnd: 0.5,
  steps: 50
});

// Check for stability issues
if (stabilityReport.isStable) {
  console.log('Rocket is stable throughout flight');
} else {
  console.warn('Stability issues detected:');
  stabilityReport.warnings.forEach(w => console.warn(`- ${w}`));
}

// Barrowman method for CP calculation
const cpLocation = analyzer.calculateCenterOfPressure('barrowman');
console.log(`CP location (Barrowman): ${cpLocation}mm`);
```

### Optimization

```typescript
import { RocketOptimizer, OptimizationGoal } from './src/optimization/RocketOptimizer';

const optimizer = new RocketOptimizer(params);

// Optimize for maximum altitude
const optimized = optimizer.optimize({
  goal: OptimizationGoal.MAX_ALTITUDE,
  constraints: {
    maxDiameter: 75, // mm
    maxLength: 800, // mm
    maxMass: 200, // grams
    minStability: 1.5, // calibers
    maxStability: 3.0 // calibers
  },
  iterations: 1000
});

console.log(`Optimized design achieves ${optimized.predictedAltitude}m`);
console.log(`Changes applied: ${optimized.modifications.length}`);
```

### Material Database

```typescript
import { MaterialDatabase, Material } from './src/materials/MaterialDatabase';

const materials = new MaterialDatabase();

// Get material properties
const balsa = materials.getMaterial('balsa_wood');
console.log(`Balsa density: ${balsa.density}g/cm³`);
console.log(`Balsa strength: ${balsa.tensileStrength}MPa`);

// Add custom material
materials.addMaterial({
  name: 'carbon_fiber_3k',
  density: 1.6, // g/cm³
  tensileStrength: 3500, // MPa
  youngsModulus: 230, // GPa
  cost: 45.00 // USD per square meter
});

// Compare materials for fin application
const comparison = materials.compareForApplication('fin', [
  'balsa_wood',
  'plywood',
  'carbon_fiber_3k'
]);
```

## Configuration

Create a `rocket.config.json` file in your project root:

```json
{
  "units": {
    "length": "mm",
    "mass": "g",
    "force": "N"
  },
  "simulation": {
    "defaultTimestep": 0.01,
    "dragModel": "barrowman",
    "windModel": "constant"
  },
  "manufacturing": {
    "defaultMaterial": "cardboard",
    "tolerance": 0.5,
    "outputFormats": ["dxf", "svg", "pdf"]
  },
  "safety": {
    "maxMotorImpulse": 160,
    "minStabilityMargin": 1.0,
    "maxStabilityMargin": 4.0
  }
}
```

Load configuration:

```typescript
import { Config } from './src/config/Config';

const config = Config.loadFromFile('./rocket.config.json');
Config.setGlobal(config);
```

## Common Patterns

### Complete Design Workflow

```typescript
import { DesignWorkflow } from './src/workflow/DesignWorkflow';

const workflow = new DesignWorkflow();

// Step 1: Initial design
const design = workflow.createDesign(params);

// Step 2: Validate design
const validation = workflow.validate(design);
if (!validation.isValid) {
  console.error('Design validation failed:', validation.errors);
  process.exit(1);
}

// Step 3: Simulate flight
const simulation = workflow.simulate(design, motor);

// Step 4: Optimize if needed
if (simulation.maxAltitude < 100) {
  design = workflow.optimize(design, { targetAltitude: 100 });
}

// Step 5: Generate manufacturing files
workflow.generateManufacturingFiles(design, './output');

// Step 6: Export report
workflow.exportReport(design, simulation, './output/report.pdf');
```

### Multi-Stage Rockets

```typescript
import { MultiStageDesigner } from './src/design/MultiStageDesigner';

const stage1 = new RocketDesigner(stage1Params).generateDesign();
const stage2 = new RocketDesigner(stage2Params).generateDesign();

const multiStage = new MultiStageDesigner();
multiStage.addStage(stage1, motor1, { separationDelay: 3 });
multiStage.addStage(stage2, motor2, { separationDelay: 5 });

const trajectory = multiStage.simulate();
console.log(`Two-stage altitude: ${trajectory.maxAltitude}m`);
```

## Troubleshooting

### Unstable Rocket Design

```typescript
// Problem: Stability margin too low
if (rocket.stabilityMargin < 1.0) {
  // Solution 1: Increase fin size
  params.fins.span += 20;
  params.fins.rootChord += 20;
  
  // Solution 2: Move CG forward
  params.noseCone.ballastMass = 15; // grams
  
  // Solution 3: Lengthen body tube
  params.bodyTube.length += 100;
}
```

### Simulation Convergence Issues

```typescript
// Reduce timestep for better accuracy
simConfig.timestep = 0.001; // from 0.01

// Enable adaptive timestep
simConfig.adaptiveTimestep = true;
simConfig.maxTimestep = 0.01;
simConfig.minTimestep = 0.0001;
```

### CAD Export Errors

```typescript
try {
  cadExporter.exportSTEP('./output/rocket.step');
} catch (error) {
  if (error.code === 'INVALID_GEOMETRY') {
    // Simplify geometry
    cadExporter.setTolerance(0.1);
    cadExporter.simplifyGeometry(true);
    cadExporter.exportSTEP('./output/rocket.step');
  }
}
```

### Memory Issues with Large Simulations

```typescript
// Use streaming for large trajectory data
const simulator = new FlightSimulator(rocket, motor, {
  ...simConfig,
  streaming: true,
  outputFile: './output/trajectory.csv'
});

// Process in chunks
simulator.on('data', (dataPoint) => {
  // Process each point individually
  processDataPoint(dataPoint);
});

await simulator.run();
```

## Environment Variables

```bash
# Set output directory
export ROCKET_OUTPUT_DIR=./output

# Enable debug logging
export ROCKET_DEBUG=true

# Set material database path
export ROCKET_MATERIALS_DB=./data/materials.json

# Set motor database path
export ROCKET_MOTORS_DB=./data/motors.json
```

## Testing Your Design

```typescript
import { DesignTester } from './src/testing/DesignTester';

const tester = new DesignTester(rocket);

// Run all safety checks
const safetyReport = tester.runSafetyChecks();
console.log(`Safety score: ${safetyReport.score}/100`);

// Structural analysis
const structural = tester.analyzeStructuralIntegrity(motor);
console.log(`Max G-force: ${structural.maxGForce}G`);
console.log(`Safety factor: ${structural.safetyFactor}`);

// Wind sensitivity
const windTest = tester.testWindSensitivity({ maxWind: 20 });
console.log(`Safe launch wind: < ${windTest.maxSafeWind}m/s`);
```
