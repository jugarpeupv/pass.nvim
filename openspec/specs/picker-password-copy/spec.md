## Purpose

Lets users copy either just the first line (the main password) or the full
content of a selected password-store entry to the clipboard from the
picker.

## Requirements

### Requirement: <CR> copies only the first line
While the password picker is open, the system SHALL copy only the first
line of the selected entry to the system clipboard when `<CR>` is pressed,
using the plugin's standard `setreg :+` clipboard path.

#### Scenario: First line is copied with <CR>
- **WHEN** the user presses `<CR>` in the picker with an entry selected
- **THEN** the first line of that entry is copied to the system clipboard
- **AND** the rest of the entry is not copied

### Requirement: <C-b> copies the full entry
While the password picker is open, the system SHALL copy the full content
(including any additional lines) of the selected entry to the clipboard
when `<C-b>` is pressed, preserving the existing all-lines copy behavior.

#### Scenario: Full entry is copied with <C-b>
- **WHEN** the user presses `<C-b>` in the picker with an entry selected
- **THEN** the full content of that entry is copied to the clipboard

### Requirement: Picker remains open after copying
After either `<CR>` or `<C-b>`, the system SHALL keep the picker open so
the user can continue selecting and copying entries.

#### Scenario: Picker stays open after copy
- **WHEN** the user presses `<CR>` or `<C-b>` in the picker
- **THEN** the picker stays open
- **AND** the copied content is available on the system clipboard