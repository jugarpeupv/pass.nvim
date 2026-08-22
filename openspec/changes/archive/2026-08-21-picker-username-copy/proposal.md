## Why

Password entries often contain metadata fields like `username:`, `url:`,
etc. Users frequently need to paste their username into login forms but
currently must open the full entry to find it. A dedicated `<C-u>`
keybinding that copies just the username value streamlines this common
workflow.

## What Changes

- Add `<C-u>` keybinding to the snacks picker
- New `M.username-copy` function that reads the entry's multiline
  content, finds the first `username:` line, extracts the value after
  the colon, and copies it to the system clipboard (`+` register)
- The picker stays open after copying (no close)
- Error notification if no `username:` line exists
- Info notification if multiple `username:` lines exist (first value is
  copied)

## Capabilities

### New Capabilities

- `picker-username-copy`: Copy the username value from a password
  entry's metadata to the clipboard via a `<C-u>` keybinding in the
  picker

### Modified Capabilities

- None

## Impact

- `fnl/pass/init.fnl`: new `M.username-copy` function, new `<C-u>`
  keybinding in picker config
- `fnl/pass/utils.fnl`: no changes needed (existing `show` function
  already returns multiline content)
- `openspec/specs/picker-username-copy/spec.md`: new spec created from
  this change
