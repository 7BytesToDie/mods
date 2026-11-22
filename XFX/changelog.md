# XFX 4.0.0

- New feature: XFX now requires Singularity 7.0.0 or later and posts a red chat warning on every load when it is missing or outdated.
- New feature: World settings (optional, need Gears, in the Mods tab when you load a save or start a new game): explosion injuries on or off, a separate chance for each explosion injury (stun, broken arm, broken leg, concussion, deafness), deafness duration, Flash Thunder self-deafness chance, enemy stun duration, melee knockdown of stunned enemies, concussion and deafness chance for players hit by stun weapons, and blood splatter from storm damage.
- New feature: Personal settings that each player picks for themselves (optional, need Gears, in the Mods menu, applied on the next world load): white or black stun flash, stun camera shake, drowning screen effect, burning screen effect, and the heartbeat while drowning or burning.
- New feature: Six loading screen tips.
- Improved: Deafness now uses Singularity's deafness effect. Its duration, icon and description are unchanged.
- Improved: The mod is now listed as XFX, with a new description and a link to its mod page.
- Improved: Reworded the ear muffs and Flash Thunder ammo descriptions, and every French text now addresses the player the same way.
- Improved: The loading screen tip is now translated into every supported language.
- Fixed: An explosion could only give one injury. Each injury now rolls separately at its own chance.
- Fixed: A stun grenade could not give a concussion and deafness at the same time.
- Fixed: Flash Thunder hits chose the full or partial flash from the victim's last stun grenade instead of where they were looking. The full flash now only applies when the victim faces the shooter.
- Fixed: After a stun grenade hit, critical hit effects could stop applying for good.
- Fixed: Players hit by a stun grenade or Flash Thunder were slowed far more than intended. They now move 90% slower and attack 50% slower.
- Fixed: Zombies and animals recovering from a stun were slowed far more than intended. They now move and attack at half speed.
- Fixed: Zombies and animals stunned more than once could stop being knocked down by melee hits.
- Fixed: The heartbeat kept playing long after a fire went out or you stopped drowning. It now stops right away.
- Fixed: Part of the Flash Thunder muzzle flash was switched off the moment it appeared.
- Fixed: God mode did not end stun grenade effects.
- Fixed: Loading screen tip titles.


# XFX 3.2.0

Add partial flashbang VFX and reduced shake for entities looking away

Previously, entities facing away from a flashbang received no visual
feedback at all, only the hearing damage and full-strength camera shake.
Now they get a short brightness flash (intensity .5, fading out over
.15s) and, if they also took hearing damage, a subtle snapshot and
overlay at reduced intensity to represent disorientation. Camera shake
for the look-away case is reduced to .05 speed and 120 amplitude,
roughly a third of the direct-hit shake. Full VFX and full shake still
apply when the entity is facing the blast.

The hearing-damage branch in the partial VFX checks for the deafness
buff, so a player protected by earmuffs only gets the brief brightness
and not the disorientation effect.

Updated localizations.

# XFX 3.1.0

Add directional flashbang gating and rework config load order

The flashbang's visual effect is now suppressed for entities that are
not facing the explosion, while ringing ears, deafness, and camera
shake apply as before. Implemented via a CVarCompare requirement on
_SG_ExplosionInFront, which Singularity's nonlethal explosive patch
sets on each entity in the blast radius. The stun grenade item opts
into this by setting SG_FacingConeDegrees to 60 (a roughly 120-degree
full cone).

blocks.xml reworked to be load-order agnostic. The SingularityConfig
block and its XMLExtensions and ScreenEffects property classes are now
created via conditional appends, and their contents are appended rather
than set, so another mod that also touches any of them will not collide
with XFX regardless of load order.

# XFX 3.0.0

- Updated to work with Singularity 4.0.0

# XFX 2.0.0

- 3.0 Compatibility

# XFX 1.1.3

- Switched to SemVer
- Improved XPath expressions to prevent all zombie projectiles from applying explosion effects and improve compatibility with other mods

# XFX 1.1.2.0

- Lowered the base duration of deafness from 5 minutes to 1
- Added a long version of deafness (10 minutes) that occurs when a player is deafened again while a deafness effect is already in place
- Fixed a minor issue with stun stacking
- Updated syntax of requirement groups
- Fixed after-stun slow effect making zeds skate for a few seconds

# XFX 1.1.1.0

- Added 2.5b18 compatibility

# XFX 1.1.0.1

- Hotfix: Deafness not getting removed correctly (oops)

# XFX 1.1.0.0

- Halved the duration of Deafness
- Slightly increased the duration of Flash Bang VFX
- Slightly increased the duration of non-player entity stun
- Slightly lowered speed penalty from Flash Bang stun for both players and non-players (actual stun unaffected)
- Made ringing ears sound loop and fadeout when the buff ends
- Fixed ringing ears sound stacking