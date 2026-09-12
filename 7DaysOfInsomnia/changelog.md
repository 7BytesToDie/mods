# 7DaysOfInsomnia 2.1.0

- New incomplete installation warning. If only one of the two required folders is present, the player gets a red chat message on game load explaining which folders must be present and that the mod is not compatible with Vortex. This catches the most common way the mod ends up half-installed and misbehaving with no obvious symptom. The warning is implemented symmetrically in both folders, so it fires whether the missing folder is the early one or the late one. It also ships a Localization file in each folder so the message is translated regardless of which folder remains.
- Localization cleanup across all supported languages. The most significant corrections were in Russian and Turkish, where the term for "sleep deprivation" had been mistranslated in a way that read as something unrelated, and in Polish, where a misspelling was corrected. Several smaller wording improvements and a few dropped or mistranslated lines were also fixed in Spanish, Japanese, and Korean.

# 7DaysOfInsomnia 2.0.1

- Re-added Localization.txt for backward compatibility.

# 7DaysOfInsomnia 2.0.0

- 3.0 Compatibility

# 7DaysOfInsomnia 1.6.4.0

- Improved compatibility with Olfactory Screamers

# 7DaysOfInsomnia 1.6.3.0

- Expanded ServerMaxPlayerCount menu option so it allows any number of players from 2 to 16 (not just 2, 4, 6, 8 or 16).

# 7DaysOfInsomnia 1.6.2.0

- Automatically wake characters up during surprise Bloodmoons (Blood Moon Chaos)

# 7DaysOfInsomnia 1.6.1.0

- Sleep heals Deafness (XFX)
- Deafness (XFX) makes it easier to fall asleep.

# 7DaysOfInsomnia 1.6.0.1

- XML Loader missing xpath function workaround (mental note: remember to roll this back in 2.2 because Alloc said he already patched it)

# 7DaysOfInsomnia 1.6.0.0

- Added compatibility with v2.0+

# 7DaysOfInsomnia 1.5.1.1

- Fix regression: diarrhea not waking up characters even when untreated.

# 7DaysOfInsomnia 1.5.1.0

- Diarrhea no longer wakes characters when their dysentery is being treated.
- Fixed snores not ending immediately when characters wake up.
- Rebalanced sleep healing primarily to make infection healing more relevant while keeping infections dangerous in the long term.
- Infections no longer increase the duration of sleep significantly more than other afflictions.
- Slightly reworded localizations to better express the intended meaning.

# 7DaysOfInsomnia 1.5.0.0

- The Time Skip Patch is now fully integrated into the main file.
- The amount of time to skip is now determined dynamically when loading a save file, and it depends on the maximum number of players (ServerMaxPlayerCount). Solo sessions work like they used to (X divided by 1 = X)
- The timescale (24-hour cycle/DayNightLength) is now determined dynamically when loading a save, which makes the old timescale patches obsolete.

# 7DaysOfInsomnia 1.4.1.0

- Added a small base hunger & thirst cost for sleeping
- Added a small cost for recovering HP and fatigue when sleeping

# 7DaysOfInsomnia 1.4.0.2

- Improved compatibility with other mods.

# 7DaysOfInsomnia 1.4.0.1

- Fixed colored bedrolls not triggering sleep
- Removed safety checks that were triggering yellow warnings in the console (will not be necessary in the future)
- Improved compatibility with other mods

# 7DaysOfInsomnia 1.4.0.0

- The Refreshed status is no longer permanently displayed on the HUD, and is only displayed when relevant instead
- The Alertness debuff no longer triggers by taking damage from passive sources (like debuffs), unless the character is under 10% max HP
- The duration of insomnia now scales with the total chance of insomnia, ranging roughly from 2 game hours at 0% chance to 36 game hours at 90% chance

# 7DaysOfInsomnia 1.3.1.1

- Fixes buffs getting applied in god mode

# 7DaysOfInsomnia 1.3.1.0

- Added previously missing translations for the sheep-counting
- Greatly reduced the wait (counting sheep) when insomnia is going to trigger anyway
- Improved description of the new Bedtime buff
- Changed the color of the icon of the Bedtime buff
- Icons for Bedtime, Sleep Deprivation, Severe Sleep Deprivation, and Insomnia chance increases now blink at the start
- Getting attacked during microsleep now immediately removes the black screen effect
- Display of the chance of insomnia now shows only up to one digit after the decimal point
- Fixed sedatives not triggering VFX before Sleep Deprivation kicks in

