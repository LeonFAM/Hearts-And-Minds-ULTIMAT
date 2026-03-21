# Mission Settings Guide by File (EN) - detailed version

Goal: know exactly which file to edit, which variable to change, and when to use `param.hpp` vs `mission.sqf`.

---

## Decision Matrix (quick)

- Lobby/admin setting -> `core\def\param.hpp` (`btc_p_*`)
- Lobby parameter -> runtime conversion -> `core\fnc\common\loadConfigFromParams.sqf`
- Global mission constants / reputation / runtime tables -> `core\def\mission.sqf`
- Map/modset content (positions, classes, lists) -> `core\fnc\...` files + `define_mod.sqf`

---

## 1) `core\def\param.hpp`

### What it does
- Defines lobby-visible options (`class Params`).

### Precise settings (useful examples)
- Resources ON/OFF: `btc_p_resources_system` (0/1)
- Hideout/cache defense: `btc_p_veh_armed_ho`, `btc_p_veh_armed_ho_groups`, `btc_p_veh_armed_ho_units_min`, `btc_p_veh_armed_ho_units_max`, `btc_p_veh_armed_ho_vehicles`
- Densities: `btc_p_density_of_occupiedCity`, `btc_p_mil_group_ratio`, `btc_p_civ_group_ratio`, `btc_p_animals_group_ratio`
- Gameplay radii: `btc_p_checkpoint_radius`, `btc_p_checkpoint_danger_radius`, `btc_p_resources_zone_radius`, `btc_p_fob_radius`, `btc_p_fob_danger_radius`
- Side automation: `btc_p_airdrop_interval`, `btc_p_roadcheckpoint_interval`

### Rule
- If a `btc_p_*` exists, do not duplicate its value elsewhere.

---

## 2) `core\def\mission.sqf`

### What it does
- Reads `btc_p_*` and initializes `btc_*`.
- Contains runtime constants (cache/hideout/patrol/etc.) and reputation.

### Precise settings
- Side mission reputation (used by `configRewards.sqf`):
  - `btc_rep_bonus_side_supply`, `btc_rep_bonus_side_checkpoint`, etc.
- General gameplay reputation:
  - `btc_rep_bonus_*`, `btc_rep_malus_*`
- Hideout runtime:
  - `btc_hideout_safezone`, `btc_hideout_range`, `btc_hideout_cap_time`, `btc_hideouts_radius`
- Patrol runtime:
  - `btc_patrol_*` (runtime distances/exclusions/timers)
- Safety:
  - civ/enemy faction index clamp fallback (prevents out-of-range)

### When to edit here
- When you need to change global logic not exposed in lobby params.

---

## 3) `core\fnc\cache\find_pos.sqf`

### What it does
- Picks cache position inside a house in a city.

### Precise settings
- Priority city types: `NameVillage` + `NameLocal`
- House search radius: `_cachingRadius/2`
- Fallback: recursive retry if no house found

### Practical case
- Cache appears too urban: adjust allowed city types in `_useful`.

---

## 4) `core\fnc\city\init.sqf`

### What it does
- Creates all cities from terrain data + random initial occupation.

### Precise settings
- Included location types: `_citiesType`
- Computed city radius: `(RadiusA + RadiusB) * 0.75`, then clamp `[120..450]`
- Base exclusion:
  - `btc_base`: 2500m
  - `btc_base_1`: 1500m
- Initial occupation density input:
  - `_density_of_occupiedCity` (from `btc_p_density_of_occupiedCity`)
- Custom cities injection:
  - `btc_custom_loc` (from `define_mod.sqf`)

### Practical case
- Too many occupied cities at start -> reduce `btc_p_density_of_occupiedCity` in `param.hpp`.

---

## 5) `core\fnc\common\configUnitTypes.sqf`

### What it does
- Advanced resources config (zones/items) + unit/vehicle type overrides.

### Precise settings
- Resource starting stock:
  - `btc_resources_config` (Fuel/Ammo/Steel/Food/Money)
- Resource zones:
  - `btc_resources_zones` format `[[x,y,z], "Type", "Name", radius]`
- Collectable objects:
  - `btc_resources_collect_items_config`
- Radii:
  - `btc_resources_collect_area_size`
  - `btc_resources_collect_depot_radius`
  - `btc_resources_status_marker_offset`
- Resource/checkpoint/FOB combat overrides:
  - `btc_resources_counter_attack_vehicle_types`
  - `btc_resources_counter_attack_unit_types`
  - `btc_resources_zone_vehicle_types`
  - `btc_deploy_counter_attack_vehicle_types`
  - `btc_fob_counter_attack_vehicle_types`, `btc_fob_counter_attack_unit_types`

