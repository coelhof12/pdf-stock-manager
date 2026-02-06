# Decisions

Key technical and design decisions made during the development of PDF Stock Manager, and the reasoning behind each.

---

## Why a PDF instead of a web application

The target machine was a Windows XP workstation on a furniture factory production floor. IT policy prevented the installation of any new software, including modern web browsers. Adobe Reader XI was already installed and was the only application capable of running interactive content.

A PDF form with embedded JavaScript was the only delivery vehicle that could provide dynamic, calculated functionality without requiring IT involvement, network access, or any installation step.

## Why embedded JavaScript

Adobe Reader XI supports Acrobat JavaScript within form fields and document actions. This was the sole runtime environment available on the machine. The embedded JS approach required:

- No external dependencies
- No network connectivity
- No compilation or build step
- No user training beyond "open the file"

The Acrobat JavaScript API (`this.getField()`, `parseFloat()`) provided everything needed to read form inputs, perform arithmetic, and write results back to fields.

## Why only 4 materials

The production section where this tool was deployed tracked exactly 4 material types. The form was purpose-built for that specific workflow rather than designed as a generic inventory system. Adding flexibility (dynamic rows, arbitrary material counts) would have increased complexity without delivering value for the actual use case.

## Why no database or external persistence

The PDF file itself acts as the database. Workers save the file at the end of the day, and the saved field values carry forward to the next session. This design was deliberate:

- No server infrastructure to set up or maintain
- No network dependency
- No credentials or authentication
- No risk of database corruption or connection failures

The simplicity is the feature. On a locked-down legacy system with no IT support, every external dependency is a potential failure point.

---

## Alternatives considered

### Excel spreadsheet

Would have been functionally adequate, but Adobe Reader was more reliable on the aging XP system than Microsoft Office. The PDF form was also simpler for non-technical factory workers -- fewer menus, fewer ways to accidentally break the layout, and a more constrained interface that guided users through the correct workflow.

### Web application

The technically superior option under normal circumstances, but it required two things that were not available: a modern browser installation and network connectivity. Both were blocked by IT policy on the production floor machines.

### Paper-based tracking

This was the existing method that PDF Stock Manager replaced. Paper tracking was error-prone (miscalculations, illegible handwriting, lost sheets) and did not provide automatic totals. The move to the PDF form eliminated arithmetic errors and gave workers an immediate view of current stock levels.
