---
name: rocket-design-manufacturing-automation-shenzhen22
description: Automated rocket design and manufacturing program built by students for aerospace engineering workflows
triggers:
  - design a rocket using the automation program
  - help me with rocket manufacturing automation
  - use the Shenzhen22 rocket design tool
  - automate rocket component design
  - generate rocket specifications automatically
  - work with the student rocket design system
---

# Rocket Design and Manufacturing Automation Program

> Skill by [ara.so](https://ara.so) — Design Skills collection.

This is a TypeScript-based automation program developed by students from Shenzhen 22nd High School for designing and manufacturing rockets. The program automates rocket design calculations, component specifications, and manufacturing workflows for aerospace engineering education and prototyping.

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

The project follows a TypeScript architecture with modular components:

```
src/
├── design/          # Rocket design modules
├── manufacturing/   # Manufacturing automation
├── calculations/    # Physics and engineering calculations
├── models/          # Data models and interfaces
└── utils/           # Utility functions
```

## Core Components

### Rocket Design Module

Design rockets with automated calculations for aerodynamics, propulsion, and structural integrity:

```typescript
import { RocketDesigner } from './design/RocketDesigner';
import { RocketSpec } from './models/RocketSpec';

// Create a new rocket design
const designer = new RocketDesigner();

const rocketSpec: RocketSpec = {
  name: "Student Rocket Alpha",
  targetAltitude: 1000, // meters
  payloadMass: 0.5, // kg
  diameter: 0.1, // meters
  length: 1.2 // meters
};

// Generate complete design
const design = designer.createDesign(rocketSpec);

console.log(`Rocket: ${design.name}`);
console.log(`Estimated apogee: ${design.calculatedApogee}m`);
console.log(`Total mass: ${design.totalMass}kg`);
```

### Propulsion System Calculator

Calculate motor requirements and performance:

```typescript
import { PropulsionCalculator } from './calculations/PropulsionCalculator';

const propulsion = new PropulsionCalculator();

const motorSpec = propulsion.calculateMotorRequirements({
  totalMass: 2.5, // kg
  targetVelocity: 150, // m/s
  burnTime: 3.5, // seconds
  efficiency: 0.85
});

console.log(`Thrust required: ${motorSpec.thrust}N`);
console.log(`Impulse: ${motorSpec.totalImpulse}Ns`);
console.log(`Propellant mass: ${motorSpec.propellantMass}kg`);
```

### Aerodynamics Analysis

Perform aerodynamic calculations for stability and performance:

```typescript
import { AerodynamicsAnalyzer } from './calculations/AerodynamicsAnalyzer';

const aero = new AerodynamicsAnalyzer();

const analysis = aero.analyze({
  diameter: 0.1,
  length: 1.2,
  finCount: 4,
  finSpan: 0.08,
  finChord: 0.15,
  noseConeType: 'ogive'
});

console.log(`Center of pressure: ${analysis.centerOfPressure}m`);
console.log(`Drag coefficient: ${analysis.dragCoefficient}`);
console.log(`Stability margin: ${analysis.stabilityMargin} calibers`);
```

### Manufacturing Automation

Generate manufacturing specifications and CNC programs:

```typescript
import { ManufacturingPlanner } from './manufacturing/ManufacturingPlanner';
import { MaterialType } from './models/Materials';

const planner = new ManufacturingPlanner();

// Generate body tube manufacturing plan
const bodyTubePlan = planner.generatePlan({
  component: 'body-tube',
  material: MaterialType.FIBERGLASS,
  diameter: 0.1,
  length: 0.8,
  wallThickness: 0.003,
  quantity: 1
});

// Export CNC program
const cncCode = bodyTubePlan.exportCNC();
console.log(cncCode);

// Export cutting template
const template = bodyTubePlan.exportTemplate('svg');
```

### Fins Design and Layout

Automated fin design with stability optimization:

```typescript
import { FinDesigner } from './design/FinDesigner';

const finDesigner = new FinDesigner();

const fins = finDesigner.design({
  rocketDiameter: 0.1,
  centerOfGravity: 0.6, // from nose
  finCount: 4,
  material: MaterialType.BALSA_WOOD,
  targetStabilityMargin: 2.0 // calibers
});

console.log(`Root chord: ${fins.rootChord}m`);
console.log(`Tip chord: ${fins.tipChord}m`);
console.log(`Span: ${fins.span}m`);
console.log(`Sweep angle: ${fins.sweepAngle}°`);

// Export fin template for cutting
const finTemplate = fins.exportTemplate('dxf');
```

## Configuration

Create a `config.json` file in the project root:

```json
{
  "units": {
    "system": "metric",
    "length": "meters",
    "mass": "kilograms",
    "force": "newtons"
  },
  "safety": {
    "maxAltitude": 3000,
    "minStabilityMargin": 1.5,
    "maxVelocity": 300
  },
  "manufacturing": {
    "defaultMaterial": "fiberglass",
    "tolerances": {
      "diameter": 0.001,
      "length": 0.005
    },
    "cncFormat": "gcode"
  },
  "simulation": {
    "timeStep": 0.01,
    "atmosphericModel": "standard",
    "windSpeed": 5
  }
}
```

Load configuration:

```typescript
import { ConfigLoader } from './utils/ConfigLoader';

const config = ConfigLoader.load('./config.json');
const designer = new RocketDesigner(config);
```

## Flight Simulation

Simulate rocket flight trajectory:

```typescript
import { FlightSimulator } from './calculations/FlightSimulator';

const simulator = new FlightSimulator();

const flightData = simulator.simulate({
  rocket: design,
  launchAngle: 90, // degrees from horizontal
  windSpeed: 5, // m/s
  temperature: 20, // celsius
  pressure: 101325 // Pa
});

// Analyze results
console.log(`Apogee: ${flightData.apogee}m`);
console.log(`Max velocity: ${flightData.maxVelocity}m/s`);
console.log(`Max acceleration: ${flightData.maxAcceleration}m/s²`);
console.log(`Flight time: ${flightData.totalTime}s`);

// Export trajectory data
flightData.exportCSV('./trajectory.csv');
```

## Complete Workflow Example

```typescript
import { RocketDesigner } from './design/RocketDesigner';
import { FlightSimulator } from './calculations/FlightSimulator';
import { ManufacturingPlanner } from './manufacturing/ManufacturingPlanner';
import { ReportGenerator } from './utils/ReportGenerator';

async function designRocket() {
  // 1. Design the rocket
  const designer = new RocketDesigner();
  
  const design = designer.createDesign({
    name: "Educational Rocket v1",
    targetAltitude: 500,
    payloadMass: 0.3,
    diameter: 0.08,
    length: 1.0
  });
  
  // 2. Validate stability
  if (!design.isStable()) {
    console.error('Design is not stable!');
    return;
  }
  
  // 3. Simulate flight
  const simulator = new FlightSimulator();
  const flight = simulator.simulate({
    rocket: design,
    launchAngle: 90
  });
  
  // 4. Generate manufacturing plans
  const planner = new ManufacturingPlanner();
  const plans = planner.generateFullPlan(design);
  
  // 5. Export all documents
  const reporter = new ReportGenerator();
  await reporter.generate({
    design: design,
    simulation: flight,
    manufacturing: plans,
    outputFormat: 'pdf',
    outputPath: './rocket-documentation.pdf'
  });
  
  console.log('Rocket design complete!');
  console.log(`Documentation saved to rocket-documentation.pdf`);
}

designRocket();
```

## Common Patterns

### Material Selection

```typescript
import { MaterialDatabase } from './models/Materials';

const materials = MaterialDatabase.getInstance();

// Get material properties
const fiberglass = materials.get('fiberglass');
console.log(`Density: ${fiberglass.density} kg/m³`);
console.log(`Tensile strength: ${fiberglass.tensileStrength} MPa`);

// Compare materials
const comparison = materials.compare(['fiberglass', 'carbon-fiber', 'cardboard']);
```

### Stress Analysis

```typescript
import { StressAnalyzer } from './calculations/StressAnalyzer';

const stress = new StressAnalyzer();

const results = stress.analyze({
  component: design.bodyTube,
  maxThrust: motorSpec.thrust,
  maxAcceleration: 15, // g
  safetyFactor: 1.5
});

if (!results.passes) {
  console.error(`Component fails at ${results.failurePoint}`);
}
```

## Troubleshooting

### Design Not Stable

If stability warnings appear:

```typescript
// Check center of gravity vs center of pressure
console.log(`CG: ${design.centerOfGravity}m`);
console.log(`CP: ${design.centerOfPressure}m`);
console.log(`Stability: ${design.stabilityMargin} calibers`);

// Adjust fins to increase stability
const newFins = finDesigner.design({
  ...fins,
  targetStabilityMargin: 2.5
});
```

### Performance Issues

For large simulations:

```typescript
// Reduce time step for faster computation
const quickSim = simulator.simulate({
  rocket: design,
  timeStep: 0.1, // larger time step
  maxTime: 60
});
```

### Export Errors

Ensure output directories exist:

```typescript
import * as fs from 'fs';

const outputDir = './output';
if (!fs.existsSync(outputDir)) {
  fs.mkdirSync(outputDir, { recursive: true });
}
```

## Environment Variables

Set these environment variables as needed:

```bash
# Output configuration
export ROCKET_OUTPUT_DIR=./output
export ROCKET_TEMP_DIR=/tmp/rocket-design

# Simulation settings
export ROCKET_SIM_PRECISION=high
export ROCKET_MAX_ITERATIONS=10000

# Manufacturing
export ROCKET_CNC_FORMAT=gcode
export ROCKET_UNITS=metric
```

## API Reference

Key classes and their methods:

- `RocketDesigner.createDesign(spec)` - Generate rocket design
- `FlightSimulator.simulate(params)` - Run flight simulation
- `ManufacturingPlanner.generatePlan(component)` - Create manufacturing plan
- `AerodynamicsAnalyzer.analyze(geometry)` - Calculate aerodynamic properties
- `PropulsionCalculator.calculateMotorRequirements(params)` - Size motor
- `StressAnalyzer.analyze(component)` - Structural analysis
- `ReportGenerator.generate(data)` - Export documentation
