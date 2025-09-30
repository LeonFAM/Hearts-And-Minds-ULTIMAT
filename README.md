# Hearts-And-Minds-ULTIMAT
Hearts And Minds ULTIMAT is a booster version of Hearts And Minds by Vdauphin on the game ARMA 3
---

## 🎯 OVERVIEW

### General description
This Hearts & Minds mission has been enriched with several advanced systems developed by [13RDPA] LEON to improve tactical and strategic gameplay experience.

### Integrated systems
- **LEON Utility Systems** : Advanced features (drone, halo jump, teleportation, cargo carrying)
- **Deployment System** : Placement of fortifications, troops and checkpoints
- **Resource System** : Economic management with capture zones
- **Centralized configuration** : Unified parameters for all systems

### Version
**Version 0.0.7** - Documentation updated 2025

---

## 🏗️ SYSTEM ARCHITECTURE

### Folder structure
```
core\fnc\
├── utility\LEON_*           # LEON utility systems
├── ressources\              # Resource system
├── deploy\                  # Deployment system
└── compile.sqf             # Centralized compilation
```

### Initialization flow
1. `init.sqf` → `core\init.sqf`
2. Function compilation (`compile.sqf`)
3. Server system initialization
4. Client system initialization
5. Centralized configuration (`LEON_config.sqf`)

---

## 🔧 LEON UTILITY SYSTEMS

### 1. Centralized Configuration (`LEON_config.sqf`)

#### Main variables
- **Vehicle inventory** : Automatic configuration by type
- **System costs** : Resources needed for use
- **Portable crates** : Transportable crate types
- **Utility functions** : Functions shared between systems

### 2. Drone System (`LEON_drone\`)

#### Features
- **Aerial surveillance** : Drone control via PC
- **TV interface** : Camera display on TV screen
- **Controls** : CTRLD for piloting, N to change view
- **Management** : Secure launch and stop

#### Files
- `init.sqf` : Initialization and usage tips
- `drone.sqf` : Drone control logic
- `cameraTV.sqf` : TV display management

### 3. Halo Jump System (`LEON_halo\`)

#### Configuration
- **Jump height** : 500m
- **Static aircraft** : C130J at 2000m altitude
- **Parachute** : Automatic opening after 3 seconds
- **Effects** : Aircraft sounds and visual transitions

#### Files
- `LEON_Halo.sqf` : Main jump script
- `defines.hpp` : Constant definitions
- `Halo_interface.hpp` : User interface
- `Music\soundPlane.ogg` : Ambient sounds

### 4. Teleportation System (`LEON_teleporteur\`)

#### Features
- **TP_1 terminal** : Main teleportation point
- **Security** : Group and alive status verification
- **Vehicles** : Automatic integration in vehicles
- **Dynamic actions** : Automatic target updates

#### Files
- `teleporter.sqf` : Teleportation logic

### 5. Cargo Carrying System (`LEON_portage\`)

#### Features
- **Portable crates** : B_CargoNet_01_ammo_F
- **Actions** : Carry/Drop with slowdown
- **Multiplayer** : Synchronization between players
- **Management** : Conflict prevention

