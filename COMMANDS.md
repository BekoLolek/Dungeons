# Dungeons Plugin - Command Reference

## 🎮 Player Commands

### `/dngparty` - Party Management
Main command for creating and managing parties. Aliases: `/party`, `/p`

**Permission:** `dungeons.party` (default: true)

| Command | Description |
|---------|-------------|
| `/dngparty create` | Create a new party |
| `/dngparty invite <player>` | Invite a player to your party (leader only) |
| `/dngparty accept` | Accept the most recent party invitation |
| `/dngparty kick <player>` | Kick a player from your party (leader only) |
| `/dngparty leave` | Leave your current party |
| `/dngparty disband` | Disband your party (leader only) |
| `/dngparty list` | List all members in your party |

---

### `/dungeon` - Dungeon Access
Main command for entering and managing dungeons. Aliases: `/d`, `/dung`

**Permission:** `dungeons.enter` (default: true)

| Command | Description |
|---------|-------------|
| `/dungeon list` | View all available dungeons you can enter |
| `/dungeon start <dungeon>` | Start a dungeon with your party (leader only) |
| `/dungeon leave` | Leave your current dungeon |
| `/dungeon info <dungeon>` | View detailed information about a dungeon |

---

## 🛠️ Admin Commands

### `/dngadmin` - Administrative Tools
Admin commands for managing the plugin. Aliases: `/dungeonadmin`, `/da`, `/dadmin`

**Permission:** `dungeons.admin` (default: op)

| Command | Description |
|---------|-------------|
| `/dngadmin reload` | Reload all configuration files |
| `/dngadmin reset player <player>` | Clear all cooldowns for a player |
| `/dngadmin reset dungeon <dungeon>` | Force close all instances of a dungeon |
| `/dngadmin list` | List all active dungeon instances |
| `/dngadmin teleport <instance-id>` | Teleport to a specific dungeon instance |
| `/dngadmin stats` | View plugin statistics (grid, parties, instances, content) |

---

## 📊 Command Summary

| Command Group | Main Command | Aliases | Sub-commands | Permission |
|--------------|--------------|---------|--------------|------------|
| Party | `/dngparty` | `/party`, `/p` | 7 | `dungeons.party` |
| Dungeon | `/dungeon` | `/d`, `/dung` | 4 | `dungeons.enter` |
| Admin | `/dngadmin` | `/dungeonadmin`, `/da`, `/dadmin` | 6 | `dungeons.admin` |

**Total:** 3 main commands, 17 sub-commands

---

## 🎯 Quick Examples

### Starting a Dungeon Run
```bash
# Create party
/dngparty create

# Invite friends
/dngparty invite Steve
/dngparty invite Alex

# They accept
/party accept   # Using alias

# Check available dungeons
/dungeon list
/d info goblin_cave   # Using alias

# Start the dungeon (as party leader)
/d start goblin_cave
```

### Admin Tasks
```bash
# View current status
/dngadmin stats

# See active instances
/dngadmin list

# Reset player cooldown
/dngadmin reset player Steve

# Reload after config changes
/dngadmin reload
```

---

## 🔒 Permissions

All commands have built-in permission checks:

- **Public Commands**: `dungeons.party`, `dungeons.enter`
- **Admin Commands**: `dungeons.admin`
- **Dungeon-Specific**: `dungeons.enter.<dungeon_id>`
- **Special**: `dungeons.bypass.cooldown`

---

## ✨ Features

- ✅ Full tab completion on all commands
- ✅ Helpful error messages
- ✅ Permission checks
- ✅ Alias support for faster typing
- ✅ Player name autocomplete
- ✅ Dungeon ID autocomplete

---

## 📝 Notes

- Party invitations expire after 60 seconds (configurable)
- Only party leaders can start dungeons
- Cooldowns are per-player, per-dungeon
- Admins can bypass cooldowns with `dungeons.bypass.cooldown` permission
- All commands support color codes in messages
