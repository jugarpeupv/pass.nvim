## Context

`M.open` builds the picker with `snacks-picker.pick` using the `select`
preset (narrow float, preview hidden) and a plain `"Password Store"`
title. Input keys are declared via the `tx` macro with only `:mode`, so
snacks' built-in `?` help (bound by default to `toggle_help_input` /
`toggle_help_list`) has no descriptions to show. See proposal.md for
motivation.

## Goals / Non-Goals

**Goals:**
- Compact legend in the picker title covering the copy family plus a
  `?` pointer.
- A short description on every custom picker key so `?` lists the full
  legend.
- Keep the `select` preset untouched.

**Non-Goals:**
- Custom layout or footer box (rejected as over-engineering).
- Changing any picker action behavior.
- README changes (key table already exists there).

## Decisions

### Title legend: full key list per user choice

Title becomes `Password Store (<CR> password, <C-x> delete, <C-e>
edit, <C-i> insert, <C-l> log, <C-s> username, <C-o> otp)`. The initial
proposal was a copy-family shortlist plus a `?` pointer to avoid
truncation on the narrow `select` float; the user chose the full list
instead and will verify fit by testing in Neovim.

**Alternative considered:** copy-family shortlist (`<CR>`, `<C-b>`,
`<C-o>`, `<C-s>`, `? more`) — set aside in favor of the explicit full
legend. Note `<C-b>` (copy all) is covered by the `?` help popup and
the README rather than the title.

### Descriptions via `:desc` on each `tx` entry

Add `:desc` to each key's props table (e.g. `(tx :rename {:mode [:i
:n] :desc "Rename entry"})`). Snacks renders these in its built-in
help popup; no custom help action needed.

**Alternative considered:** a custom help action rendering our own
legend — rejected, it reinvents the built-in popup.

### Keep snacks' default `?` binding

`?` is already bound by snacks defaults; we add nothing and override
nothing.

## Risks / Trade-offs

- **Title clipping on very narrow windows**: the legend is a suffix, so
  extreme widths may clip it. Mitigation: legend kept short; the full
  list is always one `?` away.
- **Otherwise low risk**: string-only changes in `M.open`, no new
  dependencies, no behavior change.
