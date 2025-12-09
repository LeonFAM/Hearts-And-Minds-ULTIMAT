# Guide de Configuration - Hearts and Minds ULTIMATE

![Version](https://img.shields.io/badge/Version-0.1.7-blue)

**Auteur**: [13RDPA] LEON  
**Date**: Décembre 2025

---

## Table des Matières

1. [Fichiers de Configuration Principaux](#fichiers-de-configuration-principaux)
2. [Configuration de Base](#configuration-de-base)
3. [Configuration des Systèmes Personnalisés](#configuration-des-systèmes-personnalisés)
4. [Configuration des Factions et Unités](#configuration-des-factions-et-unités)
5. [Configuration du Gameplay](#configuration-du-gameplay)
6. [Configuration Avancée](#configuration-avancée)

---

## Fichiers de Configuration Principaux

### Structure des Fichiers

```
Mission/
├── core/
│   ├── def/
│   │   ├── mission.sqf          ← Configuration principale (factions, réputation, etc.)
│   │   └── param.hpp            ← Paramètres de mission (interface serveur)
│   └── fnc/
│       ├── deploy/              ← Configuration des objets déployables
│       │   └── config.sqf       ← Objets, coûts, catégories
│       └── ressources/          ← Configuration des ressources
│           └── config.sqf       ← Types, zones, taux de génération
├── define_mod.sqf               ← Villes personnalisées, arsenal, loadouts
├── description.ext              ← Configuration générale de la mission
└── stringtable.xml              ← Traductions FR/EN
```

---

## Configuration de Base

### 1. `description.ext` - Configuration Générale

**Emplacement**: `description.ext` (racine de la mission)

**Ce que vous pouvez configurer:**

```sqf
// Informations de la mission
author = "[13RDPA] LEON";
onLoadName = "Hearts And Minds ULTIMATE";
onLoadMission = "Alpha Version 0.1.7";

// Image de chargement
loadScreen = "core\img\13rdpa.jpg";

// Console de debug (0 = désactivé, 1 = admin uniquement, 2 = tous)
enabledebugconsole = 1;

// Système de respawn
respawn = 3;                    // 3 = BASE respawn
respawnDelay = 2;               // Délai de respawn en secondes
respawnDialog = 0;              // 0 = pas de dialog
```

**Quand modifier:**
- Changement d'auteur, nom de mission, version
- Activation/désactivation de la console debug
- Modification des paramètres de respawn

---

### 2. `core\def\mission.sqf` - Configuration Principale

**Emplacement**: `core\def\mission.sqf`

**Ce que vous pouvez configurer:**

#### A. Version de la Mission

```sqf
btc_version = [
    0,      // Version majeure
    1,      // Version mineure
    7       // Version patch
];
```

#### B. Système de Réputation

```sqf
// Bonus de réputation (actions positives)
btc_rep_bonus_cache = 25;           // Destruction d'une cache
btc_rep_bonus_civ_hh = 5;           // Aide aux civils
btc_rep_bonus_disarm = 15;          // Désarmement IED
btc_rep_bonus_hideout = 35;         // Destruction planque

// Malus de réputation (actions négatives)
btc_rep_malus_civ_killed = -50;     // Tuer un civil
btc_rep_malus_civ_injured = -15;    // Blesser un civil
btc_rep_malus_building_damaged = -5; // Dégâts bâtiments

// Seuils de réputation
btc_rep_level_very_low = -500;
btc_rep_level_low = -200;
btc_rep_level_normal = 200;
btc_rep_level_high = 500;
btc_rep_level_very_high = 1000;
```

#### C. Configuration IED

```sqf
btc_ied_range = 8;                  // Distance de détection (mètres)
btc_ied_offset = -0.08;             // Offset vertical de la mine
btc_ied_suic_time = 900;            // Délai entre suiciders (secondes)
```

#### D. Configuration Patrouilles

```sqf
btc_patrol_area = 1500;             // Rayon de patrouille (mètres) - Zone de détection joueurs
btc_p_patrol_timer = 60;            // Intervalle création automatique (secondes, 0 = désactivé)
btc_p_patrol_max = 10;              // Nombre maximum de patrouilles militaires
btc_p_patrol_vehicle_percent = 50;  // Pourcentage de patrouilles avec véhicules (0-100)
```

#### E. Faction Ennemie

```sqf
// La faction est sélectionnée via param.hpp, mais vous pouvez forcer ici:
// _p_en = "OPF_F";  // CSAT
// _p_en = "OPF_G_F"; // FIA
// etc.

// Ajout de véhicules spécifiques à certaines factions
switch (_p_en) do {
    case "OPF_G_F" : {
        btc_type_motorized_armed = btc_type_motorized_armed + ["I_Heli_light_03_F"];
    };
};
```

**Quand modifier:**
- Ajuster la difficulté (réputation, IED)
- Changer les valeurs de réputation
- Personnaliser les factions ennemies

---

### 3. `core\def\param.hpp` - Paramètres de Mission

**Emplacement**: `core\def\param.hpp`

**Ce que vous pouvez configurer:**

Ces paramètres sont accessibles via l'interface de paramètres du serveur lors de la création de la mission.

#### Catégories Principales:

```cpp
// Temps et sauvegarde
class btc_p_time { /* Heure de départ */ };
class btc_p_load { /* Charger sauvegarde */ };
class btc_p_auto_db { /* Sauvegarde automatique */ };

// Respawn
class btc_p_respawn_location { /* Point de respawn */ };
class btc_p_respawn_ticketsAtStart { /* Tickets de départ */ };

// Faction ennemie
class btc_p_en { /* Faction ennemie */ };
class btc_p_AA { /* Véhicules AA */ };
class btc_p_tank { /* Véhicules lourds */ };

// Densité des unités
class btc_p_density_of_occupiedCity { /* Villes occupées */ };
class btc_p_mil_group_ratio { /* Densité ennemis */ };
class btc_p_civ_group_ratio { /* Densité civils */ };

// Patrouilles
class btc_p_patrol_max { /* Nombre maximum de patrouilles militaires (0-30) */ };
class btc_p_civ_max_veh { /* Nombre maximum de patrouilles civiles (0-30) */ };
class btc_p_patrol_timer { /* Timer de création automatique (secondes, 0 = désactivé) */ };
class btc_p_patrol_vehicle_percent { /* Pourcentage de patrouilles avec véhicules (0-100%) */ };

// IED
class btc_p_ied { /* Densité IED */ };
class btc_p_ied_power { /* Puissance IED */ };

// Caches et planques
class btc_p_hideout_n { /* Nombre de planques */ };
class btc_p_cache_info_ratio { /* Infos caches */ };

// Arsenal
class btc_p_arsenal_Type { /* Type d'arsenal */ };
class btc_p_arsenal_Restrict { /* Restrictions arsenal */ };
```

**Quand modifier:**
- Configuration du serveur avant lancement
- Ajustement de la difficulté
- Personnalisation de l'expérience de jeu

**Note:** Ces paramètres sont dans l'interface graphique du serveur, mais vous pouvez aussi les modifier directement dans le fichier pour changer les valeurs par défaut.

---

## Configuration des Systèmes Personnalisés

### 4. `core\fnc\deploy\config.sqf` - Système de Déploiement

**Emplacement**: `core\fnc\deploy\config.sqf`

**Ce que vous pouvez configurer:**

#### A. Objets Déployables

Chaque objet est défini avec:
- Classname (type d'objet Arma 3)
- Nom d'affichage
- Coûts en ressources [Fer, Munitions, Nourriture, Fuel, Argent]
- Catégorie

```sqf
// Exemple de configuration d'un objet
["Land_BagBunker_Small_F", "Bunker Sacs de Sable", [0,0,150,0,300], "defense"],
```

#### B. Catégories d'Objets

Les objets sont organisés en catégories:
- `defense` - Défenses (bunkers, nids MG, tourelles)
- `utility` - Utilitaires (hélipad, barricades, lumières)
- `checkpoint` - Checkpoints complets
- `troops` - Unités de défense
- `special` - Objets spéciaux (arsenal, téléporteur, FOB)

#### C. Configuration des Checkpoints

```sqf
// Dans param.hpp - Coûts des checkpoints
#define CHECKPOINT_COST_FER 0
#define CHECKPOINT_COST_MUNITIONS 0
#define CHECKPOINT_COST_NOURRITURE 500
#define CHECKPOINT_COST_FUEL 0
#define CHECKPOINT_COST_ARGENT 1000

// Rayon des checkpoints
#define CHECKPOINT_RADIUS 150
```

**Quand modifier:**
- Ajouter de nouveaux objets déployables
- Ajuster les coûts des objets
- Changer les catégories
- Équilibrer l'économie

---

### 5. `core\fnc\ressources\config.sqf` - Système de Ressources

**Emplacement**: `core\fnc\ressources\config.sqf`

**Ce que vous pouvez configurer:**

#### A. Types de Ressources

```sqf
btc_ressources_config = [
    [
        "Fer",                          // Nom
        "images\icon\arma3-land_pipes_large_f.jpg",  // Icône
        "#FF5733",                      // Couleur
        5,                              // Taux de génération
        5000                            // Maximum
    ],
    // ... autres ressources
];
```

#### B. Zones de Ressources

Les zones sont définies dans la mission elle-même (éditeur) ou via code:

```sqf
// Format: [Position, Type, Rayon]
// Type: 0=Fer, 1=Munitions, 2=Nourriture, 3=Fuel, 4=Argent
```

#### C. Taux d'Échange

```sqf
// Dans core\fnc\ressources\exchange.sqf
// Format: [ResourceFrom, ResourceTo, Rate]
// Exemple: 2 Fer = 1 Munition
[0, 1, 2]  // 2 Fer → 1 Munition
```

**Quand modifier:**
- Ajouter de nouvelles ressources
- Changer les taux de génération
- Modifier les limites de ressources
- Ajuster les taux d'échange

---

### 6. `define_mod.sqf` - Personnalisation Avancée

**Emplacement**: `define_mod.sqf` (racine de la mission)

**Ce que vous pouvez configurer:**

#### A. Villes Personnalisées

```sqf
btc_custom_loc = [
    // Format: [Position, Type, Nom, Rayon, Occupé]
    [[13132.8, 3315.07, 0], "NameVillage", "Petit Village", 500, true],
    [[10500, 8200, 0], "NameCity", "Grande Ville", 800, false]
];
```

**Types de localités disponibles:**
- `NameCity` - Grande ville
- `NameCityCapital` - Capitale
- `NameVillage` - Village
- `NameLocal` - Lieu-dit
- `Hill` - Colline
- `Airport` - Aéroport
- `StrongpointArea` - Base militaire
- `ViewPoint` - Point de vue
- `NameMarine` - Zone maritime
- `BorderCrossing` - Poste frontière
- `VegetationFir` - Zone forestière

#### B. Arsenal Personnalisé

```sqf
// Ajouter ou retirer des armes/équipements
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

#### C. Loadouts Automatiques

```sqf
// Loadouts par environnement: Desert, Tropic, Black, Forest
private _uniforms = [
    "U_B_CombatUniform_mcam",      // Desert
    "U_B_CTRG_Soldier_F",          // Tropic
    "U_B_CTRG_1",                  // Black
    "U_B_CombatUniform_mcam_wdl_f" // Forest
];
```

**Quand modifier:**
- Ajouter des villes sur la carte
- Personnaliser l'arsenal disponible
- Changer les loadouts de départ

---

## Configuration des Factions et Unités

### 7. Factions Ennemies

**Emplacement**: `core\def\mission.sqf` (lignes 650+)

#### A. Liste des Factions Disponibles

Plus de 200 factions sont disponibles, incluant:
- **Vanilla Arma 3**: CSAT, AAF, FIA, NATO
- **RHS**: USAF, USMC, AFRF, GREF
- **CUP**: Takistan, ChDKZ, CDF, Russia
- **3CB**: Multiples factions
- Et bien d'autres...

#### B. Personnalisation par Faction

```sqf
switch (_p_en) do {
    case "OPF_G_F" : {
        // Ajouter des véhicules spécifiques
        btc_type_motorized = btc_type_motorized + ["I_Truck_02_transport_F"];
        btc_type_motorized_armed = btc_type_motorized_armed + ["I_Heli_light_03_F"];
    };
    case "IND_C_F" : {
        // Retirer des unités
        btc_type_units = btc_type_units - ["I_C_Soldier_Camo_F"];
    };
};
```

**Quand modifier:**
- Ajouter du contenu à une faction
- Équilibrer les factions
- Corriger des problèmes de composition

---

## Configuration du Gameplay

### 8. Missions Secondaires

**Emplacement**: `core\fnc\side\`

#### A. Restrictions de Missions

**Fichier**: `core\fnc\side\missionRestrictions.sqf`

```sqf
case "rescue": {
    _excludedTypes = [];            // Types de villes exclus
    _requiresOccupied = true;       // Nécessite ville occupée
    _requiresMarine = false;        // Nécessite ville marine
    _customCheck = {true};          // Vérification personnalisée
};
```

#### B. Récompenses de Missions

**Fichier**: `core\fnc\side\configRewards.sqf`

```sqf
case "rescue": {
    _money = 500;                   // Argent gagné
    _rep = 50;                      // Réputation gagnée
    _title = "Sauvetage de Pilote";
    _description = "Récupérer le pilote abattu";
};
```

**Quand modifier:**
- Ajuster les conditions de disponibilité
- Modifier les récompenses
- Ajouter de nouvelles missions

---

### 9. Configuration des Contre-Attaques

**Emplacement**: `core\fnc\ressources\counterattack.sqf`

```sqf
// Vagues de contre-attaque
private _waves = [
    [4, 0],     // Vague 1: 4 groupes infantry, 0 véhicule
    [6, 1],     // Vague 2: 6 groupes infantry, 1 véhicule
    [8, 2]      // Vague 3: 8 groupes infantry, 2 véhicules
];

// Délais entre vagues (secondes)
private _waveDelay = 180;  // 3 minutes
```

**Quand modifier:**
- Ajuster la difficulté des contre-attaques
- Changer le nombre de vagues
- Modifier les délais

---

## Configuration Avancée

### 10. Configuration du Cache

**Emplacement**: `core\fnc\city\activate.sqf`

```sqf
// Distance d'activation des villes
private _cachingRadius = _city getVariable ["cachingRadius", 100];

// Distance de spawn des unités
private _spawningRadius = _cachingRadius/2;
```

---

### 11. Configuration du Debug

**Emplacement**: `core\def\mission.sqf`

```sqf
btc_debug = false;                  // Mode debug visuel (marqueurs)
btc_debug_log = false;              // Logs détaillés dans RPT
```

**Quand activer:**
- Debug de problèmes
- Vérification du spawn d'unités
- Test de systèmes

---

### 12. Paramètres des Checkpoints et FOB

**Emplacement**: `core\def\param.hpp`

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

## Conseils de Configuration

### Avant de Modifier

1. **Faites une sauvegarde** de la mission avant toute modification
2. **Testez en local** avant de mettre sur un serveur
3. **Modifiez une chose à la fois** pour identifier les problèmes
4. **Consultez les RPT** en cas d'erreur

### Équilibrage

- **Ressources**: Trop facile si génération > 10 par cycle
- **Réputation**: -50 par civil tué est un bon équilibre
- **IED**: Densité > 2 devient très difficile
- **Ennemis**: Ratio > 1 crée beaucoup d'ennemis

### Compatibilité

- **Factions**: Vérifiez que les mods requis sont installés
- **Objets**: Testez que les classnames existent
- **Paramètres**: Certains paramètres nécessitent un restart complet

---

## Fichiers à NE PAS Modifier

Sauf si vous savez ce que vous faites:

- `core\fnc\compile.sqf` - Compilation des fonctions
- `core\init_*.sqf` - Initialisation des systèmes
- `core\fnc\db\` - Système de sauvegarde
- `stringtable.xml` - Sauf pour traductions

---

## Support et Ressources

**Discord**: https://discord.gg/aEJ7W6QwYr  
**Documentation**: Dossier `READ_ME/`  
**Wiki Arma 3**: https://community.bistudio.com/wiki

---

**Bon modding!**

*Dernière mise à jour: Décembre 2025 - Version 0.1.7*

**Note**: Pour plus de détails sur les systèmes personnalisés, consultez `RESUME_SYSTEMES_PERSONNALISES.md` et `ARCHITECTURE_COMPLETE.md`.

