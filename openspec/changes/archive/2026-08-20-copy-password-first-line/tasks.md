## 1. Utils Support

- [x] 1.1 Add `M.copy-first-line` to `fnl/pass/utils.fnl` that returns the first line of `pass show` output (mirrors `M.show`), raising on non-zero exit
- [x] 1.2 Rename/reuse the existing `M.show`-based full copy helper as needed so the all-lines `setreg` path still works (`M.show` is unchanged)

## 2. Picker Actions

- [x] 2.1 Update `M.copy` in `fnl/pass/init.fnl` to copy only the first line via `utils.copy-first-line` and notify "Copied password <path>"
- [x] 2.2 Add `M.copy-all` in `fnl/pass/init.fnl` that copies the full entry (`utils.show` + `setreg :+`) and notifies "Copied <path>"

## 3. Picker Wiring

- [x] 3.1 In `M.open`, keep `:confirm :copy` and add `:<c-b>` input mapping (`(tx :copy-all {:mode [:i :n]})`)
- [x] 3.2 Add `:copy-all M.copy-all` to the `:actions` table
- [x] 3.3 Remove the `auto-close-picker` wrapper from the `:copy` action so both `<CR>` and `<C-b>` keep the picker open

## 4. Build & Docs

- [x] 4.1 Recompile `fnl/pass/*.fnl` to the checked-in `lua/pass/*.lua` files via nfnl and verify the diff only reflects the copy changes
- [x] 4.2 Update `README.md`: change the `<CR>` row to "Copy first line to clipboard", add `<C-b>` / "Copy all lines to clipboard", and note the auto-clear behavior