# BeaverBuiltCanada Configurator

Web-based building configurator for sheds, workshops, garages, animal housing, and custom structures.

The configurator exports a JSON configuration file which is imported into Fusion 360 to drive a parametric MasterSchema.

## Architecture

Customer
↓
Vue Configurator
↓
config.json
↓
Fusion 360 Python Script
(Load-The-Mechanism)
↓
Fusion Parameters
↓
MasterSchema
↓
Generated Building

## Current Status

### Core Pipeline

- [x] Vue 3 + Vite project created
- [x] JSON export working
- [x] Fusion import working
- [x] Parameter mapping working
- [x] MasterSchema regeneration working

### Building Dimensions

- [x] Width
- [x] Depth
- [x] Wall Height

### Foundation

- [x] Skid Type
- [x] Center Skid

### Floor

- [x] Floor Joist Size
- [x] Floor Plywood Thickness

### Walls

- [x] Wall Stud Size

### Roof

- [x] Roof Peak Direction
- [x] Roof Rafter Size
- [x] Roof Pitch
- [x] Eaves Overhang
- [ ] Gable Overhang

### Future Features

- [ ] Door Configuration
- [ ] Window Configuration
- [ ] Material Lists
- [ ] Cut Lists
- [ ] PDF Plan Generation
- [ ] Payment Processing
- [ ] Mobile Optimization

## Development

```bash
npm install
npm run dev
```

## Notes

Designs are generated using North American building conventions, including imperial measurements and standard dimensional lumber sizing.
