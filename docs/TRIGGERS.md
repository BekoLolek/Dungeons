# Triggers & Actions Guide

Triggers are event-based systems that execute actions when certain conditions are met during a dungeon run.

## Table of Contents
- [Trigger Types](#trigger-types)
- [Action Types](#action-types)
- [Configuration Examples](#configuration-examples)
- [GUI Editor](#gui-editor)
- [Advanced Techniques](#advanced-techniques)

## Trigger Types

### LOCATION
Triggers when a player enters a specific area.

**Condition Properties:**
- `x`, `y`, `z` - Center point coordinates (relative to dungeon spawn)
- `radius` - Trigger radius in blocks

```yaml
triggers:
  ambush_room:
    type: LOCATION
    condition:
      x: 25.0
      y: 5.0
      z: 30.0
      radius: 5.0        # Triggers within 5 blocks
    repeatable: false
    cooldown: 0
    actions:
      - type: MESSAGE
        message: "&c&lAMBUSH!"
```

### TIMER
Triggers after a specific amount of time has elapsed.

**Condition Properties:**
- `time` - Seconds elapsed since dungeon start

```yaml
triggers:
  time_warning:
    type: TIMER
    condition:
      time: 300          # After 5 minutes (300 seconds)
    repeatable: false
    actions:
      - type: MESSAGE
        message: "&e&lWARNING: &725 minutes remaining!"
```

### MOB_KILL
Triggers when a certain number of mobs are killed.

**Condition Properties:**
- `mob-count` - Number of mobs to kill
- `mob-type` - Optional: Specific mob type (e.g., ZOMBIE)

```yaml
triggers:
  mob_milestone:
    type: MOB_KILL
    condition:
      mob-count: 20
      mob-type: ZOMBIE    # Optional: specific type
    repeatable: false
    actions:
      - type: MESSAGE
        message: "&a&lMilestone! &720 zombies defeated!"
```

### QUEST_COMPLETE
Triggers when a specific quest is completed.

**Condition Properties:**
- `quest-id` - Quest identifier

```yaml
triggers:
  quest_reward:
    type: QUEST_COMPLETE
    condition:
      quest-id: "goblin_slayer"
    repeatable: false
    actions:
      - type: DROP_ITEM
        material: DIAMOND
        amount: 3
        x: 25.0
        y: 5.0
        z: 25.0
```

### PLAYER_DEATH
Triggers when any player dies in the dungeon.

**No condition properties required**

```yaml
triggers:
  death_penalty:
    type: PLAYER_DEATH
    repeatable: true     # Each death
    cooldown: 0
    actions:
      - type: MESSAGE
        message: "&c&lA player has fallen!"
```

### BOSS_KILL
Triggers when a boss mob is defeated.

**Condition Properties:**
- `boss-id` - Boss identifier (custom mob ID)

```yaml
triggers:
  boss_defeated:
    type: BOSS_KILL
    condition:
      boss-id: "goblin_king"
    repeatable: false
    actions:
      - type: MESSAGE
        message: "&a&lVICTORY! &7The boss has been defeated!"
```

## Action Types

### MESSAGE
Send a message to all players in the dungeon.

**Properties:**
- `message` - Text to display (supports & color codes)

```yaml
- type: MESSAGE
  message: "&6&lATTENTION: &eThe boss has awakened!"
```

### SPAWN_MOB
Spawn vanilla or custom mobs at a location.

**Properties:**
- `mob-type` - Vanilla mob type (ZOMBIE, SKELETON, etc.)
- `custom-mob-id` - Custom mob ID (overrides mob-type)
- `count` - Number of mobs to spawn
- `x`, `y`, `z` - Spawn coordinates

```yaml
# Vanilla mob
- type: SPAWN_MOB
  mob-type: ZOMBIE
  count: 5
  x: 30.0
  y: 5.0
  z: 25.0

# Custom mob
- type: SPAWN_MOB
  custom-mob-id: goblin_king
  count: 1
  x: 50.0
  y: 5.0
  z: 50.0
```

### COMMAND
Execute a console command.

**Properties:**
- `command` - Command to execute (without /)

**Placeholders:**
- `@a` - All players in dungeon
- `@p` - Triggering player (location triggers)

```yaml
- type: COMMAND
  command: "playsound minecraft:entity.ender_dragon.growl master @a"

- type: COMMAND
  command: "title @a times 10 70 20"

- type: COMMAND
  command: "title @a title {\"text\":\"BOSS FIGHT\",\"color\":\"red\"}"
```

### TELEPORT
Teleport all dungeon players to a location.

**Properties:**
- `x`, `y`, `z` - Target coordinates

```yaml
- type: TELEPORT
  x: 100.0
  y: 10.0
  z: 100.0
```

### DAMAGE_PLAYER
Deal damage to players.

**Properties:**
- `damage` - Damage amount (hearts)

```yaml
- type: DAMAGE_PLAYER
  damage: 4.0          # 2 hearts
```

### DROP_ITEM
Drop items at a location.

**Properties:**
- `material` - Item material (e.g., DIAMOND, GOLD_INGOT)
- `amount` - Stack size
- `x`, `y`, `z` - Drop coordinates

```yaml
- type: DROP_ITEM
  material: GOLD_INGOT
  amount: 5
  x: 45.0
  y: 4.5
  z: 15.0

- type: DROP_ITEM
  material: DIAMOND
  amount: 2
  x: 45.5
  y: 4.5
  z: 15.5
```

### POTION_EFFECT
Apply potion effects to players.

**Properties:**
- `effect` - Effect type (speed, strength, poison, etc.)
- `duration` - Duration in seconds
- `amplifier` - Effect level (0 = Level I, 1 = Level II)

```yaml
- type: POTION_EFFECT
  effect: POISON
  duration: 10         # 10 seconds
  amplifier: 1         # Poison II

- type: POTION_EFFECT
  effect: SPEED
  duration: 30
  amplifier: 2         # Speed III
```

## Configuration Examples

### Boss Arena

```yaml
triggers:
  boss_arena:
    type: LOCATION
    condition:
      x: 50.0
      y: 5.0
      z: 50.0
      radius: 8.0
    repeatable: false
    cooldown: 0
    actions:
      # Spawn the boss
      - type: SPAWN_MOB
        custom-mob-id: goblin_king
        count: 1
        x: 50.0
        y: 5.0
        z: 50.0

      # Spawn guards
      - type: SPAWN_MOB
        custom-mob-id: goblin_warrior
        count: 3
        x: 48.0
        y: 5.0
        z: 48.0

      # Alert players
      - type: MESSAGE
        message: "&4&lBOSS FIGHT! &7The Goblin King has appeared!"

      # Play sound
      - type: COMMAND
        command: "playsound minecraft:entity.ender_dragon.growl master @a"

      # Show title
      - type: COMMAND
        command: "title @a title {\"text\":\"BOSS FIGHT\",\"color\":\"dark_red\",\"bold\":true}"
```

### Spike Trap

```yaml
triggers:
  spike_trap:
    type: LOCATION
    condition:
      x: 20.0
      y: 3.0
      z: 30.0
      radius: 3.0
    repeatable: true     # Can trigger multiple times
    cooldown: 10         # 10 second cooldown between activations
    actions:
      - type: DAMAGE_PLAYER
        damage: 4          # 2 hearts

      - type: POTION_EFFECT
        effect: POISON
        duration: 10       # 10 seconds
        amplifier: 1       # Poison II

      - type: MESSAGE
        message: "&c&lOUCH! &7You triggered a spike trap!"

      - type: COMMAND
        command: "playsound minecraft:block.anvil.land master @p ~ ~ ~ 1.0 0.5"
```

### Treasure Room

```yaml
triggers:
  treasure_room:
    type: LOCATION
    condition:
      x: 45.0
      y: 4.0
      z: 15.0
      radius: 4.0
    repeatable: false
    cooldown: 0
    actions:
      # Drop gold
      - type: DROP_ITEM
        material: GOLD_INGOT
        amount: 5
        x: 45.0
        y: 4.5
        z: 15.0

      # Drop diamonds
      - type: DROP_ITEM
        material: DIAMOND
        amount: 2
        x: 45.5
        y: 4.5
        z: 15.5

      # Drop emeralds
      - type: DROP_ITEM
        material: EMERALD
        amount: 3
        x: 44.5
        y: 4.5
        z: 15.5

      - type: MESSAGE
        message: "&6&lTREASURE FOUND! &7You discovered the goblin's hoard!"

      - type: COMMAND
        command: "playsound minecraft:entity.player.levelup master @a ~ ~ ~ 1.0 1.5"
```

### Progressive Spawns

```yaml
triggers:
  wave_1:
    type: TIMER
    condition:
      time: 60          # 1 minute
    repeatable: false
    actions:
      - type: SPAWN_MOB
        mob-type: ZOMBIE
        count: 3
        x: 25.0
        y: 5.0
        z: 25.0
      - type: MESSAGE
        message: "&c&lWave 1: &73 Zombies!"

  wave_2:
    type: TIMER
    condition:
      time: 120         # 2 minutes
    repeatable: false
    actions:
      - type: SPAWN_MOB
        mob-type: SKELETON
        count: 4
        x: 25.0
        y: 5.0
        z: 25.0
      - type: MESSAGE
        message: "&c&lWave 2: &74 Skeletons!"

  wave_3:
    type: TIMER
    condition:
      time: 180         # 3 minutes
    repeatable: false
    actions:
      - type: SPAWN_MOB
        custom-mob-id: elite_warrior
        count: 2
        x: 25.0
        y: 5.0
        z: 25.0
      - type: MESSAGE
        message: "&c&lWave 3: &72 Elite Warriors!"
```

### Healing Fountain

```yaml
triggers:
  healing_fountain:
    type: LOCATION
    condition:
      x: 10.0
      y: 5.0
      z: 10.0
      radius: 2.0
    repeatable: true
    cooldown: 60        # 1 minute cooldown
    actions:
      - type: POTION_EFFECT
        effect: INSTANT_HEALTH
        duration: 1
        amplifier: 1     # Instant Health II

      - type: POTION_EFFECT
        effect: REGENERATION
        duration: 30
        amplifier: 1     # Regeneration II

      - type: MESSAGE
        message: "&a&lHEALED! &7The fountain's magic restores you!"
```

## GUI Editor

You can edit triggers visually using the in-game GUI editor:

### Opening the Trigger List
```
/dng edit <dungeon_id>
```
Then open the trigger GUI (implementation varies)

### Creating a New Trigger
1. Click "Create New Trigger" button
2. Enter a unique trigger ID in chat
3. A default LOCATION trigger is created
4. Click to edit properties

### Editing a Trigger
1. Left-click a trigger in the list
2. Opens the trigger editor
3. Change trigger type, condition, repeatability
4. Manage action list

### Managing Actions
1. Click "Actions" in trigger editor
2. View all actions in order
3. Add new actions by type
4. Edit action properties
5. Reorder with shift-click (up/down)
6. Delete with right-click

### Editing Action Properties
1. Click an action in the action list
2. Visual fields appear for the action type
3. Enter values via chat input
4. Save automatically

## Advanced Techniques

### Chained Triggers

Create sequences by using multiple triggers:

```yaml
# Part 1: Enter arena
arena_enter:
  type: LOCATION
  condition:
    x: 50.0
    y: 5.0
    z: 50.0
    radius: 10.0
  repeatable: false
  actions:
    - type: MESSAGE
      message: "&7The arena gates close behind you..."

# Part 2: After 5 seconds, spawn minions
arena_minions:
  type: TIMER
  condition:
    time: 5            # Assumes trigger is checked from dungeon start
  repeatable: false
  actions:
    - type: SPAWN_MOB
      mob-type: ZOMBIE
      count: 5
      x: 50.0
      y: 5.0
      z: 50.0

# Part 3: After killing minions, spawn boss
arena_boss:
  type: MOB_KILL
  condition:
    mob-count: 5
  repeatable: false
  actions:
    - type: SPAWN_MOB
      custom-mob-id: arena_boss
      count: 1
      x: 50.0
      y: 5.0
      z: 50.0
```

### Multi-Stage Boss Fight

```yaml
# Stage 1: Boss spawn
boss_spawn:
  type: LOCATION
  condition:
    x: 50.0
    y: 5.0
    z: 50.0
    radius: 5.0
  repeatable: false
  actions:
    - type: SPAWN_MOB
      custom-mob-id: dragon_boss
      count: 1
      x: 50.0
      y: 5.0
      z: 50.0
    - type: MESSAGE
      message: "&4&lBOSS FIGHT: Phase 1"

# Stage 2: At 50% health (simulated with timer)
boss_phase_2:
  type: TIMER
  condition:
    time: 60
  repeatable: false
  actions:
    - type: SPAWN_MOB
      mob-type: BLAZE
      count: 3
      x: 48.0
      y: 5.0
      z: 50.0
    - type: MESSAGE
      message: "&c&lPhase 2: The boss summons reinforcements!"
```

### Environmental Hazards

```yaml
# Lava floor warning
lava_warning:
  type: TIMER
  condition:
    time: 120          # 2 minutes
  repeatable: false
  actions:
    - type: MESSAGE
      message: "&c&lWARNING: &7The floor is getting hot!"

# Lava floor damage (repeating)
lava_damage:
  type: TIMER
  condition:
    time: 125          # 2:05 minutes
  repeatable: true     # Repeats every tick after 125s
  cooldown: 2          # Every 2 seconds
  actions:
    - type: DAMAGE_PLAYER
      damage: 2
    - type: MESSAGE
      message: "&cThe lava burns you!"
```

## Tips & Best Practices

1. **Use Meaningful IDs**
   - `boss_arena` instead of `trigger1`
   - Helps with debugging and maintenance

2. **Balance Repeatability**
   - One-time events: `repeatable: false`
   - Hazards/traps: `repeatable: true` with cooldown
   - Fountains: `repeatable: true` with high cooldown

3. **Coordinate System**
   - All coordinates are relative to dungeon spawn (0,0,0)
   - Use pos1/pos2 in editor to find exact coordinates
   - Y-level should match your schematic

4. **Action Order Matters**
   - MESSAGE before SPAWN_MOB shows warning first
   - COMMAND for effects before SPAWN_MOB
   - Multiple SPAWN_MOB actions spawn simultaneously

5. **Performance**
   - Avoid too many repeating triggers
   - Use cooldowns on repeatable triggers
   - Don't spawn excessive mobs at once

6. **Testing**
   - Test each trigger individually
   - Verify coordinates in-game with F3
   - Check console for trigger activation messages

## Troubleshooting

**Trigger not firing?**
- Check trigger type matches condition
- Verify coordinates are correct (relative to spawn)
- Ensure trigger hasn't already fired (if non-repeatable)
- Check console for errors

**Actions not executing?**
- Verify action type is spelled correctly
- Check required properties are set
- Custom mob IDs must match YAML definitions
- Material names must be valid (uppercase)

**Coordinates wrong?**
- Use `/dng pos1` and `/dng pos2` to mark positions
- Remember: coordinates are relative to schematic paste point
- Y-level is important - mobs spawn at exact Y

**Trigger firing multiple times?**
- Set `repeatable: false` for one-time triggers
- Add cooldown for repeating triggers
- Location triggers fire for each player entering

## See Also

- [Custom Mobs Guide](CUSTOM_MOBS.md) - Creating mobs for triggers
- [Editor Workflow](EDITOR_WORKFLOW.md) - Using the dungeon editor
- [GUI Editors](GUI_EDITORS.md) - Visual trigger editing
