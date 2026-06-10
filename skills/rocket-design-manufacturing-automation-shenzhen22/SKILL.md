---
name: rocket-design-manufacturing-automation-shenzhen22
description: Automate rocket design, simulation, and manufacturing workflows using TypeScript-based CAD and engineering tools
triggers:
  - design a rocket component
  - automate rocket manufacturing process
  - simulate rocket flight parameters
  - generate rocket CAD models
  - calculate rocket thrust and trajectory
  - optimize rocket design specifications
  - export rocket manufacturing files
  - validate rocket structural integrity
---

# Rocket Design and Manufacturing Automation

> Skill by [ara.so](https://ara.so) — Design Skills collection.

This project provides a TypeScript-based automation framework for rocket design, CAD modeling, flight simulation, and manufacturing file generation. Developed by students at Shenzhen 22nd High School, it streamlines the complete rocket engineering pipeline from concept to production-ready files.

## Installation

```bash
# Clone the repository
git clone https://github.com/Kevin100202/Rocket-Design-and-Manufacturing-Automation-Program-from-Shenzhen22highschool.git
cd Rocket-Design-and-Manufacturing-Automation-Program-from-Shenzhen22highschool

# Install dependencies
npm install

# Build the project
npm run build

# Run the automation program
npm start
```

## Core Modules

### 1. Rocket Design Module

Create and configure rocket specifications programmatically:

```typescript
import { RocketDesigner, RocketConfig } from './src/design';

const config: RocketConfig = {
  name: "TestRocket-Alpha",
  stages: 2,
  totalHeight: 2500, // mm
  diameter: 150, // mm
  targetApogee: 3000, // meters
  payloadMass: 500 // grams
};

const designer = new RocketDesigner(config);
const design = designer.generate();

console.log(`Generated design: ${design.name}`);
console.log(`Estimated mass: ${design.totalMass}g`);
```

### 2. Component Library

Access pre-defined rocket components:

```typescript
import { ComponentLibrary, NoseCone, BodyTube, Fins } from './src/components';

const library = new ComponentLibrary();

// Create nose cone
const noseCone = library.createNoseCone({
  type: 'ogive',
  length: 300,
  baseDiameter: 150,
  material: 'PLA'
});

// Create body tube
const bodyTube = library.createBodyTube({
  length: 1200,
  outerDiameter: 150,
  wallThickness: 3,
  material: 'fiberglass'
});

// Create fin set
const fins = library.createFins({
  count: 3,
  rootChord: 200,
  tipChord: 80,
  span: 150,
  sweepAngle: 30,
  material: 'plywood'
});
```

### 3. Flight Simulation

Simulate rocket performance before manufacturing:

```typescript
import { FlightSimulator, SimulationParams } from './src/simulation';

const simParams: SimulationParams = {
  rocketMass: 2500, // grams
  dragCoefficient: 0.45,
  motorTotalImpulse: 160, // Ns
  motorBurnTime: 2.8, // seconds
  launchAngle: 90, // degrees
  windSpeed: 5, // m/s
  airDensity: 1.225 // kg/m³
};

const simulator = new FlightSimulator(simParams);
const results = simulator.run();

console.log(`Max altitude: ${results.maxAltitude.toFixed(2)}m`);
console.log(`Max velocity: ${results.maxVelocity.toFixed(2)}m/s`);
console.log(`Flight duration: ${results.flightTime.toFixed(2)}s`);
console.log(`Landing distance: ${results.landingDistance.toFixed(2)}m`);
```

### 4. CAD Model Generation

Generate 3D CAD models in various formats:

```typescript
import { CADGenerator, ExportFormat } from './src/cad';

const generator = new CADGenerator(design);

// Generate STL for 3D printing
await generator.exportComponent('noseCone', ExportFormat.STL, './output/nose_cone.stl');

// Generate STEP for CNC machining
await generator.exportComponent('fins', ExportFormat.STEP, './output/fins.step');

// Generate full assembly
await generator.exportAssembly(ExportFormat.IGES, './output/full_assembly.iges');

console.log('CAD files generated successfully');
```

### 5. Manufacturing Automation

Generate CNC G-code and laser cutting paths:

```typescript
import { ManufacturingProcessor, MachineType } from './src/manufacturing';

const processor = new ManufacturingProcessor();

// Generate CNC G-code for fins
const gcode = processor.generateGCode({
  component: fins,
  machine: MachineType.CNC_3AXIS,
  material: 'plywood',
  toolDiameter: 6, // mm
  feedRate: 1200, // mm/min
  spindleSpeed: 18000 // RPM
});

await processor.saveGCode(gcode, './output/fins.nc');

// Generate laser cutting path
const laserPath = processor.generateLaserPath({
  component: bodyTube,
  material: 'cardboard',
  thickness: 2, // mm
  power: 80, // percent
  speed: 20 // mm/s
});

await processor.saveLaserPath(laserPath, './output/body_tube.svg');
```

## Complete Workflow Example

```typescript
import { 
  RocketDesigner, 
  FlightSimulator, 
  CADGenerator, 
  ManufacturingProcessor,
  OptimizationEngine 
} from './src';

async function automateRocketProduction() {
  // Step 1: Define requirements
  const requirements = {
    targetAltitude: 1500,
    maxDiameter: 100,
    budgetConstraint: 200, // USD
    safetyFactor: 1.5
  };

  // Step 2: Design optimization
  const optimizer = new OptimizationEngine(requirements);
  const optimizedDesign = await optimizer.optimize();

  console.log(`Optimized design mass: ${optimizedDesign.totalMass}g`);

  // Step 3: Validate with simulation
  const simulator = new FlightSimulator(optimizedDesign.getSimParams());
  const simResults = simulator.run();

  if (simResults.maxAltitude < requirements.targetAltitude * 0.9) {
    console.warn('Design does not meet altitude requirements');
    return;
  }

  // Step 4: Generate CAD models
  const cadGen = new CADGenerator(optimizedDesign);
  await cadGen.exportAllComponents('./output/cad');

  // Step 5: Generate manufacturing files
  const mfgProcessor = new ManufacturingProcessor();
  
  for (const component of optimizedDesign.components) {
    if (component.requiresCNC) {
      const gcode = mfgProcessor.generateGCode(component);
      await mfgProcessor.saveGCode(gcode, `./output/cnc/${component.name}.nc`);
    }
    
    if (component.requiresLaserCut) {
      const laserPath = mfgProcessor.generateLaserPath(component);
      await mfgProcessor.saveLaserPath(laserPath, `./output/laser/${component.name}.svg`);
    }
  }

  // Step 6: Generate assembly instructions
  const instructions = optimizedDesign.generateAssemblyInstructions();
  await instructions.exportPDF('./output/assembly_manual.pdf');

  console.log('✅ Complete rocket production package generated');
}

automateRocketProduction().catch(console.error);
```

## Configuration

Create a `rocket.config.json` file in your project root:

```json
{
  "defaults": {
    "units": "metric",
    "material": "PLA",
    "safetyFactor": 1.5,
    "windCorrection": true
  },
  "simulation": {
    "timeStep": 0.01,
    "maxIterations": 10000,
    "convergenceTolerance": 0.001
  },
  "manufacturing": {
    "outputDirectory": "./output",
    "cncTolerance": 0.1,
    "laserKerf": 0.2,
    "includeToolpaths": true
  },
  "export": {
    "cadFormats": ["STL", "STEP"],
    "includeDrawings": true,
    "generateBOM": true
  }
}
```

Load configuration:

```typescript
import { loadConfig } from './src/config';

const config = loadConfig('./rocket.config.json');
const designer = new RocketDesigner(config.defaults);
```

## Stability Analysis

Calculate rocket stability margin (caliber):

```typescript
import { StabilityAnalyzer } from './src/analysis';

const analyzer = new StabilityAnalyzer(design);
const stability = analyzer.calculateStability();

console.log(`Center of Gravity: ${stability.cg.toFixed(2)}mm from nose`);
console.log(`Center of Pressure: ${stability.cp.toFixed(2)}mm from nose`);
console.log(`Stability margin: ${stability.calibers.toFixed(2)} calibers`);

if (stability.calibers < 1.0) {
  console.error('⚠️  Unstable design! Increase fin size or move CG forward');
} else if (stability.calibers > 2.5) {
  console.warn('⚠️  Overstable design! May weathercock significantly');
} else {
  console.log('✅ Stable design');
}
```

## Material Properties

Define custom materials:

```typescript
import { MaterialLibrary, Material } from './src/materials';

const library = new MaterialLibrary();

library.addMaterial({
  name: 'carbon-fiber',
  density: 1.6, // g/cm³
  tensileStrength: 3500, // MPa
  youngsModulus: 230, // GPa
  cost: 85, // USD/kg
  printable: false,
  machinable: true
});

const material = library.getMaterial('carbon-fiber');
```

## Troubleshooting

### Simulation Divergence

```typescript
// Reduce time step if simulation fails to converge
const simParams: SimulationParams = {
  // ... other params
  timeStep: 0.001, // smaller time step
  maxIterations: 50000 // more iterations allowed
};
```

### CAD Export Failures

```typescript
// Check component validity before export
if (!design.validate()) {
  const errors = design.getValidationErrors();
  console.error('Design validation errors:', errors);
  // Fix errors before proceeding
}
```

### Manufacturing Tolerance Issues

```typescript
// Adjust tolerance for tighter fits
const gcode = processor.generateGCode({
  component: fins,
  // ... other params
  tolerance: 0.05, // tighter tolerance (mm)
  compensateKerf: true
});
```

### Memory Issues with Large Assemblies

```typescript
// Process components individually
for (const component of design.components) {
  const cadGen = new CADGenerator(component);
  await cadGen.export(ExportFormat.STL, `./output/${component.name}.stl`);
  cadGen.dispose(); // Free memory
}
```

## Testing

Run unit tests:

```bash
npm test
```

Run specific test suite:

```bash
npm test -- --grep "FlightSimulator"
```

Generate test coverage:

```bash
npm run test:coverage
```

## Common Patterns

### Iterative Design Optimization

```typescript
let bestDesign = null;
let bestScore = 0;

for (let iteration = 0; iteration < 100; iteration++) {
  const design = optimizer.generateCandidate();
  const simResult = simulator.run(design);
  const score = optimizer.scoreDesign(design, simResult);
  
  if (score > bestScore) {
    bestScore = score;
    bestDesign = design;
  }
}

console.log(`Best design found with score: ${bestScore}`);
```

### Batch Processing Multiple Designs

```typescript
const designs = ['alpha', 'beta', 'gamma'].map(name => 
  new RocketDesigner({ name, /* ... */ }).generate()
);

const results = await Promise.all(
  designs.map(async design => ({
    design,
    simulation: new FlightSimulator(design.getSimParams()).run(),
    exported: await new CADGenerator(design).exportAll('./output/' + design.name)
  }))
);

results.forEach(r => {
  console.log(`${r.design.name}: ${r.simulation.maxAltitude}m altitude`);
});
```

This automation framework enables rapid iteration from concept to manufacturable rocket designs with integrated simulation and validation.
