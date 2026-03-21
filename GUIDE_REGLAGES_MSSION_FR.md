# Guide des reglages par fichier (FR) - version detaillee

But: savoir exactement quel fichier modifier, quelle variable toucher, et quand passer par `param.hpp` ou `mission.sqf`.

---

## Matrice de decision (rapide)

- Reglage lobby joueur/admin -> `core\def\param.hpp` (`btc_p_*`)
- Conversion param lobby -> runtime -> `core\fnc\common\loadConfigFromParams.sqf`
- Constantes globales mission / reputation / tables runtime -> `core\def\mission.sqf`
- Contenu map/modset (positions, classes, listes) -> fichiers `core\fnc\...` + `define_mod.sqf`

---

## 1) `core\def\param.hpp`

### Sert a quoi
- Definit les options visibles dans le lobby (`class Params`).

### Reglages precis (exemples utiles)
- Ressources ON/OFF: `btc_p_resources_system` (0/1)
- Hideout/cache defense: `btc_p_veh_armed_ho`, `btc_p_veh_armed_ho_groups`, `btc_p_veh_armed_ho_units_min`, `btc_p_veh_armed_ho_units_max`, `btc_p_veh_armed_ho_vehicles`
- Densites: `btc_p_density_of_occupiedCity`, `btc_p_mil_group_ratio`, `btc_p_civ_group_ratio`, `btc_p_animals_group_ratio`
- Rayons gameplay: `btc_p_checkpoint_radius`, `btc_p_checkpoint_danger_radius`, `btc_p_resources_zone_radius`, `btc_p_fob_radius`, `btc_p_fob_danger_radius`
- Side automation: `btc_p_airdrop_interval`, `btc_p_roadcheckpoint_interval`

### Regle
- Si un `btc_p_*` existe, ne pas dupliquer la valeur ailleurs.

---

## 2) `core\def\mission.sqf`

### Sert a quoi
- Lit les `btc_p_*` et initialise les `btc_*`.
- Contient les constantes runtime (cache/hideout/patrol/etc.) et la reputation.

### Reglages precis
- Reputation side missions (utilisee par `configRewards.sqf`):
  - `btc_rep_bonus_side_supply`, `btc_rep_bonus_side_checkpoint`, etc.
- Reputation gameplay general:
  - `btc_rep_bonus_*`, `btc_rep_malus_*`
- Hideout runtime:
  - `btc_hideout_safezone`, `btc_hideout_range`, `btc_hideout_cap_time`, `btc_hideouts_radius`
- Patrol runtime:
  - `btc_patrol_*` (distances/exclusions/timers runtime)
- Security:
  - fallback indices factions civ/enemy avec clamp (evite out-of-range)

### Quand modifier ici
- Quand tu veux changer la logique globale non exposee au lobby.

---

## 3) `core\fnc\cache\find_pos.sqf`

### Sert a quoi
- Choisit la position du cache dans une maison d'une ville.

### Reglages precis
- Villes prioritaires: filtre `NameVillage` + `NameLocal`
- Rayon recherche maisons: `_cachingRadius/2`
- Fallback: si pas de maison, re-appel de la fonction

### Cas pratique
- Cache trop "urbain": adapte les types de ville autorises dans `_useful`.

---

## 4) `core\fnc\city\init.sqf`

### Sert a quoi
- Cree toutes les villes du terrain + random occupation.

### Reglages precis
- Types pris en compte: `_citiesType`
- Rayon ville calcule: `(RadiusA + RadiusB) * 0.75`, puis clamp `[120..450]`
- Exclusion autour base:
  - `btc_base`: 2500m
  - `btc_base_1`: 1500m
- Densite occupation initiale:
  - entree: `_density_of_occupiedCity` (vient de `btc_p_density_of_occupiedCity`)
- Ajout villes custom:
  - `btc_custom_loc` (depuis `define_mod.sqf`)

### Cas pratique
- Trop de villes occupees au depart -> baisse `btc_p_density_of_occupiedCity` dans `param.hpp`.

---

## 5) `core\fnc\common\configUnitTypes.sqf`

### Sert a quoi
- Config avancee ressources (zones/objets) + overrides de types vehicules/unites.

