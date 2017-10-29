---
title: "TDD en 5 min"
date: 2017-10-27T13:45:57+02:00
draft: true
tags: ["tests", "tdd"]
---

Je me souviens encore du jour où j'ai découvert les tests.  
Le concept m'a d'abord paru étrange.    

> Ecrire du code qui test le code ?  
  Oui mais alors, comment tester le code qui test le code ?  

Puis j'ai été séduit par l'idée.  
Ok, j'en avais compris les intérêts dans les grandes lignes : 

 - s'assurer qu'un code fonctionne tel qu'on s'y attend, 
 - s'assurer qu'un code fonctionne aux bornes du sytème, 
 - s'assurer qu'un changement n'entraîne pas une régression, 
 - automatiser une tâche répétitive donc chiante, 
 - être beaucoup plus serein pendant les phases de refactoring, 
 - rentrer chez soi avec la satisfaction d'un travail abouti, 

Il ne restait plus qu'à la mettre en pratique. 
Je me suis alors jeté dans le grand bain, sans expérience et sans réel approfondissement du sujet. Erreur, que je paierai rapidement.

![Failed](https://media.giphy.com/media/rjr9etfxrdP3i/giphy.gif "Failed")

J'ai été très vite confronté à des problèmes divers qui ralentissaient considérablement ma capacité à produire le "vrai" code.  
Une phrase revenait souvent : 

> J'ai pas le temps de faire des tests, on devrait les inclure à nos chiffrages.

Echec total, renoncement, rejet d'un concept qui semblait prometteur.

Certes, les tests prennent du temps, mais, correctement implémentés, en font gagner beaucoup plus.

## Mes erreurs

J'ai pu rencontrer, lors de conférences, des personnes emballés par les tests. Il était évident que j'étais passé à côté de quelque chose.

### Type de tests

#### Tests fonctionnels

Ma première erreur a été de ne pas choisir le bon type de test pour commencer. 
L'idée semblait pourtant plutôt bonne. Comme nous avons peu de temps, pour tester l'ensemble de la stack, nous allons la traverser de bout en bout.
D'où la création de tests qui lanceraient des requêtes http et feraient des assertions sur le contenu retourné. 
Si le contenu retourné est celui attendu, nous extrapolons le fait que les couches intermédiaire ont correctement fonctionnées.
Génial !  
Pas tant que ça...

Ce type de test est appelé tests fonctionnels car ils s'emploient à vérifier le bon fonctionement général d'une application tel que la voit un utilisateur. 
Ce sont ce qu'on pourrait appeler des tests de haut niveau.  
D'autre tests de ce type (Selenium) vont jusqu'à reproduire automatiquement le comportement d'un utilisateur directement dans le navigateur.


Premier point génant, ça ne peut s'appliquer qu'à un environnemnt web, si je veux réutiliser ma couche métier dans un autre projet 
(car comme j'essaie  de bien faire, je sépare les couches techniques et métier), je n'ai plus aucun test.

Ensuite, comme nous traversons l'ensemble de la stack, il faut que toutes les couches soient présentes.
L'environnement de test étant différent de l'environnement d'éxecution normal, il va donc falloir simuler certaines couches.
Notamment la base de données.
Pour pouvoir vérifier correctement un contenu retourné par une requête http, il va falloir maîtrise le jeu de données initiales.

Compliqué
- appel async
- Intégration continue plus complexe
- Gestion d'une session
- Gestion d'une base de données
- Mise en place d'un navigateur headless en intégration continue

Avant de voir grand, voyons petit, voyons unitaire.

#### Tests unitaire

mon code était difficilement testable.

## TDD à la rescousse

non naturel d'un premier abord.


### FIRST

### AAA

https://github.com/ghsukumar/SFDC_Best_Practices/wiki/F.I.R.S.T-Principles-of-Unit-Testing

 - modèle AAA Arrange, Act, Assert
     - Arrange all necessary preconditions and inputs.
     - Act on the object or method under test.
     - Assert that the expected results have occurred.
 - on ne peut pas tout tester
 - faire un choix sur le type de test
 - test expressif -> fluent -> BDD / DDD (Réponse au TDD)
 - limites des test
 - tester les bornes du système
 - Test Driven Development: By Example - Kent Beck
 - différent type de test unitaire/fonctionnel/behavior/accceptance
 - je n'écrivais pas du code testable
 - qualité / assurance
 - non régression
 - couverture de code (règle de non diminution de ce taux, hook)
 - rassurant
 - les tests sont l'affaire de tous
 - definition of done
 - FIRST FAST, ISOLATED/INDEPENDENT, REPEATABLE, SELF-VALIDATING and THOROUGH/TIMELY
 - Red/Green/Refactor
 - on n'écrit pas du test comme du code (valeur attendu en dur ?)
 - amélioration continue

