# Hearts and Minds ULTIMATE - Guide du Joueur

![Version](https://img.shields.io/badge/Version-0.1.7-blue)
![Statut](https://img.shields.io/badge/Statut-Production-green)

**Auteur**: [13RDPA] LEON  
**Basé sur**: Hearts & Minds 2.1.4 par Vdauphin (BTC_clan)

---

## Introduction

Hearts and Minds ULTIMATE est une mission persistante et cooperative pour Arma 3. Votre objectif est de libérer la carte en conquérant des villes, en détruisant des caches d'armes ennemies, et en gagnant le soutien de la population locale. Cette version ULTIMATE ajoute de nombreux systèmes personnalisés pour enrichir le gameplay et offrir une experience tactique approfondie.

La mission sauvegarde automatiquement votre progression. Vous pouvez partir et revenir plus tard pour continuer là où vous vous êtes arrêté.

---

## Prérequis et Installation

### Mods Requis

Cette mission nécessite les mods suivants pour fonctionner correctement:

- **CBA_A3** (Community Base Addons)
- **ACE3** (Advanced Combat Environment)
- Un pack de mods recommandé est disponible dans le dossier de la mission

### Comment Rejoindre

1. Assurez-vous d'avoir tous les mods requis installés et activés
2. Connectez-vous au serveur via le navigateur multijoueur d'Arma 3
3. Sélectionnez votre slot de départ dans l'écran de briefing
4. La mission charge automatiquement la progression sauvegardée

---

## Comment Démarrer

### Premiers Pas

1. **Choisissez votre rôle**: Au début de la mission, sélectionnez un slot correspondant à votre rôle préféré (Rifleman, Medic, Engineer, etc.)

2. **Familiarisez-vous avec la base**: Vous commencez dans une base d'opérations (FOB). Explorez-la pour trouver:
   - L'arsenal pour personnaliser votre équipement
   - Les véhicules disponibles
   - La caisse de déploiement pour créer des checkpoints et défenses

3. **Consultez la carte**: Ouvrez votre carte pour voir:
   - Les villes à libérer (marqueurs rouges = occupées, verts = libérées)
   - Les caches d'armes à détruire (marqueurs avec icône de cache)
   - Votre position et celle de votre équipe

4. **Planifiez vos actions**: Coordonnez avec votre équipe pour décider quelle zone attaquer en premier

### Actions de Base

**Actions sur les Objets Déployés** (Molette de la souris):
- Toutes les actions sur les objets que vous déployez (checkpoints, défenses, etc.) se font via la **molette de la souris** (menu d'action)
- Approchez-vous d'un objet déployé et faites défiler la molette pour voir les actions disponibles

**Actions Système** (ACE Menu):
- Les actions systèmes comme gérer les missions, sauvegarder, lancer une contre-attaque manuelle, ou afficher les ressources se font via le **menu ACE** (par défaut: touche Windows ou interaction ACE)
- Ces actions sont généralement disponibles sur vous-même ou sur des objets spécifiques

---

## Systèmes de la Mission

### Systèmes Personnalisés ULTIMATE

#### Système de Déploiement
Le système de déploiement vous permet de créer des structures tactiques sur le terrain en utilisant une caisse de déploiement. Vous pouvez déployer des checkpoints, des défenses (bunkers, nids de mitrailleuses, tourelles), des objets utilitaires (hélipad, barricades), des arsenaux mobiles, des points de téléportation, des FOB complètes, et des points de HALO Jump. Chaque objet coûte des ressources à déployer. Utilisez la molette de la souris sur la caisse pour ouvrir le menu de déploiement et sélectionnez la catégorie d'objet souhaitée.

**Configuration du Cargo Personnalisé** (Pour les administrateurs/moddeurs):
Le système de cargo ACE est automatiquement configuré pour tous les véhicules (créés en éditeur, via Zeus, ou déployés). Par défaut, les véhicules reçoivent une capacité cargo selon leur type (camions: 80 unités, hélicoptères: 50-80 unités, avions: 30-100 unités, bateaux: 70-100 unités, etc.).

Pour ajouter des classes de véhicules personnalisées avec des valeurs cargo spécifiques, modifiez le fichier `core\fnc\deploy\setupCargo.sqf` et ajoutez vos classes dans la liste `btc_deploy_customCargoClasses`:

```sqf
btc_deploy_customCargoClasses = [
    ["B_Heli_Transport_03_F", 100],        // Hélicoptère de transport lourd
    ["O_Truck_03_transport_F", 90],        // Camion de transport
    ["I_Heli_light_03_F", 50],             // Hélicoptère léger
    // Ajoutez vos classes personnalisées ici
];
```

Les classes personnalisées ont la priorité sur les règles génériques. Le format est: `["ClassName", cargoSpace]` où `ClassName` est le nom de classe exact du véhicule et `cargoSpace` est la capacité cargo en unités.

#### Système de Ressources
La mission utilise un système économique basé sur 5 types de ressources: Fer, Munitions, Nourriture, Fuel, et Argent. Vous gagnez des ressources en capturant et en contrôlant des zones de ressources marquées sur la carte. Chaque zone génère un type de ressource spécifique au fil du temps. Les ressources sont partagées entre tous les joueurs et sont consommées lors du déploiement d'objets ou de l'achat d'équipements. Vous pouvez échanger des ressources entre elles via un menu dédié. Affichez vos ressources actuelles via le menu ACE.

#### Système d'Utilitaires
Ce système regroupe plusieurs fonctionnalités pratiques: le drone de surveillance (déployez un drone pour observer une zone), le portage de caisses (transportez des caisses lourdes sur votre dos), la gestion de cargo ACE améliorée (chargez des objets déployés dans les véhicules), et la gestion des inventaires de véhicules et caisses. Le système de cargo est persistant, ce qui signifie que le contenu des véhicules est sauvegardé automatiquement. Utilisez les actions molette sur les objets pour accéder à ces fonctionnalités.

#### Garage Virtuel
Le garage virtuel vous permet de stocker et de récupérer des véhicules. Au lieu de laisser les véhicules éparpillés sur la carte, vous pouvez les stocker virtuellement dans une FOB ou un checkpoint. Pour stocker un véhicule, utilisez l'action molette sur un point de garage (marqué dans les FOB). Pour récupérer un véhicule stocké, utilisez le même menu et sélectionnez le véhicule souhaité dans la liste. Les véhicules stockés conservent leur inventaire et leurs dégâts.

---

### Systèmes de Base Hearts & Minds

#### Arsenal
Le système d'arsenal vous permet de personnaliser votre équipement et vos armes. Vous pouvez accéder à l'arsenal dans les FOB, aux points de respawn, ou en déployant un arsenal mobile. L'arsenal peut être restreint selon les paramètres de la mission (arsenal complet, arsenal limité, ou arsenal ACE avec restrictions). Votre loadout peut être sauvegardé et rechargé automatiquement au respawn si activé dans les paramètres.

#### Gestion des Corps
Lorsqu'un joueur meurt, son corps reste sur le terrain. Vous pouvez transporter les corps de vos coéquipiers pour les ramener à une zone sécurisée. Après un certain temps (configurable), un marqueur apparaît sur la carte pour indiquer la position du corps. Ce système encourage le travail d'équipe et l'évacuation des blessés. Les corps peuvent aussi être fouillés pour récupérer l'équipement.

#### Caches d'Armes
Les caches d'armes sont des objectifs principaux de la mission. Elles sont dispersées sur la carte dans des bâtiments ou des zones cachées. Vous devez les trouver et les détruire en plaçant des explosifs dessus. Obtenir des informations sur leur emplacement nécessite d'interroger des civils ou de fouiller des bâtiments. Détruire toutes les caches est l'un des objectifs de victoire de la mission.

#### Système Chimique
Dans certaines zones, l'ennemi peut utiliser des armes chimiques. Ces zones sont dangereuses et nécessitent un équipement CBRN (combinaison et masque à gaz) pour y survivre. Le système chimique ajoute un niveau de difficulté supplémentaire et force les joueurs à s'équiper correctement avant d'entrer dans ces zones. Les caches peuvent aussi être chimiques selon les paramètres.

#### Villes et Réputation
Chaque ville sur la carte a un statut: occupée (ennemie), contestée, ou libérée (alliée). Pour libérer une ville, vous devez éliminer les forces ennemies présentes et maintenir la zone sécurisée. La réputation avec les civils influence leur comportement: une bonne réputation rend les civils coopératifs et ils peuvent vous donner des informations. Une mauvaise réputation (causée par des pertes civiles) rend la population hostile et attire plus d'ennemis.

#### Civils
Les civils sont présents dans les villes et sur les routes. Ils peuvent vous fournir des informations sur l'emplacement des caches ou des patrouilles ennemies si vous les interrogez (via ACE Interact). Protégez les civils: les tuer réduit votre réputation et rend la mission plus difficile. Les civils peuvent aussi être des insurgés déguisés, restez vigilants.

#### Fonctions Communes
Ce système regroupe des fonctionnalités utilisées par plusieurs autres systèmes: gestion de spawn d'objets, calculs de distance, vérifications de conditions, gestion de groupes, et autres utilitaires. Il fonctionne en arrière-plan pour assurer le bon déroulement de la mission. En tant que joueur, vous n'interagissez pas directement avec ce système.

#### Sauvegarde et Chargement
La mission sauvegarde automatiquement la progression à intervalles réguliers (configurable). La sauvegarde inclut: les villes libérées, les caches détruites, les ressources, les objets déployés, les véhicules, et l'inventaire des joueurs. Au redémarrage du serveur, la mission charge automatiquement la dernière sauvegarde. Les administrateurs peuvent aussi sauvegarder manuellement via le menu ACE.

#### Protection Auditive
Le système de protection auditive simule les effets du bruit des explosions et des tirs sur l'audition. Après une explosion proche ou des tirs intensifs, votre audition sera temporairement réduite (acouphènes, sons étouffés). Portez des protections auditives pour réduire cet effet. Le système s'intègre avec ACE3 pour un réalisme accru.

#### Debug et Administration
Le système de debug offre des outils pour les administrateurs et les testeurs. Il permet d'afficher des informations de debug sur la carte, de vérifier l'état des systèmes, de générer des rapports, et de corriger des problèmes en cours de partie. Les joueurs normaux n'ont pas accès à ces fonctionnalités. Seuls les administrateurs avec les permissions appropriées peuvent utiliser ces outils.

#### Système de Délai
Ce système gère les temporisations et les cooldowns pour diverses actions. Par exemple, il empêche le spam d'actions, impose des délais de respawn sur certains véhicules, ou limite la fréquence d'apparition des missions secondaires. Il fonctionne en arrière-plan et assure un équilibre du gameplay en évitant les abus.

#### Portes
Le système de portes gère l'ouverture et la fermeture des portes dans les bâtiments. Certaines portes peuvent être verrouillées et nécessitent d'être fracturées (avec des explosifs ou un pied de biche). D'autres peuvent être ouvertes normalement. Ce système ajoute du réalisme et des possibilités tactiques lors des opérations de nettoyage de bâtiments.

#### Gestionnaire d'Événements
Ce système gère les événements globaux de la mission: gestion des morts, des dégâts, de l'utilisation de munitions, de l'entrée/sortie de véhicules, etc. Il est utilisé par de nombreux autres systèmes pour déclencher des actions en réponse à des événements spécifiques. Il fonctionne automatiquement en arrière-plan.

#### Drapeaux
Le système de drapeaux vous permet de capturer des positions stratégiques en y plantant votre drapeau. Capturez un drapeau ennemi en vous en approchant et en utilisant l'action molette. Les drapeaux capturés servent de points de respawn et de ralliement. Défendez vos drapeaux car l'ennemi peut les reprendre.

#### FOB (Forward Operating Base)
Les FOB sont des bases avancées que vous pouvez déployer sur le terrain. Une FOB complète comprend: un point de respawn, un arsenal, un garage virtuel, une zone de réparation, et des défenses. Déployer une FOB coûte beaucoup de ressources mais offre un point d'ancrage stratégique pour vos opérations. Vous pouvez avoir plusieurs FOB actives en même temps.

#### Planques Ennemies
Les planques (hideouts) sont des positions ennemies fortifiées dispersées sur la carte. Elles contiennent des groupes ennemis, des véhicules, et parfois des armes lourdes. Détruire les planques affaiblit l'ennemi et réduit les contre-attaques. Les planques sont marquées sur la carte une fois découvertes. Elles se régénèrent au fil du temps si non détruites.

#### IED (Engins Explosifs Improvisés)
Les IED sont des bombes artisanales placées par l'ennemi sur les routes, dans les villes, ou près des objectifs. Ils sont difficiles à repérer et peuvent détruire des véhicules ou tuer des soldats. Utilisez des détecteurs de mines ou avancez prudemment dans les zones à risque. Les ingénieurs peuvent désamorcer les IED. La fréquence d'apparition des IED dépend des paramètres de la mission.

#### Système d'Information
Ce système gère la collecte et la distribution d'informations. Interrogez des civils, fouillez des bâtiments, ou capturez des prisonniers ennemis pour obtenir des renseignements sur les caches, les patrouilles, ou les planques. Plus vous collectez d'informations, plus il devient facile de trouver vos objectifs. Les informations sont partagées avec toute l'équipe.

#### Intelligence
Le système d'intelligence complète le système d'information en fournissant des données tactiques. Il gère la découverte automatique d'objectifs lorsque vous explorez la carte, l'identification des unités ennemies, et le partage de ces informations entre joueurs. Certains objectifs ne deviennent visibles qu'après avoir collecté suffisamment de renseignements.

#### Héliportage
Le système de héliportage (sling loading) vous permet d'accrocher des objets sous un hélicoptère pour les transporter. Vous pouvez transporter des véhicules légers, des caisses de ravitaillement, ou des objets déployés. Le pilote utilise les actions molette pour accrocher/décrocher la charge. Soyez prudent lors du vol avec une charge car elle affecte la maniabilité de l'hélicoptère.

#### Logistique
Le système logistique gère le ravitaillement en munitions, en fuel, et en réparations. Vous pouvez créer des points de ravitaillement en déployant des caisses ou des véhicules logistiques. Les véhicules peuvent être réparés et ravitaillés depuis ces points. La logistique est essentielle pour maintenir vos forces opérationnelles lors de longues missions.

#### Militaires Ennemis
Ce système gère le spawn et le comportement des forces ennemies. Les ennemis patrouillent dans les villes occupées, défendent les caches et les planques, et lancent des contre-attaques. La densité et la difficulté des ennemis dépendent des paramètres de la mission. Les ennemis utilisent une IA tactique et peuvent appeler des renforts.

#### Patrouilles (Système Amélioré)
Le système de patrouilles a été considérablement amélioré avec deux types distincts :
- **Patrouilles militaires automatiques** : Créées automatiquement via un timer configurable depuis les villes ennemies occupées. Elles sont très agressives (mode combat, vigilance, vitesse maximale) et ciblent d'abord les positions des joueurs, puis tentent de recapturer les villes libérées. Elles peuvent être à pied ou en véhicule selon un pourcentage configurable.
- **Patrouilles civiles** : Créées lors de l'activation des villes, elles circulent uniquement entre les villes libérées par les joueurs pour simuler le trafic civil normal. Elles sont non agressives et ne réagissent pas au combat.

Les patrouilles militaires évitent de spawner dans un rayon de 1000m autour des joueurs pour éviter les confrontations immédiates. Si une patrouille entre en combat avec vous, ses waypoints se mettent à jour automatiquement toutes les 1 minute pour suivre vos mouvements. Les patrouilles en véhicules font sortir leurs unités avant d'arriver à destination (passagers seulement si le véhicule est armé). Le nombre maximum de patrouilles et le pourcentage de véhicules sont configurables dans les paramètres de la mission.

#### Réputation
La réputation mesure votre relation avec la population civile. Une haute réputation facilite la collecte d'informations et réduit l'hostilité. Une basse réputation (causée par des pertes civiles ou des dégâts collatéraux) augmente la résistance ennemie et rend les civils hostiles. Gérez votre réputation en minimisant les pertes civiles et en libérant des villes.

#### Respawn
Le système de respawn gère la réapparition des joueurs après leur mort. Vous pouvez respawn à la base principale, dans les FOB, ou aux drapeaux capturés selon les paramètres. La mission utilise un système de tickets: vous avez un nombre limité de respawns (partagés ou individuels). Gagnez des tickets supplémentaires en libérant des prisonniers ou en accomplissant des objectifs.

#### Missions Secondaires
Les missions secondaires sont des objectifs optionnels qui apparaissent régulièrement sur la carte. Types de missions: sauvetage d'otages, destruction de convois, élimination de HVT (cibles de haute valeur), récupération d'objets, etc. Accomplir des missions secondaires rapporte des récompenses (ressources, tickets de respawn, informations). Vous pouvez configurer les types de missions et les récompenses via un menu dédié.

#### Slots et Rôles
Le système de slots gère la sélection des rôles au début de la mission. Chaque slot correspond à un rôle spécifique: Rifleman, Medic, Engineer, Squad Leader, etc. Certains rôles ont des capacités spéciales (les medics soignent plus efficacement, les engineers peuvent désamorcer les IED). Les slots peuvent être partagés (plusieurs joueurs peuvent prendre le même rôle) selon les paramètres.

#### Spectateur
Le mode spectateur s'active lorsque vous êtes mort et en attente de respawn. Vous pouvez observer les autres joueurs (si autorisé par les paramètres), voir leur perspective, et suivre l'action. Le spectateur aide à comprendre la situation tactique et à planifier vos actions après le respawn. Les options de spectateur sont configurables.

#### Tags et Marqueurs
Le système de tags vous permet de marquer des positions sur la carte pour votre équipe. Placez des marqueurs pour indiquer des positions ennemies, des zones à éviter, des objectifs, ou des points de ralliement. Les marqueurs sont visibles par tous les joueurs. Utilisez-les pour améliorer la communication et la coordination.

#### Tâches et Objectifs
Le système de tâches gère les objectifs principaux et secondaires de la mission. Les tâches principales incluent: libérer toutes les villes, détruire toutes les caches, et améliorer la réputation. Les tâches secondaires sont générées dynamiquement. Consultez le journal des tâches (J par défaut) pour voir vos objectifs actuels et leur progression.

#### Remorquage
Le système de remorquage vous permet d'attacher un véhicule à un autre pour le tracter. Utile pour déplacer des véhicules endommagés ou pour transporter des véhicules légers. Utilisez les actions molette sur un véhicule tracteur pour attacher/détacher un véhicule. Le véhicule remorqué suit automatiquement le tracteur.

#### Véhicules
Le système de véhicules gère le spawn, la persistance, et la gestion des véhicules de la mission. Les véhicules peuvent être déployés, réparés, ravitaillés, et stockés dans le garage virtuel. Leur inventaire et leur état sont sauvegardés. Certains véhicules ont des capacités spéciales (véhicules médicaux, véhicules de réparation, véhicules de ravitaillement).

---

## Objectifs de la Mission

### Objectifs Principaux

1. **Libérer toutes les villes**: Éliminez les forces ennemies dans chaque ville et sécurisez la zone
2. **Détruire toutes les caches d'armes**: Trouvez et détruisez les caches ennemies dispersées sur la carte
3. **Maintenir une bonne réputation**: Évitez les pertes civiles et gagnez le soutien de la population

### Objectifs Secondaires

- Accomplir les missions secondaires qui apparaissent dynamiquement
- Détruire les planques ennemies pour affaiblir la résistance
- Capturer des drapeaux ennemis pour étendre votre contrôle
- Construire des FOB pour établir une présence permanente

### Conditions de Victoire

La mission est gagnée lorsque:
- Toutes les villes sont libérées
- Toutes les caches d'armes sont détruites
- La réputation globale est positive

---

## Conseils et Astuces

### Pour Bien Démarrer

- **Travaillez en équipe**: La coordination est essentielle. Utilisez les radios et communiquez vos intentions
- **Gérez vos ressources**: Ne gaspillez pas les ressources en déployant trop d'objets inutiles
- **Explorez méthodiquement**: Avancez ville par ville plutôt que de vous disperser
- **Protégez les civils**: Chaque civil tué réduit votre réputation et rend la mission plus difficile

### Tactiques Recommandées

- **Reconnaissance d'abord**: Utilisez des drones ou des éclaireurs pour repérer les ennemis avant d'attaquer
- **Établissez des FOB**: Créez des bases avancées près de vos zones d'opération pour réduire les temps de trajet
- **Sécurisez les zones de ressources**: Contrôlez les zones de ressources pour assurer un flux constant d'approvisionnement
- **Interrogez les civils**: Les informations sont précieuses. Parlez aux civils pour trouver les caches plus rapidement

### Erreurs à Éviter

- Ne pas sauvegarder régulièrement (utilisez les sauvegardes manuelles en plus de l'auto-save)
- S'aventurer seul loin de l'équipe (vous serez rapidement submergé)
- Ignorer les patrouilles ennemies (elles peuvent appeler des renforts)
- Négliger la logistique (manquer de munitions en plein combat est dangereux)

---

## Support et Contact

**Discord**: https://discord.gg/aEJ7W6QwYr  
**GitHub**: https://github.com/LeonFAM/Hearts-And-Minds-ULTIMAT  
**Steam Workshop**: https://steamcommunity.com/sharedfiles/filedetails/?id=3580378812

Pour signaler des bugs ou proposer des améliorations, contactez **[13RDPA] LEON** via Discord.

---

## Crédits

**Hearts & Minds 2.1.4** - Vdauphin (BTC_clan) & communauté (APL-SA)  
**Systèmes ULTIMATE** - [13RDPA] LEON  
**Assistance développement** - IA Claude (Anthropic)

---

**Bonne chance, soldats. La population compte sur vous.**

*Dernière mise à jour: Décembre 2025 - Version 0.1.7*

**Documentation complète**: Consultez les fichiers dans `READ_ME/FR/` pour plus d'informations sur la configuration et l'architecture des systèmes.

