## Context

The password picker already has `<C-f>` (copy first line), `<C-b>`
(copy all), and `<C-o>` (copy OTP). Each follows the same pattern:
read entry content via `utils.show(path)`, parse it, copy to `+`
register, notify. The new `<C-u>` copies the `username:` field value
using the same pattern.

## Goals / Non-Goals

**Goals:**
- Add `M.username-copy` function and `<C-u>` keybinding
- Reuse existing `utils.show(path)` — no new utility functions
- Keep the picker open after copying (like `<C-f>` and `<C-b>`)

**Non-Goals:**
- Copying other metadata fields (url, etc.) — can be added later as
  separate keybindings
- Parsing all key-value fields into a structured format — out of scope

## Decisions

### Parse approach: line-by-line prefix match

Scan each line for `^username:%s*(.+)` and capture the group. Take
the first match. This is simple, consistent with how `copy-first-line`
extracts the password, and handles both `username:value` and
`username: value` formats.

**Alternative considered:** Use `vim.split` + `vim.tbl_filter` —
functionally equivalent but more verbose for no benefit.

### Keep picker open (no close)

Unlike `<C-o>` which closes the picker, `<C-u>` keeps it open. This
matches `<C-f>` and `<C-b>` behavior and lets users copy multiple
fields from different entries without reopening the picker.

### Multiple username lines: copy first, notify

If multiple `username:` lines exist, copy the first one and show an
info notification. This is a rare case (entries normally have one
username) but worth surfacing so users know the entry is unusual.

### No new dependencies

`utils.show(path)` already returns the multiline content. No changes
to `utils.fnl` are needed.

## Risks / Trade-offs

- **Low risk**: Single file change (`init.fnl`), no new dependencies,
  no architectural changes.
- **Edge case**: If `pass show` output has no `username:` line, we
  error and leave the clipboard unchanged — same pattern as otp-copy.
