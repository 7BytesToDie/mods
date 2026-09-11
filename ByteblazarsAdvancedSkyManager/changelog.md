# ByteblazarsAdvancedSkyManager 2.2.0

Rework Advanced Sky Manager to be load-order agnostic

The AdvancedSkyManager property class was declared inline inside the
SingularityConfig block, so any mod that also added properties to it
would collide with Singularity regardless of load order. The class is
now created via a conditional append (only if it does not already
exist), and its contents are added via prepend. Other mods can extend
the class freely without conflicting with Singularity.

