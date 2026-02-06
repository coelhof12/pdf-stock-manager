# PDF Stock Manager

Stock management via embedded JavaScript in a PDF form -- designed for a Windows XP machine that couldn't install new software.

**Status:** Archived
**Live:** Not applicable (offline PDF tool) | **Docs:** [/docs](/docs) | **Demo:** [/docs/assets/demo.gif](/docs/assets/demo.gif)

## Demo

![Demo](/docs/assets/demo.gif)

## Why this exists

A furniture factory had a Windows XP machine on the production floor used to track material quantities. The machine was locked down -- no new software could be installed, no browser updates were possible, and there was no network access. The only application available was Adobe Reader XI.

The factory needed a way to replace error-prone paper-based tracking with something that could auto-calculate stock levels in real time. A PDF with embedded JavaScript was the only vehicle that could deliver interactive functionality on that machine without any IT intervention.

This project is less about the technology and more about engineering under constraints: finding a working solution when every conventional option is off the table.

## Key features

- **3-table layout** -- Received Materials, Used Materials, and Material Totals, matching the factory's actual workflow
- **Automatic calculations** -- totals update instantly when materials are received or used (received minus used equals current stock)
- **Zero installation** -- works as a single PDF file opened in Adobe Reader, nothing to install or configure
- **Adobe Reader XI compatible** -- runs on the Acrobat JavaScript runtime available in legacy PDF readers
- **Portable** -- the entire tool is one file; copy it to a USB drive, open it, and it works

## Tech stack

| Layer | Technology |
|-------|------------|
| Logic | JavaScript (Acrobat PDF embedded) |
| Form design | Adobe Acrobat Pro |
| Runtime | Adobe Reader XI |
| Platform | Windows XP |

## Architecture (quick view)

```
PDF File
 |
 |-- Form Fields (receivedMaterial1..4, usedMaterial1..4, totalMaterial1..4)
 |
 |-- Embedded JavaScript Actions
      |-- On "Received" field change: parse value, add to total, clear input
      |-- On "Used" field change: parse value, subtract from total, clear input
```

The PDF contains 12 named form fields across 3 tables. JavaScript actions are bound to button triggers using the Acrobat `this.getField()` API. When a worker enters a quantity in the Received or Used table and triggers the action, the script reads the input with `parseFloat()`, updates the corresponding total field, and clears the input for the next entry.

There is no server, no database, and no external dependency. The PDF file itself holds the current state. Workers save the file at end of day to persist the totals.

## Getting started

1. Open `Assets/PDF/PDF_example_with_JS.pdf` in Adobe Reader (version XI or later) or Adobe Acrobat
2. Enter a quantity in any Received Material field and click the corresponding action button
3. The Material Totals table updates automatically
4. Enter a quantity in any Used Material field and click the corresponding action button to subtract from the total
5. Save the PDF to persist the current stock values

> **Note:** Not all PDF readers support embedded JavaScript. For full functionality, use Adobe Reader or Adobe Acrobat.

## Known limits

- **Only 4 materials tracked** -- the form was designed for a specific production section, not as a generic inventory tool
- **No persistence beyond PDF save** -- if the file is not saved, changes are lost when it is closed
- **No print-friendly layout** -- the form is optimised for on-screen use, not paper output
- **Adobe Reader-specific** -- embedded JavaScript is not universally supported across PDF readers (e.g., browser-based viewers, Preview on macOS)
- **No undo** -- once a quantity is applied to the total, reverting requires manually entering a corrective value
- **No history or audit trail** -- there is no log of individual transactions, only the running total

## Screenshots

![Received materials table](/docs/assets/screenshot-01.png)

![Material totals auto-calculated](/docs/assets/screenshot-02.png)

## License

MIT - see [LICENSE](LICENSE)
