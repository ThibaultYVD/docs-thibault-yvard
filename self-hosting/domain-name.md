---
title: Nom de domaine et zone DNS
description: 
published: true
date: 2022-09-28T11:29:27.710Z
tags: 
editor: undefined
dateCreated: 2022-09-28T08:53:48.697Z
---

# Le nom de domaine

Un nom de domaine est **une adresse d'un site internet** qui permet de le retrouver facilement sur les moteurs de recherche. Il va être relié à l'adresse IP de votre serveur. De plus, un nom de domaine représente aussi l'identité du site C'est plus simple de retenir un nom de domaine qu'une adresse IP, c'est pour cela qu'un nom de domaine est primordial pour un site web.

## Pourquoi créer un nom de domaine ?
Le nom de domaine permet de donner une **meilleure visibilité** au site Internet, non seulement parce qu'il permet à l'utilisateur de **mémoriser plus facilement** l'adresse du site en question, mais parce qu'un nom de domaine pertinent est **indexé plus efficacement** par les moteurs de recherches.

## Comment en obtenir un ?
Pour obtenir un nom de domaine, vous devez **l'enregistrer chez un bureau d'enregistrement**. Cela ce fait très facilement : **beaucoup d'hébergeur** vous permette d'enregistrer votre nom de domaine Dans mon cas, j'ai enregistré mon nom de domaine chez OVH.

> La structure d'une adresse Internet![adresse_internet.jpg](/img/adresse_internet.jpg)
{.is-info}

> Pour créer un nom de domaine, il vous faut choisir le **nom** de votre site, mais aussi son **extension** (.fr, .com, .net, ...).
{.is-success}

## Le prix
Le prix d'un nom de domaine **varie** d'un hébergeur à un autre, mais aussi de **l'extension** utilisé. Par exemple, un nom de domaine en **.com** coûte **plus cher** qu'un nom de domaine en **.fr**, car un site en **.com** est plutôt adressé à un public **mondial**.

En ce qui me concerne, le nom de domaine **thibault-yvard.fr** me coûte **7,99€/an**.



# Zone DNS
Après avoir **obtenu un nom de domaine**, il faut maintenant le **configuer** pour **pointer sur l'adresse IP** du serveur. C'est là où intervient la zone DNS, qui va remplir cette fonction.
Le DNS, le service utilisé par une zone DNS va donc servir à **traduire le nom de domaine en adresse IP**.

## La pratique (OVH)
Configurer une zone DNS se fait chez **l'hébergeur** où vous avez acheté votre nom de domaine. En ce qui me concerne, je suis chez OVH. **L'interface peut changer** d'un hébergeur à un autre, mais le **principe est exactement le même**.

Lorsque vous arrivez sur la page de votre zone DNS, il y a déjà certaines choses pré-configuré. Ces configurations servent seulement à indiquer que le nom de domaine est relié au **serveur DNS** de votre hébergeur.
![zonedns.png](/img/zonedns.png)

### Types d'enregistrements
Une zone DNS **est configuré** sous forme de "**type d'enregistrement**", ou "**record**". **Chaque** record rempli une **fonction bien particulière** que je vais expliquer.

# {.tabset}
## Type A
C'est le type le plus important, c'est lui qui va servir à **pointer le nom de domaine à une IP v4**. Il va aussi être utile pour configurer des **sous-domaines**.

*Configuration pour cibler le nom de domaine à l'adresse IP* 
```
thibault-yvard.fr							0			A			62.171.187.32
```

*Configuration pour cibler un **sous-domaine* à l'adresse IP*
```
docs.thibault-yvard.fr							0			A			62.171.187.32
```

## Type AAAA
Même utilité que le type A, mais pour **l'IP v6**.

## Type NS
Relier la **zone DNS** au **serveur de nom**.

## Type MX
Définit le nom du serveur mail du domaine.

