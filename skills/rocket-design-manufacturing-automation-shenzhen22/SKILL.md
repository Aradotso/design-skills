---
name: rocket-design-manufacturing-automation-shenzhen22
description: Automated rocket design and manufacturing workflow system built by high school students for aerospace engineering projects
triggers:
  - "help me design a rocket"
  - "automate rocket manufacturing process"
  - "use the Shenzhen22 rocket design program"
  - "create rocket CAD models automatically"
  - "calculate rocket performance parameters"
  - "generate rocket manufacturing specifications"
  - "design and simulate model rockets"
  - "automate aerospace design workflow"
---

# Rocket Design and Manufacturing Automation Program

> Skill by [ara.so](https://ara.so) — Design Skills collection.

This TypeScript-based automation program enables systematic rocket design, performance calculation, and manufacturing specification generation. Developed by students at Shenzhen 22nd High School, it provides tools for aerospace enthusiasts and educators to streamline the rocket engineering workflow from concept to production-ready specifications.

## What It Does

- **Automated Design Generation**: Creates rocket configurations based on mission parameters
- **Performance Calculations**: Computes thrust, delta-v, stability margins, and flight trajectories
- **CAD Integration**: Generates 3D models and technical drawings
- **Manufacturing Specs**: Outputs production-ready documentation and part lists
- **Simulation**: Predicts flight characteristics and validates designs

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
src/
├── design/          # Rocket design algorithms
├── calculations/    # Performance and stability calculations
├── cad/            # CAD model generation
├── manufacturing/  # Manufacturing specification outputs
├── simulation/     # Flight simulation modules
└── utils/          # Helper functions and constants
```

## Core Modules

### Design Module

Create rocket configurations with parametric inputs:

```typescript
import { RocketDesigner } from './design/RocketDesigner';
import { DesignParameters } from './types/DesignParameters';

const params: DesignParameters = {
  targetAltitude: 1000, // meters
  payloadMass: 0.5,     // kg
  diameter: 0.075,      // meters
  motorClass: 'E',
  stabilityMargin: 2.0  // calibers
};

const designer = new RocketDesigner();
const rocketConfig = designer.generateDesign(params);

console.log(`Rocket length: ${rocketConfig.totalLength}m`);
console.log(`Center of Gravity: ${rocketConfig.cg}m from nose`);
console.log(`Center of Pressure: ${rocketConfig.cp}m from nose`);
```

### Performance Calculations

Calculate flight characteristics:

```typescript
import { PerformanceCalculator } from './calculations/PerformanceCalculator';
import { Motor } from './types/Motor';

const calculator = new PerformanceCalculator();

const motor: Motor = {
  impulseClass: 'E',
  totalImpulse: 40,     // N·s
  averageThrust: 20,    // N
  burnTime: 2.0,        // seconds
  propellantMass: 0.024 // kg
};

const performance = calculator.calculate({
  rocket: rocketConfig,
  motor: motor,
  launchAngle: 90,      // degrees
  windSpeed: 5          // m/s
});

console.log(`Predicted apogee: ${performance.maxAltitude}m`);
console.log(`Max velocity: ${performance.maxVelocity}m/s`);
console.log(`Flight time: ${performance.totalFlightTime}s`);
```

### Stability Analysis

Verify rocket stability:

```typescript
import { StabilityAnalyzer } from './calculations/StabilityAnalyzer';

const analyzer = new StabilityAnalyzer();
const stability = analyzer.analyze(rocketConfig);

if (stability.isStable) {
  console.log(`Stability margin: ${stability.staticMargin} calibers`);
  console.log('✓ Design is stable');
} else {
  console.warn('⚠ Design is unstable');
  console.log(`Suggestions: ${stability.recommendations.join(', ')}`);
}
```

### CAD Generation

Generate 3D models and technical drawings:

```typescript
import { CADGenerator } from './cad/CADGenerator';
import { ExportFormat } from './types/ExportFormat';

const cadGen = new CADGenerator();

// Generate 3D model
const model3D = cadGen.generate3DModel(rocketConfig, {
  format: ExportFormat.STEP,
  includeInternals: true,
  detailLevel: 'high'
});

model3D.exportToFile('./output/rocket_model.step');

// Generate technical drawing
const drawing = cadGen.generateDrawing(rocketConfig, {
  views: ['front', 'side', 'cross-section'],
  dimensions: true,
  scale: '1:5'
});

drawing.exportToFile('./output/technical_drawing.pdf');
```

### Manufacturing Specifications

Generate production documentation:

```typescript
import { ManufacturingSpecGenerator } from './manufacturing/ManufacturingSpecGenerator';

const specGen = new ManufacturingSpecGenerator();
const specs = specGen.generate(rocketConfig);

// Bill of Materials
console.log('Bill of Materials:');
specs.bom.forEach(item => {
  console.log(`- ${item.partName}: ${item.quantity} ${item.unit}`);
  console.log(`  Material: ${item.material}`);
  console.log(`  Dimensions: ${item.dimensions}`);
});

// Export manufacturing instructions
specs.exportInstructions('./output/manufacturing_instructions.pdf');

// Export CNC toolpaths
specs.exportToolpaths('./output/toolpaths/', {
  format: 'gcode',
  machine: 'cnc_mill'
});
```

## Configuration

Create a `rocket.config.ts` file:

```typescript
import { RocketConfig } from './types/RocketConfig';

export const config: RocketConfig = {
  // Design defaults
  design: {
    defaultDiameter: 0.075,      // meters
    defaultStabilityMargin: 2.0,  // calibers
    finCount: 3,
    finThickness: 0.003,          // meters
    noseConeType: 'ogive',
    bodyTubeMaterial: 'cardboard',
    finMaterial: 'balsa'
  },
  
  // Calculation settings
  calculations: {
    gravitationalAccel: 9.81,     // m/s²
    airDensity: 1.225,            // kg/m³
    dragCoefficientMethod: 'barrowman',
    timestep: 0.01                // seconds
  },
  
  // CAD settings
  cad: {
    defaultFormat: 'STEP',
    precision: 0.001,             // meters
    tessellationQuality: 'high'
  },
  
  // Manufacturing settings
  manufacturing: {
    toleranceClass: 'medium',
    includeMaterialSpecs: true,
    includeAssemblyInstructions: true
  }
};
```

## Common Workflows

### Complete Design Pipeline

```typescript
import { RocketPipeline } from './RocketPipeline';

const pipeline = new RocketPipeline();

// Define mission requirements
const mission = {
  targetAltitude: 500,
  payloadMass: 0.3,
  recoverySystem: 'parachute',
  motorClass: 'D'
};

// Run complete pipeline
const results = await pipeline.run(mission, {
  generateCAD: true,
  runSimulation: true,
  generateManufacturingDocs: true,
  outputDirectory: './output'
});

console.log(`Design complete: ${results.designId}`);
console.log(`Files generated: ${results.files.length}`);
```

### Iterative Optimization

```typescript
import { DesignOptimizer } from './design/DesignOptimizer';

const optimizer = new DesignOptimizer();

const optimized = optimizer.optimize({
  initialDesign: rocketConfig,
  objective: 'maximizeAltitude',
  constraints: {
    maxLength: 1.0,      // meters
    maxDiameter: 0.1,    // meters
    maxMass: 0.5,        // kg
    minStability: 1.5    // calibers
  },
  iterations: 100
});

console.log(`Optimized altitude: ${optimized.predictedAltitude}m`);
console.log(`Iterations: ${optimized.iterationCount}`);
```

### Batch Simulation

```typescript
import { BatchSimulator } from './simulation/BatchSimulator';

const simulator = new BatchSimulator();

// Test multiple configurations
const configurations = [
  { motorClass: 'D', finCount: 3 },
  { motorClass: 'D', finCount: 4 },
  { motorClass: 'E', finCount: 3 },
  { motorClass: 'E', finCount: 4 }
];

const results = await simulator.runBatch(configurations, {
  windSpeeds: [0, 5, 10],    // m/s
  launchAngles: [85, 90],    // degrees
  repetitions: 10
});

// Analyze results
results.forEach(result => {
  console.log(`Config: ${result.config.motorClass}, ${result.config.finCount} fins`);
  console.log(`Mean altitude: ${result.statistics.meanAltitude}m`);
  console.log(`Std deviation: ${result.statistics.stdDeviation}m`);
});
```

## Advanced Features

### Custom Material Properties

```typescript
import { MaterialLibrary } from './materials/MaterialLibrary';

const materials = new MaterialLibrary();

// Add custom material
materials.addMaterial({
  name: 'carbon_fiber_composite',
  density: 1600,           // kg/m³
  tensileStrength: 600e6,  // Pa
  youngsModulus: 70e9,     // Pa
  cost: 50                 // per kg
});

// Use in design
rocketConfig.bodyTube.material = 'carbon_fiber_composite';
```

### Flight Data Export

```typescript
import { FlightDataExporter } from './simulation/FlightDataExporter';

const exporter = new FlightDataExporter();

// Export to various formats
exporter.exportToCSV(performance.flightData, './output/flight_data.csv');
exporter.exportToJSON(performance.flightData, './output/flight_data.json');

// Export for analysis tools
exporter.exportForOpenRocket('./output/openrocket_import.ork');
exporter.exportForRASAero('./output/rasaero_import.xml');
```

## Troubleshooting

### Unstable Design

If stability analysis fails:

```typescript
// Check stability margin
if (stability.staticMargin < 1.0) {
  // Increase fin size
  rocketConfig.fins.rootChord *= 1.2;
  rocketConfig.fins.span *= 1.2;
  
  // Or move fins further back
  rocketConfig.fins.position += 0.05;
  
  // Re-analyze
  stability = analyzer.analyze(rocketConfig);
}
```

### Performance Issues

For large batch simulations:

```typescript
// Enable parallel processing
const simulator = new BatchSimulator({
  parallel: true,
  maxWorkers: 4
});

// Reduce precision for faster results
const quickCalc = new PerformanceCalculator({
  timestep: 0.1,  // Larger timestep
  atmosphericModel: 'simple'
});
```

### CAD Export Errors

If CAD generation fails:

```typescript
try {
  model3D.exportToFile('./output/rocket.step');
} catch (error) {
  console.error('CAD export failed:', error.message);
  
  // Try with lower detail
  const simpleModel = cadGen.generate3DModel(rocketConfig, {
    detailLevel: 'low',
    includeInternals: false
  });
  
  simpleModel.exportToFile('./output/rocket_simple.step');
}
```

## Environment Variables

```bash
# Optional configuration
ROCKET_OUTPUT_DIR=./output
ROCKET_TEMP_DIR=./temp
ROCKET_LOG_LEVEL=info
ROCKET_MATERIAL_DATABASE=./data/materials.json
```

## TypeScript Types

Key type definitions:

```typescript
interface RocketConfiguration {
  totalLength: number;
  diameter: number;
  mass: number;
  cg: number;  // Center of gravity
  cp: number;  // Center of pressure
  noseCone: NoseConeSpec;
  bodyTube: BodyTubeSpec;
  fins: FinSpec;
  recovery: RecoverySpec;
}

interface PerformanceResults {
  maxAltitude: number;
  maxVelocity: number;
  maxAcceleration: number;
  totalFlightTime: number;
  flightData: FlightDataPoint[];
}

interface FlightDataPoint {
  time: number;
  altitude: number;
  velocity: number;
  acceleration: number;
}
```

This skill enables AI agents to assist with complete rocket design workflows from initial parameters through manufacturing documentation.
