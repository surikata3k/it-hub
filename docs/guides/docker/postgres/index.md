# PostgreSQL 18 with Docker Compose

A PostgreSQL 18 container with persistent data and editable configuration files (`postgresql.conf` and `pg_hba.conf`). Everything is set in a `.env` file, so you don't need to touch `compose.yaml`.

**Download:** [:material-download: compose.yaml](compose.yaml){ download } · [:material-download: env.example](env.example){ download }

!!! warning
    The password in `env.example` is a placeholder. Use a real one, and don't publish your `.env` file.

## What you get

- **PostgreSQL 18** (official `postgres` image), listening on port `5432`.
- **Persistent data** in the `pgdata` volume or in a host folder.
- **Your own configuration files.** On the first start, if the configuration folder is empty, default `postgresql.conf` and `pg_hba.conf` files are created. From then on they are never overwritten: the container always uses your edited copies.
- **Remote access ready:** `listen_addresses = '*'` and password login (`scram-sha-256`) from any address.
- **Healthcheck** with `pg_isready`, and automatic restart unless you stop it.

## Requirements

- Docker Desktop on Windows/macOS, or Docker Engine with the Compose plugin on Linux.

## 1. Prepare the folder

Create a folder (for example `postgres`) and put both files inside. Rename `env.example` to `.env`:

```
postgres/
├── compose.yaml
└── .env
```

=== "Windows (PowerShell)"

    ```powershell
    Rename-Item env.example .env
    ```

=== "Linux"

    ```bash
    mv env.example .env
    ```

Edit `.env` and set at least `POSTGRES_PASSWORD`.

## 2. Choose where data and configuration live

Each of `PG_DATA` and `PG_CONF` accepts a **name** (Docker volume) or a **path starting with `./`** (folder next to `compose.yaml`).

| Variable  | Docker volume     | Host folder     | Mounted at               |
|-----------|-------------------|-----------------|--------------------------|
| `PG_DATA` | `PG_DATA=pgdata`  | `PG_DATA=./data`| `/var/lib/postgresql`    |
| `PG_CONF` | `PG_CONF=pgconf`  | `PG_CONF=./conf`| `/etc/postgresql/custom` |

The default `.env` keeps **data in a volume** and **configuration in the `./conf` folder**, so you can edit the config files with VS Code or Notepad.

!!! danger "Data in a Windows folder"
    Avoid `PG_DATA=./data` on Windows. PostgreSQL needs strict file ownership and permissions that NTFS folders can't provide through Docker Desktop: initialization may fail with permission errors, and it is much slower. Keep the data in a volume. On Linux a host folder works fine.

!!! note "PostgreSQL 18 data path"
    Since PostgreSQL 18 the image stores data in `/var/lib/postgresql/18/docker`, so the volume is mounted at `/var/lib/postgresql` instead of `/var/lib/postgresql/data`.

## 3. Start it

```bash
docker compose up -d
docker compose ps          # STATUS should become "healthy"
docker compose logs -f     # Ctrl+C to stop following
```

On the first start the logs show `>> Created .../postgresql.conf` and `>> Created .../pg_hba.conf`.

## 4. Connect

From the container:

```bash
docker exec -it pg18 psql -U postgres
```

From the host or another machine (pgAdmin, DBeaver, `psql`…):

| Setting  | Value                                  |
|----------|----------------------------------------|
| Host     | `localhost` (or the host's IP address) |
| Port     | `5432` (`PG_PORT`)                     |
| User     | `postgres` (`POSTGRES_USER`)           |
| Password | `POSTGRES_PASSWORD`                    |
| Database | `postgres` (`POSTGRES_DB`)             |

Check which configuration files are in use:

```sql
SHOW config_file;
SHOW hba_file;
```

## 5. Edit the configuration

=== "Config in a host folder (`./conf`)"

    Open `conf/postgresql.conf` or `conf/pg_hba.conf` with any editor and save.

=== "Config in a volume (`pgconf`)"

    The image has no text editor. Copy the file out, edit it and copy it back:

    ```bash
    docker cp pg18:/etc/postgresql/custom/pg_hba.conf ./pg_hba.conf
    # edit pg_hba.conf
    docker cp ./pg_hba.conf pg18:/etc/postgresql/custom/pg_hba.conf
    ```

    Or open it directly with the VS Code Docker / Dev Containers extension.

Then apply the changes:

```bash
# pg_hba.conf and most parameters: reload, no restart needed
docker exec pg18 psql -U postgres -c "SELECT pg_reload_conf();"

# Parameters such as listen_addresses, port or shared_buffers: restart
docker restart pg18
```

!!! tip
    To find out whether a parameter needs a restart, check its `context` column: `postmaster` means restart.

    ```sql
    SELECT name, setting, context FROM pg_settings WHERE name = 'shared_buffers';
    ```

!!! tip "Back to the defaults"
    Delete (or rename) `postgresql.conf` or `pg_hba.conf` and restart the container: a fresh default file is created.

## Useful commands

```bash
docker compose stop            # stop (keeps the container)
docker compose start           # start again
docker compose down            # remove the container, keep the data
docker compose down -v         # remove the container AND the volumes (all data!)
docker volume ls               # list volumes (pgdata, pgconf)
```

Back up and restore a database:

```bash
docker exec pg18 pg_dump -U postgres -d mydb > mydb.sql
docker exec -i pg18 psql -U postgres -d mydb < mydb.sql
```

## The files

??? example "compose.yaml"

    ```yaml
    --8<-- "guides/docker/postgres/compose.yaml"
    ```

??? example "env.example"

    ```ini
    --8<-- "guides/docker/postgres/env.example"
    ```
