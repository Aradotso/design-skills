---
name: rocket-design-manufacturing-automation-shenzhen22
description: Automate rocket design and manufacturing processes with TypeScript-based simulation and CAD generation tools
triggers:
  - "help me design a rocket"
  - "automate rocket manufacturing workflow"
  - "generate rocket CAD models"
  - "simulate rocket flight dynamics"
  - "calculate rocket propulsion parameters"
  - "optimize rocket design specifications"
  - "create rocket manufacturing automation"
  - "build rocket design pipeline"
---

# Rocket Design and Manufacturing Automation

> Skill by [ara.so](https://ara.so) — Design Skills collection.

This project provides TypeScript-based automation tools for rocket design, manufacturing workflows, and flight simulation developed by students at Shenzhen 22nd High School. It enables parametric rocket design, structural analysis, propulsion calculations, and CAD file generation.

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

The project is organized into modules for different rocket design aspects:

- **Design Module**: Parametric rocket geometry and configuration
- **Propulsion Module**: Engine calculations and thrust profiles
- **Structure Module**: Material selection and stress analysis
- **Aerodynamics Module**: Drag coefficients and flight simulation
- **Manufacturing Module**: G-code generation and toolpath planning
- **CAD Module**: 3D model export (STEP, STL formats)

## Core Usage Patterns

### Basic Rocket Design

```typescript
import { RocketDesigner } from './src/design/RocketDesigner';
import { RocketConfig } from './src/types/RocketConfig';

// Define rocket configuration
const config: RocketConfig = {
  name: "Student Rocket Alpha",
  length: 1.5, // meters
  diameter: 0.1, // meters
  bodyTube: {
    material: "fiberglass",
    thickness: 0.003, // meters
    length: 1.2
  },
  noseCone: {
    type: "ogive",
    length: 0.3,
    material: "abs_plastic"
  },
  fins: {
    count: 3,
    rootChord: 0.15,
    tipChord: 0.08,
    span: 0.1,
    sweepAngle: 30, // degrees
    material: "plywood"
  },
  motor: {
    type: "D12-5",
    impulse: 20, // Newton-seconds
    burnTime: 1.7, // seconds
    thrust: 12 // Newtons average
  }
};

// Create rocket design
const designer = new RocketDesigner(config);
const rocket = designer.generate();

console.log(`Mass: ${rocket.mass.toFixed(2)} kg`);
console.log(`Center of Gravity: ${rocket.centerOfGravity.toFixed(3)} m`);
console.log(`Center of Pressure: ${rocket.centerOfPressure.toFixed(3)} m`);
console.log(`Stability Margin: ${rocket.stabilityMargin.toFixed(2)} calibers`);
```

### Flight Simulation

```typescript
import { FlightSimulator } from './src/simulation/FlightSimulator';
import { EnvironmentConfig } from './src/types/Environment';

// Define environmental conditions
const environment: EnvironmentConfig = {
  temperature: 288.15, // Kelvin
  pressure: 101325, // Pascals
  windSpeed: 3, // m/s
  windDirection: 45, // degrees
  launchAngle: 85, // degrees from horizontal
  launchAzimuth: 0 // degrees
};

// Run simulation
const simulator = new FlightSimulator(rocket, environment);
const trajectory = simulator.simulate({
  timeStep: 0.01, // seconds
  maxTime: 60, // seconds
  terminateOnGround: true
});

// Extract key flight metrics
console.log(`Apogee: ${trajectory.maxAltitude.toFixed(1)} m`);
console.log(`Max Velocity: ${trajectory.maxVelocity.toFixed(1)} m/s`);
console.log(`Max Acceleration: ${trajectory.maxAcceleration.toFixed(1)} m/s²`);
console.log(`Flight Time: ${trajectory.flightTime.toFixed(1)} s`);
console.log(`Landing Distance: ${trajectory.landingDistance.toFixed(1)} m`);
```

### Propulsion Calculations

```typescript
import { PropulsionAnalyzer } from './src/propulsion/PropulsionAnalyzer';
import { MotorConfig } from './src/types/Motor';

const motorConfig: MotorConfig = {
  type: "custom",
  propellant: "KNSB", // Potassium Nitrate / Sorbitol
  grainGeometry: "BATES",
  coreCount: 3,
  outerDiameter: 0.029, // meters
  corediameter: 0.012, // meters
  grainLength: 0.07, // meters
  caseMaterial: "aluminum",
  caseThickness: 0.002
};

const analyzer = new PropulsionAnalyzer(motorConfig);
const performance = analyzer.calculate();

console.log(`Total Impulse: ${performance.totalImpulse.toFixed(1)} Ns`);
console.log(`Specific Impulse: ${performance.specificImpulse.toFixed(1)} s`);
console.log(`Peak Thrust: ${performance.peakThrust.toFixed(1)} N`);
console.log(`Burn Time: ${performance.burnTime.toFixed(2)} s`);
console.log(`Pressure Curve:`);
performance.pressureCurve.forEach((point, i) => {
  if (i % 10 === 0) {
    console.log(`  t=${point.time.toFixed(2)}s: ${point.pressure.toFixed(0)} kPa`);
  }
});
```

### Structural Analysis

```typescript
import { StructuralAnalyzer } from './src/structure/StructuralAnalyzer';

const analyzer = new StructuralAnalyzer(rocket);

// Calculate loads during flight
const loads = analyzer.calculateLoads(trajectory);

// Check structural integrity
const safetyFactors = analyzer.analyzeSafety(loads);

console.log('Safety Factors:');
console.log(`  Body Tube: ${safetyFactors.bodyTube.toFixed(2)}`);
console.log(`  Fins: ${safetyFactors.fins.toFixed(2)}`);
console.log(`  Nose Cone: ${safetyFactors.noseCone.toFixed(2)}`);

if (safetyFactors.minSafetyFactor < 1.5) {
  console.warn('Warning: Insufficient safety margin!');
}
```

### CAD Export

```typescript
import { CADExporter } from './src/cad/CADExporter';

const exporter = new CADExporter(rocket);

// Generate 3D models
await exporter.exportSTL('./output/rocket.stl', {
  resolution: 'high',
  units: 'millimeters'
});

await exporter.exportSTEP('./output/rocket.step', {
  includeAssembly: true,
  separateComponents: true
});

// Generate 2D technical drawings
await exporter.exportDXF('./output/fins.dxf', {
  component: 'fins',
  projection: 'top',
  scale: 1.0
});

console.log('CAD files exported successfully');
```

### Manufacturing Automation

```typescript
import { ManufacturingPlanner } from './src/manufacturing/ManufacturingPlanner';
import { CNCGenerator } from './src/manufacturing/CNCGenerator';

// Create manufacturing plan
const planner = new ManufacturingPlanner(rocket);
const plan = planner.generatePlan();

console.log('Manufacturing Steps:');
plan.steps.forEach((step, i) => {
  console.log(`${i + 1}. ${step.operation}: ${step.component}`);
  console.log(`   Tool: ${step.tool}, Material: ${step.material}`);
  console.log(`   Estimated Time: ${step.estimatedTime} min`);
});

// Generate CNC toolpaths for fin cutting
const cncGen = new CNCGenerator();
const gcode = cncGen.generateFinToolpath(rocket.fins, {
  material: 'plywood',
  thickness: 0.006, // meters
  toolDiameter: 0.003,
  feedRate: 1000, // mm/min
  spindleSpeed: 12000, // RPM
  passes: 3
});

await cncGen.saveGCode('./output/fins.nc', gcode);
console.log('G-code generated for CNC router');
```

### Optimization Workflow

```typescript
import { DesignOptimizer } from './src/optimization/DesignOptimizer';

// Define optimization objectives
const optimizer = new DesignOptimizer({
  objectives: {
    maximizeAltitude: 1.0,
    minimizeMass: 0.3,
    maximizeStability: 0.5
  },
  constraints: {
    maxLength: 2.0, // meters
    maxDiameter: 0.15, // meters
    minStabilityMargin: 1.5, // calibers
    maxMass: 2.0 // kg
  },
  variables: {
    finSpan: { min: 0.08, max: 0.15 },
    finRootChord: { min: 0.1, max: 0.2 },
    noseConeLength: { min: 0.2, max: 0.4 },
    bodyTubeLength: { min: 0.8, max: 1.5 }
  }
});

// Run optimization (genetic algorithm)
const optimized = await optimizer.optimize({
  population: 50,
  generations: 100,
  mutationRate: 0.1,
  crossoverRate: 0.7
});

console.log('Optimized Design:');
console.log(`  Predicted Apogee: ${optimized.predictedAltitude.toFixed(1)} m`);
console.log(`  Total Mass: ${optimized.mass.toFixed(2)} kg`);
console.log(`  Stability: ${optimized.stabilityMargin.toFixed(2)} calibers`);
```

## Configuration

Create a `rocket.config.json` file for project-wide settings:

```json
{
  "units": {
    "length": "meters",
    "mass": "kilograms",
    "force": "newtons"
  },
  "simulation": {
    "defaultTimeStep": 0.01,
    "atmosphereModel": "standard",
    "dragModel": "barrowman"
  },
  "manufacturing": {
    "defaultMaterial": "fiberglass",
    "tolerance": 0.0001,
    "outputFormats": ["stl", "step", "gcode"]
  },
  "safety": {
    "minimumSafetyFactor": 1.5,
    "maxWindSpeed": 10,
    "launchRodLength": 1.2
  }
}
```

## CLI Commands

If the project includes CLI tools:

```bash
# Design a rocket from config file
npm run design -- --config ./configs/alpha.json --output ./designs/

# Run flight simulation
npm run simulate -- --rocket ./designs/alpha.json --environment ./env/standard.json

# Generate manufacturing files
npm run manufacture -- --design ./designs/alpha.json --output ./manufacturing/

# Optimize design parameters
npm run optimize -- --config ./configs/optimize.json --iterations 100

# Export CAD models
npm run export -- --design ./designs/alpha.json --format stl,step --output ./cad/
```

## Common Patterns

### Batch Design Generation

```typescript
import { BatchDesigner } from './src/batch/BatchDesigner';

const parameterSweep = {
  finCount: [3, 4],
  finSpan: [0.08, 0.10, 0.12],
  noseConeType: ['ogive', 'conical', 'elliptical']
};

const batch = new BatchDesigner(baseConfig);
const designs = batch.generateVariations(parameterSweep);

designs.forEach((design, i) => {
  console.log(`Design ${i + 1}: Stability ${design.stabilityMargin.toFixed(2)}`);
});
```

### Material Database Integration

```typescript
import { MaterialDatabase } from './src/materials/MaterialDatabase';

const materials = new MaterialDatabase();

const fiberglass = materials.get('fiberglass');
console.log(`Density: ${fiberglass.density} kg/m³`);
console.log(`Tensile Strength: ${fiberglass.tensileStrength} MPa`);
console.log(`Cost: ${fiberglass.costPerKg} USD/kg`);
```

## Troubleshooting

### Unstable Rocket Design

If stability margin is negative or too low:

```typescript
// Check stability
if (rocket.stabilityMargin < 1.0) {
  console.error('Unstable design!');
  console.log('Suggestions:');
  console.log('- Increase fin span');
  console.log('- Move fins further aft');
  console.log('- Reduce nose cone length');
  console.log('- Add nose weight');
}
```

### Simulation Divergence

If flight simulation produces unrealistic results:

```typescript
// Reduce time step for better accuracy
const trajectory = simulator.simulate({
  timeStep: 0.001, // Smaller time step
  dragCoefficient: 0.45, // Verify drag coefficient
  enableLogging: true // Debug output
});
```

### CAD Export Errors

Ensure output directories exist and handle errors:

```typescript
import * as fs from 'fs';

const outputDir = './output/cad';
if (!fs.existsSync(outputDir)) {
  fs.mkdirSync(outputDir, { recursive: true });
}

try {
  await exporter.exportSTL(`${outputDir}/rocket.stl`);
} catch (error) {
  console.error('Export failed:', error.message);
}
```

### Performance Optimization

For large design spaces or high-resolution simulations:

```typescript
// Use parallel processing
import { Worker } from 'worker_threads';

const results = await Promise.all(
  designs.map(design => 
    runSimulationInWorker(design)
  )
);
```

## Best Practices

1. **Always validate stability** before manufacturing
2. **Use safety factors** of at least 1.5 for structural components
3. **Verify propulsion calculations** with test data
4. **Simulate multiple wind conditions** before launch
5. **Export designs** in multiple formats for compatibility
6. **Document all design decisions** for reproducibility
