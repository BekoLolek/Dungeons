# Dungeons Plugin

A grid-based dungeon plugin for Minecraft 1.21.4 with parties, quests, and rewards.

## Features

### Core Systems
- **Party System**: Create parties and invite friends to tackle dungeons together
- **Flexible Grid System**: Choose between automatic separate world or manual placement in existing world
- **Grid-Based Instances**: Each party gets their own isolated dungeon instance with configurable spacing
- **Quest System**: 6 quest types with full command-line creation (KILL_MOBS, KILL_BOSS, COLLECT_ITEMS, REACH_LOCATION, SURVIVE_TIME, INTERACT_BLOCKS)
- **Reward System**: Earn items, money (Vault), and experience with chance-based loot tables - manage via commands and GUI
- **Entry Cost System**: Optional economy integration with configurable dungeon entry costs (requires Vault)
- **Post-Completion Wait**: Optional wait period after completion for exploration and celebration

### Advanced Features
- **Custom Mobs**: Fully customizable mobs with equipment, enchantments, stats, and potion effects
- **MythicMobs Integration**: Optional support for MythicMobs custom mobs
- **Trigger System**: Event-based actions (location, timer, mob kills, quests, player death, boss kills)
- **Trigger Actions**: Spawn mobs, send messages, execute commands, teleport, damage, drop items, apply potion effects - add via command or GUI
- **Command-Line Creation**: Create quests, rewards, and trigger actions rapidly via commands with full tab completion
- **Visual GUI Editors**: In-game editors for custom mobs, triggers, actions, and rewards with real-time updates
- **WorldEdit Integration**: Complete schematic editing workflow with paste confirmation and region selection
- **WorldGuard Integration**: Automatic region protection with configurable flags (no block breaking/placing, explosions, fire)
- **Database Support**: SQLite and MySQL with HikariCP connection pooling
- **Completion Time Tracking**: Track dungeon completion times for statistics and leaderboards
- **Bypass Permissions**: Optional cooldown bypass for VIPs and testing
- **Fully Configurable**: Customize everything via YAML, commands, or in-game editors

## Requirements

**Required:**
- Minecraft 1.21.4 (Paper)
- Java 21

**Optional:**
- WorldEdit 7.3.0+ (for schematic loading and in-game editing)
- WorldGuard 7.0.9+ (for region protection and flag management)
- MythicMobs 5.6.1+ (for advanced custom mobs)
- Vault (for economy integration and money rewards)
- Multiverse-Core (for advanced teleportation, not required)

## Installation

1. Download the plugin JAR file
2. Place it in your server's `plugins/` folder
3. (Optional) Install WorldEdit 7.4.0+ for schematic loading
4. (Optional) Install Vault and an economy plugin for money rewards
5. (Optional) Install Multiverse-Core for advanced teleportation
6. Restart your server
7. Configure grid mode in `config.yml` (auto or manual)
8. Place dungeon schematic files in `plugins/Dungeons/schematics/` (if using WorldEdit)
9. Configure individual dungeons in `plugins/Dungeons/dungeons/` folder or use `/dng` in-game

## Building

```bash
mvn clean package
```

The compiled JAR will be in `target/Dungeons-1.0.0.jar`

## Commands

**Party Commands** (aliases: /party, /p)
- `/dngparty create` - Create a new party
- `/dngparty invite <player>` - Invite a player to your party
- `/dngparty accept` - Accept a party invitation
- `/dngparty kick <player>` - Kick a player from your party
- `/dngparty leave` - Leave your current party
- `/dngparty disband` - Disband your party
- `/dngparty list` - List party members

**Dungeon Commands** (aliases: /d, /dung)
- `/dungeon list` - List available dungeons
- `/dungeon start <dungeon>` - Start a dungeon with your party
- `/dungeon leave` - Leave your current dungeon
- `/dungeon info <dungeon>` - View dungeon information

**Editor Commands** (requires `dungeons.editor` permission)

