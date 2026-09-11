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

