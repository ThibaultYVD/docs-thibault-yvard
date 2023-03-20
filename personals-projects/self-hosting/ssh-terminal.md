---
title: Terminal SSH
description: 
published: true
date: 2022-09-28T08:26:30.977Z
tags: 
editor: undefined
dateCreated: 2022-09-27T13:31:46.271Z
---

# Terminal SSH
Le terminal SSH est une application utilisée pour se connecter à un ordinateur distant, dans mon cas, à mon VPS. Il utilise un protocole shell sécurisé pour fournir cette fonctionnalité.



# Exemples d'applications

# {.tabset}
## Console Windows/Powershell
Et oui, il est possible de se connecter en SSH directement via la **console** déjà installée sur Windows avec la commande
```
ssh <username>@<ip-address>
```
Exemple :
```
ssh root@41.243.38.203
```
Il suffira juste de remplir le **mot de passe de l'utilisateur** que vous aurez indiqué.
## PuTTy
[PuTTy](https://www.putty.org/) est sans doute le **terminal SSH** le plus connu, son utilisation est simple :

> L'interface de PuTTy se montre comme cela
![putty_main_page.png](/img/putty_main_page.png)
Il suffit ensuite d'entrer le nom d'hôte ou l'adresse IP de l'ordinateur distant puis cliquer sur Open.
{.is-info}

Et enfin, entrer **login/mot de passe** d'un utilisateur de votre serveur et on est connecté !

![putty_login.png](/img/putty_login.png)

---
[Télécharger PuTTy](https://www.putty.org/)

## Cmder
[Cmder](https://cmder.app/) est une **console** personnalisée, **open-source**, **permettant plus d'action** dans la console que celle de Windows ou même le terminal PuTTy. Je m'en sers pour accéder à mon VPS en SSH.

Cmder permet une **bonne personnalisation de l'interface** de la console et permet des actions supplémentaires comme **ouvrir plusieurs consoles en même temps** ou **copier/coller à l'intérieur du terminal**, ce qui est pratique lorsqu'il faut insérer une longue ligne.

[Télécharger Cmder](https://cmder.app/)