---
title: Base de données
description: 
published: true
date: 
tags: 
editor: undefined
dateCreated: 
---


La base de données a été au départ réalisé en SQL sur PhpMyAdmin mais un portage de la base de données sur MongoDB est en cours de réflexion car une base de données en NoSql est plus facilement gérable en Node.js qu'en utilisant du SQL.

![MCD](../../img/discord-app/mcd.png)


# Explication des tables
# Tabs {.tabset}
## La table **guilds**
![Guild table](../../img/discord-app/guild-table.png)
Elle correspond aux serveurs Discord. Le terme "Guild" est le terme générique pour désigner les serveurs dans Discord. Je garde l'identifiant du serveur : l'identifiant d'un serveur discord est une suite de 18 chiffres. Je garde aussi le nom du serveur et le nombre total de membres du serveur à des fins de statistiques personnels.

## La table **guild_members**
![Guild members table](../../img/discord-app/guild_members-table.png)
Stocke les utilisateurs présents dans un serveur. Le user_tag est désigne le compte d'un utilisateur et est sous la forme Thibault#0000. Nickname est le pseudo que l'utilisateur peut utiliser sur un serveur. Un utilisateur n'a pas forcément de pseudo personnalisé sur un serveur.

## La table **users**
![Users table](../../img/discord-app/users-table.png)
Elle garde simplement l'identifiant et le tag d'un utilisateur.

## La table **event**
![Event table](../../img/discord-app/event-table.png)
La table la plus importante, c'est ici que seront stockés les événements créés :
  - je garde **l'identifiant du salon** où a été créé l'événement car il sera utilisé pour **supprimer le message** de l'événement dans le code.
  - L'event_id est simplement **l'identifiant du message** envoyé par l'application comportant l'événement.
  - je garde le nom du créateur de l'événement et le nom du serveur où a été créé l'événement pour des fins de statistiques personnels.
  - Un événement comporte un **titre**, une **description**, la date et l'heure de l'événement ainsi que le timestamp en **epoch** (L'epoch représente la date initiale à partir de laquelle est mesuré le temps par les systèmes d'exploitation). Cette donnée est créée par un convertisseur dans le code et est utilisée pour créer un compte à rebours dynamique dans le message de l'événement.
  
## La table **members_event_choice**
![Members event choice](../../img/discord-app/members_event_choices.png)
Elle stocke le choix des utilisateurs à l'inscription d'un événement. Il y a 3 choix possibles : "Participant", "Indécis" et "Réserviste" (sous réserve de). Un utilisateur pouvant s'incrire de différentes façons à d'autres événements, il faut que je lie l'identifiant de l'événement au choix de l'utilisateur.

## La table **events_archives**
![Event archive table](../../img/discord-app/event_archive-table.png)
Elle sert uniquement pour des statistiques personnels. Un événement terminé sera stocké dans cette table.
  
# Autres informations

> La base de données est **entièrement** encodée sous la base __**utf8 mb4**__. Pourquoi ? Car cette base permet de stocker les caractères à **4 octets**. Les émoji font 4 octets et les polices personnalisés le sont aussi, ce qui est indispensable pour le bon fonctionnement de l'application.
{.is-info}

