# Commands

| Command | What it does | Permission |
| --- | --- | --- |
| `/pv <number>` | Opens your private vault with that number. | `jpvaults.use` |
| `/pv` or `/pv help` | Shows the help and how many vaults you have. | `jpvaults.use` |
| `/pv reload` | Reloads `config.yml` and the messages without restarting. | `jpvaults.reload` |

Aliases: `/playervaults`, `/jpvaults`.

## Examples

```text
/pv 1          opens vault #1
/pv 6          opens vault #6 (needs jpvaults.vaults.6 or higher)
/pv help       shows the help and "Vaults available: 3"
/pv reload     reloads config and messages (administrators)
```

## Behaviour

- The number must be a whole number. Anything else answers with the `command.invalid-number`
  message.
- If the player has no vaults at all (`vaults.default-amount: 0` and no rank permission), the
  command answers with `command.no-vaults`.
- If the number is higher than the player's rank allows, the answer is `command.locked` with the
  number they tried and the number they have.
- Switching vaults (`/pv 1` then `/pv 2` without closing) saves the first one before opening the
  second.
- Running the command from the console answers with the player-only message; `/pv reload` does work
  from the console.

## Tab completion

| Typed | Suggestions |
| --- | --- |
| `/pv ` | The vault numbers the player can open, `help`, and `reload` for administrators. |
| `/pv 1` | Numbers starting with `1`. |

## Messages

Every reply comes from `messages_<language>.yml`. To hide a message, leave it empty; see
[Languages & messages](../getting-started/languages.md).