### Rule
- Gameplay numbers (chance/count/timer) -> `param.hpp`
- Map/mod positions and class lists -> `configUnitTypes.sqf`

---

## 6) `core\fnc\deploy\cargoSizes.sqf`

### What it does
- ACE cargo size table for deployable-loadable objects.

### Precise settings
- Main table:
  - `btc_deploy_cargoSizes = [["Classname", size], ...]`
- Lookup function:
  - `btc_deploy_fnc_getCargoSize`

### Practical case
- Deployable object cannot be loaded -> add classname here.

---

## 7) `core\fnc\deploy\loadoutConfig.sqf`

### What it does
- Template loadouts for deployed groups.

### Precise settings
- Role variables:
  - `btc_deploy_loadout_rifleman`
  - `btc_deploy_loadout_mg`
  - `btc_deploy_loadout_grenadier`
  - `btc_deploy_loadout_at`
  - `btc_deploy_loadout_aa`
  - `btc_deploy_loadout_te`
- Format:
  - `[primary, launcher, handgun, uniform, vest, backpack, helmet, glasses, bino, linkedItems]`

### Practical case
- Weapon modset changed -> update this file (not `param.hpp`).

---

## 8) `core\fnc\deploy\setupCargo.sqf`

### What it does
- Configures cargo capacity on vehicles/objects + loading actions.

### Precise settings
- Priority custom classes:
  - `btc_deploy_customCargoClasses = [["Classname", cargoSpace], ...]`
- Automatic fallback by family:
  - `Truck`, `MRAP`, `APC`, `Tank`, `Air`, `Ship`, etc.
- Functions:
  - `btc_deploy_fnc_setupCargo`
  - `btc_deploy_fnc_setupCargoSize`
  - `btc_deploy_fnc_addLoadAction`
  - `btc_deploy_fnc_addCargoCheckAction`

### Technical note
- Script tries loading `LEON_vehicules_*` (FR spelling),
  while `LEON_config.sqf` publishes `LEON_vehicles_*` (EN spelling).
- Auto-add of LEON classes may fail until names are aligned.

---

## 9) `core\fnc\hideout\create.sqf`

### What it does
- Creates hideouts, links them to a city, moves city to hideout position.

### Precise settings
- Candidate city selection:
  - excludes active cities, too close to base, already-hideout cities, restricted types
- Distances:
  - `btc_hideout_safezone`, `btc_hideout_minRange` (via mission runtime)
- City state:
  - `has_ho`, `occupied`, `ho_units_spawned`
- City move:
  - save `city_realPos`, then `setPos _pos`

### Practical case
- Hideout too close to base -> increase `btc_hideout_safezone` in `mission.sqf`.

---

## 10) `core\fnc\kp\KP-Cratefiller\KPCF_config.sqf`

### What it does
- Configures KPCF (interaction, crates, item lists).

### Precise settings
- Interaction objects:
  - `KPCF_cratefillerBase`
  - `KPCF_cratefillerSpawn`
- Distances:
  - `KPCF_spawnRadius`
  - `KPCF_interactRadius`
- Permissions:
  - `KPCF_canSpawnAndDelete`
- List mode:
  - `KPCF_generateLists`
  - `KPCF_blacklistedItems`
  - `KPCF_whitelistedItems`
- Restricted mode:
  - `KPCF_weapons`, `KPCF_grenades`, `KPCF_explosives`, `KPCF_items`, `KPCF_backpacks`
  - feeds from `btc_custom_arsenal` when present

---

## 11) `core\fnc\side\checkMissionRequirements.sqf`

### What it does
- Blocks mission creation if special prerequisites are missing.

### Precise settings
- Specially handled missions:
  - `convoy`, `capture_officer`
- Checks:
  - destination must be non-occupied
  - valid occupied departure city must exist
  - valid roads must exist near departure city

### Function return
- `[canCreate, errorMessage]`

---

## 12) `core\fnc\side\configRewards.sqf`

### What it does
- Mission reward table.

### Precise settings
- Format:
  - `[money, reputation, title, description]`
- Money:
  - fixed per mission in this file (`supply`, `convoy`, etc.)
- Reputation:
  - read via `btc_rep_bonus_side_*` (from `mission.sqf`)

### Practical case
- Double `rescue` money -> edit `configRewards.sqf`
- Double `rescue` reputation -> edit `mission.sqf` (`btc_rep_bonus_side_rescue`)

---

## 13) `core\fnc\side\missionRestrictions.sqf`

### What it does
- Defines where/when each mission can appear.

