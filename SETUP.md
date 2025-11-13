# Dungeons Plugin - Setup Guide

## Requirements

- **Minecraft Server**: Paper 1.21.4 or higher
- **Java**: Java 21
- **WorldEdit**: 7.4.0+ required for MC 1.21.4 (see WORLDEDIT.md for details)
- **Vault** (Optional): For economy integration
- **Maven**: For building the plugin

⚠️ **Important**: WorldEdit 7.3.x does NOT support MC 1.21.4. You must use WorldEdit 7.4.0 or newer!

## Building the Plugin

```bash
# Navigate to project directory
cd Dungeons

# Build with Maven
mvn clean package

# Output will be in target/Dungeons-1.0.0.jar
```

## Installation

1. **Download Dependencies**:
   - Install WorldEdit on your server
   - (Optional) Install Vault and an economy plugin

2. **Install Plugin**:
   ```bash
   # Copy JAR to plugins folder
   cp target/Dungeons-1.0.0.jar /path/to/server/plugins/
   ```

3. **Start Server**:
   - The plugin will create default configuration files
   - A dungeon world will be auto-created on first start

4. **Add Schematics**:
   - Create your dungeons in-game using WorldEdit
   - Save schematics: `//copy` then `//schem save <name>`
   - Place `.schem` files in `plugins/Dungeons/schematics/`

5. **Configure Dungeons**:
   - Each dungeon has its own file in `plugins/Dungeons/dungeons/`
   - Edit existing dungeon files or create new ones (e.g., `my_dungeon.yml`)
   - Configure quests in `quests.yml`
   - Set up rewards in `rewards.yml`

6. **Reload Plugin**:
   ```
   /dngadmin reload
   ```

## Quick Start

### For Players

1. **Create a Party**:
   ```
   /dngparty create
   /dngparty invite <player>
   ```

2. **View Available Dungeons**:
   ```
   /dungeon list
   /dungeon info <dungeon>
   ```

3. **Start a Dungeon**:
   ```
   /dungeon start <dungeon>
   ```

4. **Leave Dungeon**:
   ```
   /dungeon leave
   ```

### For Admins

1. **View Statistics**:
   ```
   /dngadmin stats
   ```

2. **List Active Instances**:
   ```
   /dngadmin list
   ```

3. **Reset Player Cooldowns**:
   ```
   /dngadmin reset player <player>
   ```

4. **Reload Configuration**:
   ```
   /dngadmin reload
   ```

## Configuration Files

### config.yml
Main configuration for grid system, party settings, and database.

### dungeons/*.yml
Each dungeon has its own configuration file in the `dungeons/` folder:
- `id` - Unique dungeon identifier
- `display-name` - Name shown to players
- `schematic-file` - Reference to .schem file
- `party-size` - Min/max party members
- `time-limit` - Duration in seconds
- `spawn-offset` - Where players spawn (x, y, z offset from schematic origin)
- `quests` - List of quest IDs
- `rewards` - List of reward table IDs
- `cooldown` - Seconds before re-run
- `description` - Info shown to players

**Example**: `dungeons/goblin_cave.yml`

### quests.yml
Quest definitions with objectives:
- KILL_MOBS - Kill specific mob types
- KILL_BOSS - Defeat a boss
- COLLECT_ITEMS - Gather items
- REACH_LOCATION - Navigate to coordinates
- SURVIVE_TIME - Stay alive for duration
- INTERACT_BLOCKS - Interact with blocks

### rewards.yml
Reward tables with:
- Items with enchantments and custom names
- Money rewards (requires Vault)
- Experience points
- Command execution

### messages.yml
Customizable messages and localization.

## Database

The plugin uses HikariCP for connection pooling and supports:

### SQLite (Default)
- File-based database
- No additional setup required
- Data stored in `plugins/Dungeons/data.db`

### MySQL (Optional)
Configure in config.yml:
```yaml
database:
  type: mysql
  mysql:
    host: "localhost"
    port: 3306
    database: "dungeons"
    username: "root"
    password: "password"
```

## Permissions

- `dungeons.party` - Create and manage parties (default: true)
- `dungeons.enter` - Enter dungeons (default: true)
- `dungeons.enter.<dungeon>` - Access specific dungeon
- `dungeons.admin` - Admin commands (default: op)
- `dungeons.bypass.cooldown` - Bypass cooldowns (default: op)

## Troubleshooting

### "Editing on unsupported versions is disabled" Error
**Problem:** WorldEdit 7.3.x doesn't support Minecraft 1.21.4

**Solution:**
1. Download WorldEdit 7.4.0+ from https://dev.bukkit.org/projects/worldedit
2. Remove old WorldEdit: `rm plugins/worldedit-bukkit-7.3*.jar`
3. Install new version in `plugins/` folder
4. Restart server
5. See `WORLDEDIT.md` for detailed instructions

**Fallback:** Plugin will work without WorldEdit by creating basic platforms

### Schematic Not Loading
- Verify `.schem` file exists in `plugins/Dungeons/schematics/`
- Check WorldEdit 7.4.0+ is installed and running
- Ensure schematic filename in dungeons.yml matches actual file
- Test WorldEdit independently: `//wand` in-game

### No Available Slots
- Increase `grid.max-slots` in config.yml
- Check for abandoned instances: `/dngadmin list`
- Reset stuck dungeons: `/dngadmin reset dungeon <dungeon>`

### Database Errors
- Check database credentials in config.yml
- Verify database server is running (MySQL)
- Check file permissions (SQLite)
- Review logs for detailed error messages

### Cooldown Issues
- Reset player cooldowns: `/dngadmin reset player <player>`
- Bypass with permission: `dungeons.bypass.cooldown`
- Adjust cooldown values in dungeons.yml

## Support

For issues and feature requests:
- Check the logs in `logs/latest.log`
- Use `/dngadmin stats` to view system status
- Read `WORLDEDIT.md` for WorldEdit compatibility issues
- Report bugs with full error logs and configuration

## Features Overview

✅ **Grid-Based Instancing** - Efficient spatial isolation
✅ **Party System** - Full party management with invitations
✅ **WorldEdit Integration** - Load dungeons from schematics
✅ **Quest System** - 6 quest types with progress tracking
✅ **Reward System** - Items, money, XP, commands
✅ **Database Persistence** - SQLite or MySQL support
✅ **Cooldown System** - Per-dungeon, per-player cooldowns
✅ **Auto-Monitoring** - Timeout detection and completion
✅ **Full Localization** - Customizable messages
✅ **Admin Tools** - Management and debugging commands

## Architecture

- **Grid System**: Dungeon instances placed in grid slots in separate world
- **Party-Based**: One instance per party, shared progress
- **Asynchronous**: Schematic pasting and cleanup run async
- **Thread-Safe**: Concurrent collections for multi-threading
- **Event-Driven**: Quest progress tracked via Bukkit events
- **Stateless Commands**: Clean command handler architecture

Enjoy your dungeon adventures!
