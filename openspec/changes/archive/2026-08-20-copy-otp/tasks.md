## 1. Utils Support

- [x] 1.1 Add `M.otp` to `fnl/pass/utils.fnl` that runs `[:pass :otp path]` and returns cleaned stdout, raising on non-zero exit (mirrors `M.show` in fnl/pass/utils.fnl:105)

## 2. Picker Action

- [x] 2.1 Add `M.otp-copy` action to `fnl/pass/init.fnl` that runs `utils.otp` on the selected entry, copies the result to the `+` register on success, and shows an error notification (via `utils.error`) when it fails; the picker stays open

## 3. Picker Wiring

- [x] 3.1 Register `:<c-o>` input mapping (`(tx :otp-copy {:mode [:i :n]})`) in `M.open` in `fnl/pass/init.fnl`
- [x] 3.2 Add `:otp-copy M.otp-copy` to the `:actions` table in `M.open`

## 4. Build & Docs

- [x] 4.1 Recompile `fnl/pass/*.fnl` to the checked-in `lua/pass/*.lua` files via nfnl and verify the diff only reflects the new OTP code
- [x] 4.2 Add the `<C-o>` / "Copy OTP code to clipboard" row to the mapping table in `README.md`