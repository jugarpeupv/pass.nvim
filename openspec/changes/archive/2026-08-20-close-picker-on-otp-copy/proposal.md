## Why

When the user presses `<C-o>` to copy an OTP code, the snacks picker
currently stays open. Closing the picker after copying the OTP makes the
action feel like a terminal action (copy-and-exit), matching how users
typically grab a one-time code and then move on to paste it elsewhere.

## What Changes

- Pressing `<C-o>` in the picker now closes the picker after the OTP code
  has been copied to the system clipboard.
- The picker still copies the OTP code successfully before closing; only
  the post-copy behavior changes.
- **BREAKING** (behavior): `<C-o>` no longer keeps the picker open; users
  who want to copy multiple OTP codes in one session must reopen the picker
  between copies.

## Capabilities

### New Capabilities
<!-- None -->

### Modified Capabilities
- `picker-otp-copy`: The "Copy OTP code with <C-o>" requirement changes so
  the picker closes after a successful OTP copy instead of staying open.

## Impact

- `fnl/pass/init.fnl`: `M.otp-copy` closes the picker after a successful
  copy, mirroring the auto-close wrapper used by the other terminal actions.
- `lua/pass/init.lua`: regenerated from Fennel.
- `README.md`: note the closing behavior in the `<C-o>` row (if applicable).