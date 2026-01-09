# asdf-paradedb (mise plugin)

An ASDF (legacy) plugin compatible with [mise](https://mise.jdx.dev/) for installing ParadeDB’s **pg_search** Postgres extension from GitHub Releases.

## Status

- macOS (arm64) supported.
- Linux/Windows are not implemented yet.

## Install (mise)

```sh
mise plugin install paradedb https://github.com/<you>/asdf-paradedb.git

# The plugin will try to auto-detect your PostgreSQL major version via `pg_config`/`postgres` on PATH.
# Override if needed:
PARADEDB_PG_MAJOR=16 mise install paradedb@0.21.0
```

## Install (asdf)

```sh
asdf plugin add paradedb https://github.com/<you>/asdf-paradedb.git

# The plugin will try to auto-detect your PostgreSQL major version via `pg_config`/`postgres` on PATH.
# Override if needed:
PARADEDB_PG_MAJOR=16 asdf install paradedb 0.21.0
```

## macOS version selection

By default the plugin detects:

- macOS 14 → `sonoma`
- macOS 15 → `sequoia`

Override if needed:

```sh
PARADEDB_MACOS_CODENAME=sonoma PARADEDB_PG_MAJOR=16 mise install paradedb@0.21.0
```

## Using the installed files

This plugin installs the extension files into the tool prefix (e.g. `$(mise where paradedb@0.21.0)`), not directly into your Postgres installation.

To copy into a specific Postgres install (example for PG 16):

```sh
PREFIX="$(mise where paradedb@0.21.0)"

# Copy the shared library
cp -f "$PREFIX/lib/postgresql/pg_search.dylib" "$(pg_config --pkglibdir)/"

# Copy extension control + SQL files
cp -f "$PREFIX/share/postgresql@16/extension/"* "$(pg_config --sharedir)/extension/"
```

Then in Postgres:

```sql
CREATE EXTENSION pg_search;
```

## About Me

I am Ziyan Junaideen, lead engineer (Tooling) at [Edge Payments Technologies](https://www.tryedge.io). If you have any questions, feel free to reach out on [Twitter](https://x.com/ZiyanJunaideen).
