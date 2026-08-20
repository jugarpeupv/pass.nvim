## Why

Many password entries in a `pass` store have an associated TOTP secret
managed via the `pass-otp` extension. Today, users must drop to a terminal
to run `pass otp <path>` to get a one-time code for an entry they just see
in the picker. We want a single `<C-o>` keypress in the picker to copy the
OTP code for the selected entry, matching the existing `<CR>` behavior for
passwords.

## What Changes

- Add a new "copy OTP" action to the snacks picker, triggered by `<C-o>`.
- The action reads the OTP code for the selected entry via the `pass` CLI
  (`pass otp`), mirroring how `M.copy` uses `pass show`.
- The code is placed on the system clipboard (`+` register) exactly like the
  existing `M.copy` action.
- The picker stays open after copying, so the user can also copy the
  password or run further actions.
- If the selected entry has no OTP secret, an error notification is shown
  and nothing is copied.

## Capabilities

### New Capabilities
- `picker-otp-copy`: Copying the OTP code of a password entry to the system
  clipboard from the picker.

### Modified Capabilities
<!-- No existing specs/modules are being changed. -->

## Impact

- `fnl/pass/init.fnl`: add an `otp-copy` action and the `<C-o>` input
  mapping in `M.open`.
- `fnl/pass/utils.fnl`: add an `otp` helper that runs the `pass otp`
  command, similar to the existing `M.show`.
- `lua/pass/init.lua` and `lua/pass/utils.lua`: regenerated from Fennel
  (the compiled `.lua` files are checked in).
- `README.md`: document the new `<C-o>` mapping.
- Relies on the `pass-otp` extension being installed (`pass otp` subcommand
  and its `oathtool`/`otptool` dependency).