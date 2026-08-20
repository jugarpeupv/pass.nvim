## 1. Picker Action

- [x] 1.1 In `M.otp-copy` (`fnl/pass/init.fnl`), call `picker:close` in the
      success branch after `add-otp` sets the `+` register (before the
      info notification), so the picker closes only when the copy succeeds

## 2. Build & Docs

- [x] 2.1 Recompile `fnl/pass/*.fnl` to the checked-in `lua/pass/*.lua` files
      via nfnl and verify the diff only reflects the OTP-copy close change
- [x] 2.2 Update `README.md`: note in the `<C-o>` row that the picker closes
      after copying the OTP code