---
name: rocket-design-manufacturing-automation-shenzhen22
description: Automated rocket design and manufacturing program developed by Shenzhen22 high school students using TypeScript
triggers:
  - design a rocket using the automation program
  - calculate rocket trajectory and performance
  - generate rocket manufacturing specifications
  - optimize rocket design parameters
  - simulate rocket flight dynamics
  - automate rocket component design
  - use the shenzhen rocket design tool
  - create rocket blueprints automatically
---

# Rocket Design and Manufacturing Automation Program

> Skill by [ara.so](https://ara.so) — Design Skills collection.

This is a TypeScript-based automation program for rocket design and manufacturing developed by students at Shenzhen22 High School. The project provides tools for automated rocket design calculations, performance simulations, and manufacturing specification generation.

## Installation

```bash
# Clone the repository
git clone https://github.com/Kevin100202/Rocket-Design-and-Manufacturing-Automation-Program-from-Shenzhen22highschool.git
cd Rocket-Design-and-Manufacturing-Automation-Program-from-Shenzhen22highschool

# Install dependencies
npm install

# Build the TypeScript project
npm run build

# Run the program
npm start
```

## Core Features

### Rocket Design Automation
- Automated structural calculations
- Aerodynamic analysis
- Propulsion system design
- Stability and control optimization
- Weight and balance calculations

### Manufacturing Automation
- Component specification generation
- Material selection optimization
- Manufacturing process planning
- Quality control parameters

## Basic Usage

### Importing the Library

```typescript
import { RocketDesigner, FlightSimulator, ManufacturingSpec } from './rocket-automation';

// Initialize a new rocket design
const designer = new RocketDesigner({
  targetAltitude: 1000, // meters
  payloadMass: 0.5, // kg
  motorType: 'solid'
});
```

### Creating a Rocket Design

```typescript
interface RocketConfig {
  name: string;
  targetAltitude: number;
  payloadMass: number;
  motorType: 'solid' | 'liquid' | 'hybrid';
  diameter?: number;
  materials?: string[];
}

const rocketConfig: RocketConfig = {
  name: 'Student Rocket Alpha',
  targetAltitude: 1500,
  payloadMass: 0.8,
  motorType: 'solid',
  diameter: 0.075, // meters
  materials: ['fiberglass', 'carbon-fiber']
};

const design = designer.createDesign(rocketConfig);
console.log('Design completed:', design);
```

### Running Flight Simulations

```typescript
const simulator = new FlightSimulator(design);

// Configure simulation parameters
const simParams = {
  windSpeed: 5, // m/s
  windDirection: 270, // degrees
  temperature: 25, // celsius
  pressure: 101325, // pascals
  launchAngle: 90 // degrees (vertical)
};

// Run the simulation
const results = await simulator.run(simParams);

console.log('Maximum altitude:', results.maxAltitude);
console.log('Maximum velocity:', results.maxVelocity);
console.log('Flight duration:', results.flightTime);
console.log('Landing coordinates:', results.landingPosition);
```

### Generating Manufacturing Specifications

```typescript
const manufacturing = new ManufacturingSpec(design);

// Generate component specifications
const bodyTube = manufacturing.generateBodyTube({
  length: design.length,
  diameter: design.diameter,
  thickness: 0.003, // meters
  material: 'fiberglass'
});

const noseCone = manufacturing.generateNoseCone({
  type: 'ogive',
  length: design.diameter * 2,
  material: 'abs-plastic'
});

const fins = manufacturing.generateFins({
  count: 4,
  span: 0.12, // meters
  rootChord: 0.15, // meters
  tipChord: 0.05, // meters
  material: 'plywood'
});

// Export manufacturing files
manufacturing.exportToCAD('rocket-design.step');
manufacturing.exportToJSON('manufacturing-spec.json');
manufacturing.generateBOM('bill-of-materials.csv');
```

## Aerodynamic Analysis

```typescript
import { AerodynamicAnalyzer } from './rocket-automation/aerodynamics';

const analyzer = new AerodynamicAnalyzer(design);

// Calculate center of pressure
const cp = analyzer.calculateCenterOfPressure();

// Calculate center of gravity
const cg = analyzer.calculateCenterOfGravity({
  motorMass: 0.125,
  propellantMass: 0.062,
  payloadMass: 0.5
});

// Check stability margin (CP should be behind CG)
const stabilityMargin = analyzer.getStabilityMargin(cp, cg);
console.log('Stability margin:', stabilityMargin, 'calibers');

if (stabilityMargin < 1.0) {
  console.warn('Warning: Rocket may be unstable. Add fin area or move CG forward.');
}

// Calculate drag coefficient
const dragCoefficient = analyzer.calculateDragCoefficient({
  velocity: 100, // m/s
  altitude: 500 // m
});
```

## Propulsion System Design

```typescript
import { PropulsionDesigner } from './rocket-automation/propulsion';

const propulsion = new PropulsionDesigner({
  type: 'solid',
  totalImpulse: 160, // Newton-seconds (G-class motor)
  burnTime: 2.5, // seconds
  propellantType: 'APCP'
});

// Calculate thrust curve
const thrustCurve = propulsion.generateThrustCurve();

// Optimize nozzle design
const nozzle = propulsion.optimizeNozzle({
  throatDiameter: 0.012, // meters
  exitDiameter: 0.024, // meters
  expansionRatio: 4
});

// Calculate specific impulse
const isp = propulsion.calculateSpecificImpulse();
console.log('Specific Impulse:', isp, 'seconds');
```

## Structural Analysis

```typescript
import { StructuralAnalyzer } from './rocket-automation/structural';

const structural = new StructuralAnalyzer(design);

// Calculate maximum aerodynamic load
const maxQ = structural.calculateMaxDynamicPressure(results);

// Analyze body tube stress
const tubeStress = structural.analyzeBodyTubeStress({
  maxQ: maxQ,
  acceleration: results.maxAcceleration,
  material: 'fiberglass',
  thickness: 0.003
});

if (tubeStress.safetyFactor < 2.0) {
  console.warn('Increase body tube thickness for safety');
}

// Analyze fin flutter
const flutterSpeed = structural.calculateFinFlutter({
  finThickness: 0.006,
  finSpan: 0.12,
  material: 'plywood'
});

console.log('Flutter speed:', flutterSpeed, 'm/s');
```

## Recovery System Design

```typescript
import { RecoverySystem } from './rocket-automation/recovery';

const recovery = new RecoverySystem(design);

// Design parachute
const parachute = recovery.designParachute({
  descentRate: 5, // m/s
  rocketMass: design.totalMass,
  altitude: results.maxAltitude
});

console.log('Parachute diameter:', parachute.diameter, 'meters');
console.log('Shroud line length:', parachute.shroudLength, 'meters');

// Calculate ejection charge
const ejectionCharge = recovery.calculateEjectionCharge({
  bodyTubeDiameter: design.diameter,
  bodyTubeLength: 0.3, // meters (recovery bay)
  pressure: 15 // psi
});

console.log('Black powder mass:', ejectionCharge.mass, 'grams');
```

## Configuration Files

### rocket-config.json

```json
{
  "project": {
    "name": "Student Rocket Project",
    "version": "1.0.0",
    "safetyLevel": "high-power"
  },
  "defaults": {
    "units": "metric",
    "material": "fiberglass",
    "finCount": 4,
    "stabilityMargin": 1.5
  },
  "simulation": {
    "timeStep": 0.01,
    "atmosphericModel": "standard",
    "windModel": "constant"
  },
  "manufacturing": {
    "tolerance": 0.0005,
    "outputFormats": ["step", "stl", "dxf"],
    "bomFormat": "csv"
  }
}
```

### Environment Variables

```bash
# Optional: API keys for advanced features
export WEATHER_API_KEY=your_weather_api_key
export CAD_EXPORT_PATH=/path/to/export
export SIMULATION_THREADS=4
```

## Common Patterns

### Complete Design Workflow

```typescript
async function designRocket(requirements: RocketConfig) {
  // 1. Initialize designer
  const designer = new RocketDesigner(requirements);
  
  // 2. Generate initial design
  const design = designer.createDesign(requirements);
  
  // 3. Optimize design
  const optimizer = new DesignOptimizer(design);
  const optimizedDesign = await optimizer.optimize({
    objectives: ['maxAltitude', 'stability', 'costEfficiency'],
    constraints: {
      maxDiameter: 0.1,
      maxLength: 1.5,
      maxMass: 2.0
    }
  });
  
  // 4. Validate design
  const validator = new DesignValidator(optimizedDesign);
  const validation = validator.validate();
  
  if (!validation.isValid) {
    throw new Error(`Design validation failed: ${validation.errors.join(', ')}`);
  }
  
  // 5. Run simulations
  const simulator = new FlightSimulator(optimizedDesign);
  const simResults = await simulator.run({
    windSpeed: 5,
    temperature: 25,
    pressure: 101325,
    launchAngle: 90
  });
  
  // 6. Generate manufacturing specs
  const manufacturing = new ManufacturingSpec(optimizedDesign);
  await manufacturing.exportAll({
    cadFormat: 'step',
    bomFormat: 'csv',
    drawingsFormat: 'pdf'
  });
  
  return {
    design: optimizedDesign,
    simulation: simResults,
    manufacturing: manufacturing
  };
}
```

## Troubleshooting

### Design Instability
```typescript
// If stability margin is too low
if (stabilityMargin < 1.0) {
  // Option 1: Increase fin area
  design.fins.span *= 1.2;
  
  // Option 2: Move CG forward
  design.noseCone.ballast += 0.05; // kg
  
  // Option 3: Lengthen body tube
  design.length *= 1.1;
}
```

### Excessive Weight
```typescript
// Optimize for weight reduction
const weightOptimizer = designer.optimizeWeight({
  minSafetyFactor: 2.0,
  materialAlternatives: ['carbon-fiber', 'fiberglass'],
  hollowComponents: true
});
```

### Simulation Convergence Issues
```typescript
// Reduce time step for better accuracy
simulator.setTimeStep(0.005); // smaller time step

// Use adaptive integration
simulator.setIntegrationMethod('runge-kutta-45');
```

### Manufacturing Export Errors
```typescript
// Ensure CAD export path exists
import { existsSync, mkdirSync } from 'fs';

const exportPath = process.env.CAD_EXPORT_PATH || './exports';
if (!existsSync(exportPath)) {
  mkdirSync(exportPath, { recursive: true });
}

manufacturing.setExportPath(exportPath);
```

## Safety Considerations

Always validate designs for safety compliance:

```typescript
const safetyChecker = new SafetyValidator(design);

const safetyReport = safetyChecker.validateAll({
  checkStructuralIntegrity: true,
  checkStability: true,
  checkRecovery: true,
  checkPropulsion: true
});

if (!safetyReport.approved) {
  console.error('Safety violations:', safetyReport.violations);
  throw new Error('Design does not meet safety requirements');
}
```
