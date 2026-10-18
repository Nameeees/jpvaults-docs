# Permissions

| Permission | Description | Default |
| --- | --- | --- |
| `jpvaults.use` | Allows using `/pv` and opening your own vaults. | `true` |
| `jpvaults.vaults.<n>` | The rank gets access to `n` vaults. The highest number the player has is used. | `false` |
| `jpvaults.reload` | Allows `/pv reload`. | OP |

## Vault count per rank

The plugin checks every number from `vaults.max-amount` down to 1 and grants the highest one the
player has, so a group with several nodes gets the best of them.

```text
/lp group default permission set jpvaults.vaults.1 true
/lp group vip     permission set jpvaults.vaults.3 true
/lp group mvp     permission set jpvaults.vaults.6 true
/lp group legend  permission set jpvaults.vaults.10 true
```

A player with `jpvaults.vaults.6` can open `/pv 1` to `/pv 6`; `/pv 7` answers with the locked
message.

Permission nodes with numbers above the ones listed in `plugin.yml` work too — the plugin only
looks at `vaults.max-amount`. Raise it (up to 100) if a rank should get more vaults:

```yaml
vaults:
  max-amount: 25
```

## Players without a rank

Players with no `jpvaults.vaults.<n>` permission get `vaults.default-amount` vaults (1 by default).
Set it to `0` to require a rank for everything:

```yaml
vaults:
  default-amount: 0
```

## Disabling the command

`jpvaults.use` is granted to everyone by default. Deny it to a group to close the command for them
(for example on a lobby server) without touching the rank nodes:

```text
/lp group lobby permission set jpvaults.use false
```

## LuckPerms tips

```text
# Check what a player actually has
/lp user <player> permission check jpvaults.vaults.6

# Show the inherited nodes
/lp user <player> permission info
```

Permissions are evaluated when the player runs the command, so a rank change applies immediately —
no relog and no `/pv reload` needed.
