# FAQ

## Do I need Vault or an economy plugin?

No. Access is controlled only by permissions: give a rank `jpvaults.vaults.<n>` and its players get
that many vaults.

## Can a player have more vaults than `max-amount`?

Raise `vaults.max-amount` (up to 100) and grant the permission with that number. There is no need to
declare the node anywhere.

## Can I edit a player's vault from the console?

The data is plain YAML in `plugins/JPPlayerVaults/data/<uuid>.yml`. Have the player quit (or stop the
server) before editing it. An in-game administrator editor is not part of 1.0.

## Can I edit a player's vault from the console?

Yes. `/pv admin open <player|uuid> <number>` opens it in game, and `/pv admin info <player>` shows
what that player has stored. On YAML storage you can also edit
`plugins/JPPlayerVaults/data/<uuid>.yml` with the server stopped or the player offline. See
[Admin commands](reference/admin-commands.md).

## What happens if the server crashes while a vault is open?

The last save is restored. Autosave runs every 300 seconds by default, so at most a few minutes of
changes can be lost; lower `storage.autosave-seconds` if your server is unstable.

## Will my players lose items when I change the vault size?

No. Lowering `vaults.size` hides the extra rows but keeps the items in the file; raising it again
brings them back.

## Does it work with MySQL?

Yes, optionally. Set `storage.type: mysql` and the vaults are stored in a database over JDBC (MySQL,
MariaDB or anything compatible), with the tables created automatically. YAML stays the default, and
`/pv admin migrate` copies your existing files into the database. If the database cannot be reached
the plugin logs the error and keeps running on YAML rather than leaving the server without vaults.
See [Storage backends](getting-started/storage.md).

## Does it work with Folia?

No, it is a standard Bukkit plugin that uses the regular scheduler.

## Can I translate the plugin?

Yes. Copy `messages_en.yml` to `messages_<code>.yml`, translate the values and set
`language: <code>`. Missing keys fall back to English and the console lists them once. See
[Languages & messages](getting-started/languages.md).

## Can players share a vault?

No, vaults are private per player and keyed by UUID. Nothing in the plugin exposes another player's
vault; administrators can open one with [admin commands](reference/admin-commands.md), which is
logged.

## Which files do I need to back up?

`plugins/JPPlayerVaults/` — the `data/` folder holds every player's items, and `config.yml` plus the
`messages_*.yml` files hold your setup.

## Why is the inventory title shortened?

Clients from 1.8 to 1.12 crash when a container title is longer than 32 characters, so the plugin
cuts longer titles and warns once in the console.

## Something is broken. What do I send?

The server version, the plugin version, your `config.yml`, and the console log around the error.
Reports are welcome in the [community Discord](https://discord.gg/VQSvfQw8v8) or the SpigotMC
discussion thread.