#### Files
- `init.sqf` : System initialization
- `config_caisses.sqf` : Type configuration
- `functions\` : Carrying and dropping functions

### 6. Crate Configuration (`LEON_vehicles\`)

#### Supported types
- **ACE_Box_Misc** : Various equipment (laser, NVG, tools)
- **Box_NATO_WpsLaunch_F** : Launchers (FIM-92, MRAWS)
- **B_CargoNet_01_ammo_F** : Ammunition and tactical equipment
- **Box_T_East_Ammo_F** : Repair crates

#### Files
- `initCaisse.sqf` : Automatic configuration (295 lines)
- `initCaisseRepair.sqf` : Repair system
- `medic_repa.sqf` : Medical/repair system
- `vehicleInventory.sqf` : Vehicle inventory
- `gargo.sqf` : Cargo management

---

## ⚔️ DEPLOYMENT SYSTEM

### Overview
The deployment system allows players to strategically place tactical elements on the terrain.

### Available object types

#### 1. Fortifications
- **Walls** : Defensive barriers
- **Sandbags** : Firing positions
- **Barriers** : Tactical obstacles

#### 2. Troops
- **Allied units** : Infantry deployment
- **Tactical groups** : Specialized units
- **Support** : Support units

#### 3. Vehicles
- **Support vehicles** : Logistics and transport
- **Logistics vehicles** : Supply
- **Specialized vehicles** : Specific tactical role

#### 4. Checkpoints
- **Flags** : Flag_FD_Blue_F, Flag_FD_Red_F, Flag_FD_Green_F
- **Capture** : Control by allied presence
- **Defense** : Counter-attack system

### Counter-attack system

#### Configuration
- **Enemy groups** : 3-5 base groups
- **Units per group** : 6-9 units (base + random)
- **Vehicles** : 60% chance per group
- **Vehicle types** : Ifrit, Qilin, Marid, BMP-2, T-100, Ka-52

#### Triggering
- **Automatic** : After checkpoint capture
- **Manual** : By administrator
- **Objective** : Retake captured positions

### Wreck repair
- **Cost** : Resources (fuel, ammunition, iron)
- **Action** : Available on crates near wrecks
- **Compatibility** : Base H&M system

### Main files
- `init.sqf` : System initialization
- `configCentral.sqf` : Centralized configuration
- `createDocumentation.sqf` : Journal documentation
- `addPlayerActions.sqf` : ACE actions
- `saveDatabase.sqf` : Data save
- `loadDatabase.sqf` : Data loading

---

## 💰 RESOURCE SYSTEM

### Overview
The resource system manages the mission economy with 4 main resource types.

### Resource types

#### 1. Fuel
- **Usage** : Vehicles and equipment
- **Color** : Red (#FF6600)
- **Icon** : Gas station
- **Starting amount** : 100

#### 2. Ammunition
- **Usage** : Weapons and explosives
- **Color** : Orange (#FF0000)
- **Icon** : Artillery
- **Starting amount** : 100

#### 3. Iron
- **Usage** : Maintenance and construction
- **Color** : Gray (#808080)
- **Icon** : Maintenance
- **Starting amount** : 100

#### 4. Food
- **Usage** : Survival and morale
- **Color** : Green (#00FF00)
- **Icon** : Medical
- **Starting amount** : 100

### Resource zones

#### Available zones
1. **Fuel Zone** : Position (Altis Map) [23292.4,16486.5,0]
2. **Ammunition Zone** : Position  (Altis Map) [16081.6,16998.9,0]
3. **Iron Zone** : Position (Altis Map) [10924.2,19936.7,0]
4. **Food Zone** : Position (Altis Map) [16563.1,12493.9,0]

#### Capture system
- **Discovery** : Map markers with icons
- **Capture** : Eliminate enemy defenders
- **Generation** : Automatic resource production
- **Defense** : Possible enemy counter-attacks

#### Defensive forces
- **Base groups** : 6 groups
- **Random groups** : 0-1 additional group
- **Units per group** : 6-8 units
- **Action radius** : 500m per zone
- **Maximum limit** : 6 groups per zone
- **Spawn distance** : Groups spawn within 800m radius
- **Counter-attacks** : Spawn at 500-1000m from center
- **Automatic recapture** : After 10 minutes of enemy occupation

### User interface

#### Display
- **Position** : Top left of screen
- **Update** : Real-time with colored icons
- **Controls** : F2 to toggle display

#### Conditional display system
- **First player** : 10-second display at startup
- **Other players** : Interface hidden by default
- **Admin** : Manual display via ACE actions

### Main files
- `init.sqf` : Server initialization
- `initClient.sqf` : Client initialization
- `configCentral.sqf` : Zone configuration
- `createUI.sqf` : User interface
- `initDisplaySystem.sqf` : Display system
- `activateZone.sqf` : Zone activation
- `counterAttack.sqf` : Counter-attack system

---

## 🔗 HEARTS & MINDS INTEGRATION

### Compatibility
All systems are fully integrated with Hearts & Minds.

### Factions and units
- **Automatic adaptation** : To configured factions
- **Enemy units** : According to `btc_type_units`
- **Sides** : Based on `btc_player_side` and `btc_enemy_side`

### Save and persistence
- **Persistence** : Data preserved between sessions
- **Synchronization** : Multiplayer
- **Automatic save** : Every 5 minutes

### Performance
- **Optimized** : For multiplayer
- **Automatic cleanup** : Of deleted objects
- **Memory management** : Efficient

### Recent improvements (Version 0.0.7)
- **Cache system removal** : Performance optimization
- **Automatic cleanup** : Unit removal every 5 minutes
- **Group limit** : Maximum 6 groups per resource zone
- **Smart recapture** : Recapture mechanism after 10 minutes of enemy occupation
- **Restart reactivation** : Zones and checkpoints reactivate automatically
- **Bug fixes** : All synchronization issues resolved
- **Spawn distance** : Groups spawn within 800m radius, counter-attacks at 500-1000m
- **Documentation update** : Player journal and complete documentation updated

---

## ⚙️ CONFIGURATION AND CUSTOMIZATION

### Centralized configuration

#### Main variables (`LEON_config.sqf`)
```sqf
// System costs
LEON_cout_caisse = 10;        // Ammunition for crate
LEON_cout_vehicule = 5;       // Ammunition for vehicle

