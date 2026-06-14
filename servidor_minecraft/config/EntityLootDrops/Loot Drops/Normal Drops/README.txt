================================================================================
                         NORMAL DROPS CONFIGURATION
================================================================================

OVERVIEW
--------
Normal Drops are ALWAYS ACTIVE regardless of events. Use this directory for
permanent server-wide loot modifications that should apply at all times.

DIRECTORY CONTENTS
------------------
Normal Drops/
??? README.txt                          # This file
??? Global_Hostile_Drops.json           # Main configuration (auto-regenerates)
??? Global_Hostile_Drops.example        # Comprehensive property reference
??? NBT_Entity_Data_Examples.example    # NBT condition examples
??? Mobs/                               # Entity-specific configurations
    ??? Zombie_Example.json             # Basic zombie example
    ??? Skeleton_Example.json           # Basic skeleton example
    ??? NBT_Zombie_Example.json         # NBT condition example
    ??? [custom folders]/               # Your custom organization

FILE BEHAVIOR
-------------
Auto-Regenerating:
  • Global_Hostile_Drops.json - Recreated if deleted (contains your main config)

Safe to Delete:
  • *_Example.json files - Will NOT regenerate individually
  • *.example files - Reference documentation only
  • Custom .json files - Your configurations

To Regenerate All Examples:
  1. Delete entire Mobs/ folder
  2. Use /lootdrops reload or restart server
  3. All example files will be recreated

CONFIGURATION PROPERTIES
------------------------

REQUIRED PROPERTIES:
  itemId       - Minecraft item ID (e.g., "minecraft:diamond")
  dropChance   - Percentage chance to drop (0-100)
  minAmount    - Minimum number of items to drop
  maxAmount    - Maximum number of items to drop

ITEM CUSTOMIZATION:
  nbtData      - Custom NBT data for the dropped item
                 Example: "{Enchantments:[{id:\"sharpness\",lvl:5}]}"

DROP BEHAVIOR:
  requirePlayerKill  - Only drop if killed by player (default: true)
  allowDefaultDrops  - Allow vanilla drops alongside custom (default: true)
  allowModIDs        - Array of mod IDs that can trigger drops
                       Empty = all mods allowed

EXTRA VANILLA DROPS:
  extraDropChance    - Percentage chance for extra vanilla drops (0-100)
  extraAmountMin     - Minimum amount of extra drops
  extraAmountMax     - Maximum amount of extra drops

PLAYER REQUIREMENTS:
  requiredAdvancement - Player must have this advancement
  requiredEffect      - Player must have this potion effect
  requiredEquipment   - Player must have this item equipped

ENVIRONMENTAL CONDITIONS:
  requiredWeather    - "clear", "rain", or "thunder"
  requiredTime       - "day", "night", "dawn", or "dusk"
  requiredDimension  - Dimension ID (e.g., "minecraft:the_nether")
  requiredBiome      - Biome ID (e.g., "minecraft:desert")

COMMAND EXECUTION:
  command            - Command to execute on entity death
  commandChance      - Percentage chance to execute command (default: 100)
  dropCommand        - Command to execute only when item drops
  dropCommandChance  - Percentage chance for drop command (default: 100)
  commandCoolDown    - Cooldown in seconds between command executions

NBT ENTITY DATA CONDITIONS:
  nbtEntityData      - NBT path to check (e.g., "Health", "ForgeCaps.mod:data")
  nbtDataCondition   - Comparison operator:
                       "<", ">", "<=", ">=", "==", "!=", "contains"
  nbtDataValue       - Value to compare against (string, number, or boolean)
  nbtEntityDrop      - Item to drop if NBT condition is met
  nbtEntityDropChance - Drop chance for NBT-specific drop
  nbtEntityDropMin   - Minimum amount for NBT drop
  nbtEntityDropMax   - Maximum amount for NBT drop

DOCUMENTATION:
  _comment           - Add comments to your configuration for documentation

NBT ENTITY DATA SYSTEM
----------------------
Check entity NBT data before dropping items - perfect for modded mobs!

Use Cases:
  • MMORPG mobs with level/health/stats requirements
  • Boss mobs with phase-based drops
  • Custom entities with special abilities or states
  • Equipment-based conditional drops
  • Modded mob data stored in NBT or ForgeCaps

