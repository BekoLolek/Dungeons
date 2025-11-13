# Dungeon Configuration Guide

## Overview

Each dungeon is defined in its own YAML file in the `plugins/Dungeons/dungeons/` folder. This allows for easy organization and modification of individual dungeons.

## File Structure

Each dungeon file should be named descriptively (e.g., `goblin_cave.yml`, `dragon_lair.yml`) and follow this format:

```yaml
# Unique identifier for this dungeon (required)
id: "unique_dungeon_id"

# Display name shown to players (supports color codes)
display-name: "&aGoblin Cave"

# Schematic file name (must exist in plugins/Dungeons/schematics/)
schematic-file: "goblin_cave.schem"

# Party size requirements
party-size:
  min: 2    # Minimum party members
  max: 4    # Maximum party members

# Time limit in seconds
time-limit: 1800  # 30 minutes

# Spawn point offset from schematic paste location
# This is where players will be teleported when entering
spawn-offset:
  x: 25.5   # X offset (can be decimal for centering)
  y: 1.0    # Y offset (usually 1-2 blocks above floor)
  z: 25.5   # Z offset (can be decimal for centering)

# Quest IDs that this dungeon uses (must exist in quests.yml)
quests:
  - "goblin_slayer"
  - "treasure_hunter"
  - "escape_route"

# Reward table IDs to use on completion (must exist in rewards.yml)
rewards:
  - "goblin_loot"
  - "common_rewards"

# Cooldown in seconds before players can re-run this dungeon
cooldown: 3600  # 1 hour

# (Optional) Difficulty level for display
difficulty: "MEDIUM"

# (Optional) Entry cost in money (requires Vault)
entry-cost: 100.0

# (Optional) Permission node required to enter
permission: "dungeons.enter.goblin_cave"

# (Optional) Whether dungeon is enabled
enabled: true

# (Optional) Icon for GUI menus
icon:
  material: "STONE_SWORD"
  custom-model-data: 0

# Description shown in /dungeon info
description:
  - "&7A dark cave infested with goblins."
  - "&7Clear the cave and find the treasure!"
  - ""
  - "&eObjectives:"
  - "&7- Defeat 20 goblins"
  - "&7- Find the hidden treasure"
  - "&7- Locate the escape route"
```

## Field Reference

### Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | String | Unique identifier for the dungeon |
| `display-name` | String | Name shown to players (supports color codes) |
| `schematic-file` | String | Schematic filename in schematics folder |
| `party-size.min` | Integer | Minimum party size required |
| `party-size.max` | Integer | Maximum party size allowed |
| `time-limit` | Integer | Time limit in seconds |
| `spawn-offset.x` | Double | X offset for player spawn point |
| `spawn-offset.y` | Double | Y offset for player spawn point |
| `spawn-offset.z` | Double | Z offset for player spawn point |
| `quests` | List | Quest IDs to assign to this dungeon |
| `rewards` | List | Reward table IDs for completion |
| `cooldown` | Long | Cooldown in seconds before re-run |

### Optional Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `difficulty` | String | "MEDIUM" | Difficulty level (EASY/MEDIUM/HARD/EXTREME) |
| `entry-cost` | Double | 0.0 | Cost in money to enter (requires Vault) |
| `permission` | String | `dungeons.enter.<id>` | Permission node required |
| `enabled` | Boolean | true | Whether dungeon is enabled |
| `icon.material` | String | "STONE" | Material for GUI icon |
| `icon.custom-model-data` | Integer | 0 | Custom model data for icon |
| `description` | List | Empty | Lines shown in /dungeon info |

## Spawn Offset Guide

The `spawn-offset` determines where players are teleported when they enter the dungeon, relative to where the schematic is pasted.

### Understanding Spawn Offset

- **Base Location**: The schematic is pasted at the grid slot location (e.g., `0, 100, 0`)
- **Spawn Offset**: Added to the base location to position players correctly
- **Example**: If schematic is pasted at `(0, 100, 0)` and spawn offset is `(25.5, 1.0, 25.5)`, players spawn at `(25.5, 101.0, 25.5)`

### Common Patterns

**Center of 50x50 platform:**
```yaml
spawn-offset:
  x: 25.5
  y: 1.0
  z: 25.5
```

**Near entrance (corner):**
```yaml
spawn-offset:
  x: 5.0
  y: 1.0
  z: 5.0
```

**Elevated platform:**
```yaml
spawn-offset:
  x: 25.5
  y: 10.0
  z: 25.5
```

### Finding Your Spawn Point

