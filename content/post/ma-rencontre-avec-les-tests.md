---
title: "Ma rencontre avec les tests"
date: 2017-10-27T13:45:57+02:00
draft: false
tags: ["tests"]
---

Je me souviens du jour où j'ai découvert les tests.  
Le concept m'a d'abord paru étrange.    

> Ecrire du code qui teste le code ?  

Une question stupide m'a traversé l'esprit.

> Oui mais alors, comment tester le code qui teste le code ?  

Puis, j'ai été séduit par l'idée.  

Ok, j'en avais compris les intérêts dans les grandes lignes : 

 - s'assurer qu'un code fonctionne tel qu'on s'y attend, 
 - s'assurer qu'un code fonctionne aux bornes du système, 
 - s'assurer qu'un changement n'entraîne pas une régression, 
 - automatiser une tâche répétitive donc chiante, 
 - être beaucoup plus serein pendant les phases de refactoring, 
 - rentrer chez soi avec la satisfaction d'un travail abouti. 

Il ne restait plus qu'à la mettre en pratique. 
Je me suis alors jeté dans le grand bain, sans expérience et sans réel approfondissement du sujet.  
Erreur, que j'allais payer rapidement.

![Failed](https://media.giphy.com/media/rjr9etfxrdP3i/giphy-downsized-large.gif "Failed")

J'ai été très vite confronté à des problèmes divers qui ralentissaient considérablement ma capacité à produire le "vrai" code.  

> J'ai pas le temps de faire des tests, on devrait les inclure à nos chiffrages.

Echec total, renoncement, rejet d'un concept qui semblait prometteur.

**Certes, les tests prennent du temps, mais, correctement implémentés, en font gagner beaucoup plus.**

## Mes erreurs

J'ai pu rencontrer, lors de conférences, des développeurs manifestement conquis par les tests. 
Il était évident que j'étais passé à côté de quelque chose.

L'idée m'avait semblée pourtant bonne.  

> Comme j'ai peu de temps, je vais traverser la stack de bout en bout pour la tester dans son intégralité.  

D'où la création de tests qui lanceraient des requêtes http et feraient des assertions sur le contenu retourné. 
Si le contenu retourné est celui attendu, j'extrapole le fait que les couches intermédiaires ont correctement fonctionnées. 
Génial ! Pas tant que ça...

Premiers points génants :  

- Ca ne peut s'appliquer qu'à un environnemnt web, 
- si je veux réutiliser la couche métier dans un autre projet,    
  je n'ai plus aucun test,
- les données d'entrées ne peuvent être envoyées que par http,
- les assertions sont complexes à réaliser car je dois au préalable parser le retour html.

De plus, comme l'ensemble de la stack est traversé, il est indispensable que toutes les couches soient présentes. 
L'environnement de test n'étant pas un environnement d'exécution "normal" (comme dev ou prod), je dois simuler certaines couches (cf. [mock](https://fr.wikipedia.org/wiki/Mock_(programmation_orient%C3%A9e_objet))). 
Dans mon cas, par exemple, la session utilisateur.

Enfin, pour pouvoir faire des assertions correctes, il faut également maîtriser le jeu de données initiales.
Or, je n'ai pas d'accès direct à la couche métier. 
Je suis donc dans l'obligation de créer une base données spécifique à l'environnement de test. 

Avant d'avoir commencer, on voit déjà beaucoup de complications apparaîtrent.

Une fois l'environnement en place, un autre problème est rapidement apparu. Les données de test étant centralisées dans une base de données, 
elles devenaient communes à l'ensemble des tests. 
Un test qui modifiait l'état de cette base, impactait directement les tests suivants ! 

En conséquence, j'ai dû réinitialiser la base de données avant chaque test. C'est bourrin et très inefficace !
Malgré cette réinitialisation systèmatique, mes tests étaient toujours très dépendants les uns des autres.
L'insertion de données initiales dans la base impactait également l'ensemble des tests.

Il devenait très compliqué de modifier ou d'ajouter un test.

**L'environnement de tests doit être simple à mettre oeuvre, l'ajout d'un test doit être une opération triviale et rapide. 
Si ce n'est pas le cas, vous constaterez un désengagement des développeurs à les maintenir et à les faire évoluer.**

Le package "tests" est devenu peu à peu une ville fantôme où n'erraient plus que quelques âmes perdues.

![Far west](https://media.giphy.com/media/W0KiMlQIj4nzq/giphy.gif "Far west")

Ce type de tests est appelé tests fonctionnels car ils s'emploient à vérifier le bon fonctionnement général d'une application 
tel que le ferait un humain.  
Ce sont ce qu'on pourrait appeler des tests de haut niveau.  
*D'autre tests de ce genre vont jusqu'à reproduire automatiquement le comportement d'un utilisateur directement depuis un navigateur 
(cf. [Selenium](https://fr.wikipedia.org/wiki/Selenium_(informatique))).*

**Est-ce que les tests fonctionnels sont intrinsèquement mauvais ?**  
Je ne pense pas. 

Ils peuvent être très adaptés pour tester les retours d'une api rest, ou dans le test d'interfaces.
Mais pas pour une couche métier.

Retour à la case départ, je devais trouver un socle de test plus en adéquation avec mon besoin.

La suite au prochain post.