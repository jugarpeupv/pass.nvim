## Purpose

Makes the picker's keybindings discoverable inside the picker itself,
so users learn the available actions without leaving Neovim to read the
README.

## Requirements

### Requirement: Title shows key legend
When the password picker opens, the system SHALL include a key legend
in the picker title naming the main actions and their keys.

#### Scenario: Legend visible on open
- **WHEN** the user opens the password picker
- **THEN** the picker title names the keys `<CR>`, `<C-x>`, `<C-e>`,
  `<C-i>`, `<C-l>`, `<C-s>`, and `<C-o>` alongside their actions

### Requirement: `?` shows full keymap help
Every picker keymap SHALL carry a short description, so pressing `?` in
the picker shows the full list of keybindings with their actions via
the built-in help popup.

#### Scenario: Full legend on demand
- **WHEN** the user presses `?` in the picker
- **THEN** the help popup lists each picker keybinding alongside its
  action description
