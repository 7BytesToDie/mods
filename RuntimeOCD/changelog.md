# RuntimeOCD 0.15.2

- Removed leftover debug messages

# RuntimeOCD 0.15.1

- Fixed long-standing AudioMixer Heap race condition that occasionally got states stuck in multiplayer
- Refactoring

# RuntimeOCD 0.15.0.1

- Fixed Audio Mixer states getting stuck sometimes, and made the Audio Mixer heap more robust overall.

# RuntimeOCD 0.15.0.0

- More consistent conflict logging
- Replaced the ID builder for heap features so it's more reliable

# RuntimeOCD 0.14.0.0

- Fix: The Audio Mixer heap feature now correctly handles the case of sources turning off AM states without first turning them on (that means unconditionally turning them off)
- New feature: Explosion Buffs Merger. Similar to the BuffsOnWalkedOn merger, except for buffs applied by explosives. Configurable (On by default)
- Refactoring

# RuntimeOCD 0.13.0.0

- Fixed occasional *IndexOutOfRangeException* caused by mishandled indexing of hardcoded ScreenEffects (oops).
- Refactored the ConflictDetector for more robustness and added a try-catch that logs an exception but doesn't rethrow, to prevent non-essential code (the conflict logger) from interrupting the normal loading of XML files in the rare scenario of RuntimeOCD clashing with other mods.

# RuntimeOCD 0.12.1.1

- Fixed and improved reflection helpers to amend 0.12.1.0 and hopefully this time really really improve compatibility with other mods.

# RuntimeOCD 0.12.1.0

- Added safeguards when building MinEventParams ID strings to hopefully improve compatibility with other mods and versions of the game.

# RuntimeOCD 0.12.0.0

- New feature: AudioMixer states from different sources are now applied as a heap, similar to the Screen Effects heap introduced in 0.11.0.0. This will automagically improve compatibility between mods that trigger AudioMixer transitions (stunned, deafened).
- Added configuration option in settings.json to control the new AudioMixer heap.
- Fixed conflict detector logging the summary more than once per session (after loading multiple times).
- Fixed some edge-case errors and race conditions.
- Minor optimizations.

# RuntimeOCD 0.11.0.0

- Fixed BuffsWhenWalkedOn regression
- New feature: Screen Effects from different sources are now applied as a heap, where the ones with strongest intensity take priority over the weaker ones. This will automagically improve compatibility between mods that apply screen effects (such as blur, greyscale, you name it).
- Added "ScreenEffectsCompatibility" configuration option for the Screen Effects heap feature (enabled by default)

# RuntimeOCD 0.10.1.0

- Changed the color of the console message showing the path, for better visibility.
- The summary log entries no longer show the difference with previous results between brackets when previous results were not found.

# RuntimeOCD 0.10.0.0

- Overhauled logging: Conflicts are logged to files now, grouped into directories to ease searching. Once the host finishes loading into the game, a summary is printed to the console listing how many potential conflicts were detected in each category.
- Implemented configuration options. Load a savefile once for the files to be generated. The path is Roaming\7DaysToDie\RuntimeOCD\settings.json
- DetectConflicts (default true): Toggle conflict detection on/off. The Conflict Detector runs only once per game session, regardless of how many times a world is loaded.
- DetectConflictsOnlyWhenModsChanged (default true): If true, this new feature will allow the Conflict Detector to run only when the load order of mods has changed, or when mods were added, removed, or updated to a different version.
- MergeBuffsWhenWalkedOn (default true): Toggles merging BuffsWhenWalkedOn properties on/off.
- PreventChallengeCategoryCollisions (default true): Toggles the tiny hack that prevents the game from throwing an error when adding a category that already exists.
- Code refactoring and optimizations.

# RuntimeOCD 0.9.0.2

- Removed leftover Log.Warn() call previously used for debugging.
- Fixed regression from 0.9.0.1: BuffsWhenWalkedOn Merger logs a change when the attribute is first added to an element (which is not necessary)
- Refactored MergeChildren() and MergeSiblings() into a single method.

# RuntimeOCD 0.9.0.1

Fix multiple edge-case isues with the BuffsWhenWalkedOn merger

# RuntimeOCD 0.9.0.0

Initial release