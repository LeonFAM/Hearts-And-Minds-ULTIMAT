# Complete Architecture - Hearts and Minds Ultimate v0.2.1

**Mission**: Hearts and Minds Ultimate  
**Version**: 0.2.1  
**Author**: [13RDPA] LEON  
**Date**: December 15, 2025

---

## 📋 TABLE OF CONTENTS

1. [Overview](#overview)
2. [File Structure](#file-structure)
3. [Base Systems (Hearts and Minds)](#base-systems-hearts-and-minds)
4. [Custom Systems (LEON)](#custom-systems-leon)
5. [Initialization Flow](#initialization-flow)
6. [Interdependencies](#interdependencies)
7. [Main Global Variables](#main-global-variables)
8. [Event Handlers](#event-handlers)
9. [Save/Load System](#saveload-system)

---

## 🎯 OVERVIEW

Hearts and Minds Ultimate is a persistent automated Arma 3 mission based on Hearts and Minds, with custom systems added. The mission is divided into two main parts:

1. **Base Mission (Hearts and Minds)**: Complex system with 33 interdependent systems
2. **Custom Systems (LEON)**: Additions for deployment, resources, and utilities

**CRITICAL WARNING**: The base mission has many interdependencies between files. Any modification requires great caution.

---

## 📁 FILE STRUCTURE

### Root files
- `init.sqf`: Main entry point
- `description.ext`: Mission configuration
- `define_mod.sqf`: Required mods definition

### Main directories
- `core/`: Main mission code
  - `def/`: Definitions and parameters
  - `fnc/`: Functions organized by system
  - `img/`: Images
  - `sounds/`: Sounds
  - `init*.sqf`: Initialization scripts

---

## 🔧 BASE SYSTEMS (HEARTS AND MINDS)

### 1. CITY SYSTEM (city)
**Directory**: `core/fnc/city/`

**Main function**: Manages cities on the map, their occupation, liberation, and population.

**Files**:
- `init.sqf`: Initializes all cities on the map
- `create.sqf`: Creates a city with its properties
- `activate.sqf`: Activates a city (spawn enemies/civilians)
- `de_activate.sqf`: Deactivates a city
- `setClear.sqf`: Marks a city as liberated
- `setPlayerTrigger.sqf`: Configures player triggers
- `cleanUp.sqf`: Cleans up city units
- `getHouses.sqf`: Retrieves city houses
- `send.sqf`: Sends reinforcements to a city
- `trigger_free_condition.sqf`: Condition to liberate a city

**Global variables**:
- `btc_city_all`: HashMap of all cities
- `btc_city_occupied`: Occupied cities

**Interdependencies**:
- Uses `btc_mil_fnc_create_group` to spawn enemies
- Uses `btc_civ_fnc_populate` to spawn civilians
- Uses `btc_rep_fnc_change` for reputation

---

### 2. CACHE SYSTEM (cache)
**Directory**: `core/fnc/cache/`

**Main function**: Manages weapon caches to find and destroy.

**Files**:
- `init.sqf`: Initializes the cache system
- `find_pos.sqf`: Finds a position for the cache
- `create.sqf`: Creates a cache
- `create_attachto.sqf`: Creates a cache attached to an object
- `hd.sqf`: Handles cache destruction

**Global variables**:
- `btc_cache_n`: Number of caches
- `btc_cache_obj`: Current cache object
- `btc_cache_pos`: Cache position
- `btc_cache_markers`: Cache markers
- `btc_cache_pictures`: Cache pictures
- `btc_cache_info`: Required information

**Interdependencies**:
- Uses `btc_info_fnc_cache` for intel
- Uses `btc_city_all` to find a position
- Triggers creation of a new cache after destruction

---

### 3. HIDEOUT SYSTEM (hideout)
**Directory**: `core/fnc/hideout/`

**Main function**: Manages enemy hideouts to destroy.

**Files**:
- `create.sqf`: Creates a hideout
- `create_composition.sqf`: Creates hideout composition
- `hd.sqf`: Handles hideout destruction

**Global variables**:
- `btc_hideouts`: Array of all hideouts

**Interdependencies**:
- Used by counter-attacks (resources, checkpoints, FOB)
- Used by `btc_info_fnc_hideout` for intel

---

### 4. MILITARY SYSTEM (mil)
**Directory**: `core/fnc/mil/`

**Main function**: Manages enemy military units.

**Files**:
- `create_group.sqf`: Creates a unit group
- `create_patrol.sqf`: Creates a patrol
- `create_static.sqf`: Creates a static weapon
- `create_staticOnRoof.sqf`: Creates a static weapon on a roof
- `createVehicle.sqf`: Creates a vehicle
- `createUnits.sqf`: Creates units
- `addWP.sqf`: Adds waypoints
- `send.sqf`: Sends reinforcements
- `set_skill.sqf`: Configures AI skills
- `check_cap.sqf`: Checks capacity
- `getStructures.sqf`: Retrieves structures
- `getBuilding.sqf`: Retrieves a building
- `unit_killed.sqf`: Handles unit death
- `class.sqf`: Defines unit classes
- `ammoUsage.sqf`: Manages ammunition usage

**Global variables**:
- `btc_type_units`: Enemy unit types
- `btc_type_vehicles`: Enemy vehicle types
- `btc_type_boats`: Enemy boat types

**Interdependencies**:
- Used by `btc_city_fnc_activate` to spawn enemies
- Used by counter-attacks
- Used by side missions

---

### 5. CIVILIAN SYSTEM (civ)
**Directory**: `core/fnc/civ/`

**Main function**: Manages civilians and their interactions.

**Files**:
- `populate.sqf`: Populates a city with civilians
- `create_patrol.sqf`: Creates a civilian patrol
- `class.sqf`: Defines civilian classes
- `add_weapons.sqf`: Adds weapons to civilians
- `get_weapons.sqf`: Retrieves civilian weapons
- `add_grenade.sqf`: Adds grenades
- `get_grenade.sqf`: Retrieves grenades
- `add_leaflets.sqf`: Adds leaflets
- `leaflets.sqf`: Manages leaflets
- `evacuate.sqf`: Evacuates civilians
- `addWP.sqf`: Adds waypoints to civilians
- `createFlower.sqf`: Creates flowers (memorial)
- `createGrave.sqf`: Creates a grave

**Global variables**:
- `btc_type_civ`: Civilian types
- `btc_type_civ_veh`: Civilian vehicle types

**Interdependencies**:
- Used by `btc_city_fnc_activate`
- Affects reputation (`btc_rep_fnc_change`)

---

### 6. REPUTATION SYSTEM (rep)
**Directory**: `core/fnc/rep/`

**Main function**: Manages reputation with civilians.

**Files**:
- `change.sqf`: Changes reputation
- `notify.sqf`: Notifies changes
- `killed.sqf`: Handles death (affects reputation)
- `hh.sqf`: Handles interactions
- `eh_effects.sqf`: Event handler effects
- `buildingchanged.sqf`: Destroyed buildings
- `explosives_defuse.sqf`: Explosive defusal
- `wheelChange.sqf`: Wheel change
- `grave.sqf`: Grave management
- `call_militia.sqf`: Calls militia
- `hd.sqf`: Handles destruction
- `suppressed.sqf`: Handles suppression
- `foodRemoved.sqf`: Food removed
- `treatment.sqf`: Medical treatment

**Global variables**:
- `btc_global_reputation`: Global reputation
- `btc_rep_militia`: Militia reputation

**Interdependencies**:
- Affected by player actions
- Affects civilian behavior
- Triggers events based on level

---

### 7. IED SYSTEM (ied)
**Directory**: `core/fnc/ied/`

**Main function**: Manages improvised explosive devices.

**Files**:
- `initArea.sqf`: Initializes an IED area
- `create.sqf`: Creates an IED
- `boom.sqf`: Detonates an IED
- `check.sqf`: Checks an IED
- `checkLoop.sqf`: Check loop
- `fired_near.sqf`: Detects nearby shots
- `suicider_active.sqf`: Activates a suicide bomber
- `suicider_activeLoop.sqf`: Suicide bomber loop
- `suicider_create.sqf`: Creates a suicide bomber
- `suiciderLoop.sqf`: Main suicide bomber loop
- `allahu_akbar.sqf`: Suicide bomber cry
- `drone_active.sqf`: Activates an IED drone
- `drone_create.sqf`: Creates an IED drone
- `droneLoop.sqf`: IED drone loop
- `drone_fire.sqf`: Makes an IED drone fire
- `randomRoadPos.sqf`: Random road position
- `belt.sqf`: Explosive belt
- `effects.sqf`: Visual effects
- `effect_smoke.sqf`: Smoke effect
- `effect_color_smoke.sqf`: Colored smoke effect
- `effect_rocks.sqf`: Rocks effect
- `effect_blurEffect.sqf`: Blur effect
- `effect_shock_wave.sqf`: Shock wave
- `deleteLoop.sqf`: Deletion loop

**Global variables**:
- `btc_ied_list`: IED list
- `btc_p_ied`: IED probability

**Interdependencies**:
- Uses `btc_rep_fnc_change` for reputation
- Uses `btc_civ_fnc_create_patrol` for suicide bombers

---

### 8. INFORMATION SYSTEM (info)
**Directory**: `core/fnc/info/`

**Main function**: Manages intel and information.

**Files**:
- `cache.sqf`: Cache information
- `hideout.sqf`: Hideout information
- `give_intel.sqf`: Gives intel
- `has_intel.sqf`: Checks if intel exists
- `ask.sqf`: Requests information
- `hideout_asked.sqf`: Requested hideout
- `search_for_intel.sqf`: Searches for intel
- `troops.sqf`: Troop information
- `ask_reputation.sqf`: Request based on reputation
- `cachePicture.sqf`: Cache picture
- `cacheMarker.sqf`: Cache marker
- `path.sqf`: Path to objective
- `createIntels.sqf`: Creates intel

**Global variables**:
- `btc_info_intel_id`: Intel ID
- `btc_info_cache_def`: Default cache information

**Interdependencies**:
- Used by civilians (`btc_int_fnc_ask`)
- Used to find caches and hideouts

---

### 9. SIDE MISSIONS SYSTEM (side)
**Directory**: `core/fnc/side/`

**Main function**: Manages dynamic side missions.

**Files**:
- `create.sqf`: Creates a side mission
- `createWithParams.sqf`: Creates with parameters
- `configRewards.sqf`: Configures rewards
- `getAvailableMissions.sqf`: Retrieves available missions
- `missionRestrictions.sqf`: Mission restrictions
- `isCityValidForMission.sqf`: Checks if a city is valid
- `checkMissionRequirements.sqf`: Checks prerequisites
- `selectConvoyRoute.sqf`: Selects a convoy route
- `giveRewards.sqf`: Gives rewards
- `initRequestDialog.sqf`: Initializes request dialog
- `updateMissionList.sqf`: Updates list
- `updateRewardsDisplay.sqf`: Updates rewards display
- `acceptMission.sqf`: Accepts a mission
- `addPlayerActions.sqf`: Adds player actions
- `mines.sqf`: Mines mission
- `supply.sqf`: Supply mission
- `vehicle.sqf`: Vehicle mission
- `tower.sqf`: Tower mission
- `checkpoint.sqf`: Checkpoint mission
- `underwater_generator.sqf`: Underwater generator mission
- `convoy.sqf`: Convoy mission
- `rescue.sqf`: Rescue mission
- `capture_officer.sqf`: Officer capture mission
- `hostage.sqf`: Hostage mission
- `hack.sqf`: Hack mission
- `kill.sqf`: Elimination mission
- `chemicalLeak.sqf`: Chemical leak mission
- `EMP.sqf`: EMP mission
- `removeRubbish.sqf`: Cleanup mission
- `pandemic.sqf`: Pandemic mission
- `massacre.sqf`: Massacre mission

**Global variables**:
- `btc_side_list`: Side missions list
- `btc_side_jip_data`: JIP data

**Interdependencies**:
- Uses `btc_city_all` for positions
- Uses `btc_mil_fnc_create_group` for enemies
- Uses `btc_task_fnc_create` for tasks

---

### 10. FOB SYSTEM (fob)
**Directory**: `core/fnc/fob/`

**Main function**: Manages Forward Operating Bases.

**Files**:
- `create_s.sqf`: Creates a FOB (server)
- `create.sqf`: Creates a FOB (client)
- `dismantle_s.sqf`: Dismantles a FOB (server)
- `killed.sqf`: Handles FOB destruction
- `rallypointTimer.sqf`: Rally point timer
- `detectFOBs.sqf`: Detects deployed FOBs
- `zoneStatus.sqf`: FOB zone status
- `activateFOBTask.sqf`: Activates FOB task
- `monitorFOBTask.sqf`: Monitors FOB task
- `completeFOBTask.sqf`: Completes FOB task
- `fobCounterAttack.sqf`: FOB counter-attack
- `manualFOBCounterAttack.sqf`: Manual FOB counter-attack
- `monitorFOBsCounterAttacks.sqf`: Monitors FOB counter-attacks
- `addInteraction.sqf`: Adds interactions
- `redeploy.sqf`: Redeploys a FOB
- `redeployCheck.sqf`: Checks redeployment
- `rallypointAssemble.sqf`: Assembles rally point

**Global variables**:
- `btc_fobs`: FOB array
- `btc_fob_last_attacked_fobs`: Last attacked FOBs

**Interdependencies**:
- Uses `btc_mil_fnc_create_group` for counter-attacks
- Uses `btc_task_fnc_create` for tasks
- Uses respawn system

---

### 11. LOGISTICS SYSTEM (log)
**Directory**: `core/fnc/log/`

**Main function**: Manages logistics (towing, loading, repair).

**Files**:
- `init.sqf`: Initializes logistics system
- `create.sqf`: Creates a logistics point
- `createVehicle.sqf`: Creates a logistics vehicle
- `create_apply.sqf`: Applies creation
- `create_load.sqf`: Loads an object
- `create_change_target.sqf`: Changes target
- `delete.sqf`: Deletes a logistics point
- `server_delete.sqf`: Deletes (server)
- `place.sqf`: Places an object
- `place_create_camera.sqf`: Creates placement camera
- `place_destroy_camera.sqf`: Destroys camera
- `place_key_down.sqf`: Handles keys
- `get_corner_points.sqf`: Retrieves corner points
- `repair_wreck.sqf`: Repairs a wreck
- `server_repair_wreck.sqf`: Repairs (server)
- `copy.sqf`: Copies an object
- `paste.sqf`: Pastes an object
- `refuelSource.sqf`: Refuel source
- `rearmSource.sqf`: Rearm source
- `inventoryGet.sqf`: Retrieves inventory
- `inventorySet.sqf`: Sets inventory
- `inventoryCopy.sqf`: Copies inventory
- `inventoryPaste.sqf`: Pastes inventory
- `inventoryRestore.sqf`: Restores inventory

**Global variables**:
- `btc_log_point_selected`: Selected logistics point
- `btc_log_obj_selected`: Selected object

**Interdependencies**:
- Uses `btc_tow_fnc_*` for towing
- Uses `btc_lift_fnc_*` for lifting

---

### 12. TOWING SYSTEM (tow)
**Directory**: `core/fnc/tow/`

**Main function**: Manages vehicle towing.

**Files**:
- `int.sqf`: Initializes towing
- `ropeCreate.sqf`: Creates rope
- `hitch_points.sqf`: Hitch points
- `unhook.sqf`: Unhooks
- `check.sqf`: Checks towing
- `ropeBreak.sqf`: Broken rope
- `ViV.sqf`: Vehicle in Vehicle

**Interdependencies**:
- Used by logistics system
- Uses ACE3 for ropes

---

### 13. LIFTING SYSTEM (lift)
**Directory**: `core/fnc/lift/`

**Main function**: Manages object lifting with helicopter.

**Files**:
- `check.sqf`: Checks lifting
- `deployRopes.sqf`: Deploys ropes
- `destroyRopes.sqf`: Destroys ropes
- `hook.sqf`: Hooks
- `hookFake.sqf`: Hooks (simulation)
- `hud.sqf`: HUD interface
- `hudLoop.sqf`: HUD loop
- `shortcuts.sqf`: Keyboard shortcuts
- `disabled.sqf`: Disabled

**Interdependencies**:
- Used by logistics system
- Uses ACE3 for ropes

---

### 14. BODY SYSTEM (body)
**Directory**: `core/fnc/body/`

**Main function**: Manages bodies and dogtags.

**Files**:
- `create.sqf`: Creates a body
- `get.sqf`: Retrieves a body
- `bagRecover.sqf`: Recovers backpack (client)
- `bagRecover_s.sqf`: Recovers backpack (server)
- `createMarker.sqf`: Creates marker
- `dogtagGet.sqf`: Retrieves dogtag
- `dogtagSet.sqf`: Sets dogtag
- `setBodyBag.sqf`: Puts in body bag

**Global variables**:
- `btc_body_deadPlayers`: Dead players

**Interdependencies**:
- Used by respawn system
- Uses `btc_rep_fnc_change` for reputation

---

### 15. RESPAWN SYSTEM (respawn)
**Directory**: `core/fnc/respawn/`

**Main function**: Manages respawn and tickets.

**Files**:
- `player.sqf`: Handles player respawn
- `playerConnected.sqf`: Player connected
- `addTicket.sqf`: Adds ticket
- `screen.sqf`: Respawn screen
- `force.sqf`: Forces respawn
- `intro.sqf`: Introduction

**Global variables**:
- `btc_respawn_tickets`: Respawn tickets
- `btc_p_respawn_ticketsAtStart`: Tickets at start

**Interdependencies**:
- Uses `btc_fob_fnc_*` for FOBs
- Uses `btc_body_fnc_*` for bodies

---

### 16. DOOR SYSTEM (door)
**Directory**: `core/fnc/door/`

**Main function**: Manages locked doors.

**Files**:
- `lock.sqf`: Locks a door
- `get.sqf`: Retrieves door state
- `break.sqf`: Breaks a door (client)
- `broke.sqf`: Breaks a door (server)

**Interdependencies**:
- Used by `btc_city_fnc_activate` to lock doors

---

### 17. CHEMICAL SYSTEM (chem)
**Directory**: `core/fnc/chem/`

**Main function**: Manages chemical warfare.

**Files**:
- `checkLoop.sqf`: Check loop
- `propagate.sqf`: Propagates chemical agents
- `handleShower.sqf`: Manages decontamination showers
- `damage.sqf`: Chemical damage (client)
- `damageLoop.sqf`: Damage loop
- `biopsy.sqf`: Biopsy
- `ehDetector.sqf`: Detector event handler
- `updateDetector.sqf`: Updates detector

**Global variables**:
- `btc_chem_contaminated`: Contaminated zones
- `btc_p_chem_sides`: Chemical warfare enabled

**Interdependencies**:
- Uses `btc_cache_fnc_create` for chemical caches
- Uses ACE3 Medical

---

### 18. SPECTRUM SYSTEM (spect)
**Directory**: `core/fnc/spect/`

**Main function**: Manages spectrum devices (electronic detection).

**Files**:
- `checkLoop.sqf`: Check loop
- `electronicFailure.sqf`: Electronic failure
- `updateDevice.sqf`: Updates device
- `frequencies.sqf`: Frequencies
- `disableDevice.sqf`: Disables device

**Global variables**:
- `btc_p_spect`: Spectrum system enabled

---

### 19. DEAFNESS SYSTEM (deaf)
**Directory**: `core/fnc/deaf/`

**Main function**: Manages temporary deafness after explosions.

**Files**:
- `earringing.sqf`: Ear ringing

---

### 20. TASK SYSTEM (task)
**Directory**: `core/fnc/task/`

**Main function**: Manages mission tasks.

**Files**:
- `create.sqf`: Creates a task
- `setState.sqf`: Sets state
- `setDescription.sqf`: Sets description (client)
- `abort.sqf`: Abandons a task (client)

**Global variables**:
- `btc_task_list`: Task list

**Interdependencies**:
- Used by all systems that create tasks

---

### 21. FLAG SYSTEM (flag)
**Directory**: `core/fnc/flag/`

**Main function**: Manages flags.

**Files**:
- `deploy.sqf`: Deploys a flag (client)
- `int.sqf`: Initializes flags

**Interdependencies**:
- Used by deployment system for checkpoints

---

### 22. TAG SYSTEM (tag)
**Directory**: `core/fnc/tag/`

**Main function**: Manages tagging (spray paint).

**Files**:
- `initArea.sqf`: Initializes tag area
- `eh.sqf`: Event handlers
- `create.sqf`: Creates a tag
- `vehicle.sqf`: Tag on vehicle

**Interdependencies**:
- Uses ACE3 Tagging

---

### 23. PATROL SYSTEM (patrol) - ENHANCED
**Directory**: `core/fnc/patrol/`

**Main function**: Manages enemy patrols with automatic system, strict military/civilian separation, configurable aggressive behavior, and dynamic waypoint updates.

**Files**:
- `autoSpawn.sqf`: Automatic creation system for military patrols (configurable timer)
- `init.sqf`: Initializes patrol routes between cities
- `addWP.sqf`: Adds waypoints with parameters according to type (military/civilian)
- `WPCheck.sqf`: Checks waypoints and reinitializes if necessary
- `playersInAreaCityGroup.sqf`: Checks player presence (deletion if too far)
- `usefulCity.sqf`: Smart destination selection (priority players > liberated cities > occupied cities)
- `eh.sqf`: Event handlers and deletion management
- `addEH.sqf`: Adds event handlers to groups
- `updateCombatWaypoints.sqf`: Dynamic waypoint update every 1 minute if in combat
- `hasPlayersNearby.sqf`: Detects player presence in radius (spawn zone exclusion)
- `getOutVehicle.sqf`: Vehicle exit management (passengers only if armed)

**Strict separation**:
- **Military patrols** (`btc_patrol_active`): Created only via automatic timer
  - Aggressive behavior: AWARE/RED/FULL/COLUMN
  - Departure cities: Only enemy-occupied cities (`occupied = true`)
  - Exclusion: No spawn within 1000m radius around players
  - Destinations: Priority player positions > liberated cities > occupied cities

- **Civilian patrols** (`btc_civ_veh_active`): Created during city activation
  - Passive behavior: CARELESS/BLUE/LIMITED/COLUMN
  - Departure/destination cities: Only liberated cities (`occupied = false`)
  - No spawn if no liberated city on map

**Global variables**:
- `btc_patrol_active`: List of active military patrols
- `btc_civ_veh_active`: List of active civilian patrols
- `btc_patrol_area`: Patrol radius (1500m default)
- `btc_p_patrol_timer`: Automatic creation interval (seconds, 0 = disabled)
- `btc_p_patrol_max`: Maximum number of military patrols
- `btc_p_patrol_vehicle_percent`: Percentage of patrols with vehicles (0-100%)
- `btc_p_patrol_exclusion_base_distance`: Base exclusion distance (500m-5000m, default: 1500m)
- `btc_patrol_exclusion_zones`: Custom exclusion zones (defined in `define_mod.sqf`)
- `btc_patrol_recent_cities`: List of recently selected cities (tracking to avoid repetitions)
- `btc_auto_patrol`: Variable marking automatic patrols (aggressive behavior)

**Advanced features**:
- **GETOUT waypoint**: Land vehicle patrols exit units before reaching destination (100m before)
  - Armed vehicles: Only passengers exit (driver/gunners stay)
  - Unarmed vehicles: All units exit
- **Dynamic update**: Waypoints recreated every 1 minute in combat to track players
- **Player zone exclusion**: No spawns within 1000m radius around players
- **Sensitive zone exclusion**: No spawns within 1500m radius around resource zones, FOBs, checkpoints
- **Configurable base exclusion**: No spawns within configurable radius around `btc_base` marker (500m-5000m, default: 1500m)
- **Custom exclusion zones**: Ability to add exclusion zones in `define_mod.sqf` via `btc_patrol_exclusion_zones`
- **Smart prioritization**: Patrols target player positions first, then liberated cities
- **City tracking**: Tracking system to prevent patrols from always heading to the same cities (exclusion of cities selected in the last 5 minutes)
- **City recapture**: Patrols can recapture liberated cities (40% chance) and create checkpoint missions (40% chance)

**Interdependencies**:
- Uses `btc_mil_fnc_create_patrol` to create military patrols
- Uses `btc_civ_fnc_create_patrol` to create civilian patrols
- Uses `btc_fnc_find_closecity` to find nearby cities
- Uses `btc_city_all` to access cities

---

### 24. VEHICLE SYSTEM (veh)
**Directory**: `core/fnc/veh/`

**Main function**: Manages vehicles.

**Files**:
- `init.sqf`: Initializes vehicles
- `add.sqf`: Adds a vehicle
- `addRespawn.sqf`: Adds respawn
- `respawn.sqf`: Respawns a vehicle
- `killed.sqf`: Handles destruction
- `propertiesGet.sqf`: Retrieves properties
- `propertiesSet.sqf`: Sets properties
- `inventoryRestore.sqf`: Restores inventory

**Global variables**:
- `btc_vehicles`: Vehicle list
- `btc_veh_respawnable`: Respawnable vehicles

---

### 25. ARSENAL SYSTEM (arsenal)
**Directory**: `core/fnc/arsenal/`

**Main function**: Manages arsenal.

**Files**:
- `data.sqf`: Arsenal data
- `garage.sqf`: Garage
- `loadout.sqf`: Loadout
- `trait.sqf`: Traits
- `ammoUsage.sqf`: Ammunition usage (client)
- `weaponsfilter.sqf`: Weapon filter (client)

**Interdependencies**:
- Uses ACE3 Arsenal or BI Arsenal

---

### 26. DATA SYSTEM (data)
**Directory**: `core/fnc/data/`

**Main function**: Manages shared data (headless).

**Files**:
- `add_group.sqf`: Adds a group
- `get_group.sqf`: Retrieves a group
- `spawn_group.sqf`: Spawns a group

**Interdependencies**:
- Used by headless system

---

### 27. DELAY SYSTEM (delay)
**Directory**: `core/fnc/delay/`

**Main function**: Manages delayed creations.

**Files**:
- `createUnit.sqf`: Creates a unit with delay
- `createVehicle.sqf`: Creates a vehicle with delay
- `createAgent.sqf`: Creates an agent with delay
- `exec.sqf`: Executes with delay
- `waitAndExecute.sqf`: Waits and executes

**Interdependencies**:
- Used to optimize spawns

---

### 28. SLOT SYSTEM (slot)
**Directory**: `core/fnc/slot/`

**Main function**: Manages slot serialization.

**Files**:
- `serializeState.sqf`: Serializes state
- `deserializeState.sqf`: Deserializes (client)
- `deserializeState_s.sqf`: Deserializes (server)
- `createKey.sqf`: Creates a key

**Interdependencies**:
- Used by save system

---

### 29. DATABASE SYSTEM (db)
**Directory**: `core/fnc/db/`

**Main function**: Manages save and load.

**Files**:
- `save.sqf`: Saves mission state
- `load.sqf`: Loads mission state
- `load_old.sqf`: Loads (old format)
- `delete.sqf`: Deletes a save
- `autoSaveLoop.sqf`: Auto-save loop
- `autoRestart.sqf`: Auto-restart
- `autoRestartLoop.sqf`: Restart loop
- `loadcargo.sqf`: Loads cargo
- `loadObjectStatus.sqf`: Loads object status
- `saveObjectStatus.sqf`: Saves object status
- `setTurretMagazines.sqf`: Sets turret magazines

**Global variables**:
- `btc_db_load`: Load on startup
- `btc_db_auto_save_enabled`: Auto-save enabled
- `btc_db_auto_save_interval`: Save interval

**Interdependencies**:
- Saves all mission systems

---

### 30. EVENT HANDLERS SYSTEM (eh)
**Directory**: `core/fnc/eh/`

**Main function**: Manages event handlers.

**Files**:
- `server.sqf`: Server event handlers
- `player.sqf`: Player event handlers
- `playerConnected.sqf`: Player connected
- `headless.sqf`: Headless event handlers
- `CuratorObjectPlaced.sqf`: Object placed via Zeus
- `trackItem.sqf`: Item tracking
- `xeh_PreInit_EH.hpp`: PreInit handlers
- `xeh_InitPost_EH_Vehicle.hpp`: InitPost vehicle handlers

**Interdependencies**:
- Used by all systems

---

### 31. COMMON SYSTEM (common)
**Directory**: `core/fnc/common/`

**Main function**: Common utility functions.

**Files**:
- `check_los.sqf`: Checks line of sight
- `create_composition.sqf`: Creates composition
- `house_addWP.sqf`: Adds waypoints in a house
- `house_addWP_loop.sqf`: House waypoint loop
- `set_damage.sqf`: Sets damage
- `road_direction.sqf`: Road direction
- `findsafepos.sqf`: Finds safe position
- `find_closecity.sqf`: Finds nearby city
- `delete.sqf`: Deletes objects
- `deleteEntities.sqf`: Deletes entities
- `final_phase.sqf`: Final phase
- `findposoutsiderock.sqf`: Finds position outside rock
- `typeOf.sqf`: Object type
- `roof.sqf`: Roof
- `moveOut.sqf`: Moves out
- `changeWeather.sqf`: Changes weather
- `get_cardinal.sqf`: Cardinal point
- `show_hint.sqf`: Shows hint
- `set_markerTextLocal.sqf`: Sets marker text
- `showSubtitle.sqf`: Shows subtitle
- `get_composition.sqf`: Retrieves composition
- `checkArea.sqf`: Checks area
- `typeOfPreview.sqf`: Preview type
- `get_class.sqf`: Retrieves class
- `randomize_pos.sqf`: Randomizes position
- `getHouses.sqf`: Retrieves houses
- `end_mission.sqf`: End mission
- `loadConfigFromParams.sqf`: Loads config from parameters
- `configUnitTypes.sqf`: Unit type configuration

**Interdependencies**:
- Used by all systems

---

### 32. INTERACTION SYSTEM (int)
**Directory**: `core/fnc/int/`

**Main function**: Manages interactions with civilians.

**Files**:
- `add_actions.sqf`: Adds actions
- `orders.sqf`: Orders
- `orders_give.sqf`: Gives orders
- `orders_behaviour.sqf`: Order behavior
- `shortcuts.sqf`: Keyboard shortcuts
- `terminal.sqf`: Terminal
- `foodGive.sqf`: Gives food
- `ordersLoop.sqf`: Orders loop
- `ask_var.sqf`: Requests variable
- `checkSirenBeacons.sqf`: Checks sirens/beacons
- `horn.sqf`: Horn

**Interdependencies**:
- Uses `btc_info_fnc_*` for information
- Uses `btc_rep_fnc_change` for reputation

---

### 33. DEBUG SYSTEM (debug)
**Directory**: `core/fnc/debug/`

**Main function**: Debug tools.

**Files**:
- `message.sqf`: Debug messages
- `marker.sqf`: Debug markers (client)
- `units.sqf`: Debug units (client)
- `fps.sqf`: FPS (client)
- `graph.sqf`: Graphs (client)
- `defines.hpp`: Definitions
- `dlg.hpp`: Interface

**Global variables**:
- `btc_debug`: Debug mode enabled
- `btc_debug_log`: Debug logs

---

## 🎮 CUSTOM SYSTEMS (LEON)

### 1. DEPLOYMENT SYSTEM (deploy)
**Directory**: `core/fnc/deploy/`

**Main function**: Deployment system for objects, units, and checkpoints with resource management.

**Key files**:

#### Initialization
- `init.sqf`: Initializes deployment system
  - Configures EventHandlers (`EntityCreated`, `CuratorObjectPlaced`)
  - Configures cargo for all vehicles
  - Initializes global variables
  - Starts monitoring systems

#### User Interface
- `initUI.sqf`: Initializes deployment interface
  - Defines object categories (fortifications, troops, checkpoints, crates)
  - Configures resource costs
  - Creates interactive menus

- `create.sqf`: Creates deployment interface (client)
  - Displays selection menu
  - Manages category navigation

- `create_apply.sqf`: Applies object creation
- `create_load.sqf`: Loads an object into a vehicle
- `create_change_target.sqf`: Changes deployment target
- `create_change_subcategory.sqf`: Changes subcategory

#### Placement and Preview
- `startPreviewMode.sqf`: Starts preview mode
- `previewLoop.sqf`: Preview loop (movement, rotation)
- `exitPreviewMode.sqf`: Exits preview mode
- `confirmPlacement.sqf`: Confirms placement
  - Creates final object
  - Deducts resources
  - Configures cargo if necessary
  - Applies inventories to crates
  - Adds actions

- `place_create_camera.sqf`: Creates placement camera
- `place_destroy_camera.sqf`: Destroys camera
- `place_key_down.sqf`: Handles keys (rotation, validation)

#### Object Management
- `deleteDeployedObject.sqf`: Deletes deployed object (with refund)
- `deleteDeployedObjectNoRefund.sqf`: Deletes without refund
- `redeplacerObjet.sqf`: Redeploys an object

#### Cargo Configuration
- `setupCargo.sqf`: Configures ACE cargo space for a vehicle
  - Supports all vehicle types (air, land, sea)
  - Customizable via `btc_deploy_customCargoClasses`
  - Manages containers (`Land_Cargo20_military_green_F`, `Land_Cargo40_military_green_F`)

- `cargoSizes.sqf`: Defines cargo sizes for deployable objects

#### Costs and Refunds
- `getObjectCosts.sqf`: Retrieves object costs
- `getRefundCategory.sqf`: Retrieves refund category
- `calculateCrateRefund.sqf`: Calculates crate refund
- `refundSingleCrate.sqf`: Refunds a crate (client)
- `refundSingleCrate_server.sqf`: Refunds (server)
- `refundSingleCrateFull.sqf`: Full refund

#### Troops and Units
- `addTroop.sqf`: Adds a troop
- `getUnitClasses.sqf`: Retrieves unit classes
- `loadouts.sqf`: Defines loadouts
- `loadoutConfig.sqf`: Loadout configuration
- `placeCarriedUnit.sqf`: Places carried unit
- `dropCarriedUnit.sqf`: Drops carried unit
- `createDroneCrew.sqf`: Creates drone crew
- `assignUnitToVehicle.sqf`: Assigns unit to vehicle
- `clearUnitVehicleAssignment.sqf`: Clears assignment
- `setUnitStance.sqf`: Sets unit stance
- `exitUnitFromVehicle.sqf`: Exits unit from vehicle
- `autoAssignUnits.sqf`: Automatically assigns units to vehicles/statics

#### Checkpoints
- `detectFlags.sqf`: Detects deployed flags and creates checkpoints
- `activateCheckpointTask.sqf`: Activates checkpoint task
- `monitorCheckpointTask.sqf`: Monitors task
- `completeCheckpointTask.sqf`: Completes task (success/failure)
- `deactivateCheckpointTask.sqf`: Deactivates task
- `monitorCheckpoints.sqf`: Monitors all active checkpoints
- `checkpointCounterAttack.sqf`: Counter-attack on checkpoint
- `manualCheckpointCounterAttack.sqf`: Manual counter-attack
- `monitorCounterAttacks.sqf`: Monitors checkpoint counter-attacks
- `syncCheckpoints.sqf`: Synchronizes checkpoints (JIP)
- `checkpointStatus.sqf`: Checkpoint status
- `manageNearestCity.sqf`: Manages nearest city to checkpoint

#### Liberation Zones
- `liberateZone.sqf`: Liberates zone around checkpoint

#### Resource Exchange
- `openResourceExchangeDialog.sqf`: Opens exchange dialog
- `initResourceExchangeDialog.sqf`: Initializes dialog
- `processResourceExchange.sqf`: Processes resource exchange

#### Repair
- `getVehicleRepairType.sqf`: Retrieves repair type
- `repairWreckWithResources.sqf`: Repairs wreck with resources

#### Editor Computers
- `initEditorComputers.sqf`: Initializes editor computers

#### Updates
- `updates.sqf`: System updates

**Global variables**:
- `btc_deploy_fortifications`: Available fortifications array
- `btc_deploy_troops`: Available troops array
- `btc_deploy_checkpoints`: Available checkpoints array
- `btc_deploy_activeCheckpoints`: Active checkpoints
- `btc_deploy_customCargoClasses`: Custom cargo configuration
- `btc_deploy_last_attacked_checkpoints`: Last attacked checkpoints

**Interdependencies**:
- Uses `btc_ressources_fnc_*` for resources
- Uses `btc_task_fnc_create` for tasks
- Uses `btc_mil_fnc_create_group` for counter-attacks
- Uses `btc_city_fnc_*` for cities

---

### 2. RESOURCE SYSTEM (ressources)
**Directory**: `core/fnc/ressources/`

**Main function**: Manages 4 resources (Iron, Ammunition, Food, Fuel) with generation zones, collection, and counter-attacks.

**Key files**:

#### Initialization
- `init.sqf`: Initializes resource system
  - Loads or creates zones
  - Initializes collection depots
  - Initializes collection areas
  - Starts all monitoring systems
  - Handles restoration after loading

- `initClient.sqf`: Initializes client
  - Creates user interface
  - Configures keyboard shortcuts

- `initDepots.sqf`: Initializes collection depots
  - Detects existing depots
  - Creates configured depots
  - Prevents duplicates

- `initCollectAreas.sqf`: Initializes collection areas
  - Creates visual barriers
  - Configures collection positions

#### Zone Management
- `createZones.sqf`: Creates resource zones
- `zoneManager.sqf`: Manages zones
- `activateZone.sqf`: Activates a zone
- `deactivateZone.sqf`: Deactivates a zone
  - Deletes all resource objects
  - Cleans variables
- `captureZone.sqf`: Captures a zone

#### Resource Generation
- `generationLoop.sqf`: Main generation loop
- `monitorZoneSlots.sqf`: Monitors zone slots
  - Progressively creates objects (1 per cycle)
  - Checks available slots
  - Stops if zone full or task active
- `generateResourceItem.sqf`: Generates a resource object
- `checkResourceItemSpace.sqf`: Checks available space

#### Collection
- `monitorDepots.sqf`: Monitors depots for automatic collection
  - Detects objects in radius
  - Checks they are not in cargo/carried
  - Excludes objects in collection area
  - Collects automatically
- `collectResourceItem.sqf`: Collects a resource object
  - Adds resources to total
  - Deletes object
  - Updates UI
- `addResourceItemActions.sqf`: Adds actions to objects
  - Manual carrying
  - Loading into vehicle
  - Position/rotation adjustment

#### Tasks
- `createCollectTask.sqf`: Creates collection task
- `monitorCollectTask.sqf`: Monitors collection task
  - Checks if zone is empty
  - Validates task when zone empty
  - Restarts generation
- `verifyCollectTasks.sqf`: Periodically checks tasks (5 min)
  - Creates missing tasks
  - Synchronizes variables

#### Defense
- `monitorZonesDefense.sqf`: Monitors zones for counter-attacks
- `activateDefenseTask.sqf`: Activates defense task
- `monitorDefenseTask.sqf`: Monitors defense task
  - Checks if zone is still controlled
  - Fails if zone recaptured
  - Success if defended

#### Counter-Attacks
- `counterAttackLoop.sqf`: Main counter-attack loop
  - Random zone selection
  - Avoids repetitions (memory of 5)
- `counterAttack.sqf`: Launches counter-attack
  - Selects hideout or random position
  - Creates enemy groups
  - Manages boats if spawn in water
  - Creates waypoints (beach → disembark → zone → patrol)
  - Checks distances to buildings (50m minimum)
  - Checks roads for vehicles
- `manualZoneCounterAttack.sqf`: Manual counter-attack

#### Resource Management
- `addResources.sqf`: Adds resources
- `setResources.sqf`: Sets resources
- `resetResources.sqf`: Resets resources
- `showResources.sqf`: Shows resources (client)
- `deductCost.sqf`: Deducts a cost
- `deductCostsArray.sqf`: Deducts multiple costs
- `checkCost.sqf`: Checks if a cost can be paid
- `getCosts.sqf`: Retrieves costs
- `syncCosts.sqf`: Synchronizes costs (multiplayer)
- `moneyManagement.sqf`: Money management (compiled)

#### User Interface
- `createUI.sqf`: Creates interface
- `updateUI.sqf`: Updates interface
- `updateUILoop.sqf`: Update loop
- `showUI.sqf`: Shows interface
- `hideUI.sqf`: Hides interface
- `keyHandler.sqf`: Handles keys
- `initDisplaySystem.sqf`: Initializes display system
- `getResourceIconType.sqf`: Retrieves resource icon

#### Collection Areas
- `createCollectAreaBarriers.sqf`: Creates visual barriers

#### Admin Commands
- `adminCommands.sqf`: Administrator commands
- `startAllGeneration.sqf`: Starts all generation
- `stopAllGeneration.sqf`: Stops all generation
- `toggleZoneGeneration.sqf`: Enables/disables zone generation

#### Monitoring
- `monitorZones.sqf`: Monitors zones to detect capture
- `monitorCache.sqf`: Monitors cache (obsolete?)

**Global variables**:
- `btc_ressources_zones_objects`: Zones array
- `btc_ressources_config`: Resource configuration
- `btc_ressources_amounts`: Current amounts [Iron, Ammunition, Food, Fuel, Money]
- `btc_ressources_collected_items`: Collected items
- `btc_ressources_collect_depots_objects`: Collection depots
- `btc_ressources_last_attacked_zones`: Last attacked zones (memory 5)
- `btc_ressources_last_used_hideouts`: Last used hideouts (memory 5)
- `btc_ressources_generation_rate`: Generation rate
- `btc_ressources_collect_area_size`: Collection area size
- `btc_ressources_collect_depot_radius`: Depot radius

**Interdependencies**:
- Used by deployment system for costs
- Uses `btc_mil_fnc_create_group` for counter-attacks
- Uses `btc_task_fnc_create` for tasks
- Uses `btc_hideouts` for counter-attacks

---

### 3. ACTION SYSTEM (addaction)
**Directory**: `core/fnc/addaction/`

**Main function**: Centralized management of all scroll wheel actions.

**Files**:
- `init.sqf`: Initializes action system
  - Configures `EntityCreated` EventHandler
  - Starts synchronization loop
  - Adds actions to existing objects

- `syncAllActions.sqf`: Synchronizes all actions (JIP)
- `syncActionsForObject.sqf`: Synchronizes object actions
- `addPlayerActions.sqf`: Adds player actions
- `addCheckpointActions.sqf`: Adds checkpoint actions

#### Admin Actions
- `adminReactivateCheckpoint.sqf`: Reactivates checkpoint (client)
- `adminReactivateCheckpointServer.sqf`: Reactivates (server)
- `adminReactivateZone.sqf`: Reactivates zone
- `adminToggleGeneration.sqf`: Enables/disables generation
- `adminReactivateTriggersCheckpoint.sqf`: Reactivates checkpoint triggers
- `adminReactivateTriggersZone.sqf`: Reactivates zone triggers
- `adminResetAllActions.sqf`: Resets all actions
- `adminVerifySystem.sqf`: Verifies system

#### Counter-Attack Actions
- `launchCheckpointCounterAttack.sqf`: Launches checkpoint counter-attack
- `launchZoneCounterAttack.sqf`: Launches zone counter-attack
- `launchFOBCounterAttack.sqf`: Launches FOB counter-attack

**Interdependencies**:
- Used by all systems with actions
- Strict separation between deployment and resource actions

---

### 4. UTILITY SYSTEM (utility)
**Directory**: `core/fnc/utility/`

#### 4.1 Configuration (LEON_config)
- `LEON_config.sqf`: General configuration of custom systems

#### 4.2 Drone (LEON_drone)
- `LEON_drone/init.sqf`: Initializes drone system
- `LEON_drone/drone.sqf`: Manages surveillance drone
- `LEON_drone/gui_classes.hpp`: Interface classes

#### 4.3 Halo (LEON_halo)
- `LEON_halo/LEON_Halo.sqf`: HALO jump system
- `LEON_halo/defines.hpp`: Definitions
- `LEON_halo/Halo_interface.hpp`: Interface
- `LEON_halo/Halo_sounds.hpp`: Sounds
- `LEON_halo/standard_controls.hpp`: Controls

#### 4.4 Teleporter (LEON_teleporteur)
- `LEON_teleporteur/teleporter.sqf`: Teleportation system

#### 4.5 Crate Carrying (LEON_portage)
- `LEON_portage/init.sqf`: Initializes crate carrying
- `LEON_portage/config_caisses.sqf`: Portable crate configuration
- `LEON_portage/functions/fn_initPortageCaisse.sqf`: Initializes carrying
- `LEON_portage/functions/fn_gererMouvementCaisse.sqf`: Manages movement

#### 4.6 Invincibility (LEON_invincible)
- `LEON_invincible/init.sqf`: Initializes invincibility
- `LEON_invincible/invincible.sqf`: Manages temporary invincibility

#### 4.7 Vehicles (LEON_vehicles)
- `LEON_vehicles/initCaisse.sqf`: Initializes supply crates
  - Defines `OPEX_fnc_*` functions
  - Configures custom inventories
  - Manages crates created via Zeus/editor
- `LEON_vehicles/initCaisseRepair.sqf`: Initializes repair crates
- `LEON_vehicles/medic_repa.sqf`: Medical/vehicle repair system
- `LEON_vehicles/vehicleInventory.sqf`: Vehicle inventory management

#### 4.8 Debug (LEON_debug)
- `LEON_debug/testSystem.sqf`: System tests
- `LEON_debug/verifySystem.sqf`: Verifies system
- `LEON_debug/closeDebugUI.sqf`: Closes debug UI
- `LEON_debug/debugCheckpoints.sqf`: Checkpoint debug
- `LEON_debug/createDocumentation.sqf`: Creates documentation
- `LEON_debug/verificationCohérence.sqf`: Coherence verification

#### 4.9 Test (LEON_test)
- `LEON_test/giveLoadout.sqf`: Gives loadout
- `LEON_test/disablePathOppfor.sqf`: Disables enemy pathfinding

---

### 5. VIRTUAL GARAGE SYSTEM (virtual_garage)
**Directory**: `core/fnc/virtual_garage/`

**Main function**: Virtual garage to store/retrieve vehicles.

**Files**:
- `init.sqf`: Initializes garage
- `fnc_openGarage.sqf`: Opens garage
- `fnc_openZenGarage.sqf`: Opens Zeus garage

**Interdependencies**:
- Uses save system to persist vehicles

---

## 🔄 INITIALIZATION FLOW

### Complete initialization sequence

1. **init.sqf** (root)
   - Compiles `core/def/mission.sqf` (definitions)
   - Compiles `define_mod.sqf` (mods)
   - If server: `core/init_server.sqf`
   - `core/init_common.sqf` (common)
   - `core/fnc/compile.sqf` (function compilation)
   - `core/fnc/deploy/init.sqf` (deployment)
   - If player: `core/init_player.sqf`
   - If headless: `core/init_headless.sqf`
   - `LEON_config` (configuration)
   - If interface: `LEON_drone_fnc_init`
   - If server: Initializes crates, vehicles, Halo

2. **core/init_server.sqf**
   - `btc_city_fnc_init` (cities)
   - Initializes dynamic groups
   - Configures time
   - Creates main tasks
   - If loading: `btc_db_fnc_load`
   - Else: Creates hideouts, cache, configures vehicles
   - `btc_eh_fnc_server` (event handlers)
   - `btc_ied_fnc_fired_near` (IED)
   - `btc_chem_fnc_checkLoop` (chemical)
   - `btc_spect_fnc_checkLoop` (spectrum)
   - `btc_db_fnc_autoRestartLoop` (auto-restart)
   - If auto-save: `btc_db_fnc_autoSaveLoop`
   - `btc_deploy_fnc_init` (deployment)
   - Waits for deployment initialization
   - `btc_deploy_fnc_monitorCheckpoints` (checkpoint monitoring)
   - `btc_deploy_fnc_monitorCounterAttacks` (checkpoint counter-attacks)
   - `btc_ressources_fnc_init` (resources)
   - Configures vehicle respawn
   - If side missions: `btc_side_fnc_create`
   - Configures custom tags
   - Configures respawn tickets

3. **core/init_common.sqf**
   - Common server/client configuration

4. **core/init_player.sqf**
   - Player-specific configuration
   - User interface
   - Player event handlers

5. **core/init_headless.sqf**
   - Headless client configuration

---

## 🔗 INTERDEPENDENCIES

### Critical dependencies

**Resource System**:
- Depends on: `btc_mil_fnc_create_group`, `btc_task_fnc_create`, `btc_hideouts`
- Used by: Deployment system (costs)

**Deployment System**:
- Depends on: Resource system (costs), `btc_task_fnc_create`, `btc_mil_fnc_create_group`, `btc_city_fnc_*`
- Used by: Actions, save

**City System**:
- Depends on: `btc_mil_fnc_create_group`, `btc_civ_fnc_populate`, `btc_rep_fnc_change`
- Used by: All spawn systems

**Cache System**:
- Depends on: `btc_city_all`, `btc_info_fnc_cache`
- Used by: Main task system

**Hideout System**:
- Depends on: None
- Used by: Counter-attacks (resources, checkpoints, FOB), `btc_info_fnc_hideout`

**Save System**:
- Depends on: All systems
- Used by: Initialization on startup

---

## 📊 MAIN GLOBAL VARIABLES

### Base system
- `btc_version`: Mission version [0, 2, 1]
- `btc_player_side`: Player side
- `btc_enemy_side`: Enemy side
- `btc_city_all`: HashMap of all cities
- `btc_hideouts`: Hideout array
- `btc_cache_obj`: Current cache object
- `btc_global_reputation`: Global reputation
- `btc_type_units`: Enemy unit types
- `btc_type_vehicles`: Enemy vehicle types
- `btc_type_boats`: Enemy boat types
- `btc_vehicles`: Vehicle list
- `btc_fobs`: FOB array
- `btc_respawn_tickets`: Respawn tickets

### Deployment system
- `btc_deploy_fortifications`: Available fortifications
- `btc_deploy_troops`: Available troops
- `btc_deploy_checkpoints`: Available checkpoints
- `btc_deploy_activeCheckpoints`: Active checkpoints
- `btc_deploy_customCargoClasses`: Custom cargo configuration
- `btc_deploy_last_attacked_checkpoints`: Last attacked checkpoints

### Resource system
- `btc_ressources_zones_objects`: Resource zones
- `btc_ressources_config`: Resource configuration
- `btc_ressources_amounts`: Amounts [Iron, Ammunition, Food, Fuel, Money]
- `btc_ressources_collected_items`: Collected items
- `btc_ressources_collect_depots_objects`: Collection depots
- `btc_ressources_last_attacked_zones`: Last attacked zones
- `btc_ressources_last_used_hideouts`: Last used hideouts

---

## 🎯 EVENT HANDLERS

### Server EventHandlers (`btc_eh_fnc_server`)
- `EntityCreated`: Initializes created objects
- `EntityKilled`: Handles entity death
- `EntityRespawned`: Handles respawn

### Player EventHandlers (`btc_eh_fnc_player`)
- `CuratorObjectPlaced`: Object placed via Zeus
- `GetInMan`: Enter vehicle
- `GetOutMan`: Exit vehicle
- `Killed`: Player death
- `Respawn`: Player respawn

### Deployment EventHandlers
- `EntityCreated`: Configures vehicle cargo
- `CuratorObjectPlaced`: Configures Zeus objects

### Resource EventHandlers
- `EntityCreated`: Adds actions to resource objects

---

## 💾 SAVE/LOAD SYSTEM

### Save format

**Resource zones**: 17 elements
- Position, direction, name, type, config, slots, etc.

**Collected items**: 7 elements
- Position, direction, type, quantity, zone, etc.

**Cargo**: 5 elements
- Classname, position, direction, damage, content

**Checkpoints**: Specific format
- ID, position, flag, task, etc.

**FOB**: Specific format
- Position, direction, name, structure, etc.

**Cities**: Specific format
- ID, position, state, etc.

### Loading
- Checks save version
- Loads all systems in order
- Restores objects and their properties
- Restarts monitoring systems
- Synchronizes with `publicVariable`

---

## ⚠️ IMPORTANT NOTES

1. **Multiple interdependencies**: The base mission has many interdependencies. Any modification requires checking impacts.

2. **Multiplayer synchronization**: Use `publicVariable` to synchronize important variables.

3. **Performance**: Use `distanceSqr` instead of `distance`, filter before iterating, cache calculated values.

4. **Save format**: The format is strict. Do not modify without planning migration.

5. **Event Handlers**: Check execution order and avoid conflicts.

6. **Actions**: Strictly separate deployment and resource actions via `btc_ressources_item_valid`.

---

## 📝 DEVELOPMENT NOTES

- All scripts must have a header with author, description, version
- Use `diag_log` for debugging (with clear prefixes)
- Avoid `systemChat` and `dialog` (user preference)
- Use backslashes (`\`) for file paths
- Comments should be essential only
- Respect existing code format

---

**Document created**: December 10, 2025  
**Last update**: December 15, 2025  
**Mission version**: 0.2.1

