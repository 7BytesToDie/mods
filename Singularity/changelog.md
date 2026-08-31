# Singularity 4.0.0

SINGULARITY REFACTOR RELEASE NOTES

OVERVIEW

This refactor represents a complete architectural overhaul of the Singularity framework. The primary focus is the introduction of the SGAI system, which replaces the previous ad-hoc group assist, target selection, and flee patches with a unified, role-based AI architecture. New systems for material overrides and entity scaling are also added. The new design prioritizes performance, consistency, and modder configurability while adding emergent behaviors such as group coordination, hunger-driven feeding, startle responses, and rage mechanics.

All previously existing features (ShaderlessFX, entity stun, non-lethal explosives, ammo sounds, screen effect overrides, server policies, advanced sky manager, and XML patch operations) remain functional with only naming convention changes. This document focuses exclusively on new or significantly reworked systems.

AI SYSTEM OVERHAUL (SGAI)

Core Data Model

The SGAI system introduces a persistent per-entity data container EData, stored in a ConditionalWeakTable keyed by EntityAlive. EData tracks the entity's group role (Solitary, Alpha, Follower), emotional state (Startled, Enraged), hunger level, digestion status, confidence scale, self-preservation flag, and references to its group and alpha.

Group Roles and Formation

Group roles are assigned through the new EAIGroupUpSG task. This task reads two parameters from its XML data: chance (0.0 to 1.0, probability of being social) and groupSize (maximum members, -1 for unlimited). On execution, the task either initializes the entity as solitary, attempts to join an existing alpha with available slots, or starts a new group as alpha. Group formation occurs once per entity and is not re-evaluated on every tick. The group size property determines how many slots the alpha has available; slots are consumed when followers join and are not freed on follower death, intentionally leading to multiple smaller groups rather than one large group.

GroupData structure stores the alpha as a WeakReference, member count, combined maximum health, slots left, shared target, shared target type set, last sense time, target lost start time, grace period (default 10 seconds), and frame-based sense detection flags. The group's member count squared is used in threat calculations.

Shared Target Coordination

A critical innovation is the group-wide shared target. When any group member detects a valid threat through EAISetTargetSG, the target is propagated to the group's SharedTarget property. Other members adopt this target via TryAdoptSharedTarget in both the target acquisition and attack tasks. Shared target persistence is governed by a grace period: if no group member reports sensing the target for the configured grace period, the group abandons it. Frame-based coordination ensures that detection status is evaluated once per frame, preventing redundant timer resets. This eliminates the need for every member to independently acquire and maintain the same target, reducing spatial queries and pathfinding requests.

Confidence and Threat Evaluation

The system replaces hard-coded fight/flee rules with a quantitative confidence model. Confidence is computed as: individual threat multiplied by confidence scale, multiplied by hunger factor (1.0 if hungry, 0.5 otherwise), multiplied by group multiplier. Individual threat for non-players is health multiplied by damage per second derived from the hand item's damage and delay. For players, individual threat is health multiplied by level (capped at 300). Group multiplier follows Lanchester's square law: member count squared. The confidence scale is a per-entity random range, configurable via the entity class property SG_AIConfidenceScale (min,max). Emotional states override confidence: enraged sets confidence to infinity, startled sets confidence to zero.

Target threat is computed similarly. For players in a party, group threat aggregates the party's average level and total health. For non-player groups, threat is the sum of individual threats multiplied by the group's member count squared. A torch threat multiplier (default 2) is applied if the target is a player holding an item tagged heldTorch, configurable via entity class property SG_AITorchThreatMultiplier. ShouldEngage returns true if the entity's confidence meets or exceeds the target's threat.

Emotional States - Startle and Rage

Two emotional states influence all AI decisions. Startle is triggered by loud noises (noisePlayerVolume above threshold) or heat damage (fire). The probability of startle is controlled by entity class property SG_AIStartleChance, and the duration by SG_AIStartleDuration (default 60 seconds). When startled, the entity flees, does not engage in combat, and cannot initiate new combat. Startle is suppressed if the entity is already enraged. Heat damage startle has a separate probability controlled by SG_AIHeatStartleChance (default 0.15).

Rage is triggered when health drops below 20 percent and a random roll succeeds against entity class property SG_AIRageChance. Rage can also be triggered by repeated stuck attempts during flee (cornered rage). Rage overrides startle, sets confidence to infinity, prevents fleeing, and enables aggressive destruction (via EAIDestroyAreaSG). Rage lasts for SG_AIRageDuration (default 120 seconds, or 8 seconds for cornered rage). A rage event plays the entity's distressed sound.