1. Build your dungeon in-game
2. Stand where you want players to spawn
3. Note your coordinates (F3 debug screen)
4. Calculate offset from schematic origin:
   - If schematic starts at `(0, 0, 0)` and you're at `(30, 5, 15)`
   - Use `spawn-offset: { x: 30.0, y: 5.0, z: 15.0 }`

## Creating a New Dungeon

### Step 1: Build the Dungeon

Use WorldEdit to create your dungeon structure in-game.

### Step 2: Save the Schematic

```
//wand
(select your build)
//copy
//schem save my_dungeon
```

### Step 3: Move the Schematic File

Move `my_dungeon.schem` from `plugins/WorldEdit/schematics/` to `plugins/Dungeons/schematics/`

### Step 4: Create Configuration File

Create `plugins/Dungeons/dungeons/my_dungeon.yml`:

```yaml
id: "my_dungeon"
display-name: "&6My Awesome Dungeon"
schematic-file: "my_dungeon.schem"
party-size:
  min: 1
  max: 5
time-limit: 1200
spawn-offset:
  x: 25.5
  y: 1.0
  z: 25.5
quests:
  - "my_quest"
rewards:
  - "my_rewards"
cooldown: 3600
description:
  - "&7An awesome custom dungeon!"
```

### Step 5: Create Quests and Rewards

Add corresponding quest in `quests.yml` and reward table in `rewards.yml`.

### Step 6: Reload Plugin

```
/dngadmin reload
```

### Step 7: Test

```
/dungeon list
/dungeon info my_dungeon
/dungeon start my_dungeon
```

## Tips and Best Practices

### Spawn Point Selection

- ✅ Place spawn point in a safe, enclosed area
- ✅ Ensure players can see the dungeon layout from spawn
- ✅ Leave space around spawn for entire party
- ❌ Don't spawn players mid-air or in walls
- ❌ Don't spawn directly on mob spawners

### Party Size

- Small dungeons: `min: 1, max: 2`
- Medium dungeons: `min: 2, max: 4`
- Large dungeons: `min: 3, max: 5`
- Raids: `min: 4, max: 5`

### Time Limits

- Quick dungeons: 600-1200 seconds (10-20 minutes)
- Standard dungeons: 1800-2400 seconds (30-40 minutes)
- Epic dungeons: 3600-5400 seconds (60-90 minutes)

### Cooldowns

- Prevent farming: Use longer cooldowns (3600-7200 seconds)
- Encourage replaying: Use shorter cooldowns (1800-3600 seconds)
- Special dungeons: Use very long cooldowns (14400+ seconds)

### Difficulty Balance

- **EASY**: Solo-friendly, short, forgiving time limit
- **MEDIUM**: Small party, moderate challenge
- **HARD**: Full party recommended, strict time limit
- **EXTREME**: Full party required, difficult quests, tight timing

## Troubleshooting

### "Dungeon not found"
- Check that `id` field matches what you use in commands
- Ensure file is in `plugins/Dungeons/dungeons/` folder
- Verify file extension is `.yml`
- Run `/dngadmin reload`

### "Schematic not found"
- Verify schematic file exists in `plugins/Dungeons/schematics/`
- Check `schematic-file` field matches actual filename
- Ensure file extension is `.schem`

### Players spawn in wrong location
- Recalculate spawn offset based on schematic origin
- Use decimal values (e.g., `25.5`) for centering
- Test with `/dungeon start <dungeon>` and adjust

### Quests not loading
- Verify quest IDs exist in `quests.yml`
- Check for typos in quest ID names
- Ensure quests list is properly formatted

## Examples

See the default dungeons for reference:
- `dungeons/goblin_cave.yml` - Basic dungeon example
- `dungeons/undead_crypt.yml` - Boss fight dungeon
- `dungeons/ice_temple.yml` - Advanced dungeon with multiple objectives

## Advanced Configuration

### Multiple Reward Tables

Give multiple reward sets on completion:
```yaml
rewards:
  - "common_loot"
  - "rare_loot"
  - "bonus_crystals"
```

### Custom Permissions

Create unique permissions for special dungeons:
```yaml
permission: "dungeons.vip.special"
```

### Entry Costs

Require payment to enter (needs Vault):
```yaml
entry-cost: 500.0  # 500 money
```

### Decimal Party Sizes

Currently not supported - use integers only.

---

For more information, see:
- **SETUP.md** - Installation and setup guide
- **COMMANDS.md** - Command reference
- **WORLDEDIT.md** - WorldEdit integration guide
