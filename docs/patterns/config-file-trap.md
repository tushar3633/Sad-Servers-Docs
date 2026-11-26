# Pattern: The Wrong Config File Trap

**Scenario Reference:** [Bucharest](../incidents/2026-11-19-bucharest.md)

## The Problem
You edit a configuration file, restart the service, and the problem persists. You check the file again, and your changes are definitely there.

## Why it happens
Modern Linux services (especially databases and containerized apps) don't always use the default `/etc/` paths. They might have been initialized with custom data directories, environment variables (like `PGDATA`), or custom configuration flags.

## How to find the *actual* config file

Instead of using `find`, ask the running service where its config lives.

*   **PostgreSQL:**
    ```bash
    sudo -u postgres psql -c "SHOW config_file;"
    sudo -u postgres psql -c "SHOW hba_file;"