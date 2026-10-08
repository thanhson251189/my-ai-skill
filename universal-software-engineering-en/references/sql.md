# SQL / Database

## Check commands before reporting done
There is no single linter required for every project. If the project has sqlfluff, a migration runner, or database tests, run that.

If it has none: reread the statements you changed and check parameterization and transactions. Run `EXPLAIN` only when the change is about slowness and a database is available to run it. Do not add a migration framework yourself.

## Practices
- Follow the project's SQL style. If it has none, write keywords in uppercase (`SELECT`, `INSERT`, `JOIN`, `WHERE`).
- **No `SELECT *` in application code**; list column names explicitly so code does not break when the schema changes.
- **Always use parameterized queries / prepared statements**; never concatenate user input into SQL strings (prevents SQL injection).
- Schema changes must go through reversible migrations, never manual edits on a live environment. If the project has no migration tool, say so; do not pick one yourself.
- Add indexes based on real queries; use `EXPLAIN` when something looks slow. Watch for N+1 queries inside loops.
- Wrap related write operations in a transaction.
