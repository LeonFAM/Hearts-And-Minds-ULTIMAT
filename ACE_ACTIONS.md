# 🎯 Guide des Actions ACE - Hearts and Minds ULTIMATE

![Version](https://img.shields.io/badge/Version-0.0.8-blue)
![ACE3](https://img.shields.io/badge/ACE3-Required-orange)

## 📋 Table des matières

- [Introduction](#introduction)
- [Actions sur caisses de déploiement](#actions-sur-caisses-de-déploiement)
- [Actions sur objets déployés](#actions-sur-objets-déployés)
- [Actions sur véhicules](#actions-sur-véhicules)
- [Actions sur checkpoints](#actions-sur-checkpoints)
- [Permissions et Sécurité](#permissions-et-sécurité)

---

## Introduction

Les actions ACE sont le cœur de l'interaction avec les systèmes personnalisés de la mission. Ce guide détaille toutes les actions disponibles, leurs conditions d'affichage et leur fonctionnement.

### Comment accéder aux actions ACE
1. **Molette souris** sur un objet compatible
2. Sélectionnez l'action désirée dans le menu radial
3. Suivez les instructions à l'écran

---

## Actions sur caisses de déploiement

### 🎯 B_supplyCrate_F (Caisse OTAN)

#### **Système de Déploiement**
- **Icône** : 📦 Exit
- **Description** : Ouvre le menu principal de déploiement
- **Conditions** : Toujours visible
- **Fonctionnement** :
  1. Ouvre un sous-menu avec catégories
  2. Choisissez : Fortifications, Troupes, Véhicules, etc.
  3. Sélectionnez l'objet à déployer
  4. Prévisualisez avec molette (position/rotation)
  5. Clic molette pour confirmer

**Catégories disponibles** :
- **Fortifications** : Murs, barrières, sacs de sable
- **Armes statiques** : M2, TOW, MK19, Mortiers
- **Troupes** : Infanterie, snipers, AT, médics
- **Véhicules** : Transport, combat, logistique
- **Caisses** : Munitions, médical, équipement
- **Ravitaillement** : Fuel, ammo, réparation
- **FOB** : Tentes médicales
- **Décontamination** : Douches de décon

---

#### **Rembourser caisse (50%)**
- **Icône** : 📤 Upload
- **Description** : Récupère 50% des ressources dépensées
- **Conditions** : Au moins une caisse remboursable dans un rayon de 10m
- **Fonctionnement** :
  - **Une seule caisse** : Remboursement immédiat
  - **Plusieurs caisses** : Menu de sélection intelligent
    - Affiche le nom et le remboursement estimé
    - Cliquez pour sélectionner
    - Menu disparaît automatiquement après sélection

**Caisses remboursables (6 types)** :
1. `ACE_medicalSupplyCrate_advanced` - Caisse médicale
2. `ACE_Box_Misc` - Caisse divers ACE
3. `Box_NATO_Equip_F` - Caisse équipement NATO
4. `B_CargoNet_01_ammo_F` - Filet cargo munitions
5. `Box_NATO_WpsLaunch_F` - Caisse lanceurs
6. `Box_T_East_Ammo_F` - Caisse réappro véhicules

⚠️ **B_supplyCrate_F** NON REMBOURSABLE (caisse système)

**Exemple de remboursement** :
```
Caisse médicale : Coût 10 Fer + 20 Nourriture
→ Remboursement : +5 Fer + 10 Nourriture (50%)
```

---

#### **Réparation d'épave**
- **Icône** : 🔧 Repair
- **Description** : Répare un véhicule détruit à proximité
- **Conditions** : Une épave dans un rayon de 25m
- **Fonctionnement** :
  1. Action crée un **sous-menu** sur l'épave :
     - **CONFIRMER RÉPARATION** ✅ (icône thumbs up)
     - **ANNULER** ❌ (icône thumbs down)
  2. Affiche le nom du véhicule et le coût
  3. Confirmation → Véhicule réparé avec actions ACE restaurées

**Coûts de réparation** :
- **Véhicule non-armé** : 15 Carburant + 15 Fer
- **Véhicule armé léger** : 25 Carburant + 25 Fer
- **Véhicule armé lourd** : 40 Carburant + 40 Fer

**Actions ACE restaurées automatiquement** :
- Déplacer l'objet
- Supprimer l'objet
- Changer la stance (pour unités)

---

#### **Porter la caisse**
- **Icône** : 🎒 Exit
- **Description** : Permet de porter physiquement la caisse
- **Conditions** : Caisse portable (B_supplyCrate_F, etc.)
- **Fonctionnement** :
  1. Caisse attachée au joueur
  2. Ralentissement de mouvement
  3. **Clic gauche** : Poser la caisse
  4. **Clic droit** : Annuler (lâcher)

---

## Actions sur objets déployés

### 🏗️ Fortifications et objets statiques

#### **Déplacer l'objet**
- **Icône** : 📍 Move
- **Description** : Repositionne l'objet
- **Permissions** : Créateur OU Admin
- **Fonctionnement** :
  1. Objet entre en mode preview
  2. **Molette haut/bas** : Ajuster position
  3. **Molette gauche/droite** : Rotation
  4. **Clic molette** : Confirmer nouvelle position
  5. Position synchronisée avec tous les joueurs

**Contraintes** :
- Distance maximale : 12 mètres
- Hauteur maximale : 12 mètres
- Pas de zones interdites

---

#### **Supprimer l'objet**
- **Icône** : ❌ Exit
- **Description** : Supprime l'objet avec remboursement 100%
- **Permissions** : Créateur OU Admin
- **Fonctionnement** :
  1. Calcul automatique du remboursement
  2. Ajout des ressources (100% du coût)
  3. Suppression de l'objet
  4. Mise à jour des tableaux de suivi

**Remboursement par type** :
- **Fortifications** : 100% du coût initial
- **Armes statiques** : 5 Munition + 5 Fer
- **Troupes** : Coût variable selon le type
- **Véhicules** : Coût variable selon armement

---

### 👤 Unités déployées

#### **Changer la stance**
- **Icône** : 🧍 Position
- **Description** : Change la position de l'unité
- **Permissions** : Créateur OU Admin
- **Options** :
  - **Debout (STAND)** : Position par défaut
  - **Accroupi (CROUCH)** : Position tactique
  - **Couché (PRONE)** : Position défensive

**Fonctionnement** :
1. Sélectionnez la stance désirée
2. L'unité change immédiatement de position
3. Stance sauvegardée (persistante)

---

#### **Porter l'unité**
- **Icône** : 🎒 Carry
- **Description** : Porte l'unité (déplacement rapide)
- **Permissions** : Créateur OU Admin
- **Fonctionnement** :
  1. Unité attachée au joueur
  2. AI désactivée temporairement
  3. **Clic gauche** : Poser l'unité
  4. **Clic droit** : Annuler

**Avantages** :
- Repositionnement rapide
- Utile pour ajustements fins
- Compatible multijoueur

---

## Actions sur véhicules

### 🚗 Véhicules déployés

Toutes les actions des objets déployés +

#### **Monter dans le véhicule**
- Actions ACE standard d'Arma 3
- Positions : Conducteur, Tourelle, Cargo

#### **Actions logistiques** (si configuré)
- Ravitaillement carburant
- Ravitaillement munitions
- Réparation
- Transport de cargaison

---

## Actions sur checkpoints

### 🚩 Drapeaux (Checkpoints)

#### **Informations checkpoint**
- **Icône** : ℹ️ Info
- **Description** : Affiche les détails du checkpoint
- **Permissions** : Tous
- **Informations** :
  - ID du checkpoint
  - Faction propriétaire
  - Zone de protection (500m)
  - Zone de danger (150m)
  - État de la tâche de défense

---

#### **Déplacer/Supprimer** (comme objets déployés)
- **Permissions** : Créateur OU Admin
- ⚠️ Suppression du drapeau → Suppression du checkpoint entier
- Les marqueurs et triggers sont nettoyés automatiquement

---

## Permissions et Sécurité

### 🔐 Système de propriété

Chaque objet déployé possède une variable `btc_deploy_createdBy` contenant le nom du créateur.

#### Niveaux de permissions

**1. Créateur**
- Peut déplacer ses objets
- Peut supprimer ses objets
- Peut modifier la stance de ses unités

**2. Administrateur**
```sqf
serverCommandAvailable "#kick"
```
- Peut gérer TOUS les objets
- Peut forcer la suppression
- Accès aux commandes de debug

**3. Objets "System"**
- Créés après restauration serveur
- `btc_deploy_createdBy = "System"`
- Gérables par TOUS les joueurs

#### Vérification des permissions
Les conditions ACE vérifient automatiquement :
```sqf
(_creator == name _player) ||          // Est le créateur
(serverCommandAvailable "#kick") ||    // Est admin
(_creator == "System") ||              // Objet système
(_creator == "")                       // Pas de créateur défini
```

---

## 🌐 Compatibilité Multijoueur

### Synchronisation des actions

**Toutes les actions sont synchronisées** :
- Déplacement → `remoteExec` sur serveur
- Suppression → `remoteExec` sur serveur
- Remboursement → Calcul serveur + sync clients
- Création → `publicVariable` pour tous

### Join In Progress (JIP)

Les nouveaux joueurs reçoivent automatiquement :
- ✅ Actions ACE sur objets existants (via `syncActionsForNewPlayers`)
- ✅ Permissions correctes (vérification `btc_deploy_createdBy`)
- ✅ Coûts de déploiement (via event handler `PlayerConnected`)

### Protection anti-spam

- Délai entre actions : 0.5 secondes
- Vérification objet valide (`!isNull` + `alive`)
- Nettoyage automatique des actions obsolètes

---

## 🐛 Résolution de problèmes

### Actions ACE manquantes après redémarrage
**✅ Corrigé v0.0.8**
- `syncActionsForNewPlayers` exécuté automatiquement
- Actions restaurées pour tous les objets

### Actions visibles mais inutilisables
**Cause** : Problème de permissions
**Solution** : Vérifier `btc_deploy_createdBy` de l'objet
```sqf
_object getVariable ["btc_deploy_createdBy", ""]
```

### Menu remboursement qui ne disparaît pas
**✅ Corrigé v0.0.8**
- Auto-nettoyage après sélection
- Timeout de 30 secondes

### Icône manquante (croix rouge)
**✅ Corrigé v0.0.8**
- `money_ca.paa` → `upload_ca.paa`
- Toutes les icônes validées

---

## 📊 Liste complète des actions

| Objet | Action | Icône | Permission |
|-------|--------|-------|------------|
| B_supplyCrate_F | Système de Déploiement | 📦 | Tous |
| B_supplyCrate_F | Rembourser caisse (50%) | 📤 | Tous |
| B_supplyCrate_F | Réparation d'épave | 🔧 | Tous |
| B_supplyCrate_F | Porter la caisse | 🎒 | Tous |
| Fortification | Déplacer l'objet | 📍 | Créateur/Admin |
| Fortification | Supprimer l'objet | ❌ | Créateur/Admin |
| Unité | Déplacer l'objet | 📍 | Créateur/Admin |
| Unité | Supprimer l'objet | ❌ | Créateur/Admin |
| Unité | Changer stance | 🧍 | Créateur/Admin |
| Unité | Porter l'unité | 🎒 | Créateur/Admin |
| Véhicule | Déplacer l'objet | 📍 | Créateur/Admin |
| Véhicule | Supprimer l'objet | ❌ | Créateur/Admin |
| Drapeau | Informations checkpoint | ℹ️ | Tous |
| Drapeau | Déplacer l'objet | 📍 | Créateur/Admin |
| Drapeau | Supprimer l'objet | ❌ | Créateur/Admin |
| Épave | Confirmer réparation | ✅ | Tous |
| Épave | Annuler réparation | ❌ | Tous |

---

## 📝 Conseils et Astuces

### Déploiement efficace
1. Prévisualisez toujours avant de confirmer
2. Vérifiez vos ressources disponibles (F2)
3. Utilisez la rotation pour aligner précisément

### Économie de ressources
1. Récupérez 50% en supprimant les caisses inutiles
2. Récupérez 100% en supprimant fortifications/unités
3. Planifiez vos déploiements pour éviter le gaspillage

### Checkpoints stratégiques
1. Placez des drapeaux aux positions clés
2. Défendez la zone de danger (150m) en priorité
3. Utilisez des fortifications autour du drapeau

### Multijoueur
1. Communiquez avec votre équipe avant déploiements majeurs
2. Les admins peuvent nettoyer les objets abandonnés
3. Objets "System" après restart = gérables par tous

---

**Version** : 0.0.8 STABLE  
**Auteur** : [13RDPA] LEON  
**Documentation mise à jour** : Octobre 2025

