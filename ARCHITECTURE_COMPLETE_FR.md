# Architecture Complète - Hearts and Minds Ultimate v0.2.1

**Mission**: Hearts and Minds Ultimate  
**Version**: 0.2.1  
**Auteur**: [13RDPA] LEON  
**Date**: 15 Décembre 2025

---

## 📋 TABLE DES MATIÈRES

1. [Vue d'ensemble](#vue-densemble)
2. [Structure des fichiers](#structure-des-fichiers)
3. [Systèmes de base (Hearts and Minds)](#systèmes-de-base-hearts-and-minds)
4. [Systèmes personnalisés (LEON)](#systèmes-personnalisés-leon)
5. [Flux d'initialisation](#flux-dinitialisation)
6. [Interdépendances](#interdépendances)
7. [Variables globales principales](#variables-globales-principales)
8. [Event Handlers](#event-handlers)
9. [Système de sauvegarde/chargement](#système-de-sauvegardechargement)

---

## 🎯 VUE D'ENSEMBLE

Hearts and Minds Ultimate est une mission Arma 3 persistante automatisée basée sur Hearts and Minds, avec des systèmes personnalisés ajoutés. La mission est divisée en deux parties principales :

1. **Mission de base (Hearts and Minds)** : Système complexe avec 33 systèmes interdépendants
2. **Systèmes personnalisés (LEON)** : Ajouts pour le déploiement, les ressources, et les utilitaires

**ATTENTION CRITIQUE** : La mission de base a de nombreuses interdépendances entre fichiers. Toute modification nécessite une grande prudence.

---

## 📁 STRUCTURE DES FICHIERS

### Fichiers racine
- `init.sqf` : Point d'entrée principal
- `description.ext` : Configuration de la mission
- `define_mod.sqf` : Définition des mods requis

### Répertoires principaux
- `core/` : Code principal de la mission
  - `def/` : Définitions et paramètres
  - `fnc/` : Fonctions organisées par système
  - `img/` : Images
  - `sounds/` : Sons
  - `init*.sqf` : Scripts d'initialisation

---

## 🔧 SYSTÈMES DE BASE (HEARTS AND MINDS)

### 1. SYSTÈME DE VILLES (city)
**Répertoire**: `core/fnc/city/`

**Fonction principale**: Gestion des villes sur la carte, leur occupation, libération, et population.

**Fichiers**:
- `init.sqf` : Initialise toutes les villes de la carte
- `create.sqf` : Crée une ville avec ses propriétés
- `activate.sqf` : Active une ville (spawn ennemis/civils)
- `de_activate.sqf` : Désactive une ville
- `setClear.sqf` : Marque une ville comme libérée
- `setPlayerTrigger.sqf` : Configure les triggers joueur
- `cleanUp.sqf` : Nettoie les unités d'une ville
- `getHouses.sqf` : Récupère les maisons d'une ville
- `send.sqf` : Envoie des renforts à une ville
- `trigger_free_condition.sqf` : Condition pour libérer une ville

**Variables globales**:
- `btc_city_all` : HashMap de toutes les villes
- `btc_city_occupied` : Villes occupées

**Interdépendances**:
- Utilise `btc_mil_fnc_create_group` pour spawner ennemis
- Utilise `btc_civ_fnc_populate` pour spawner civils
- Utilise `btc_rep_fnc_change` pour la réputation

---

### 2. SYSTÈME DE CACHES (cache)
**Répertoire**: `core/fnc/cache/`

**Fonction principale**: Gestion des caches d'armes à trouver et détruire.

**Fichiers**:
- `init.sqf` : Initialise le système de cache
- `find_pos.sqf` : Trouve une position pour le cache
- `create.sqf` : Crée un cache
- `create_attachto.sqf` : Crée un cache attaché à un objet
- `hd.sqf` : Gère la destruction du cache

**Variables globales**:
- `btc_cache_n` : Nombre de caches
- `btc_cache_obj` : Objet cache actuel
- `btc_cache_pos` : Position du cache
- `btc_cache_markers` : Marqueurs du cache
- `btc_cache_pictures` : Images du cache
- `btc_cache_info` : Informations nécessaires

**Interdépendances**:
- Utilise `btc_info_fnc_cache` pour les indices
- Utilise `btc_city_all` pour trouver une position
- Déclenche la création d'un nouveau cache après destruction

---

### 3. SYSTÈME DE PLANQUES (hideout)
**Répertoire**: `core/fnc/hideout/`

**Fonction principale**: Gestion des planques ennemies à détruire.

**Fichiers**:
- `create.sqf` : Crée une planque
- `create_composition.sqf` : Crée la composition d'une planque
- `hd.sqf` : Gère la destruction d'une planque

**Variables globales**:
- `btc_hideouts` : Array de toutes les planques

**Interdépendances**:
- Utilisé par les contre-attaques (ressources, checkpoints, FOB)
- Utilisé par `btc_info_fnc_hideout` pour les indices

---

### 4. SYSTÈME MILITAIRE (mil)
**Répertoire**: `core/fnc/mil/`

**Fonction principale**: Gestion des unités militaires ennemies.

**Fichiers**:
- `create_group.sqf` : Crée un groupe d'unités
- `create_patrol.sqf` : Crée une patrouille
- `create_static.sqf` : Crée une arme statique
- `create_staticOnRoof.sqf` : Crée une arme statique sur un toit
- `createVehicle.sqf` : Crée un véhicule
- `createUnits.sqf` : Crée des unités
- `addWP.sqf` : Ajoute des waypoints
- `send.sqf` : Envoie des renforts
- `set_skill.sqf` : Configure les compétences IA
- `check_cap.sqf` : Vérifie la capacité
- `getStructures.sqf` : Récupère les structures
- `getBuilding.sqf` : Récupère un bâtiment
- `unit_killed.sqf` : Gère la mort d'une unité
- `class.sqf` : Définit les classes d'unités
- `ammoUsage.sqf` : Gère l'utilisation des munitions

**Variables globales**:
- `btc_type_units` : Types d'unités ennemis
- `btc_type_vehicles` : Types de véhicules ennemis
- `btc_type_boats` : Types de bateaux ennemis

**Interdépendances**:
- Utilisé par `btc_city_fnc_activate` pour spawner ennemis
- Utilisé par les contre-attaques
- Utilisé par les missions secondaires

---

### 5. SYSTÈME CIVIL (civ)
**Répertoire**: `core/fnc/civ/`

**Fonction principale**: Gestion des civils et de leurs interactions.

**Fichiers**:
- `populate.sqf` : Peuple une ville avec des civils
- `create_patrol.sqf` : Crée une patrouille civile
- `class.sqf` : Définit les classes de civils
- `add_weapons.sqf` : Ajoute des armes aux civils
- `get_weapons.sqf` : Récupère les armes des civils
- `add_grenade.sqf` : Ajoute des grenades
- `get_grenade.sqf` : Récupère les grenades
- `add_leaflets.sqf` : Ajoute des tracts
- `leaflets.sqf` : Gère les tracts
- `evacuate.sqf` : Évacue les civils
- `addWP.sqf` : Ajoute des waypoints aux civils
- `createFlower.sqf` : Crée des fleurs (mémorial)
- `createGrave.sqf` : Crée une tombe

**Variables globales**:
- `btc_type_civ` : Types de civils
- `btc_type_civ_veh` : Types de véhicules civils

**Interdépendances**:
- Utilisé par `btc_city_fnc_activate`
- Affecte la réputation (`btc_rep_fnc_change`)

---

### 6. SYSTÈME DE RÉPUTATION (rep)
**Répertoire**: `core/fnc/rep/`

**Fonction principale**: Gestion de la réputation avec les civils.

**Fichiers**:
- `change.sqf` : Change la réputation
- `notify.sqf` : Notifie les changements
- `killed.sqf` : Gère la mort (affecte la réputation)
- `hh.sqf` : Gère les interactions
- `eh_effects.sqf` : Effets des event handlers
- `buildingchanged.sqf` : Bâtiments détruits
- `explosives_defuse.sqf` : Désamorçage d'explosifs
- `wheelChange.sqf` : Changement de pneu
- `grave.sqf` : Gestion des tombes
- `call_militia.sqf` : Appelle la milice
- `hd.sqf` : Gère la destruction
- `suppressed.sqf` : Gère la suppression
- `foodRemoved.sqf` : Nourriture retirée
- `treatment.sqf` : Traitement médical

**Variables globales**:
- `btc_global_reputation` : Réputation globale
- `btc_rep_militia` : Réputation milice

**Interdépendances**:
- Affecté par les actions des joueurs
- Affecte le comportement des civils
- Déclenche des événements selon le niveau

---

### 7. SYSTÈME IED (ied)
**Répertoire**: `core/fnc/ied/`

**Fonction principale**: Gestion des engins explosifs improvisés.

**Fichiers**:
- `initArea.sqf` : Initialise une zone IED
- `create.sqf` : Crée un IED
- `boom.sqf` : Fait exploser un IED
- `check.sqf` : Vérifie un IED
- `checkLoop.sqf` : Boucle de vérification
- `fired_near.sqf` : Détection de tirs à proximité
- `suicider_active.sqf` : Active un kamikaze
- `suicider_activeLoop.sqf` : Boucle des kamikazes
- `suicider_create.sqf` : Crée un kamikaze
- `suiciderLoop.sqf` : Boucle principale des kamikazes
- `allahu_akbar.sqf` : Cri du kamikaze
- `drone_active.sqf` : Active un drone IED
- `drone_create.sqf` : Crée un drone IED
- `droneLoop.sqf` : Boucle des drones IED
- `drone_fire.sqf` : Fait tirer un drone IED
- `randomRoadPos.sqf` : Position aléatoire sur route
- `belt.sqf` : Ceinture explosive
- `effects.sqf` : Effets visuels
- `effect_smoke.sqf` : Effet fumée
- `effect_color_smoke.sqf` : Effet fumée colorée
- `effect_rocks.sqf` : Effet rochers
- `effect_blurEffect.sqf` : Effet flou
- `effect_shock_wave.sqf` : Onde de choc
- `deleteLoop.sqf` : Boucle de suppression

**Variables globales**:
- `btc_ied_list` : Liste des IED
- `btc_p_ied` : Probabilité IED

**Interdépendances**:
- Utilise `btc_rep_fnc_change` pour la réputation
- Utilise `btc_civ_fnc_create_patrol` pour les kamikazes

---

### 8. SYSTÈME D'INFORMATIONS (info)
**Répertoire**: `core/fnc/info/`

**Fonction principale**: Gestion des indices et informations.

**Fichiers**:
- `cache.sqf` : Informations sur les caches
- `hideout.sqf` : Informations sur les planques
- `give_intel.sqf` : Donne un indice
- `has_intel.sqf` : Vérifie si un indice existe
- `ask.sqf` : Demande des informations
- `hideout_asked.sqf` : Planque demandée
- `search_for_intel.sqf` : Recherche d'indices
- `troops.sqf` : Informations sur les troupes
- `ask_reputation.sqf` : Demande selon réputation
- `cachePicture.sqf` : Image du cache
- `cacheMarker.sqf` : Marqueur du cache
- `path.sqf` : Chemin vers l'objectif
- `createIntels.sqf` : Crée des indices

**Variables globales**:
- `btc_info_intel_id` : ID des indices
- `btc_info_cache_def` : Informations par défaut pour cache

**Interdépendances**:
- Utilisé par les civils (`btc_int_fnc_ask`)
- Utilisé pour trouver caches et planques

---

### 9. SYSTÈME DE MISSIONS SECONDAIRES (side)
**Répertoire**: `core/fnc/side/`

**Fonction principale**: Gestion des missions secondaires dynamiques.

**Fichiers**:
- `create.sqf` : Crée une mission secondaire
- `createWithParams.sqf` : Crée avec paramètres
- `configRewards.sqf` : Configure les récompenses
- `getAvailableMissions.sqf` : Récupère les missions disponibles
- `missionRestrictions.sqf` : Restrictions des missions
- `isCityValidForMission.sqf` : Vérifie si une ville est valide
- `checkMissionRequirements.sqf` : Vérifie les prérequis
- `selectConvoyRoute.sqf` : Sélectionne une route de convoi
- `giveRewards.sqf` : Donne les récompenses
- `initRequestDialog.sqf` : Initialise le dialogue de demande
- `updateMissionList.sqf` : Met à jour la liste
- `updateRewardsDisplay.sqf` : Met à jour l'affichage des récompenses
- `acceptMission.sqf` : Accepte une mission
- `addPlayerActions.sqf` : Ajoute les actions joueur
- `mines.sqf` : Mission mines
- `supply.sqf` : Mission ravitaillement
- `vehicle.sqf` : Mission véhicule
- `tower.sqf` : Mission tour
- `checkpoint.sqf` : Mission checkpoint
- `underwater_generator.sqf` : Mission générateur sous-marin
- `convoy.sqf` : Mission convoi
- `rescue.sqf` : Mission sauvetage
- `capture_officer.sqf` : Mission capture d'officier
- `hostage.sqf` : Mission otage
- `hack.sqf` : Mission piratage
- `kill.sqf` : Mission élimination
- `chemicalLeak.sqf` : Mission fuite chimique
- `EMP.sqf` : Mission EMP
- `removeRubbish.sqf` : Mission nettoyage
- `pandemic.sqf` : Mission pandémie
- `massacre.sqf` : Mission massacre

**Variables globales**:
- `btc_side_list` : Liste des missions secondaires
- `btc_side_jip_data` : Données JIP

**Interdépendances**:
- Utilise `btc_city_all` pour les positions
- Utilise `btc_mil_fnc_create_group` pour les ennemis
- Utilise `btc_task_fnc_create` pour les tâches

---

### 10. SYSTÈME FOB (fob)
**Répertoire**: `core/fnc/fob/`

**Fonction principale**: Gestion des Forward Operating Bases.

**Fichiers**:
- `create_s.sqf` : Crée un FOB (serveur)
- `create.sqf` : Crée un FOB (client)
- `dismantle_s.sqf` : Démantèle un FOB (serveur)
- `killed.sqf` : Gère la destruction d'un FOB
- `rallypointTimer.sqf` : Timer du point de ralliement
- `detectFOBs.sqf` : Détecte les FOB déployés
- `zoneStatus.sqf` : Statut de la zone FOB
- `activateFOBTask.sqf` : Active la tâche FOB
- `monitorFOBTask.sqf` : Surveille la tâche FOB
- `completeFOBTask.sqf` : Complète la tâche FOB
- `fobCounterAttack.sqf` : Contre-attaque FOB
- `manualFOBCounterAttack.sqf` : Contre-attaque manuelle FOB
- `monitorFOBsCounterAttacks.sqf` : Surveille les contre-attaques FOB
- `addInteraction.sqf` : Ajoute les interactions
- `redeploy.sqf` : Redéploie un FOB
- `redeployCheck.sqf` : Vérifie le redéploiement
- `rallypointAssemble.sqf` : Assemble le point de ralliement

**Variables globales**:
- `btc_fobs` : Array des FOB
- `btc_fob_last_attacked_fobs` : Derniers FOB attaqués

**Interdépendances**:
- Utilise `btc_mil_fnc_create_group` pour les contre-attaques
- Utilise `btc_task_fnc_create` pour les tâches
- Utilise le système de respawn

---

### 11. SYSTÈME DE LOGISTIQUE (log)
**Répertoire**: `core/fnc/log/`

**Fonction principale**: Gestion de la logistique (remorquage, chargement, réparation).

**Fichiers**:
- `init.sqf` : Initialise le système logistique
- `create.sqf` : Crée un point logistique
- `createVehicle.sqf` : Crée un véhicule logistique
- `create_apply.sqf` : Applique la création
- `create_load.sqf` : Charge un objet
- `create_change_target.sqf` : Change la cible
- `delete.sqf` : Supprime un point logistique
- `server_delete.sqf` : Supprime (serveur)
- `place.sqf` : Place un objet
- `place_create_camera.sqf` : Crée la caméra de placement
- `place_destroy_camera.sqf` : Détruit la caméra
- `place_key_down.sqf` : Gère les touches
- `get_corner_points.sqf` : Récupère les points de coin
- `repair_wreck.sqf` : Répare une épave
- `server_repair_wreck.sqf` : Répare (serveur)
- `copy.sqf` : Copie un objet
- `paste.sqf` : Colle un objet
- `refuelSource.sqf` : Source de ravitaillement
- `rearmSource.sqf` : Source de réarmement
- `inventoryGet.sqf` : Récupère l'inventaire
- `inventorySet.sqf` : Définit l'inventaire
- `inventoryCopy.sqf` : Copie l'inventaire
- `inventoryPaste.sqf` : Colle l'inventaire
- `inventoryRestore.sqf` : Restaure l'inventaire

**Variables globales**:
- `btc_log_point_selected` : Point logistique sélectionné
- `btc_log_obj_selected` : Objet sélectionné

**Interdépendances**:
- Utilise `btc_tow_fnc_*` pour le remorquage
- Utilise `btc_lift_fnc_*` pour le levage

---

### 12. SYSTÈME DE REMORQUAGE (tow)
**Répertoire**: `core/fnc/tow/`

**Fonction principale**: Gestion du remorquage de véhicules.

**Fichiers**:
- `int.sqf` : Initialise le remorquage
- `ropeCreate.sqf` : Crée la corde
- `hitch_points.sqf` : Points d'attache
- `unhook.sqf` : Détache
- `check.sqf` : Vérifie le remorquage
- `ropeBreak.sqf` : Corde cassée
- `ViV.sqf` : Vehicle in Vehicle

**Interdépendances**:
- Utilisé par le système logistique
- Utilise ACE3 pour les cordes

---

### 13. SYSTÈME DE LEVAGE (lift)
**Répertoire**: `core/fnc/lift/`

**Fonction principale**: Gestion du levage d'objets avec hélicoptère.

**Fichiers**:
- `check.sqf` : Vérifie le levage
- `deployRopes.sqf` : Déploie les cordes
- `destroyRopes.sqf` : Détruit les cordes
- `hook.sqf` : Accroche
- `hookFake.sqf` : Accroche (simulation)
- `hud.sqf` : Interface HUD
- `hudLoop.sqf` : Boucle HUD
- `shortcuts.sqf` : Raccourcis clavier
- `disabled.sqf` : Désactivé

**Interdépendances**:
- Utilisé par le système logistique
- Utilise ACE3 pour les cordes

---

### 14. SYSTÈME DE CORPS (body)
**Répertoire**: `core/fnc/body/`

**Fonction principale**: Gestion des corps et des dogtags.

**Fichiers**:
- `create.sqf` : Crée un corps
- `get.sqf` : Récupère un corps
- `bagRecover.sqf` : Récupère un sac à dos (client)
- `bagRecover_s.sqf` : Récupère un sac à dos (serveur)
- `createMarker.sqf` : Crée un marqueur
- `dogtagGet.sqf` : Récupère un dogtag
- `dogtagSet.sqf` : Définit un dogtag
- `setBodyBag.sqf` : Met dans un sac mortuaire

**Variables globales**:
- `btc_body_deadPlayers` : Joueurs morts

**Interdépendances**:
- Utilisé par le système de respawn
- Utilise `btc_rep_fnc_change` pour la réputation

---

### 15. SYSTÈME DE RESPAWN (respawn)
**Répertoire**: `core/fnc/respawn/`

**Fonction principale**: Gestion du respawn et des tickets.

**Fichiers**:
- `player.sqf` : Gère le respawn joueur
- `playerConnected.sqf` : Joueur connecté
- `addTicket.sqf` : Ajoute un ticket
- `screen.sqf` : Écran de respawn
- `force.sqf` : Force le respawn
- `intro.sqf` : Introduction

**Variables globales**:
- `btc_respawn_tickets` : Tickets de respawn
- `btc_p_respawn_ticketsAtStart` : Tickets au démarrage

**Interdépendances**:
- Utilise `btc_fob_fnc_*` pour les FOB
- Utilise `btc_body_fnc_*` pour les corps

---

### 16. SYSTÈME DE PORTES (door)
**Répertoire**: `core/fnc/door/`

**Fonction principale**: Gestion des portes verrouillées.

**Fichiers**:
- `lock.sqf` : Verrouille une porte
- `get.sqf` : Récupère l'état d'une porte
- `break.sqf` : Brise une porte (client)
- `broke.sqf` : Brise une porte (serveur)

**Interdépendances**:
- Utilisé par `btc_city_fnc_activate` pour verrouiller les portes

---

### 17. SYSTÈME CHIMIQUE (chem)
**Répertoire**: `core/fnc/chem/`

**Fonction principale**: Gestion de la guerre chimique.

**Fichiers**:
- `checkLoop.sqf` : Boucle de vérification
- `propagate.sqf` : Propagation des agents chimiques
- `handleShower.sqf` : Gère les douches de décontamination
- `damage.sqf` : Dégâts chimiques (client)
- `damageLoop.sqf` : Boucle de dégâts
- `biopsy.sqf` : Biopsie
- `ehDetector.sqf` : Event handler détecteur
- `updateDetector.sqf` : Met à jour le détecteur

**Variables globales**:
- `btc_chem_contaminated` : Zones contaminées
- `btc_p_chem_sides` : Guerre chimique activée

**Interdépendances**:
- Utilise `btc_cache_fnc_create` pour les caches chimiques
- Utilise ACE3 Medical

---

### 18. SYSTÈME SPECTRE (spect)
**Répertoire**: `core/fnc/spect/`

**Fonction principale**: Gestion des dispositifs de spectre (détection électronique).

**Fichiers**:
- `checkLoop.sqf` : Boucle de vérification
- `electronicFailure.sqf` : Panne électronique
- `updateDevice.sqf` : Met à jour le dispositif
- `frequencies.sqf` : Fréquences
- `disableDevice.sqf` : Désactive le dispositif

**Variables globales**:
- `btc_p_spect` : Système spectre activé

---

### 19. SYSTÈME DE SOURDITÉ (deaf)
**Répertoire**: `core/fnc/deaf/`

**Fonction principale**: Gestion de la surdité temporaire après explosions.

**Fichiers**:
- `earringing.sqf` : Acouphènes

---

### 20. SYSTÈME DE TÂCHES (task)
**Répertoire**: `core/fnc/task/`

**Fonction principale**: Gestion des tâches de mission.

**Fichiers**:
- `create.sqf` : Crée une tâche
- `setState.sqf` : Définit l'état
- `setDescription.sqf` : Définit la description (client)
- `abort.sqf` : Abandonne une tâche (client)

**Variables globales**:
- `btc_task_list` : Liste des tâches

**Interdépendances**:
- Utilisé par tous les systèmes qui créent des tâches

---

### 21. SYSTÈME DE DRAPEAUX (flag)
**Répertoire**: `core/fnc/flag/`

**Fonction principale**: Gestion des drapeaux.

**Fichiers**:
- `deploy.sqf` : Déploie un drapeau (client)
- `int.sqf` : Initialise les drapeaux

**Interdépendances**:
- Utilisé par le système de déploiement pour les checkpoints

---

### 22. SYSTÈME DE TAGS (tag)
**Répertoire**: `core/fnc/tag/`

**Fonction principale**: Gestion du marquage (spray paint).

**Fichiers**:
- `initArea.sqf` : Initialise une zone de tag
- `eh.sqf` : Event handlers
- `create.sqf` : Crée un tag
- `vehicle.sqf` : Tag sur véhicule

**Interdépendances**:
- Utilise ACE3 Tagging

---

### 23. SYSTÈME DE PATROUILLES (patrol) - AMÉLIORÉ
**Répertoire**: `core/fnc/patrol/`

**Fonction principale**: Gestion des patrouilles ennemies avec système automatique, séparation stricte militaire/civile, comportement agressif configurable, et mise à jour dynamique des waypoints.

**Fichiers**:
- `autoSpawn.sqf` : Système de création automatique de patrouilles militaires (timer configurable)
- `init.sqf` : Initialise les routes de patrouille entre villes
- `addWP.sqf` : Ajoute des waypoints avec paramètres selon type (militaires/civiles)
- `WPCheck.sqf` : Vérifie les waypoints et réinitialise si nécessaire
- `playersInAreaCityGroup.sqf` : Vérifie présence de joueurs (suppression si trop loin)
- `usefulCity.sqf` : Sélection intelligente des destinations (priorité joueurs > villes libérées > villes occupées)
- `eh.sqf` : Event handlers et gestion de suppression
- `addEH.sqf` : Ajoute les event handlers aux groupes
- `updateCombatWaypoints.sqf` : Mise à jour dynamique des waypoints toutes les 1 minute si en combat
- `hasPlayersNearby.sqf` : Détecte présence de joueurs dans un rayon (exclusion zones spawn)
- `getOutVehicle.sqf` : Gestion de sortie des véhicules (passagers seulement si armé)

**Séparation stricte**:
- **Patrouilles militaires** (`btc_patrol_active`) : Créées uniquement via timer automatique
  - Comportement agressif : AWARE/RED/FULL/COLUMN
  - Villes de départ : Uniquement villes ennemies occupées (`occupied = true`)
  - Exclusion : Aucun spawn dans un rayon de 1000m autour des joueurs
  - Destinations : Priorité positions joueurs > villes libérées > villes occupées

- **Patrouilles civiles** (`btc_civ_veh_active`) : Créées lors de l'activation des villes
  - Comportement passif : CARELESS/BLUE/LIMITED/COLUMN
  - Villes de départ/destination : Uniquement villes libérées (`occupied = false`)
  - Pas de spawn si aucune ville libérée sur la carte

**Variables globales**:
- `btc_patrol_active` : Liste des patrouilles militaires actives
- `btc_civ_veh_active` : Liste des patrouilles civiles actives
- `btc_patrol_area` : Rayon de patrouille (1500m par défaut)
- `btc_p_patrol_timer` : Intervalle de création automatique (secondes, 0 = désactivé)
- `btc_p_patrol_max` : Nombre maximum de patrouilles militaires
- `btc_p_patrol_vehicle_percent` : Pourcentage de patrouilles avec véhicules (0-100%)
- `btc_p_patrol_exclusion_base_distance` : Distance d'exclusion autour de la base (500m-5000m, défaut: 1500m)
- `btc_patrol_exclusion_zones` : Zones d'exclusion personnalisées (définies dans `define_mod.sqf`)
- `btc_patrol_recent_cities` : Liste des villes récemment sélectionnées (tracking pour éviter répétitions)
- `btc_auto_patrol` : Variable marquant les patrouilles automatiques (comportement agressif)

**Fonctionnalités avancées**:
- **Waypoint GETOUT** : Les patrouilles en véhicules terrestres font sortir les unités avant d'arriver à destination (100m avant)
  - Véhicules armés : Seuls les passagers sortent (conducteur/tireurs restent)
  - Véhicules non armés : Toutes les unités sortent
- **Mise à jour dynamique** : Waypoints recréés toutes les 1 minute en combat pour suivre les joueurs
- **Exclusion zones joueurs** : Aucun spawn dans un rayon de 1000m autour des joueurs
- **Exclusion zones sensibles** : Aucun spawn dans un rayon de 1500m autour des zones de ressources, FOB, checkpoints
- **Exclusion base configurable** : Aucun spawn dans un rayon configurable autour du marqueur `btc_base` (500m-5000m, défaut: 1500m)
- **Zones d'exclusion personnalisées** : Possibilité d'ajouter des zones d'exclusion dans `define_mod.sqf` via `btc_patrol_exclusion_zones`
- **Priorisation intelligente** : Les patrouilles ciblent d'abord les positions des joueurs, puis les villes libérées
- **Tracking des villes** : Système de tracking pour éviter que les patrouilles se dirigent toujours vers les mêmes villes (exclusion des villes sélectionnées dans les 5 dernières minutes)
- **Recapture des villes** : Les patrouilles peuvent recapturer les villes libérées (40% de chance) et créer des missions checkpoint (40% de chance)

**Interdépendances**:
- Utilise `btc_mil_fnc_create_patrol` pour créer les patrouilles militaires
- Utilise `btc_civ_fnc_create_patrol` pour créer les patrouilles civiles
- Utilise `btc_fnc_find_closecity` pour trouver les villes proches
- Utilise `btc_city_all` pour accéder aux villes

---

### 24. SYSTÈME DE VÉHICULES (veh)
**Répertoire**: `core/fnc/veh/`

**Fonction principale**: Gestion des véhicules.

**Fichiers**:
- `init.sqf` : Initialise les véhicules
- `add.sqf` : Ajoute un véhicule
- `addRespawn.sqf` : Ajoute un respawn
- `respawn.sqf` : Fait respawn un véhicule
- `killed.sqf` : Gère la destruction
- `propertiesGet.sqf` : Récupère les propriétés
- `propertiesSet.sqf` : Définit les propriétés
- `inventoryRestore.sqf` : Restaure l'inventaire

**Variables globales**:
- `btc_vehicles` : Liste des véhicules
- `btc_veh_respawnable` : Véhicules respawnables

---

### 25. SYSTÈME D'ARSENAL (arsenal)
**Répertoire**: `core/fnc/arsenal/`

**Fonction principale**: Gestion de l'arsenal.

**Fichiers**:
- `data.sqf` : Données de l'arsenal
- `garage.sqf` : Garage
- `loadout.sqf` : Loadout
- `trait.sqf` : Traits
- `ammoUsage.sqf` : Utilisation des munitions (client)
- `weaponsfilter.sqf` : Filtre des armes (client)

**Interdépendances**:
- Utilise ACE3 Arsenal ou BI Arsenal

---

### 26. SYSTÈME DE DONNÉES (data)
**Répertoire**: `core/fnc/data/`

**Fonction principale**: Gestion des données partagées (headless).

**Fichiers**:
- `add_group.sqf` : Ajoute un groupe
- `get_group.sqf` : Récupère un groupe
- `spawn_group.sqf` : Spawne un groupe

**Interdépendances**:
- Utilisé par le système headless

---

### 27. SYSTÈME DE RETARD (delay)
**Répertoire**: `core/fnc/delay/`

**Fonction principale**: Gestion des créations différées.

**Fichiers**:
- `createUnit.sqf` : Crée une unité avec délai
- `createVehicle.sqf` : Crée un véhicule avec délai
- `createAgent.sqf` : Crée un agent avec délai
- `exec.sqf` : Exécute avec délai
- `waitAndExecute.sqf` : Attend et exécute

**Interdépendances**:
- Utilisé pour optimiser les spawns

---

### 28. SYSTÈME DE SLOTS (slot)
**Répertoire**: `core/fnc/slot/`

**Fonction principale**: Gestion de la sérialisation des slots.

**Fichiers**:
- `serializeState.sqf` : Sérialise l'état
- `deserializeState.sqf` : Désérialise (client)
- `deserializeState_s.sqf` : Désérialise (serveur)
- `createKey.sqf` : Crée une clé

**Interdépendances**:
- Utilisé par le système de sauvegarde

---

### 29. SYSTÈME DE BASE DE DONNÉES (db)
**Répertoire**: `core/fnc/db/`

**Fonction principale**: Gestion de la sauvegarde et du chargement.

**Fichiers**:
- `save.sqf` : Sauvegarde l'état de la mission
- `load.sqf` : Charge l'état de la mission
- `load_old.sqf` : Charge (ancien format)
- `delete.sqf` : Supprime une sauvegarde
- `autoSaveLoop.sqf` : Boucle de sauvegarde automatique
- `autoRestart.sqf` : Redémarrage automatique
- `autoRestartLoop.sqf` : Boucle de redémarrage
- `loadcargo.sqf` : Charge le cargo
- `loadObjectStatus.sqf` : Charge le statut des objets
- `saveObjectStatus.sqf` : Sauvegarde le statut des objets
- `setTurretMagazines.sqf` : Définit les munitions des tourelles

**Variables globales**:
- `btc_db_load` : Charger au démarrage
- `btc_db_auto_save_enabled` : Sauvegarde auto activée
- `btc_db_auto_save_interval` : Intervalle de sauvegarde

**Interdépendances**:
- Sauvegarde tous les systèmes de la mission

---

### 30. SYSTÈME D'EVENT HANDLERS (eh)
**Répertoire**: `core/fnc/eh/`

**Fonction principale**: Gestion des event handlers.

**Fichiers**:
- `server.sqf` : Event handlers serveur
- `player.sqf` : Event handlers joueur
- `playerConnected.sqf` : Joueur connecté
- `headless.sqf` : Event handlers headless
- `CuratorObjectPlaced.sqf` : Objet placé via Zeus
- `trackItem.sqf` : Suivi d'objets
- `xeh_PreInit_EH.hpp` : PreInit handlers
- `xeh_InitPost_EH_Vehicle.hpp` : InitPost handlers véhicules

**Interdépendances**:
- Utilisé par tous les systèmes

---

### 31. SYSTÈME COMMUN (common)
**Répertoire**: `core/fnc/common/`

**Fonction principale**: Fonctions utilitaires communes.

**Fichiers**:
- `check_los.sqf` : Vérifie la ligne de vue
- `create_composition.sqf` : Crée une composition
- `house_addWP.sqf` : Ajoute des waypoints dans une maison
- `house_addWP_loop.sqf` : Boucle waypoints maison
- `set_damage.sqf` : Définit les dégâts
- `road_direction.sqf` : Direction d'une route
- `findsafepos.sqf` : Trouve une position sûre
- `find_closecity.sqf` : Trouve une ville proche
- `delete.sqf` : Supprime des objets
- `deleteEntities.sqf` : Supprime des entités
- `final_phase.sqf` : Phase finale
- `findposoutsiderock.sqf` : Trouve une position hors rocher
- `typeOf.sqf` : Type d'objet
- `roof.sqf` : Toit
- `moveOut.sqf` : Fait sortir
- `changeWeather.sqf` : Change la météo
- `get_cardinal.sqf` : Point cardinal
- `show_hint.sqf` : Affiche un hint
- `set_markerTextLocal.sqf` : Définit le texte d'un marqueur
- `showSubtitle.sqf` : Affiche un sous-titre
- `get_composition.sqf` : Récupère une composition
- `checkArea.sqf` : Vérifie une zone
- `typeOfPreview.sqf` : Type de preview
- `get_class.sqf` : Récupère une classe
- `randomize_pos.sqf` : Randomise une position
- `getHouses.sqf` : Récupère les maisons
- `end_mission.sqf` : Fin de mission
- `loadConfigFromParams.sqf` : Charge la config depuis paramètres
- `configUnitTypes.sqf` : Configuration des types d'unités

**Interdépendances**:
- Utilisé par tous les systèmes

---

### 32. SYSTÈME D'INTERACTIONS (int)
**Répertoire**: `core/fnc/int/`

**Fonction principale**: Gestion des interactions avec les civils.

**Fichiers**:
- `add_actions.sqf` : Ajoute les actions
- `orders.sqf` : Ordres
- `orders_give.sqf` : Donne des ordres
- `orders_behaviour.sqf` : Comportement des ordres
- `shortcuts.sqf` : Raccourcis clavier
- `terminal.sqf` : Terminal
- `foodGive.sqf` : Donne de la nourriture
- `ordersLoop.sqf` : Boucle des ordres
- `ask_var.sqf` : Demande une variable
- `checkSirenBeacons.sqf` : Vérifie les sirènes/gyrophares
- `horn.sqf` : Klaxon

**Interdépendances**:
- Utilise `btc_info_fnc_*` pour les informations
- Utilise `btc_rep_fnc_change` pour la réputation

---

### 33. SYSTÈME DE DEBUG (debug)
**Répertoire**: `core/fnc/debug/`

**Fonction principale**: Outils de débogage.

**Fichiers**:
- `message.sqf` : Messages de debug
- `marker.sqf` : Marqueurs de debug (client)
- `units.sqf` : Unités de debug (client)
- `fps.sqf` : FPS (client)
- `graph.sqf` : Graphiques (client)
- `defines.hpp` : Définitions
- `dlg.hpp` : Interface

**Variables globales**:
- `btc_debug` : Mode debug activé
- `btc_debug_log` : Logs de debug

---

## 🎮 SYSTÈMES PERSONNALISÉS (LEON)

### 1. SYSTÈME DE DÉPLOIEMENT (deploy)
**Répertoire**: `core/fnc/deploy/`

**Fonction principale**: Système de déploiement d'objets, unités, et checkpoints avec gestion des ressources.

**Fichiers principaux**:

#### Initialisation
- `init.sqf` : Initialise le système de déploiement
  - Configure les EventHandlers (`EntityCreated`, `CuratorObjectPlaced`)
  - Configure le cargo pour tous les véhicules
  - Initialise les variables globales
  - Démarre les systèmes de monitoring

#### Interface Utilisateur
- `initUI.sqf` : Initialise l'interface de déploiement
  - Définit les catégories d'objets (fortifications, troupes, checkpoints, caisses)
  - Configure les coûts en ressources
  - Crée les menus interactifs

- `create.sqf` : Crée l'interface de déploiement (client)
  - Affiche le menu de sélection
  - Gère la navigation dans les catégories

- `create_apply.sqf` : Applique la création d'un objet
- `create_load.sqf` : Charge un objet dans un véhicule
- `create_change_target.sqf` : Change la cible de déploiement
- `create_change_subcategory.sqf` : Change la sous-catégorie

#### Placement et Preview
- `startPreviewMode.sqf` : Démarre le mode preview
- `previewLoop.sqf` : Boucle de preview (déplacement, rotation)
- `exitPreviewMode.sqf` : Sort du mode preview
- `confirmPlacement.sqf` : Confirme le placement
  - Crée l'objet final
  - Déduit les ressources
  - Configure le cargo si nécessaire
  - Applique les inventaires aux caisses
  - Ajoute les actions

- `place_create_camera.sqf` : Crée la caméra de placement
- `place_destroy_camera.sqf` : Détruit la caméra
- `place_key_down.sqf` : Gère les touches (rotation, validation)

#### Gestion des Objets
- `deleteDeployedObject.sqf` : Supprime un objet déployé (avec remboursement)
- `deleteDeployedObjectNoRefund.sqf` : Supprime sans remboursement
- `redeplacerObjet.sqf` : Redéplace un objet

#### Configuration Cargo
- `setupCargo.sqf` : Configure l'espace cargo ACE pour un véhicule
  - Supporte tous les types de véhicules (air, terre, mer)
  - Configuration personnalisable via `btc_deploy_customCargoClasses`
  - Gère les conteneurs (`Land_Cargo20_military_green_F`, `Land_Cargo40_military_green_F`)

- `cargoSizes.sqf` : Définit les tailles de cargo pour les objets déployables

#### Coûts et Remboursements
- `getObjectCosts.sqf` : Récupère les coûts d'un objet
- `getRefundCategory.sqf` : Récupère la catégorie de remboursement
- `calculateCrateRefund.sqf` : Calcule le remboursement d'une caisse
- `refundSingleCrate.sqf` : Rembourse une caisse (client)
- `refundSingleCrate_server.sqf` : Rembourse (serveur)
- `refundSingleCrateFull.sqf` : Rembourse complètement

#### Troupes et Unités
- `addTroop.sqf` : Ajoute une troupe
- `getUnitClasses.sqf` : Récupère les classes d'unités
- `loadouts.sqf` : Définit les loadouts
- `loadoutConfig.sqf` : Configuration des loadouts
- `placeCarriedUnit.sqf` : Place une unité portée
- `dropCarriedUnit.sqf` : Lâche une unité portée
- `createDroneCrew.sqf` : Crée l'équipage d'un drone
- `assignUnitToVehicle.sqf` : Assigne une unité à un véhicule
- `clearUnitVehicleAssignment.sqf` : Efface l'assignation
- `setUnitStance.sqf` : Définit la posture d'une unité
- `exitUnitFromVehicle.sqf` : Fait sortir une unité d'un véhicule
- `autoAssignUnits.sqf` : Assigne automatiquement les unités aux véhicules/statiques

#### Checkpoints
- `detectFlags.sqf` : Détecte les drapeaux déployés et crée des checkpoints
- `activateCheckpointTask.sqf` : Active la tâche d'un checkpoint
- `monitorCheckpointTask.sqf` : Surveille la tâche
- `completeCheckpointTask.sqf` : Complète la tâche (succès/échec)
- `deactivateCheckpointTask.sqf` : Désactive la tâche
- `monitorCheckpoints.sqf` : Surveille tous les checkpoints actifs
- `checkpointCounterAttack.sqf` : Contre-attaque sur un checkpoint
- `manualCheckpointCounterAttack.sqf` : Contre-attaque manuelle
- `monitorCounterAttacks.sqf` : Surveille les contre-attaques de checkpoints
- `syncCheckpoints.sqf` : Synchronise les checkpoints (JIP)
- `checkpointStatus.sqf` : Statut des checkpoints
- `manageNearestCity.sqf` : Gère la ville la plus proche d'un checkpoint

#### Zones de Libération
- `liberateZone.sqf` : Libère une zone autour d'un checkpoint

#### Échange de Ressources
- `openResourceExchangeDialog.sqf` : Ouvre le dialogue d'échange
- `initResourceExchangeDialog.sqf` : Initialise le dialogue
- `processResourceExchange.sqf` : Traite l'échange de ressources

#### Réparation
- `getVehicleRepairType.sqf` : Récupère le type de réparation
- `repairWreckWithResources.sqf` : Répare une épave avec ressources

#### Ordinateurs Éditeur
- `initEditorComputers.sqf` : Initialise les ordinateurs de l'éditeur

#### Mises à jour
- `updates.sqf` : Mises à jour du système

**Variables globales**:
- `btc_deploy_fortifications` : Array des fortifications disponibles
- `btc_deploy_troops` : Array des troupes disponibles
- `btc_deploy_checkpoints` : Array des checkpoints disponibles
- `btc_deploy_activeCheckpoints` : Checkpoints actifs
- `btc_deploy_customCargoClasses` : Configuration cargo personnalisée
- `btc_deploy_last_attacked_checkpoints` : Derniers checkpoints attaqués

**Interdépendances**:
- Utilise `btc_ressources_fnc_*` pour les ressources
- Utilise `btc_task_fnc_create` pour les tâches
- Utilise `btc_mil_fnc_create_group` pour les contre-attaques
- Utilise `btc_city_fnc_*` pour les villes

---

### 2. SYSTÈME DE RESSOURCES (ressources)
**Répertoire**: `core/fnc/ressources/`

**Fonction principale**: Gestion des 4 ressources (Fer, Munition, Nourriture, Fuel) avec zones de génération, collecte, et contre-attaques.

**Fichiers principaux**:

#### Initialisation
- `init.sqf` : Initialise le système de ressources
  - Charge ou crée les zones
  - Initialise les dépôts de collecte
  - Initialise les zones de collecte
  - Démarre tous les systèmes de monitoring
  - Gère la restauration après chargement

- `initClient.sqf` : Initialise le client
  - Crée l'interface utilisateur
  - Configure les raccourcis clavier

- `initDepots.sqf` : Initialise les dépôts de collecte
  - Détecte les dépôts existants
  - Crée les dépôts configurés
  - Prévention des doublons

- `initCollectAreas.sqf` : Initialise les zones de collecte
  - Crée les barrières visuelles
  - Configure les positions de collecte

#### Gestion des Zones
- `createZones.sqf` : Crée les zones de ressources
- `zoneManager.sqf` : Gère les zones
- `activateZone.sqf` : Active une zone
- `deactivateZone.sqf` : Désactive une zone
  - Supprime tous les objets de ressources
  - Nettoie les variables
- `captureZone.sqf` : Capture une zone

#### Génération de Ressources
- `generationLoop.sqf` : Boucle principale de génération
- `monitorZoneSlots.sqf` : Surveille les slots d'une zone
  - Crée progressivement les objets (1 par cycle)
  - Vérifie les slots disponibles
  - Arrête si zone pleine ou tâche active
- `generateResourceItem.sqf` : Génère un objet de ressource
- `checkResourceItemSpace.sqf` : Vérifie l'espace disponible

#### Collecte
- `monitorDepots.sqf` : Surveille les dépôts pour collecte automatique
  - Détecte les objets dans le rayon
  - Vérifie qu'ils ne sont pas en cargo/portés
  - Exclut les objets dans la zone de collecte
  - Collecte automatiquement
- `collectResourceItem.sqf` : Collecte un objet de ressource
  - Ajoute les ressources au total
  - Supprime l'objet
  - Met à jour l'UI
- `addResourceItemActions.sqf` : Ajoute les actions aux objets
  - Portage manuel
  - Chargement dans véhicule
  - Ajustement position/rotation

#### Tâches
- `createCollectTask.sqf` : Crée la tâche de collecte
- `monitorCollectTask.sqf` : Surveille la tâche de collecte
  - Vérifie si la zone est vide
  - Valide la tâche quand zone vide
  - Relance la génération
- `verifyCollectTasks.sqf` : Vérifie périodiquement les tâches (5 min)
  - Crée les tâches manquantes
  - Synchronise les variables

#### Défense
- `monitorZonesDefense.sqf` : Surveille les zones pour contre-attaques
- `activateDefenseTask.sqf` : Active la tâche de défense
- `monitorDefenseTask.sqf` : Surveille la tâche de défense
  - Vérifie si la zone est toujours contrôlée
  - Échoue si zone reprise
  - Succès si défendue

#### Contre-Attaques
- `counterAttackLoop.sqf` : Boucle principale des contre-attaques
  - Sélection aléatoire des zones
  - Évite les répétitions (mémoire de 5)
- `counterAttack.sqf` : Lance une contre-attaque
  - Sélectionne une planque ou position aléatoire
  - Crée les groupes d'ennemis
  - Gère les bateaux si spawn dans l'eau
  - Crée les waypoints (plage → débarquement → zone → patrouille)
  - Vérifie les distances aux bâtiments (50m minimum)
  - Vérifie les routes pour véhicules
- `manualZoneCounterAttack.sqf` : Contre-attaque manuelle

#### Gestion des Ressources
- `addResources.sqf` : Ajoute des ressources
- `setResources.sqf` : Définit les ressources
- `resetResources.sqf` : Réinitialise les ressources
- `showResources.sqf` : Affiche les ressources (client)
- `deductCost.sqf` : Déduit un coût
- `deductCostsArray.sqf` : Déduit plusieurs coûts
- `checkCost.sqf` : Vérifie si un coût peut être payé
- `getCosts.sqf` : Récupère les coûts
- `syncCosts.sqf` : Synchronise les coûts (multijoueur)
- `moneyManagement.sqf` : Gestion de l'argent (compilé)

#### Interface Utilisateur
- `createUI.sqf` : Crée l'interface
- `updateUI.sqf` : Met à jour l'interface
- `updateUILoop.sqf` : Boucle de mise à jour
- `showUI.sqf` : Affiche l'interface
- `hideUI.sqf` : Cache l'interface
- `keyHandler.sqf` : Gère les touches
- `initDisplaySystem.sqf` : Initialise le système d'affichage
- `getResourceIconType.sqf` : Récupère l'icône d'une ressource

#### Zones de Collecte
- `createCollectAreaBarriers.sqf` : Crée les barrières visuelles

#### Commandes Admin
- `adminCommands.sqf` : Commandes administrateur
- `startAllGeneration.sqf` : Démarre toute la génération
- `stopAllGeneration.sqf` : Arrête toute la génération
- `toggleZoneGeneration.sqf` : Active/désactive la génération d'une zone

#### Monitoring
- `monitorZones.sqf` : Surveille les zones pour détecter la capture
- `monitorCache.sqf` : Surveille le cache (obsolète ?)

**Variables globales**:
- `btc_ressources_zones_objects` : Array des zones
- `btc_ressources_config` : Configuration des ressources
- `btc_ressources_amounts` : Quantités actuelles [Fer, Munition, Nourriture, Fuel, Argent]
- `btc_ressources_collected_items` : Objets collectés
- `btc_ressources_collect_depots_objects` : Dépôts de collecte
- `btc_ressources_last_attacked_zones` : Dernières zones attaquées (mémoire 5)
- `btc_ressources_last_used_hideouts` : Dernières planques utilisées (mémoire 5)
- `btc_ressources_generation_rate` : Taux de génération
- `btc_ressources_collect_area_size` : Taille de la zone de collecte
- `btc_ressources_collect_depot_radius` : Rayon des dépôts

**Interdépendances**:
- Utilisé par le système de déploiement pour les coûts
- Utilise `btc_mil_fnc_create_group` pour les contre-attaques
- Utilise `btc_task_fnc_create` pour les tâches
- Utilise `btc_hideouts` pour les contre-attaques

---

### 3. SYSTÈME D'ACTIONS (addaction)
**Répertoire**: `core/fnc/addaction/`

**Fonction principale**: Gestion centralisée de toutes les actions scroll wheel.

**Fichiers**:
- `init.sqf` : Initialise le système d'actions
  - Configure l'EventHandler `EntityCreated`
  - Démarre la boucle de synchronisation
  - Ajoute les actions aux objets existants

- `syncAllActions.sqf` : Synchronise toutes les actions (JIP)
- `syncActionsForObject.sqf` : Synchronise les actions d'un objet
- `addPlayerActions.sqf` : Ajoute les actions joueur
- `addCheckpointActions.sqf` : Ajoute les actions de checkpoint

#### Actions Admin
- `adminReactivateCheckpoint.sqf` : Réactive un checkpoint (client)
- `adminReactivateCheckpointServer.sqf` : Réactive (serveur)
- `adminReactivateZone.sqf` : Réactive une zone
- `adminToggleGeneration.sqf` : Active/désactive la génération
- `adminReactivateTriggersCheckpoint.sqf` : Réactive les triggers checkpoint
- `adminReactivateTriggersZone.sqf` : Réactive les triggers zone
- `adminResetAllActions.sqf` : Réinitialise toutes les actions
- `adminVerifySystem.sqf` : Vérifie le système

#### Actions Contre-Attaques
- `launchCheckpointCounterAttack.sqf` : Lance contre-attaque checkpoint
- `launchZoneCounterAttack.sqf` : Lance contre-attaque zone
- `launchFOBCounterAttack.sqf` : Lance contre-attaque FOB

**Interdépendances**:
- Utilisé par tous les systèmes qui ont des actions
- Séparation stricte entre actions de déploiement et de ressources

---

### 4. SYSTÈME UTILITAIRE (utility)
**Répertoire**: `core/fnc/utility/`

#### 4.1 Configuration (LEON_config)
- `LEON_config.sqf` : Configuration générale des systèmes personnalisés

#### 4.2 Drone (LEON_drone)
- `LEON_drone/init.sqf` : Initialise le système de drone
- `LEON_drone/drone.sqf` : Gère le drone de surveillance
- `LEON_drone/gui_classes.hpp` : Classes d'interface

#### 4.3 Halo (LEON_halo)
- `LEON_halo/LEON_Halo.sqf` : Système de saut HALO
- `LEON_halo/defines.hpp` : Définitions
- `LEON_halo/Halo_interface.hpp` : Interface
- `LEON_halo/Halo_sounds.hpp` : Sons
- `LEON_halo/standard_controls.hpp` : Contrôles

#### 4.4 Téléporteur (LEON_teleporteur)
- `LEON_teleporteur/teleporter.sqf` : Système de téléportation

#### 4.5 Portage (LEON_portage)
- `LEON_portage/init.sqf` : Initialise le portage de caisses
- `LEON_portage/config_caisses.sqf` : Configuration des caisses portables
- `LEON_portage/functions/fn_initPortageCaisse.sqf` : Initialise le portage
- `LEON_portage/functions/fn_gererMouvementCaisse.sqf` : Gère le mouvement

#### 4.6 Invincibilité (LEON_invincible)
- `LEON_invincible/init.sqf` : Initialise l'invincibilité
- `LEON_invincible/invincible.sqf` : Gère l'invincibilité temporaire

#### 4.7 Véhicules (LEON_vehicles)
- `LEON_vehicles/initCaisse.sqf` : Initialise les caisses de ravitaillement
  - Définit les fonctions `OPEX_fnc_*`
  - Configure les inventaires personnalisés
  - Gère les caisses créées via Zeus/éditeur
- `LEON_vehicles/initCaisseRepair.sqf` : Initialise les caisses de réparation
- `LEON_vehicles/medic_repa.sqf` : Système médical/réparation véhicules
- `LEON_vehicles/vehicleInventory.sqf` : Gestion d'inventaire des véhicules

#### 4.8 Debug (LEON_debug)
- `LEON_debug/testSystem.sqf` : Tests du système
- `LEON_debug/verifySystem.sqf` : Vérifie le système
- `LEON_debug/closeDebugUI.sqf` : Ferme l'UI de debug
- `LEON_debug/debugCheckpoints.sqf` : Debug des checkpoints
- `LEON_debug/createDocumentation.sqf` : Crée la documentation
- `LEON_debug/verificationCohérence.sqf` : Vérification de cohérence

#### 4.9 Test (LEON_test)
- `LEON_test/giveLoadout.sqf` : Donne un loadout
- `LEON_test/disablePathOppfor.sqf` : Désactive le pathfinding ennemi

---

### 5. SYSTÈME GARAGE VIRTUEL (virtual_garage)
**Répertoire**: `core/fnc/virtual_garage/`

**Fonction principale**: Garage virtuel pour stocker/récupérer des véhicules.

**Fichiers**:
- `init.sqf` : Initialise le garage
- `fnc_openGarage.sqf` : Ouvre le garage
- `fnc_openZenGarage.sqf` : Ouvre le garage Zeus

**Interdépendances**:
- Utilise le système de sauvegarde pour persister les véhicules

---

## 🔄 FLUX D'INITIALISATION

### Séquence d'initialisation complète

1. **init.sqf** (racine)
   - Compile `core/def/mission.sqf` (définitions)
   - Compile `define_mod.sqf` (mods)
   - Si serveur : `core/init_server.sqf`
   - `core/init_common.sqf` (commun)
   - `core/fnc/compile.sqf` (compilation fonctions)
   - `core/fnc/deploy/init.sqf` (déploiement)
   - Si joueur : `core/init_player.sqf`
   - Si headless : `core/init_headless.sqf`
   - `LEON_config` (configuration)
   - Si interface : `LEON_drone_fnc_init`
   - Si serveur : Initialise caisses, véhicules, Halo

2. **core/init_server.sqf**
   - `btc_city_fnc_init` (villes)
   - Initialise groupes dynamiques
   - Configure temps
   - Crée tâches principales
   - Si chargement : `btc_db_fnc_load`
   - Sinon : Crée planques, cache, configure véhicules
   - `btc_eh_fnc_server` (event handlers)
   - `btc_ied_fnc_fired_near` (IED)
   - `btc_chem_fnc_checkLoop` (chimique)
   - `btc_spect_fnc_checkLoop` (spectre)
   - `btc_db_fnc_autoRestartLoop` (redémarrage auto)
   - Si sauvegarde auto : `btc_db_fnc_autoSaveLoop`
   - `btc_deploy_fnc_init` (déploiement)
   - Attend initialisation déploiement
   - `btc_deploy_fnc_monitorCheckpoints` (surveillance checkpoints)
   - `btc_deploy_fnc_monitorCounterAttacks` (contre-attaques checkpoints)
   - `btc_ressources_fnc_init` (ressources)
   - Configure respawn véhicules
   - Si missions secondaires : `btc_side_fnc_create`
   - Configure tags personnalisés
   - Configure tickets respawn

3. **core/init_common.sqf**
   - Configuration commune serveur/client

4. **core/init_player.sqf**
   - Configuration spécifique joueur
   - Interface utilisateur
   - Event handlers joueur

5. **core/init_headless.sqf**
   - Configuration headless client

---

## 🔗 INTERDÉPENDANCES

### Dépendances critiques

**Système de Ressources** :
- Dépend de : `btc_mil_fnc_create_group`, `btc_task_fnc_create`, `btc_hideouts`
- Utilisé par : Système de déploiement (coûts)

**Système de Déploiement** :
- Dépend de : Système de ressources (coûts), `btc_task_fnc_create`, `btc_mil_fnc_create_group`, `btc_city_fnc_*`
- Utilisé par : Actions, sauvegarde

**Système de Villes** :
- Dépend de : `btc_mil_fnc_create_group`, `btc_civ_fnc_populate`, `btc_rep_fnc_change`
- Utilisé par : Tous les systèmes de spawn

**Système de Cache** :
- Dépend de : `btc_city_all`, `btc_info_fnc_cache`
- Utilisé par : Système de tâches principales

**Système de Planques** :
- Dépend de : Aucun
- Utilisé par : Contre-attaques (ressources, checkpoints, FOB), `btc_info_fnc_hideout`

**Système de Sauvegarde** :
- Dépend de : Tous les systèmes
- Utilisé par : Initialisation au démarrage

---

## 📊 VARIABLES GLOBALES PRINCIPALES

### Système de base
- `btc_version` : Version de la mission [0, 2, 1]
- `btc_player_side` : Côté des joueurs
- `btc_enemy_side` : Côté ennemi
- `btc_city_all` : HashMap de toutes les villes
- `btc_hideouts` : Array des planques
- `btc_cache_obj` : Objet cache actuel
- `btc_global_reputation` : Réputation globale
- `btc_type_units` : Types d'unités ennemis
- `btc_type_vehicles` : Types de véhicules ennemis
- `btc_type_boats` : Types de bateaux ennemis
- `btc_vehicles` : Liste des véhicules
- `btc_fobs` : Array des FOB
- `btc_respawn_tickets` : Tickets de respawn

### Système de déploiement
- `btc_deploy_fortifications` : Fortifications disponibles
- `btc_deploy_troops` : Troupes disponibles
- `btc_deploy_checkpoints` : Checkpoints disponibles
- `btc_deploy_activeCheckpoints` : Checkpoints actifs
- `btc_deploy_customCargoClasses` : Configuration cargo personnalisée
- `btc_deploy_last_attacked_checkpoints` : Derniers checkpoints attaqués

### Système de ressources
- `btc_ressources_zones_objects` : Zones de ressources
- `btc_ressources_config` : Configuration des ressources
- `btc_ressources_amounts` : Quantités [Fer, Munition, Nourriture, Fuel, Argent]
- `btc_ressources_collected_items` : Objets collectés
- `btc_ressources_collect_depots_objects` : Dépôts de collecte
- `btc_ressources_last_attacked_zones` : Dernières zones attaquées
- `btc_ressources_last_used_hideouts` : Dernières planques utilisées

---

## 🎯 EVENT HANDLERS

### EventHandlers serveur (`btc_eh_fnc_server`)
- `EntityCreated` : Initialise les objets créés
- `EntityKilled` : Gère la mort des entités
- `EntityRespawned` : Gère le respawn

### EventHandlers joueur (`btc_eh_fnc_player`)
- `CuratorObjectPlaced` : Objet placé via Zeus
- `GetInMan` : Entrée dans véhicule
- `GetOutMan` : Sortie de véhicule
- `Killed` : Mort du joueur
- `Respawn` : Respawn du joueur

### EventHandlers déploiement
- `EntityCreated` : Configure le cargo des véhicules
- `CuratorObjectPlaced` : Configure les objets Zeus

### EventHandlers ressources
- `EntityCreated` : Ajoute les actions aux objets de ressources

---

## 💾 SYSTÈME DE SAUVEGARDE/CHARGEMENT

### Format de sauvegarde

**Zones de ressources** : 17 éléments
- Position, direction, nom, type, config, slots, etc.

**Objets collectés** : 7 éléments
- Position, direction, type, quantité, zone, etc.

**Cargo** : 5 éléments
- Classname, position, direction, dégâts, contenu

**Checkpoints** : Format spécifique
- ID, position, drapeau, tâche, etc.

**FOB** : Format spécifique
- Position, direction, nom, structure, etc.

**Villes** : Format spécifique
- ID, position, état, etc.

### Chargement
- Vérifie la version de sauvegarde
- Charge tous les systèmes dans l'ordre
- Restaure les objets et leurs propriétés
- Relance les systèmes de monitoring
- Synchronise avec `publicVariable`

---

## ⚠️ POINTS D'ATTENTION

1. **Interdépendances multiples** : La mission de base a de nombreuses interdépendances. Toute modification nécessite de vérifier les impacts.

2. **Synchronisation multijoueur** : Utiliser `publicVariable` pour synchroniser les variables importantes.

3. **Performance** : Utiliser `distanceSqr` au lieu de `distance`, filtrer avant d'itérer, cacher les valeurs calculées.

4. **Format de sauvegarde** : Le format est strict. Ne pas modifier sans prévoir la migration.

5. **Event Handlers** : Vérifier l'ordre d'exécution et éviter les conflits.

6. **Actions** : Séparer strictement les actions de déploiement et de ressources via `btc_ressources_item_valid`.

---

## 📝 NOTES DE DÉVELOPPEMENT

- Tous les scripts doivent avoir un header avec auteur, description, version
- Utiliser `diag_log` pour le débogage (avec préfixes clairs)
- Éviter les `systemChat` et `dialog` (préférence utilisateur)
- Utiliser des backslashes (`\`) pour les chemins de fichiers
- Les commentaires doivent être essentiels uniquement
- Respecter le format de code existant

---

**Document créé le**: 10 Décembre 2025  
**Dernière mise à jour**: 15 Décembre 2025  
**Version mission**: 0.2.1

