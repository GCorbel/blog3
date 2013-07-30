---
layout: post
title: Mon premier Blog avec Jekyll
category: french
comments: true
---

Cette semaine, je me suis décidé à créer un blog. Je souhaite écrire quelques articles pour partager les choses que j'apprend. J'avais donc besoin de quelque chose de simple, que je puisse créer rapidement et qu'il soit facile d'ajouter des articles ou de les éditer.

Choix d'un moteur
-----------------

Tout d'abord, j'ai regardé avec quoi les autres blogs de développeur étaient fait. Beaucoup d'entre eux utilisent WordPress ou ce genre de système. Pour ma part, mon language préféré étant Ruby, j'ai choisi d'utiliser un système Ruby.

J'aurais pu créé une application Rails ou Sinatra mais il existe déjà de très bon outils simplifiant la vie du développeur. 

D'habitude, j'utilise RefineryCMS pour faire des sites. C'est excelent pour la simplicité d'utilisation. Monsieur Tout-le-monde peut l'utiliser sans problème. Cependant, j'aime bien le fait de pouvoir éditer des articles en markdown. WYMEditor est fortement attaché à RefineryCms. Le fait de changer l'éditeur aurait pris plusieurs semaines.


Jekyll
------

Je suis rapidement tombé sur Jekyll. Jekyll est un outil permettant de générer des pages statiques. Son principale avantage vient du fait qu'il est possible de générer des pages avec de simple fichiers texte. La configuration est très rapide. Cet outil est particulièrement orienté pour les développeurs. Je n'imagine pas un utilisateur s'y connaissant peu en informatique créer un fichier texte en markdown, ajouter les options de la page en texte et faire un push sur github. Pour moi, c'est parfait.

Le projet JekyllBootrap offre tout un tas d'outils préinstallé. Il s'agit en fait d'un site utilisant Jekyll que l'on doit cloner et modifier. J'ai choisi de ne pas l'utiliser étant donné que je pense qu'il faut déjà bien connaitre Jekyll avant d'utiliser Jekyll-Bootstrap. Ce n'était pas mon cas. De plus, ce que je souhaitais comme site étant très simple, j'ai préféré partir de 0.

Il existe également le projet OctoPress, similaire à Jekyll-Boostrap mais plus orienté pour les blogs. Des tas d'éléments sont préinstallé. Par exemple, les derniers posts ou les derniers tweets sont disponibles . Quelques minutes suffisent pour créer le blog et l'heberger sur internet. Je ne l'ai pas choisi un peu pour les même raisons que Jekyll-Boostrap. Je crois qu'il est nécessaire de bien comprendre comment fonctionne Jekyll avant d'utiliser un dérivé.

Github-pages
------------
