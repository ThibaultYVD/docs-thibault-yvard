# VTOMJOB

# Présentation du projet

# Situation existente

Pour les besoins de l’entreprise, des données issues de l’ERP sont copiées dans des dossiers à intervalle régulier. (Jours, mois, heure…) Ces fichiers sont ensuite recopiés dans d’autres dossiers pour être mis à disposition des utilisateurs ou des systèmes. Lors de la copie ils peuvent être renommés, changés d’extension et/ou être suffixés d’une date ou dateheure.

# Répartition des tâches

J'ai commencé par organiser mon travail en étudiant les attentes du cahier des charges, j'y ai dégagé 4 grandes étapes :

- La création de la **base de données**
- la création du **cœur du programme** du scheduler
- la création d'une **bibliothèque de classes** servant à interroger la base de données
- et la création de **l'IHM** afin de contrôler le programme.

# Organisation

J'ai effectué mon travail en **autonomie**, j'ai fais du versioning de mon programme en local. L'utilisation d'un système de versioning comme Git était impossible dû à un incident dans l'entreprise qui a engendré la coupure de tout accès à internet pendant une bonne partie du stage. Cependant, j'ai mis le programme final sur **[GitHub](https://github.com/Thibault53/VTOMJOB)**. J'ai fais un **suivi de mes activités** chaque jours afin de faire le point à la fin de la journée sur ce qui a été fait et de décider sur quoi je vais travailler le lendemain.

# Architecture logiciel

[![architecture vtomjob.png](https://thibault-yvard.fr/docs/uploads/images/gallery/2023-02/scaled-1680-/huFykeib94v3P52W-architecture-vtomjob.png)](https://thibault-yvard.fr/docs/uploads/images/gallery/2023-02/huFykeib94v3P52W-architecture-vtomjob.png)

# Base de données

# Les besoins

La base de données doit pouvoir subvenir a des besoins précis :

- stocker les paramètres des Jobs
- stocker les paramètres des destinations
- et stocker l'historique du programme

# Diagramme MCD[![MCD BDD.png](https://thibault-yvard.fr/docs/uploads/images/gallery/2023-02/scaled-1680-/5LaiqCnPOcEPCPYi-mcd-bdd.png)](https://thibault-yvard.fr/docs/uploads/images/gallery/2023-02/5LaiqCnPOcEPCPYi-mcd-bdd.png)

# Sauvegardes

La base de données a subit plusieurs modifications suivant les **nouveaux besoins**.

J'ai donc fais **plusieurs sauvegardes** des commandes SQL pour la création de la base de données en suivant les modifications.

---

##### **[Répos Github](https://github.com/Thibault53/VTOMJOB)**

# Coeur du programme

# Besoins

La base du programme est la suivante : copier un **fichier d'un répertoire source dans un ou plusieurs répertoire(s) de destination(s)** tout en faisant des traitements sur le fichier. Cela est appelé un **Job**.

Sauf que ce n'est pas qu'un seul fichier qui est à gérer, mais **plusieurs centaines**. Vient donc l'utilité de la **base de données**, qui sert à traiter tout les jobs facilement.

# Finalité du programme

Le programme final est utilisé via une **tâche planifiée** Windows :

Cette tâche est lancée à **intervalle régulier** et démarre le programme.

### Pourquoi utiliser une tâche planifiée ?

- La tâche planifiée fonctionne **automatiquement** en tâche de fond.
- La plupart des exécutables du service informatique est géré via les tâches planifiées.

# Attendues du traitement d'un Job

Conditions pour qu'un Job soit traité :

- L'état du Job est **"en attente" ou l'état et le statut du Job est "échec" et "actif"**
- Son **fichier source** est **disponible**

Si un des deux cas **n'est pas valide**, le Job n'est **pas traité.**

Ensuite, suivant **les paramètres** du Job dans **la base de données**, on peut vouloir :

- **Remplacer** les retours à ligne présent dans le document en retour charriot avance ligne (remplacer les strings "\\n" par "\\r\\n")
- **Incrémenter** le nom du fichier par **la date et/ou heure** au moment du traitement
- **Changer les droits d'accès** du fichier

Un Job sera mis comme **succès** si **toutes** les actions sont été bien réalisés.

Sinon, s'il y a **la moindre erreur**, le Job sera mis en **échec** et créera un **historique** de l'erreur.

Si le Job est en **succès**, le fichier source sera déplacé dans un dossier de **sauvegarde paramétrable**

A la fin du programme, **l'historique** vieux de plus de *x* jours (x en paramètre) et les **fichiers mis en backup** vieux de *y* jours (y en paramètre) seront **supprimés**.

# Diagramme des flux

*(ourvir l'image dans le navigateur pour une meilleure qualitée)*

# Les tâches asynchrones

A cause du **volume** de certains fichier, on ne peut pas se permettre de lancer les copies les unes après les autres. C'était le cas dans la version d'origine de ce programme et cela causait toute sortes de problèmes.

C'est pourquoi j'ai utilisé la **programmation asynchrone**, puis les tâches asynchrones :

La **programmation asynchrone** permet de lancer **plusieurs opérations en même temps**. Dans le cas d'une copie, la **programmation synchrone** va copier un fichier et attendre que la copie soit terminée pour lancer la suivante. A l'inverse, **la programmation asynchrone** va **démarer** les copies en même temps.

Cependant, la tâche de copier en elle même reste synchrone car les copies sont géré via une seule **tâche**. Donc même si toutes les copies ont été lancé, elles se feront une à une.

Vient l'utilité des **[Tasks](https://learn.microsoft.com/fr-fr/dotnet/api/system.threading.tasks.task?view=net-7.0)**, je m'en sers pour créer une **liste de tâche** : il suffit de créer et lancer **une tâche par fichier !** Toutes les copies vont donc se faire en même temps, ce qui est un **gain de temps** considérable.

# Diagramme des classes

[![Diagramme de classe(core).png](https://thibault-yvard.fr/docs/uploads/images/gallery/2023-02/scaled-1680-/cHEG8TQUSmxeDGa2-diagramme-de-classecore.png)](https://thibault-yvard.fr/docs/uploads/images/gallery/2023-02/cHEG8TQUSmxeDGa2-diagramme-de-classecore.png)

# Gestion des erreurs

Les erreurs sont gérées via des **try/catch** pour toutes les **méthodes et fonctions du programme**. S'il y a une erreur, cela **génère un historique** et est **stocké** directement **dans la base de données.**

L'historique généré contient :

- **l'horodatage** de l'erreur
- **l'id du Job** et de **la destination** auquel l'erreur est survenu
- Le **message** de l'erreur

# Paramètres de l'application

Pour les besoins du programme cité ci-dessus, il est nécessaire d'avoir certaines données en **paramètre**, j'ai donc utilisé un fichier **App.Config**.

Se retrouve les paramètres pour indiquer :

- Le **dossier de sauvegarde**
- Le **nombre de jours** avant de supprimer les historiques plus vieux que ce paramètre
- Le **nombre de jours** avant de supprimer les fichiers plus vieux que ce paramètre mis en sauvegarde

---

##### **[Répos Github](https://github.com/Thibault53/VTOMJOB)**

# Bibliothèque de classe

# Son utilité

Pour avoir mes fonctions CRUD utilisable pour le programme et l'IHM, j'ai opté pour l'utilisation d'une **bibliothèque de classes** (ou Library).

Cette bibliothèque de classes contient donc toute les fonctions pour **insérer** des données, les **supprimer**, les **sélectionner** ou les **mettre à jour**.

**L'avantage** d'une bibliothèque de classe est qu'elle peut être **utilisée partout**. Si par exemple on souhaite changer d'interface, le cœur du programme sera toujours fonctionnel vu qu'il suffit **d'appeler** les fonctions dans la bibliothèque de classe. Cela permet de ne **pas redévelopper ces fonctions** et donc de gagner un temps considérable.

# Diagramme des classes

[![Diagramme de classe (library).png](https://thibault-yvard.fr/docs/uploads/images/gallery/2023-02/scaled-1680-/cAhp5PpL8zTkAW3l-diagramme-de-classe-library.png)](https://thibault-yvard.fr/docs/uploads/images/gallery/2023-02/cAhp5PpL8zTkAW3l-diagramme-de-classe-library.png)

---

##### **[Répos Github](https://github.com/Thibault53/VTOMJOB)**

# IHM

# Besoins

Pour les besoins de l'entreprise, l'IHM est réalisé sur **WINDEV 24.**

L'IHM sert à :

- **Lister** tout les Jobs
- **Supprimer**, **modifier** ou **ajouter** un Job
- **Lister** l'historique

# Page Liste des Jobs

[![IHM-listejobs.png](https://thibault-yvard.fr/docs/uploads/images/gallery/2023-02/scaled-1680-/BEyNNSNIXCh05iGi-ihm-listejobs.png)](https://thibault-yvard.fr/docs/uploads/images/gallery/2023-02/BEyNNSNIXCh05iGi-ihm-listejobs.png)

# Page Gestion Job

[![IHM-gestionjob.png](https://thibault-yvard.fr/docs/uploads/images/gallery/2023-02/scaled-1680-/g8Q1Sx6l07Qz1lNM-ihm-gestionjob.png)](https://thibault-yvard.fr/docs/uploads/images/gallery/2023-02/g8Q1Sx6l07Qz1lNM-ihm-gestionjob.png)

# Page Liste des logs

[![IHM-listelogs.png](https://thibault-yvard.fr/docs/uploads/images/gallery/2023-02/scaled-1680-/PhITVeO3geYo6Tl6-ihm-listelogs.png)](https://thibault-yvard.fr/docs/uploads/images/gallery/2023-02/PhITVeO3geYo6Tl6-ihm-listelogs.png)

---

##### **[Répos Github](https://github.com/Thibault53/VTOMJOB)**

# Retour d'expérience

Ce stage était très instructif, beaucoup de thème a été abordé pour la réalisation de ce projet; notament **l'encodage**, **les tâches** et plus encore.

L'utilisation d'une bibliothèque de classe était **très intéressante** et je n'hésiterais pas à m'en servir dans un autre projet si besoin.

C'était aussi la première fois que j'utilisais Windev, cela **ne m'a pas vraiment plu**. Windev à la réputation **d'être efficace** en simplifiant beaucoup de choses, mais malheureusement, cette simplification **ne rend pas Windev instructif et intéressant**.

Pour conclure, ce stage était très enrichissant, j'ai pu apprendre de nouvelles notions en développement, consolider mes connaissances et surtout, un point très personnel, celui de me donner une preuve que je suis capable d'aller jusqu'au bout d'un projet.

---

Merci de m'avoir lu

Thibault Yvard