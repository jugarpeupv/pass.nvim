## Context

`M.otp-copy` (`fnl/pass/init.fnl`) copies the OTP code for the entry under
the picker cursor to the `+` register and leaves the picker open. The picker
actions that terminate the interaction use an `auto-close-picker` wrapper
(see `M.log`), which closes the picker then invokes the action. See proposal.md
for motivation.

## Goals / Non-Goals

**Goals:**
- Close the snacks picker immediately after a successful OTP copy.
- Keep the copy behavior unchanged (OTP code on the `+` register) and keep
  the error path (no OTP secret) visual feedback as-is.

**Non-Goals:**
- Changing how OTP codes are fetched (`pass otp <path>`).
- Closing the picker for the other copy actions (`:copy`, `:copy-all`).

## Decisions

**1. Close within `M.otp-copy` after a successful copy.**
On the success branch, call `picker:close` before notifying, so the picker
closes only when the copy succeeded. The error branch (no OTP secret) leaves
the picker open so the user can see the message and choose another entry.
- Alternatives considered: wrap `M.otp-copy` in `auto-close-picker`. Rejected —
  the wrapper closes the picker unconditionally before the action runs, which
  would also close it on the no-OTP error path.

## Risks / Trade-offs

- [Picker closes even if the user wanted to copy more codes] → Intentional
  behavior change; user requested close-after-copy. Reopen with `:Pass`.