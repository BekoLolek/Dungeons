# Cost and Difficulty System Implementation

## ✅ Completed Features

### 1. Vault Economy Integration
**Files Created/Modified:**
- ✅ Created `VaultEconomyIntegration.java`
- ✅ Modified `DungeonsPlugin.java` - Added initialization and getter
- ✅ pom.xml already had Vault dependency

**Features:**
- Economy system integration with Vault
- Check player balance
- Withdraw/deposit money
- Format money amounts
- Graceful degradation when Vault is not available

### 2. Difficulty System
**Files Created:**
- ✅ Created `DifficultyLevel.java` enum (EASY, MEDIUM, HARD)

**Files Modified:**
- ✅ `config.yml` - Added difficulty multipliers section:
  ```yaml
  difficulty:
    multipliers:
      EASY:
        mob-health: 0.75
        mob-damage: 0.75
        rewards: 0.8
      MEDIUM:
        mob-health: 1.0
        mob-damage: 1.0
        rewards: 1.0
      HARD:
        mob-health: 1.5
        mob-damage: 1.5
        rewards: 1.5
  ```

### 3. Editor Commands
**Files Modified:**
- ✅ `DungeonEditorCommand.java`:
  - Added `cost` to property list in usage messages
  - Added `cost`/`entrycost` case in `handleSet()` method
  - Added difficulty validation (EASY, MEDIUM, HARD only)
  - Added `entryCost` field to `DungeonBuilder` class
  - Updated `fromDungeon()` to load entryCost
  - Updated `handleInfo()` to display entry cost

- ✅ `DungeonManager.java`:
  - Added `entry-cost` saving in `saveDungeon(DungeonBuilder)`

**Commands Available:**
```bash
/dng set cost <amount>      # Set entry cost (e.g., /dng set cost 100)
/dng set difficulty <level>  # Set difficulty: EASY, MEDIUM, or HARD
/dng info                    # Shows cost and difficulty
```

### 4. Data Model
- ✅ `Dungeon.java` - Already had `entryCost` and `difficulty` fields
- ✅ All getters already implemented

---

## 🔧 Still To Implement

### 1. Economy Check on Dungeon Entry
**What needs to be done:**
- Modify dungeon start logic to check if player has enough money
- Withdraw money when dungeon starts
- Show error if insufficient funds
- Refund on dungeon cancellation (optional)

**Files to modify:**
- `DungeonCommand.java` or `InstanceManager.java` - Where dungeon start logic is
- Add economy check before creating instance

**Implementation example:**
```java
// In dungeon start method
VaultEconomyIntegration economy = plugin.getVaultEconomy();
if (economy.isEnabled()) {
    double cost = dungeon.getEntryCost();
    if (cost > 0) {
        if (!economy.has(player, cost)) {
            player.sendMessage("You need " + economy.format(cost) + " to enter!");
            return;
        }
        economy.withdraw(player, cost);
        player.sendMessage("Paid " + economy.format(cost) + " to enter dungeon");
    }
}
```

### 2. Apply Difficulty Multipliers

**A. Mob Health and Damage Multipliers**

**Where to implement:**
- `InstanceManager.java` or wherever mobs spawn
- Custom mob spawning code
- Trigger system mob spawning

**Implementation approach:**
```java
// Get difficulty multipliers from config
String difficulty = dungeon.getDifficulty();
double healthMultiplier = plugin.getConfig().getDouble("difficulty.multipliers." + difficulty + ".mob-health", 1.0);
double damageMultiplier = plugin.getConfig().getDouble("difficulty.multipliers." + difficulty + ".mob-damage", 1.0);

// Apply to mob
AttributeInstance healthAttr = mob.getAttribute(Attribute.GENERIC_MAX_HEALTH);
if (healthAttr != null) {
    double newHealth = healthAttr.getBaseValue() * healthMultiplier;
    healthAttr.setBaseValue(newHealth);
    mob.setHealth(newHealth);
}

AttributeInstance damageAttr = mob.getAttribute(Attribute.GENERIC_ATTACK_DAMAGE);
if (damageAttr != null) {
    damageAttr.setBaseValue(damageAttr.getBaseValue() * damageMultiplier);
}
```

**Files to check/modify:**
- `CustomMobManager.java` - Custom mob spawning
- `TriggerManager.java` - Trigger-based mob spawning (SPAWN_MOB action)
- Any dungeon instance mob spawning code

