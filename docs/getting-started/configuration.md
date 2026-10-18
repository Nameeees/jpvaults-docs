# Configuration

Everything lives in `plugins/JPPlayerVaults/config.yml`:

```yaml
config-version: 1
language: en

vaults:
  size: 54              # 9-54, multiple of 9
  default-amount: 1     # vaults for players without a rank permission
  max-amount: 10        # highest jpvaults.vaults.<n> checked

storage:
  type: yaml            # yaml or mysql (database over JDBC)
  folder: 'data'
  autosave-seconds: 300 # 0 disables autosave
  mysql:                # only used when type is mysql
    host: '127.0.0.1'
    port: 3306
    database: 'jpvaults'
    username: 'root'
    password: ''
    table-prefix: 'jpvaults_'

sounds:
  enabled: true
  volume: 0.7
  pitch: 1.4
```

| Key | Description | Default |
| --- | --- | --- |
| `language` | Message language (`messages_<language>.yml`). Bundled: `en`, `es`. | `en` |
| `vaults.size` | Slots per vault. Multiple of 9, from 9 to 54. | `54` |
| `vaults.default-amount` | Vaults for players without any `jpvaults.vaults.<n>` permission. | `1` |
| `vaults.max-amount` | Highest `jpvaults.vaults.<n>` the plugin checks (1–100). | `10` |
| `storage.type` | `yaml` or `mysql`. Changing it requires a restart. | `yaml` |
| `storage.mysql.*` | Connection details for the database backend; see [Storage backends](storage.md). | — |
| `storage.folder` | Folder inside the plugin folder with the per-player files. Changing it requires a restart. | `data` |
| `storage.autosave-seconds` | How often open vaults are saved. `0` disables autosave; close, quit and shutdown still save. | `300` |
| `sounds.enabled` | Interface sounds. | `true` |
| `sounds.volume` | Sound volume (0.0–1.0). | `0.7` |
| `sounds.pitch` | Sound pitch (0.5–2.0). | `1.4` |

## Validation

Invalid values are clamped and reported in the console instead of breaking the plugin:

- `vaults.size` is rounded down to the nearest multiple of 9 and limited to 9–54.
- `vaults.max-amount` is limited to 1–100.
- `sounds.volume` is limited to 0.0–1.0 and `sounds.pitch` to 0.5–2.0.
- An empty or missing `storage.folder` falls back to `data`.

## Applying changes

- Most settings take effect on the next vault that opens.
- `storage.folder` needs a server restart; the plugin warns you if you change it with `/pv reload`.
- The vault size can be lowered temporarily **without deleting anything**: items in hidden rows stay
  in the file and reappear when the size is raised again.

## Example: three vaults for VIP, six for MVP

```yaml
vaults:
  size: 54
  default-amount: 1
  max-amount: 10
```

```text
/lp group vip permission set jpvaults.vaults.3 true
/lp group mvp permission set jpvaults.vaults.6 true
```

Players without any of those permissions keep `default-amount` (1) vault.
