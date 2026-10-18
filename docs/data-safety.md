# Data safety

Vaults usually hold a server's most valuable items, so the plugin is designed not to duplicate or
lose them.

## Protections

| Protection | What it prevents |
| --- | --- |
| **Durable save on close** | Closing a vault or quitting waits until the file is on disk. If the server dies right after, the content is already safe — there is no rollback to duplicate. |
| **`Q` / `Ctrl+Q` cancelled** | Dropping items out of the vault GUI by accident. |
| **Outside clicks cancelled** | Losing the item held on the cursor to the void, which a vanilla chest would do. |
| **Off-hand key (`F`) guarded** | The 1.16.x cursor desync vector; the swap is cancelled while a vault is open. |
| **No zombie GUIs** | Reloading the plugin closes open vaults, and on enable any surviving vault view is closed, so an old inventory cannot be emptied twice. |
| **Switch without loss** | `/pv 1` then `/pv 2` saves the first vault regardless of event timing differences between versions. |
| **Atomic writes** | Every save writes a `.tmp` file and moves it over the real file: a crash mid-write cannot leave a half-written file. |
| **No cross-session cache** | Data is read from disk when the vault opens and dropped when the player quits. |
| **Autosave** | `storage.autosave-seconds` (300 by default) limits the loss if the server crashes while a vault is open. |
| **Backup on unreadable files** | A file that cannot be parsed, or an item that fails to deserialize, is copied to `<name>.<timestamp>.bak` before anything can overwrite it. |

## Storage format

One readable file per player:

```text
plugins/JPPlayerVaults/data/<uuid>.yml
```

```yaml
uuid: 30fecbe1-2271-3418-8553-d3ded0e95f56
last-known-name: TestBot
vaults:
  '1':
    items:
      '0':
        ==: org.bukkit.inventory.ItemStack
        type: DIAMOND
        amount: 5
```

This makes backups, migrations and manual repairs possible with any text editor. Stop the server (or
have the player quit) before editing a file by hand, and never edit it while the server is running.

## Known limits

- If the process is **killed while a player has a vault open**, the last save is restored (hence
  autosave). Saving on every click would tax the whole server.
- Closing a vault blocks the calling thread until the write finishes, bounded to 5 seconds. That is
  deliberate: it is what makes the durable-close guarantee possible.
- Do not share one `data/` folder between several running servers. The plugin does not sync servers
  live: two active servers with the same player is not a supported scenario.
- Autosave writes all open vaults every cycle; with hundreds of players holding vaults open at the
  same time, tune `storage.autosave-seconds` to your hardware.

## Recovering from a bad file

1. Stop the server or ask the player to quit.
2. Look for `data/<uuid>.yml.<timestamp>.bak` next to the file.
3. Compare both files, keep the good one and rename it to `<uuid>.yml`.
4. Start the server and open the vault.

If an item cannot be deserialized after a Minecraft update, the console names the player, the vault
and the slot, and the rest of the vault loads normally.
