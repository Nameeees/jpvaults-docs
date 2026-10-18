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

## What happens if the server crashes while a vault is open?

The last save is restored. Autosave runs every 300 seconds by default, so at most a few minutes of
changes can be lost; lower `storage.autosave-seconds` if your server is unstable.

## Will my players lose items when I change the vault size?

No. Lowering `vaults.size` hides the extra rows but keeps the items in the file; raising it again
brings them back.

## Does it work with Folia or with MySQL?

Neither is supported. The plugin is a standard Bukkit plugin with YAML storage.

## Can I translate the plugin?

Yes. Copy `messages_en.yml` to `messages_<code>.yml`, translate the values and set
`language: <code>`. Missing keys fall back to English and the console lists them once. See
[Languages & messages](getting-started/languages.md).

## Can players share a vault?

No, vaults are private per player and keyed by UUID. Nothing in the plugin exposes another player's
vault.

## Which files do I need to back up?

`plugins/JPPlayerVaults/` — the `data/` folder holds every player's items, and `config.yml` plus the
`messages_*.yml` files hold your setup.

## Why is the inventory title shortened?

Clients from 1.8 to 1.12 crash when a container title is longer than 32 characters, so the plugin
cuts longer titles and warns once in the console.

## Something is broken. What do I send?

The server version, the plugin version, your `config.yml`, and the console log around the error.
Reports are welcome in the [community Discord](https://discord.gg/HATd9QgukF) or the SpigotMC
discussion thread.
