# Custom Systems Summary - Hearts and Minds Ultimate

**Version**: 0.2.1  
**Author**: [13RDPA] LEON  
**Date**: December 15, 2025

---

## Overview

This document summarizes the custom systems added to Hearts and Minds Ultimate. These systems are organized in the following directories:

- `core\fnc\addaction\` : Centralized action management
- `core\fnc\deploy\` : Deployment system
- `core\fnc\ressources\` : Resource system
- `core\fnc\utility\` : Utility systems
- `core\fnc\virtual_garage\` : Virtual garage

---

## 1. ACTION SYSTEM (`core\fnc\addaction\`)

### Main function
Centralized management of all scroll wheel actions for all mission objects.

### Key components
- **Initialization**: `init.sqf` configures `EntityCreated` EventHandler and synchronizes actions
- **Synchronization**: `syncAllActions.sqf` and `syncActionsForObject.sqf` for JIP
- **Player actions**: `addPlayerActions.sqf` for system actions
- **Checkpoint actions**: `addCheckpointActions.sqf` for checkpoints
- **Admin actions**: Zone/checkpoint reactivation, system verification
- **Counter-attack actions**: Manual counter-attack launch

### Important points
- Strict separation between deployment and resource actions via `btc_ressources_item_valid`
- Automatic synchronization for joining players (JIP)
- Periodic verification loop (30s) to add actions to new objects

---

## 2. DEPLOYMENT SYSTEM (`core\fnc\deploy\`)

### Main function
Complete system for deploying tactical objects (fortifications, troops, checkpoints, crates) with resource management, ACE cargo, and counter-attacks.

### Main components

#### Initialization and Interface
- `init.sqf`: Configures EventHandlers, vehicle cargo, starts monitoring systems
- `initUI.sqf`: Defines object categories and their costs
- `create.sqf`: Selection interface (client)

#### Placement and Preview
- `startPreviewMode.sqf` / `previewLoop.sqf` / `exitPreviewMode.sqf`: Preview mode with rotation/movement
- `confirmPlacement.sqf`: Final creation, resource deduction, cargo configuration, crate inventories
- `place_*.sqf`: Camera and key management

#### Object Management
- `deleteDeployedObject.sqf`: Deletion with refund
- `redeplacerObjet.sqf`: Object redeployment

#### Cargo Configuration
- `setupCargo.sqf`: Configures ACE cargo space for all vehicles (air, land, sea)
  - Supports `btc_deploy_customCargoClasses` for custom classes
  - Manages containers (`Land_Cargo20_military_green_F`: 100, `Land_Cargo40_military_green_F`: 200)
- `cargoSizes.sqf`: Defines cargo sizes for deployable objects

#### Costs and Refunds
- `getObjectCosts.sqf`: Retrieves costs
- `calculateCrateRefund.sqf` / `refundSingleCrate*.sqf`: Crate calculation and refund

#### Troops and Units
- `addTroop.sqf`: Troop addition
- `getUnitClasses.sqf` / `loadouts.sqf`: Unit configuration
- `autoAssignUnits.sqf`: Automatic assignment to vehicles/statics
- `assignUnitToVehicle.sqf` / `placeCarriedUnit.sqf`: Unit management

#### Checkpoints
- `detectFlags.sqf`: Automatic detection of deployed flags
- `activateCheckpointTask.sqf` / `monitorCheckpointTask.sqf` / `completeCheckpointTask.sqf`: Task management
- `monitorCheckpoints.sqf`: Monitoring of all active checkpoints
- `checkpointCounterAttack.sqf`: Automatic counter-attacks
- `monitorCounterAttacks.sqf`: Counter-attack monitoring
- `syncCheckpoints.sqf`: JIP synchronization

#### Resource Exchange
- `openResourceExchangeDialog.sqf` / `processResourceExchange.sqf`: Resource exchange

### Main global variables
- `btc_deploy_fortifications`: Available fortifications
- `btc_deploy_troops`: Available troops
- `btc_deploy_checkpoints`: Available checkpoints
- `btc_deploy_activeCheckpoints`: Active checkpoints
- `btc_deploy_customCargoClasses`: Custom cargo configuration

### Interdependencies
- Uses resource system for costs
- Uses `btc_task_fnc_create` for tasks
- Uses `btc_mil_fnc_create_group` for counter-attacks

---

## 3. RESOURCE SYSTEM (`core\fnc\ressources\`)

### Main function
Complete management of 4 resources (Iron, Ammunition, Food, Fuel) with generation zones, automatic collection, tasks, and counter-attacks.

### Main components

#### Initialization
- `init.sqf`: Initializes zones, depots, collection areas, starts all monitoring systems
- `initClient.sqf`: User interface and keyboard shortcuts
- `initDepots.sqf`: Initializes collection depots (duplicate prevention)
- `initCollectAreas.sqf`: Collection areas with visual barriers

#### Zone Management
- `createZones.sqf`: Zone creation
- `activateZone.sqf` / `deactivateZone.sqf`: Activation/deactivation
- `captureZone.sqf`: Zone capture
- `monitorZones.sqf`: Monitoring to detect capture

#### Resource Generation
- `generationLoop.sqf`: Main loop (60s)
- `monitorZoneSlots.sqf`: Progressive creation (1 object per cycle)
  - Checks available slots
  - Stops if zone full or task active
- `generateResourceItem.sqf`: Object generation
- `checkResourceItemSpace.sqf`: Space verification

#### Collection
- `monitorDepots.sqf`: Automatic collection within depot radius
  - Excludes objects in cargo/carried
  - Excludes objects in collection area
- `collectResourceItem.sqf`: Manual/automatic collection
- `addResourceItemActions.sqf`: Actions (carrying, loading, adjustment)

#### Tasks
- `createCollectTask.sqf`: Collection task creation
- `monitorCollectTask.sqf`: Monitoring (validates when zone empty)
- `verifyCollectTasks.sqf`: Periodic verification (5 min) to create missing tasks
- `activateDefenseTask.sqf` / `monitorDefenseTask.sqf`: Defense tasks

#### Counter-Attacks
- `counterAttackLoop.sqf`: Main loop (random selection, memory 5 zones)
- `counterAttack.sqf`: Counter-attack launch
  - Selects hideout or random position
  - Boat management (spawn 100m from shore, all in boat, waypoints beach → disembark → zone)
  - Building distance checks (50m minimum)
  - Road checks for vehicles
  - Multi-stage waypoints for amphibious assaults
- `manualZoneCounterAttack.sqf`: Manual counter-attack

#### Resource Management
- `addResources.sqf` / `setResources.sqf` / `resetResources.sqf`: Quantity management
- `deductCost.sqf` / `deductCostsArray.sqf`: Cost deduction
- `checkCost.sqf` / `getCosts.sqf`: Cost verification and retrieval
- `syncCosts.sqf`: Multiplayer synchronization

#### User Interface
- `createUI.sqf` / `updateUI.sqf` / `updateUILoop.sqf`: Resource interface
- `showUI.sqf` / `hideUI.sqf`: Show/hide
- `keyHandler.sqf`: Keyboard key management

### Main global variables
- `btc_ressources_zones_objects`: Resource zones
- `btc_ressources_config`: Configuration
- `btc_ressources_amounts`: Amounts [Iron, Ammunition, Food, Fuel, Money]
- `btc_ressources_collected_items`: Collected items
- `btc_ressources_collect_depots_objects`: Collection depots
- `btc_ressources_last_attacked_zones`: Last attacked zones (memory 5)
- `btc_ressources_last_used_hideouts`: Last used hideouts (memory 5)

### Interdependencies
- Used by deployment system for costs
- Uses `btc_mil_fnc_create_group` for counter-attacks
- Uses `btc_task_fnc_create` for tasks

---

## 4. UTILITY SYSTEM (`core\fnc\utility\`)

### Main function
Groups several practical utility systems to improve gameplay.

### Components

#### Configuration (`LEON_config.sqf`)
- General configuration of custom systems

#### Drone (`LEON_drone\`)
- `init.sqf` / `drone.sqf`: Surveillance drone system
- Interface to deploy and control a drone

#### Halo (`LEON_halo\`)
- `LEON_Halo.sqf`: HALO jump system
- Complete interface with controls and sounds

#### Teleporter (`LEON_teleporteur\`)
- `teleporter.sqf`: Teleportation system between points

#### Crate Carrying (`LEON_portage\`)
- `init.sqf`: Initializes crate carrying
- `config_caisses.sqf`: Portable crate configuration
- `functions\fn_initPortageCaisse.sqf` / `fn_gererMouvementCaisse.sqf`: Carrying management

#### Invincibility (`LEON_invincible\`)
- `init.sqf` / `invincible.sqf`: Temporary invincibility on spawn

#### Vehicles (`LEON_vehicles\`)
- `initCaisse.sqf`: Initializes supply crates
  - Defines `OPEX_fnc_*` for custom inventories
  - Manages crates created via Zeus/editor
  - Prevents conflicts with deployment system
- `initCaisseRepair.sqf`: Repair crates
- `medic_repa.sqf`: Medical/vehicle repair system
- `vehicleInventory.sqf`: Vehicle inventory management

#### Debug (`LEON_debug\`)
- `testSystem.sqf`: System tests
- `verifySystem.sqf`: System verification
- `debugCheckpoints.sqf`: Checkpoint debug
- `createDocumentation.sqf`: Documentation generation
- `verificationCohérence.sqf`: Coherence verification

---

## 5. VIRTUAL GARAGE SYSTEM (`core\fnc\virtual_garage\`)

### Main function
Storage and retrieval of vehicles in a virtual garage.

### Components
- `init.sqf`: Initializes garage
- `fnc_openGarage.sqf`: Opens garage (player)
- `fnc_openZenGarage.sqf`: Opens garage (Zeus)

### Features
- Vehicle storage with inventory and damage
- Retrieval of stored vehicles
- Persistence via save system

---

## 6. ENHANCED PATROL SYSTEM (`core\fnc\patrol\`)

### Main function
Automatic patrol system with strict separation between military and civilian patrols, configurable aggressive behavior, and dynamic waypoint updates during combat.

### Main components

#### Initialization
- `autoSpawn.sqf`: Automatic creation system for military patrols based on timer
- Strict separation: Military patrols (`btc_patrol_active`) vs Civilian (`btc_civ_veh_active`)

#### Patrol Creation
- `create_patrol.sqf` (military): Creation from enemy-occupied cities only
- `create_patrol.sqf` (civilian): Creation during city activation
- Player zone exclusion: No patrol spawns within 1000m radius around players
- Sensitive zone exclusion: No spawns within 1500m radius around resource zones, FOBs, checkpoints
- Configurable base exclusion: No spawns within configurable radius around `btc_base` marker (500m-5000m, default: 1500m)
- Custom exclusion zones: Ability to add exclusion zones in `define_mod.sqf` via `btc_patrol_exclusion_zones`

#### Waypoint Management
- `addWP.sqf`: Waypoint creation with parameters according to type (military/civilian)
- `init.sqf`: Patrol route initialization
- `updateCombatWaypoints.sqf`: Dynamic update every 1 minute if in combat
- `getOutVehicle.sqf`: Vehicle exit management (passengers only if armed)

#### Destination Selection
- `usefulCity.sqf`: Destination prioritization system
  - Priority 1: Player positions (direct attack)
  - Priority 2: Liberated cities (recapture)
  - Priority 3: Normal occupied cities
  - Recent city tracking: Exclusion of cities selected in the last 5 minutes to avoid repetitions

#### Detection and Filtering
- `hasPlayersNearby.sqf`: Checks for player presence in configurable radius
- `playersInAreaCityGroup.sqf`: Checks player presence for deletion
- `WPCheck.sqf`: Waypoint verification and reinitialization
- `eh.sqf`: Event handling and deletion

### Patrol Behavior

#### Automatic Patrols (Military)
- **Combat mode**: Fire at will (RED)
- **Behavior**: Awareness (AWARE)
- **Formation**: Column (COLUMN)
- **Speed**: Full (FULL)
- **Creation**: Only via automatic timer (`btc_p_patrol_timer`)

#### Civilian Patrols
- **Combat mode**: Hold fire (BLUE)
- **Behavior**: Careless (CARELESS)
- **Formation**: Column (COLUMN)
- **Speed**: Limited (LIMITED)
- **Destinations**: Only cities liberated by players

### Main global variables
- `btc_patrol_active`: List of active military patrols
- `btc_civ_veh_active`: List of active civilian patrols
- `btc_patrol_area`: Patrol radius (1500m default)
- `btc_p_patrol_timer`: Automatic creation interval (seconds)
- `btc_p_patrol_max`: Maximum number of military patrols
- `btc_p_patrol_vehicle_percent`: Percentage of patrols with vehicles (0-100%)
- `btc_p_patrol_exclusion_base_distance`: Base exclusion distance (500m-5000m, default: 1500m)
- `btc_patrol_exclusion_zones`: Custom exclusion zones (defined in `define_mod.sqf`)
- `btc_patrol_recent_cities`: List of recently selected cities (tracking to avoid repetitions)
- `btc_auto_patrol`: Variable marking automatic patrols (for aggressive behavior)

### Configurable parameters (param.hpp)
- `btc_p_patrol_timer`: Automatic creation timer (0 = disabled)
- `btc_p_patrol_max`: Maximum number of military patrols (0-30)
- `btc_p_civ_max_veh`: Maximum number of civilian patrols (0-30)
- `btc_p_patrol_vehicle_percent`: Percentage of patrols with vehicles (0-100%)
- `btc_p_patrol_exclusion_base_distance`: Base exclusion distance (500m-5000m, default: 1500m)

### Advanced features
- **GETOUT waypoint**: Vehicle patrols exit before reaching destination
  - Armed vehicles: Only passengers exit (driver/gunners stay)
  - Unarmed vehicles: All units exit
- **Dynamic update**: Waypoints recreated every 1 minute in combat to track players
- **Player zone exclusion**: No spawns within 1000m radius around players
- **Sensitive zone exclusion**: No spawns within 1500m radius around resource zones, FOBs, checkpoints
- **Configurable base exclusion**: No spawns within configurable radius around `btc_base` marker (500m-5000m, default: 1500m)
- **Custom exclusion zones**: Ability to add exclusion zones in `define_mod.sqf` via `btc_patrol_exclusion_zones`
- **City tracking**: Tracking system to prevent patrols from always heading to the same cities (exclusion of cities selected in the last 5 minutes)
- **City recapture**: Patrols can recapture liberated cities (40% chance) and create checkpoint missions (40% chance)
- **Smart prioritization**: Patrols target players first, then liberated cities

### Interdependencies
- Uses `btc_mil_fnc_create_patrol` to create military patrols
- Uses `btc_civ_fnc_create_patrol` to create civilian patrols
- Uses `btc_fnc_find_closecity` to find nearby cities
- Uses `btc_patrol_fnc_WPCheck` to verify waypoints

---

## Associated Modifications

### Save System (`core\fnc\db\`)
- **save.sqf**: Saves resource zones (17 elements), collected items (7 elements), cargo (5 elements), checkpoints
- **load.sqf**: Loading and restoration with strict format verification, JIP synchronization

### Event Handlers (`core\fnc\eh\`)
- **CuratorObjectPlaced.sqf**: Cargo and inventory configuration for Zeus objects
- **EntityCreated**: Automatic cargo configuration for all vehicles

### Compilation (`core\fnc\compile.sqf`)
- Compilation of all custom functions

---

## Important Notes

1. **Action Separation**: Deployment and resource actions are strictly separated via `btc_ressources_item_valid`

2. **Save Format**: Strict formats (17 elements zones, 7 elements items, 5 elements cargo) - do not modify without migration

3. **Synchronization**: Use `publicVariable` to synchronize important variables (multiplayer)

4. **Performance**: Use `distanceSqr`, filter before iteration, cache calculated values

5. **Cargo**: Automatic configuration for all vehicles (editor, Zeus, deployed) via `setupCargo.sqf`

6. **Duplicates**: Duplicate prevention for collection depots and resource objects

---

**Document created**: December 10, 2025  
**Last update**: December 15, 2025  
**Mission version**: 0.2.1

