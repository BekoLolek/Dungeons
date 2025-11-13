# WorldEdit Integration Guide

## Overview

The Dungeons plugin can use WorldEdit to load schematic files. However, there are version compatibility requirements.

## WorldEdit Version Requirements

### For Minecraft 1.21.4

⚠️ **Current Status**: WorldEdit support for MC 1.21.4 is **LIMITED**

- ⚠️ **WorldEdit 7.4.0-beta** - Partial support, may show "unsupported version" errors
- ❌ **WorldEdit 7.3.x** - Does NOT support MC 1.21+ (will show "unsupported version" error)
- ✅ **Fallback Mode** - Plugin works without WorldEdit (creates basic platforms)

### Why the Issues?

Minecraft 1.21.4 is very new and WorldEdit development takes time to catch up. Even beta versions may not fully support all features yet.

### Download WorldEdit

Get the latest version from:
- **Official:** https://dev.bukkit.org/projects/worldedit
- **GitHub:** https://github.com/EngineHub/WorldEdit/releases
- **Modrinth:** https://modrinth.com/plugin/worldedit

## Installation

1. Download WorldEdit 7.4.0+ for Bukkit/Paper
2. Place `worldedit-bukkit-7.4.x.jar` in your `plugins/` folder
3. Restart your server
4. WorldEdit should load successfully

## Common Issues

### Error: "Editing on unsupported versions is disabled"

**Cause:** You're using WorldEdit 7.3.x on Minecraft 1.21.4

**Solution:** Upgrade to WorldEdit 7.4.0+

```bash
# Remove old version
rm plugins/worldedit-bukkit-7.3*.jar

# Download and install WorldEdit 7.4.0+
# Then restart server
```

### WorldEdit Not Found

If WorldEdit is not installed or fails to load, the plugin will use a **fallback mode**:

- Creates basic stone platforms instead of pasting schematics
- Dungeons will still function but won't have custom layouts
- You'll see warnings in console about fallback mode

## Creating Schematics

Once WorldEdit 7.4+ is installed:

1. **Build your dungeon** in-game
2. **Select the area** with WorldEdit wand (`//wand`)
3. **Copy the selection**: `//copy`
4. **Save as schematic**: `//schem save dungeon_name`
5. **Find the file** in `plugins/WorldEdit/schematics/`
6. **Move to plugin**: Copy the `.schem` file to `plugins/Dungeons/schematics/`

## Alternative: Native Structure Blocks

If you can't use WorldEdit, you can use Minecraft's native structure blocks:

1. Save your dungeon using structure blocks
2. Place the `.nbt` files in a custom location
3. Use `/structure load` command to paste manually
4. The plugin will still handle instance isolation and quests

## Fallback Mode

If WorldEdit is unavailable, the plugin will:

1. ✅ Still create dungeon instances
2. ✅ Still track quests and rewards
3. ✅ Still manage parties
4. ⚠️ Create basic platforms instead of custom dungeons
5. ⚠️ Show warnings in console

## Testing WorldEdit Integration

After installing WorldEdit, test it works:

```bash
# In-game as op
//wand

# You should get a wooden axe
# If you see "unsupported version" error, upgrade WorldEdit
```

## Recommended Setup

For **best experience** on Minecraft 1.21.4:

```
Paper 1.21.4
+ WorldEdit 7.4.0+
+ Dungeons Plugin
```

This combination provides:
- Full schematic support
- Fast pasting/clearing
- No API errors
- Complete feature set

## Version Compatibility Table

| Minecraft Version | WorldEdit Version | Status |
|-------------------|-------------------|--------|
| 1.21.4 | 7.4.0+ | ✅ Fully Supported |
| 1.21.4 | 7.3.x | ❌ Not Supported |
| 1.20.x | 7.3.0+ | ✅ Supported |
| 1.19.x | 7.2.x | ✅ Supported |

## Support

If you continue to have issues:

1. Check WorldEdit version: `/version WorldEdit`
2. Check for errors in `logs/latest.log`
3. Verify `.schem` files are in correct folder
4. Test WorldEdit independently first

The plugin will always work, with or without WorldEdit - you'll just get basic platforms instead of custom dungeons in fallback mode.