// Vehicle inventory
LEON_vehicules_objets = createHashMapFromArray [
    ["vehicules_semi_blinde", [weapons, ammunition, equipment]],
    ["vehicules_air", [weapons, ammunition, equipment, parachutes]]
];

// Portable crates
LEON_caisses_portables = ["B_CargoNet_01_ammo_F"];
```

#### Resource configuration (`configCentral.sqf`)
```sqf
// Resource configuration
btc_ressources_config = [
    ["Fuel", 100, "icon", "color"],
    ["Munition", 100, "icon", "color"],
    ["Fer", 100, "icon", "color"],
    ["Nourriture", 100, "icon", "color"]
];

// Resource zones
btc_ressources_zones = [
    [[pos], "type", "name", "object", radius]
];
```

#### Deployment configuration (`configCentral.sqf`)
```sqf
// Counter-attacks
btc_deploy_counter_attack_groups_count = 3;
btc_deploy_counter_attack_units_per_group = 6;
btc_deploy_counter_attack_vehicle_chance = 0.6;
```

### Customization

#### Adding new types
1. Modify lists in `LEON_config.sqf`
2. Add corresponding inventories
3. Update documentation

#### Modifying costs
1. Adjust cost variables
2. Synchronize with resource system
3. Test in-game

---

## 👨‍💼 ADMINISTRATOR COMMANDS

### Resource management

#### Adding resources
```sqf
btc_add_fuel [amount]     // Add fuel
btc_add_ammo [amount]     // Add ammunition
btc_add_iron [amount]     // Add iron
btc_add_food [amount]     // Add food
```

#### Setting resources
```sqf
btc_set_fuel [amount]     // Set fuel
btc_set_ammo [amount]     // Set ammunition
btc_set_iron [amount]     // Set iron
btc_set_food [amount]     // Set food
```

#### General management
```sqf
btc_show_resources         // Show resources
btc_reset_resources        // Reset resources
```

### Zone management
```sqf
btc_show_zones             // Show resource zones
btc_reset_zones            // Reset zones
btc_trigger_zone_attack    // Trigger zone counter-attack
```

### Tactical control
```sqf
btc_trigger_counter_attack // Trigger manual counter-attack
btc_show_config            // Show configuration
btc_reset_config           // Reset configuration
```

### Test and debug commands
```sqf
btc_test_system            // Test complete system
btc_debug_resources        // Resource debug mode
btc_debug_deploy           // Deployment debug mode
btc_verify_system          // Verify system integrity
```

### ACE Actions
Administrators have access to special actions via ACE menu:
- **Manual counter-attack** : Trigger a counter-attack
- **Resource display** : Toggle resource UI
- **System configuration** : Show/modify config
- **Deployment information** : Show statistics

---

## 🔧 TROUBLESHOOTING AND MAINTENANCE

### Common problems

#### Resource system
- **UI not displayed** : Check `btc_ressources_ui_created`
- **Resources not synchronized** : Check `publicVariable`
- **Zones not active** : Check `btc_ressources_zones_active`

#### Deployment system
- **Actions not available** : Check `btc_deploy_fortificationTypes`
- **Save failing** : Check `saveDatabase.sqf`
- **Counter-attacks not triggered** : Check configuration

#### Utility systems
- **Drone not controllable** : Check `BTC_drone_vars`
- **Teleportation failing** : Check `TP_1` and group
- **Cargo carrying impossible** : Check `LEON_caisses_portables`

### Logs and diagnostics

#### Debug variables
```sqf
// Resource system
btc_ressources_debug = true;

// Deployment system
btc_deploy_debug = true;

