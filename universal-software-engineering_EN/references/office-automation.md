# Office Automation (VBA / Google Apps Script / Office Scripts)

General rule: **read and write data in batches (in-memory arrays)**, not cell by cell in a loop. Every round trip to the spreadsheet is slow.

## VBA
- Always put `Option Explicit` at the top of each module.
- Do not use `.Select` / `.Activate`; operate directly on Range/Worksheet objects.
- Read a whole range into an array (`arr = rng.Value`), process it in memory, then write it back once.
- Turn off screen updating while running: `Application.ScreenUpdating = False`, and **always turn it back on** (even on error; use `On Error GoTo` for cleanup).

## Google Apps Script
- Use `getValues()` / `setValues()` over ranges, not `getValue()` per cell.
- Use `LockService` to avoid conflicts when several runs write to the same sheet at once.
- Call `SpreadsheetApp.flush()` when write ordering must be guaranteed.
- Mind Apps Script's execution time limit; process in batches if the data is large.

## Office Scripts (Excel on the web, written in TypeScript)
- Apply the TypeScript rules (`references/typescript.md`).
- Read/write by range with `getValues()` / `setValues()`; minimize per-cell API calls.