How to Use:
  1. Find NBT path: Use /data get entity <selector>
  2. Set nbtEntityData to the path (e.g., "Health", "ForgeCaps.mod:data.level")
  3. Set nbtDataCondition ("<", ">", "<=", ">=", "==", "!=", "contains")
  4. Set nbtDataValue to compare against
  5. Configure nbtEntityDrop and related fields

Supported Operators:
  <, >, <=, >=  - Numeric comparisons
  ==, !=        - Equality checks (works with strings, numbers, booleans)
  contains      - String contains check

Examples:
  • Health check: "nbtEntityData": "Health", "nbtDataCondition": "<", "nbtDataValue": "10"
  • Level check: "nbtEntityData": "ForgeCaps.mmorpg:entity_data.level", "nbtDataCondition": ">", "nbtDataValue": "50"
  • Name check: "nbtEntityData": "CustomName", "nbtDataCondition": "contains", "nbtDataValue": "Boss"
  • Equipment: "nbtEntityData": "ArmorItems[3].id", "nbtDataCondition": "contains", "nbtDataValue": "diamond"

For comprehensive NBT examples, see:
  • NBT_Entity_Data_Examples.example - Detailed reference
  • Mobs/NBT_Zombie_Example.json - Working example

DROP COUNTING LIMITATION
------------------------
IMPORTANT: "enableDropCount": true does NOT work in Normal Drops!

Reason:
  Normal drops are always active and would create massive data files over time.
  Drop counting is designed for time-limited competitive events.

For Drop Counting:
  1. Use Event Drops/ directory instead
  2. Create or use an existing event folder
  3. Add "enableDropCount": true to configurations
  4. Enable event: /lootdrops event <name> on
  5. Enable counting: /lootdrops dropcount true

NESTED FOLDER ORGANIZATION
--------------------------
The Mobs/ folder supports unlimited nested directories for organization.

Suggested Organization Patterns:
  By Mod:
    • Mobs/thermal_expansion/machines.json
    • Mobs/twilight_forest/bosses.json

  By Entity Type:
    • Mobs/undead/zombie_variants.json
    • Mobs/arthropods/spider_types.json

  By Biome:
    • Mobs/nether/nether_mobs.json
    • Mobs/end/end_mobs.json

  By Difficulty:
    • Mobs/boss_mobs/raid_bosses.json
    • Mobs/elite_mobs/champions.json

  By NBT Conditions:
    • Mobs/nbt_conditions/mmorpg/high_level.json
    • Mobs/nbt_conditions/health_based/low_health.json

All .json files in any subfolder are automatically loaded.

CONFIGURATION EXAMPLES
----------------------

Basic Drop:
[
  {
    "itemId": "minecraft:diamond",
    "dropChance": 5.0,
    "minAmount": 1,
    "maxAmount": 3
  }
]

With NBT Condition:
[
  {
    "itemId": "minecraft:iron_sword",
    "dropChance": 10.0,
    "minAmount": 1,
    "maxAmount": 1,
    "nbtEntityData": "Health",
    "nbtDataCondition": "<",
    "nbtDataValue": "10",
    "nbtEntityDrop": "minecraft:diamond_sword",
    "nbtEntityDropChance": 50.0,
    "nbtEntityDropMin": 1,
    "nbtEntityDropMax": 1
  }
]

With Player Requirements:
[
  {
    "itemId": "minecraft:emerald",
    "dropChance": 15.0,
    "minAmount": 1,
    "maxAmount": 5,
    "requiredAdvancement": "minecraft:story/mine_diamond",
    "requiredEffect": "minecraft:luck"
  }
]

COMMANDS
--------
  • /lootdrops reload  - Reload all configurations

TROUBLESHOOTING
---------------
• Drops not working: Check console for JSON parsing errors
• NBT conditions failing: Use /data get entity to verify NBT paths
• File not loading: Ensure .json extension and valid JSON syntax

For comprehensive examples, see:
  • Global_Hostile_Drops.example - All available properties
  • NBT_Entity_Data_Examples.example - NBT condition examples
  • Mobs/ folder - Working configuration examples

================================================================================
