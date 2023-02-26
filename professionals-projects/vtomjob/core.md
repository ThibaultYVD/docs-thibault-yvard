---
title: Coeur du programme
description: 
published: true
date: 
tags: 
editor: undefined
dateCreated:
---


## Besoins

La base du programme est la suivante : copier un **fichier d'un répertoire source dans un ou plusieurs répertoire(s) de destination(s)** tout en faisant des traitements sur le fichier. Cela est appelé un **Job**.

Sauf que ce n'est pas qu'un seul fichier qui est à gérer, mais **plusieurs centaines**. Vient donc l'utilité de la **base de données**, qui sert à traiter tout les jobs facilement.

## Finalité du programme

Le programme final est utilisé via une **tâche planifiée** Windows :

Cette tâche est lancée à **intervalle régulier** et démarre le programme.

### Pourquoi utiliser une tâche planifiée ?

- La tâche planifiée fonctionne **automatiquement** en tâche de fond.
- La plupart des exécutables du service informatique est géré via les tâches planifiées.

## Attendues du traitement d'un Job

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

## Diagramme des flux

*(ourvir l'image dans le navigateur pour une meilleure qualitée)*
![diagramme flux.png](/img/vtomjob/Diagramme%20de%20flux.png)

## Les tâches asynchrones

A cause du **volume** de certains fichier, on ne peut pas se permettre de lancer les copies les unes après les autres. C'était le cas dans la version d'origine de ce programme et cela causait toute sortes de problèmes.

C'est pourquoi j'ai utilisé la **programmation asynchrone**, puis les tâches asynchrones :

La **programmation asynchrone** permet de lancer **plusieurs opérations en même temps**. Dans le cas d'une copie, la **programmation synchrone** va copier un fichier et attendre que la copie soit terminée pour lancer la suivante. A l'inverse, **la programmation asynchrone** va **démarer** les copies en même temps.

Cependant, la tâche de copier en elle même reste synchrone car les copies sont géré via une seule **tâche**. Donc même si toutes les copies ont été lancé, elles se feront une à une.

Vient l'utilité des **[Tasks](https://learn.microsoft.com/fr-fr/dotnet/api/system.threading.tasks.task?view=net-7.0)**, je m'en sers pour créer une **liste de tâche** : il suffit de créer et lancer **une tâche par fichier !** Toutes les copies vont donc se faire en même temps, ce qui est un **gain de temps** considérable.

## Diagramme des classes

![Diagramme de classe(core).png](/img/vtomjob/Diagramme%20de%20classe(core).png)

## Gestion des erreurs

Les erreurs sont gérées via des **try/catch** pour toutes les **méthodes et fonctions du programme**. S'il y a une erreur, cela **génère un historique** et est **stocké** directement **dans la base de données.**

L'historique généré contient :

- **l'horodatage** de l'erreur
- **l'id du Job** et de **la destination** auquel l'erreur est survenu
- Le **message** de l'erreur

## Paramètres de l'application

Pour les besoins du programme cité ci-dessus, il est nécessaire d'avoir certaines données en **paramètre**, j'ai donc utilisé un fichier **App.Config**.

Se retrouve les paramètres pour indiquer :

- Le **dossier de sauvegarde**
- Le **nombre de jours** avant de supprimer les historiques plus vieux que ce paramètre
- Le **nombre de jours** avant de supprimer les fichiers plus vieux que ce paramètre mis en sauvegarde

---

## **[Répos Github](https://github.com/Thibault53/VTOMJOB)**
