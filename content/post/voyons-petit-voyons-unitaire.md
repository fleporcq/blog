---
title: "Voyons petit, voyons unitaire"
date: 2017-11-01T11:55:23+01:00
draft: false
tags: ["tests"]
---

Comme je le disais dans [mon article précédent]({{< ref "post/ma-rencontre-avec-les-tests.md" >}}), il fallait que je trouve une solution adaptée au test de ma couche métier. 
Je m'étais clairement trompé en choisissant les tests fonctionnels.

Pour commencer ce voyage initiatique, j'allais devoir retourner aux origines.

![Indiana Jones](https://media.giphy.com/media/VnlQppcCuYRmo/giphy.gif "Indiana Jones")

## Les légendaires tests unitaires ! 

Tout le monde en parle mais que sont-ils réélement ?

Ils ont été popularisés par [Kent Beck](https://fr.wikipedia.org/wiki/Kent_Beck) et [Erich Gamma](https://fr.wikipedia.org/wiki/Erich_Gamma) avec la création du framework JUnit en 1997. 
Puis remis au goût du jour avec les méthodes eXtreme Programming et Test Driven Development.

Malgré son apparente simplicité, le test unitaire n'est pas facile à définir. 
Beaucoup de développeurs se font leur propre idée de ce qu'ils sont.

Cependant, tout le monde s'accorde pour dire qu'un test est un procédé s'assurant du bon fonctionnement d'un traitement.

Maintenant, voici une définition d'unité selon le Larousse : 

> Caractère de ce qui est considéré comme formant un tout dont les diverses parties concourent à constituer un ensemble indivisible.

**On peut en déduire qu'un test unitaire doit s'assurer du bon fonctionnement d'un et un seul élément.**

Cette idée s'oppose directement à celle des tests fonctionnels qui testent l'application dans sa globalité.

Le plus difficile reste à venir, nous devons encore identifier cette unité.

En POO, de quoi est composée une application ?

 - de packages,
 - de classes,
 - de méthodes,
 - de variables.

Nous n'allons évidemment pas tester une variable car elle n'embarque aucune logique ni traitement. 
Quant au package, il parait trop vaste, sa propre existence est liée à la notion de regroupement.  

**Une unité peut donc être soit une classe soit une méthode, c'est une question de point de vue.**

Et ce débat fait rage parmi les développeurs.

Selon [Martin Fowler](https://fr.wikipedia.org/wiki/Martin_Fowler)

> (...)Despite the variations, there are some common elements. 
  Firstly there is a notion that unit tests are low-level, focusing on a small part of the software system. 
  Secondly unit tests are usually written these days by the programmers themselves using their regular tools. 
  Thirdly unit tests are expected to be significantly faster than other kinds of tests.  
  So there's some common elements, but there are also differences.  
  One difference is what people consider to be a unit. 
  Object-oriented design tends to treat a class as the unit, procedural or functional approaches might consider a single function as a unit. 
  But really it's a situational thing - the team decides what makes sense to be a unit for the purposes of their understanding of the system and its testing. 
  However you define it doesn't really matter.(...)[^1]

[^1]: https://martinfowler.com/bliki/UnitTest.html

On peut en retenir : 

{{% alert note %}}
Un test unitaire est un procédé de bas niveau écrit par les développeurs, s'éxecutant rapidement et s'assurant du bon fonctionnement d'une petite partie d'un système.
{{% /alert %}}

*Dans le contexte qui m'intéresse, j'ai fait le choix qu'une classe de test était associée au test d'une classe métier (entité ou service), 
et qu'un test ne vérifiait le bon fonctionnement que d'une seule de ses méthodes.*

## Le modèle AAA 

Maintenant que nous avons une vision assez bonne de ce qu'est un test unitaire, comment l'écrire correctement ?

**Arrange/Act/Assert** (AAA) est un modèle pour organiser son code dans une méthode de test unitaire.

L'idée est de développer un test unitaire en suivant ces 3 étapes simples : 

 - **Arrange**  
   Mise en place des prés-requis à l'exécution du code testé : données d'entrée et dépendances.
 - **Act**  
   Exécution du code à tester.
 - **Assert**  
   Vérification des critères de réussite du test.
   
Voici un exemple volontairement simpliste avec le test de la méthode pow (puissance) de la class Math de java.
   
```java
@Test
public void testPow() {
    // Arrange
    double base = 2d;
    double exponent = 3d;

    // Act
    double result = Math.pow(base, exponent);

    // Assert
    assertEquals(8d, result);
}
```
On trouve également le modèle **Four Phase Test** : 

- Setup
- Exercise
- Verify
- Teardown
 
Les 3 premiers points reprennent exactement les mêmes idées que le modèle AAA. 
Le dernier point "Teardown" est une étape de démontage permettant de remettre le système dans l'état dans lequel il était avant l'exécution du test. 

Celui-ci me dérange beaucoup. Je trouve ça dangereux et compliqué qu'un simple test unitaire influe sur l'état global de mon système. 
Et si mon test échoue ou plante, l'étape de démontage risque de ne pas être exécutée et pourra laisser mon système dans un état incertain.  

{{% alert note %}}
**Si vous êtes obligé de réinitialiser l'état de votre système après l'éxecution d'un test unitaire, remettez en cause votre test, vos données d'entrée et votre code.**

- Suis-je vraiment obligé de le faire ?
- Ne suis-je pas en train d'écrire un test d'intégration ?
- N'ai-je pas une dépendance maladroite ?
{{% /alert %}}
 

## Les principes FIRST

Voici quelques bonnes pratiques à respecter.  
Ces 5 principes peuvent constituer la base d'une checklist pour l'écriture d'un bon test unitaire. 

### Fast  

Un test unitaire se doit d'être rapide (quelque ms). 
Un développeur ne doit jamais hésiter à lancer les tests car ils sont trop long.
La suite de tests doit pouvoir être lancée à chaque commit en intégration, voir à chaque sauvegarde en développement.

### Isolated

Les tests doivent pouvoir être exécutés dans n'importe quel ordre. 
Un test doit pouvoir être lancé individuellement.
Evitez les étapes de démontage qui peuvent masquer un problème de dépendance centralisée (comme une base de données).
 
### Repeatable

Un test doit être déterministe. 
Le résultat doit toujours être le même quelque soit l'environnement et l'instant.
Chaque test doit préparer ses propres données d'entrée. 
Créez une méthode utilitaire si vous avez besoin de mutualiser leur création mais ne les centralisez pas.

### Self-verifying

Aucune étape manuelle ne doit être nécessaire pour déterminer si le test à réussi ou échoué.

### Timely

En pratique vous pouvez écrire des tests n'importe quand, mais le plus tôt sera le mieux.

## Et mes dépendances ?

Comme je l'ai déjà exposé, un bon test doit avoir des dépendances minimales. 
Si un test en a beaucoup, demandez-vous si elles sont légitimes et remettez votre code en question.
Si ces dépendances sont justifiées, essayez de vous en abstraire au maxium. 
Un test doit vérifier le bon fonctionnement d'un objet, pas de ses dépendances. 
Gardez vos dépendances sous contrôle de vos tests en les simulant à travers d'instances que vous créez dans vos tests 
ou de [mocks](https://fr.wikipedia.org/wiki/Mock_(programmation_orient%C3%A9e_objet)) si ce n'est pas possible.

 
## La couverture

La couverture de test est le taux de code couvert par les tests.
Plus ce taux est élevé mieux c'est. Bien sûr, on aurait envie de courir après le 100%. Mais est-ce nécessaire ?
Et même atteint, mon code est-il vraiment testé dans tous les cas possibles ? Bien sûr que non.  

Alors quel est le bon taux ?
Pour ma part, je pense qu'il doit simplement augmenter avec le temps.
N'essayez pas de couvrir votre application en entier dès le début, faite le par petits incréments successifs.
Dans un premier temps vous pouvez vous focaliser sur les parties de votre application que vous jugez les plus critiques ou qui ont le plus de valeur ajoutée.
Quand un bug est remonté dans votre tracker, vous pouvez le couvrir par un test supplémentaire.  
Et si vous atteignez 75%, vous pouvez déjà être fier de vous ! 

Lorsque vous écrivez un test, vous pouvez utiliser des données d'entrée arbitraires, mais n'oubliez pas les valeurs remarquables (tout de suite je pense à la division par 0) ainsi que les bornes de votre système.

Voici ce qu'en dit Kent Beck sur Stack Overflow : 

> I get paid for code that works, not for tests, so my philosophy is to test as little as possible to reach a given level of confidence (...)[^2]

[^2]: https://stackoverflow.com/questions/153234/how-deep-are-your-unit-tests/153565#153565

## Mot de la fin
         
Ne sous-estimez pas vos tests. Ils doivent mériter la même attention que votre code et doivent être expressifs.
Si l'ajout d'un test n'est pas une opération triviale, remettez en question votre environnement et votre code.

**Votre code doit être facilement testable, pensez :**

> Design for testability

Nous verrons dans un prochain article qu'une méthode de développement en particulier ([TDD](https://fr.wikipedia.org/wiki/Test_driven_development)) 
peut grandement nous aider à concevoir du code facilement testable.

