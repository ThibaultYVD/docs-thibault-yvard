---
title: Markdown
description: 
published: true
date: 2022-09-27T21:58:41.665Z
tags: 
editor: undefined
dateCreated: 2022-09-27T21:58:39.220Z
---

# Présentation

Markdown est un langage de balisage léger permettant de simplifier la rédaction d'un texte avec des balises simple à comprendre.

# Mon usage

J'utilise le Markdown au quotidien avec l'utilisation de Discord ou [Notion](../software-and-apps/notion.md).

# La syntaxe Markdown


### Mettre en **gras** et/ou en *italique et/ou barré*


##### Mettre en gras
La mise d'un texte en gras se fait avec l'utilisation de **2 astérisques** (ou "étoiles") avant et après le texte :
```
    # Ce texte n'est pas en gras mais en gras mais **celui-ci l'est !**
```
Ceci donnera donc :
> Ce texte n'est pas en gras mais en gras mais **celui-ci l'est !**


##### Mettre en italique
L'italique lui se fait simplement avec **1 astérisque** (avant et après le texte bien sûr):
```
    # *Un texte en italique !*
```
Donnera ...
> *Un texte en italique !*


##### Cumuler un texte en gras et en italique
Et oui, c'est possible avec l'utilisation de **3 astérisques**

```
    # ***Ce texte est en gras ET en italique***
```
> ***Ce texte est en gras ET en italique***


##### Barrer un texte
Barrer un texte se fait avec **2 tildes**

```
    ~~Voici un texte barré~~
```

> ~~Voici un texte barré~~


###### Cumuler un texte en gras, en italique et barré
Petit exemple de ce qu'on peut faire en combinant les 3

```
    ***~~On combine les 3 !~~***
```

> ***~~On combine les 3 !~~***



### Les titres
Un titre se signale avec des **dièses** (#)
Il existe 6 niveau de titre, en fonction du **nombre** de dièse écrit :

```
#           Titre 1
##          Titre 2
###         Titre 3
####        Titre 4
#####       Titre 5
######      Titre 6
```

> #           Titre 1
##          Titre 2
###         Titre 3
####        Titre 4
#####       Titre 5
######      Titre 6


### Les citations
Une citation peut se faire avec le **signe supérieur à** (>)

```
> HAMLET - Être ou ne pas être : telle est la question.
```

Donnera ...
> HAMLET - Être ou ne pas être : telle est la question.

###### Les plus de Wiki.Js

Wiki.Js ajoute plusieurs variations de ces citations avec les "avertissements", les "succcès" et les "informations"

- Un **avertissment** se fait en ajoutant **{is.warning}** à la fin de la citation :
```
> Attention ! Un chat sauvage apparait !
{is.warning}
```
> Attention ! Un chat sauvage apparait !
{is.warning}


- Un **succès** se fait avec **{is.success}** : 
```
> Félicitation ! Vous avez réussi !
{is.success}
```
> Félicitation ! Vous avez réussi !
{is.success}


- Enfin, une **information** se fait avec **{is.info}**
```
> Vous êtes sur la page du Markdown.
{is.info}
```
> Vous êtes sur la page du Markdown.
{is.info}


### Les listes



###### Les listes simples

Pour faire une liste **simple**, il y a 3 choix : utiliser le **signe plus**, le **tiret** ou un **astérique**

Exemple avec le tiret (celui que j'utilise le plus)
```
- Element 1
- Element 2 
- Element 3
```

Qui donnera ...
- Element 1
- Element 2 
- Element 3


###### Les listes numérotées

Il est aussi possible de faire des listes numérotés : il suffit de mettre un **chiffre suivi d'un point**, peu importe l'ordre du chiffre écrit.
```
1. Element 1
2. Element 2
3. Element 3
```
1. Element 1
2. Element 2
3. Element 3


###### Listes à cocher

Une liste à cocher peut se faire avec des **crochets** puis **laisser un espace** entre les deux pour avoir une case vide. Pour créer une case déjà cochée, il suffit d'ajouter un **x** entre les crochets :

```
[ ] Element 1
[x] Element 2
[ ] Element 3
```

Qui donnera le résultat suivant :
[ ] Element 1
[x] Element 2
[ ] Element 3


### Autres

###### Liste de liens

La liste de lien premet de structurer des liens, cela peut service pour faire des tables des matières :
```
- [Nom lien *C'est une description !*]([lien])
- [Mon site web *C'est un super site !*](https://thibault-yvard.fr)
{.links-list}
```
Le résultat :

- [Nom lien *C'est une description !*]([lien])
- [Mon site web *C'est un super site !*](https://thibault-yvard.fr)
{.links-list}

###### Onglets

```
# Tabs {.tabset}
## First Tab

Any content here will go into the first tab...

## Second Tab

Any content here will go into the second tab...

## Third Tab

Any content here will go into the third tab...
```

# {.tabset}
## Premier onglet

Bienvenue dans le premier onglet !

## Deuxième onglet

Bienvenue dans le deuxième onglet !

## Troisième onglet

Bienvenue dans le troisième onglet !