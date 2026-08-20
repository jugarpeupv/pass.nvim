## Why

When an entry in the password store holds multiple lines (created or
updated with `pass insert -m`), pressing `<CR>` in the picker copies the
whole entry, including metadata lines such as usernames, URLs, or OTP
secrets. Most use cases only need the primary password on the first line.
We want `<CR>` to copy just that first line, while keeping the full entry
available on a separate key (`<C-b>`).

## What Changes

- `<CR>` (confirm) now copies only the first line of the selected entry,
  using the plugin's standard `setreg :+` clipboard path on the first line
  of `pass show` output.
- A new `<C-b>` action copies the full entry (all lines), preserving the
  current behavior of copying everything via the plugin's `setreg` path.
- The picker stays open after both `<CR>` and `<C-b>` so the user can
  select and copy further entries.
- **BREAKING** (behavior): `<CR>` no longer copies multiline content;
  users who want the full entry must use `<C-b>`.

## Capabilities

### New Capabilities
- `picker-password-copy`: Copying the first line, or the full content, of a
  password entry to the clipboard from the picker.

### Modified Capabilities
<!-- No existing specs/modules are being changed. -->

## Impact

- `fnl/pass/utils.fnl`: add a helper that returns the first line of an
  entry, reusing the existing `pass show` command.
- `fnl/pass/init.fnl`: change the confirm action to first-line copy, add a
  `copy-all` action and its `<C-b>` input mapping, and stop closing the
  picker after copies.
- `lua/pass/utils.lua` and `lua/pass/init.lua`: regenerated from Fennel.
- `README.md`: document the `<C-b>` mapping and the changed `<CR>` behavior.