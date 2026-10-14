# MagarZ Cloud upgrade and compatibility notes

This archive keeps the existing `.data` directory layout, server IDs, container names (`jtg-server-*`), Docker Compose service/container identifiers, and legacy API-key authentication prefixes so existing deployments can be migrated without renaming running containers.

## Before upgrading an existing panel

1. Stop the panel process.
2. Back up `.data/`, `backups/`, `.env`, and any custom installer/service configuration.
3. Keep a copy of the previous source directory.
4. Extract this release over a separate directory first and compare any locally modified files before replacing the running source.
5. Install dependencies and build from the new source, then restart the panel and check `/api/health`.

Do not delete `.data/servers/`; it contains service files, Minecraft worlds, bot templates, and Lavalink configuration. Do not overwrite your existing `.env` without carrying over `JWT_SECRET`, owner credentials, node connection settings, and any integration keys.

## New service types

- Web apps use the existing `nodejs` and `python` runtimes.
- Bot services use `bot-nodejs` or `bot-python` and require Docker runtime.
- Lavalink uses `lavalink` and is currently supported on the local Docker node only.
- New API keys use the `mz-` prefix; existing `jtg-` and `jtg_` keys remain accepted for compatibility.

## Recovery

If a service does not start, check the service console/logs, confirm Docker is healthy, verify the selected host port is available, and restore from your backup if the migration altered existing data.
