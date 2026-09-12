# ByteblazarsRWGTweaks 2.1.0

- 3.0 compatibility
- Improved Prevent Empty Lots logic
- Improved logging
- Updated icon

# ByteblazarsRWGTweaks 2.0.0

- Switched to SemVer.
- New feature: **Prevent Empty Lots** – When no unique POI remains for a settlement, allows duplicates to fill the lot instead of leaving an empty space. Disabled by default (like everything else), but highly recommended instead of the other exceptions.
- New feature: **Alternative Priorities (Experimental)** – POIs now prioritize biomes matching their difficulty tier. Uses runtime heuristics to avoid breaking settlements or skipping POIs it shouldn't. Replaces the old "Cap POI by biome" option. This is much safer and should be compatible with everything.
- Fixed: Static data is now cleared after world generation finishes instead of at the start, reducing the memory footprint.
- Fixed: Trader counting – Now case-insensitive (e.g., `Trader_Bob` and `trader_bob` are treated as the same). Previously uppercase names could bypass per-biome trader limits.
- Removed: **Cap POI by biome** – Its functionality is fully replaced by the more effective "Alternative Priorities" system.
- Improved: Major refactoring with multiple small optimizations and improvements I refuse to list. Improved code readability too. Be grateful I bothered writing this changelog at all because I was honestly **this close** to say "F it, just try the update and leave me alone."

# ByteblazarsRWGTweaks 2.0.0-rc

- Switched to SemVer.
- Replaced the Cap POI by Biome option with a new Alternative Priorities option which uses more advanced heuristics, leading to vastly improved compatibility with other mods and better results overall. Upon generating a world, you can expect to see better POI variety, fewer low-tier POIs in the higher-tier biomes, and fewer or no POIs that are missing altogether. Settlements should no longer break either.
- Performance improvements.
- Fixed a few minor bugs, including one that caused the Max Traders per Biome option not to count traders with file names that included uppercase letters.

# ByteblazarsRWGTweaks 1.4.0.3

- Hotfix: Fixed NRE and trader features I hotbroke in the previous hotfixes because I was in a rush as usual. Thanks ALo for reporting and for the help testing!

# ByteblazarsRWGTweaks 1.4.0.2

- Hotfix: prevent Unique traders and Max traders from interfering with Cap POI tier by biome

# ByteblazarsRWGTweaks 1.4.0.1

- Hotfix: clearing count of variants between consecutive generations.

# ByteblazarsRWGTweaks 1.4.0.0

- Fully separated traders from the rest of the logic to make options more intuitive to use.
- Fixed Max Traders = 0. Now it truly prevents traders from spawning entirely without having to enable the destroyed variants.
- Added a Max Variants option to have more fine-grained control over those.
- Fixed other potential issues with traders in very specific scenarios I'm too tired to list.
- Refactored to simplify and streamline the code dealing with traders.
- Updated localizations.

# ByteblazarsRWGTweaks 1.3.0.0

- Added an experimental "Performance Mode" option that saves CPU cycles and speeds up random world generation by skipping vanilla logic that retries POI placement unnecessarily (especially when using this mod). Gains are situational and can range from marginal to massive.
- Added checks to prevent this mod from affecting server-initiated generations, as I've made up my mind not to support them. What this means in practice: if you want to share maps made using this mod, you have to do so manually. Otherwise, I would have to use a custom netpackage for this, and clients would still have to install this mod manually so there wouldn't be much of an advantage to it.
- Added a simple icon to the Mod menu entry.
- Improved logging.
- Updated localizations.
- Refactoring.
- Fixed regression: "Max traders per biome" not working as expected when set to 0 (I broke this in the 1.2.0.1 hotfix).
- Fixed temporary variables not fully resetting between consecutive generations.

# ByteblazarsRWGTweaks 1.2.0.1

- Hotfix: ERR KeyNotFoundException hard-locking the game when the new destroyed variants feature was disabled.

# ByteblazarsRWGTweaks 1.2.0.0

- Removed the "Bypass tag filters" feature. It is not needed anymore.
- Added a new "Traders" category and added two new features: "Max traders per biome" (set a maximum number of traders per biome) and "Use destroyed variants" (replaces trader POI duplicates with destroyed variants that are actually questable POIs - this requires ZZTong's Custom POIs). You can set the maximum number of traders per biome to zero to do a no-trader run.
- Moved the "Unique trader POIs" feature to the new "Traders" category.
- Improved "Cap POI tier by biome" feature: It now uses minimalist heuristics to determine if higher-tier POIs would normally be allowed to spawn anywhere else and ignores them if they wouldn't (otherwise they would end up not spawning at all). This is the reason "Bypass tag filters" is not needed anymore. This mostly covers wilderness POIs, but settlements from other mods can still end up broken, so keep that in mind.
- Added minimal logging.
- Renamed the "Danger Zone" section to "Experimental".
- Updated tooltips where needed.
- Minor optimizations.
- Fixed edge-case NullReferenceException (thanks again Fin!).
- Fixed "Cap POI tier by biome" not allowing POIs of difficulty 6 or above to spawn.
- Fixed temporary variables not getting flushed after server-initiated generations.
- Known issue: A few of the POIs by ZZTong spawned when "Use destroyed variants" is enabled can throw errors about INS files in the console when loading, but they are harmless as far as I can tell.

# ByteblazarsRWGTweaks 1.1.2.0

I take feedback seriously, especially if it comes from fellow modders I respect but accidentally pissed off, so I stayed up a couple of hours rushing this update, which:

- Makes all features disabled by default.
- Automatically detects mods this mod is known to break and forces the conflicting features in this mod off to prevent issues.
- Updates the GUI elements and descriptions in tooltips to better inform players of the risks of using those features so they can make more informed choices (if they read, that is).

I might make further improvements in the future, but for now this will have to do. Thanks Stallionsden for the help.

# ByteblazarsRWGTweaks 1.1.1.0

- Tied the Enabled state of the menu controls for allowing duplicates to the main toggle.
- Renamed "Disallow duplicates" to "Prevent duplicates" and improved tooltips with more information for clarity.

# ByteblazarsRWGTweaks 1.1.0.0

- Fixed settings not loading correctly.
- Reorganized settings tab
- Added an option to allow duplicates as long as they are in different biomes

# ByteblazarsRWGTweaks 1.0.0.0

- Initial release