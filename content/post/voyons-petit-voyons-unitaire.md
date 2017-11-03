---
title: "Voyons petit, voyons unitaire"
date: 2017-11-01T11:55:23+01:00
draft: true
tags: ["tests"]
---

Comme je le disais dans mon article précédent, il fallait que je trouve une solution adaptée au test de ma couche métier. 
Je m'étais clairement trompé en choisissant les tests fonctionnels.

Pour commencer ce voyage initiatique, j'allais devoir retourner aux origines.

![Indiana Jones](https://media.giphy.com/media/VnlQppcCuYRmo/giphy.gif "Indiana Jones")

## Les légendaires tests unitaires ! 

Tout le monde en parle mais qui sont-ils réélement ?

Ils ont été popularisés par Kent Beck et Erich Gamma avec la création du framework JUnit en 1997. 
Puis remis au goût du jour avec les méthodes eXtreme Programming et Test Driven Development.

Malgré son apparente simplicité, le concept de test unitaire n'est pas facile à définir. 
Beaucoup de développeurs se font leur propre idée de ce qu'est un test unitaire.

Tout le monde s'accorde pour dire qu'un test est un procédé s'assurant du bon fonctionnement de quelque chose.

Mais qu'en est t'il du concept d' **"unitaire"**. J'ai cherché une définition sur internet et étonnement je n'ai rien trouvé de probant.  
Voici la définition la plus récente de l'académie française (1932) :  

> Qui est partisan  de l'unité. Il se dit spécialement d'une Ecole de théologiens protestants qui, en admettant la révélation, ne reconnaît qu'un seule personne en Dieu. On dit dans le même sens "Unitarien".

Super, je suis bien avancé là...

Cherchons la définition d'"unité". Selon le Larousse : 

> Caractère de ce qui est un, unique.

**On peut en déduire qu'un test unitaire doit s'assurer du bon fonctionnement d'un et un seul élément.**

Cette idée s'oppose directement à celle des tests fonctionnels qui testent l'application dans sa globalité.

Le plus difficile reste à venir, nous devons encore identifier cette unité.

En programmation objet, de quoi est composée une application ?

 - de packages,
 - de classes,
 - de méthodes,
 - de variables.

Nous n'allons évidemment pas tester une variable car elle n'embarque aucune logique ni traitement. 
Quand au package, sa propre existence est liée à la notion de regroupement.  

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
  However you define it doesn't really matter.[^1]

[^1]: https://martinfowler.com/bliki/UnitTest.html

**Un test unitaire est un procédé de bas niveau écrit par les développeurs s'assurant du bon fonctionnement d'une petite partie du système.**

Dans le context qui m'intéresse, j'ai fait le choix qu'une classe de test était associée à une entité ou un service métier et qu'un test ne vérifiait le bon fonctionnement que d'une seule méthode.

## Le plan AAA 


 - **Arrange**  
   Mise en place des prés-requis à l'exécution du code testé : Dépendances, mocks et données d'entrée.
 - **Act**  
   Exécution du code à tester.
 - **Assert**  
   Définition des critères de réussite du test.
   
```
 @Test
 public void testReverse() {
    // Arrange
    String input = "abc";
    		
    // Act
    String result = Util.reverse(input);
    
    // Assert
    assertEquals("cba", result);
 }
```

setup, exercise, verify, teardown  
Design for testability

## En premier

FIRST
 - **Fast**
 - **Isolated (Independent)**
 - **Repeatable**
 - **Self-verifying**
 - **Timely (Thorough)**
 
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
