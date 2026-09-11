## Why

The picker's keybindings are only documented in README.md, so users
inside the picker cannot discover them without leaving Neovim. Surfacing
hints in the picker itself removes that round-trip.

## What Changes

- The picker title carries a compact key legend (copy family plus a
  `?` pointer to the full help).
- Every picker keymap gets a short description so snacks' built-in `?`
  help popup lists the full legend on demand.
- No behavior change to any existing picker action.

## Capabilities

### New Capabilities

- `picker-keymap-hints`: in-picker discoverability of the picker
  keybindings via a title legend and `?` help descriptions.

### Modified Capabilities

- None (existing copy/edit requirements are unchanged; only their
  discoverability is added).

## Impact

- `fnl/pass/init.fnl`: `M.open` title and input key specs; recompiled
  `lua/pass/init.lua` via nfnl.
- No new dependencies, no snacks layout fork.
