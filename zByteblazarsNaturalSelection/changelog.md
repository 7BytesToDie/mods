# zByteblazarsNaturalSelection 2.2.0

Add per-entity speed variation and expand wasteland night spawns

New entityclasses_speedVariation.xml applies MinEventActionRollEntitySpeed
to timid and enemy animals, giving each individual a deterministic pace
derived from its entityId. Zombie animals are unchanged by default; the
blocks for them are present but commented out for anyone who wants to
opt in. The file is wired into entityclasses.xml.

Wasteland night and dark spawn groups now include animalZombieBoar and
animalBossGrace at low weights. The weights for animalDireWolf and
animalZombieBear were reduced to make room.

blocks.xml reworked to be load-order agnostic. The SingularityConfig
block and its XMLExtensions property class are now created via
conditional appends, and their contents are appended rather than set,
so another mod that also touches either one will not collide with
Natural Selection regardless of load order.

