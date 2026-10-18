# Admin commands

Everything under `/pv admin` needs the `jpvaults.admin` permission (OP by default). The target of a
command can be the name of someone who has played on the server, or a UUID — useful for accounts
that are no longer in the player cache; copy it from the logs or from a stored row.

| Command | What it does |
| --- | --- |
| `/pv admin open <player\|uuid> <number>` | Opens that player's vault, online or offline. |
| `/pv admin info` | Reports the plugin status. |
| `/pv admin info <player\|uuid>` | Reports one player. |
| `/pv admin delete <player\|uuid> <number> confirm` | Wipes one vault. |
| `/pv admin migrate [force]` | Copies the YAML files into the database. |

## Opening someone's vault

```text
/pv admin open Steve 3
/pv admin open 30fecbe1-2271-3418-8553-d3ded0e95f56 3
```

- The vault opens like any other, with the owner's name in the title, and you can move items. Every
  change is saved to the **owner's** data, never to yours.
- A vault can only be open for one person at a time. If the owner (or another administrator) has it
  open, the command refuses and tells you who is using it. The same rule protects
  `/pv admin delete`.
- When you close the vault, the data is written and dropped from memory again, so nothing stays
  cached for an offline player.
- The console logs who opened whose vault, and when.

## Inspecting

```text
/pv admin info
/pv admin info Steve
```

Without arguments you get the backend in use and where it points, how many players have data, and
the vault settings (size, default amount, maximum per rank, autosave).

For a player you get the UUID, whether they are online, how many vaults hold items, how many items
are stored in total, how many vaults their rank allows (if online) and who has a vault open right
now.

## Deleting one vault

```text
/pv admin delete Steve 3 confirm
```

The literal word `confirm` is required, and the vault must not be open. Deleting a vault removes its
contents from the active backend (the file row or the database row); the player's other vaults are
untouched. The action is logged.

## Migrating YAML into the database

Only useful when `storage.type: mysql` is active:

```text
/pv admin migrate          # skips players already in the database
/pv admin migrate force    # overwrites them
```

While it runs you get progress messages, and a summary at the end: players copied, vaults, items,
players skipped and errors. The YAML files are left in place as a backup; see
[Storage backends](../getting-started/storage.md).

## Permissions

| Permission | Allows |
| --- | --- |
| `jpvaults.admin` | All `/pv admin` subcommands. |
| `jpvaults.reload` | `/pv reload`. |
| `jpvaults.use` | `/pv` for the player's own vaults. |

Nothing under `/pv admin` is granted by default except to operators. To let a specific group help
players without giving them full operator access:

```text
/lp group helper permission set jpvaults.admin true
```
