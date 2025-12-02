# 📋 Changelog - Hearts and Minds ULTIMATE

Toutes les modifications notables de ce projet seront documentées dans ce fichier.

---

## [0.0.8] - 2025-10-10 🎉 STABLE

### 🔥 Corrections majeures multijoueur

#### Système de Permissions
- ✅ **Ajout** : Système de permissions sur actions ACE (créateur/admin uniquement)
- ✅ **Fix** : Variable `btc_deploy_createdBy` stocke maintenant `name player` au lieu de `player`
- ✅ **Fix** : Vérification permissions dans `addMoveActions.sqf` (Déplacer + Supprimer)
- ✅ **Amélioration** : Objets "System" après restart gérables par tous

#### Système de Remboursement
- ✅ **Fix** : Menu remboursement disparaît automatiquement après sélection
- ✅ **Fix** : B_supplyCrate_F exclue de la liste des caisses remboursables
- ✅ **Amélioration** : Vérification `alive` et `!isNull` pour chaque caisse
- ✅ **Fix** : Liste limitée aux 6 caisses spécifiques définies par l'utilisateur
- ✅ **Ajout** : Support SimplexSupportService via event handler `EntityCreated`
- ✅ **Ajout** : Support Zeus via event handler `CuratorObjectPlaced`
- ✅ **Amélioration** : Déduction automatique des ressources pour caisses externes

