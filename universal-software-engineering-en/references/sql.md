# SQL / Database

## Practices
- Write keywords in uppercase (`SELECT`, `INSERT`, `JOIN`, `WHERE`) for readability.
- **No `SELECT *` in application code**; list column names explicitly so code does not break when the schema changes.
- **Always use parameterized queries / prepared statements**; never concatenate user input into SQL strings (prevents SQL injection).
- Schema changes must go through reversible migrations, never manual edits on a live environment.
- Add indexes based on real queries; use `EXPLAIN` when something looks slow. Watch for N+1 queries inside loops.
- Wrap related write operations in a transaction.