# 7DaysOfInsomnia 1.3.0.0

- Improved the reminder at 20 hours awake by adding a new 'Bedtime' status (orange bed icon) that is removed when the player gets full rest, or is otherwise replaced by Sleep Deprivation if the player stays up long enough (thanks KNcreepy98 for the suggestion). This new status makes it significantly easier to fall asleep during that timeframe
- Visual effect of the reminder now only triggers if the player is not under the effect of Stimulants, Coffee or Mega Crush
- Mega crush now prevents all visual effects of Sleep Deprivation except for hallucinations
- Coffee now prevents microsleep during Sleep Deprivation (thanks Darkikos2 for the suggestion)
- Reduced the maximum and average amount of time to wait to fall asleep
- Set a hard cap of 80 to 100 seconds (random) of maximum sleeping time per attempt even if the healing quota isn't fully used up, to prevent excessive sleeping and make things a bit more predictable
- Slightly tweaked falling asleep speed multipliers for the following statuses: Sedatives, Sleep Deprivation, Severe Sleep Deprivation
- Fixed Refreshed status ticking during sleep, which caused the sleeping timer to reset if the Refreshed status happened to end before the player finished sleeping

# 7DaysOfInsomnia 1.2.1.0-TSP

- Sleep reminder and Bedtime buff trigger after 16 hours awake instead of 20

# 7DaysOfInsomnia 1.2.0.0

- Refactored code to simplify the implementation of patches for alternative 24-hour cycle settings (yes, I don't want to have to update these ever - sue me)
- Added optional patches for alternative 24-hour cycle settings
- Updated the Time Skip patch to work with the new system
- Fixed a typo in Localizations.txt
- Added "Microsleep" to the list of effects in the description of Sleep Deprivation
- Improved the logic of Sleep Deprivation visual effects to fix a bug that could trigger when taking Stimulants right before the visual effects ended (thanks viking093 for reporting) and other potential similar problems

# 7DaysOfInsomnia 1.1.0.0

- Fixed chance of insomnia from alcohol applied multiple times on interrupted sleeping attempts
- Removed minor long-term chance of insomnia from alcohol and coffee to make insomnia more predictable
- Lowered short-term chance of insomnia from coffee from 10% to 6%
- Added missing short-term increase in chance of insomnia from drinking mega crush (8%)
- Random visual effects for sleepiness now start triggering at the same time the player gets the Sleep Deprivation debuff
- Changed the time window for Sleep Deprivation to kick in from 20-28 hours to 24-28 hours since the last time the player slept
- Added a toolbelt message after 20 hours awake reminding players to sleep soon, with a single microsleep effect triggering at the same time
- Added a buff that displays the current total chance of insomnia. It's only visible on the status interface, not the HUD.
- Added a short-duration buff that displays increases in the chance of insomnia so players can more easily tell what increases the chance and what does not
- Improved translations (or tried to)

# 7DaysOfInsomnia 1.0.1.0-TSP

- Reduced the duration of the Refreshed status to 8 hours for balance and to make schedules more manageable
- Increased the maximum healing from sleeping by 33% for balance
- Increased the injury healing rate values by 50% so sleeping while hurt does not take much longer than before the update

# 7DaysOfInsomnia 1.0.1.0

- Made stimulants and sedatives craftable at the chemistry table. Progression not touched for compatibility with other mods
- Replaced dummy sound for attracting zombies with snoring sounds (thanks Darkikos2 and josefdark for the suggestions)
- Fixed refreshed status getting removed after oversleeping
- Fixed minimum sleeping time not taking into account refreshed status
- Slightly tweaked auditory hallucinations

# 7DaysOfInsomnia 1.0.0.1

- Fixed error that was triggering a game-breaking ArgumentNullException

# 7DaysOfInsomnia 1.0.0.0-NSP

- Add optional patch to disable snoring (and its associated danger)

# 7DaysOfInsomnia 1.0.0.0b

- Initial release