## 1. Implementation

- [x] 1.1 Update picker `:title` in `M.open`
  (`fnl/pass/init.fnl`) to include the full key legend.
- [x] 1.2 Add `:desc` to every custom input key spec in `M.open` so the
  `?` help popup lists all nine keybindings with their actions.

## 2. Verify

- [x] 2.1 Recompile Fennel to Lua via nfnl headless.
- [x] 2.2 Run `openspec validate --all` to confirm specs pass.
- [x] 2.3 Hand over to the user for in-Neovim testing of the title and
  `?` popup. (User tested and approved.)
