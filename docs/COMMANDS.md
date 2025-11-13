# Commands Reference

Complete command reference for the Dungeons plugin.

## Table of Contents
- [Player Commands](#player-commands)
- [Party Commands](#party-commands)
- [Editor Commands](#editor-commands)
- [Admin Commands](#admin-commands)
- [Permissions](#permissions)

## Player Commands

Commands for regular players to interact with dungeons.

### `/dungeon` (aliases: `/dng`, `/d`, `/dung`)

Main dungeon command for players.

#### `/dungeon list`
List all available dungeons.

**Usage:**
```
/dungeon list
```

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
         Available Dungeons

Goblin Cave - MEDIUM
  Party: 2-4 players
  Time: 30 minutes
  Status: AVAILABLE

Dragon's Lair - HARD
  Party: 3-5 players
  Time: 45 minutes
  Status: ON COOLDOWN (15m)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

#### `/dungeon info <dungeon_id>`
View detailed information about a specific dungeon.

**Usage:**
```
/dungeon info goblin_cave
```

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
      Goblin Cave
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Difficulty: MEDIUM
Party Size: 2-4 players
Time Limit: 30 minutes (or "No Time Limit" if disabled)
Cooldown: 1 hour
Entry Cost: $500 (or "Free" if no cost)

Quests:
- Defeat 20 Goblins
- Find the Hidden Treasure
- Locate the Escape Route

Status: AVAILABLE
Use /dungeon join goblin_cave to start
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Note:** Completion time is tracked even for dungeons with no time limit.

#### `/dungeon join <dungeon_id>`
Join/start a dungeon with your party.

**Usage:**
```
/dungeon join goblin_cave
```

**Requirements:**
- Must be in a party
- Party must meet size requirements
- Must have permission for dungeon
- Dungeon must not be on cooldown (bypass with `dungeons.bypass.cooldown`)
- Must have sufficient money if dungeon has entry cost
- Party leader must execute command

**Output (Success):**
```
✓ Starting Goblin Cave...
  Loading dungeon instance...
  Teleporting party...

Welcome to Goblin Cave!
Time Limit: 30 minutes
Good luck!
```

**Output (Error Examples):**
```
✗ You are not in a party. Use /dngparty create
✗ Party too small (2 required, you have 1)
✗ Party too large (max 4, you have 5)
✗ Dungeon on cooldown (45 minutes remaining)
✗ Permission denied: dungeons.enter.goblin_cave
```

#### `/dungeon leave`
Leave your current dungeon instance.

**Usage:**
```
/dungeon leave
```

**Effects:**
- Teleports to spawn/return point
- Fails all active quests
- No rewards given
- Cooldown still applies

**Confirmation:**
```
> Are you sure you want to leave?
> This will fail all quests!
> Type /dungeon leave again to confirm
```

## Party Commands

Commands for creating and managing parties.

### `/dngparty` (aliases: `/party`, `/p`)

Main party command.

#### `/dngparty create`
Create a new party.

**Usage:**
```
/dngparty create
```

**Output:**
```
✓ Party created!
  You are now the party leader.
  Use /dngparty invite <player> to add members
```

#### `/dngparty invite <player>`
Invite a player to your party.

**Usage:**
```
/dngparty invite Steve
```

**Output (You):**
```
✓ Invitation sent to Steve
```

**Output (Steve):**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
      Party Invitation

Alex has invited you to their party!

/dngparty accept - Join the party
/dngparty decline - Decline invitation

Invitation expires in 60 seconds
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

#### `/dngparty accept`
Accept a party invitation.

**Usage:**
```
/dngparty accept
```

**Output:**
```
✓ Joined Alex's party!
  Members: Alex (Leader), Steve, You
```

#### `/dngparty decline`
Decline a party invitation.

**Usage:**
```
/dngparty decline
```

#### `/dngparty kick <player>`
Kick a player from your party (leader only).

**Usage:**
```
/dngparty kick Steve
```

**Output:**
```
✓ Kicked Steve from the party
```

#### `/dngparty leave`
Leave your current party.

**Usage:**
```
/dngparty leave
```

**Output:**
```
✓ Left the party
```

**Note:** If leader leaves, party is disbanded.

#### `/dngparty disband`
Disband your party (leader only).

**Usage:**
```
/dngparty disband
```

**Output:**
```
✓ Party disbanded
  All members have been removed
```

#### `/dngparty list`
List all party members.

**Usage:**
```
/dngparty list
```

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
         Party Members

★ Alex (Leader)
  Steve
  Bob
  Alice

Total: 4 members
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Editor Commands

Commands for creating and editing dungeons (requires `dungeons.editor`).

### `/dng`

Main editor command.

#### Basic Editing

##### `/dng create <id>`
Create a new dungeon.

**Usage:**
```
/dng create my_dungeon
```

**Output:**
```
✓ Creating new dungeon: my_dungeon
  Use /dng set <property> <value> to configure
  Use /dng setspawn to set party spawn point
  Use /dng save when finished
```

##### `/dng edit <id>`
Edit an existing dungeon.

**Usage:**
```
/dng edit goblin_cave
```

**Output:**
```
✓ Editing dungeon: goblin_cave
  Use /dng info to see current configuration
  Use /dng set <property> <value> to modify
  Use /dng save when finished
```

##### `/dng save`
Save current dungeon configuration.

**Usage:**
```
/dng save
```

**Output:**
```
✓ Dungeon goblin_cave saved successfully!
  Use /dngadmin reload to load the changes
```

##### `/dng cancel`
Cancel editing session without saving.

**Usage:**
```
/dng cancel
```

**Output:**
```
✓ Editing session cancelled. Changes not saved.
```

##### `/dng info`
Show current dungeon configuration.

**Usage:**
```
/dng info
```

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Dungeon Editor: goblin_cave

Display Name: Goblin Cave
Schematic: goblin_cave.schem
Party Size: 2 - 4
Time Limit: 1800 seconds
Cooldown: 3600 seconds
Difficulty: MEDIUM
Entry Cost: $500.00
Spawn Offset: X:25.50 Y:1.00 Z:25.50
Quests: goblin_slayer, treasure_hunter
Rewards: goblin_loot, common_rewards
Enabled: true
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

##### `/dng list`
List all dungeons.

**Usage:**
```
/dng list
```

#### Configuration Commands

##### `/dng set <property> <value>`
Set dungeon properties.

**Properties:**
- `name` / `displayname` - Display name
- `schematic` / `schem` - Schematic filename
- `minparty` / `minsize` - Minimum party size
- `maxparty` / `maxsize` - Maximum party size
- `timelimit` / `time` - Time limit in seconds (0 = no limit, completion time still tracked)
- `cooldown` - Cooldown in seconds
- `difficulty` - Difficulty level (freeform text)
- `cost` / `entrycost` - Entry cost in money (requires Vault)
- `enabled` - Enable/disable (true/false)

**Examples:**
```
/dng set name &6My Awesome Dungeon
/dng set schematic my_dungeon.schem
/dng set minparty 2
/dng set maxparty 4
/dng set timelimit 1800
/dng set cooldown 3600
/dng set difficulty HARD
/dng set cost 500
/dng set enabled true
```

##### `/dng set spawn`
Set spawn point at targeted block. **Look at a block and run this command.**

**Usage:**
```
# Look at the block where you want players to spawn
/dng set spawn
```

**Output:**
```
✓ Spawn point set to targeted block:
  X: 25.50 Y: 1.00 Z: 25.50
  Note: This is relative to where the schematic is pasted (0, 100, 0)
```

**Important:** You must be looking at a block within 5 blocks when you run this command.

#### Content Management

##### `/dng add quest <quest_id>`
Add a quest to the dungeon.

**Usage:**
```
/dng add quest goblin_slayer
```

**Output:**
```
✓ Added quest: goblin_slayer
```

##### `/dng add reward <reward_table>`
Add a reward table to the dungeon.

**Usage:**
```
/dng add reward goblin_loot
```

**Output:**
```
✓ Added reward table: goblin_loot
```

##### `/dng add trigger <id> <type>`
Add a trigger at the targeted block (for LOCATION triggers) or with default values (other types). **Look at a block before running for LOCATION triggers.**

**Trigger Types:**
- `LOCATION` - When player enters an area (requires looking at block)
- `TIMER` - After X seconds in dungeon
- `MOB_KILL` - When X mobs are killed
- `QUEST_COMPLETE` - When a specific quest completes
- `PLAYER_DEATH` - When a player dies
- `BOSS_KILL` - When a boss is killed

**Usage:**
```
# For LOCATION trigger (look at a block first!)
/dng add trigger ambush LOCATION

# For other trigger types
/dng add trigger timer_warning TIMER
/dng add trigger boss_spawn BOSS_KILL
```

**Output:**
```
✓ Added LOCATION trigger: ambush
  Location: 45.5, 5.5, 32.5
  Use /dng trigger edit ambush to configure actions and properties
```

##### `/dng remove quest <quest_id>`
Remove a quest from the dungeon.

**Usage:**
```
/dng remove quest old_quest
```

**Output:**
```
✓ Removed quest: old_quest
```

##### `/dng remove reward <reward_table>`
Remove a reward table from the dungeon.

**Usage:**
```
/dng remove reward old_rewards
```

**Output:**
```
✓ Removed reward table: old_rewards
```

##### `/dng remove trigger <id>`
Remove a trigger from the dungeon.

**Usage:**
```
/dng remove trigger ambush
```

**Output:**
```
✓ Removed trigger: ambush
```

#### Schematic Editing

All schematic commands are under `/dng schematic` (or `/dng schem` for short).

##### `/dng schematic paste`
Request to paste schematic for editing.

**Usage:**
```
/dng schematic paste
# Or use the alias:
/dng schem paste
```

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
      PASTE CONFIRMATION

You are about to paste schematic:
goblin_cave.schem

At location:
100, 64, 200

This will overwrite existing blocks!

Type /dng schematic confirm to proceed
(Expires in 30 seconds)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

##### `/dng schematic confirm`
Confirm schematic paste.

**Usage:**
```
/dng schematic confirm
```

**Output:**
```
✓ Loading schematic...
✓ Pasting schematic...
✓ Schematic pasted successfully!
  You can now modify it and use /dng schematic save to save changes
  Use /dng schematic pos1 and pos2 to select the area to save
```

##### `/dng schematic pos1`
Set first selection corner. **Look at a block and run this command.**

**Usage:**
```
# Look at the first corner block
/dng schematic pos1
```

**Output:**
```
✓ Position 1 set to: 100, 64, 200
```

**With both positions:**
```
✓ Position 1 set to: 100, 64, 200
  Selection size: 51x11x51 (28611 blocks)
```

**Important:** You must be looking at a block within 5 blocks when you run this command.

##### `/dng schematic pos2`
Set second selection corner. **Look at a block and run this command.**

**Usage:**
```
# Look at the second corner block
/dng schematic pos2
```

**Output:**
```
✓ Position 2 set to: 150, 74, 250
  Selection size: 51x11x51 (28611 blocks)
```

**Important:** You must be looking at a block within 5 blocks when you run this command.

##### `/dng schematic save`
Save selected area to schematic file.

**Usage:**
```
/dng schematic save
```

**Requirements:**
- Both pos1 and pos2 must be set
- Positions must be in same world
- Must be in editing session

**Output:**
```
✓ Saving schematic...
  Region size: 51x11x51 (28611 blocks)
✓ Schematic saved successfully!
  File: goblin_cave.schem
  Don't forget to /dng save to update the dungeon configuration!
```

#### Trigger Management

All trigger commands are under `/dng trigger`.

##### `/dng trigger set <id>`
Update a LOCATION trigger's position to the targeted block. **Look at a block and run this command.**

**Usage:**
```
# Look at the new location for the trigger
/dng trigger set ambush
```

**Output:**
```
✓ Trigger ambush location updated!
  New location: 50.5, 10.5, 40.5
```

**Important:** Only works for LOCATION triggers. You must be looking at a block within 5 blocks.

##### `/dng trigger edit <id>`
Open GUI editor for a trigger to configure conditions and actions.

**Usage:**
```
/dng trigger edit ambush
```

**Opens:** Trigger Editor GUI where you can:
- Set trigger type and conditions
- Add/remove/edit actions
- Configure repeatability and cooldown
- Set trigger description

##### `/dng trigger list`
List all configured triggers for the dungeon.

**Usage:**
```
/dng trigger list
```

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Triggers: goblin_cave

ambush (LOCATION)
  Location: 35.5, 5.5, 25.5 (Radius: 5.0)
  Actions: 2 | Repeatable: No

boss_arena (LOCATION)
  Location: 50.5, 5.5, 50.5 (Radius: 8.0)
  Actions: 4 | Repeatable: No

time_warning (TIMER)
  Actions: 1 | Repeatable: No

Use /dng trigger edit <id> to configure
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

##### `/dng trigger add action <type>`
Add an action to the currently selected trigger via command line (faster than GUI).

**Requirements:**
- Must have a trigger selected via `/dng trigger set <id>` first
- For location-based actions (SPAWN_MOB, DROP_ITEM, TELEPORT), look at a block before running

**Action Types:**
- `SPAWN_MOB` - Spawn mobs at location (requires looking at block)
- `DROP_ITEM` - Drop items at location (requires looking at block)
- `TELEPORT` - Teleport players to location (requires looking at block)
- `DAMAGE_PLAYER` - Deal damage to players
- `MESSAGE` - Send message to players
- `COMMAND` - Execute console command
- `POTION_EFFECT` - Apply potion effect to players

**Usage:**
```
# First, select a trigger
/dng trigger set ambush

# Then add actions (look at block for location-based actions)
/dng trigger add action SPAWN_MOB       # Adds 1x ZOMBIE at targeted location
/dng trigger add action MESSAGE         # Adds default message
/dng trigger add action DAMAGE_PLAYER   # Adds 5 damage
/dng trigger add action TELEPORT        # Teleport to targeted location
```

**Output:**
```
✓ Added SPAWN_MOB action to trigger ambush
  Location: 45.5, 10.5, 32.5
  Default: 1x ZOMBIE. Use GUI to customize: /dng trigger edit ambush
  Total actions: 3
```

**Notes:**
- Actions are created with sensible defaults
- Use the GUI editor (`/dng trigger edit <id>`) to customize action details
- Action IDs are auto-generated (e.g., `spawn_mob_1`, `teleport_2`)

#### Custom Mobs

All custom mob commands are under `/dng mobs` (or `/dng mob` for short).

##### `/dng mobs create <id>`
Create a new custom mob with default values.

**Usage:**
```
/dng mobs create goblin_warrior
```

**Output:**
```
✓ Created custom mob: goblin_warrior
  Use /dng mobs edit goblin_warrior to configure it
```

##### `/dng mobs edit <id>`
Open GUI editor for a custom mob.

**Usage:**
```
/dng mobs edit goblin_warrior
```

**Opens:** Custom Mob Editor GUI where you can:
- Set entity type (ZOMBIE, SKELETON, etc.)
- Configure name and visibility
- Set health, damage, and speed
- Add equipment and enchantments
- Add potion effects

##### `/dng mobs remove <id>`
Remove a custom mob from the dungeon.

**Usage:**
```
/dng mobs remove old_mob
```

**Output:**
```
✓ Removed custom mob: old_mob
```

##### `/dng mobs list`
List all custom mobs for the dungeon.

**Usage:**
```
/dng mobs list
```

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Custom Mobs: goblin_cave

goblin_warrior - ZOMBIE (HP: 40.0)
  Name: &2&lGoblin Warrior

goblin_archer - SKELETON (HP: 25.0)
  Name: &a&lGoblin Archer

goblin_king - ZOMBIE (HP: 150.0)
  Name: &6&l&nGOBLIN KING

Use /dng mobs edit <id> to configure a mob
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

#### Quest Management

All quest commands are under `/dng quest` (or `/dng quests` for short).

##### `/dng quest create <id> <type>`
Create a new quest with the specified type.

**Quest Types:**
- `KILL_MOBS` - Kill specific mob types
- `KILL_BOSS` - Kill a specific boss mob
- `COLLECT_ITEMS` - Collect specific items
- `REACH_LOCATION` - Reach a specific location
- `SURVIVE_TIME` - Survive for a duration
- `INTERACT_BLOCKS` - Interact with specific blocks

**Usage:**
```
/dng quest create boss_fight KILL_BOSS
/dng quest create treasure_hunt COLLECT_ITEMS
/dng quest create survive_wave SURVIVE_TIME
```

**Output:**
```
✓ Created quest: boss_fight (Type: KILL_BOSS)
  Use /dng quest set boss_fight <property> <value> to configure
  Use /dng quest objective add boss_fight to add objectives
```

##### `/dng quest set <id> <property> <value>`
Set quest properties.

**Properties:**
- `name` - Display name (supports color codes)
- `desc` / `description` - Quest description
- `required` - true/false (required quests must complete for dungeon success)
- `showprogress` / `show-progress` - true/false (show progress to players)
- `bonusreward` / `bonus-reward` - Reward table ID for completion bonus

**Usage:**
```
/dng quest set boss_fight name &c&lDefeat the Goblin King
/dng quest set boss_fight desc Kill the mighty Goblin King
/dng quest set boss_fight required true
/dng quest set boss_fight bonusreward boss_bonus_loot
```

**Output:**
```
✓ Set quest name to: &c&lDefeat the Goblin King
```

##### `/dng quest objective add <quest_id> <params...>`
Add an objective to a quest (parameters depend on quest type).

**Parameters by Quest Type:**
```
KILL_MOBS:        /dng quest objective add <id> <amount> <mob_type>
                  Example: /dng quest objective add quest1 50 ZOMBIE

KILL_BOSS:        /dng quest objective add <id> <amount> <boss_id>
                  Example: /dng quest objective add quest1 1 goblin_king

COLLECT_ITEMS:    /dng quest objective add <id> <amount> <material>
                  Example: /dng quest objective add quest1 64 DIAMOND

REACH_LOCATION:   /dng quest objective add <id> 1 [radius]
                  (Look at block first! Radius optional, default: 3)
                  Example: /dng quest objective add quest1 1 5

SURVIVE_TIME:     /dng quest objective add <id> <duration_seconds>
                  Example: /dng quest objective add quest1 300

INTERACT_BLOCKS:  /dng quest objective add <id> <amount> <material>
                  Example: /dng quest objective add quest1 10 LEVER
```

**Output:**
```
✓ Added objective to quest boss_fight
  Total objectives: 1
```

##### `/dng quest objective list <quest_id>`
List all objectives for a quest.

**Usage:**
```
/dng quest objective list treasure_hunt
```

**Output:**
```
Objectives for: treasure_hunt
1. Collect 10 GOLD_INGOT (Amount: 10)
2. Collect 5 DIAMOND (Amount: 5)
3. Collect 1 EMERALD (Amount: 1)
```

##### `/dng quest view <id>`
View detailed information about a quest.

**Usage:**
```
/dng quest view boss_fight
```

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Quest: boss_fight

Name: &c&lDefeat the Goblin King
Description: Kill the mighty Goblin King
Type: KILL_BOSS
Required: Yes
Show Progress: Yes
Bonus Reward: boss_bonus_loot

Objectives: 1
1. Kill goblin_king (x1)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

##### `/dng quest list`
List all configured quests.

**Usage:**
```
/dng quest list
```

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Available Quests:

boss_fight (KILL_BOSS) REQUIRED
  Name: Defeat the Goblin King
  Objectives: 1

treasure_hunt (COLLECT_ITEMS) Optional
  Name: Treasure Hunt
  Objectives: 3

Use /dng quest view <id> to see details
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

##### `/dng quest delete <id>`
Delete a quest permanently.

**Usage:**
```
/dng quest delete old_quest
```

**Output:**
```
✓ Deleted quest: old_quest
```

#### Reward Management

All reward commands are under `/dng reward` (or `/dng rewards` for short).

##### `/dng reward add item <table> <material> [amount] [chance]`
Add an item to a reward table (creates table if it doesn't exist).

**Parameters:**
- `table` - Reward table ID
- `material` - Minecraft material name (tab completion supported!)
- `amount` - Amount or range (e.g., "5" or "1-5") [default: 1]
- `chance` - Drop chance percentage (0-100) [default: 100]

**Usage:**
```
/dng reward add item boss_loot DIAMOND 5 80.0
/dng reward add item boss_loot NETHERITE_SWORD 1 100.0
/dng reward add item rare_chest GOLDEN_APPLE 2-4 50.0
```

**Output:**
```
✓ Added DIAMOND x5 to reward table boss_loot
  Chance: 80.0%
  Use /dng reward view boss_loot to see all rewards
```

**Note:** Full material tab completion is supported - just press TAB!

##### `/dng reward set money <table> <amount>`
Set money reward for a reward table.

**Parameters:**
- `table` - Reward table ID
- `amount` - Single value or range (e.g., "1000" or "500-1500")

**Usage:**
```
/dng reward set money boss_loot 1000
/dng reward set money rare_chest 500-1000
```

**Output:**
```
✓ Set money reward for boss_loot to $1000
```

##### `/dng reward set exp <table> <amount>`
Set experience reward for a reward table.

**Parameters:**
- `table` - Reward table ID
- `amount` - Experience points to give

**Usage:**
```
/dng reward set exp boss_loot 5000
```

**Output:**
```
✓ Set experience reward for boss_loot to 5000 XP
```

##### `/dng reward view <table>`
Open GUI to view and manage reward table contents.

**Usage:**
```
/dng reward view boss_loot
```

**Opens:** Reward Manager GUI where you can:
- View all items in the reward table
- See money and experience rewards
- **Right-click to remove** items, money, or experience
- Changes save immediately

##### `/dng reward list`
List all configured reward tables.

**Usage:**
```
/dng reward list
```

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Reward Tables:

boss_loot
  Items: 5 | Money: 1000-5000 | XP: 10000

rare_chest
  Items: 3 | Money: 500-1000 | XP: 2500

common_rewards
  Items: 8 | Money: 100 | XP: 500

Use /dng reward view <table> to manage a reward table
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Admin Commands

Commands for server administrators (requires `dungeons.admin`).

### `/dngadmin` (aliases: `/dungeonadmin`, `/da`, `/dadmin`)

Main admin command.

#### `/dngadmin reload`
Reload plugin configuration.

**Usage:**
```
/dngadmin reload
```

**Output:**
```
✓ Reloading configuration...
  Loaded 5 dungeons
  Loaded 12 quests
  Loaded 8 reward tables
✓ Configuration reloaded successfully!
```

#### `/dngadmin reset <player|dungeon>`
Reset player data or dungeon cooldowns.

**Usage:**
```
/dngadmin reset player Steve
/dngadmin reset dungeon goblin_cave
```

**Output:**
```
✓ Reset player data for Steve
✓ Reset cooldowns for goblin_cave
```

#### `/dngadmin list`
List all active dungeon instances.

**Usage:**
```
/dngadmin list
```

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
      Active Instances

#1 goblin_cave
   Party: Alex, Steve, Bob
   Time: 15:30 remaining
   Grid: [2, 3]

#2 dragon_lair
   Party: Alice, Charlie
   Time: 38:45 remaining
   Grid: [1, 1]

Total: 2 active instances
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

#### `/dngadmin stats`
View plugin statistics.

**Usage:**
```
/dngadmin stats
```

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
      Plugin Statistics

Total Dungeons: 5
Active Instances: 2
Total Completions: 1,234
Success Rate: 67%

Grid Usage: 2/100 slots
Database: SQLite
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

#### `/dngadmin teleport <instance_id>`
Teleport to a dungeon instance (spectate).

**Usage:**
```
/dngadmin teleport 1
```

**Output:**
```
✓ Teleported to instance #1 (goblin_cave)
```

#### `/dngadmin cleanup`
Force cleanup of all instances.

**Usage:**
```
/dngadmin cleanup
```

**Output:**
```
⚠ Cleaning up all active instances...
  Removed 2 instances
  Released 2 grid slots
✓ Cleanup complete
```

**Warning:** This will forcefully end all active dungeon runs!

## Permissions

### Player Permissions

```yaml
dungeons.party:
  description: Use party commands
  default: true

dungeons.enter:
  description: Enter any dungeon
  default: true

dungeons.enter.<dungeon_id>:
  description: Enter specific dungeon
  default: true
  example: dungeons.enter.goblin_cave
```

### Editor Permissions

```yaml
dungeons.editor:
  description: Use dungeon editor commands
  default: op

dungeons.editor.create:
  description: Create new dungeons
  default: op

dungeons.editor.edit:
  description: Edit existing dungeons
  default: op

dungeons.editor.delete:
  description: Delete dungeons
  default: op
```

### Admin Permissions

```yaml
dungeons.admin:
  description: Use admin commands
  default: op

dungeons.admin.reload:
  description: Reload configuration
  default: op

dungeons.admin.reset:
  description: Reset player/dungeon data
  default: op

dungeons.admin.teleport:
  description: Teleport to instances
  default: op

dungeons.admin.cleanup:
  description: Force cleanup instances
  default: op
```

### Bypass Permissions

```yaml
dungeons.bypass.cooldown:
  description: Bypass dungeon cooldowns
  default: op

dungeons.bypass.party-size:
  description: Bypass party size requirements
  default: op

dungeons.bypass.time-limit:
  description: No time limit in dungeons
  default: op
```

## Tab Completion

All commands support tab completion:

```
/dungeon <tab>               → list, info, join, leave
/dungeon info <tab>          → goblin_cave, dragon_lair, ...
/dngparty <tab>              → create, invite, accept, leave, ...
/dng <tab>                   → create, edit, save, quest, reward, trigger, ...
/dng set <tab>               → name, schematic, minparty, cost, ...
/dng quest <tab>             → create, set, objective, list, view, delete
/dng quest create <tab>      → KILL_MOBS, KILL_BOSS, COLLECT_ITEMS, ...
/dng reward <tab>            → add, set, view, list
/dng reward add item <tab>   → [All reward tables]
/dng reward add item t <tab> → DIAMOND, EMERALD, ... [All MC materials!]
/dng trigger <tab>           → set, edit, list, add
```

## Command Aliases

```
/dungeon    → /dng, /d, /dung
/dngparty   → /party, /p
/dngadmin   → /dungeonadmin, /da, /dadmin
```

## See Also

- [Editor Workflow](EDITOR_WORKFLOW.md) - Dungeon creation guide
- [Custom Mobs](CUSTOM_MOBS.md) - Custom mob configuration
- [Triggers](TRIGGERS.md) - Trigger system reference
- [GUI Editors](GUI_EDITORS.md) - Visual editor guide
