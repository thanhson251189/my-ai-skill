# Office Automation (VBA / Google Apps Script / Office Scripts)

General rule: **read and write data in batches (in-memory arrays)**, not cell by cell in a loop. Every round trip to the spreadsheet is slow.

## VBA
- Always put `Option Explicit` at the top of each module.
- Do not use `.Select` / `.Activate`; operate directly on Range/Worksheet objects.
- Read a whole range into an array (`arr = rng.Value`), process it in memory, then write it back once. A single cell makes `rng.Value` a scalar, not an array. Assign it straight into an array only when the range has two or more cells; wrap a single cell into an array before shared processing.
- Turn off screen updating while running: `Application.ScreenUpdating = False`, and **always turn it back on** (even on error; use `On Error GoTo` for cleanup).

## Google Apps Script
- Use `getValues()` / `setValues()` over ranges, not `getValue()` per cell.
- Use a `LockService` lock when concurrent runs may write the same sheet. Take the lock, wait, and always release it in `finally`:
  ```javascript
  const lock = LockService.getScriptLock();
  lock.waitLock(30000);
  try {
    // read and write in batches
  } finally {
    lock.releaseLock();
  }
  ```
- Call `SpreadsheetApp.flush()` when write ordering must be guaranteed.
- Mind Apps Script's execution time limit; process in batches if the data is large.

## Office Scripts (Excel on the web, written in TypeScript)
- Apply the typing rules in `typescript.md` in this same directory: no `any`, validate data at the boundary. Do not apply Biome, Vitest, or `tsc` unless the project already has those commands.
- Read/write by range with `getValues()` / `setValues()`; minimize per-cell API calls.