### Reglages precis
- Stock depart ressources:
  - `btc_resources_config` (Fuel/Ammo/Steel/Food/Money)
- Zones ressources:
  - `btc_resources_zones` format `[[x,y,z], "Type", "Nom", rayon]`
- Objets collectables:
  - `btc_resources_collect_items_config`
- Rayons:
  - `btc_resources_collect_area_size`
  - `btc_resources_collect_depot_radius`
  - `btc_resources_status_marker_offset`
- Overrides combat ressources/checkpoint/FOB:
  - `btc_resources_counter_attack_vehicle_types`
  - `btc_resources_counter_attack_unit_types`
  - `btc_resources_zone_vehicle_types`
  - `btc_deploy_counter_attack_vehicle_types`
  - `btc_fob_counter_attack_vehicle_types`, `btc_fob_counter_attack_unit_types`

### Regle
- Chiffres gameplay (chance/nombre/timer) -> `param.hpp`
- Positions/listes map/mod -> `configUnitTypes.sqf`

---

## 6) `core\fnc\deploy\cargoSizes.sqf`

### Sert a quoi
- Taille ACE des objets chargeables (`ace_cargo_size` logique cote deployable).

### Reglages precis
- Tableau principal:
  - `btc_deploy_cargoSizes = [["Classname", size], ...]`
- Fonction lookup:
  - `btc_deploy_fnc_getCargoSize`

### Cas pratique
- Objet deployable non chargeable -> ajouter son classname ici.

---

## 7) `core\fnc\deploy\loadoutConfig.sqf`

### Sert a quoi
- Loadouts templates des groupes deployes.

### Reglages precis
- Variables role:
  - `btc_deploy_loadout_rifleman`
  - `btc_deploy_loadout_mg`
  - `btc_deploy_loadout_grenadier`
  - `btc_deploy_loadout_at`
  - `btc_deploy_loadout_aa`
  - `btc_deploy_loadout_te`
- Format:
  - `[primary, launcher, handgun, uniform, vest, backpack, helmet, glasses, bino, linkedItems]`

### Cas pratique
- Changement de mod armes -> adapte ce fichier (pas `param.hpp`).

---

## 8) `core\fnc\deploy\setupCargo.sqf`

### Sert a quoi
- Configure capacite des vehicules/objets + actions de chargement.

### Reglages precis
- Classes custom prioritaires:
  - `btc_deploy_customCargoClasses = [["Classname", cargoSpace], ...]`
- Fallback auto par famille:
  - `Truck`, `MRAP`, `APC`, `Tank`, `Air`, `Ship`, etc.
- Fonctions:
  - `btc_deploy_fnc_setupCargo`
  - `btc_deploy_fnc_setupCargoSize`
  - `btc_deploy_fnc_addLoadAction`
  - `btc_deploy_fnc_addCargoCheckAction`

### Attention technique
- Le script tente de charger des listes `LEON_vehicules_*` (orthographe FR),
  alors que `LEON_config.sqf` publie `LEON_vehicles_*` (orthographe EN).
- Donc l'auto-ajout des classes LEON peut ne pas se faire tant que les noms ne sont pas alignes.

---

## 9) `core\fnc\hideout\create.sqf`

### Sert a quoi
- Cree les hideouts, les rattache a une ville, deplace la ville sur la position hideout.

### Reglages precis
- Selection ville candidate:
  - exclut villes actives, trop proches base, deja hideout, type limite
- Distances:
  - `btc_hideout_safezone`, `btc_hideout_minRange` (via mission runtime)
- Etat ville:
  - `has_ho`, `occupied`, `ho_units_spawned`
- Deplacement ville:
  - sauvegarde `city_realPos`, puis `setPos _pos`

### Cas pratique
- Hideout trop proche base -> augmenter `btc_hideout_safezone` (dans `mission.sqf`).

---

## 10) `core\fnc\kp\KP-Cratefiller\KPCF_config.sqf`

### Sert a quoi
- Configure KPCF (interaction, crates, listes items).

### Reglages precis
- Objet interaction:
  - `KPCF_cratefillerBase`
  - `KPCF_cratefillerSpawn`