Unified Target Selection - EAISetTargetSG

This new task replaces both EAISetNearestEntityAsTarget and EAISetNearestCorpseAsTarget. It executes a single spatial query per evaluation cycle, evaluating living and corpse candidates inline. Priority scoring ranks candidates from 1 to 6: 6 = urgent flee (target is a threat but entity is not confident), 5 = cautious flee (nearby threat, not confident), 4 = engage (revenge, attacked, hostile player and confident), 3 = defend group member, 2 = hunt prey (confident), 1 = eat corpse (if hungry). The highest priority candidate is selected, with ties broken by distance.

Living targets are filtered by the class parameter, with optional hearDistMax and seeDistMax overrides. Corpse targets are filtered by corpseClass, corpseFlags, and cannibalismChance. The alwaysHungry flag forces the entity to treat corpses as targets regardless of actual hunger. The seeRequired flag enforces line-of-sight requirements. DangerDistance controls the distance at which stealth checks still apply even without direct line-of-sight.

The task also performs early evaluation of existing attack target, revenge target, and noise player before the world scan, allowing short-circuit exit when a valid high-priority target already exists. Noise players that are not immediately targeted may trigger investigation position updates with breadcrumb logic.

Approach and Attack - EAIApproachAndAttackTargetSG

This task inherits from the vanilla EAIApproachAndAttackTarget but adds group and emotional state awareness. CanExecute returns false if the entity is startled or if the target is a group member. If the entity has no personal target but belongs to a group with a valid shared target, it adopts that target. Continue checks for group membership conflicts and target validity, and attempts to adopt the shared target if the personal target is lost.

The update loop integrates ShouldEngage: if the target is lost, solitary entities abandon after a timeout, while group entities use the group's shared target grace period to determine abandonment. Attack range calculations use the hand item's range with adjustments. Pathing to the target respects the entity's movement speed and includes counter-based throttling with early checks against PathFinderThread activity. The attack execution handles group circle repositioning, mutual damaged target flag clearing, and attack timeout management.

Block Breaking and Area Destruction - SG Variants

EAIBreakBlockSG overrides CanExecute to conditionally allow block breaking based on confidence and hunger. The task returns true only if the entity is not confident against its attack target, or if it is hungry or enraged. EAIDestroyAreaSG executes only when the entity is enraged and not startled, preventing environmental destruction during flee or confusion states.

Flee Behavior - EAIRunawayFromEntitySG

This task replaces the patched vanilla flee with a comprehensive implementation. It supports filtering by entity flags (for valid flee targets) and safeFlags (for entities to ignore). Forced types, specified via the forced parameter, always trigger flee regardless of confidence. SafeDistance and DangerDistance are configurable; DangerDistance affects stealth check behavior at close range.

Startle integration: when noisePlayerVolume exceeds the hearThreshold and EData.TryStartle succeeds, the entity enters a startled state and flees for the configured duration. Stuck detection uses a 1-second window comparing actual movement against expected movement (speed multiplied by window time). If the ratio falls below 0.1, the entity is considered stuck. After up to 4 repath attempts, if stuck persists and rage chance is positive, the entity triggers cornered rage and stops fleeing. Otherwise, it attempts a random reposition.

The task integrates ShouldEngage: if confidence rises above the target's threat during flee, the entity stops fleeing and transitions to combat. It also respects group membership, never fleeing from group members. The fleeCorpses flag allows fleeing from dead bodies.

Runaway When Hurt - EAIRunawayWhenHurtSG

This task manages safe location recording and return-to-safety behavior. It is not derived from EAIRunAway; it implements its own state management. When health is above the healthPer threshold and the entity is not in danger (no active revenge target, no attack from non-group entity), it records the current position as a safe location. Updates occur only when cooldown (updateCooldown) has elapsed and the entity has moved more than updateDistance since the last recording.

When health drops below the threshold, the task paths the entity back to the last recorded safe location. If the safe location becomes invalid (e.g., block changed), it falls back to a random position. The task continues until the entity reaches the safe location or the location becomes invalid. It integrates with active flee tasks to avoid overriding them.

Hunger and Feeding

A complete hunger system is introduced. Each entity has Food and MaxFood values; MaxFood is half the entity's maximum health. Food is accumulated by eating corpses. FullUntil is stored as a world-time minute value, making hunger persistent across game sessions. The entity is hungry when the current world time exceeds FullUntil. DigestionDurationMinutes (default 240) controls how many minutes of world time pass before hunger resets.