*Basic Editing:*
- `/dng create <id>` - Create new dungeon
- `/dng edit <id>` - Edit existing dungeon
- `/dng save` - Save current dungeon configuration
- `/dng cancel` - Cancel editing session
- `/dng info` - Show current configuration
- `/dng list` - List all dungeons

*Configuration:*
- `/dng set <property> <value>` - Set dungeon properties (name, schematic, minparty, maxparty, timelimit, cooldown, difficulty, cost, enabled)
- `/dng set spawn` - Set spawn point at targeted block (look at block and run command)

*Content Management:*
- `/dng add quest <id>` - Add quest to dungeon
- `/dng add reward <table>` - Add reward table to dungeon
- `/dng add trigger <id> <type>` - Add trigger at targeted block (Types: LOCATION, TIMER, MOB_KILL, QUEST_COMPLETE, PLAYER_DEATH, BOSS_KILL)
- `/dng remove quest <id>` - Remove quest from dungeon
- `/dng remove reward <table>` - Remove reward table from dungeon
- `/dng remove trigger <id>` - Remove trigger from dungeon

*Quest Management:*
- `/dng quest create <id> <type>` - Create new quest (Types: KILL_MOBS, KILL_BOSS, COLLECT_ITEMS, REACH_LOCATION, SURVIVE_TIME, INTERACT_BLOCKS)
- `/dng quest set <id> <property> <value>` - Set quest properties (name, desc, required, showprogress, bonusreward)
- `/dng quest objective add <id> <params>` - Add objective (parameters depend on quest type)
- `/dng quest objective list <id>` - List objectives for a quest
- `/dng quest view <id>` - View quest details
- `/dng quest list` - List all quests
- `/dng quest delete <id>` - Delete a quest
- Alias: `/dng quests <subcommand>`

