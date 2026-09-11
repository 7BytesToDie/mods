# zzzByteblazarsXFX 3.2.0

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

# zzzByteblazarsXFX 3.1.0

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

