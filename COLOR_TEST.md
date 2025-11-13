# Color Code Test Guide

To test if color codes are working properly after the fix, use these test commands in-game:

## Test Commands

### Test 1: Basic Color Codes
Run: `/dng info`
- Should show colored output with yellow, green, and gray text

### Test 2: Help Command
Run: `/dng`
- Should show the help menu with yellow headers and colored command syntax

### Test 3: Editor Commands
```
/dng create test_colors
```
- Should show green success message
- "&a" should show as green
- "&e" should show as yellow
- "&7" should show as gray

### Test 4: Direct Color Test
If you want to test a specific message, add this temporary command:

```java
// Test in DungeonEditorCommand.java - add this case:
case "colortest":
    MessageUtil.sendRaw(player, "&aGreen &bAqua &cRed &dPink &eYellow &fWhite");
    MessageUtil.sendRaw(player, "&1Dark Blue &2Dark Green &3Dark Aqua &4Dark Red");
    MessageUtil.sendRaw(player, "&5Purple &6Gold &7Gray &8Dark Gray &9Blue");
    MessageUtil.sendRaw(player, "&l&aBold &r&o&cItalic &r&n&eUnderline");
    break;
```

## Expected Behavior

All `&` codes should be properly converted to colored text.

**Before fix:** Some `&` codes appeared as literal `&` symbols
**After fix:** All `&` codes display as proper Minecraft colors

## Common Color Codes

- `&a` = Green (success messages)
- `&c` = Red (error messages)
- `&e` = Yellow (highlights, IDs)
- `&6` = Gold (headers)
- `&7` = Gray (secondary text)
- `&8` = Dark Gray (borders)
- `&f` = White (normal text)
- `&r` = Reset (removes all formatting)
- `&l` = Bold
- `&o` = Italic
- `&n` = Underline

## Hex Color Support

The updated ColorUtil also supports hex colors:
- `&#RRGGBB` format (e.g., `&#FF5555` for light red)
- Example: `&#00FF00This is custom green`