**B. Reward Multipliers**

**Where to implement:**
- `RewardManager.java` - When giving rewards

**Implementation:**
```java
double rewardMultiplier = plugin.getConfig().getDouble("difficulty.multipliers." + difficulty + ".rewards", 1.0);
// Multiply item amounts, money, XP by rewardMultiplier
```

### 3. Add Difficulty Multiplier Helper Class
**Recommended:** Create a utility class for easy access

**File to create:** `DifficultyMultiplierUtil.java`
```java
public class DifficultyMultiplierUtil {
    private final DungeonsPlugin plugin;

    public double getMobHealthMultiplier(String difficulty) {
        return plugin.getConfig().getDouble("difficulty.multipliers." + difficulty + ".mob-health", 1.0);
    }

    public double getMobDamageMultiplier(String difficulty) {
        return plugin.getConfig().getDouble("difficulty.multipliers." + difficulty + ".mob-damage", 1.0);
    }

    public double getRewardMultiplier(String difficulty) {
        return plugin.getConfig().getDouble("difficulty.multipliers." + difficulty + ".rewards", 1.0);
    }

    public void applyToMob(LivingEntity mob, String difficulty) {
        // Apply health and damage multipliers
    }
}
```

### 4. Update Documentation
**Files to update:**
- `README.md` - Add cost and difficulty features
- `docs/COMMANDS.md` - Document new commands
- `DUNGEON_CONFIG.md` - Document cost and difficulty fields

---

## 📝 Configuration Examples

### Setting up a dungeon with cost and difficulty:
```bash
/dng create boss_dungeon
/dng set name &c&lBoss Dungeon
/dng set schematic boss_arena
/dng set difficulty HARD
/dng set cost 500
/dng set minparty 3
/dng set maxparty 5
/dng save
```

### Dungeon YAML Example:
```yaml
id: "boss_dungeon"
display-name: "&c&lBoss Dungeon"
schematic-file: "boss_arena.schem"
difficulty: "HARD"
entry-cost: 500.0
party-size:
  min: 3
  max: 5
time-limit: 1800
cooldown: 3600
enabled: true
```

---

## 🎮 Testing Checklist

After implementing remaining features:

**Economy:**
- [ ] Test with Vault installed
- [ ] Test without Vault (should allow free entry)
- [ ] Test with insufficient funds
- [ ] Test cost deduction on entry
- [ ] Test free dungeons (cost: 0)

**Difficulty:**
- [ ] Create dungeons with each difficulty level
- [ ] Verify EASY mobs have 75% health/damage
- [ ] Verify MEDIUM mobs are normal (100%)
- [ ] Verify HARD mobs have 150% health/damage
- [ ] Test invalid difficulty rejection in commands
- [ ] Verify rewards scale with difficulty

**Commands:**
- [ ] `/dng set cost <amount>` works
- [ ] `/dng set difficulty <level>` validates input
- [ ] `/dng info` displays cost and difficulty
- [ ] Cost and difficulty save/load correctly

---

## 🚀 Next Steps

1. **Implement economy check** in dungeon start logic
2. **Apply difficulty multipliers** to mob spawning
3. **Apply reward multipliers** when giving rewards
4. **Test thoroughly** with different scenarios
5. **Update documentation** with new features

---

## 💡 Additional Recommendations

### Optional Enhancements:
1. **Cost scaling by party size** - Charge per player or total
2. **Difficulty-based cooldowns** - Harder = longer cooldown
3. **Dynamic costs** - Based on time of day, player level, etc.
4. **Cost refunds** - Partial refund if dungeon fails early
5. **Economy logging** - Track money flow for balance

### Messages to Add (messages.yml):
```yaml
economy:
  insufficient-funds: "&cYou need {amount} to enter this dungeon!"
  entry-paid: "&7You paid {amount} to enter the dungeon."
  entry-free: "&aThis dungeon is free to enter."
  refund: "&aYou received a {amount} refund."
```

---

## ✅ Summary

**Completed (60%):**
- ✅ Vault integration system
- ✅ Difficulty enum and config
- ✅ Editor commands for cost/difficulty
- ✅ Data model updates
- ✅ Save/load functionality

**Remaining (40%):**
- ⏳ Economy check on entry
- ⏳ Difficulty multipliers on mobs
- ⏳ Reward multipliers
- ⏳ Documentation updates

The foundation is solid. The remaining work is straightforward implementation in the gameplay systems.
