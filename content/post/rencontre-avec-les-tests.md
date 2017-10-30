---
title: "Rencontre avec les tests"
date: 2017-10-27T13:45:57+02:00
draft: true
tags: ["tests"]
---

Je me souviens du jour où j'ai découvert les tests.  
Le concept m'a d'abord paru étrange.    

> Ecrire du code qui teste le code ?  

Une question stupide m'a traversé l'esprit.

> Oui mais alors, comment tester le code qui teste le code ?  

Puis j'ai été séduit par l'idée.  

Ok, j'en avais compris les intérêts dans les grandes lignes : 

 - s'assurer qu'un code fonctionne tel qu'on s'y attend, 
 - s'assurer qu'un code fonctionne aux bornes du système, 
 - s'assurer qu'un changement n'entraîne pas une régression, 
 - automatiser une tâche répétitive donc chiante, 
 - être beaucoup plus serein pendant les phases de refactoring, 
 - rentrer chez soi avec la satisfaction d'un travail abouti. 

Il ne restait plus qu'à la mettre en pratique. 
Je me suis alors jeté dans le grand bain, sans expérience et sans réel approfondissement du sujet. Erreur, que je paierai rapidement.

![Failed](https://media.giphy.com/media/rjr9etfxrdP3i/giphy.gif "Failed")

J'ai été très vite confronté à des problèmes divers qui ralentissaient considérablement ma capacité à produire le "vrai" code.  
Une phrase revenait souvent dans l'open-space :  

> J'ai pas le temps de faire des tests, on devrait les inclure à nos chiffrages.

Echec total, renoncement, rejet d'un concept qui semblait prometteur.

Certes, les tests prennent du temps, mais, correctement implémentés, en font gagner beaucoup plus.

## Mes erreurs

J'ai pu rencontrer, lors de conférences, des développeurs emballés par les tests.  
Il était évident que j'étais passé à côté de quelque chose.

Ma première erreur était de ne pas avoir choisi le bon type de tests pour commencer.  

L'idée semblait pourtant plutôt bonne. Comme nous avons peu de temps, nous allons traverser la stack de bout en bout pour la tester dans son intégralité.
D'où la création de tests qui lanceraient des requêtes http et feraient des assertions sur le contenu retourné. 
Si le contenu retourné est celui attendu, nous extrapolons le fait que les couches intermédiaire ont correctement fonctionnées.  
Génial !  
Pas tant que ça...

Ce type de test est appelé tests fonctionnels car ils s'emploient à vérifier le bon fonctionnement général d'une application 
tel que la voit un utilisateur. 
Ce sont ce qu'on pourrait appeler des tests de haut niveau.  
D'autre tests de ce type (voir [Selenium](https://fr.wikipedia.org/wiki/Selenium_(informatique))) vont jusqu'à reproduire automatiquement le comportement 
d'un utilisateur directement dans le navigateur.

Premiers points génants :  

- ça ne peut s'appliquer qu'à un environnemnt web, 
- si je veux réutiliser ma couche métier dans un autre projet,  
  je n'ai plus aucun test.

Ensuite, comme nous traversons l'ensemble de la stack, il faut que toutes les couches soient présentes.  
L'environnement de test étant différent des environnements d'exécution "normal" (dev,prod,...), nous devons simuler certaines couches (les mocker).  
Par exemple, dans mon cas, la session utilisateur.

Comme pour tout type de test, pour pouvoir faire des assertions correctes, il va falloir maîtriser le jeu de données initiales.
Or, avec les tests fonctionnels, je n'ai pas d'accès direct à la couche métier, 
je vais être obligé de créer une base données spécifique à l'environnement de test. 

Avant d'avoir commencer, on voit déjà beacoup de pré-requis apparaîtrent.

Nous avons donc mis en place une base de données mémoire ([H2](https://fr.wikipedia.org/wiki/H2_(base_de_donn%C3%A9es))).

Un autre problème est alors très rapidement apparu, les données de test étant centralisées dans une base de données, 
elles devenaient communes à l'ensemble des tests. 
Un test qui modifiait l'état de cette base, impactait directement les tests suivants.

On a donc du mette en place un système réinitialisant le jeu de données à chaque test. C'est bourrin et très inefficace !
Malgré cette réinitialisation systèmatique des données, nos tests étaient toujours très dépendants les uns des autres.
L'insertion de données initiales dans la base impactait directement l'ensemble de nos tests.

Il devenait très compliqué de modifier ou d'ajouter un test.

**L'environnement de tests doit ête simple, l'ajout d'un test doit être une opération triviale et rapide.   
Si ce n'est pas le cas, vous constaterez un désengagement des développeurs à maintenir et à faire évoluer les tests.**

Notre package de tests est devenu peu à peu une ville fantôme où n'erraient plus que quelques âmes en peine.

![Far west](https://media.giphy.com/media/W0KiMlQIj4nzq/giphy.gif "Far west")

Je devais trouver un socle de test plus simple.

Est-ce que les tests fonctionnels sont une mauvaise chose ?  
Je ne pense pas.  

Cependant ils sont insuffisants, arrivaient beaucoup trop tôt dans ma réflexion 
et mon implémentation était certainement très maladroite. 

La suite au prochain post.