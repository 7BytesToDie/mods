# ByteblazarsHostileArchitecture 1.4.0

1.4.0 Hostile Architecture changes:
- New feature: Enemies can lose their arms from returned damage. Returned damage accumulates on one arm, and crossing the "Lower Arm Threshold (%)" tears off the lower arm; after that, crossing the "Upper Arm Threshold (%)" tears off the upper arm on the same side. Which arm takes the damage is rolled on the server, so it will not always match the arm the attack animation shows. Both thresholds are in the Advanced tab, and the whole feature sits behind the new "Dismemberment" switch.
- New feature: "Facial Dismemberment". A returned-damage hit that would kill the enemy now has a chance to destroy parts of its face. The visible effect is partial destruction of the face rather than a clean decapitation. Controlled by the "Facial Dismemberment" switch and the "Facial Dismemberment Chance (%)" slider, and checked before the arm thresholds so a killing hit takes priority over an arm loss on the same hit.
- New feature: The Grounded preset, now the default. It is Newtonian with "Block Damage Factor" enabled, so strong enemies can damage strong blocks instead of being locked out of the hardest materials, and it does not reduce the returned damage itself the way Hollywood does. A fresh install now starts on Grounded. The bundled config.json keeps Custom as its preset but now uses the Grounded values for its own defaults, including "Max Returned Damage (%)" at 100 instead of 10.
- New feature: Statistics. The mod can count returned-damage events and totals, arm dismemberments per stage, and facial dismemberment rolls and successes. Print them with "hostilearchitecture stats" and clear them with "hostilearchitecture stats reset". The switch and the existing Logging switch now sit in a new "Diagnostics" category on the Main tab.
- Improved: The "Level of Realism" selector now shows the preset that matches the loaded configuration when the settings page opens, instead of a stale value, and the Statistics tooltip states how to print the counters.
- Fixed: The hardness curve no longer returns NaN or a saturated value when the curve start, midpoint and end are set to degenerate values. This is only reachable by hand-editing config.json, since the Gears sliders enforce an increasing order.

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