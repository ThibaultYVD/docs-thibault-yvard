---
title: Apache
description: 
published: true
date: 2022-10-05T18:15:58.512Z
tags: 
editor: markdown
dateCreated: 2022-09-28T15:13:51.857Z
---

# Qu'est-ce qu'Apache
Apache est un **serveur web httpd**. Un serveur web **répond et renvoie les pages HTML** au navigateur lorsqu'on consulte un site Internet.

# Installation
Installer le paquet Apache 
```
sudo apt-get install apache2
```

# Configuration
Les fichiers de configuration se trouve dans le répertoire `/etc/apache2/`
Si on regarde ce que contient ce répertoire, on trouve des choses très intéressantes :
![dirapache.png](/img/dirapache.png)

Ce qui va nous intéresser c'est le fichier **apache2.conf**, le fichier **ports.conf**, et les répertoires **sites-available** et **sites-enabled**

# {.tabset}
## Fichier apache2.conf
Comme c'est souvent le cas dans les fichiers de configuration, ce fichier est beaucoup commenté, on peut y retrouver **l'arborescence** du répertoire /etc/apache2/ :

```
/etc/apache2/
|-- apache2.conf
|       `--  ports.conf
|-- mods-enabled
|       |-- *.load
|       `-- *.conf
|-- conf-enabled
|       `-- *.conf
`-- sites-enabled
        `-- *.conf
```
---
Des paramètres comme **l'utilisateur et le groupe** sous lesquels **tourne Apache** :

```
# These need to be set in /etc/apache2/envvars
User ${APACHE_RUN_USER}
Group ${APACHE_RUN_GROUP}
```
> Comme indiqué en commentaire, ce sont des valeurs à modifier dans /envvars/
{.is-info}
---
Ou celui des **logs**

```
LogFormat "%v:%p %h %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" vhost_combined
LogFormat "%h %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" combined
LogFormat "%h %l %u %t \"%r\" %>s %O" common
LogFormat "%{Referer}i -> %U" referer
LogFormat "%{User-agent}i" agent
```

## Fichier ports.conf
Le fichier contient **les ports** qu'Apache **va écouter** pour tourner, de base, on peut trouver l'instruction qui indique **qu'Apache écoute le port 80**.
```
Listen 80
```
Le port **80** est **le port par défaut pour HTTP**.

## Dossier sites-available
C'est le dossier où sera stocké la configuration de votre site. Si vous regardez ce que contient ce répertoire, il existe déjà un fichier nommé `000-default.conf`. Ce fichier n'aura pas grand intérêt lorsque qu'on va créer notre propre fichier de configuration.

## Dossier sites-enabled
Ce répertoire va stocker les configurations actives, **ne pas toucher aux fichiers présents ici**. 
Pour activer une configuration, il faudra créer un **lien symbolique** entre votre fichier de configuration présent dans `/sites-available/` et ce répertoire. Quand vous aller démarrer  le service Apache, cela va **automatiquement générer le fichier de configuration** dans `/sites-enabled/` à partir du fichier dans `/sites-available/`.

# Configurer un site web avec Apache
Cela commence par la création d'un fichier, `exemple.fr.conf` (remplacez le nom du fichier avec le **nom de domaine** de votre site par exemple, en ajoutant bien **l'extension .conf**) dans le répertoire `sites-available/`. Ouvrez le puis ajoutez ceci :
```
<VirtualHost *:80>
    ServerName www.exemple.fr
    ServerAlias exemple.fr
    ServerAdmin webmaster@exemple.com
    DocumentRoot /var/www/html/www.exemple.com

    <Directory /var/www/html/www.exemple.com>
        Options All
        AllowOverride None
    </Directory>
</VirtualHost>
```
## Configuration minimal
## {.tabset}
### Balises VirtualHost
Un Hôte Virtuel (Virtual Host) **fait référence à 1 site web**. 
La partie `*:80` indique que le virtual host peut être utilisé sur le **port 80** sur **toutes les interfaces** .

### ServerName, ServerAlias et ServerAdmin
- Le `ServerName` précise **l’adresse** (l'hôte) pour laquelle cette configuration sera utilisée. Il ne peut y avoir **qu’un seul** `ServerName` par VirtualHost.

- `ServerAlias`. Il ne peut y avoir qu'un seul `ServerName` mais vous pouvez indiquer **autant de `ServerAlias` que vous voulez**.

- Le `ServerAdmin` sert à indiquer une **adresse mail de contact** qui sera **affichée** sur certains **messages d'erreur**.

### DocumentRoot
Indique **la racine** de l’arborescence **de votre site web**, là où vous **mettrez tous vos fichiers**.






## La balise Directory

> On pourrait s'arrêter ici pour une **configuration minimal et fonctionnel**, mais nous pouvons ajouter encore quelques directives.
{.is-info}

La balise Directory sert à définir des règles qui s’appliquent uniquement au contenu d’un répertoire, en l'occurrence, le répertoire `/var/www/html/www.exemple.com`.

### Les règles actives
### {.tabset}
#### Options All
...
#### AllowOverride None
Cette directive indique que la configuration contenu dans un **fichier .htaccess n'a pas la priorité** sur celles du **Virtual Host**. 
A l'inverse, on pourrait utiliser  **All**  pour que le fichier **.htaccess ai la priorité** sur le Virtual Host.
Sinon il est possible de On pourrait préciser **une à une** les options qui peuvent être réécrites.

## Mettre en ligne le site
Pour pouvoir tester la configuration, on peut déjà mettre un **premier fichier** .html dans le **répertoire** que nous avons **indiqué**.


> Si le répertoire en question n'existe pas, vous devez **le créer avant** :
{.is-info}
```
mkdir /var/www/html/www.exemple.com/
```

Placer un fichier .html :
```
cp /var/www/html/index.html /var/www/html/www.exemple.com
```

Après ça, il faut **activer** la configuration du VirtualHost
```
a2ensite exemple.fr
```
> Cette commande créé aussi le **lien symbolique** entre le fichier de configuration présent dans `/sites-available/` et le répertoire `/sites-enable/` comme dit plus tôt.
{.is-info}

Il suffit maintenant de **recharger la configuration d’Apache** pour prendre en compte les modifications.
```
systemctl reload apache2
```

> Et voilà ! Votre serveur est prêt, il ne reste plus qu'à visiter votre site web pour voir la page d'accueil d'Apache.![apache-default-page.jpg](/img/apache-default-page.jpg)
{.is-success}

