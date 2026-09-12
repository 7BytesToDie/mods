# NaturalSelection 2.2.0

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

# NaturalSelection 2.1.3

- Improved compatibility with other mods

# NaturalSelection 2.1.2

- Fixed vulture behavior

# NaturalSelection 2.1.1

- Reordered priorities
- Fixed animals not fleeing from swarms of insects
- Removed vanilla Territorial from bears

# NaturalSelection 2.1.0

- Fixed chicken cannibalism rate (100 percent to 45 percent)
- Split entityclasses.xml
- Improved compatibility with Watchguards (removes the glow from those animals too)

# NaturalSelection 2.0.2

- Fixed slide-walking

# NaturalSelection 2.0.1

- Hotfixed XML encoding

# NaturalSelection 2.0.0

I may have a problem.

I started writing detailed release notes for the Singularity + Natural Selection rework and, when I was about halfway through, I stopped and looked at the character count. It was already over 8k characters in length.

After nearly two months of working like I'm trying to impress Peter Norvig himself, or at least Satoru Iwata, this thing became so sophisticated it's a pain in the ass to list all of the implications of my work beyond a generalization like "animals are now likely smarter than some of you out there; install the update and see for yourself." I've also fixed a bunch of edge cases and even some vanilla exploits, and I've optimized it in every way I could conceive, to the point it not only outperforms the previous version, it should at least in theory outperform vanilla now (I haven't profiled to confirm though, but if anyone does please let me know so I have an excuse to slap the performance optimization tag on Nexus, etc).

I guess I'll simply list some highlights and call it a day. I deserve a little break before I get back to working on my other mods, so half-assed release notes it is. Here we go...

Highlights (off the top of my head):
- Significantly more sophisticated AI. Some examples of what I mean: animals gauge enemy threat before deciding whether to fight or flee, using Lanchester's square law for groups of enemies. They naturally fear fire and just wielding a torch can sometimes cause some of them to flee because players seem much more threatening that way. Loud noises can startle them. They now react to attacks from enemies they can't detect and to traps, instead of ignoring them. Predators often attack other members of their own species (they're seen as competitors). They have a lightweight hunger system and their behavior changes based on their hunger. Digestion heals them up. The list goes on...
- Removed the weird glowing saran wrap effect from all animals (mountain lions, stags, coyotes, and bears), so they don't glow in the dark anymore. This could be considered a vanilla visual bug in the material definitions of some of the animals. I give credit to @Daemonjax for figuring out the specific property that needed tweaking and for sharing the info. I just made it automatic via C# so there's no need to hack `animals.bundle` directly or redistribute vanilla assets anymore.
- Vastly improved compatibility with everything in the known universe (except older versions of the game) because I switched from the old minimalist Harmony hacks to proper EAI tasks fully architected by myself.
- Animals run significantly faster and are more cautious overall, so hunting is not exactly easier now. On the other hand, you may run into half-eaten corpses every now and then, so scavenging is an option. I may need to fine-tune some parameters though, because I've had almost no time to actually play, as usual. I played Wordle though, for what it's worth.
- Animals now spawn with randomized size variation (85% to 115% of normal). Adds visual variety and makes each animal feel slightly unique. No synchronization issues because I made it deterministic, to avoid having to network it.
- They also have some degree of individuality in terms of behavior. Some are more courageous than others, even amongst the same species.

Like I said, lots of stuff. I'm so done... for now. Why do I do this to myself? I need sleep. Good night.

# NaturalSelection 1.3.1

- Fixed compatibility with sandbox option for respawn delays
- Removed vultures from previous night groups where needed (vultures are in their own groups in 3.0)
- Fixed rabbit and chicken spawn rates in the wasteland

# NaturalSelection 1.3.0

Fine-tuned entity groups using the binomial distribution to:
- Make spawns per area less uniform (more unpredictable/luck-based). You can walk around without coming across a single animal for hours and suddenly find yourself facing a pack of wolves or some larger surprise. Extreme events are incredibly unlikely though (just like in real life).
- Make hunting less trivial (closer to vanilla)
- Lower spawn rates of vultures (those flying turds weren't letting people drive in peace)

More significant changes in upcoming releases. Stay tuned.

# NaturalSelection 1.2.0

- Slightly tweaked spawn rates

# NaturalSelection 1.1.3

- Switched to SemVer.
- Removed `safeDistance` parameter from all animals so they use the default value (they keep a bit more distance now).
- Fixed: Grace no longer groups up with boars sometimes, and attacks them on sight instead.

# NaturalSelection 1.1.2.0

- Further adjusted detection ranges and chasing times of boars and snakes to make them more defensive/opportunistic
- Removed Stags and Does from the list of potential targets boars can try to hunt

# NaturalSelection 1.1.1.0

- Adjusted detection ranges and chaseTimeMax for all animals.

# NaturalSelection 1.1.0.0

IMPORTANT: **remove zzzzzzzIncreasedAnimalSpawning.** If you still want higher spawn rates, you can keep it, but you should update it to the most recent version. It is now an optional addon, so you must do this manually.

- Natural Selection is no longer merely an AI overhaul: it also tweaks animal entitygroups of each biome for balance and to increase animal diversity.
- Thanks to the new feature added to Singularity in 1.3.0.0, NS now applies reasonable limits to animal groups so they can't grow forever. The new system also makes it more likely for groups of different sizes to exist simultaneously.
- Tweaked animal spawn rates and chances of animals being gregarious to accomplish multiple things:
- animals that are typically solitary (like boars), can't so easily be found in groups now.
- the pine forest biome is a lot less challenging, better balanced.
- animals that can't easily be found in large groups in the pine forest may form larger groups in other biomes. For example: in the pine forest, wolves are mostly alone, or sometimes in packs of 2 or 3, but their packs can grow much more rapidly in the snow biome, so they can eventually get to a size of 7 or 8.
- if you clear an area of predators, it now stays cleared for at least a day, so you don't have to worry about predators showing up all the time (unless they happen to pass by as they move around the map).

# NaturalSelection 1.0.1.2

- Lowered the previously obscene spawn maximums to the point animals can still form groups over time, but without overpopulating areas very quickly, which was also causing unexpected behavior (bunny stacks?!!!). Hosts are encouraged to further tweak these values to their liking. (Thanks Yumi)
- Reminder: update Singularity to see gregarious animals form groups. Before 1.2.0.0 they didn't have that behavior.

# NaturalSelection 1.0.1.1

- Added additional spawning.xml definitions for when players use other mods that vastly increase zombie spawn rates/maximums, to balance out the population of animals.
- Split spawning.xml off the main folder, into a folder that loads last, and removed the conditional that was meant to ensure animal spawning changes from other mods took priority. If someone does NOT want the increased animal spawn rates/maximums from this mod for whatever reason, they can easily edit those files or delete that folder.
- Hotfix (I just woke up, bear with me)

# NaturalSelection 1.0.0.1

- Hotfix: removed one line I added for testing purposes.

# NaturalSelection 1.0.0.0

- Initial release