- Distances:
  - `KPCF_spawnRadius`
  - `KPCF_interactRadius`
- Autorisation:
  - `KPCF_canSpawnAndDelete`
- Mode listes:
  - `KPCF_generateLists`
  - `KPCF_blacklistedItems`
  - `KPCF_whitelistedItems`
- En mode restreint:
  - `KPCF_weapons`, `KPCF_grenades`, `KPCF_explosives`, `KPCF_items`, `KPCF_backpacks`
  - alimente depuis `btc_custom_arsenal` si present

---

## 11) `core\fnc\side\checkMissionRequirements.sqf`

### Sert a quoi
- Bloque la creation mission si preconditions specifiques absentes.

### Reglages precis
- Missions traitees specialement:
  - `convoy`, `capture_officer`
- Checks:
  - destination non occupee
  - existence ville depart valide/occupee
  - routes valides a la ville depart

### Retour fonction
- `[canCreate, errorMessage]`

---

## 12) `core\fnc\side\configRewards.sqf`

### Sert a quoi
- Table recompenses par mission.

### Reglages precis
- Format:
  - `[money, reputation, title, description]`
- Argent:
  - fixe ici par mission (`supply`, `convoy`, etc.)
- Reputation:
  - lue via `btc_rep_bonus_side_*` (dans `mission.sqf`)

### Cas pratique
- Doubler l'argent de `rescue` -> `configRewards.sqf`
- Doubler la reputation de `rescue` -> `mission.sqf` (`btc_rep_bonus_side_rescue`)

---

## 13) `core\fnc\side\missionRestrictions.sqf`

### Sert a quoi
- Definir ou/quand chaque mission peut apparaitre.

### Reglages precis
- Structure retour:
  - `[excludedTypes, requiresOccupied, requiresMarine, requiresInactive, customCheck]`
- Exemples:
  - `underwater_generator` -> `requiresMarine = true`
  - `pandemic` -> ville inactive + mini civils
  - `removeRubbish` -> mini IED routes

### Regle
- Pour changer la disponibilite d'une mission, modifie ce fichier avant tout.

---

## 14) `core\fnc\utility\LEON_portage\config_caisses.sqf`

### Sert a quoi
- Liste des caisses portables manuellement.

### Reglages precis
- `LEON_ammo_crates = ["Class1", "Class2", ...]`

---

## 15) `core\fnc\utility\LEON_vehicles\initCaisse.sqf`

### Sert a quoi
- Initialise le contenu des caisses (mission + spawn dynamique + Zeus).

### Reglages precis
- Tables contenu:
  - `Box_NATO_WpsLaunch_F_WeaponsList`
  - `Box_NATO_WpsLaunch_F_MagazinesList`
  - `B_CargoNet_01_ammo_F_ItemsList`
  - `ACE_medicalSupplyCrate_advanced_ItemsList`
  - etc.
- Anti-double init:
  - variable `crate_initialized`
- Hooks auto:
  - boucle periodique
  - `EntityCreated`
  - `CuratorObjectPlaced`

---

## 16) `core\fnc\utility\LEON_config.sqf`

### Sert a quoi
- Hub utilitaires LEON.

### Reglages precis
- Loadouts vehicules:
  - `LEON_vehicles_loadouts`
- Classes vehicules suivies:
  - `LEON_vehicles_semi_armored`
  - `LEON_vehicles_armored`
  - `LEON_vehicles_heli`
  - `LEON_vehicles_plane`
- Intervalles:
  - `LEON_monitoring_interval_*`
- HALO:
  - `LEON_halo_height`, `LEON_halo_aircraft_offset`, `LEON_halo_parachute_delay`, `LEON_halo_landing_delay`

---

## 17) `define_mod.sqf`

### Sert a quoi
- Personnalisation terrain/modset.

### Reglages precis
- Villes custom:
  - `btc_custom_loc` format `[[x,y,z], type, nom, rayon, occupied]`
- Exclusion patrouilles:
  - `btc_patrol_exclusion_zones` format `[[x,y,z], distance]`
- Arsenal:
  - `btc_custom_arsenal = [weapons, mags, items, backpacks]`
