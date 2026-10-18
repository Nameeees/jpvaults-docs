# JPPlayerVaults

![JPPlayerVaults](assets/banner.png)

**Private, rank-based player vaults for Minecraft 1.8 to the latest version — one jar, no dependencies.**

JPPlayerVaults gives every player a set of private chests: `/pv 1`, `/pv 2`, and so on. How many
vaults a player can open is decided by their rank, not by the plugin, and each vault is a normal
inventory of up to **54 slots** (6 rows) — the largest Minecraft allows.

The same jar runs from Minecraft 1.8.8 to the latest release on Spigot, Paper and their forks. It is
built against the Spigot 1.8.8 API, uses no NMS and no third-party libraries, and sends nothing over
the network.

<div class="grid cards" markdown>

-   :material-download:{ .lg .middle } **Installation**

    ---

    Drop the jar in `plugins/`, start the server and grant the rank permissions.

    [:octicons-arrow-right-24: Install](getting-started/installation.md)

-   :material-cog:{ .lg .middle } **Configuration**

    ---

    Vault size, vault count per rank, autosave, storage folder and sounds.

    [:octicons-arrow-right-24: Configure](getting-started/configuration.md)

-   :material-translate:{ .lg .middle } **Languages**

    ---

    English and Spanish are included; every message lives in a language file.

    [:octicons-arrow-right-24: Translate](getting-started/languages.md)

-   :material-shield-check:{ .lg .middle } **Data safety**

    ---

    Durable saves, atomic writes and backups when a player file cannot be read.

    [:octicons-arrow-right-24: How it works](data-safety.md)

</div>

## Quick reference

| Command | What it does | Permission |
| --- | --- | --- |
| `/pv <number>` | Opens your vault with that number. | `jpvaults.use` |
| `/pv` or `/pv help` | Shows the help and how many vaults you have. | `jpvaults.use` |
| `/pv reload` | Reloads `config.yml` and the messages. | `jpvaults.reload` |

Aliases: `/playervaults`, `/jpvaults`. See [Commands](reference/commands.md).

## Compatibility

| Item | Value |
| --- | --- |
| Minecraft | 1.8 through the latest version (26.3 at the time of writing) |
| Server software | Spigot, Paper and forks that implement the Bukkit API |
| Java | Java 8 or newer for the plugin; the server itself needs the Java version its Minecraft release requires |
| Dependencies | None |

The full tested matrix is in [Compatibility](compatibility.md).

## Support

- Community Discord: <https://discord.gg/VQSvfQw8v8>
- Source code and releases: <https://github.com/Nameeees/JPPlayerVaults>
- Bug reports: open an issue in the plugin repository, or use the discussion thread on the SpigotMC
  resource page. Please include the server version, the plugin version, your `config.yml` and the
  relevant console log.

## License

MIT — you may use, modify and redistribute JPPlayerVaults, including on commercial servers, as long
as the copyright notice is kept.
