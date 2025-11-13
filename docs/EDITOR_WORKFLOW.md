# Dungeon Editor Workflow

Complete guide to creating and editing dungeons using the in-game editor with WorldEdit integration.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Creating a New Dungeon](#creating-a-new-dungeon)
- [Editing Existing Dungeons](#editing-existing-dungeons)
- [Schematic Editing Workflow](#schematic-editing-workflow)
- [Configuration Commands](#configuration-commands)
- [Best Practices](#best-practices)

## Prerequisites

**Required:**
- Permission: `dungeons.editor`
- WorldEdit 7.3.0+ installed

**Recommended:**
- WorldEdit selection tool (wooden axe)
- Creative mode for building
- Fly permission for easier editing

## Creating a New Dungeon

### Step 1: Build Your Dungeon

Build your dungeon structure in a flat, empty area:

```
1. Find a flat area or create a platform
2. Build your dungeon (recommended size: 50x50 or smaller)
3. Add spawn points, mob areas, treasure rooms
4. Note the spawn point location for players
```

### Step 2: Save as Schematic

Use WorldEdit to save your build:

```
//wand                          # Get selection tool
//pos1                          # Click first corner
//pos2                          # Click opposite corner
//copy                          # Copy to clipboard
//schem save my_dungeon         # Save schematic
```

The schematic is saved to `plugins/WorldEdit/schematics/my_dungeon.schem`

### Step 3: Move Schematic to Dungeons Plugin

```bash
# Copy from WorldEdit folder to Dungeons folder
# Linux/Mac:
cp plugins/WorldEdit/schematics/my_dungeon.schem plugins/Dungeons/schematics/

# Windows:
copy plugins\WorldEdit\schematics\my_dungeon.schem plugins\Dungeons\schematics\
```

### Step 4: Create Dungeon Configuration

```
/dng create my_dungeon
```

This starts an editing session. You'll see:
```
Creating new dungeon: my_dungeon
Use /dng set <property> <value> to configure
Use /dng setspawn to set party spawn point
Use /dng save when finished
```

### Step 5: Configure Basic Properties

```
/dng set name &6My Awesome Dungeon
/dng set schematic my_dungeon.schem
/dng set minparty 2
/dng set maxparty 4
/dng set timelimit 1800          # 30 minutes
/dng set cooldown 3600            # 1 hour
/dng set difficulty MEDIUM
```

### Step 6: Set Spawn Point

Stand where players should spawn:
```
/dng setspawn
```

### Step 7: Add Quests and Rewards

```
/dng addquest goblin_slayer
/dng addquest treasure_hunter
/dng addreward goblin_loot
/dng addreward common_rewards
```

### Step 8: Save Configuration (Optional)

**Note:** All changes are automatically saved as you make them. The `/dng save` command is optional and primarily used to close your editing session.

```
/dng save
```

**Auto-save features:**
- Changes are saved automatically after each command
- Progress is preserved on disconnect or death
- Manual save closes the editing session

### Step 9: Reload Plugin

```
/dngadmin reload
```

Your dungeon is now ready to use!

## Editing Existing Dungeons

### Step 1: Start Editing Session

```
/dng edit goblin_cave
```

You'll see current configuration and editing instructions.

### Step 2: View Current Configuration

```
/dng info
```

Shows all current settings.

### Step 3: Modify Properties

```
/dng set name &aUpdated Name
/dng set timelimit 2400
/dng removequest old_quest
/dng addquest new_quest
```

### Step 4: Reload Changes

**Note:** Changes are automatically saved as you make them. You only need to reload the plugin to apply them.

```
/dngadmin reload
```

Optionally, close your editing session:
```
/dng save
```

## Schematic Editing Workflow

This is the full workflow for editing dungeon schematics with WorldEdit integration.

### Overview

```
1. Start editing session
2. Paste schematic into world
3. Confirm paste location
4. Make modifications
5. Select modified area
6. Save back to schematic (configuration auto-saves)
7. Reload plugin to apply changes
```

**Note:** Dungeon configuration changes auto-save. Only schematics need manual saving.

### Step 1: Start Editing

```
/dng edit goblin_cave
```

### Step 2: Paste Schematic

Stand where you want to paste the dungeon:

```
/dng paste
```

You'll see a confirmation screen:
```
══════════════════════════════════════
         PASTE CONFIRMATION

You are about to paste schematic:
goblin_cave.schem

At location:
100, 64, 200

This will overwrite existing blocks!

Type /dng confirm to proceed
(Expires in 30 seconds)
══════════════════════════════════════
```

**Important Notes:**
- This pastes at your current location
- Existing blocks will be overwritten
- You have 30 seconds to confirm
- If you move, use `/dng paste` again

### Step 3: Confirm Paste

```
/dng confirm
```

The schematic will be pasted:
```
Loading schematic...
Pasting schematic...
Schematic pasted successfully!
You can now modify it and use /dng saveschem to save changes
Use /dng pos1 and /dng pos2 to select the area to save
```

### Step 4: Make Your Changes

Now you can modify the pasted dungeon:
- Add/remove blocks
- Change decorations
- Add new rooms
- Modify mob spawn areas
- Update treasure locations

**Tips:**
- Use WorldEdit for large changes: `//set`, `//replace`, etc.
- Use regular building for details
- Test trigger locations with `/dng pos1` at trigger points

### Step 5: Select Modified Area

Mark the corners of your modified dungeon:

**At first corner (e.g., lowest X,Y,Z):**
```
/dng pos1
```

**At opposite corner (e.g., highest X,Y,Z):**
```
/dng pos2
```

You'll see:
```
Position 1 set to: 100, 64, 200
```

After both positions:
```
Position 2 set to: 150, 74, 250
Selection size: 51x11x51 (28611 blocks)
```

**Important:**
- Select slightly larger than needed to avoid cutting off edges
- Both corners must encompass all changes
- Y-coordinates matter - include all vertical space

### Step 6: Save Modified Schematic

```
/dng saveschem
```

You'll see:
```
Saving schematic...
Region size: 51x11x51 (28611 blocks)
Schematic saved successfully!
File: goblin_cave.schem
Don't forget to /dng save to update the dungeon configuration!
```

This overwrites the original schematic file with your changes.

**Note:** Dungeon configuration auto-saves, so if you only modified the schematic and didn't change settings, you can skip Step 7.

### Step 7: Update Configuration (If Needed)

If you changed spawn point or other settings:

```
/dng setspawn              # If spawn moved (auto-saves)
/dng set <property> <value> # If other changes (auto-saves)
```

**Changes are automatically saved.** `/dng save` is only needed if you want to close your editing session.

### Step 8: Reload Plugin

```
/dngadmin reload
```

Your modified dungeon is now live!

## Configuration Commands

### Property Commands

```
/dng set name <display name>       # Set display name (supports & colors)
/dng set schematic <file>          # Set schematic file name
/dng set minparty <number>         # Minimum party size
/dng set maxparty <number>         # Maximum party size
/dng set timelimit <seconds>       # Time limit in seconds
/dng set cooldown <seconds>        # Cooldown in seconds
/dng set difficulty <level>        # Difficulty (EASY, MEDIUM, HARD)
/dng set enabled <true|false>      # Enable/disable dungeon
```

### Quest/Reward Commands

```
/dng addquest <quest_id>           # Add quest requirement
/dng removequest <quest_id>        # Remove quest
/dng addreward <reward_table>      # Add reward table
/dng removereward <reward_table>   # Remove reward table
```

### Schematic Commands

```
/dng paste                         # Request schematic paste
/dng confirm                       # Confirm paste (30 sec window)
/dng pos1                          # Set first selection corner
/dng pos2                          # Set second selection corner
/dng saveschem                     # Save selected area to schematic
```

### Information Commands

```
/dng info                          # Show current configuration
/dng list                          # List all dungeons
/dng cancel                        # Cancel editing session
```

## Best Practices

### Building Guidelines

1. **Size Recommendations:**
   - Small: 25x25 to 50x50 blocks
   - Medium: 50x50 to 100x100 blocks
   - Large: 100x100 to 150x150 blocks
   - Keep under 200x200 for performance

2. **Spawn Point:**
   - Place in a safe, open area
   - Away from immediate danger
   - Clear line of sight
   - Room for full party to spawn

3. **Mob Areas:**
   - Designate specific spawn zones
   - Use different heights for variety
   - Create ambush points
   - Plan boss arena separately

4. **Treasure Rooms:**
   - Hidden or challenging to reach
   - Protected by mobs or puzzles
   - Use trigger locations
   - Reward exploration

5. **Exit Points:**
   - Clear path to completion
   - Obvious or quest-based
   - Safe from mob spawns

### WorldEdit Tips

1. **Selection:**
   - Always select corner-to-corner
   - Include ceiling and floor
   - Buffer space around edges
   - Check selection with `//count`

2. **Building:**
   - Use `//set` for large areas
   - `//replace` to swap blocks
   - `//copy` and `//paste` for patterns
   - `//undo` if mistakes happen

3. **Optimization:**
   - Minimize redstone complexity
   - Avoid excessive entities
   - Use simple blocks where possible
   - Test with `/debug start` for lag

### Schematic Management

1. **Naming Convention:**
   - Use lowercase and underscores
   - Descriptive names: `goblin_cave.schem`
   - Version if iterating: `goblin_cave_v2.schem`

2. **Backup:**
   - Keep original schematics backed up
   - Copy before major edits
   - Version control recommended
   - Store separately from server

3. **Organization:**
   - Group by theme or difficulty
   - Document schematic dimensions
   - Note special requirements
   - Track which dungeons use which schematics

### Testing Workflow

1. **Initial Test:**
   - `/dng paste` in test area
   - Walk through entire dungeon
   - Check spawn point location
   - Verify all areas accessible

2. **Trigger Testing:**
   - Note trigger coordinates with F3
   - Stand at trigger points
   - Use `/dng pos1` to mark locations
   - Test trigger radius

3. **Mob Testing:**
   - Verify spawn points are valid
   - Check pathfinding works
   - Test mob difficulty balance
   - Ensure custom mobs load

4. **Performance Testing:**
   - Run with full party
   - Monitor TPS with `/tps`
   - Check for lag sources
   - Optimize if needed

### Common Mistakes to Avoid

1. **Selection Errors:**
   - ❌ Cutting off corners
   - ❌ Missing vertical space
   - ❌ Including too much empty space
   - ✅ Select complete structure with buffer

2. **Coordinate Confusion:**
   - ❌ Using world coordinates for triggers
   - ❌ Forgetting relative positioning
   - ✅ All trigger coordinates relative to spawn

3. **Paste Location:**
   - ❌ Pasting over important structures
   - ❌ Pasting at wrong Y-level
   - ✅ Use flat, empty area for editing

4. **Schematic Save Forgetting:**
   - ❌ Modify blocks, forget `/dng saveschem`
   - ✅ Always save schematic after modifying blocks
   - ℹ️ Configuration auto-saves, but schematics must be saved manually

5. **Reload Missing:**
   - ❌ Make changes, test immediately without reload
   - ✅ Always `/dngadmin reload` after making changes

## Troubleshooting

### Schematic Issues

**"Schematic file not found"**
- Check file is in `plugins/Dungeons/schematics/`
- Verify filename matches (case-sensitive on Linux)
- Ensure `.schem` extension

**"Failed to paste schematic"**
- Check WorldEdit is installed and loaded
- Verify you have enough space
- Check console for WorldEdit errors
- Try pasting in different location

**"Schematic looks wrong"**
- Check you selected the correct area
- Verify you didn't clip corners
- Make sure Y-level is correct
- Redo selection and save again

### Position Selection

**"Positions must be in the same world"**
- Both pos1 and pos2 must be in same world
- Re-set both positions
- Don't teleport between position commands

**"Selection size shows wrong dimensions"**
- Verify you selected opposite corners
- Check F3 coordinates
- Use WorldEdit visual selection if available

**Paste confirmation expired"**
- You have 30 seconds to `/dng confirm`
- Use `/dng paste` again if expired
- Plan your location before pasting

### Configuration

**"Cannot save: validation failed"**
- Check all required fields are set
- Schematic file must exist
- Must have at least one quest
- Must have at least one reward table

**Changes not appearing in-game"**
- Must use `/dngadmin reload` after save
- Check console for reload errors
- Verify file was actually saved

## Advanced Workflows

### Iterative Development

```bash
1. /dng edit my_dungeon
2. /dng paste
3. /dng confirm
4. Make changes
5. /dng pos1 (corner 1)
6. /dng pos2 (corner 2)
7. /dng saveschem
8. Test in separate instance
9. Repeat 3-8 until satisfied
10. /dng save
11. /dngadmin reload
```

### Multiple Versions

Keep different versions during development:

```
my_dungeon_v1.schem - Original
my_dungeon_v2.schem - With new rooms
my_dungeon_v3.schem - Balanced version
my_dungeon.schem - Final/live version
```

Change which version is active:
```
/dng edit my_dungeon
/dng set schematic my_dungeon_v3.schem
/dng save
/dngadmin reload
```

### Team Editing

For multiple editors:

1. One person owns the session (`/dng edit`)
2. Others can help build after paste
3. Session owner does pos1/pos2 and save
4. Clear communication on who saves when
5. Test immediately after save

## See Also

- [Commands Reference](COMMANDS.md) - All editor commands
- [Configuration Guide](CONFIGURATION.md) - YAML format details
- [Custom Mobs](CUSTOM_MOBS.md) - Adding custom mobs to dungeons
- [Triggers](TRIGGERS.md) - Setting up trigger events
