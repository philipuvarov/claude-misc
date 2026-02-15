---
name: duckdb
description: Run a SQL query against a DuckDB database and display results.
allowed-tools: Bash, AskUserQuestion
---

## Instructions

### Find database

1. Check if user specified a db path in `${ARGUMENTS}`. If so, use that path.
2. If user did not specify a path, check the current directory for `*.duckdb` files. If exactly one is found, use it. If multiple are found, list them and ask the user which one to use.
3. If no db file is found, STOP and ask the user to provide the path to a DuckDB database file.

### Understand the request

- If `${ARGUMENTS}` is empty or asks for help, run `.tables` and `DESCRIBE` each table to show the full schema.
- If `${ARGUMENTS}` is a raw SQL query (starts with SELECT, INSERT, UPDATE, DELETE, CREATE, ALTER, DROP, PRAGMA, WITH, EXPLAIN, or similar SQL keywords), use it directly.
- If `${ARGUMENTS}` is a natural language question, translate it to SQL. **Always show the generated SQL to the user before executing it.**

### Confirm mutating queries

Before executing any mutating query (INSERT, UPDATE, DELETE, DROP, ALTER, CREATE, TRUNCATE), use AskUserQuestion to ask the user for confirmation. Show them the exact SQL that will be run.

### Execute query

Run the query using the DuckDB CLI via Bash:

```
duckdb path/to/db.duckdb "YOUR SQL QUERY HERE"
```

- For large result sets, add `LIMIT 20` unless the user explicitly specifies a different limit or asks for all rows.
- Use DuckDB's default output format (do not transform results).
- Display results and explain findings clearly to the user.

### Handle errors

If a query fails:

1. Read the DuckDB error message.
2. Attempt to diagnose and fix the issue (e.g. typo in table/column name, syntax error, wrong data type).
3. Show the corrected SQL and re-run it.
4. If it fails again, attempt one more fix (up to 2 retries total).
5. If it still fails after 2 retries, show the final error message to the user and suggest what might be wrong.