EAIDigestSG is a passive task that runs every 10 seconds. If the entity is hungry and has food (Food equals MaxFood), it digests: Food resets to 0, FullUntil advances by digestion duration, and optionally heals the entity by the MaxFood amount (controlled by the heal property).

EAIFeedSG inherits from EAIApproachAndAttackTargetSG and targets corpses. It uses the same approach and pathing logic but adjusted for eating range (based on entity height). When in range, it plays eating animations, deals damage to the corpse (using HandItemDamage from the entity's hand item, default 35), spawns blood particle effects, and adds the damage amount to Food. It stops eating if a threat appears (revenge target, or group member being attacked). The task uses counter-based pathing and manual movement fallback.

Follow Alpha - EAIFollowAlphaSG

This dedicated task handles follower movement toward the alpha. It uses a configurable followDistance (default is half the hardcoded group radius, 25). The task employs hysteresis: it stops following when the distance drops below followDistance multiplied by 0.9, and resumes only when distance exceeds followDistance. This prevents rapid stop-start oscillation near the ideal distance.

Path recalculation occurs every 2 seconds, and only if the alpha has moved more than 5 units since the last path was set. The task continues only while the entity should follow (not solitary, not alpha, and no active shared target in the group). When stopped, the task's Continue method returns false, removing it from the execution loop.

Territorial - EAITerritorialSG

This task overrides EAITerritorial to restrict territorial roaming to solitary or alpha entities. Followers do not roam territorially; they remain with the group.

MATERIAL OVERRIDES

This is a completely new system. It allows per-entity-class material customization without code changes. Two XML properties are recognized on entity_class: SG_MaterialColor and SG_MaterialFloat. Each property requires a materialProperty attribute (the shader property name) and a value attribute (color string or float). The system parses these during EntityClassesFromXml.Load, stores them in MaterialOverrideManager.OverridesByClass, and generates all combinatorial variants.

For example, if an entity has two color overrides and one float override, six variants are generated (2x3). Variants are assigned deterministically to each entity using a hash mixer based on entityId. The same hash mixer is used for material variant selection, entity scale selection, and any other deterministic random needs. Cloned materials are cached in a dictionary keyed by (original material instance ID, variant hash) to avoid repeated instantiation and memory waste.

Material override application occurs during EntityAlive.PostInit. The system selects the variant for the entity's class, clones the shared material once per variant per original material, and applies all color and float overrides. The cloned material is reused for all entities with the same variant and original material.

ENTITY SCALING

This is also a completely new feature. A new MinEventActionRollEntityScale is provided for use in XML triggered effects. This action takes min and max attributes and sets the target entity's scale to a deterministic value between min and max, derived from the entity's ID using the same hash mixer. The scale is applied via OverrideSize and SetScale, ensuring consistency across clients and save/load cycles.

ENTITY CLASSES

All new entity classes now inherit from EntityEnemyAnimalSG, which overrides CanDamageEntity to prevent friendly fire between group members. The override checks whether the damage source is in the same group using IsInSameGroupAs; if so, it returns false, preventing damage entirely. This is a hard protection at the entity level, not an AI task filter.

New classes include: EntityAnimalChickenSG, EntityAnimalBearSG, EntityAnimalBoarSG, EntityAnimalMountainLionSG, EntityAnimalWolfSG, EntityAnimalCoyoteSG, EntityAnimalSupernaturalSG, EntityAnimalZombieBearSG, EntityAnimalDireWolfSG, EntityAnimalBossGraceSG, EntityZombieSmartSG, EntityZombieScreamerSG.

These classes are registered in EntityFactory.GetEntityType and EntityClassesFromXml.Load for assembly-qualified name resolution, preventing log spam from slow type lookups.

PERFORMANCE IMPROVEMENTS

Single-pass scanning: EAISetTargetSG issues one spatial query per cycle instead of separate living and corpse queries, halving the number of GetEntitiesAround calls.

Pathfinding throttling: All path requests check PathFinderThread.Instance.IsCalculatingPath before queuing, preventing contention on the global pathfinding thread. Follow tasks use distance thresholds and repath intervals to reduce requests. Attack tasks use counter-based throttling with early exit when CanNavigatePath returns false.

Task scheduling: EAIGroupUpSG has an 8-second execution delay, reducing group validation frequency. EAIFollowAlphaSG stops executing entirely when the follower is within the stop distance. EAIApproachAndAttackTargetSG uses emotional state early exits to skip heavy logic.

Memory allocation: All spatial queries use a single static list cleared after use. No intermediate collections are allocated during target evaluation. ConditionalWeakTable usage is limited to data storage, with direct mutation of existing objects.

Short-circuiting: CanExecute methods in attack, target, and break tasks check IsStartled and IsEnraged first, bypassing confidence calculations, spatial queries, and group checks for entities in strong emotional states.

Stuck detection: Uses a movement ratio threshold (0.1 over 1 second) instead of raw positional checks, reducing false positives and unnecessary repaths.

Batch digestion: EAIDigestSG executes digest checks every 10 seconds rather than per tick, amortizing the cost of hunger updates.

CONFIGURATION PROPERTIES

Entity class properties (read from entity_class XML):
SG_AIConfidenceScale - Vector2 (min,max) for random confidence scaling.
SG_AISelfPreservation - bool, defaults true for animals, false for zombies.
SG_AIStartleChance - float 0-1, probability of startle from noise.
SG_AIStartleDuration - seconds, default 60.
SG_AIHeatStartleChance - float 0-1, probability of startle from heat damage, default 0.15.
SG_AIRageChance - float 0-1, probability of rage on low health, default 0.
SG_AIRageDuration - seconds, default 120.
SG_AITorchThreatMultiplier - float, default 2.

XML task data parameters (used in AITask/AITarget property data strings):
EAIGroupUpSG: chance (float 0-1), groupSize (int, -1 unlimited).
EAIFollowAlphaSG: followDistance (float).
EAISetTargetSG: class (with optional hear/see overrides), corpseClass, corpseFlags, cannibalismChance, alwaysHungry, seeRequired, dangerDistance.
EAIDigestSG: heal (bool), digestDuration (int minutes).
EAIRunawayFromEntitySG: flags, safeFlags, forced (comma-separated types), safeDistance, dangerDistance, hearThreshold, hearDistance, fleeCorpses.

Material override properties (on entity_class):
SG_MaterialColor - requires materialProperty and value (color string).
SG_MaterialFloat - requires materialProperty and value (float).

New MinEvent action:
MinEventActionRollEntityScale - attributes min and max (floats).

DEPRECATED AND REMOVED FEATURES

The following previous features are now fully replaced and should be removed from mod configuration:
- The old Gregariousness system (Singularity_Gregariousness entity property and Singularity_GroupSize entity property) is replaced by the chance and groupSize parameters on EAIGroupUpSG.
- The SetAsTargetIfHurt bypass transpiler is removed; revenge targeting is now handled by the unified target scoring.
- The RunawayFromEntity no-implicit-player patch is superseded by the new flee task's flag and forced type system.
- The SetNearestCorpseAsTarget class filter and cannibalism patches are replaced by the unified corpse targeting in EAISetTargetSG.
- The assist-related SetAttackTarget patch is replaced by group shared target and defense priority (priority 3) in target scoring.

All old feature switches (e.g., EntityGregariousness, SetAsTargetIfHurt_BypassSameTypeCheck, RunawayFromEntity_NoImplicitPlayer, SetNearestCorpseAsTarget_Filters) are now obsolete and have no effect.

BREAKING CHANGES

Entity class names have changed to include the SG suffix. Mods that reference these classes in entity_class Class properties must update to the new names.

The old Singularity_Stunned CVar has been renamed to SG_CVarStunned.

The old Singularity_Sound_start and Singularity_Sound_loop ammo properties have been renamed to Sound_startSG and Sound_loopSG.

The old Singularity_IsNonlethalExplosive property has been renamed to SG_XFXIsNonlethalExplosive.

The old ShaderlessFX effect names Singularity_Snapshot and Singularity_Overlay have been renamed to SG_VFXSnapshot and SG_VFXOverlay.

The SGAI system requires that entities using group behavior inherit from EntityEnemyAnimalSG or a derived class to enable friendly fire protection. Entities not inheriting from this class will not have the CanDamageEntity override and may damage group members.

MinEventActionModifyScreenEffect color attribute now only affects SG_VFXOverlay. I added this attribute specifically for the shaderless overlay. It has no effect on vanilla screen effects like Dark or Hot3.

The Config class's CheckFeatureStatus method now requires the class parameter for feature checks; the overload without class is deprecated.

MIGRATION NOTES

I am not going to write a full migration guide here. Read the updated blocks.xml file thoroughly. It contains all the new properties and task parameters documented in-place. Then take a look at how I updated Natural Selection to use these new systems for real-world examples. If you still have questions, you know how to reach me.

# Singularity 3.0.2

- Fixed assist radius sometimes applying incorrectly or using stale data
- Improved performance and memory usage of group AI scans
- Improved robustness of group tracking

# Singularity 3.0.1

- Improved compatibility with other mods

# Singularity 3.0.0

- 3.0 Compatibility
- Added: Override Trader Schedule switch

