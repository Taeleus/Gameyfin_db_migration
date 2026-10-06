# Gameyfin H2 → PostgreSQL Migration

A Python converter and step-by-step guide from my migration of an existing Gameyfin H2 database to PostgreSQL.

The converter takes a SQL export produced by H2's `org.h2.tools.Script` tool and writes a PostgreSQL-compatible SQL file for import. It does not connect to either database directly.

## Files

- [`h2_to_postgres.py`](Gameyfin_db_conversion/h2_to_postgres.py) — converts the H2 SQL export.
- [`HOW-TO.md`](Gameyfin_db_conversion/HOW-TO.md) — the full migration walkthrough, including H2 export, PostgreSQL setup, pgAdmin import, and Gameyfin configuration.

## Requirements

- Python 3; no third-party Python packages are required.
- An existing Gameyfin H2 database and an H2 SQL export.
- A PostgreSQL server and a client such as pgAdmin.
- A Gameyfin build with PostgreSQL support, including the JDBC driver and PostgreSQL Flyway migrations referenced in the guide.

The guide uses Docker on Unraid to export the H2 database. Adjust paths and credentials for your installation.

## Quick start

Back up your original database and stop Gameyfin before exporting it. Follow the [migration guide](Gameyfin_db_conversion/HOW-TO.md) to create `export.sql`, then run:

```bash
python Gameyfin_db_conversion/h2_to_postgres.py export.sql gameyfin_postgres.sql
```

Run this command from the repository root. On systems where Python 3 is named `python3`, use that instead.

Review the converter's output and any warnings, then follow the guide to import `gameyfin_postgres.sql` into PostgreSQL and configure Gameyfin to use it.

## What the converter handles

- Reassembles H2 large text objects into their stored values.
- Converts H2-specific data types and schema syntax.
- Adjusts sequences and identity column syntax.
- Converts binary and Unicode literals.
- Normalizes quoted uppercase identifiers to lowercase.
- Reports remaining H2-specific terms and uppercase quoted identifiers for review.

## Scope and database exports

This is a migration utility from a specific Gameyfin migration, not a general-purpose H2-to-PostgreSQL converter. Compatibility with every Gameyfin or database version has not been established. Its syntax checks do not replace validating the imported database and checking that Gameyfin starts and works correctly.

Database exports are intentionally excluded from this repository because they can contain user records and application configuration. The included `.gitignore` ignores SQL exports; keep your backups and generated files private.
