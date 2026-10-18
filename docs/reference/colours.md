# Messages and colours

Everything a player sees lives in `plugins/JPPlayerVaults/messages_<language>.yml`. Nothing is
hardcoded, and the same file works on every supported Minecraft version.

## Two colour systems, one file

| Format | Where it works | What it looks like |
| --- | --- | --- |
| **Legacy** | Every version, 1.8 → latest | `&a`, `&l`, and `&#RRGGBB` (hex shows from 1.16) |
| **MiniMessage** | Paper and forks with Adventure (1.16+) | `<green>`, `<#ff8800>`, `<gradient:#2ec4b6:#12727f>`, `<bold>`, `<click:...>`, `<hover:...>` |

`config.yml` decides how the file is read:

```yaml
# legacy      -> & codes everywhere, works on any version
# minimessage -> the whole file is parsed as MiniMessage (needs Adventure)
# auto        -> legacy, but a message that uses tags is sent as a Component
messages-format: auto
```

**`auto` is the default and the safe choice.** The bundled files use `&` colours, so nothing changes
on old servers, and the moment you rewrite one message with MiniMessage tags that message is sent as
a proper Component — with gradients, hover and click — while the rest of the file stays as it was.

A message is treated as MiniMessage only when it contains tags *and* no `&` codes, which is what
keeps placeholders like `<number>` in the help text from being interpreted as tags.

## Examples

```yaml
# A gradient title, the vault number in grey
vault-title: '<gradient:#2ec4b6:#12727f><bold>Vault</bold></gradient> <gray>#{number}</gray>'

# Clickable help: the player clicks the command instead of typing it
command:
  help:
    - '<dark_gray><st>          </st></dark_gray> <green><bold>Vaults</bold></green> <dark_gray><st>          </st></dark_gray>'
    - ''
    - '<gray>» <click:run_command:''/pv 1''><aqua>/pv 1</aqua></click> <dark_gray>—</dark_gray> <gray>Open that private vault.</gray>'
    - '<gray>» <hover:show_text:''Reloads config.yml and the messages''><aqua>/pv reload</aqua></hover> <dark_gray>—</dark_gray> <gray>Reload.</gray>'

# A hover hint on the locked-vault error
command:
  locked: '<red>Vault <white>#{number}</white> is locked.</red> <hover:show_text:''Ask an administrator for a higher rank''><gray>[?]</gray></hover>'
```

The same applies to the vault picker: its title, the item name and the lore are built as Components
too, so `<gradient>` and `<hover>` work there as well.

## What is not a Component

To keep behaviour identical for existing installations, a message that does not use MiniMessage is
sent as plain text with legacy colours — exactly what the plugin did before. `/pv admin info` reports
which system is active:

```
text: auto (Adventure + MiniMessage)
text: auto (legacy only)          # Spigot, 1.8-1.15, or no Adventure
```

## Servers without Adventure

If you set `messages-format: minimessage` on a server that has no Adventure (Spigot, or an old
version), the tags are stripped so the text stays readable instead of showing raw `<red>` in the
chat, and the console warns once. Set `messages-format: legacy` there to remove even the warning.

## Placeholders

| Placeholder | Where it is available |
| --- | --- |
| `{prefix}` | Anywhere the prefix should appear (it is inserted as-is) |
| `{number}` | Vault titles, vault messages and the picker items |
| `{player}` | Admin messages and the admin vault title |
| `{available}` | Help and the locked-vault message |
| `{items}` | The picker lore (how many items a vault holds) |

Placeholders are replaced **before** the text is parsed, so do not put colour codes inside them.

## Adding a language

1. Copy `messages_en.yml` to `messages_<code>.yml` in the plugin folder.
2. Translate the values (not the keys).
3. Set `language: <code>` in `config.yml` and run `/pv reload`.

Missing keys fall back to the bundled English text, and the console lists them once at startup, so a
half-finished translation is safe to use. The code must be 2–16 characters: letters, digits, `_`
or `-`.
