# "Bucharest": Connecting to Postgres — Easy/Fix

**Date:** 2026-11-19
**Status:** Resolved

## Symptom
*   **Behavior:** A web application could not connect to the PostgreSQL database over TCP (127.0.0.1). The test command `PGPASSWORD=app1user psql -h 127.0.0.1 -d app1 -U app1user -c '\q'` failed with a `FATAL: pg_hba.conf rejects connection` error.

## Investigation Log
*   **Hypothesis 1:** The `pg_hba.conf` file is misconfigured. The scenario mentioned a "client-authentication change."
    *   **Test:** Located and edited `/etc/postgresql/17/main/pg_hba.conf`.
    *   **Result:** Found a "catch-all" `reject` rule placed above the specific `app1user` rules. Moved the `app1user` rules to the top, saved, and reloaded/restarted Postgres.
*   **Failed Attempts:** After editing the `/etc/` file, the connection still failed. This meant Postgres was not reading the file I edited.
*   **Hypothesis 2:** Postgres is pointing to a different configuration directory.
    *   **Test:** `sudo -u postgres psql -c "SHOW hba_file;"`
    *   **Result:** Revealed the database was actually reading from `/var/lib/postgresql/17/main/pg_hba.conf` (likely due to a custom `PGDATA` environment variable or container setup).

## Root Cause
Two issues compounded:
1. The `pg_hba.conf` file had broad `reject` rules placed *before* the specific `app1user` rules. Because Postgres evaluates the file top-to-bottom, it rejected the connection before ever reaching the allow rule.
2. The service was reading the config from a non-standard `/var/lib/` path instead of the default `/etc/` path.

## Solution
*   **Fix:** Edited the correct file at `/var/lib/postgresql/17/main/pg_hba.conf`. Moved the two `app1user` lines (for `127.0.0.1/32` and `::1/128`) above the `reject` lines. Restarted the service with `sudo systemctl restart postgresql`.
*   **Verification:** Re-ran the `psql` test command. It exited cleanly with 0.

## Lesson / Pattern
*   **HBA Order Matters:** `pg_hba.conf` is evaluated top-to-bottom. The first matching rule wins. Catch-all `reject` rules must always go at the very bottom.
*   **Config File Trap:** Never assume a service is reading from `/etc/`. Always ask the service directly (e.g., `SHOW hba_file;` in Postgres) to find the exact path of the active config file.