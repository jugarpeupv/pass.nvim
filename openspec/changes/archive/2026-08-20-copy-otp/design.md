## Context

`pass` calls pass-otp via the `otp` subcommand (`pass otp [...pass-name]`,
defaulting to `code`). The plugin already shells out to `pass` for every
operation (`show`, `insert`, `mv`, `rm`) through `fnl/pass/utils.fnl`, and
copy-to-clipboard is done with `vim.fn.setreg :+` in `M.copy`
(`fnl/pass/init.fnl`). The picker mappings and actions live in `M.open`.

## Goals / Non-Goals

**Goals:**
- Add a `<C-o>` mapping in the snacks picker that copies the OTP code of the
  selected entry to the `+` register.
- Reuse the existing `pass` invocation pattern and clipboard mechanism.

**Non-Goals:**
- Inserting/appending OTP secrets (`pass otp insert`/`append`) - out of scope.
- Auto-clearing the clipboard after `PASSWORD_STORE_CLIP_TIME`; the regular
  password copy does not do this either.
- Failing to copy when pass-otp/`oathtool` is not installed no more than the
  plugin already fails for other missing CLI tools.

## Decisions

**1. Invoke `pass otp <path>` (not `pass otp --clip`) and copy via `setreg`.**
`--clip` bakes in an xclip/wl-clip dependency and 45s clearing that the rest
of the plugin doesn't honor. Instead, run `pass otp <path>` to get the code
on stdout and copy it with `vim.fn.setreg :+`, identical to `M.copy`.
- Alternatives considered: `pass otp --clip --quiet` - rejected because it
  delegates clipboard handling to xclip/wl-clip and auto-clears, diverging
  from existing behavior and adding a platform dependency.

**2. Add `utils.otp` helper using the existing `run` pattern.**
Extend `fnl/pass/utils.fnl` with a `M.otp` function exactly like `M.show`:
run `[:pass :otp path]`, return cleaned stdout, or raise on non-zero exit.
This keeps the pass-otp interface isolated in utils.

**3. Graceful error reporting on entries without an OTP secret.**
`pass otp <path>` exits non-zero for entries lacking an `otpauth://` URI
("OTP secret not found"). `M.copy` calls `utils.show` unguarded, but OTP
entries will commonly not have a secret, so the action wraps `utils.otp` in
`pcall` and calls `utils.error` (which already notifies with the plugin
icon) when it fails, per the spec.
- Alternatives considered: pre-scan the entry for an `otpauth://` line and
  skip otherwise. Rejected - it duplicates pass-otp's own logic and leaks
  passfile parsing into the plugin.

**4. New picker action `otp-copy`, picker stays open.**
Add `M.otp-copy`, no auto-close (unlike `copy`/`log`, which close). It runs
`utils.otp` on the selected entry, copies on success, and notifies on
failure. Register it in `M.open` as `:<c-o> (tx :otp-copy {:mode [:i :n]})`
and in `:actions`. The action keeps the picker open per the spec.

## Risks / Trade-offs

- [Entries without OTP secrets are common] → Any failure is caught with
  `pcall` and surfaced as an error notification; nothing is copied.
- [pass-otp or oathtool not installed] → `pass otp` fails with a non-zero
  exit; surfaced as an error notification like other `pass` failures.
- [The code may expire before it is pasted] → same exposure as `pass otp`
  on the CLI; OTP codes are valid for the window regardless of when copied.