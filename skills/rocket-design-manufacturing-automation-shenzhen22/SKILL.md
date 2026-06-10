---
name: rocket-design-manufacturing-automation-shenzhen22
description: Automated rocket design and manufacturing program with TypeScript-based CAD generation and simulation tools
triggers:
  - design a rocket with automated manufacturing
  - generate rocket component specifications
  - simulate rocket performance parameters
  - automate rocket assembly workflow
  - create rocket design blueprints
  - calculate rocket propulsion requirements
  - export rocket manufacturing files
  - optimize rocket design parameters
---

# Rocket Design and Manufacturing Automation

> Skill by [ara.so](https://ara.so) — Design Skills collection.

This project provides an automated rocket design and manufacturing program developed by students from Shenzhen 22nd High School. It enables parametric rocket design, component specification generation, performance simulation, and automated manufacturing file export using TypeScript.

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

### Rocket Components

The system models rockets as assemblies of discrete components:
- **Nose Cone**: Aerodynamic tip with various profile options
- **Body Tube**: Main structural cylinder
- **Fins**: Stabilization surfaces
- **Motor Mount**: Engine attachment system
- **Recovery System**: Parachute/deployment mechanism

### Design Workflow

1. Define mission parameters (target altitude, payload mass)
2. Generate component specifications
3. Run stability and performance simulations
4. Export manufacturing files (CAD, G-code, assembly instructions)

## Basic Usage

### Creating a Rocket Design

```typescript
import { RocketDesigner, RocketConfig } from './src/designer';
import { MaterialType, NoseConeProfile } from './src/types';

// Initialize designer
const designer = new RocketDesigner();

// Configure rocket parameters
const config: RocketConfig = {
  targetAltitude: 1000, // meters
  payloadMass: 0.5, // kg
  diameter: 0.075, // meters (75mm)
  material: MaterialType.CARBON_FIBER,
  noseConeProfile: NoseConeProfile.VON_KARMAN,
  finCount: 4,
  motorClass: 'H'
};

// Generate design
const rocket = designer.create(config);

console.log(`Rocket mass: ${rocket.totalMass}kg`);
console.log(`Stability margin: ${rocket.stabilityMargin}`);
```

### Component Specification

```typescript
import { ComponentGenerator } from './src/components';

const generator = new ComponentGenerator();

// Generate nose cone
const noseCone = generator.createNoseCone({
  profile: NoseConeProfile.VON_KARMAN,
  length: 0.3, // meters
  baseDiameter: 0.075,
  material: MaterialType.CARBON_FIBER,
  wallThickness: 0.002 // 2mm
});

// Generate fins
const fins = generator.createFins({
  count: 4,
  span: 0.1, // meters
  rootChord: 0.12,
  tipChord: 0.06,
  sweepAngle: 30, // degrees
  material: MaterialType.PLYWOOD,
  thickness: 0.003
});

// Generate body tube
const bodyTube = generator.createBodyTube({
  length: 0.8,
  diameter: 0.075,
  wallThickness: 0.002,
  material: MaterialType.CARBON_FIBER
});
```

### Performance Simulation

```typescript
import { Simulator, SimulationParams } from './src/simulation';

const simulator = new Simulator();

const params: SimulationParams = {
  rocket: rocket,
  windSpeed: 5, // m/s
  launchAngle: 85, // degrees from horizontal
  timestep: 0.01, // seconds
  gravity: 9.81
};

// Run flight simulation
const results = simulator.runFlightSimulation(params);

console.log(`Max altitude: ${results.maxAltitude}m`);
console.log(`Max velocity: ${results.maxVelocity}m/s`);
console.log(`Flight time: ${results.flightTime}s`);
console.log(`Landing drift: ${results.landingDrift}m`);

// Stability analysis
const stability = simulator.analyzeStability(rocket);
console.log(`Center of Gravity: ${stability.cg}m from tip`);
console.log(`Center of Pressure: ${stability.cp}m from tip`);
console.log(`Static margin: ${stability.staticMargin} calibers`);
```

### Manufacturing Export

```typescript
import { ManufacturingExporter } from './src/export';
import { ExportFormat } from './src/types';

const exporter = new ManufacturingExporter();

// Export CAD files
await exporter.exportCAD(rocket, {
  format: ExportFormat.STEP,
  outputPath: './output/rocket.step',
  includeAssembly: true,
  separateComponents: true
});

// Export CNC G-code
await exporter.exportGCode(fins, {
  machine: 'CNC_ROUTER',
  outputPath: './output/fins.gcode',
  toolDiameter: 3.175, // mm
  feedRate: 1000,
  spindleSpeed: 12000
});

// Export assembly instructions
await exporter.exportAssemblyGuide(rocket, {
  format: ExportFormat.PDF,
  outputPath: './output/assembly.pdf',
  includePartsList: true,
  includeToolList: true,
  language: 'en'
});

// Export technical drawings
await exporter.exportDrawings(rocket, {
  format: ExportFormat.DXF,
  outputPath: './output/drawings/',
  views: ['front', 'side', 'top'],
  scale: 1,
  dimensions: true
});
```

## Advanced Features

### Custom Motor Definition

```typescript
import { MotorDatabase, CustomMotor } from './src/motors';

const motorDB = new MotorDatabase();

// Define custom motor
const customMotor: CustomMotor = {
  designation: 'H128-10',
  manufacturer: 'Custom',
  impulseClass: 'H',
  totalImpulse: 160, // Newton-seconds
  thrustCurve: [
    { time: 0, thrust: 0 },
    { time: 0.1, thrust: 140 },
    { time: 1.5, thrust: 130 },
    { time: 2.0, thrust: 0 }
  ],
  propellantMass: 0.0625,
  totalMass: 0.125,
  diameter: 0.029,
  length: 0.124
};

motorDB.addCustomMotor(customMotor);
```

### Optimization

```typescript
import { Optimizer, OptimizationGoals } from './src/optimization';

const optimizer = new Optimizer();

// Optimize for maximum altitude
const optimized = optimizer.optimize({
  baseConfig: config,
  goals: {
    targetAltitude: 1500,
    minimizeMass: true,
    maximizeStability: true
  },
  constraints: {
    maxDiameter: 0.1,
    maxLength: 1.5,
    minStaticMargin: 1.5,
    maxStaticMargin: 3.0
  },
  iterations: 100
});

console.log(`Optimized configuration:`);
console.log(JSON.stringify(optimized.config, null, 2));
console.log(`Predicted altitude: ${optimized.predictedAltitude}m`);
```

### Material Database

```typescript
import { MaterialDatabase } from './src/materials';

const materials = new MaterialDatabase();

// Get material properties
const carbonFiber = materials.get(MaterialType.CARBON_FIBER);
console.log(`Density: ${carbonFiber.density}kg/m³`);
console.log(`Young's modulus: ${carbonFiber.youngsModulus}GPa`);
console.log(`Tensile strength: ${carbonFiber.tensileStrength}MPa`);

// Add custom material
materials.addCustom({
  name: 'Custom Composite',
  density: 1600,
  youngsModulus: 70,
  tensileStrength: 600,
  cost: 25.5 // per kg
});
```

## Configuration

### Project Configuration File

Create `rocket-config.json`:

```json
{
  "units": "metric",
  "defaultMaterial": "CARBON_FIBER",
  "simulation": {
    "timestep": 0.01,
    "defaultWindSpeed": 3,
    "gravity": 9.81,
    "airDensity": 1.225
  },
  "manufacturing": {
    "tolerance": 0.1,
    "defaultToolDiameter": 3.175,
    "cnc": {
      "feedRate": 1000,
      "plungeRate": 300,
      "spindleSpeed": 12000
    }
  },
  "safety": {
    "minStaticMargin": 1.0,
    "maxStaticMargin": 3.5,
    "structuralSafetyFactor": 1.5
  },
  "export": {
    "defaultFormat": "STEP",
    "cadPrecision": 0.001,
    "includeMetadata": true
  }
}
```

Load configuration:

```typescript
import { ConfigLoader } from './src/config';

const config = ConfigLoader.load('./rocket-config.json');
designer.setConfig(config);
```

## Common Patterns

### Complete Design to Manufacturing Pipeline

```typescript
import { Pipeline } from './src/pipeline';

async function designAndManufacture() {
  const pipeline = new Pipeline();
  
  // Define mission
  const mission = {
    targetAltitude: 1000,
    payloadMass: 0.5,
    launchSite: { elevation: 100, latitude: 22.5 }
  };
  
  // Run complete pipeline
  const result = await pipeline.run({
    mission,
    config: {
      diameter: 0.075,
      material: MaterialType.CARBON_FIBER,
      finCount: 4,
      motorClass: 'H'
    },
    steps: [
      'design',
      'simulate',
      'optimize',
      'validate',
      'export-cad',
      'export-gcode',
      'export-docs'
    ],
    outputDir: './output'
  });
  
  console.log(`Pipeline completed: ${result.success}`);
  console.log(`Files generated: ${result.files.length}`);
  
  return result;
}
```

### Batch Design Generation

```typescript
async function generateDesignVariants() {
  const baseConfig: RocketConfig = {
    targetAltitude: 1000,
    payloadMass: 0.5,
    diameter: 0.075,
    material: MaterialType.CARBON_FIBER,
    noseConeProfile: NoseConeProfile.VON_KARMAN,
    finCount: 4,
    motorClass: 'H'
  };
  
  const variants = [
    { ...baseConfig, finCount: 3 },
    { ...baseConfig, finCount: 4 },
    { ...baseConfig, finCount: 5 },
    { ...baseConfig, noseConeProfile: NoseConeProfile.OGIVE },
    { ...baseConfig, noseConeProfile: NoseConeProfile.PARABOLIC }
  ];
  
  const results = [];
  
  for (const variant of variants) {
    const rocket = designer.create(variant);
    const simulation = simulator.runFlightSimulation({ rocket });
    
    results.push({
      config: variant,
      mass: rocket.totalMass,
      altitude: simulation.maxAltitude,
      stability: rocket.stabilityMargin
    });
  }
  
  // Sort by altitude
  results.sort((a, b) => b.altitude - a.altitude);
  
  console.log('Best design:', results[0]);
  return results;
}
```

## Troubleshooting

### Stability Issues

```typescript
// Check if rocket is stable
if (rocket.stabilityMargin < 1.0) {
  console.error('Rocket is unstable! Margin:', rocket.stabilityMargin);
  
  // Increase fin size
  rocket.fins.span *= 1.2;
  rocket.fins.rootChord *= 1.1;
  
  // Or move fins back
  rocket.finPosition += 0.05;
  
  // Recalculate
  rocket.recalculateStability();
}
```

### Export Errors

```typescript
try {
  await exporter.exportCAD(rocket, options);
} catch (error) {
  if (error.code === 'INVALID_GEOMETRY') {
    console.error('Invalid geometry detected');
    // Validate and fix
    rocket.validateGeometry();
    rocket.repairIntersections();
  } else if (error.code === 'FILE_WRITE_ERROR') {
    console.error('Cannot write file:', error.path);
    // Check permissions and disk space
  }
}
```

### Simulation Convergence

```typescript
// If simulation doesn't converge, reduce timestep
const results = simulator.runFlightSimulation({
  ...params,
  timestep: 0.001, // Smaller timestep
  maxIterations: 100000
});

if (!results.converged) {
  console.warn('Simulation did not converge');
  console.log('Last valid state:', results.lastValidState);
}
```

### Material Selection

```typescript
// Ensure material is appropriate for component
function validateMaterialSelection(component, material) {
  const props = materials.get(material);
  
  const stress = component.calculateMaxStress();
  const safetyFactor = props.tensileStrength / stress;
  
  if (safetyFactor < 1.5) {
    console.warn(`Low safety factor: ${safetyFactor}`);
    console.log('Consider stronger material');
    return false;
  }
  
  return true;
}
```
