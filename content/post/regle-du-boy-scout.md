---
title: "La règle du boy-scout"
date: 2017-10-25T20:31:18+02:00
draft: false
tags: ["bonnes-pratiques", "amelioration-continue"]
---

Je ne pouvais pas commencer ce blog, sans vous parler de la règle du boy-scout. 
Je l'ai découverte dans un des premiers chapitres du livre *Clean Code* de Robert C. Martin et elle a immédiatement résonné en moi comme une évidence.  

> Leave the campground cleaner that you found it.  
  Laissez le camp plus propre que vous ne l'avez trouvé. 

L'idée est tellement simple et pleine de bon sens que je ne comprends pas qu'on ne l'enseigne pas à l'école.

J'avoue que ça fait un peu penser à un panneau trouvé dans des toilettes publiques, mais cette règle est beaucoup plus puissante 
que de juste laisser l'endroit aussi propre que nous l'avons trouvé. Tout est dans le "**plus** propre".

Cette idée vient de la lettre d'adieu de [Robert Baden-Powell](https://fr.wikipedia.org/wiki/Robert_Baden-Powell) fondateur du scoutisme :

> Essayez de quitter ce monde en le laissant un peu meilleur que vous l’avez trouvé. Et quand l’heure de la mort approchera, vous pourrez mourir heureux en pensant (…) que vous avez fait de votre mieux .

Je suis convaincu que ce devrait être le premier mantra de l'amélioration continue, et ce, quelque soit le domaine d'application.  

En développement logiciel, nous pouvons la reformuler ainsi :

> Push a code cleaner that you pulled it.  
  Pushez un code plus propre que vous ne l'avez pullé.
  
Dans un monde où [l'entropie](https://fr.wikipedia.org/wiki/Entropie) est la régle (principe du bordel croissant), imaginez que chacun applique cette petite recommandation. 
 
Le système ne pourrait pas se dégrader.  

Bon, ça semble un peu utopique dit comme ça, mais je suis de nature optimiste et certain que si dans une équipe de développement tout le monde adhère à cet état d'esprit, 
le code ne se dégradera pas, ou du moins beaucoup plus lentement.

Beaucoup de choses peuvent imperceptiblement dégrader du code : 

- un changement d'avis du client, 
- une nouvelle fonctionnalité, 
- une évolution, 
- une correction de bug, 
- ...

Quel est le point commun des éléments de cette liste ?  

Le changement.  

Le changement concourt à dédrager le code, tout comme l'évolution d'un système concourt à sa désorganisation. C'est frustrant mais c'est ainsi !

Pour autant, il ne faut pas s'emballer et refaire le monde, il y a tout de même quelques règles à respecter.

L'idée n'est pas d'attaquer un gros chantier de refactoring et de tout casser, mais de mener cette action au quotidien, avant chaque push sur votre repository.  
L'amélioration sera lente mais pérenne.  
D'ailleurs, si vous faites du [Scrum](https://fr.wikipedia.org/wiki/Scrum_(Boite_%C3%A0_outils)), il n'y a pas de sprint dédié à la refactorisation, chaque sprint doit produire de la plus-value fonctionnelle.  

Veillez à garder des commits cohérents, n'améliorez que le code concerné par la fonctionnalité sur laquelle vous travaillez à cet instant, ne modifiez pas des fichiers sans rapport avec celle-ci.
Ne vous laissez pas tenter par une amélioration possible si ce n'est pas le moment, notez là (mieux, faites un ticket dans votre tracker), et revenez dessus ultérieurement.  

Ces améliorations ne sont pas nécessairement importantes, cela peut être juste un nom plus adapté pour une classe ou l'extraction d'une méthode.  
Dans le cas de refactorisations importantes, assurez-vous d'avoir un harnais de tests suffisant (avec une bonne couverture de code).

Le [TDD](https://fr.wikipedia.org/wiki/Test_driven_development) peut être une méthode efficace d'amélioration continue.
Ce type de développement laisse la place au "mauvais" code, puis à son amélioration sans risque de régression. 
J'entends par mauvais, la première version passant les tests, car personne n'écrit d'emblée du bon code.

N'oubliez pas la formule de [Kent Beck](https://fr.wikipedia.org/wiki/Kent_Beck) (entre autre co-développeur de Junit) :  

> Make It Work, Make It Right, Make It Fast.

Nous nous arrêtons souvent au premier point de cette formule, sans prendre en considération les suivants.  
Ce n'est pas parcequ'un code fonctionne qu'il est bon.

Pour finir je prendrais l'image de la décharge sauvage.  

![Décharge sauvage](/img/post/regle-du-boy-scout-decharge.jpg "Décharge sauvage")

Vous savez, celle qui commence par trois canettes et un vieux reste de McDo à l'orée d'un bois...  Et qui finit avec un pneu et une machine à laver.  
Qui n'a jamais été tenté de faire un fix ou un évolution rapide dont il n'est pas fier, mais qui se dit (honteusement) que de toute façon le code était déjà mauvais avant ? 
Et je vous l'assure, ce sera de pire en pire... Si rien n'est fait, le chaos ne fera que croître, et ce de façon exponentielle.  
Le désordre appelle le désordre et délite la responsabilité.

Le code que j'écris dans le cadre professionnel ne m'appartient pas, évidemment il appartient au client, mais ce n'est pas ce que je veux dire.  
Ce n'est pas MON code, c'est celui de l'application, et tous mes collègues sont libres d'y apporter les améliorations qui leur semble nécessaires.  

Vous l'aurez compris, je n'aime pas le tag @author,  
nous sommes tous responsable de la qualité du produit que nous livrons.