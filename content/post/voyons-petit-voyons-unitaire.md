---
title: "Voyons petit, voyons unitaire"
date: 2017-11-01T11:55:23+01:00
draft: true
tags: ["tests"]
---

Comme je le disais dans [mon article précédent]({{< ref "post/ma-rencontre-avec-les-tests.md" >}}), il fallait que je trouve une solution adaptée au test de ma couche métier. 
Je m'étais clairement trompé en choisissant les tests fonctionnels.

Pour commencer ce voyage initiatique, j'allais devoir retourner aux origines.

![Indiana Jones](https://media.giphy.com/media/VnlQppcCuYRmo/giphy.gif "Indiana Jones")

## Les légendaires tests unitaires ! 

Tout le monde en parle mais qui sont-ils réélement ?

Ils ont été popularisés par Kent Beck et Erich Gamma avec la création du framework JUnit en 1997. 
Puis remis au goût du jour avec les méthodes eXtreme Programming et Test Driven Development.

Malgré son apparente simplicité, le concept de test unitaire n'est pas facile à définir. 
Beaucoup de développeurs se font leur propre idée de ce qu'est un test unitaire.

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
Quand au package, il parait trop vaste, sa propre existence est liée à la notion de regroupement.  

**Une unité peut donc être soit une classe soit une méthode, c'est une question de point de vue.**

Et ce débat fait rage parmi les développeurs.

Selon Martin Fowler (Auteur reconnu dans la conception logicielle et ami de Kent Beck)

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

*Dans le context qui m'intéresse, j'ai fait le choix qu'une classe de test était associée au test d'une classe métier (entité ou service), 
et qu'un test ne vérifiait le bon fonctionnement que d'une seule de ses méthodes.*

## Le modèle AAA 

Maintenant que nous avons une vision assez bonne de ce qu'est un test unitaire, comment l'écrire correctement ?

Arrange/Act/Assert (AAA) est un modèle pour organiser son code dans une méthode de test unitaire.

L'idée est de développer un test unitaire en suivant ces 3 étapes simples : 

 - **Arrange**  
   Mise en place des prés-requis à l'exécution du code testé : données d'entrée et dépendances.
 - **Act**  
   Exécution du code à tester.
 - **Assert**  
   Vérification des critères de réussite du test.
   
Voici un exemple volontairement simpliste avec le test de la méthode pow (puissance) de la class Math de java.
   
```
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
On trouve également le modèle Four Phase Test : 

- Setup
- Exercise
- Verify
- Teardown
 
Les 3 premiers points reprennent exactement la même idée que le modèle AAA. 
Mais le dernier point "Teardown" (démontage) me dérange.
Cette étape de démontage sert à remettre le système dans l'état dans lequel il était avant l'exécution du test.  

- Si mon test influe sur l'état du système, est-ce toujours un test unitaire, n'est ce pas un test d'intégration ?  
- Si mon test échoue ou plante, mon étape de démontage risque de ne pas être exécutée et pourra laisser mon système dans un état incertain.  
- Si je me trouve dans l'incapacité d'écrire un test unitaire isolé du reste du système, n'ai je pas un problème de couplage ?  

## Les principes FIRST

FIRST

- **Fast**
- **Isolated (Independent)**
- **Repeatable**
- **Self-verifying**
- **Timely (Thorough)**
 
 
 
## Et mes dépendances ?
 
 https://github.com/ghsukumar/SFDC_Best_Practices/wiki/F.I.R.S.T-Principles-of-Unit-Testing






https://stackoverflow.com/questions/153234/how-deep-are-your-unit-tests/153565#153565

I get paid for code that works, not for tests, so my philosophy is to test as little as possible to reach a given level of confidence ...
Kent Beck

un test doit être expressif  comme le code

unitaire = Différent test d'intégration
si l'ajout d'un test n'est pas une opération triviale, remettez en question votre environnement de test et votre code.

Another mental picture—programming is like exploring a dark house. You go from
room to room to room. Writing the test is like turning on the light. Then you can avoid
the furniture and save your shins (the clean design resulting from refactoring). Then
you’re ready to explore the next room

Test-driven development (TDD) is a way of managing fear during programming. I
don’t mean fear in a bad way, pow widdle prwogwammew needs a pacifiew, but fear
in the legitimate, this-is-a-hard-problem-and-I-can’t-see-the-end-from-the-beginning
sense. If pain is nature’s way of saying “Stop!”, fear is nature’s way of saying “Be
careful.” The problem is that fear has a host of other effects:
• Makes you tentative
• Makes you grumpy
• Makes you want to communicate less
• Makes you shy from feedback

un test doit être déterministe

WHAT’S A CODE SMELL? A smell in code is a hint that something might be
wrong with the code. To quote the Portland Pattern Repository’s Wiki, “If
something smells, it definitely needs to be checked out, but it may not actually
need fixing or might have to just be tolerated.”

1 cas de test par méthode
Design for testability

Faire un lien de ma rencontre avec les tests vers cet article.