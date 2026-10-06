# ByteblazarsHostileArchitecture 1.3.0

- Improved: Updated for Gears v5 (GearsAPI v3). If you use Gears, the settings menu now requires Gears v5. Gears remains optional, and the mod works without it, otherwise this would be a major release (2.0.0 instead of 1.3.0).
- Improved: The preset selector now updates the settings it controls in real time, instead of waiting for the Apply button.
- Improved: Damage scaling now runs after other mods that patch the same method, so its result is no longer overwritten by a mod that loads earlier.
- Fixed: The hardness threshold curve start slider was saving to the wrong setting, so your change was discarded and the block damage curve used an out-of-range multiplier.
- Fixed: Preset changes are now committed to the sliders and switches they control, so saving the settings no longer writes their stale values to disk.
- Improved: Cleaned up the localization files: fixed a typo, removed trailing whitespace, and corrected punctuation and German register.
- Improved: Updated the mod banner.

# HostileArchitecture 1.2.0

- Updated icon, localizations and ModInfo
- Minor refactoring

# HostileArchitecture 1.1.0

- Added a config.json file for dedicated servers or folks that don't use Gears. When Gears is installed, it takes priority over the config.json file.
- Added a "hostilearchitecture reload" console command for reloading config.json, in case such users or server admins want to make changes without restarting.
- Updated localizations.

# HostileArchitecture 1.0.0

Initial release