*Reward Management:*
- `/dng reward add item <table> <material> [amount] [chance]` - Add item to reward table (creates if doesn't exist)
- `/dng reward set money <table> <amount>` - Set money reward (single value or range like "100-250")
- `/dng reward set exp <table> <amount>` - Set experience reward
- `/dng reward view <table>` - Open GUI to view/remove rewards (right-click to remove)
- `/dng reward list` - List all reward tables
- Alias: `/dng rewards <subcommand>`

*Schematic Editing:*
- `/dng schematic paste` - Paste schematic for editing (requires confirmation)
- `/dng schematic confirm` - Confirm schematic paste (30 second window)
- `/dng schematic pos1` - Set first selection corner (look at block)
- `/dng schematic pos2` - Set second selection corner (look at block)
- `/dng schematic save` - Save modified schematic back to file
- Alias: `/dng schem <subcommand>`

*Trigger Management:*
- `/dng trigger set <id>` - Update LOCATION trigger position (or select trigger for action commands)
- `/dng trigger edit <id>` - Open GUI editor for trigger
- `/dng trigger list` - List all configured triggers
- `/dng trigger add action <type>` - Add action to selected trigger (Types: SPAWN_MOB, DROP_ITEM, TELEPORT, DAMAGE_PLAYER, MESSAGE, COMMAND, POTION_EFFECT)

*Custom Mobs:*
- `/dng mobs create <id>` - Create new custom mob
- `/dng mobs edit <id>` - Edit custom mob in GUI
- `/dng mobs remove <id>` - Remove custom mob
- `/dng mobs list` - List all custom mobs
- Alias: `/dng mob <subcommand>`

**Admin Commands** (aliases: /dungeonadmin, /da, /dadmin)
- `/dngadmin reload` - Reload configuration
- `/dngadmin reset <player|dungeon>` - Reset player or dungeon data
- `/dngadmin list` - List active dungeon instances
- `/dngadmin stats` - View plugin statistics
- `/dngadmin teleport <instance>` - Teleport to instance

## Permissions

- `dungeons.party` - Use party commands (default: true)
- `dungeons.enter` - Enter dungeons (default: true)
- `dungeons.enter.<dungeon>` - Enter specific dungeon
- `dungeons.editor` - Use in-game dungeon editor (default: op)
- `dungeons.admin` - Use admin commands (default: op)
- `dungeons.bypass.cooldown` - Bypass cooldowns (default: op)

## Configuration

### Main Config (`config.yml`)
- **Grid system settings** (auto or manual mode - see detailed section below)
- Party settings (size limits, friendly fire, invitations)
- Instance settings (time limits, cooldowns, post-completion wait period)
- Return point configuration (where players teleport after dungeon)
- Database configuration (SQLite or MySQL)
- Economy settings (Vault integration)
- **WorldGuard integration** (region protection and flags)

### Dungeons (`dungeons/*.yml`)
Each dungeon has its own configuration file in the `dungeons/` folder:
- Unique dungeon ID and display name
- Schematic file references
- Party size requirements (min/max)
- Time limits and cooldowns (set time-limit to 0 to disable, completion time is still tracked)
- Spawn point offsets (where players teleport)
- Quest assignments and reward tables
- See **DUNGEON_CONFIG.md** for detailed guide

### Quests (`quests.yml`)
- Quest objectives
- Quest types (kill mobs, collect items, reach location, etc.)
- Progress tracking

### Rewards (`rewards.yml`)
- Loot tables
- Item rewards with enchantments
- Money and experience rewards
- Chance-based rewards

## Grid System Configuration

The plugin uses a grid-based system to place dungeon instances. Each dungeon gets its own "slot" on the grid, ensuring spatial isolation between parties. The grid system supports two modes: **Auto** and **Manual**.

### Grid Modes

#### Auto Mode (Recommended)

Creates a separate void world automatically. This is the recommended mode for most servers.

**Configuration:**
```yaml
grid:
  mode: "auto"
  auto:
    world-name: "dungeon_world"
  slot-size: 256
  padding: 100
  base-y-level: 100
  max-slots: 100
```

**How it works:**
- Plugin creates a void world named `dungeon_world`
- Grid starts at coordinates (0, 0)
- Completely isolated from main world
- No terrain generation, just empty void

**Advantages:**
- ✅ No interference with main world
- ✅ No terrain generation overhead
- ✅ Clean and simple setup
- ✅ Players can't accidentally stumble into dungeons

**Use when:**
- You want dungeons completely separate from your world
- You don't mind having multiple worlds
- You want the cleanest setup (recommended)

#### Manual Mode (Advanced)

Uses an existing world and places dungeons at custom coordinates.

**Configuration:**
```yaml
grid:
  mode: "manual"
  manual:
    world-name: "world"
    start-x: 10000
    start-z: 10000
  slot-size: 256
  padding: 100
  base-y-level: 100
  max-slots: 100
```

**How it works:**
- Uses existing world (e.g., overworld, nether, custom world)
- Grid starts at specified coordinates (`start-x`, `start-z`)
- Dungeons placed in grid pattern from start point
- Uses same world as players

**Advantages:**
- ✅ No separate world needed
- ✅ Can use existing world infrastructure
- ✅ Dungeons accessible via overworld travel (if desired)
- ✅ Works with single-world setups

**Use when:**
- You want to minimize world count
- You have a large world with unused space
- Your host restricts number of worlds
- You want dungeons in same world as players

**Important considerations:**
- Choose start coordinates **far from spawn** (10000+ recommended)
- Ensure coordinates don't overlap with existing builds
- Calculate total grid area: `(slot-size + padding) × √max-slots`
- Example: 100 slots with 256 size + 100 padding = ~3,560 blocks radius

### Grid Parameters

| Parameter | Description | Default | Recommendation |
|-----------|-------------|---------|----------------|
| `slot-size` | Size of each dungeon slot in blocks | 256 | Should be larger than your largest dungeon schematic |
| `padding` | Space between dungeon slots | 100 | 50-100 for normal, 100-200 for large dungeons |
| `base-y-level` | Y level where dungeons spawn | 100 | 60-120 for overworld, adjust for nether/end |
| `max-slots` | Maximum concurrent instances | 100 | Based on expected player count |

### Grid Layout

**How dungeons are positioned:**

```
Formula: worldX = startX + (gridX × (slotSize + padding))
         worldZ = startZ + (gridZ × (slotSize + padding))
```

**Auto mode example** (start: 0, 0):
```
Slot [0,0]: (0, 100, 0)
Slot [1,0]: (356, 100, 0)     ← 256 + 100 = 356 blocks apart
Slot [0,1]: (0, 100, 356)
Slot [2,0]: (712, 100, 0)
```

**Manual mode example** (start: 10000, 10000):
```
Slot [0,0]: (10000, 100, 10000)
Slot [1,0]: (10356, 100, 10000)
Slot [0,1]: (10000, 100, 10356)
Slot [2,0]: (10712, 100, 10000)
```

### Calculating Grid Space

To determine how much space your grid needs:

```
Total slots per side = √max-slots
Distance per slot = slot-size + padding
Total area per side = slots-per-side × distance-per-slot

Example with max-slots: 100
Slots per side: √100 = 10
Distance per slot: 256 + 100 = 356
Total area: 10 × 356 = 3,560 blocks
```

For **manual mode**, ensure your start coordinates plus grid size don't:
- Overlap with spawn protection
- Interfere with existing builds
- Exceed world border (if set)

### Padding Recommendations

The padding prevents players in one instance from seeing another instance:

- **50-100 blocks**: Standard dungeons with normal render distance (8-12 chunks)
- **100-150 blocks**: Large dungeons or higher render distance (12-16 chunks)
- **150-200 blocks**: Very large dungeons or maximum render distance (16+ chunks)

**Formula for safe padding:**
```
Safe padding = (Render distance in chunks × 16) + buffer
Example: 12 chunks render = (12 × 16) + 40 = 232 blocks recommended
```

### Switching Between Modes

To change modes:

1. Stop server
2. Edit `config.yml` and change `grid.mode`
3. Update mode-specific settings
4. Delete old grid world folder if switching from auto to manual
5. Start server
6. Existing parties will need to restart their dungeons

**Note:** Changing modes will reset all active dungeon instances.

### Best Practices

**For auto mode:**
- Use default settings unless you have specific needs
- Consider backing up dungeon world separately
- Set `world.setAutoSave(false)` is automatic (no saves needed for temp dungeons)

**For manual mode:**
- Use world border to prevent players reaching dungeon area
- Set start coordinates at least 10,000 blocks from spawn
- Consider using a custom world instead of overworld
- Document your grid location for admins
- Add WorldGuard regions if you want extra protection

**Performance tips:**
- Keep `max-slots` reasonable (50-100 for most servers)
- Larger `padding` = safer but uses more space
- `slot-size` should be 20-30% larger than your biggest schematic
- Use auto mode for best performance (no terrain gen)

### Troubleshooting Grid Issues

**"Dungeon world is not initialized"**
- Check console for world creation errors
- Ensure world name doesn't conflict with existing worlds
- Verify file permissions for world folders

**"No available grid slots"**
- Increase `max-slots` in config
- Check for stuck/abandoned instances with `/dngadmin list`
- Manually release slots with server restart if needed

**"Players can see other instances"**
- Increase `padding` value
- Reduce server render distance
- Use opaque walls/barriers in dungeon designs

**Manual mode: "World not found"**
- Ensure world exists and is loaded
- Check world name spelling in config
- World must be loaded before plugin enables

## WorldGuard Integration

The plugin automatically integrates with WorldGuard to protect dungeon instances from griefing and unwanted block modifications.

### Enabling WorldGuard Protection

**Requirements:**
- WorldGuard 7.0.9+ installed on your server
- `worldguard.enabled: true` in config.yml (default)

**How it works:**
- When a dungeon instance starts, a protected region is created
- Region covers entire dungeon slot (slot-size × slot-size area)
- Region is named `dungeon_<instance-id>` (e.g., `dungeon_a1b2c3d4-...`)
- Region is automatically deleted when instance completes/fails

### Configuration

Edit `config.yml` to customize WorldGuard protection:

```yaml
worldguard:
  # Enable/disable WorldGuard integration
  enabled: true

  # Region flags (true = allow, false = deny)
  flags:
    block-break: false           # Prevent block breaking
    block-place: false           # Prevent block placing
    explosion: false             # Prevent TNT/creeper explosions
    fire-spread: false           # Prevent fire spreading
    lava-fire: false             # Prevent lava creating fire
    lighter: false               # Prevent flint and steel use
    pvp: true                    # Allow PVP combat
    mob-damage: true             # Allow mob damage
    mob-spawning: true           # Allow mob spawning
    creeper-explosion: false     # Prevent creeper explosions
    ghast-fireball: false        # Prevent ghast fireballs
```

### Default Protection

By default, dungeons have the following protection:
- ✅ Players cannot break blocks
- ✅ Players cannot place blocks
- ✅ Explosions disabled (TNT, creepers, etc.)
- ✅ Fire cannot spread
- ✅ PVP allowed (customize per dungeon)
- ✅ Mob damage allowed
- ✅ Mob spawning allowed

### Custom Flag Configurations

**For PVP dungeons:**
```yaml
worldguard:
  flags:
    pvp: true
    mob-damage: true
```

**For peaceful exploration dungeons:**
```yaml
worldguard:
  flags:
    pvp: false
    mob-damage: false
    mob-spawning: false
```

**For building/puzzle dungeons:**
```yaml
worldguard:
  flags:
    block-break: true
    block-place: true
    explosion: false
```

### Disabling WorldGuard

To disable WorldGuard integration:

```yaml
worldguard:
  enabled: false
```

The plugin will continue to work normally without region protection.

### Troubleshooting

**"Failed to create WorldGuard region"**
- Ensure WorldGuard is installed and enabled
- Check console for specific WorldGuard errors
- Verify world is loaded before dungeon starts

**"Regions not being deleted"**
- Check `/dngadmin cleanup` to force cleanup
- Manually remove regions with `/region remove dungeon_<id>`
- Restart server to clear stuck regions

## Creating Dungeons

1. Build your dungeon in-game
2. Use WorldEdit to save it as a schematic: `//copy` then `//schem save <name>`
3. Copy the `.schem` file to `plugins/Dungeons/schematics/`
4. Create a new dungeon file in `plugins/Dungeons/dungeons/` (e.g., `my_dungeon.yml`)
5. Configure the dungeon (ID, schematic file, spawn offset, party size, etc.)
6. Create quests in `quests.yml`
7. Define rewards in `rewards.yml`
8. Reload the plugin: `/dngadmin reload`

See **DUNGEON_CONFIG.md** for a detailed guide on creating and configuring dungeons.

## Documentation

- **[README.md](README.md)** (this file) - Overview, grid system guide, and quick reference
- **[COMMANDS.md](docs/COMMANDS.md)** - Complete command reference with examples
- **[CONFIGURATION.md](docs/CONFIGURATION.md)** - YAML configuration guide
- **[CUSTOM_MOBS.md](docs/CUSTOM_MOBS.md)** - Creating custom dungeon mobs with equipment and effects
- **[TRIGGERS.md](docs/TRIGGERS.md)** - Event-based trigger system and action types
- **[EDITOR_WORKFLOW.md](docs/EDITOR_WORKFLOW.md)** - Step-by-step dungeon editing with WorldEdit
- **[GUI_EDITORS.md](docs/GUI_EDITORS.md)** - Using the in-game visual editors
- **[QUESTS_REWARDS.md](docs/QUESTS_REWARDS.md)** - Quest objectives and loot tables

## Quick Start

**For most servers (recommended):**
1. Keep default grid mode (`auto`) in config.yml
2. Plugin creates separate `dungeon_world` automatically
3. Create dungeons with `/dng create <id>` or edit YAML files
4. Players use `/dngparty create` and `/dungeon start <id>`

**For single-world setups:**
1. Change `grid.mode: "manual"` in config.yml
2. Set `grid.manual.start-x` and `start-z` far from spawn (10000+)
3. Plugin uses existing world for dungeons
4. See **Grid System Configuration** section above for details

## License

All rights reserved.
