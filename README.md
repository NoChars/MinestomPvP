# MinestomPvP

[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)
[![license](https://img.shields.io/github/license/Bloepiloepi/MinestomPvP.svg?style=flat-square)](LICENSE)

MinestomPvP is an extension for Minestom.
It tries to mimic vanilla (and pre-1.9) PvP as good as possible, while also focusing on customizability and usability.

But, MinestomPvP does not only provide PvP, it also provides everything around it (e.g., status effects and food).
You can easily choose which features you want to use.

**I am aware of an issue where projectile collision is not entirely accurate, because of the Minestom new block api. I will implement a fix for this at some point.**

## Table of Contents

- [Features](#features)
- [Future Plans](#plans)
- [Usage](#usage)
- [Integration](#integration)
- [Events](#events)
- [Customization](#customization)
- [Contributing](#contributing)
- [Credits](#credits)

## Features

Currently, most vanilla PvP features are supported.

- Attack cooldown
- Damage invulnerability
- Weapons
- Armor
- Shields (or sword blocking)
- Food
- Totems
- Bows and crossbows
- Fishing rods (only hooking entities or legacy knockback, not fishing)
- Other projectiles (potions, snowballs, eggs, ender pearls)
- All enchantments possible with the above features (this includes protection, sharpness, knockback, ...)

## Plans

- Lingering potions
- Fall damage
- Fireworks (for crossbows)

Also, projectiles are a little bit of a mess right now.
I might change that in the future, but I'm not sure, since I don't think there is a better way.
(As a user you will probably not notice this, they do work as intended.)

## Usage

You can get an `EventNode` with all PvP related events listening using `PvpExtension.events()`.
By adding this node as a child to any other node, you enable pvp in that scope.
Separated features of this extension are also available as static methods in `PvpExtension`.

### Legacy PvP

You can get the `EventNode` for legacy PvP using `PvpExtension.legacyEvents()`.
**Do not combine it with any non-legacy node, as this will cause issues.**

To disable attack cooldown for a player and set their attack damage to the legacy value, use `PvpExtension.setLegacyAttack(player, true)`.
To enable the cooldown again and set the attack damage to the new value, use `false` instead of `true`.

#### Knockback

A lot of servers like to customize their 1.8 knockback. It is also possible to do so with this extension. In `EntityKnockbackEvent`, you can set a `LegacyKnockbackSettings` object. It contains information about how the knockback is calculated. A builder is obtainable by using `LegacyKnockbackSettings.builder()`. For more information, check the [config of BukkitOldCombatMechanics](https://github.com/kernitus/BukkitOldCombatMechanics/blob/d222286fd84fe983fdbdff79699182837871ab9b/src/main/resources/config.yml#L279).

### Integration

To integrate this extension into your minestom server, you may have to tweak a little bit to make sure everything works correctly.

When applying damage to an entity, use `CustomDamageType` instead of `DamageType` (except if you use the default ones: `GRAVITY`, `ON_FIRE` and `VOID`).
If you have your own damage type, also extend `CustomDamageType` instead of `DamageType`.

Potions and milk buckets are considered food: the Minestom food events are also called for drinkable items.

### Events

This extension provides several events:

- `DamageBlockEvent`: cancellable, called when an entity blocks damage using a shield. This event can be used to set the remaining damage.
- `EntityKnockbackEvent`: cancellable, called when an entity gets knocked back by another entity. Gets called twice for weapons with the knockback enchantment (once for default damage knockback, once for the extra knockback). This event can be used to set the knockback strength.
- `FinalDamageEvent`: cancellable, called when the final damage calculation (including armor and effects) is completed. This event should be used instead of `EntityDamageEvent`, unless you want to detect how much damage was originally dealt.
- `LegacyKnockbackEvent`: cancellable, called when an entity gets knocked back by another entity using legacy pvp. Same applies as for `EntityKnockbackEvent`. This event can be used to change the knockback settings.
- `PickupArrowEvent`: cancellable, called when a player picks up an arrow.
- `PlayerExhaustEvent`: cancellable, called when a players exhaustion level changes.
- `PlayerSpectateEvent`: cancellable, called when a spectator tries to spectate an entity by attacking it.
- `ProjectileBlockHitEvent`: called when a projectile hits a block.
- `ProjectileEntityHitEvent`: cancellable, called when a projectile hits an entity.
- `TotemUseEvent`: cancellable, called when a totem prevents an entity from dying.

### Customization

It is possible to add your own features to this extension. For example, you can extend the current enchantment behavior by registering an enchantment using `CustomEnchantments`. This will provide you with a few methods for when the enchantment is used. It is also possible to do the same for potion effects using `CustomPotionEffects`, which will provide you with a few methods for when the effect is applied and removed.

You can use the class `Tool`, which contains all tools and their properties (not all properties are currently included, will change soon).
The same applies for `ToolMaterial` (wood, stone, ...) and `ArmorMaterial`.

For further customization, it is always possible to use events or, if really necessary, a mixin.

## Contributing

You can contribute in multiple ways.
If you have an issue or have a great idea, you can open an issue.
You may also open a new pull request if you have made something for this project and you think it will fit in well.

If anything does not integrate with your project, you can also open an issue (or submit a pull request).
I aim towards making this extension as usable as possible!

## Credits

Thanks to [kiipy](https://github.com/kiipy) for testing and finding bugs.

I used [BukkitOldCombatMechanics](https://github.com/kernitus/BukkitOldCombatMechanics) as a resource for recreating legacy pvp.
