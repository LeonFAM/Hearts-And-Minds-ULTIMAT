# Hearts and Minds ULTIMATE - Player Guide

![Version](https://img.shields.io/badge/Version-0.1.7-blue)
![Status](https://img.shields.io/badge/Status-Production-green)

**Author**: [13RDPA] LEON  
**Based on**: Hearts & Minds 2.1.4 by Vdauphin (BTC_clan)

---

## Introduction

Hearts and Minds ULTIMATE is a persistent cooperative mission for Arma 3. Your objective is to liberate the map by conquering cities, destroying enemy weapon caches, and winning the support of the local population. This ULTIMATE version adds numerous custom systems to enrich gameplay and offer an in-depth tactical experience.

The mission automatically saves your progress. You can leave and come back later to continue where you left off.

---

## Prerequisites and Installation

### Required Mods

This mission requires the following mods to function properly:

- **CBA_A3** (Community Base Addons)
- **ACE3** (Advanced Combat Environment)
- A recommended mod pack is available in the mission folder

### How to Join

1. Make sure you have all required mods installed and activated
2. Connect to the server via Arma 3's multiplayer browser
3. Select your starting slot in the briefing screen
4. The mission automatically loads the saved progress

---

## Getting Started

### First Steps

1. **Choose your role**: At the beginning of the mission, select a slot corresponding to your preferred role (Rifleman, Medic, Engineer, etc.)

2. **Familiarize yourself with the base**: You start at a Forward Operating Base (FOB). Explore it to find:
   - The arsenal to customize your equipment
   - Available vehicles
   - The deployment crate to create checkpoints and defenses

3. **Check the map**: Open your map to see:
   - Cities to liberate (red markers = occupied, blue = liberated)
   - Weapon caches to destroy (markers with cache icon)
   - Your position and that of your team

4. **Plan your actions**: Coordinate with your team to decide which area to attack first

### Basic Actions

**Actions on Deployed Objects** (Mouse wheel):
- All actions on objects you deploy (checkpoints, defenses, etc.) are done via the **mouse wheel** (action menu)
- Approach a deployed object and scroll the wheel to see available actions

**System Actions** (ACE Menu):
- System actions like managing missions, saving, launching a manual counter-attack, or displaying resources are done via the **ACE menu** (default: Windows key or ACE interaction)
- These actions are usually available on yourself or on specific objects

---

## Mission Systems

### ULTIMATE Custom Systems

#### Deployment System
The deployment system allows you to create tactical structures on the field using a deployment crate. You can deploy checkpoints, defenses (bunkers, machine gun nests, turrets), utility objects (helipad, barricades), mobile arsenals, teleportation points, complete FOBs, and HALO Jump points. Each object costs resources to deploy. Use the mouse wheel on the crate to open the deployment menu and select the desired object category.

**Custom Cargo Configuration** (For administrators/modders):
The ACE cargo system is automatically configured for all vehicles (created in editor, via Zeus, or deployed). By default, vehicles receive cargo capacity based on their type (trucks: 80 units, helicopters: 50-80 units, planes: 30-100 units, ships: 70-100 units, etc.).

To add custom vehicle classes with specific cargo values, edit the file `core\fnc\deploy\setupCargo.sqf` and add your classes to the `btc_deploy_customCargoClasses` list:

```sqf
btc_deploy_customCargoClasses = [
    ["B_Heli_Transport_03_F", 100],        // Heavy transport helicopter
    ["O_Truck_03_transport_F", 90],        // Transport truck
    ["I_Heli_light_03_F", 50],             // Light helicopter
    // Add your custom classes here
];
```

Custom classes have priority over generic rules. The format is: `["ClassName", cargoSpace]` where `ClassName` is the exact vehicle class name and `cargoSpace` is the cargo capacity in units.

#### Resource System
The mission uses an economic system based on 5 types of resources: Iron, Ammunition, Food, Fuel, and Money. You earn resources by capturing and controlling resource zones marked on the map. Each zone generates a specific type of resource over time. Resources are shared among all players and are consumed when deploying objects or purchasing equipment. You can exchange resources with each other via a dedicated menu. Display your current resources via the ACE menu.

#### Utility System
This system groups several practical features: surveillance drone (deploy a drone to observe an area), crate carrying (transport heavy crates on your back), improved ACE cargo management (load deployed objects into vehicles), and vehicle and crate inventory management. The cargo system is persistent, meaning vehicle contents are automatically saved. Use mouse wheel actions on objects to access these features.

#### Virtual Garage
The virtual garage allows you to store and retrieve vehicles. Instead of leaving vehicles scattered across the map, you can store them virtually in a FOB or checkpoint. To store a vehicle, use the mouse wheel action on a garage point (marked in FOBs). To retrieve a stored vehicle, use the same menu and select the desired vehicle from the list. Stored vehicles retain their inventory and damage.

---

### Hearts & Minds Base Systems

#### Arsenal
The arsenal system allows you to customize your equipment and weapons. You can access the arsenal in FOBs, at respawn points, or by deploying a mobile arsenal. The arsenal can be restricted according to mission parameters (full arsenal, limited arsenal, or ACE arsenal with restrictions). Your loadout can be saved and automatically reloaded at respawn if enabled in the parameters.

#### Body Management
When a player dies, their body remains on the field. You can carry your teammates' bodies to bring them back to a secure area. After a certain time (configurable), a marker appears on the map to indicate the body's position. This system encourages teamwork and casualty evacuation. Bodies can also be searched to recover equipment.

#### Weapon Caches
Weapon caches are main objectives of the mission. They are scattered across the map in buildings or hidden areas. You must find and destroy them by placing explosives on them. Getting information about their location requires interrogating civilians or searching buildings. Destroying all caches is one of the mission's victory objectives.

#### Chemical System
In certain areas, the enemy may use chemical weapons. These zones are dangerous and require CBRN equipment (suit and gas mask) to survive. The chemical system adds an extra level of difficulty and forces players to equip themselves properly before entering these zones. Caches can also be chemical depending on parameters.

#### Cities and Reputation
Each city on the map has a status: occupied (enemy), contested, or liberated (allied). To liberate a city, you must eliminate enemy forces present and maintain the area secured. Reputation with civilians influences their behavior: good reputation makes civilians cooperative and they can give you information. Bad reputation (caused by civilian casualties) makes the population hostile and attracts more enemies.

#### Civilians
Civilians are present in cities and on roads. They can provide you with information about cache locations or enemy patrols if you interrogate them (via ACE Interact). Protect civilians: killing them reduces your reputation and makes the mission harder. Civilians can also be disguised insurgents, stay vigilant.

#### Common Functions
This system groups functionalities used by several other systems: object spawn management, distance calculations, condition checks, group management, and other utilities. It runs in the background to ensure the mission runs smoothly. As a player, you don't directly interact with this system.

#### Save and Load
The mission automatically saves progress at regular intervals (configurable). The save includes: liberated cities, destroyed caches, resources, deployed objects, vehicles, and player inventory. At server restart, the mission automatically loads the last save. Administrators can also save manually via the ACE menu.

#### Hearing Protection
The hearing protection system simulates the effects of explosion and gunfire noise on hearing. After a close explosion or intensive shooting, your hearing will be temporarily reduced (tinnitus, muffled sounds). Wear hearing protection to reduce this effect. The system integrates with ACE3 for increased realism.

#### Debug and Administration
The debug system offers tools for administrators and testers. It allows displaying debug information on the map, checking system status, generating reports, and fixing problems during the game. Normal players don't have access to these features. Only administrators with appropriate permissions can use these tools.

#### Delay System
This system manages timers and cooldowns for various actions. For example, it prevents action spam, imposes respawn delays on certain vehicles, or limits the frequency of secondary mission appearances. It runs in the background and ensures gameplay balance by preventing abuse.

#### Doors
The door system manages the opening and closing of doors in buildings. Some doors can be locked and need to be broken (with explosives or a crowbar). Others can be opened normally. This system adds realism and tactical possibilities during building clearing operations.

#### Event Handler
This system manages global mission events: death management, damage, ammunition usage, vehicle entry/exit, etc. It is used by many other systems to trigger actions in response to specific events. It runs automatically in the background.

#### Flags
The flag system allows you to capture strategic positions by planting your flag there. Capture an enemy flag by approaching it and using the mouse wheel action. Captured flags serve as respawn and rally points. Defend your flags because the enemy can recapture them.

#### FOB (Forward Operating Base)
FOBs are forward bases that you can deploy on the field. A complete FOB includes: a respawn point, an arsenal, a virtual garage, a repair area, and defenses. Deploying a FOB costs a lot of resources but offers a strategic anchor point for your operations. You can have multiple active FOBs at the same time.

#### Enemy Hideouts
Hideouts are fortified enemy positions scattered across the map. They contain enemy groups, vehicles, and sometimes heavy weapons. Destroying hideouts weakens the enemy and reduces counter-attacks. Hideouts are marked on the map once discovered. They regenerate over time if not destroyed.

#### IED (Improvised Explosive Devices)
IEDs are improvised bombs placed by the enemy on roads, in cities, or near objectives. They are difficult to spot and can destroy vehicles or kill soldiers. Use mine detectors or advance carefully in risk areas. Engineers can defuse IEDs. The frequency of IED appearance depends on mission parameters.

#### Information System
This system manages the collection and distribution of information. Interrogate civilians, search buildings, or capture enemy prisoners to obtain intelligence on caches, patrols, or hideouts. The more information you collect, the easier it becomes to find your objectives. Information is shared with the whole team.

#### Intelligence
The intelligence system complements the information system by providing tactical data. It manages automatic objective discovery when you explore the map, enemy unit identification, and sharing this information between players. Some objectives only become visible after collecting enough intelligence.

#### Sling Loading
The sling loading system allows you to hook objects under a helicopter to transport them. You can transport light vehicles, supply crates, or deployed objects. The pilot uses mouse wheel actions to hook/unhook the load. Be careful when flying with a load as it affects helicopter maneuverability.

#### Logistics
The logistics system manages ammunition, fuel, and repair supply. You can create supply points by deploying crates or logistics vehicles. Vehicles can be repaired and resupplied from these points. Logistics is essential to maintain your forces operational during long missions.

#### Enemy Military
This system manages enemy force spawn and behavior. Enemies patrol in occupied cities, defend caches and hideouts, and launch counter-attacks. Enemy density and difficulty depend on mission parameters. Enemies use tactical AI and can call reinforcements.

#### Patrols (Enhanced System)
The patrol system has been significantly enhanced with two distinct types:
- **Automatic military patrols**: Created automatically via a configurable timer from enemy-occupied cities. They are very aggressive (combat mode, awareness, maximum speed) and target player positions first, then attempt to recapture liberated cities. They can be on foot or in vehicles according to a configurable percentage.
- **Civilian patrols**: Created during city activation, they travel only between cities liberated by players to simulate normal civilian traffic. They are non-aggressive and do not react to combat.

Military patrols avoid spawning within a 1000m radius around players to avoid immediate confrontations. If a patrol enters combat with you, its waypoints update automatically every 1 minute to follow your movements. Vehicle patrols make their units exit before reaching destination (passengers only if the vehicle is armed). The maximum number of patrols and vehicle percentage are configurable in mission parameters.

#### Reputation
Reputation measures your relationship with the civilian population. High reputation facilitates information collection and reduces hostility. Low reputation (caused by civilian casualties or collateral damage) increases enemy resistance and makes civilians hostile. Manage your reputation by minimizing civilian casualties and liberating cities.

#### Respawn
The respawn system manages player reappearance after death. You can respawn at the main base, in FOBs, or at captured flags depending on parameters. The mission uses a ticket system: you have a limited number of respawns (shared or individual). Earn additional tickets by freeing prisoners or completing objectives.

#### Side Missions
Side missions are optional objectives that appear regularly on the map. Mission types: hostage rescue, convoy destruction, HVT (high-value target) elimination, object recovery, etc. Completing side missions rewards you (resources, respawn tickets, information). You can configure mission types and rewards via a dedicated menu.

#### Slots and Roles
The slot system manages role selection at the beginning of the mission. Each slot corresponds to a specific role: Rifleman, Medic, Engineer, Squad Leader, etc. Some roles have special abilities (medics heal more effectively, engineers can defuse IEDs). Slots can be shared (multiple players can take the same role) depending on parameters.

#### Spectator
Spectator mode activates when you are dead and waiting to respawn. You can observe other players (if allowed by parameters), see their perspective, and follow the action. Spectator helps understand the tactical situation and plan your actions after respawn. Spectator options are configurable.

#### Tags and Markers
The tag system allows you to mark positions on the map for your team. Place markers to indicate enemy positions, areas to avoid, objectives, or rally points. Markers are visible to all players. Use them to improve communication and coordination.

#### Tasks and Objectives
The task system manages main and secondary mission objectives. Main tasks include: liberate all cities, destroy all caches, and improve reputation. Secondary tasks are dynamically generated. Check the task log (J by default) to see your current objectives and their progress.

#### Towing
The towing system allows you to attach one vehicle to another to tow it. Useful for moving damaged vehicles or transporting light vehicles. Use mouse wheel actions on a towing vehicle to attach/detach a vehicle. The towed vehicle automatically follows the towing vehicle.

#### Vehicles
The vehicle system manages mission vehicle spawn, persistence, and management. Vehicles can be deployed, repaired, resupplied, and stored in the virtual garage. Their inventory and condition are saved. Some vehicles have special capabilities (medical vehicles, repair vehicles, supply vehicles).

---

## Mission Objectives

### Main Objectives

1. **Liberate all cities**: Eliminate enemy forces in each city and secure the area
2. **Destroy all weapon caches**: Find and destroy enemy caches scattered across the map
3. **Maintain good reputation**: Avoid civilian casualties and win population support

### Secondary Objectives

- Complete side missions that appear dynamically
- Destroy enemy hideouts to weaken resistance
- Capture enemy flags to extend your control
- Build FOBs to establish permanent presence

### Victory Conditions

The mission is won when:
- All cities are liberated
- All weapon caches are destroyed
- Overall reputation is positive

---

## Tips and Tricks

### Getting Started Right

- **Work as a team**: Coordination is essential. Use radios and communicate your intentions
- **Manage your resources**: Don't waste resources by deploying too many unnecessary objects
- **Explore methodically**: Advance city by city rather than spreading out
- **Protect civilians**: Each civilian killed reduces your reputation and makes the mission harder

### Recommended Tactics

- **Reconnaissance first**: Use drones or scouts to spot enemies before attacking
- **Establish FOBs**: Create forward bases near your operation zones to reduce travel times
- **Secure resource zones**: Control resource zones to ensure a constant supply flow
- **Interrogate civilians**: Information is valuable. Talk to civilians to find caches faster

### Mistakes to Avoid

- Not saving regularly (use manual saves in addition to auto-save)
- Venturing alone far from the team (you'll be quickly overwhelmed)
- Ignoring enemy patrols (they can call reinforcements)
- Neglecting logistics (running out of ammunition in combat is dangerous)

---

## Support and Contact

**Discord**: https://discord.gg/aEJ7W6QwYr  
**GitHub**: https://github.com/LeonFAM/Hearts-And-Minds-ULTIMAT  
**Steam Workshop**: https://steamcommunity.com/sharedfiles/filedetails/?id=3580378812

To report bugs or suggest improvements, contact **[13RDPA] LEON** via Discord.

---

## Credits

**Hearts & Minds 2.1.4** - Vdauphin (BTC_clan) & community (APL-SA)  
**ULTIMATE Systems** - [13RDPA] LEON  
**Development assistance** - AI Claude (Anthropic)

---

**Good luck, soldiers. The population is counting on you.**

*Last update: December 2025 - Version 0.1.7*

**Complete documentation**: See files in `READ_ME/EN/` for more information on system configuration and architecture.

