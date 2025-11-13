# Changelog

All notable changes to the Dungeons plugin.

## [Unreleased]

### Added

#### Quest Management System
- **Complete quest creation via commands** (`/dng quest`)
  - Create quests with 6 different types: KILL_MOBS, KILL_BOSS, COLLECT_ITEMS, REACH_LOCATION, SURVIVE_TIME, INTERACT_BLOCKS
  - Set quest properties: name, description, required status, progress display, bonus rewards
  - Add objectives with type-specific parameters
  - View and list all configured quests
  - Delete quests
  - Full save/load support to `quests.yml`
- **Quest Types Supported:**
  - `KILL_MOBS` - Kill specific mob types with amount
  - `KILL_BOSS` - Kill custom boss mobs
  - `COLLECT_ITEMS` - Collect specific materials
  - `REACH_LOCATION` - Reach a location (uses targeted block)
  - `SURVIVE_TIME` - Survive for a duration in seconds
  - `INTERACT_BLOCKS` - Interact with specific block types
- Commands: `/dng quest create`, `/dng quest set`, `/dng quest objective add/list`, `/dng quest view/list/delete`

#### Reward Management System
- **Complete reward system via commands and GUI** (`/dng reward`)
  - Add items with amount ranges and drop chances
  - Set money rewards (single value or ranges like "100-250")
  - Set experience rewards
  - View rewards in interactive GUI
  - Remove rewards via right-click in GUI
  - Full tab completion for all Minecraft materials
  - Auto-saves to `rewards.yml`
- **Reward Manager GUI**
  - Visual display of all items in reward table
  - Shows money and experience rewards
  - Right-click to remove items/money/experience
  - Real-time save on changes
- Commands: `/dng reward add item`, `/dng reward set money/exp`, `/dng reward view/list`

#### Trigger Action Commands
- **Add trigger actions via command line** (`/dng trigger add action`)
  - Faster alternative to GUI for adding actions
  - Auto-generated action IDs
  - Uses targeted block for location-based actions (SPAWN_MOB, DROP_ITEM, TELEPORT)
  - Creates actions with sensible defaults
  - All 7 action types supported: SPAWN_MOB, DROP_ITEM, TELEPORT, DAMAGE_PLAYER, MESSAGE, COMMAND, POTION_EFFECT
- Requires selecting trigger first with `/dng trigger set <id>`
- Actions can be customized further in GUI

#### Economy Integration
- **Entry cost system** for dungeons (requires Vault)
  - Set dungeon entry cost with `/dng set cost <amount>`
  - Displays in `/dungeon info` command
  - Checks balance before allowing dungeon entry
  - Withdraws cost from party leader
  - Shows formatted money amounts using Vault
  - Graceful degradation when Vault unavailable
- Config option: `economy.enabled` and `economy.default-entry-cost`

#### Permissions
- **Cooldown bypass permission**: `dungeons.bypass.cooldown`
  - Allows players (or party leaders) to bypass dungeon cooldowns
  - Default: `op`
  - Inherited by `dungeons.admin` permission
  - Useful for testing and VIP players

### Fixed

#### GUI System
- **Fixed inventory click handling**
  - Player inventory clicks no longer trigger GUI handlers
  - Fixed armor slots not working in Mob Editor GUI
  - Changed from `event.getInventory()` to `event.getClickedInventory()` for proper inventory detection
  - All GUIs now properly distinguish between top (GUI) and bottom (player inventory) clicks

#### Color Code System
- **Enhanced color code rendering**
  - Fixed `&` color codes not rendering properly
  - Configured `LegacyComponentSerializer` with full support
  - Added hex color support: `&#RRGGBB`
  - Added unusual hex format support: `&x&r&r&g&g&b&b`
  - All messages now render colors correctly

### Changed

#### Difficulty System
- **Simplified difficulty field**
  - Changed from enum with multipliers to freeform text field
  - Removed mob health/damage multipliers from config
  - Deleted `DifficultyLevel.java` enum
  - Removed `difficulty.multipliers` section from config.yml
  - Difficulty now purely descriptive (EASY, MEDIUM, HARD, or custom text)

#### Message System
- **Complete message audit performed**
  - Reviewed 400+ chat messages across all files
  - Fixed minor wording issues
  - Verified all color code usage
  - Created `MESSAGE_AUDIT.md` documentation

### Documentation

#### New Documentation
- **CHANGELOG.md** - This file
- **MESSAGE_AUDIT.md** - Complete audit of all chat messages
- **COLOR_TEST.md** - Color code testing guide
- **COST_AND_DIFFICULTY_IMPLEMENTATION.md** - Implementation details for cost/difficulty system

#### Updated Documentation
- **docs/COMMANDS.md** - Complete rewrite with all new commands
  - Added Quest Management section
  - Added Reward Management section
  - Added Trigger Action commands
  - Updated dungeon properties
  - Updated tab completion examples
  - Updated permissions section
  - Added entry cost to dungeon info display

### Technical Details

#### New Files
- `VaultEconomyIntegration.java` - Vault economy integration
- `RewardManagerGUI.java` - GUI for managing reward tables
- Extended `QuestManager.java` with save/load methods
- Extended `RewardManager.java` with manipulation methods

#### Modified Files
- `DungeonEditorCommand.java` - Added quest, reward, and trigger action commands
- `DungeonCommand.java` - Added entry cost checking
- `GUIManager.java` - Fixed click handling
- `ColorUtil.java` - Enhanced color code support
- `QuestManager.java` - Added save methods
- `RewardManager.java` - Added manipulation and save methods

#### Config Changes
- Added `economy.enabled` and `economy.default-entry-cost`
- Removed `difficulty.multipliers` section
- All quest and reward data now saves to respective YAML files

### Migration Notes

#### For Server Admins
- **Vault Required** for entry cost feature (optional)
- **No breaking changes** to existing configurations
- Difficulty multipliers removed but difficulty text field remains
- All existing dungeons, quests, and rewards remain compatible

#### For Developers
- `DifficultyLevel` enum removed - use String for difficulty
- `GUIManager` now properly handles click events - no changes needed
- All GUI classes now require `DungeonsPlugin` parameter in constructor

---

## See Also

- [Commands Reference](docs/COMMANDS.md) - Complete command documentation
- [Setup Guide](SETUP.md) - Installation and configuration
- [Editor Workflow](docs/EDITOR_WORKFLOW.md) - Dungeon creation guide
