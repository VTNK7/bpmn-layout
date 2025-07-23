# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is `bpmn-auto-layout`, a library that automatically creates and layouts BPMN (Business Process Model and Notation) diagrams by generating missing DI (Diagram Interchange) information. It takes BPMN XML files and adds proper positioning and layout information using a grid-based layout system.

## Development Commands

```bash
# Run all checks (lint + test) - use this before committing
npm run all

# Build the library
npm run build

# Run tests
npm test

# Update test snapshots when layout changes are intentional
npm run test:update-snapshots

# Visually inspect test results (opens browser with generated diagrams)
npm run test:inspect

# Lint code
npm run lint

# Start example/demo
npm start
```

## Architecture Overview

The codebase uses a **handler pattern** with a **grid-based layout system**:

- **Grid System** (`Grid.js`): Organizes BPMN elements in a 2D grid structure for positioning
- **Layouter** (`Layouter.js`): Core class that orchestrates the layout process using specialized handlers
- **Handler Pattern** (`/handler/` directory): Specialized processors for different layout aspects:
  - `elementHandler` - Core element processing and positioning
  - `incomingHandler` - Manages incoming sequence flows
  - `outgoingHandler` - Manages outgoing sequence flows  
  - `attachersHandler` - Handles boundary events and attached elements
- **DI Factory** (`/di/` directory): Generates BPMN Diagram Interchange XML information

Main entry point is `layoutProcess()` in `index.js` which processes BPMN XML and returns XML with added layout information.

## Testing Approach

The project uses **snapshot-based testing** with visual verification:

- Each `.bpmn` file in `/test/fixtures/` automatically becomes a test case
- Tests compare layout output against stored snapshots in `/test/snapshots/`
- Visual output is generated in `/test/output/` for manual inspection
- When making intentional layout changes, update snapshots with `npm run test:update-snapshots`
- Use `npm run test:inspect` to visually verify layout changes in a browser

## Key Constraints

- Only the first participant's process in collaborations is laid out
- Sub-processes are laid out as collapsed
- Groups, text annotations, associations, and message flows are not currently laid out
- Grid-based positioning means elements snap to grid coordinates

## Dependencies

- `bpmn-moddle`: BPMN XML parsing and serialization
- `min-dash`: Utility functions (similar to lodash but lighter)
- Built for both Node.js (>= 18) and browser environments