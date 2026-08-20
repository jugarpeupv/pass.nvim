## MODIFIED Requirements

### Requirement: Copy OTP code with <C-o>
While the password picker is open, the system SHALL copy the OTP code for
the entry under the cursor to the system clipboard when `<C-o>` is pressed,
and then close the picker.

#### Scenario: Copy OTP code for the selected entry
- **WHEN** the user presses `<C-o>` in the picker with an entry selected that has an OTP secret
- **THEN** the OTP code for that entry is copied to the system clipboard (`+` register)

#### Scenario: Picker remains open after copying OTP
- **WHEN** the user presses `<C-o>` in the picker with an entry selected that has an OTP secret
- **THEN** the picker closes after the OTP code is copied to the system clipboard