# Résumé des Systèmes Personnalisés - Hearts and Minds Ultimate

**Version**: 0.2.1  
**Auteur**: [13RDPA] LEON  
**Date**: 15 Décembre 2025

---

## Vue d'ensemble

Ce document résume les systèmes personnalisés ajoutés à Hearts and Minds Ultimate. Ces systèmes sont organisés dans les répertoires suivants :

- `core\fnc\addaction\` : Gestion centralisée des actions
- `core\fnc\deploy\` : Système de déploiement
- `core\fnc\ressources\` : Système de ressources
- `core\fnc\utility\` : Systèmes utilitaires
- `core\fnc\virtual_garage\` : Garage virtuel

---

## 1. SYSTÈME D'ACTIONS (`core\fnc\addaction\`)

### Fonction principale
Gestion centralisée de toutes les actions scroll wheel (molette de la souris) pour tous les objets de la mission.

### Composants clés
- **Initialisation** : `init.sqf` configure l'EventHandler `EntityCreated` et synchronise les actions
- **Synchronisation** : `syncAllActions.sqf` et `syncActionsForObject.sqf` pour JIP
- **Actions joueur** : `addPlayerActions.sqf` pour les actions système
- **Actions checkpoint** : `addCheckpointActions.sqf` pour les checkpoints
- **Actions admin** : Réactivation de zones/checkpoints, vérification système
- **Actions contre-attaques** : Lancement manuel de contre-attaques

### Points importants
- Séparation stricte entre actions de déploiement et de ressources via `btc_ressources_item_valid`
- Synchronisation automatique pour les joueurs qui rejoignent (JIP)
- Boucle de vérification périodique (30s) pour ajouter les actions aux nouveaux objets

---

## 2. SYSTÈME DE DÉPLOIEMENT (`core\fnc\deploy\`)

### Fonction principale
Système complet de déploiement d'objets tactiques (fortifications, troupes, checkpoints, caisses) avec gestion des ressources, cargo ACE, et contre-attaques.

### Composants principaux

#### Initialisation et Interface
- `init.sqf` : Configure les EventHandlers, le cargo des véhicules, démarre les systèmes de monitoring
- `initUI.sqf` : Définit les catégories d'objets et leurs coûts
- `create.sqf` : Interface de sélection (client)

#### Placement et Preview
- `startPreviewMode.sqf` / `previewLoop.sqf` / `exitPreviewMode.sqf` : Mode preview avec rotation/déplacement
- `confirmPlacement.sqf` : Création finale, déduction ressources, configuration cargo, inventaires caisses
- `place_*.sqf` : Gestion caméra et touches

#### Gestion des Objets
- `deleteDeployedObject.sqf` : Suppression avec remboursement
- `redeplacerObjet.sqf` : Redéploiement d'objets

#### Configuration Cargo
- `setupCargo.sqf` : Configure l'espace cargo ACE pour tous les véhicules (air, terre, mer)
  - Supporte `btc_deploy_customCargoClasses` pour classes personnalisées
  - Gère les conteneurs (`Land_Cargo20_military_green_F` : 100, `Land_Cargo40_military_green_F` : 200)
- `cargoSizes.sqf` : Définit les tailles cargo des objets déployables

#### Coûts et Remboursements
- `getObjectCosts.sqf` : Récupère les coûts
- `calculateCrateRefund.sqf` / `refundSingleCrate*.sqf` : Calcul et remboursement des caisses

#### Troupes et Unités
- `addTroop.sqf` : Ajout de troupes
- `getUnitClasses.sqf` / `loadouts.sqf` : Configuration des unités
- `autoAssignUnits.sqf` : Assignation automatique aux véhicules/statiques
- `assignUnitToVehicle.sqf` / `placeCarriedUnit.sqf` : Gestion des unités

#### Checkpoints
- `detectFlags.sqf` : Détection automatique des drapeaux déployés
- `activateCheckpointTask.sqf` / `monitorCheckpointTask.sqf` / `completeCheckpointTask.sqf` : Gestion des tâches
- `monitorCheckpoints.sqf` : Surveillance de tous les checkpoints actifs
- `checkpointCounterAttack.sqf` : Contre-attaques automatiques
- `monitorCounterAttacks.sqf` : Monitoring des contre-attaques
- `syncCheckpoints.sqf` : Synchronisation JIP

#### Échange de Ressources
- `openResourceExchangeDialog.sqf` / `processResourceExchange.sqf` : Échange entre ressources

### Variables globales principales
- `btc_deploy_fortifications` : Fortifications disponibles
- `btc_deploy_troops` : Troupes disponibles
- `btc_deploy_checkpoints` : Checkpoints disponibles
- `btc_deploy_activeCheckpoints` : Checkpoints actifs
- `btc_deploy_customCargoClasses` : Configuration cargo personnalisée

### Interdépendances
- Utilise le système de ressources pour les coûts
- Utilise `btc_task_fnc_create` pour les tâches
- Utilise `btc_mil_fnc_create_group` pour les contre-attaques

---

## 3. SYSTÈME DE RESSOURCES (`core\fnc\ressources\`)

### Fonction principale
Gestion complète des 4 ressources (Fer, Munition, Nourriture, Fuel) avec zones de génération, collecte automatique, tâches, et contre-attaques.

### Composants principaux

#### Initialisation
- `init.sqf` : Initialise zones, dépôts, zones de collecte, démarre tous les systèmes de monitoring
- `initClient.sqf` : Interface utilisateur et raccourcis clavier
- `initDepots.sqf` : Initialise les dépôts de collecte (prévention doublons)
- `initCollectAreas.sqf` : Zones de collecte avec barrières visuelles

#### Gestion des Zones
- `createZones.sqf` : Création des zones
- `activateZone.sqf` / `deactivateZone.sqf` : Activation/désactivation
- `captureZone.sqf` : Capture d'une zone
- `monitorZones.sqf` : Surveillance pour détecter la capture

#### Génération de Ressources
- `generationLoop.sqf` : Boucle principale (60s)
- `monitorZoneSlots.sqf` : Création progressive (1 objet par cycle)
  - Vérifie les slots disponibles
  - Arrête si zone pleine ou tâche active
- `generateResourceItem.sqf` : Génération d'un objet
- `checkResourceItemSpace.sqf` : Vérification espace

#### Collecte
- `monitorDepots.sqf` : Collecte automatique dans le rayon des dépôts
  - Exclut objets en cargo/portés
  - Exclut objets dans zone de collecte
- `collectResourceItem.sqf` : Collecte manuelle/automatique
- `addResourceItemActions.sqf` : Actions (portage, chargement, ajustement)

#### Tâches
- `createCollectTask.sqf` : Création tâche de collecte
- `monitorCollectTask.sqf` : Surveillance (valide quand zone vide)
- `verifyCollectTasks.sqf` : Vérification périodique (5 min) pour créer tâches manquantes
- `activateDefenseTask.sqf` / `monitorDefenseTask.sqf` : Tâches de défense

#### Contre-Attaques
- `counterAttackLoop.sqf` : Boucle principale (sélection aléatoire, mémoire 5 zones)
- `counterAttack.sqf` : Lancement contre-attaque
  - Sélection planque ou position aléatoire
  - Gestion bateaux (spawn 100m du bord, tous dans bateau, waypoints plage → débarquement → zone)
  - Vérification distances bâtiments (50m minimum)
  - Vérification routes pour véhicules
  - Waypoints multi-étapes pour assauts amphibies
- `manualZoneCounterAttack.sqf` : Contre-attaque manuelle

#### Gestion des Ressources
- `addResources.sqf` / `setResources.sqf` / `resetResources.sqf` : Gestion quantités
- `deductCost.sqf` / `deductCostsArray.sqf` : Déduction coûts
- `checkCost.sqf` / `getCosts.sqf` : Vérification et récupération coûts
- `syncCosts.sqf` : Synchronisation multijoueur

#### Interface Utilisateur
- `createUI.sqf` / `updateUI.sqf` / `updateUILoop.sqf` : Interface ressources
- `showUI.sqf` / `hideUI.sqf` : Affichage/masquage
- `keyHandler.sqf` : Gestion touches clavier

### Variables globales principales
- `btc_ressources_zones_objects` : Zones de ressources
- `btc_ressources_config` : Configuration
- `btc_ressources_amounts` : Quantités [Fer, Munition, Nourriture, Fuel, Argent]
- `btc_ressources_collected_items` : Objets collectés
- `btc_ressources_collect_depots_objects` : Dépôts de collecte
- `btc_ressources_last_attacked_zones` : Dernières zones attaquées (mémoire 5)
- `btc_ressources_last_used_hideouts` : Dernières planques utilisées (mémoire 5)

### Interdépendances
- Utilisé par le système de déploiement pour les coûts
- Utilise `btc_mil_fnc_create_group` pour les contre-attaques
- Utilise `btc_task_fnc_create` pour les tâches

---

## 4. SYSTÈME UTILITAIRE (`core\fnc\utility\`)

### Fonction principale
Regroupe plusieurs systèmes utilitaires pratiques pour améliorer le gameplay.

### Composants

#### Configuration (`LEON_config.sqf`)
- Configuration générale des systèmes personnalisés

#### Drone (`LEON_drone\`)
- `init.sqf` / `drone.sqf` : Système de drone de surveillance
- Interface pour déployer et contrôler un drone

#### Halo (`LEON_halo\`)
- `LEON_Halo.sqf` : Système de saut HALO
- Interface complète avec contrôles et sons

#### Téléporteur (`LEON_teleporteur\`)
- `teleporter.sqf` : Système de téléportation entre points

#### Portage (`LEON_portage\`)
- `init.sqf` : Initialise le portage de caisses
- `config_caisses.sqf` : Configuration des caisses portables
- `functions\fn_initPortageCaisse.sqf` / `fn_gererMouvementCaisse.sqf` : Gestion du portage

#### Invincibilité (`LEON_invincible\`)
- `init.sqf` / `invincible.sqf` : Invincibilité temporaire au spawn

#### Véhicules (`LEON_vehicles\`)
- `initCaisse.sqf` : Initialise les caisses de ravitaillement
  - Définit `OPEX_fnc_*` pour inventaires personnalisés
  - Gère les caisses créées via Zeus/éditeur
  - Prévention conflits avec système de déploiement
- `initCaisseRepair.sqf` : Caisses de réparation
- `medic_repa.sqf` : Système médical/réparation véhicules
- `vehicleInventory.sqf` : Gestion inventaires véhicules

#### Debug (`LEON_debug\`)
- `testSystem.sqf` : Tests système
- `verifySystem.sqf` : Vérification système
- `debugCheckpoints.sqf` : Debug checkpoints
- `createDocumentation.sqf` : Génération documentation
- `verificationCohérence.sqf` : Vérification cohérence

---

## 5. SYSTÈME GARAGE VIRTUEL (`core\fnc\virtual_garage\`)

### Fonction principale
Stockage et récupération de véhicules dans un garage virtuel.

### Composants
- `init.sqf` : Initialise le garage
- `fnc_openGarage.sqf` : Ouvre le garage (joueur)
- `fnc_openZenGarage.sqf` : Ouvre le garage (Zeus)

### Fonctionnalités
- Stockage de véhicules avec inventaire et dégâts
- Récupération de véhicules stockés
- Persistance via système de sauvegarde

---

## 6. SYSTÈME DE PATROUILLES AMÉLIORÉ (`core\fnc\patrol\`)

### Fonction principale
Système de patrouilles automatiques avec séparation stricte entre patrouilles militaires et civiles, comportement agressif configurable, et mise à jour dynamique des waypoints en combat.

### Composants principaux

#### Initialisation
- `autoSpawn.sqf` : Système de création automatique de patrouilles militaires basé sur un timer
- Séparation stricte : Patrouilles militaires (`btc_patrol_active`) vs Civiles (`btc_civ_veh_active`)

#### Création des Patrouilles
- `create_patrol.sqf` (militaires) : Création depuis villes ennemies occupées uniquement
- `create_patrol.sqf` (civiles) : Création lors de l'activation des villes
- Exclusion des zones joueurs : Aucune patrouille ne spawn dans un rayon de 1000m autour des joueurs
- Exclusion des zones sensibles : Aucun spawn dans un rayon de 1500m autour des zones de ressources, FOB, checkpoints
- Exclusion base configurable : Aucun spawn dans un rayon configurable autour du marqueur `btc_base` (500m-5000m, défaut: 1500m)
- Zones d'exclusion personnalisées : Possibilité d'ajouter des zones d'exclusion dans `define_mod.sqf` via `btc_patrol_exclusion_zones`

#### Gestion des Waypoints
- `addWP.sqf` : Création de waypoints avec paramètres selon type (militaires/civiles)
- `init.sqf` : Initialisation des routes de patrouille
- `updateCombatWaypoints.sqf` : Mise à jour dynamique toutes les 1 minute si en combat
- `getOutVehicle.sqf` : Gestion de sortie des véhicules (passagers seulement si armé)

#### Sélection des Destinations
- `usefulCity.sqf` : Système de priorisation des destinations
  - Priorité 1 : Positions des joueurs (attaque directe)
  - Priorité 2 : Villes libérées (recapture)
  - Priorité 3 : Villes occupées normales
  - Tracking des villes récentes : Exclusion des villes sélectionnées dans les 5 dernières minutes pour éviter répétitions

#### Détection et Filtrage
- `hasPlayersNearby.sqf` : Vérifie présence de joueurs dans un rayon configurable
- `playersInAreaCityGroup.sqf` : Vérifie présence joueurs pour suppression
- `WPCheck.sqf` : Vérification waypoint et réinitialisation
- `eh.sqf` : Gestion des événements et suppression

### Comportement des Patrouilles

#### Patrouilles Automatiques (Militaires)
- **Mode de combat** : Ouvrez le feu (RED)
- **Comportement** : Vigilance (AWARE)
- **Formation** : Colonne (COLUMN)
- **Vitesse** : Maximale (FULL)
- **Création** : Uniquement via timer automatique (`btc_p_patrol_timer`)

#### Patrouilles Civiles
- **Mode de combat** : Ne pas ouvrir (BLUE)
- **Comportement** : Insouciant (CARELESS)
- **Formation** : Colonne (COLUMN)
- **Vitesse** : Limitée (LIMITED)
- **Destinations** : Uniquement villes libérées par les joueurs

### Variables globales principales
- `btc_patrol_active` : Liste des patrouilles militaires actives
- `btc_civ_veh_active` : Liste des patrouilles civiles actives
- `btc_patrol_area` : Rayon de patrouille (1500m par défaut)
- `btc_p_patrol_timer` : Intervalle de création automatique (secondes)
- `btc_p_patrol_max` : Nombre maximum de patrouilles militaires
- `btc_p_patrol_vehicle_percent` : Pourcentage de patrouilles avec véhicules (0-100%)
- `btc_p_patrol_exclusion_base_distance` : Distance d'exclusion autour de la base (500m-5000m, défaut: 1500m)
- `btc_patrol_exclusion_zones` : Zones d'exclusion personnalisées (définies dans `define_mod.sqf`)
- `btc_patrol_recent_cities` : Liste des villes récemment sélectionnées (tracking pour éviter répétitions)
- `btc_auto_patrol` : Variable marquant les patrouilles automatiques (pour comportement agressif)

### Paramètres configurables (param.hpp)
- `btc_p_patrol_timer` : Timer de création automatique (0 = désactivé)
- `btc_p_patrol_max` : Nombre maximum de patrouilles militaires (0-30)
- `btc_p_civ_max_veh` : Nombre maximum de patrouilles civiles (0-30)
- `btc_p_patrol_vehicle_percent` : Pourcentage de patrouilles avec véhicules (0-100%)
- `btc_p_patrol_exclusion_base_distance` : Distance d'exclusion autour de la base (500m-5000m, défaut: 1500m)

### Fonctionnalités avancées
- **Waypoint GETOUT** : Les patrouilles en véhicules sortent avant d'arriver à destination
  - Véhicules armés : Seuls les passagers sortent (conducteur/tireurs restent)
  - Véhicules non armés : Toutes les unités sortent
- **Mise à jour dynamique** : Waypoints recréés toutes les 1 minute en combat pour suivre les joueurs
- **Exclusion zones joueurs** : Aucun spawn dans un rayon de 1000m autour des joueurs
- **Exclusion zones sensibles** : Aucun spawn dans un rayon de 1500m autour des zones de ressources, FOB, checkpoints
- **Exclusion base configurable** : Aucun spawn dans un rayon configurable autour du marqueur `btc_base` (500m-5000m, défaut: 1500m)
- **Zones d'exclusion personnalisées** : Possibilité d'ajouter des zones d'exclusion dans `define_mod.sqf` via `btc_patrol_exclusion_zones`
- **Tracking des villes** : Système de tracking pour éviter que les patrouilles se dirigent toujours vers les mêmes villes (exclusion des villes sélectionnées dans les 5 dernières minutes)
- **Recapture des villes** : Les patrouilles peuvent recapturer les villes libérées (40% de chance) et créer des missions checkpoint (40% de chance)
- **Priorisation intelligente** : Les patrouilles ciblent d'abord les joueurs, puis les villes libérées

### Interdépendances
- Utilise `btc_mil_fnc_create_patrol` pour créer les patrouilles militaires
- Utilise `btc_civ_fnc_create_patrol` pour créer les patrouilles civiles
- Utilise `btc_fnc_find_closecity` pour trouver les villes proches
- Utilise `btc_patrol_fnc_WPCheck` pour vérifier les waypoints

---

## Modifications Associées

### Système de Sauvegarde (`core\fnc\db\`)
- **save.sqf** : Sauvegarde zones ressources (17 éléments), objets collectés (7 éléments), cargo (5 éléments), checkpoints
- **load.sqf** : Chargement et restauration avec vérification formats stricts, synchronisation JIP

### Event Handlers (`core\fnc\eh\`)
- **CuratorObjectPlaced.sqf** : Configuration cargo et inventaires pour objets Zeus
- **EntityCreated** : Configuration automatique cargo pour tous les véhicules

### Compilation (`core\fnc\compile.sqf`)
- Compilation de toutes les fonctions personnalisées

---

## Points d'Attention

1. **Séparation Actions** : Les actions de déploiement et de ressources sont strictement séparées via `btc_ressources_item_valid`

2. **Format Sauvegarde** : Formats stricts (17 éléments zones, 7 éléments objets, 5 éléments cargo) - ne pas modifier sans migration

3. **Synchronisation** : Utilisation de `publicVariable` pour synchroniser les variables importantes (multijoueur)

4. **Performance** : Utilisation de `distanceSqr`, filtrage avant itération, cache des valeurs calculées

5. **Cargo** : Configuration automatique pour tous les véhicules (éditeur, Zeus, déployés) via `setupCargo.sqf`

6. **Doublons** : Prévention des doublons pour dépôts de collecte et objets de ressources

---

**Document créé le**: 10 Décembre 2025  
**Dernière mise à jour**: 15 Décembre 2025  
**Version mission**: 0.2.1

