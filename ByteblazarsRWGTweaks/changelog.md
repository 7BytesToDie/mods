# ByteblazarsRWGTweaks 4.0.0

Migration from 3.0.0:
- Every setting has moved to a new tab and category path.
- The Main tab is renamed to POIs and the Experimental tab to Advanced. The Traders tab keeps its name but its single Traders category is split into Spawn limits, Duplicates and Spacing.
- The one-line generic tooltip that every tab and section used is gone; each tab and section now has its own description.
- Prevent empty lots is replaced by Settlement duplicates (Strict / Only as needed / Always). Set it to Always to keep the previous on behavior. Only as needed, the new middle option, allows duplicates only when the town is running low on unique buildings; the Tight supply threshold slider controls when that kicks in.
- Max traders per biome is replaced by five per-biome selectors: Forest, Burnt forest, Desert, Snow and Wasteland trader limit. To keep the previous value, set all five to the same number.
- Use destroyed variants and Max variants are merged into one Duplicate trader variants selector (Disabled, 1-5, Unlimited). Pick the cap you want, or Unlimited for no cap.
- Allow up to this tier is a selector (Off, Tier0 through Tier4) instead of a numeric slider. The range is the same.

POIs tab:
- New feature: Town POIs now has five per-biome selectors (Forest, Burnt Forest, Desert, Snow, Wasteland) that cap how many non-trader buildings each town block can have in that biome. Each selector is [Unlimited, 1, 2, 3, 4, 5]. Unlimited matches vanilla. Traders do not count and always appear.
- New feature: Wilderness POI count now has five per-biome multiplier sliders (range 10-400) over the count rwgmixer.xml sets. 100 leaves rwgmixer.xml unchanged.
- New feature: Vanilla rules for mod prefabs. Exempts prefabs added by other mods from Prevent duplicate POIs. Useful when a mod adds a small number of prefabs that would otherwise be left under-placed.
- Improved: Alternative Priorities runs a second wilderness pass, so tier-matched prefabs that could not find a slot in their home biome still get a chance on leftover tiles.
- Fixed: With Prevent duplicate POIs off, vanilla's wilderness retry path runs normally. Before, the mod disabled that path unconditionally, so RWG ran slightly differently from vanilla even with every setting off.

Traders tab:
- New feature: Trader source selector (All, Vanilla only, Mods only, None). None disables every trader, including mod-added ones.
- New feature: Trader spawn location selector (Both, Towns only, Wilderness only).
- New feature: five per-biome trader limit selectors (Forest, Burnt forest, Desert, Snow, Wasteland trader limit) limit how many traders may spawn in each biome. Each selector is [Unlimited, None, 1, 2, 3, 4, 5]. None blocks new traders in that biome.
- New feature: Trader theme repeat distance slider. Overrides every trader prefab's own spacing value. Applies to the next generation when changed mid-session.
- New feature: Minimum trader spacing slider. Enforces a minimum distance between any two traders regardless of type.
- New feature: Config/trader-duplicate-whitelist.txt. Trader prefabs listed there always bypass Unique trader POIs and the per-biome limits.
- Fixed: Variant traders now count toward Minimum trader spacing, so a base trader and its destroyed variant no longer land next to each other.

World tab:
- New feature: Allow 0% biome weight. Lifts the 5% floor on the world generation screen so a biome can be excluded entirely.
- New feature: Keep desert and snow apart. Reorders biome placement so at least one other biome sits between desert and snow. Applies to Line and Circle layouts; Circle 2 is treated as Circle for that generation because a doubled ring cannot always separate the two. Best-effort; if the buffer biome runs out of tiles first, the desert-snow adjacency count reports the shortfall in the log and in rwg_diagnostics.txt.
- New feature: POIs from removed biomes (Off, Anywhere, Wilderness only). Lets prefabs whose biome tags reference only turned-off biomes still appear elsewhere.
- New feature: Highway connection attempts slider. Controls how many nearby townships the highway planner tries to connect each town to. Applies to the next generation when changed mid-session.
- New feature: Radiation zone width slider. Widens the radiation ring around the world edge.
- New feature: six player spawn controls. Spawn point count (up to 32; vanilla places exactly 12 and lower values reject candidates, higher values add spawns after the first 12 using the filters on the same tab), Trader proximity (blocks), Biome restriction (Vanilla, Any, or a specific biome), Max spawn prefab difficulty (Tier0 through Tier5, Any; Tier 1 matches vanilla), Min distance between spawns, and Allow spawns in towns.
- Fixed: A zeroed forest biome weight now actually produces a forest-free world. Vanilla always filled leftover tiles with forest, so the weight change had no effect on its own.
- Fixed: A zeroed center biome no longer causes a seed collision in the biome generation pass; the layout is temporarily treated as Circle for that generation only.

Advanced tab:
- New feature: Allow towns in any biome. Lifts each township's rwgmixer.xml biome restriction, so a township type can appear in any biome that has room for a town.
- New feature: Log verbosity selector (Normal, Verbose, Detailed). Normal writes the startup config summary, prefab pool counts, spawn summary and second-pass summary; Verbose adds per-trader replacement logs, ZZTong variant checks and a theme saturation summary; Detailed adds a per-leftover-POI diagnostic and writes rwg_diagnostics.txt.
- New feature: rwg_diagnostics.txt (written at Log verbosity Detailed). Saved into the generated world folder: effective config, spawn points, prefab pool, used prefabs, per-district POI counts, functional traders per biome, and the desert-snow adjacency count. Useful for bug reports.
- New feature: bbrwgtest console command. Runs RWG scenarios from a JSON file. Not required for normal play.

# ByteblazarsRWGTweaks 3.0.0

- Gears API v3 compatibility: Updated the mod to target Gears API v3. Settings bindings now use the attribute-based API, replacing the manual tab and category lookups and the per-setting change handlers with [Setting], [SettingOnValueChanged], and [SettingOnSelectedChanged] attributes. ModSettings.xml was updated to the V2 schema. Gears is now mandatory, and GearsAPI.dll is no longer bundled, so it must be installed separately.
- Fixed: The trader rekt alt was spelled "trader_rekt" instead of "trader_rektx", so ReplaceTraderDupesWithVariants never replaced a Rekt dupe.
- Improved: Cleaned up the localization files. Removed leftover empty color tags from ten UI labels, trimmed trailing whitespace, and corrected wording in several translations.

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