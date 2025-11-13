# Visual GUI Editors

Guide to using the in-game visual editors for mobs, triggers, and actions.

## Table of Contents
- [Mob Editor GUI](#mob-editor-gui)
- [Trigger List GUI](#trigger-list-gui)
- [Trigger Editor GUI](#trigger-editor-gui)
- [Action Editor GUI](#action-editor-gui)
- [Tips & Shortcuts](#tips--shortcuts)

## Mob Editor GUI

Edit custom mob properties, equipment, and effects visually.

### Opening the Mob Editor

Currently integrated into dungeon editing workflow. Access methods TBD based on implementation.

### Interface Layout

```
┌─────────────────────────────────────────────────┐
│              Edit Mob: goblin_king              │
├─────────────────────────────────────────────────┤
│                                                 │
│    [Mob Type]    [Name]    [Health]  [Damage]  │
│     [Speed]                 [Effects]           │
│                                                 │
│    Equipment:                                   │
│    [Helmet]   [Chestplate] [Leggings] [Boots]  │
│    [MainHand] [OffHand]                         │
│                                                 │
│    [SAVE]                            [CANCEL]   │
└─────────────────────────────────────────────────┘
```

### Editing Properties

**Mob Type:**
- Click to see available entity types
- ZOMBIE, SKELETON, CREEPER, etc.
- Changes mob base stats

**Custom Name:**
- Left-click to set name via chat
- Right-click to toggle visibility
- Supports & color codes

**Stats (Health, Damage, Speed):**
- Click to enter value via chat
- Enter number when prompted
- -1 or blank = use default

**Equipment:**
- Drag and drop items from inventory
- Click to place current held item
- Right-click to remove

**Potion Effects:**
- Click "Potion Effects" button
- Opens effect selection GUI
- Click effects to toggle on/off
- Active effects shown in green

### Saving

**Save Button:**
- Saves mob to dungeon configuration
- Writes to YAML file immediately
- Can continue editing other mobs

**Cancel Button:**
- Discards all changes
- Returns to previous screen

**Auto-save:**
- Equipment auto-saved on GUI close
- Configuration saved when clicking SAVE

## Trigger List GUI

View and manage all triggers for a dungeon.

### Interface Layout

```
┌─────────────────────────────────────────────────┐
│          Triggers: goblin_cave                  │
├─────────────────────────────────────────────────┤
│                                                 │
│  [Compass]  ambush_chamber                      │
│  Type: LOCATION                                 │
│  Actions: 2, Repeatable: No                     │
│  Left-Click: Edit | Right-Click: Delete        │
│                                                 │
│  [Clock]  time_warning                          │
│  Type: TIMER                                    │
│  Actions: 1, Repeatable: No                     │
│  Left-Click: Edit | Right-Click: Delete        │
│                                                 │
│  [Sword]  boss_arena                            │
│  Type: LOCATION                                 │
│  Actions: 4, Repeatable: No                     │
│  Left-Click: Edit | Right-Click: Delete        │
│                                                 │
│  [CREATE NEW TRIGGER]              [BACK]       │
└─────────────────────────────────────────────────┘
```

### Actions

**Left-Click Trigger:**
- Opens Trigger Editor GUI
- Edit all trigger properties
- Manage action list

**Right-Click Trigger:**
- Deletes trigger
- Requires confirmation
- Saves immediately

**Create New Trigger:**
- Opens chat input
- Enter unique trigger ID
- Creates default LOCATION trigger
- Opens editor automatically

## Trigger Editor GUI

Edit individual trigger properties and conditions.

### Interface Layout

```
┌─────────────────────────────────────────────────┐
│       Edit Trigger: boss_arena                  │
├─────────────────────────────────────────────────┤
│                                                 │
│  [Trigger Type: LOCATION]                       │
│                                                 │
│  Condition:                                     │
│  [X: 50.0]  [Y: 5.0]  [Z: 50.0]  [Radius: 8.0] │
│                                                 │
│  [Repeatable: OFF]  [Cooldown: 0]               │
│                                                 │
│  [Actions: 4]  ← Click to manage                │
│                                                 │
│  [SAVE]                            [BACK]       │
└─────────────────────────────────────────────────┘
```

### Editing Trigger Type

**Click "Trigger Type":**
- Shows all available types
- LOCATION, TIMER, MOB_KILL, etc.
- Click to select
- Condition fields update automatically

### Editing Conditions

Condition fields change based on trigger type:

**LOCATION:**
- X, Y, Z coordinates
- Radius in blocks

**TIMER:**
- Time in seconds

**MOB_KILL:**
- Mob type (optional)
- Kill count

**QUEST_COMPLETE:**
- Quest ID

**PLAYER_DEATH / BOSS_KILL:**
- No condition fields

**Entering Values:**
- Click a field
- Enter value via chat
- Validates automatically
- Updates display

### Managing Properties

**Repeatable Toggle:**
- Click to toggle on/off
- Green = repeatable
- Gray = one-time only

**Cooldown:**
- Click to enter seconds
- Only applies if repeatable
- 0 = no cooldown

**Actions Button:**
- Shows current action count
- Click to open Action List GUI

## Action List GUI

View and manage all actions for a trigger.

### Interface Layout

```
┌─────────────────────────────────────────────────┐
│       Actions: boss_arena                       │
├─────────────────────────────────────────────────┤
│                                                 │
│  1. [Zombie] SPAWN_MOB                          │
│     Custom Mob: goblin_king                     │
│     Count: 1                                    │
│     Shift+Click: Move | Right-Click: Delete     │
│                                                 │
│  2. [Zombie] SPAWN_MOB                          │
│     Custom Mob: goblin_warrior                  │
│     Count: 3                                    │
│     Shift+Click: Move | Right-Click: Delete     │
│                                                 │
│  3. [Paper] MESSAGE                             │
│     "BOSS FIGHT! The Goblin King appears!"      │
│     Shift+Click: Move | Right-Click: Delete     │
│                                                 │
│  4. [Command] COMMAND                           │
│     playsound minecraft:entity.ender_dragon...  │
│     Shift+Click: Move | Right-Click: Delete     │
│                                                 │
│  [ADD ACTION]                         [BACK]    │
└─────────────────────────────────────────────────┘
```

### Managing Actions

**Left-Click Action:**
- Opens Action Editor
- Edit all action properties
- Save automatically

**Right-Click Action:**
- Deletes action
- Confirmation required
- Saves immediately

**Shift+Left-Click:**
- Moves action up in order
- First action can't move up

**Shift+Right-Click:**
- Moves action down in order
- Last action can't move down

**Add Action:**
- Opens action type selector
- Choose action type
- Creates with default values
- Opens editor

**Action Order:**
- Actions execute top to bottom
- Reorder to change execution
- MESSAGE before SPAWN_MOB warns players
- Multiple SPAWN_MOB actions spawn simultaneously

## Action Editor GUI

Edit specific action properties.

### Interface varies by action type

#### MESSAGE Action

```
┌─────────────────────────────────────────────────┐
│         Edit Action #1                          │
├─────────────────────────────────────────────────┤
│  Type: MESSAGE                                  │
│                                                 │
│  [Message]                                      │
│  "&4&lBOSS FIGHT! The King appears!"            │
│  Click to edit                                  │
│                                                 │
│  [SAVE]                            [BACK]       │
└─────────────────────────────────────────────────┘
```

#### SPAWN_MOB Action

```
┌─────────────────────────────────────────────────┐
│         Edit Action #2                          │
├─────────────────────────────────────────────────┤
│  Type: SPAWN_MOB                                │
│                                                 │
│  [Custom Mob ID: goblin_king]                   │
│  [Mob Type: ZOMBIE]  (if no custom mob)         │
│  [Count: 1]                                     │
│  [X: 50.0]  [Y: 5.0]  [Z: 50.0]                 │
│                                                 │
│  [SAVE]                            [BACK]       │
└─────────────────────────────────────────────────┘
```

#### POTION_EFFECT Action

```
┌─────────────────────────────────────────────────┐
│         Edit Action #3                          │
├─────────────────────────────────────────────────┤
│  Type: POTION_EFFECT                            │
│                                                 │
│  [Effect Type: POISON]                          │
│  [Duration: 10 seconds]                         │
│  [Amplifier: 1]  (Level 2)                      │
│                                                 │
│  [SAVE]                            [BACK]       │
└─────────────────────────────────────────────────┘
```

#### Other Action Types

- **COMMAND**: Command field (no /)
- **TELEPORT**: X, Y, Z coordinates
- **DAMAGE_PLAYER**: Damage amount
- **DROP_ITEM**: Material, amount, X, Y, Z

### Chat Input

When you click a field:
1. GUI closes
2. Prompt appears in chat
3. Type your value
4. Press Enter
5. GUI reopens with new value

**Example:**
```
Click [Message]
> Enter message (use & for color codes):
> &c&lDANGER AHEAD!
✓ Value updated
> Opens GUI again
```

**Special Keywords:**
- Type `cancel` to cancel input
- Invalid input shows error
- Returns to GUI automatically

## Tips & Shortcuts

### Navigation

- **ESC key** - Close current GUI
- **Click air** - Sometimes closes GUI
- **Click BACK** - Return to previous screen
- **Click CANCEL** - Discard changes

### Efficiency

1. **Batch Editing:**
   - Edit multiple mobs before saving dungeon
   - Changes save per-mob automatically
   - Configuration saves when closing editor

2. **Copy Equipment:**
   - Open mob editor
   - Place equipment in slots
   - Save mob
   - Open new mob, repeat

3. **Trigger Duplication:**
   - Can't copy triggers directly
   - Note coordinates and actions
   - Create new trigger manually
   - Faster than YAML sometimes

### Color Codes

When entering text with colors:
```
&0 - Black          &8 - Dark Gray
&1 - Dark Blue      &9 - Blue
&2 - Dark Green     &a - Green
&3 - Dark Aqua      &b - Aqua
&4 - Dark Red       &c - Red
&5 - Dark Purple    &d - Light Purple
&6 - Gold           &e - Yellow
&7 - Gray           &f - White

&l - Bold           &n - Underline
&m - Strikethrough  &o - Italic
&r - Reset          &k - Magic
```

Example:
```
&6&l[BOSS] &c&nGoblin King
Result: [BOSS] Goblin King (gold bold, red underlined)
```

### Validation

The GUIs validate input automatically:
- Numbers must be valid doubles/integers
- Materials must exist (DIAMOND, GOLD_INGOT)
- Mob types must be valid (ZOMBIE, SKELETON)
- Effect names must be valid (speed, strength)
- Custom mob IDs must exist in dungeon

### Error Handling

If you enter invalid input:
```
> Enter health value:
> abc
✗ Invalid number!
> Opens GUI again (no change)
```

### Safety Features

1. **Auto-save Equipment:**
   - Closing mob editor saves equipment
   - No need to click SAVE
   - Prevents losing placed items

2. **Confirmation Dialogs:**
   - Deleting triggers requires confirmation
   - Deleting actions requires confirmation
   - Can't accidentally delete

3. **Session Persistence:**
   - Editor state persists between reloads
   - Can logout and resume editing
   - Changes saved incrementally

## Keyboard Controls

**While GUI is open:**
- `ESC` - Close GUI
- `E` - Close inventory/GUI
- `1-9` - Quick-select hotbar (for placing items)
- `Shift+Click` - Reorder actions
- `Ctrl+Q` - Drop item (don't do in GUI!)

**While entering chat:**
- `Enter` - Submit input
- `ESC` - Cancel (same as typing "cancel")
- `Up Arrow` - Previous command (if available)

## Troubleshooting

**GUI won't open:**
- Check you have `dungeons.editor` permission
- Verify you're in editing session (`/dng edit <id>`)
- Try closing other GUIs first
- Check console for errors

**Chat input not working:**
- Make sure you press Enter
- Don't include commands (/, etc.)
- Check for typos in numbers
- Use "cancel" to abort

**Items disappearing:**
- Click SAVE before closing
- Equipment auto-saves on close
- Don't drop items (Ctrl+Q) in GUI

**Can't place equipment:**
- Make sure you're in equipment slot
- Try clicking with item in hand
- Check slot isn't protected
- Some slots may have restrictions

**Changes not saving:**
- Click SAVE button
- Check console for errors
- Verify you have edit permission
- Make sure dungeon editing session active

## Best Practices

1. **Start Simple:**
   - Create basic mob first
   - Add equipment gradually
   - Test before adding effects

2. **Test Frequently:**
   - Save and reload often
   - Test in-game after changes
   - Verify triggers work

3. **Organize Actions:**
   - Use logical order
   - Group related actions
   - MESSAGE before SPAWN_MOB

4. **Document:**
   - Use descriptive mob IDs
   - Use clear trigger names
   - Add comments in YAML if needed

5. **Backup:**
   - Save dungeon config before major changes
   - Keep old schematic versions
   - Test in copy before editing live

## See Also

- [Editor Workflow](EDITOR_WORKFLOW.md) - Complete editing guide
- [Custom Mobs](CUSTOM_MOBS.md) - Mob configuration details
- [Triggers](TRIGGERS.md) - Trigger system reference
- [Commands](COMMANDS.md) - All available commands
