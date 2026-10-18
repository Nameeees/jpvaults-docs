# Installation

## Requirements

- A Spigot or Paper server, or any fork that implements the Bukkit API.
- Minecraft 1.8 or newer.
- The Java version your Minecraft release requires: Java 8 for 1.8, Java 21 for 1.21+, Java 25 for
  26.x. The plugin jar itself targets Java 8 bytecode.

No other plugin is needed: there is no Vault, no economy plugin, no database and no ProtocolLib
dependency.

## Steps

1. Download `JPPlayerVaults-<version>.jar` from the
   [releases page](https://github.com/Nameeees/JPPlayerVaults/releases) (or from the SpigotMC
   resource page).
2. Copy it into the server's `plugins/` folder.
3. Start the server (or run `/reload confirm` if it is already running).
4. Grant `jpvaults.vaults.<n>` to your ranks — see [Permissions](../reference/permissions.md).
5. Adjust `plugins/JPPlayerVaults/config.yml` if you need a different vault size, vault count or
   language — see [Configuration](configuration.md).

## Files created on first start

```text
plugins/JPPlayerVaults/
├── config.yml            settings (size, amounts, autosave, sounds, language)
├── messages_en.yml       English texts
└── data/                 one <uuid>.yml per player, created when they open a vault
```

`messages_es.yml` is written the first time you set `language: es`; both language files ship inside
the jar.

## Verify the installation

- `/plugins` lists **JPPlayerVaults** in green.
- `/pv help` answers with the player's available vault count.
- The console shows a line like:

```text
[JPPlayerVaults] Enabled: 54 slots per vault, up to 10 vaults per rank, language 'en'.
```

## Updating

Replace the jar and restart the server. Player data lives in `plugins/JPPlayerVaults/data/` and is
never modified by an update. If a new version adds configuration keys, the plugin keeps your values
and reports invalid ones in the console instead of failing.

## Uninstalling

Remove the jar. If you also want to remove the stored vaults, delete
`plugins/JPPlayerVaults/` — **that deletes every player's items**, so back the folder up first.
