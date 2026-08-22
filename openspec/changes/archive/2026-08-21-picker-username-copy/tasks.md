## 1. Implementation

- [x] 1.1 Add `M.username-copy` function in `fnl/pass/init.fnl`: read
  entry content via `utils.show(path)`, find first line matching
  `^username:%s*(.+)`, copy captured value to `+` register, show info
  notification. If no match, show error. If multiple matches, copy
  first and show info notification.
- [x] 1.2 Add `<C-u>` keybinding to picker config in `M.open` and
  register `username-copy` action in the `:actions` table

## 2. Verify

- [x] 2.1 Recompile Fennel to Lua via nfnl headless
- [x] 2.2 Run `openspec validate --all` to confirm specs pass
