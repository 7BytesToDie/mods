# AdvancedSkyManager 2.2.0

Rework Advanced Sky Manager to be load-order agnostic

The AdvancedSkyManager property class was declared inline inside the
SingularityConfig block, so any mod that also added properties to it
would collide with Singularity regardless of load order. The class is
now created via a conditional append (only if it does not already
exist), and its contents are added via prepend. Other mods can extend
the class freely without conflicting with Singularity.

# AdvancedSkyManager 2.1.0

- Added Override Trader Schedule switch

# AdvancedSkyManager 2.0.0

Advanced Sky Manager changes:
- New feature: The visual start and end times of Blood Moons are now configurable (see blocks.xml).
- Improved: Reworked trader opening/closing times. They can now be configured using a digital clock format like in vanilla, and they are auto-adjusted as needed when Realistic Noons is enabled (see blocks.xml). They are also more robust and fail gracefully if badly configured.
- Improved: Fixed typo in blocks.xml and reworded a little bit.
- Fixed: NullReferenceException when joining dedicated servers.
- Fixed: Time of trader close warning not updated correctly.

# AdvancedSkyManager 1.0.0

Initial release