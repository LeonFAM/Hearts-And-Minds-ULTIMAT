# Configuration Guide - Hearts and Minds ULTIMATE

![Version](https://img.shields.io/badge/Version-0.2.1-blue)

**Author**: [13RDPA] LEON  
**Date**: December 10, 2025

---

## Table of Contents

1. [Main Configuration Files](#main-configuration-files)
2. [Basic Configuration](#basic-configuration)
3. [Custom Systems Configuration](#custom-systems-configuration)
4. [Factions and Units Configuration](#factions-and-units-configuration)
5. [Gameplay Configuration](#gameplay-configuration)
6. [Advanced Configuration](#advanced-configuration)

---

## Main Configuration Files

### File Structure

```
Mission/
├── core/
│   ├── def/
│   │   ├── mission.sqf          ← Main configuration (factions, reputation, etc.)
│   │   └── param.hpp            ← Mission parameters (server interface)
│   └── fnc/
│       ├── deploy/              ← Deployable objects configuration
│       │   └── config.sqf       ← Objects, costs, categories
│       └── ressources/          ← Resources configuration
│           └── config.sqf       ← Types, zones, generation rates
├── define_mod.sqf               ← Custom cities, arsenal, loadouts
├── description.ext              ← General mission configuration
└── stringtable.xml              ← FR/EN translations
```

---

## Basic Configuration

### 1. `description.ext` - General Configuration

**Location**: `description.ext` (mission root)

**What you can configure:**

```sqf
// Mission information
author = "[13RDPA] LEON";
onLoadName = "Hearts And Minds ULTIMATE";
onLoadMission = "Stable Version 0.2.1";

// Loading screen
loadScreen = "core\img\13rdpa.jpg";

// Debug console (0 = disabled, 1 = admin only, 2 = everyone)
enabledebugconsole = 1;

// Respawn system
respawn = 3;                    // 3 = BASE respawn
respawnDelay = 2;               // Respawn delay in seconds
respawnDialog = 0;              // 0 = no dialog
```

**When to modify:**
- Change author, mission name, version
- Enable/disable debug console
- Modify respawn parameters

---

### 2. `core\def\mission.sqf` - Main Configuration

**Location**: `core\def\mission.sqf`

**What you can configure:**

#### A. Mission Version

```sqf
btc_version = [
    0,      // Major version
    2,      // Minor version
    1       // Patch version
];
```

#### B. Reputation System

```sqf
// Reputation bonuses (positive actions)
btc_rep_bonus_cache = 25;           // Cache destruction
btc_rep_bonus_civ_hh = 5;           // Helping civilians
btc_rep_bonus_disarm = 15;          // IED disarming
btc_rep_bonus_hideout = 35;         // Hideout destruction

// Reputation penalties (negative actions)
btc_rep_malus_civ_killed = -50;     // Killing a civilian
btc_rep_malus_civ_injured = -15;    // Injuring a civilian
btc_rep_malus_building_damaged = -5; // Building damage

// Reputation thresholds
btc_rep_level_very_low = -500;
btc_rep_level_low = -200;
btc_rep_level_normal = 200;
btc_rep_level_high = 500;
btc_rep_level_very_high = 1000;
```

#### C. IED Configuration

```sqf
btc_ied_range = 8;                  // Detection distance (meters)
btc_ied_offset = -0.08;             // Mine vertical offset
btc_ied_suic_time = 900;            // Delay between suiciders (seconds)
```

#### D. Patrol Configuration

```sqf
btc_patrol_area = 1500;             // Patrol radius (meters) - Player detection zone
btc_p_patrol_timer = 60;            // Automatic creation interval (seconds, 0 = disabled)
btc_p_patrol_max = 10;              // Maximum number of military patrols
btc_p_patrol_vehicle_percent = 50;  // Percentage of patrols with vehicles (0-100)
btc_p_patrol_exclusion_base_distance = 1500;  // Exclusion distance around base (meters, configurable via param.hpp)
```

**Note on exclusion zones** : Patrols do not spawn within a configurable radius around the base (marker `btc_base`). This distance is configurable via the `btc_p_patrol_exclusion_base_distance` parameter (500m to 5000m, default: 1500m). You can also add custom exclusion zones in `define_mod.sqf` (see section 6.C).

#### E. Enemy Faction

```sqf
// Faction is selected via param.hpp, but you can force it here:
// _p_en = "OPF_F";  // CSAT
// _p_en = "OPF_G_F"; // FIA
// etc.

// Add specific vehicles to certain factions
switch (_p_en) do {
    case "OPF_G_F" : {
        btc_type_motorized_armed = btc_type_motorized_armed + ["I_Heli_light_03_F"];
    };
};
```

**When to modify:**
- Adjust difficulty (reputation, IED)
- Change reputation values
- Customize enemy factions

---

### 3. `core\def\param.hpp` - Mission Parameters

**Location**: `core\def\param.hpp`

**What you can configure:**

These parameters are accessible via the server parameter interface when creating the mission.

#### Main Categories:

```cpp
// Time and save
class btc_p_time { /* Start time */ };
class btc_p_load { /* Load save */ };
class btc_p_auto_db { /* Auto-save */ };

// Respawn
class btc_p_respawn_location { /* Respawn point */ };
class btc_p_respawn_ticketsAtStart { /* Starting tickets */ };

// Enemy faction
class btc_p_en { /* Enemy faction */ };
class btc_p_AA { /* AA vehicles */ };
class btc_p_tank { /* Heavy vehicles */ };

// Unit density
class btc_p_density_of_occupiedCity { /* Occupied cities */ };
class btc_p_mil_group_ratio { /* Enemy density */ };
class btc_p_civ_group_ratio { /* Civilian density */ };

// Patrols
class btc_p_patrol_max { /* Maximum number of military patrols (0-30) */ };
class btc_p_civ_max_veh { /* Maximum number of civilian patrols (0-30) */ };
class btc_p_patrol_timer { /* Automatic creation timer (seconds, 0 = disabled) */ };
class btc_p_patrol_vehicle_percent { /* Percentage of patrols with vehicles (0-100%) */ };
class btc_p_patrol_exclusion_base_distance { /* Patrol exclusion distance around base (500m-5000m, default: 1500m) */ };

// Triggers and activation
class btc_p_trigger { /* Disable city activation when plane or helicopter (>80 km/h) is flying above (0=disabled, 1=enabled, default: 1) */ };

// IED
class btc_p_ied { /* IED density */ };
class btc_p_ied_power { /* IED power */ };

// Caches and hideouts
class btc_p_hideout_n { /* Number of hideouts */ };
class btc_p_cache_info_ratio { /* Cache info */ };

// Arsenal
class btc_p_arsenal_Type { /* Arsenal type */ };
class btc_p_arsenal_Restrict { /* Arsenal restrictions */ };
```

**When to modify:**
- Server configuration before launch
- Difficulty adjustment
- Gameplay experience customization

**Note:** These parameters are in the server's GUI, but you can also modify them directly in the file to change default values.

#### D. Trigger Configuration

**Trigger height limitations** : All triggers now have a maximum height to prevent activation from the air :

- **Resource zones, checkpoints and FOBs** : Limited to **50 meters** height
- **Cities** : Limited to **100 meters** height

These limitations prevent accidental triggers from aircraft or helicopters in flight.

**Parameter `btc_p_trigger`** : This parameter controls whether cities activate when an aircraft or helicopter is above the zone :

- **Enabled (1, default)** : Cities will not activate if :
  - An aircraft is present in the trigger
  - A helicopter flying faster than **80 km/h** is present in the trigger
- **Disabled (0)** : Cities will activate normally, even with aircraft above

**Technical note** : The helicopter speed limit was reduced from 190 km/h to 80 km/h in version 0.1.9 for better protection against fast overflights.

---

## Custom Systems Configuration

### 4. `core\fnc\deploy\config.sqf` - Deployment System

**Location**: `core\fnc\deploy\config.sqf`

**What you can configure:**

#### A. Deployable Objects

Each object is defined with:
- Classname (Arma 3 object type)
- Display name
- Resource costs [Iron, Ammo, Food, Fuel, Money]
- Category

```sqf
// Example object configuration
["Land_BagBunker_Small_F", "Sandbag Bunker", [0,0,150,0,300], "defense"],
```

#### B. Object Categories

Objects are organized into categories:
- `defense` - Defenses (bunkers, MG nests, turrets)
- `utility` - Utilities (helipad, barricades, lights)
- `checkpoint` - Complete checkpoints
- `troops` - Defense units
- `special` - Special objects (arsenal, teleporter, FOB)

#### C. Checkpoint Configuration

```sqf
// In param.hpp - Checkpoint costs
#define CHECKPOINT_COST_FER 0
#define CHECKPOINT_COST_MUNITIONS 0
#define CHECKPOINT_COST_NOURRITURE 500
#define CHECKPOINT_COST_FUEL 0
#define CHECKPOINT_COST_ARGENT 1000

// Checkpoint radius
#define CHECKPOINT_RADIUS 150
```

**When to modify:**
- Add new deployable objects
- Adjust object costs
- Change categories
- Balance economy

---

### 5. `core\fnc\ressources\config.sqf` - Resource System

**Location**: `core\fnc\ressources\config.sqf`

**What you can configure:**

#### A. Resource Types

```sqf
btc_ressources_config = [
    [
        "Iron",                         // Name
        "images\icon\arma3-land_pipes_large_f.jpg",  // Icon
        "#FF5733",                      // Color
        5,                              // Generation rate
        5000                            // Maximum
    ],
    // ... other resources
];
```

#### B. Resource Zones

Zones are defined in the mission itself (editor) or via code:

```sqf
// Format: [Position, Type, Radius]
// Type: 0=Iron, 1=Ammo, 2=Food, 3=Fuel, 4=Money
```

#### C. Exchange Rates

```sqf
// In core\fnc\ressources\exchange.sqf
// Format: [ResourceFrom, ResourceTo, Rate]
// Example: 2 Iron = 1 Ammo
[0, 1, 2]  // 2 Iron → 1 Ammo
```

**When to modify:**
- Add new resources
- Change generation rates
- Modify resource limits
- Adjust exchange rates

---

### 6. `define_mod.sqf` - Advanced Customization

**Location**: `define_mod.sqf` (mission root)

**What you can configure:**

#### A. Custom Cities

```sqf
btc_custom_loc = [
    // Format: [Position, Type, Name, Radius, Occupied]
    [[13132.8, 3315.07, 0], "NameVillage", "Small Village", 500, true],
    [[10500, 8200, 0], "NameCity", "Big City", 800, false]
];
```

**Available location types:**
- `NameCity` - Large city
- `NameCityCapital` - Capital
- `NameVillage` - Village
- `NameLocal` - Hamlet
- `Hill` - Hill
- `Airport` - Airport
- `StrongpointArea` - Military base
- `ViewPoint` - Viewpoint
- `NameMarine` - Maritime zone
- `BorderCrossing` - Border crossing
- `VegetationFir` - Forest area

#### B. Custom Arsenal

```sqf
// Add or remove weapons/equipment
private _weapons = [
    "arifle_MX_F",
    "arifle_MX_SW_F"
];

private _items = [
    "G_Shades_Black",
    "G_Shades_Blue"
];

btc_custom_arsenal = [_weapons, _magazines, _items, _backpacks];
```

#### C. Patrol Exclusion Zones

```sqf
btc_patrol_exclusion_zones = [
    // Format: [POSITION, DISTANCE]
    // POSITION: [x, y, z] - Position on the map
    // DISTANCE: Exclusion radius in meters
    [[13132.8, 3315.07, 0], 2000],  // 2000m exclusion zone
    [[15000, 12000, 0], 3000],      // 3000m exclusion zone
    [[8500, 9500, 0], 1500]         // 1500m exclusion zone
];
```

**Note**: These zones are added to the main base zone (marker `btc_base`). The base exclusion distance is configurable via the `btc_p_patrol_exclusion_base_distance` parameter in mission parameters (500m to 5000m, default: 1500m).

#### D. Auto Loadouts

```sqf
// Loadouts by environment: Desert, Tropic, Black, Forest
private _uniforms = [
    "U_B_CombatUniform_mcam",      // Desert
    "U_B_CTRG_Soldier_F",          // Tropic
    "U_B_CTRG_1",                  // Black
    "U_B_CombatUniform_mcam_wdl_f" // Forest
];
```

**When to modify:**
- Add cities to the map
- Customize available arsenal
- Change starting loadouts
- Add patrol exclusion zones

---

## Factions and Units Configuration

### 7. Enemy Factions

**Location**: `core\def\mission.sqf` (lines 650+)

#### A. Available Faction List

Over 200 factions are available, including:
- **Vanilla Arma 3**: CSAT, AAF, FIA, NATO
- **RHS**: USAF, USMC, AFRF, GREF
- **CUP**: Takistan, ChDKZ, CDF, Russia
- **3CB**: Multiple factions
- And many more...

#### B. Per-Faction Customization

```sqf
switch (_p_en) do {
    case "OPF_G_F" : {
        // Add specific vehicles
        btc_type_motorized = btc_type_motorized + ["I_Truck_02_transport_F"];
        btc_type_motorized_armed = btc_type_motorized_armed + ["I_Heli_light_03_F"];
    };
    case "IND_C_F" : {
        // Remove units
        btc_type_units = btc_type_units - ["I_C_Soldier_Camo_F"];
    };
};
```

**When to modify:**
- Add content to a faction
- Balance factions
- Fix composition issues

---

## Gameplay Configuration

### 8. Side Missions

**Location**: `core\fnc\side\`

#### A. Mission Restrictions

**File**: `core\fnc\side\missionRestrictions.sqf`

```sqf
case "rescue": {
    _excludedTypes = [];            // Excluded city types
    _requiresOccupied = true;       // Requires occupied city
    _requiresMarine = false;        // Requires marine city
    _customCheck = {true};          // Custom check
};
```

#### B. Mission Rewards

**File**: `core\fnc\side\configRewards.sqf`

```sqf
case "rescue": {
    _money = 500;                   // Money earned
    _rep = 50;                      // Reputation earned
    _title = "Pilot Rescue";
    _description = "Recover the downed pilot";
};
```

**When to modify:**
- Adjust availability conditions
- Modify rewards
- Add new missions

---

### 9. Counter-Attack Configuration

**Location**: `core\fnc\ressources\counterattack.sqf`

```sqf
// Counter-attack waves
private _waves = [
    [4, 0],     // Wave 1: 4 infantry groups, 0 vehicle
    [6, 1],     // Wave 2: 6 infantry groups, 1 vehicle
    [8, 2]      // Wave 3: 8 infantry groups, 2 vehicles
];

// Delay between waves (seconds)
private _waveDelay = 180;  // 3 minutes
```

**When to modify:**
- Adjust counter-attack difficulty
- Change number of waves
- Modify delays

---

## Advanced Configuration

### 10. Caching Configuration

**Location**: `core\fnc\city\activate.sqf`

```sqf
// City activation distance
private _cachingRadius = _city getVariable ["cachingRadius", 100];

// Unit spawn distance
private _spawningRadius = _cachingRadius/2;
```

---

### 11. Debug Configuration

**Location**: `core\def\mission.sqf`

```sqf
btc_debug = false;                  // Visual debug mode (markers)
btc_debug_log = false;              // Detailed logs in RPT
```

**When to enable:**
- Debugging issues
- Verifying unit spawn
- Testing systems

---

### 12. Checkpoint and FOB Parameters

**Location**: `core\def\param.hpp`

```sqf
// Checkpoints
#define CHECKPOINT_COST_FER 0
#define CHECKPOINT_COST_MUNITIONS 0
#define CHECKPOINT_COST_NOURRITURE 500
#define CHECKPOINT_COST_FUEL 0
#define CHECKPOINT_COST_ARGENT 1000
#define CHECKPOINT_RADIUS 150
#define CHECKPOINT_DEFENSE_RADIUS 50

// FOB
#define FOB_COST_FER 0
#define FOB_COST_MUNITIONS 0
#define FOB_COST_NOURRITURE 2000
#define FOB_COST_FUEL 0
#define FOB_COST_ARGENT 5000
#define FOB_RADIUS 200
```

---

## Configuration Tips

### Before Modifying

1. **Backup** the mission before any modification
2. **Test locally** before putting on a server
3. **Modify one thing at a time** to identify issues
4. **Check RPT logs** in case of errors

### Balancing

- **Resources**: Too easy if generation > 10 per cycle
- **Reputation**: -50 per killed civilian is good balance
- **IED**: Density > 2 becomes very difficult
- **Enemies**: Ratio > 1 creates many enemies

### Compatibility

- **Factions**: Verify required mods are installed
- **Objects**: Test that classnames exist
- **Parameters**: Some parameters require full restart

---

## Files NOT to Modify

Unless you know what you're doing:

- `core\fnc\compile.sqf` - Function compilation
- `core\init_*.sqf` - System initialization
- `core\fnc\db\` - Save system
- `stringtable.xml` - Except for translations

---

## Support and Resources

**Discord**: https://discord.gg/aEJ7W6QwYr  
**Documentation**: `READ_ME/` folder  
**Arma 3 Wiki**: https://community.bistudio.com/wiki

---

**Happy modding!**

*Last update: December 15, 2025 - Version 0.2.1*

