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