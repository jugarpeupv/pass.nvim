## Context

The picker confirm action (`M.copy` in `fnl/pass/init.fnl`) currently runs
`utils.show` (`pass show <path>`) and copies the returned content with
`vim.fn.setreg :+`. `pass show` returns the whole entry (all lines). The
user wants `<CR>` to copy only the first line and `<C-b>` to keep the
current all-lines copy. Per explicit user choice: first-line copy must be
delegated to `pass show -c` (pass manages the clipboard and auto-clears),
and the picker must stay open after both actions.

## Goals / Non-Goals

**Goals:**
- `<CR>` copies the first line via `pass show -c`.
- Add `<C-b>` that copies the full entry via the existing `setreg` path.
- Keep the picker open after both copies.

**Non-Goals:**
- Emitting a separate notification with the copied secret.
- Copying an arbitrary line number (pass supports `-c<n>`, out of scope).
- Adding a user-facing option to toggle first-line vs full copy.

## Decisions

**1. First-line copy takes the first line of `pass show` and uses `setreg`.**
Run `pass show <path>` and copy only the first line via `vim.fn.setreg :+`,
matching the clipboard handling already used by the other copy paths in the
plugin (no extra clipboard-tool dependency, no `pass`-managed clipboard).
- Alternatives considered: `pass show -c <path>` (pass manages the
  clipboard and auto-clears after `PASSWORD_STORE_CLIP_TIME`). Rejected —
  user chose to keep the plugin's `setreg` approach for consistency with
  the rest of the plugin.

**2. Full copy reuses the existing `setreg` path.**
Extract the current copy logic into `M.copy-all` (keeps `pass show <path>`
+ `setreg :+`), mapped to `<C-b>`. No behavior change for this path.
- Alternatives considered: using `pass show -c"+"`-style existing flags;
  not applicable — no flag copies the full entry with auto-clear parity.

**3. Picker stays open for both actions.**
Remove the `auto-close-picker` wrapper from the copy actions and the
`confirm: :copy` closing behavior, so `<CR>` and `<C-b>` leave the picker
open. `M.copy` gains a `[picker entry]` signature (or a shared helper) to
keep the confirm action compatible when required.
- Alternatives considered: keep closing on `<CR>` only. Rejected — user
  chose "stay open" for both.

## Risks / Trade-offs

- [No auto-clear behavior for the first-line copy] → The plugin uses the
  same `setreg :+` path for all copies; the clipboard content persists like
  any other Vim copy.
- [First-line copy no longer provides the full multiline entry on `<CR>`]
  → Addressed by the new `<C-b>` action; called out as a breaking behavior
  change in the proposal.
- [Empty first line produces an empty copy] → `utils.copy-first-line`
  returns an empty string fallback; identical to base `pass` behavior for
  empty-first-line entries.