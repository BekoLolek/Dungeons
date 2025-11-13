# Custom Mobs Guide

Custom mobs allow you to create unique enemies for your dungeons with custom equipment, stats, enchantments, and potion effects.

## Table of Contents
- [Creating Custom Mobs](#creating-custom-mobs)
- [Configuration Reference](#configuration-reference)
- [Using MythicMobs](#using-mythicmobs)
- [Examples](#examples)

## Creating Custom Mobs

### Method 1: YAML Configuration

Custom mobs are defined in your dungeon's YAML file under the `custom-mobs` section:

```yaml
custom-mobs:
  my_custom_mob:
    type: ZOMBIE                    # Entity type
    name: "&c&lPowerful Zombie"     # Display name (color codes supported)
    show-name: true                 # Show name above mob
    health: 100.0                   # Max health
    damage: 15.0                    # Attack damage
    speed: 0.30                     # Movement speed

    equipment:
      helmet:
        material: DIAMOND_HELMET
        drop-chance: 0.1            # 10% chance to drop
        enchantments:
          protection: 4
          unbreaking: 3

      chestplate:
        material: DIAMOND_CHESTPLATE
        drop-chance: 0.1
        enchantments:
          protection: 4
          thorns: 2

      leggings:
        material: DIAMOND_LEGGINGS
        drop-chance: 0.1

      boots:
        material: DIAMOND_BOOTS
        drop-chance: 0.1

      main-hand:
        material: DIAMOND_SWORD
        drop-chance: 0.2
        enchantments:
          sharpness: 5
          fire_aspect: 2

      off-hand:
        material: SHIELD
        drop-chance: 0.0

    potion-effects:
      speed:
        duration: 999999            # Permanent (in seconds)
        amplifier: 1                # Speed II
      strength:
        duration: 999999
        amplifier: 0                # Strength I
      regeneration:
        duration: 999999
        amplifier: 0                # Regeneration I
```

### Method 2: In-Game GUI Editor

1. Start editing a dungeon:
   ```
   /dng edit my_dungeon
   ```

2. Open the mob editor GUI (implementation varies by setup)

3. Use the visual interface to:
   - Set mob type and name
   - Configure health, damage, and speed
   - Place equipment directly in slots
   - Add/remove potion effects with click interface
   - Save changes instantly

## Configuration Reference

### Basic Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `type` | String | ZOMBIE | Entity type (ZOMBIE, SKELETON, etc.) |
| `name` | String | null | Custom display name (supports & color codes) |
| `show-name` | Boolean | true | Show name above mob |
| `health` | Double | -1 | Max health (-1 = default for mob type) |
| `damage` | Double | -1 | Attack damage (-1 = default) |
| `speed` | Double | -1 | Movement speed (-1 = default, normal ~0.25) |

### Equipment Slots

Each equipment slot supports:
- `material` - Item material (e.g., DIAMOND_SWORD)
- `drop-chance` - Probability to drop on death (0.0 to 1.0)
- `enchantments` - Map of enchantment names to levels

**Available Slots:**
- `helmet`
- `chestplate`
- `leggings`
- `boots`
- `main-hand`
- `off-hand`

### Enchantments

Use lowercase namespaced format for Minecraft 1.21.4:

| Enchantment Name | Effect |
|-----------------|--------|
| `protection` | Protection |
| `fire_protection` | Fire Protection |
| `blast_protection` | Blast Protection |
| `projectile_protection` | Projectile Protection |
| `thorns` | Thorns |
| `unbreaking` | Unbreaking |
| `mending` | Mending |
| `sharpness` | Sharpness |
| `smite` | Smite |
| `bane_of_arthropods` | Bane of Arthropods |
| `knockback` | Knockback |
| `fire_aspect` | Fire Aspect |
| `looting` | Looting |
| `power` | Power (bow) |
| `punch` | Punch (bow) |
| `flame` | Flame (bow) |
| `infinity` | Infinity (bow) |

*See [Minecraft Wiki](https://minecraft.wiki/w/Enchanting) for complete list*

### Potion Effects

Each potion effect has:
- **Duration** - Time in seconds (999999 = permanent)
- **Amplifier** - Effect level (0 = Level I, 1 = Level II, etc.)

**Common Effects:**

| Effect Name | Description |
|------------|-------------|
| `speed` | Movement speed boost |
| `slowness` | Movement speed reduction |
| `strength` | Melee damage boost |
| `weakness` | Melee damage reduction |
| `regeneration` | Health regeneration |
| `resistance` | Damage resistance |
| `fire_resistance` | Fire immunity |
| `water_breathing` | Underwater breathing |
| `invisibility` | Invisibility |
| `blindness` | Blindness |
| `night_vision` | Night vision |
| `hunger` | Hunger depletion |
| `poison` | Poison damage |
| `wither` | Wither effect |
| `health_boost` | Additional hearts |
| `absorption` | Absorption hearts |
| `glowing` | Glowing effect |
| `levitation` | Levitation |
| `slow_falling` | Slow falling |

## Using MythicMobs

If you have MythicMobs installed, you can reference MythicMobs mobs instead:

```yaml
custom-mobs:
  my_mythic_mob:
    mythic-mobs:
      enabled: true
      id: "SkeletalKnight"          # MythicMobs mob ID
```

When `mythic-mobs.enabled` is true:
- All other properties are ignored
- Mob spawns using MythicMobs system
- Inherits all MythicMobs configurations
- Supports MythicMobs skills and mechanics

## Examples

### Boss Mob

```yaml
custom-mobs:
  dungeon_boss:
    type: ZOMBIE
    name: "&4&l&n[BOSS] &cDungeon Lord"
    show-name: true
    health: 500.0
    damage: 20.0
    speed: 0.28

    equipment:
      helmet:
        material: NETHERITE_HELMET
        drop-chance: 0.5
        enchantments:
          protection: 5
          unbreaking: 3
          thorns: 3

      chestplate:
        material: NETHERITE_CHESTPLATE
        drop-chance: 0.5
        enchantments:
          protection: 5
          unbreaking: 3

      main-hand:
        material: NETHERITE_SWORD
        drop-chance: 0.8
        enchantments:
          sharpness: 6
          fire_aspect: 2
          looting: 3
          unbreaking: 3

    potion-effects:
      strength:
        duration: 999999
        amplifier: 2                # Strength III
      resistance:
        duration: 999999
        amplifier: 1                # Resistance II
      speed:
        duration: 999999
        amplifier: 0                # Speed I
      regeneration:
        duration: 999999
        amplifier: 1                # Regeneration II
```

### Ranged Attacker

```yaml
custom-mobs:
  elite_archer:
    type: SKELETON
    name: "&e&lElite Archer"
    show-name: true
    health: 40.0

    equipment:
      helmet:
        material: CHAINMAIL_HELMET
        drop-chance: 0.1
        enchantments:
          protection: 3

      main-hand:
        material: BOW
        drop-chance: 0.3
        enchantments:
          power: 4
          punch: 2
          flame: 1
          infinity: 1

      off-hand:
        material: ARROW
        drop-chance: 0.0

    potion-effects:
      invisibility:
        duration: 15                # 15 seconds
        amplifier: 0
```

### Tank Mob

```yaml
custom-mobs:
  iron_guardian:
    type: IRON_GOLEM
    name: "&7&lIron Guardian"
    show-name: true
    health: 200.0
    damage: 10.0
    speed: 0.20                     # Slow movement

    potion-effects:
      resistance:
        duration: 999999
        amplifier: 2                # Resistance III
      slowness:
        duration: 999999
        amplifier: 1                # Slowness II
```

### Speed Demon

```yaml
custom-mobs:
  shadow_runner:
    type: ZOMBIE
    name: "&8&lShadow Runner"
    show-name: true
    health: 30.0
    damage: 6.0
    speed: 0.40                     # Very fast

    equipment:
      helmet:
        material: LEATHER_HELMET
        drop-chance: 0.0

      boots:
        material: LEATHER_BOOTS
        drop-chance: 0.0
        enchantments:
          feather_falling: 4

    potion-effects:
      speed:
        duration: 999999
        amplifier: 3                # Speed IV
      jump_boost:
        duration: 999999
        amplifier: 2                # Jump Boost III
```

## Using Custom Mobs in Triggers

Once defined, reference custom mobs in trigger actions:

```yaml
triggers:
  boss_spawn:
    type: LOCATION
    condition:
      x: 50.0
      y: 5.0
      z: 50.0
      radius: 5.0
    actions:
      - type: SPAWN_MOB
        custom-mob-id: dungeon_boss  # Reference custom mob
        count: 1
        x: 50.0
        y: 5.0
        z: 50.0

      - type: MESSAGE
        message: "&4&lBOSS FIGHT! &cDefeat the Dungeon Lord!"
```

## Tips & Best Practices

1. **Balance is Key**
   - Don't make mobs too powerful for the dungeon difficulty
   - Consider player gear level when setting stats
   - Test with different party sizes

2. **Visual Identity**
   - Use consistent color schemes for mob types
   - Make bosses visually distinct with special armor
   - Use name formatting to indicate difficulty

3. **Drop Rates**
   - Keep drop chances low (0.0-0.2) for valuable items
   - Boss mobs can have higher drop rates (0.5-1.0)
   - Balance with reward tables to avoid over-rewarding

4. **Potion Effects**
   - Use permanent effects (999999 duration) for mob identity
   - Combine effects thoughtfully (speed + jump for parkour mobs)
   - Avoid effects that make mobs impossible (very high resistance)

5. **Performance**
   - Limit the number of permanent potion effects per mob
   - Avoid spawning too many heavily-equipped mobs at once
   - Use MythicMobs for very complex mob behaviors

6. **Equipment Enchantments**
   - Match enchantment levels to dungeon difficulty
   - Use themed enchantments (fire for lava dungeons)
   - Remember enchantment caps (Sharpness max: 5 in vanilla)

## Troubleshooting

**Mob not spawning?**
- Check entity type is valid (ZOMBIE, SKELETON, etc.)
- Verify custom-mob-id in trigger matches YAML definition
- Check console for loading errors

**Enchantments not working?**
- Use lowercase namespaced format (e.g., `sharpness` not `DAMAGE_ALL`)
- Check enchantment is compatible with item
- Verify enchantment names are spelled correctly

**Potion effects not applying?**
- Use lowercase effect names (e.g., `strength` not `INCREASE_DAMAGE`)
- Duration must be in seconds (multiply by 20 for ticks)
- Check console for invalid effect warnings

**MythicMobs integration not working?**
- Ensure MythicMobs plugin is installed and loaded
- Check MythicMobs mob ID exists in MythicMobs configs
- Verify `mythic-mobs.enabled: true` is set

## See Also

- [Triggers Guide](TRIGGERS.md) - Using custom mobs in triggers
- [Configuration Guide](CONFIGURATION.md) - Dungeon YAML structure
- [Editor Workflow](EDITOR_WORKFLOW.md) - Creating dungeons