- Loadout joueur:
  - `btc_arsenal_loadout`

### Cas pratique
- Nouveau terrain -> commence ici avant de toucher les scripts systeme.

---

## 18) `description.ext`

### Sert a quoi
- Configuration racine mission.

### Reglages precis
- Includes UI/dialogues:
  - deploy, side, fob, debug, KPCF, halo/drone
- Hooks CBA:
  - `Extended_PreInit_EventHandlers`
  - `Extended_InitPost_EventHandlers`
- Respawn:
  - `respawn`, `respawnDelay`, `respawnTemplates`
- Managers:
  - `wreckManagerMode = 0`, `corpseManagerMode = 0`

---

## Fichiers lies a ajouter absolument

### `core\fnc\common\loadConfigFromParams.sqf`
- Fichier pivot parametres -> runtime:
  - `btc_resources_disabled`
  - `btc_deploy_counter_*`
  - `btc_fob_counter_*`
  - `btc_resources_*`

### `core\fnc\city\activate.sqf`
- Application runtime des densites et des defenses hideout/cache.
- Utilise `btc_p_veh_armed_ho*` + `btc_p_veh_armed_spawn_more`.

### `core\fnc\deploy\initUI.sqf`
- Catalogue deployable et couts visibles joueur.

### `core\fnc\deploy\getUnitClasses.sqf`
- Classes de troupes deployables (modset dependant).

### `core\fnc\side\getAvailableMissions.sqf`
- Selection finale des missions disponibles par ville.

### `core\fnc\virtual_garage\init.sqf`
- Logique garage (objets zone/garage, action ACE, detection vehicule).
- Important pour aligner l'usage de `btc_p_garage`.

---

## Cas d'usage (si tu veux X -> modifie Y)

- Activer/desactiver completement les ressources -> `param.hpp` (`btc_p_resources_system`)
- Changer la reputation d'une side mission -> `mission.sqf` (`btc_rep_bonus_side_*`)
- Changer l'argent d'une side mission -> `side\configRewards.sqf`
- Ajouter une ville custom -> `define_mod.sqf` (`btc_custom_loc`)
- Changer les restrictions d'apparition d'une mission -> `side\missionRestrictions.sqf`
- Ajuster contenu exact d'une caisse -> `LEON_vehicles\initCaisse.sqf`
- Rendre un nouvel objet deployable chargeable -> `deploy\cargoSizes.sqf`
- Changer les positions des zones ressources -> `common\configUnitTypes.sqf`
- Ajuster le nombre d'IA defenses hideout/cache -> `param.hpp` (`btc_p_veh_armed_ho_*`)
# Guide des reglages par fichier (FR)

Ce document explique **ou regler quoi** dans la mission, fichier par fichier.
Objectif: eviter les doublons et savoir quand passer par `param.hpp`, `mission.sqf`, ou le script cible.

---

## Regle rapide

- Si un reglage existe en `btc_p_*` -> regle-le dans `core\def\param.hpp`.
- `core\def\mission.sqf` lit ces `btc_p_*` et construit les variables runtime `btc_*`.
- Les fichiers `core\fnc\...` servent surtout a la logique et aux configurations avancees non exposees au lobby.

---

## Fichiers principaux demandes

### `core\def\param.hpp`
- **Role**: source de verite des parametres lobby (`btc_p_*`).
- **Regler ici**:
  - difficultes, densites, factions, timers, systeme ressources, hideout/cache, checkpoint/FOB, etc.
- **Canal recommande**: **`param.hpp` uniquement**.
- **Note**: nom correct du switch ressources = `btc_p_resources_system`.