#### Système de Réparation Véhicules
- ✅ **Amélioration** : Menu restructuré avec sous-menu CONFIRMER/ANNULER
- ✅ **Fix** : Actions ACE restaurées automatiquement après réparation d'épave
- ✅ **Fix** : Variables `btc_deploy_*` copiées de l'épave au véhicule réparé
- ✅ **Fix** : Correction erreur `getAllVariables` (n'existe pas dans Arma 3)
- ✅ **Amélioration** : Sauvegarde manuelle des 7 variables btc_deploy avant suppression épave

#### Interface Ressources
- ✅ **Fix** : Protection contre double initialisation en mode local/multi
- ✅ **Fix** : Variables UI stockées dans `missionNamespace` au lieu de global
- ✅ **Fix** : Une seule méthode de synchronisation dans `initClient.sqf`
- ✅ **Fix** : Protection boucle `updateUILoop` contre instances multiples
- ✅ **Fix** : Protection `createUI` contre création multiple des contrôles
- ✅ **Fix** : Appel `initDisplaySystem` retiré du serveur (client uniquement)
- ✅ **Amélioration** : Messages debug systemChat pour traçage

#### Système de Checkpoints
- ✅ **Fix** : Anti-duplication drapeaux via variable `btc_deploy_hidden`
- ✅ **Fix** : Ajout immédiat à `btc_deploy_detectedFlags` pour éviter race conditions
- ✅ **Fix** : Vérification position (< 10m) ET objet pour détecter doublons
- ✅ **Amélioration** : Synchronisation `publicVariable "btc_deploy_detectedFlags"`
- ✅ **Fix** : Drapeaux restaurés marqués `btc_deploy_hidden = true`

#### Système de Tâches de Défense
- ✅ **Fix** : Protection contre tâches fantômes via `BIS_fnc_taskExists`
- ✅ **Fix** : Réactivation rapide si ennemis reviennent avant délai
- ✅ **Fix** : Suppression ancienne tâche AVANT création nouvelle
- ✅ **Amélioration** : Messages debug systemChat pour tâches
- ✅ **Fix** : Zones - Réinitialisation flags si délai écoulé
- ✅ **Fix** : Checkpoints - Même logique anti-tâches fantômes
- ✅ **Amélioration** : Vérification état réel tâche, pas juste le flag

#### AI et Unités
- ✅ **Fix** : Compétences AI définies immédiatement (pas en `spawn`)
- ✅ **Fix** : Suppression `allowDamage false` qui bloquait le combat
- ✅ **Fix** : AI activées AVANT désactivation mouvement
- ✅ **Amélioration** : Compétences combat = 1.0 (100%)
- ✅ **Fix** : Configuration identique dans `confirmPlacement` et `addMoveActions`

#### Génération de Ressources
- ✅ **Fix** : Suppression fichier `startGeneration.sqf` (doublon)
- ✅ **Fix** : `generationLoop.sqf` comme source unique de génération
- ✅ **Fix** : Vérification `_captured` ET `_generating` avant ajout ressources
- ✅ **Fix** : Retrait appel `startGeneration` dans `captureZone.sqf`
- ✅ **Amélioration** : Variable `btc_ressources_generating` gérée centralement

#### Icônes et UI
- ✅ **Fix** : Remplacement `money_ca.paa` → `upload_ca.paa` (icône manquante)
- ✅ **Fix** : Correction erreur "Diviseur nul" dans menu ACE
- ✅ **Fix** : Validation de toutes les icônes utilisées

### 📝 Documentation

- ✅ **Ajout** : Section "Multijoueur & JIP" complète
- ✅ **Ajout** : Documentation permissions et sécurité
- ✅ **Ajout** : Support systèmes externes (Simplex/Zeus)
- ✅ **Ajout** : Protections anti-bugs détaillées
- ✅ **Mise à jour** : Tous les coûts et mécaniques actualisés
- ✅ **Mise à jour** : Version système 1.0.0 STABLE in-game

### 🔧 Fichiers modifiés (liste partielle)

**Core Déploiement :**
- `core/fnc/deploy/init.sqf` - Event handler EntityCreated
- `core/fnc/deploy/confirmPlacement.sqf` - AI + creator name
- `core/fnc/deploy/addMoveActions.sqf` - Permissions + AI fix
- `core/fnc/deploy/refundNearbyAmmoboxes.sqf` - Auto-cleanup menu
- `core/fnc/deploy/refundSingleCrate.sqf` - Logique simplifiée
- `core/fnc/deploy/repairWreckWithResources.sqf` - Menu restructuré
- `core/fnc/deploy/loadDatabase.sqf` - Anti-duplication drapeaux
- `core/fnc/deploy/detectFlags.sqf` - Protection race conditions
- `core/fnc/deploy/activateCheckpointTask.sqf` - Tâches fantômes
- `core/fnc/deploy/monitorCheckpoints.sqf` - Réactivation rapide
- `core/fnc/deploy/syncActionsForNewPlayers.sqf` - Icône fix
- `core/fnc/deploy/createDocumentation.sqf` - Mise à jour complète

**Core Ressources :**
- `core/fnc/ressources/init.sqf` - Retrait initDisplaySystem serveur
- `core/fnc/ressources/initClient.sqf` - Méthode unique + debug
- `core/fnc/ressources/initDisplaySystem.sqf` - missionNamespace
- `core/fnc/ressources/createUI.sqf` - Protection doublon
- `core/fnc/ressources/updateUILoop.sqf` - Protection boucle
- `core/fnc/ressources/showUI.sqf` - missionNamespace
- `core/fnc/ressources/hideUI.sqf` - missionNamespace
- `core/fnc/ressources/captureZone.sqf` - Retrait startGeneration
- `core/fnc/ressources/generationLoop.sqf` - Checks _generating
- `core/fnc/ressources/activateDefenseTask.sqf` - BIS_fnc_taskExists
- `core/fnc/ressources/monitorZonesDefense.sqf` - Réinit flags
- `core/fnc/ressources/startGeneration.sqf` - **SUPPRIMÉ**

**Core Event Handlers :**
- `core/fnc/eh/CuratorObjectPlaced.sqf` - Déduction Zeus
- `core/fnc/log/server_repair_wreck.sqf` - Restauration variables

**Core Compile :**
- `core/fnc/compile.sqf` - Retrait compilation startGeneration

### 📊 Statistiques v0.0.8

- **Fichiers modifiés** : 30+
- **Lignes ajoutées** : 800+
- **Lignes supprimées** : 400+
- **Bugs corrigés** : 15+
- **Nouvelles protections** : 10+

---

## [0.0.7] - 2025-21-09

### ✨ Nouvelles fonctionnalités

#### Système de Déploiement
- Ajout système de déploiement complet
- Fortifications, troupes, véhicules déployables
- Preview 3D avec ajustement position/rotation
- Système de coûts en ressources
- Sauvegarde/chargement automatique

#### Système de Ressources
- 4 types de ressources (Fuel, Munition, Fer, Nourriture)
- Zones de ressources à capturer
- Génération automatique après capture
- Interface UI en temps réel
- Commandes admin console

#### Système de Checkpoints
- Drapeaux déployables
- Zones de protection (500m) et danger (150m)
- Tâches de défense automatiques
- Contre-attaques ennemies
- Marqueurs et triggers dynamiques

#### Garage Virtuel
- Stockage véhicules personnels
- Sauvegarde inventaire complet
- Récupération à tout moment
- Limites par joueur

#### Utilitaires LEON
- Système de drone
- HALO Jump
- Téléportation entre joueurs
- Portage de caisses
- Configuration auto caisses/véhicules

### 🔧 Améliorations
- Intégration complète avec Hearts & Minds
- Système de réparation d'épaves amélioré
- Synchronisation multijoueur
- Persistance complète
- Interface utilisateur intuitive

### 🐛 Bugs connus (corrigés en 0.0.8)
- Interface ressources bloquée en local
- Tâches défense qui ne se valident pas
- Actions ACE perdues après restart
- Checkpoints en double
- Unités qui ne tirent pas
- Menu remboursement qui ne disparaît pas

---

## [0.0.6] et versions antérieures - 2024-12-10

### Développement initial
- Fork de Hearts & Minds
- Setup de base du framework
- Tests initiaux
- Développement des concepts

---

## Légende

- ✅ **Ajout** : Nouvelle fonctionnalité
- 🔧 **Amélioration** : Amélioration de fonctionnalité existante
- 🐛 **Fix** : Correction de bug
- 📝 **Documentation** : Mise à jour documentation
- ⚠️ **Déprécié** : Fonctionnalité dépréciée
- 🗑️ **Supprimé** : Fonctionnalité/fichier supprimé

---

## Support et Contributions

Pour signaler un bug ou suggérer une amélioration :
1. Vérifiez que vous utilisez la dernière version
2. Consultez la documentation complète
3. Contactez [13RDPA] LEON

---

**Projet** : Hearts and Minds ULTIMATE  
**Auteur** : [13RDPA] LEON  
**Dernière mise à jour** : 2025-10-10

