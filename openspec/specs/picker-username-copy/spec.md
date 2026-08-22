## Purpose

Lets users copy the username value from a password entry's metadata
fields to the system clipboard directly from the picker without closing
it.

## Requirements

### Requirement: Copy username value with <C-s>
While the password picker is open, the system SHALL copy the value from
the first `username:` line of the selected entry to the system clipboard
(`+` register) when `<C-s>` is pressed.

#### Scenario: Username is copied for the selected entry
- **WHEN** the user presses `<C-s>` in the picker with an entry selected that has a `username:` line
- **THEN** the value after `username:` (trimmed of whitespace) is copied to the system clipboard

#### Scenario: Picker stays open after copying
- **WHEN** the user presses `<C-s>` in the picker with an entry selected that has a `username:` line
- **THEN** the picker stays open after the value is copied

### Requirement: Error notification when no username exists
If the selected entry has no `username:` line in its content, the system
SHALL NOT copy anything to the clipboard.

#### Scenario: Entry without a username line
- **WHEN** the user presses `<C-s>` in the picker with an entry selected that has no `username:` line
- **THEN** the system shows an error notification naming the entry
- **AND** nothing is copied to the clipboard

### Requirement: Notify when multiple username lines exist
If the selected entry has more than one `username:` line, the system
SHALL copy the value from the first one and show an info notification.

#### Scenario: Multiple username lines in entry
- **WHEN** the user presses `<C-s>` in the picker with an entry selected that has multiple `username:` lines
- **THEN** the value from the first `username:` line is copied to the clipboard
- **AND** an info notification indicates that multiple `username:` lines were found
