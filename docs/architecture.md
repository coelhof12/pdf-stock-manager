# Architecture

## Overview

A single PDF file serves as both the interface and the data store. The PDF contains form fields organized into 3 tables (Received Materials, Used Materials, Material Totals) with embedded JavaScript actions that run calculations when triggered by user interaction. There is no server, no network, and no external dependencies -- the entire application lives inside the PDF.

## Structure

```
PDF Document
├── Received Materials Table
│   ├── receivedMaterial1 (input field)
│   ├── receivedMaterial2 (input field)
│   ├── receivedMaterial3 (input field)
│   └── receivedMaterial4 (input field)
│
├── Used Materials Table
│   ├── usedMaterial1 (input field)
│   ├── usedMaterial2 (input field)
│   ├── usedMaterial3 (input field)
│   └── usedMaterial4 (input field)
│
├── Material Totals Table
│   ├── totalMaterial1 (calculated field)
│   ├── totalMaterial2 (calculated field)
│   ├── totalMaterial3 (calculated field)
│   └── totalMaterial4 (calculated field)
│
└── JavaScript Actions
    ├── Add Received (per material): total += received; clear input
    └── Subtract Used (per material): total -= used; clear input
```

## Data flow

1. Worker enters a quantity in a `receivedMaterialN` or `usedMaterialN` field
2. Worker triggers the corresponding action button
3. Embedded JavaScript reads the input field value via `this.getField()`
4. JavaScript reads the current total via `this.getField("totalMaterialN")`
5. For received: adds the input to the total. For used: subtracts the input from the total
6. JavaScript clears the input field after processing
7. The updated total is visible immediately in the Material Totals table
8. Worker saves the PDF to persist the current state

## Runtime

- **Execution environment:** Adobe Reader XI JavaScript engine (Acrobat JavaScript API)
- **Key API:** `this.getField(name).value` for reading and writing form field values
- **Numeric handling:** `parseFloat()` with `|| 0` fallback for empty/invalid fields
- **Persistence:** PDF file save (the form field values are stored in the PDF itself)
