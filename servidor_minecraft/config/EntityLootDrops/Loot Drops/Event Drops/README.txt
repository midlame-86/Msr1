================================================================================
                         EVENT DROPS CONFIGURATION
================================================================================

OVERVIEW
--------
Event Drops are time-limited, competitive drop configurations that can be
toggled on/off. Perfect for seasonal events, competitions, and special
occasions with player statistics tracking.

DIRECTORY STRUCTURE
-------------------
Event Drops/
??? README.txt                              # This file
??? Winter/                                 # Winter event
?   ??? Winter_Event_Drops_Example.json     # Example configuration
?   ??? Drop_Count.json                     # Auto-generated statistics
?   ??? Mobs/                               # Entity-specific configs
?       ??? [custom folders]/               # Organize as needed
??? Summer/                                 # Summer event
??? Easter/                                 # Easter event
??? Halloween/                              # Halloween event
??? [custom events]/                        # Create your own events

CREATING A NEW EVENT
--------------------
1. Create a new folder in Event Drops/ with your event name
2. Add .json configuration files (directly or in Mobs/ subfolder)
3. Enable the event: /lootdrops event <name> on
4. Optionally enable drop counting: /lootdrops dropcount true

Example Structure:
  Event Drops/
  ??? MyCustomEvent/
      ??? custom_drops.json
      ??? Mobs/
          ??? boss_drops.json
          ??? elite_drops.json

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

EVENT-SPECIFIC FEATURES:
  enableDropCount    - Track drops for statistics (default: false)
                       Creates Drop_Count.json automatically

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
  commandChance      - Percentage chance to execute (default: 100)
  dropCommand        - Command to execute only when item drops
  dropCommandChance  - Percentage chance for drop command (default: 100)
  commandCoolDown    - Cooldown in seconds between executions

DOCUMENTATION:
  _comment           - Add comments to your configuration for documentation

NBT ENTITY DATA SYSTEM
----------------------
Check entity NBT data before dropping items - perfect for modded mobs!

Properties:
  • nbtEntityData      - NBT path (e.g., "Health", "ForgeCaps.mod:data.level")
  • nbtDataCondition   - Operator: "<", ">", "<=", ">=", "==", "!=", "contains"
  • nbtDataValue       - Value to compare (string, number, or boolean)
  • nbtEntityDrop      - Item to drop if condition is met
  • nbtEntityDropChance - Drop chance for NBT-specific drop
  • nbtEntityDropMin   - Minimum amount for NBT drop
  • nbtEntityDropMax   - Maximum amount for NBT drop

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

DROP COUNTING SYSTEM
--------------------
Track player statistics for competitive events.

Setup:
  1. Add "enableDropCount": true to drop configurations
  2. Enable event: /lootdrops event <name> on
  3. Enable counting: /lootdrops dropcount true
  4. Drop_Count.json is created automatically in event folder

Commands:
  • /lootdrops dropcount true/false  - Toggle drop counting
  • /lootdrops dropcount reset       - Reset all statistics
  • /lootdrops top [count]           - Global leaderboard
  • /lootdrops eventtop <event> [n]  - Event-specific leaderboard
  • /lootdrops playerstats <player>  - Player statistics
  • /lootdrops alltop                - Comprehensive leaderboard

Drop_Count.json Structure:
{
  "players": {
    "<uuid>": {
      "playerName": "PlayerName",
      "totalDrops": 15,
      "itemCounts": {
        "minecraft:diamond": 10,
        "minecraft:emerald": 5
      },
      "lastUpdated": "2024-01-15T10:30:00Z"
    }
  },
  "summary": {
    "totalItems": 15,
    "totalPlayers": 1,
    "itemBreakdown": {
      "minecraft:diamond": 10,
      "minecraft:emerald": 5
    },
    "lastUpdated": "2024-01-15T10:30:00Z"
  }
}

NESTED FOLDER ORGANIZATION
--------------------------
Each event's Mobs/ folder supports unlimited nested directories.

Suggested Organization Patterns:
  By Tier:
    • Mobs/tier1/common_drops.json
    • Mobs/tier2/rare_drops.json
    • Mobs/tier3/legendary_drops.json

  By Entity Type:
    • Mobs/bosses/raid_bosses.json
    • Mobs/elites/champions.json
    • Mobs/common/regular_mobs.json

  By Reward Type:
    • Mobs/currency/event_tokens.json
    • Mobs/cosmetics/special_items.json
    • Mobs/materials/crafting_drops.json

All .json files in any subfolder are automatically loaded.

EVENT MANAGEMENT
----------------
Commands:
  • /lootdrops event <name> on       - Enable event
  • /lootdrops event <name> off      - Disable event
  • /lootdrops event list            - List all events and their status
  • /lootdrops reload                - Reload all configurations

Multiple Events:
  • Multiple events can be active simultaneously
  • Each event has independent drop counting
  • Event-specific leaderboards track individual event progress

CONFIGURATION EXAMPLES
----------------------

Basic Event Drop:
[
  {
    "itemId": "minecraft:diamond",
    "dropChance": 10.0,
    "minAmount": 1,
    "maxAmount": 3,
    "enableDropCount": true
  }
]

With NBT Condition and Counting:
[
  {
    "itemId": "minecraft:iron_sword",
    "dropChance": 15.0,
    "minAmount": 1,
    "maxAmount": 1,
    "nbtEntityData": "ForgeCaps.mmorpg:entity_data.level",
    "nbtDataCondition": ">",
    "nbtDataValue": "50",
    "nbtEntityDrop": "minecraft:diamond_sword",
    "nbtEntityDropChance": 50.0,
    "nbtEntityDropMin": 1,
    "nbtEntityDropMax": 1,
    "enableDropCount": true
  }
]

Competitive Event with Requirements:
[
  {
    "itemId": "minecraft:emerald",
    "dropChance": 20.0,
    "minAmount": 5,
    "maxAmount": 10,
    "requiredWeather": "thunder",
    "requiredTime": "night",
    "enableDropCount": true,
    "_comment": "Bonus drops during thunderstorms at night"
  }
]

FILE MANAGEMENT
---------------

Auto-Generated:
  • Drop_Count.json - Created when drop counting is enabled
                      Contains player statistics and leaderboards

Safe to Delete:
  • All *_Example.json files
  • Custom .json files you create
  • Drop_Count.json (will regenerate when counting resumes)

Disabling Event Drops:
  1. Disable event: /lootdrops event <name> off
  2. Empty file: Replace content with []
  3. Zero chance: Set "dropChance": 0 and "nbtEntityDropChance": 0
  4. Rename: Change .json to .json.disabled

BEST PRACTICES
--------------
• Use descriptive event folder names (e.g., "Winter2024", "SummerCompetition")
• Enable drop counting for competitive events
• Use _comment fields to document your configurations
• Organize complex events with Mobs/ subfolders
• Test configurations with /lootdrops reload before announcing events
• Reset statistics between events: /lootdrops dropcount reset
• Disable events when not in use to reduce server load

TROUBLESHOOTING
---------------
• Event not active: Use /lootdrops event <name> on
• Drops not counting: Ensure "enableDropCount": true and /lootdrops dropcount true
• Drop_Count.json not created: Enable drop counting and trigger at least one drop
• Leaderboard empty: Check that event is active and drops have occurred
• Multiple events conflicting: Each event operates independently

For comprehensive examples, see:
  • Winter_Event_Drops_Example.json - Complete event configuration
  • Mobs/ folder - Entity-specific examples
  • Main README.txt - Full property documentation

================================================================================
