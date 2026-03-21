## Configuration Guide (EN) - code-audited 2026

This guide lists the files that actually drive the mission, with the exact parameter names used by the scripts.

**Golden rule**
- If a setting exists as `btc_p_*` in `core\def\param.hpp`, configure it in `param.hpp`.
- `.sqf` files should mainly contain logic, not duplicate lobby values.

---

## 1) Entry point and init order

### `init.sqf`
- Starts `core\init.sqf`, then LEON modules (`LEON_portage`, `LEON_invincible`, `LEON_arsenal`, addactions).
- `core\fnc\deploy\autoAssignUnits.sqf` runs **server-side only**.

### `core\init.sqf`
- Loads `core\def\mission.sqf` and `define_mod.sqf`.
- Runs `core\init_server.sqf` (server), `core\init_player.sqf` (client), `core\init_headless.sqf` (HC).
- Calls `btc_deploy_fnc_init` (function compilation already happens in PreInit via `description.ext` / CBA XEH).

### `core\init_common.sqf`
- Currently empty (stub).

---

## 2) Mission parameters (single source of truth)

### `core\def\param.hpp`
- Contains all lobby-exposed `btc_p_*`.
- **Exact resource toggle name**: `btc_p_resources_system` (not `btc_p_ressources_system`).
- Also includes the new hideout/cache defense parameters:
  - `btc_p_veh_armed_ho`
  - `btc_p_veh_armed_ho_groups`
  - `btc_p_veh_armed_ho_units_min`
  - `btc_p_veh_armed_ho_units_max`
  - `btc_p_veh_armed_ho_vehicles`

### `core\def\mission.sqf`
- Reads `btc_p_*` and initializes global runtime variables `btc_*`.
- Contains reputation, global distances/radii, and base runtime tables.
- Faction indexes are safeguarded (fallback when out of bounds + `diag_log`).

---

## 3) Computed parameters and global systems

### `core\fnc\common\loadConfigFromParams.sqf`
- Converts lobby parameters into runtime variables:
  - checkpoints: `btc_deploy_counter_*`
  - FOB: `btc_fob_counter_*`
  - resources: `btc_resources_*`
- Builds `btc_resources_disabled` from:
  - `btc_p_resources_system` (`0` => no resources, `1` => full system)

### `core\fnc\common\configUnitTypes.sqf`
- Configures unit/vehicle types used by AI, resources, and counter-attacks.
- Also acts as advanced resource config (zones, collectable items, depots).

---

## 4) Cities, hideouts, cache

### `core\fnc\city\init.sqf`
- Creates cities from `CfgWorlds`, applies occupation density.
- Injects custom cities from `define_mod.sqf` via `btc_custom_loc`.

### `core\fnc\city\activate.sqf`
- Handles AI/civilian/patrol/cache runtime spawns.
- Dedicated hideout/cache defenders use:
  - `btc_p_veh_armed_ho` (+ groups/units/vehicles)
- `spawn_more` logic (`btc_p_veh_armed_spawn_more`) remains separate.

---

## 5) Deployment

### `core\fnc\deploy\initUI.sqf`
- Deployable catalog (objects, categories, costs).
- In no-resources mode (`btc_resources_disabled`), removes specific objects from the menu.

### `core\fnc\deploy\getUnitClasses.sqf`
- Deployable troop classes + associated costs.

### `core\fnc\deploy\loadoutConfig.sqf` and `core\fnc\deploy\loadouts.sqf`
- Role -> loadout mapping and detailed kit contents.

### `core\fnc\deploy\cargoSizes.sqf`
- ACE cargo/logistics sizes for deployable objects and vehicles.

---

## 6) Resources

### Key files
- `core\fnc\ressources\init.sqf`
- `core\fnc\ressources\zoneManager.sqf`
- `core\fnc\ressources\monitorZones.sqf`
- `core\fnc\ressources\counterAttack.sqf`

### Rules
- Numeric balancing: first in `param.hpp`.
- Parameter-to-runtime conversion logic: `loadConfigFromParams.sqf`.
- Deep behavior changes: edit `core\fnc\ressources\*.sqf`.

---

## 7) Side missions

### `core\fnc\side\configRewards.sqf`
- Reward format:
  - `[money, reputation, title, description]`
- Reputation values come from `btc_rep_bonus_side_*` (in `core\def\mission.sqf`).

### `core\fnc\side\getAvailableMissions.sqf`
- Filters mission availability by city type, occupation, sea/chemical flags, and custom restrictions.

---

## 8) Map/modset customization

### `define_mod.sqf`
- Custom cities/bases (`btc_custom_loc`)
- Patrol exclusion zones (`btc_patrol_exclusion_zones`)
- Global LEON arsenal/loadouts

### `core\fnc\kp\KP-Cratefiller\KPCF_config.sqf`
- KPCF crate presets and contents.

### `core\fnc\utility\LEON_*.sqf`
- Optional LEON modules (fuel, drone, arsenal, vehicles, etc.).

---

## 9) Virtual garage

### `core\fnc\virtual_garage\init.sqf`
- Garage setup and ACE interaction actions.
- Works through mission objects (`btc_create_virtual_garage*`, `btc_create_virtual_garage_zone*`).

---

## 10) Audit notes

- The guide previously used `btc_p_ressources_system`: **wrong name**. Correct is `btc_p_resources_system`.
- Runtime naming uses `btc_resources_*` (not `btc_ressources_*`).
- `btc_p_garage` exists in `param.hpp` and is read in `mission.sqf`, but is not actively used elsewhere yet.

---

## Final reminder

- All `btc_p_*` => `core\def\param.hpp`.
- To adapt a map/modset:
  - `param.hpp` (lobby settings)
  - `define_mod.sqf` (terrain customization)
  - `configUnitTypes.sqf`, `initUI.sqf`, `loadouts.sqf` (gameplay content)
- Only edit system scripts when you need behavior changes, not simple value tuning.
