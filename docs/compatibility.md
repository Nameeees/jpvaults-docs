# Compatibility

| Item | Value |
| --- | --- |
| Built against | Spigot API 1.8.8 (`--release 8`, class file version 52) |
| Java for the plugin | Java 8 or newer |
| Java for the server | Whatever your Minecraft release requires: Java 8 for 1.8, Java 21 for 1.21+, Java 25 for 26.x |
| Minecraft | 1.8 through the latest version |
| Server software | Spigot, Paper and forks that implement the Bukkit API |
| Dependencies | None |

## Tested matrix

| Minecraft | Server build | Java | Integration test | Real client |
| --- | --- | --- | --- | --- |
| 1.8.8 | Paper 445 | Temurin 8u504 | PASS — 20 checks, 0 failures | PASS (headless 1.8.8 client) |
| 26.3 | Paper 205 | Temurin 25.0.4 | PASS — 44 checks, 0 failures | not supported by the test client |

The integration test drives the real plugin inside a running server: storage round-trip of 54 slots
with item meta and non-ASCII text, `/pv` command paths, rank permissions, save on close, reopen from
disk, language switching, title truncation and corrupt-file backups. The real-client test opens
`/pv 1`, moves an item into the vault, disconnects, reconnects and verifies the item came back from
disk on a genuine 1.8.8 client.

## How one jar covers every version

- Only API that has existed since 1.8 is used, compiled against the Spigot 1.8.8 API.
- No NMS and no version-specific server classes.
- Version-specific pieces are detected at runtime:
    - the off-hand swap event (1.9+) is registered only if the class exists;
    - the click-type method is looked up by reflection (`getClickType` on modern versions,
      `getClick` on 1.8);
    - sound names are resolved by trying the modern name first (`BLOCK_ENDER_CHEST_OPEN`), then
      `BLOCK_ENDERCHEST_OPEN` (1.9–1.12), then `CHEST_OPEN` (1.8);
    - inventory titles are capped at 32 visible characters, which is what 1.8–1.12 clients accept.
- Text uses legacy `&` colour codes, which every version understands. `&#RRGGBB` hex colours are
  shown on 1.16+.

## Data across Minecraft versions

Player files are YAML written by Bukkit's own item serializer. When Minecraft renamed materials in
1.13+ (for example `WOOL` → `WHITE_WOOL`), Bukkit converts the names while loading, so a vault
created on 1.8 opens on a modern server with its items intact. Take a backup of
`plugins/JPPlayerVaults/data/` before a major upgrade anyway — it is one folder.

## Folia, proxies and databases

- **Folia** is not supported: the plugin uses the regular Bukkit scheduler.
- **BungeeCord / Velocity** work as usual; run the plugin on the backend servers, not on the proxy.
- **MySQL** is not supported; storage is local YAML, which keeps the plugin dependency-free and the
  files easy to back up.