// Utility systems
LEON_debug_mode = true;
```

#### Verification commands
```sqf
btc_verify_system          // Complete verification
btc_show_config            // Current configuration
btc_test_system            // Test all systems
```

### Preventive maintenance

#### Regular cleanup
- Remove orphaned objects
- Verify database integrity
- Clean temporary variables

#### Backup
- Backup configuration files
- Preserve databases
- Document modifications

---

## 📁 FILE STRUCTURE

### Main files
```
init.sqf                          # Main entry point
core\init.sqf                     # Core initialization
core\fnc\compile.sqf              # Function compilation
```

### LEON Utility Systems
```
core\fnc\utility\
├── LEON_config.sqf              # Centralized configuration (315 lines)
├── LEON_drone\
│   ├── init.sqf                 # Drone initialization
│   ├── drone.sqf                # Drone logic
│   └── cameraTV.sqf             # TV interface
├── LEON_halo\
│   ├── LEON_Halo.sqf            # Halo jump script
│   ├── defines.hpp              # Definitions
│   └── Music\soundPlane.ogg     # Sounds
├── LEON_teleporteur\
│   └── teleporter.sqf           # Teleportation system
├── LEON_portage\
│   ├── init.sqf                 # Cargo carrying system (110 lines)
│   ├── config_caisses.sqf       # Crate configuration
│   └── functions\               # Carrying functions
└── LEON_vehicles\
    ├── initCaisse.sqf           # Crate configuration (295 lines)
    ├── initCaisseRepair.sqf     # Repair crates
    ├── medic_repa.sqf           # Medical system
    ├── vehicleInventory.sqf     # Vehicle inventory
    └── gargo.sqf                # Cargo management
```

### Resource system
```
core\fnc\ressources\
├── init.sqf                     # Server initialization
├── initClient.sqf               # Client initialization
├── configCentral.sqf            # Zone configuration (287 lines)
├── createUI.sqf                 # User interface
├── initDisplaySystem.sqf        # Display system
├── activateZone.sqf             # Zone activation
├── counterAttack.sqf            # Counter-attacks
├── addPlayerActions.sqf         # ACE actions
├── exposeCommands.sqf           # Console commands (471 lines)
└── [other files...]             # 40+ system files
```

### Deployment system
```
core\fnc\deploy\
├── init.sqf                     # System initialization
├── configCentral.sqf            # Centralized configuration
├── createDocumentation.sqf      # Journal documentation (278 lines)
├── addPlayerActions.sqf         # ACE actions
├── saveDatabase.sqf             # Data save
├── loadDatabase.sqf             # Data loading
├── addCheckpointActions.sqf     # Checkpoint actions
├── addCounterAttackActions.sqf  # Counter-attack actions
├── addVehicleActions.sqf        # Vehicle actions
└── [other files...]             # 50+ system files
```

### Documentation
```
README_ME\
└── DOCUMENTATION_COMPLETE_SYSTEMES_LEON.md  # This document
```

---

## 📊 PROJECT STATISTICS

### File count
- **LEON Utility Systems** : ~15 files
- **Resource system** : ~40 files
- **Deployment system** : ~50 files
- **Total** : ~105 code files

### Lines of code
- **LEON_config.sqf** : 315 lines
- **initCaisse.sqf** : 295 lines
- **exposeCommands.sqf** : 471 lines
- **createDocumentation.sqf** : 278 lines
- **Total estimated** : 15,000+ lines of code

### Features
- **4 utility systems** : Drone, Halo, Teleportation, Cargo carrying
- **1 deployment system** : Fortifications, troops, checkpoints
- **1 resource system** : 4 resource types, capture zones
- **Centralized configuration** : Unified parameters
- **User interface** : Real-time UI with controls
- **Save system** : Data persistence

---

## 🎯 CONCLUSION

This Hearts & Minds mission enriched by [13RDPA] LEON represents a complete and integrated set of advanced tactical systems. All systems are designed to work together coherently, offering a rich and immersive gameplay experience.

### Strengths
- **Perfect integration** with Hearts & Minds
- **Centralized configuration** for easy maintenance
- **User interface** intuitive and informative
- **Robust save system**
- **Complete documentation** for users and administrators

### Maintenance
- Regular documentation of modifications
- Systematic testing after changes
- Configuration backup
- Performance monitoring

---

**Document version** : 0.0.7  
**Author** : [13RDPA] LEON  
**Update date** : 2025  
**Status** : Complete and up-to-date documentation
