# Features

## Vaults that behave like chests

- `/pv <number>` opens that private vault; `/pv` or `/pv help` shows the help and how many vaults
  the player has.
- Configurable size from **9 to 54 slots** (always a multiple of 9). 54 is the biggest inventory
  Minecraft allows.
- Normal container behaviour: shift-click, hotbar number keys, drag and drop, double-click to
  collect a stack.
- Tab completion for vault numbers, `help` and `reload`.

## Rank-based access

The plugin grants the **highest** `jpvaults.vaults.<n>` permission the player has. There is no
economy involved:

```text
/lp group vip permission set jpvaults.vaults.3 true
/lp group mvp permission set jpvaults.vaults.6 true
```

Players without any of those nodes get `vaults.default-amount` (1 by default). Rank changes apply
the next time the player runs `/pv`, with no relog required.

## Saving you do not have to think about

- Vaults are saved **when they close**, when the player **quits** and when the **server stops**.
- Open vaults are also written periodically (`storage.autosave-seconds`, 300 seconds by default;
  `0` disables it).
- Data is read from disk when a vault opens and dropped when the player quits, so a restart always
  starts from the real file.
- The plugin keeps no state between sessions: no cache to corrupt, no stale inventory to empty twice.

## Built to protect items

- Closing a vault waits until the file is on disk before releasing the inventory.
- `Q` and `Ctrl+Q` are cancelled inside the vault GUI, with a deny sound.
- Clicks outside the window never drop the item held on the cursor.
- The off-hand swap key (`F`) is guarded on 1.9+, including the classic 1.16.x cursor desync.
- Every save is written to a `.tmp` file and moved over the real file (atomic write).
- A player file that cannot be read is copied to a timestamped `.bak` before it can be overwritten.

The details, including the known limits, are in [Data safety](data-safety.md).

## Messages and languages

- All texts live in `messages_<language>.yml`: prefix, inventory title, help and errors.
- Legacy `&` colours everywhere, plus `&#RRGGBB` hex colours on 1.16+.
- English and Spanish ship with the plugin; any other language is one file away.
- An incomplete translation falls back to English key by key.

See [Languages & messages](getting-started/languages.md).

## Interface sounds

Open, success and denied actions play a sound whose name is resolved for the running Minecraft
version, so the same jar sounds right on 1.8 and on the latest release. Volume, pitch and the whole
feature can be configured or disabled.

## Small footprint

- One jar, **no NMS**, no shaded libraries, no ProtocolLib, no database.
- No bStats, no update checker, no network access at all.
- Only stable Bukkit API is used; the few version-specific pieces (off-hand event, renamed
  click-type method, renamed sound constants, the 1.8 title limit) are detected at runtime.

## Compatibility across versions

The same file runs from 1.8.8 to the latest release. Compiling against the Spigot 1.8.8 API and
avoiding NMS is what makes that possible; the tested matrix is in
[Compatibility](compatibility.md).

