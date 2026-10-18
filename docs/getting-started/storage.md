# Storage backends

The plugin ships with two backends and you pick one in `config.yml`:

```yaml
storage:
  type: yaml      # yaml (default) or mysql
```

Changing `type` requires a server restart; `/pv reload` warns you if you do it at runtime.

## YAML (default)

- One readable file per player: `plugins/JPPlayerVaults/data/<uuid>.yml`.
- Format: `vaults.<number>.items.<slot>: <Bukkit item>`, plus the last known player name so
  administrators can identify the file.
- Data is read from disk when a vault opens and dropped when the player quits, so a restart always
  starts from the real file.
- Lowering `vaults.size` never deletes anything: items in hidden rows stay in the file and reappear
  when the size is raised again.
- Bukkit converts material names that were renamed in 1.13+ while loading, so data survives
  Minecraft upgrades.
- Every save is written to a `.tmp` file and moved over the real one (atomic write).

If a file cannot be read (corrupted or hand-edited YAML), the plugin keeps a timestamped `.bak` copy
and **refuses to open that vault** until an administrator restores it. Showing an empty vault would
let the next save overwrite the real items. A single item that fails to deserialize does not prevent
the rest of the vault from loading.

## Database over JDBC (MySQL / MariaDB)

```yaml
storage:
  type: mysql
  mysql:
    host: '127.0.0.1'
    port: 3306
    database: 'jpvaults'
    username: 'root'
    password: ''
    table-prefix: 'jpvaults_'
    properties: 'useSSL=false&characterEncoding=utf8'
    pool-size: 4
    connection-timeout-ms: 5000
```

| Key | Description | Default |
| --- | --- | --- |
| `host`, `port`, `database` | Where the database lives. | `127.0.0.1`, `3306`, `jpvaults` |
| `username`, `password` | Credentials. | `root`, empty |
| `table-prefix` | Prefix of the two tables (letters, digits and `_` only). | `jpvaults_` |
| `properties` | Extra JDBC URL parameters, MySQL syntax. | `useSSL=false&characterEncoding=utf8` |
| `jdbc-url` | Full URL; overrides host/port/database. Use it for another engine. | empty |
| `driver-class` | JDBC driver to load. | empty (auto-detect) |
| `pool-size` | Connections the plugin keeps and reuses. | `4` |
| `connection-timeout-ms` | Connection timeout hint passed to the driver. | `5000` |

### Schema

The plugin creates the tables on startup:

```text
<prefix>players   (uuid VARCHAR(36) PRIMARY KEY, last_name VARCHAR(16), updated_at BIGINT)
<prefix>vaults    (uuid VARCHAR(36), vault INT, data BLOB, updated_at BIGINT,
                   PRIMARY KEY (uuid, vault))
```

One row per vault. Saving writes the player row and replaces that player's vault rows inside a
single transaction (delete + insert), so a failure halfway cannot leave a vault half saved.

### The driver

The plugin uses plain `java.sql` and takes the driver from the server: Spigot and Paper ship MySQL
Connector/J, so normally you do not have to do anything. It tries, in order:

1. `storage.mysql.driver-class`, if you set one.
2. `com.mysql.cj.jdbc.Driver`
3. `com.mysql.jdbc.Driver`
4. `org.mariadb.jdbc.Driver`

For another engine, set `driver-class` and `jdbc-url` (for example
`jdbc:postgresql://localhost:5432/jpvaults` with `org.postgresql.Driver`), as long as that driver is
on the server's classpath. The driver is instantiated directly instead of going through
`DriverManager`, which is what lets drivers loaded by other plugins or by the server work.

### Behaviour

- Reads happen on the server thread when a vault opens. Writes happen on a single plugin thread, in
  order, and closing a vault waits for its transaction (bounded to 5 seconds).
- If the database cannot be reached at startup, the plugin logs the error and **keeps working with
  YAML** instead of leaving the server without vaults. Fix the settings, restart, and run
  `/pv admin migrate` to copy those files into the database.
- `/pv admin info` shows which backend is active and where it points.

## Moving between backends

`/pv admin migrate` copies every YAML file of the configured folder into the **active database**:

- Without arguments it skips players that already have rows in the database.
- `/pv admin migrate force` overwrites the players that are already stored.
- It runs in batches (5 players per tick) on the server thread, with the database writes
  asynchronous, so it cannot freeze the server. Progress goes to whoever ran it and the console.
- The YAML files are never deleted: they remain as a backup. Delete them yourself once you are happy
  with the database.

The opposite direction is **not** automated. If you switch `type` back to `yaml`, the plugin starts
from the files it finds, and anything written to the database meanwhile would be missing. Keep the
YAML folder (or a database dump) until you are sure, and prefer migrating YAML → database.