### `core\def\mission.sqf`
- **Role**: charge les `btc_p_*`, initialise les globals `btc_*`, reputations, rayons, tables runtime.
- **Regler ici**:
  - constantes globales non exposees au lobby
  - tables techniques (types d'objets, multipliers batiments, listes de classes runtime)
  - bonus/malus reputation (`btc_rep_*`, `btc_rep_bonus_side_*`)
- **Canal recommande**:
  - valeur joueur/lobby -> `param.hpp`
  - logique globale avancee -> `mission.sqf`

### `core\fnc\cache\find_pos.sqf`
- **Role**: choisit la ville/maison pour placer le cache.
- **Regler ici**:
  - types de villes privilegiees (`NameVillage`, `NameLocal`)
  - logique fallback si aucune maison
- **Canal recommande**: script direct (pas `param.hpp`).
- **Dependances**: `btc_city_all`, `cachingRadius`, `btc_fnc_getHouses`.

### `core\fnc\city\init.sqf`
- **Role**: cree les villes depuis `CfgWorlds`, calcule rayon, randomise l'occupation initiale.
- **Regler ici**:
  - filtrage types de localites
  - bornes du rayon de ville
  - exclusion autour de `btc_base` / `btc_base_1`
- **Canal recommande**:
  - densite d'occupation -> `param.hpp` (`btc_p_density_of_occupiedCity`)
  - regles de creation ville -> `city\init.sqf`
- **Lien important**: integre aussi `btc_custom_loc` de `define_mod.sqf`.

### `core\fnc\common\configUnitTypes.sqf`
- **Role**: configuration avancee ressources + types vehicules/unites pour contre-attaques.
- **Regler ici**:
  - `btc_resources_config`, `btc_resources_zones`
  - objets de collecte, depots, rayons de collecte
  - overrides de types de vehicules/unites (`btc_*_vehicle_types`, `btc_*_unit_types`)
- **Canal recommande**:
  - quantites/chances/timers globaux -> `param.hpp`
  - positions/lists specifiques map/mod -> `configUnitTypes.sqf`

### `core\fnc\deploy\cargoSizes.sqf`
- **Role**: map `classname -> ace_cargo_size` pour objets deployables.
- **Regler ici**:
  - tableau `btc_deploy_cargoSizes`
  - fonction `btc_deploy_fnc_getCargoSize`
- **Canal recommande**: script direct.

### `core\fnc\deploy\loadoutConfig.sqf`
- **Role**: loadouts types des troupes deployees (rifleman, mg, grenadier, at, aa, te).
- **Regler ici**:
  - armes, items, medical, sac, viseurs, etc. par role
- **Canal recommande**: script direct.
- **A noter**: c'est un mapping role -> equipement, distinct de l'arsenal joueur.

### `core\fnc\deploy\setupCargo.sqf`
- **Role**: configure la capacite cargo ACE des vehicules/objets et ajoute actions de chargement.
- **Regler ici**:
  - classes custom `btc_deploy_customCargoClasses`
  - regles de fallback par type (`Truck`, `Car`, `Air`, `Ship`, etc.)
- **Canal recommande**: script direct.
- **Dependances**:
  - utilise listes `LEON_*` publiees par `LEON_config.sqf`
  - utilise `btc_deploy_fnc_getCargoSize` de `cargoSizes.sqf`.

### `core\fnc\hideout\create.sqf`
- **Role**: creation hideout, rattachement ville, deplacement de ville sur position hideout, triggers/markers.
- **Regler ici**:
  - logique de selection ville candidate
  - comportement si hideout restaure/deplace
  - creation markers hideout/debug
- **Canal recommande**:
  - nombre/scope hideout -> `param.hpp`/`mission.sqf`
  - logique de creation/deplacement -> `hideout\create.sqf`

### `core\fnc\kp\KP-Cratefiller\KPCF_config.sqf`
- **Role**: configuration KPCF (base d'interaction, crates, whitelist/blacklist, mode generatedLists).
- **Regler ici**:
  - `KPCF_cratefillerBase`, `KPCF_crates`, `KPCF_spawnRadius`, `KPCF_interactRadius`
  - mode restreint via `btc_custom_arsenal` (si `KPCF_generateLists = false`)
- **Canal recommande**: script direct.
- **Lien**: synchronise naturellement avec `define_mod.sqf` via `btc_custom_arsenal`.

### `core\fnc\side\checkMissionRequirements.sqf`
- **Role**: verifie preconditions speciales avant creation mission side (ex: convoy/capture_officer).
- **Regler ici**:
  - contraintes supplementaires de creation (routes, ville de depart, occupation)
- **Canal recommande**: script direct.
- **Lien**: s'appuie sur `missionRestrictions.sqf`.

### `core\fnc\side\configRewards.sqf`
- **Role**: table de recompenses side missions.
- **Regler ici**:
  - argent par mission (dans ce fichier)
  - reputation lue depuis `btc_rep_bonus_side_*` (dans `mission.sqf`)
- **Canal recommande**:
  - argent -> `configRewards.sqf`
  - reputation -> `mission.sqf`

### `core\fnc\side\missionRestrictions.sqf`
- **Role**: restrictions centralisees par mission (types de ville exclus, occupied/inactive/marine, custom check).
- **Regler ici**:
  - disponibilite mission par contexte
  - checks metier (IED mini, eglise, etc.)
- **Canal recommande**: script direct.

### `core\fnc\utility\LEON_portage\config_caisses.sqf`
- **Role**: liste des caisses portables a la main (`LEON_ammo_crates`).
- **Regler ici**: classes portables autorisees.
- **Canal recommande**: script direct.

### `core\fnc\utility\LEON_vehicles\initCaisse.sqf`
- **Role**: initialisation contenu des caisses (deployees, map, Zeus, EntityCreated).
- **Regler ici**:
  - listes d'objets et quantites par type de caisse
  - logique d'initialisation anti-double (`crate_initialized`)
- **Canal recommande**: script direct.

### `core\fnc\utility\LEON_config.sqf`
- **Role**: hub config utilitaires LEON (loadouts vehicules, classes suivies, intervalles monitoring, HALO, etc.).
- **Regler ici**:
  - `LEON_vehicles_loadouts`, classes vehicules par categorie
  - intervals `LEON_monitoring_interval_*`
  - variables HALO de base
- **Canal recommande**: script direct.

### `define_mod.sqf`
- **Role**: personnalisation map/modset (villes custom, zones exclusion patrouilles, arsenal/loadout joueur).
- **Regler ici**:
  - `btc_custom_loc`
  - `btc_patrol_exclusion_zones`
  - `btc_custom_arsenal` et `btc_arsenal_loadout`
- **Canal recommande**: script direct (pas `param.hpp`).

### `description.ext`
- **Role**: structure mission (includes UI/dialogs, CBA XEH PreInit/PostInit, respawn templates, managers).
- **Regler ici**:
  - includes interface/dialogues
  - comportement respawn global
  - options globales mission (`wreckManagerMode`, `corpseManagerMode`, etc.)
- **Canal recommande**: script config direct.

---

## Fichiers lies a ajouter (important)

Tu as bien cible les bons fichiers. Pour que ce soit complet, ajoute aussi:

### `core\fnc\common\loadConfigFromParams.sqf`
- Pont entre `param.hpp` et runtime (`btc_resources_disabled`, contre-attaques checkpoint/FOB/ressources, timers).
- C'est **le fichier cle** pour comprendre "param lobby -> variables effectives".

### `core\fnc\city\activate.sqf`
- Impact direct des params de densite + hideout/cache defense (`btc_p_veh_armed_ho*`) en runtime.

### `core\fnc\deploy\initUI.sqf`
- Catalogue deployable et couts affiches; filtre objets en no-resources.

### `core\fnc\deploy\getUnitClasses.sqf`
- Classes deployables par role/camp; indispensable si changement de modset.

### `core\fnc\side\getAvailableMissions.sqf`
- Selection finale des side missions en fonction de `missionRestrictions`.

### `core\fnc\virtual_garage\init.sqf`
- Logique operationnelle du garage (objets zone/garage, action ACE, detection vehicules).
- Utile car `btc_p_garage` existe dans `param.hpp/mission.sqf` mais son usage effectif doit etre aligne ici si tu veux le rendre vraiment actif.

---

## Methode conseillee de reglage

1. **Lobby gameplay**: passe par `core\def\param.hpp`.
2. **Constantes globales/metier**: ajuste `core\def\mission.sqf`.
3. **Contenu map/modset**: ajuste `define_mod.sqf`, `configUnitTypes.sqf`, `initUI.sqf`, `getUnitClasses.sqf`.
4. **Logiques avancees**: touche les scripts cibles (`side`, `deploy`, `hideout`, `cache`) seulement si la regle de jeu doit changer.

