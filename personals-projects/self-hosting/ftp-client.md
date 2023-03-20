---
title: Client FTP
description: 
published: true
date: 2022-09-27T21:36:43.980Z
tags: 
editor: undefined
dateCreated: 2022-09-27T19:42:12.557Z
---

# Les protocole FTP et SFTP

Le **File Transfert Protocole** (FTP) est un protocole servant, comme son nom l'indique, au **transfert de fichier**. Le protocole fonctionne avec le modèle **client-serveur**. Pour pouvoir faire ces transferts nous pouvons utiliser des clients prévus à cet effet.

Le protocole **SFTP** fonctionne de la même manière, à la seule différence où **le SFTP effectue les transferts par SSH**.


# Exemples de clients FTP
# {.tabset}
## FileZilla

[FileZilla](https://filezilla-project.org/) est sans doute le client le plus utilisé, il est assez intuitif à utiliser.

Son interface se présente comme cela :
![filezilla.png](/img/filezilla.png)

> Pour **se connecter au serveur**, il suffit de remplir **l'hôte avec l'adresse IP** du serveur, le **nom d'un utilisateur et son mot de passe** puis entrer le **port 22**.
{.is-info}

> La partie **site local** liste les **fichiers présents sur votre machine** et **site distant** les **fichiers de votre serveur**.
{.is-info}

---
[*Télécharger FileZilla*](https://filezilla-project.org/)
## WinSCP

[WinSCP](https://winscp.net/eng/index.php) est un autre client qui est beaucoup utilisé, son interface quand à lui est un peu moins intuitif mais son fonctionnement reste le même.

![winscp.png](/img/winscp.png)

---
[*Télécharger WinSCP*](https://winscp.net/eng/index.php)