### Precise settings
- Return structure:
  - `[excludedTypes, requiresOccupied, requiresMarine, requiresInactive, customCheck]`
- Examples:
  - `underwater_generator` -> `requiresMarine = true`
  - `pandemic` -> inactive city + minimum civs
  - `removeRubbish` -> minimum roadside IEDs

### Rule
- To change mission availability logic, edit this file first.

---

## 14) `core\fnc\utility\LEON_portage\config_caisses.sqf`

### What it does
- List of manually portable crates.

### Precise settings
- `LEON_ammo_crates = ["Class1", "Class2", ...]`

---

## 15) `core\fnc\utility\LEON_vehicles\initCaisse.sqf`

### What it does
- Initializes crate contents (mission placed + dynamic spawn + Zeus).

### Precise settings
- Content tables:
  - `Box_NATO_WpsLaunch_F_WeaponsList`
  - `Box_NATO_WpsLaunch_F_MagazinesList`
  - `B_CargoNet_01_ammo_F_ItemsList`
  - `ACE_medicalSupplyCrate_advanced_ItemsList`
  - etc.
- Anti-double init:
  - `crate_initialized`
- Auto hooks:
  - periodic loop
  - `EntityCreated`
  - `CuratorObjectPlaced`

---

## 16) `core\fnc\utility\LEON_config.sqf`

### What it does
- LEON utility hub.

### Precise settings
- Vehicle loadouts:
  - `LEON_vehicles_loadouts`
- Tracked vehicle classes:
  - `LEON_vehicles_semi_armored`
  - `LEON_vehicles_armored`
  - `LEON_vehicles_heli`
  - `LEON_vehicles_plane`
- Intervals:
  - `LEON_monitoring_interval_*`
- HALO:
  - `LEON_halo_height`, `LEON_halo_aircraft_offset`, `LEON_halo_parachute_delay`, `LEON_halo_landing_delay`

---

## 17) `define_mod.sqf`

### What it does
- Terrain/modset customization.

### Precise settings
- Custom cities:
  - `btc_custom_loc` format `[[x,y,z], type, name, radius, occupied]`
- Patrol exclusion:
  - `btc_patrol_exclusion_zones` format `[[x,y,z], distance]`
- Arsenal:
  - `btc_custom_arsenal = [weapons, mags, items, backpacks]`
- Player loadout:
  - `btc_arsenal_loadout`

### Practical case
- New terrain port -> start here before changing system scripts.

---

## 18) `description.ext`

### What it does
- Root mission configuration.

### Precise settings
- UI/dialog includes:
  - deploy, side, fob, debug, KPCF, halo/drone
- CBA hooks:
  - `Extended_PreInit_EventHandlers`
  - `Extended_InitPost_EventHandlers`
- Respawn:
  - `respawn`, `respawnDelay`, `respawnTemplates`
- Managers:
  - `wreckManagerMode = 0`, `corpseManagerMode = 0`

---

## Must-add related files

### `core\fnc\common\loadConfigFromParams.sqf`
- Main parameter-to-runtime bridge:
  - `btc_resources_disabled`
  - `btc_deploy_counter_*`
  - `btc_fob_counter_*`
  - `btc_resources_*`

### `core\fnc\city\activate.sqf`
- Runtime application of densities and hideout/cache defenses.
- Uses `btc_p_veh_armed_ho*` + `btc_p_veh_armed_spawn_more`.

### `core\fnc\deploy\initUI.sqf`
- Player-facing deploy catalog and costs.

### `core\fnc\deploy\getUnitClasses.sqf`
- Deployable troop classes (modset dependent).

### `core\fnc\side\getAvailableMissions.sqf`
- Final city-based mission availability selection.

### `core\fnc\virtual_garage\init.sqf`
- Garage logic (zone/garage objects, ACE action, vehicle detection).
- Important to align real use of `btc_p_garage`.

---

## Use cases (if you want X -> edit Y)

- Enable/disable full resources system -> `param.hpp` (`btc_p_resources_system`)
- Change side mission reputation -> `mission.sqf` (`btc_rep_bonus_side_*`)
- Change side mission money -> `side\configRewards.sqf`
- Add a custom city -> `define_mod.sqf` (`btc_custom_loc`)
- Change mission availability restrictions -> `side\missionRestrictions.sqf`
- Adjust exact crate contents -> `LEON_vehicles\initCaisse.sqf`
- Make a new deployable object loadable -> `deploy\cargoSizes.sqf`
- Change resource zone positions -> `common\configUnitTypes.sqf`
- Adjust hideout/cache defender counts -> `param.hpp` (`btc_p_veh_armed_ho_*`)
