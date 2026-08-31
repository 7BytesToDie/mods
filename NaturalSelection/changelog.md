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

