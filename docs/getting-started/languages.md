# Languages & messages

Every visible text is configurable; nothing is hardcoded in the plugin.

```text
plugins/JPPlayerVaults/
├── messages_en.yml    English (generated on first start)
└── messages_es.yml    Spanish (generated when you switch language)
```

Set the language in `config.yml` and reload:

```yaml
language: es
```

```text
/pv reload
```

## File format

```yaml
config-version: 1

prefix: "&8[&a&lVaults&8] &7» "
vault-title: "&8» &a&lVault &f#{number} &8«"

command:
  help:
    - "&8&m         &r &a&lVaults &8» &7Help &8&m         "
    - ""
    - "&8» &f/pv 1&7, &f/pv 2&7… &7— Open that private vault."
    - ""
    - "&7Vaults available: &a{available}&7."
  opened: "{prefix}&7Vault &f#{number}&7 opened."
  reloaded: "{prefix}&aConfig and messages reloaded."
  player-only: "{prefix}&cThis command can only be used in game."
  no-permission: "{prefix}&cYou don't have permission to do that."
  invalid-number: "{prefix}&f{input} &cis not a valid vault number. Try &f/pv 1&c."
  no-vaults: "{prefix}&cYou don't have any vaults assigned. Ask an administrator."
  locked: "{prefix}&cVault &f#{number} &cis locked. Your rank allows &f{available}&c."
```

## Placeholders

| Placeholder | Used in | Value |
| --- | --- | --- |
| `{prefix}` | Any message | The `prefix` value of the same file. |
| `{number}` | `vault-title`, `command.opened`, `command.locked` | The vault number. |
| `{available}` | `command.help`, `command.locked` | How many vaults the player can open. |
| `{input}` | `command.invalid-number` | What the player typed. |

Placeholders are inserted **raw**: do not put colour codes inside them.

## Colours

- Legacy codes with `&` work on every supported version: `&a`, `&c`, `&l`, `&o`, …
- Hex colours (`&#RRGGBB`) are also accepted and are displayed on 1.16 and newer; older versions
  show the text without that colour.

## Hiding a message

Leave a message empty to stop sending it:

```yaml
command:
  opened: ""
```

Messages that end up without visible content (empty or only colour codes) are never sent.

## Adding another language

1. Copy `messages_en.yml` to `messages_<code>.yml` in the plugin folder.
2. Translate the values, keeping the keys and the placeholders.
3. Set `language: <code>` in `config.yml` and run `/pv reload`.

The code must be 2–16 characters long and use only letters, digits, `_` or `-`. Any code is accepted
as long as the file exists; if it does not, the plugin warns and falls back to English. Missing keys
inside an existing translation also fall back to English, key by key, and the console lists them
once at startup.

## Inventory title limit

The inventory title is capped at **32 visible characters** for compatibility with 1.8–1.12 clients.
A longer `vault-title` is shortened and the plugin logs a warning once.
