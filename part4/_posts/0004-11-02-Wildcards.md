---
layout: default
---

# Wildcards

Een wildcard wordt geschreven als `?`. Zo is List<?> supertype van alle instantiaties van List. Als je een `List<?> myList` hebt, dan is het volgende perfect mogelijk: `myList.add(new Integer())`, or worden geen types nagekeken. Zo kan ook `Object o = myList.get(0)` gebruikt worden.

## Upperbound

Je kan zo'n wildcard een upperbound geven: `List<? extends Number>` bijvoorbeeld. Dit is dus een lijst van **één** van de subtypes van Number. Hier kan je geen `Integer` aan toevoegen. Een `List<? extends Number>` is wel het supertype van `List<Integer>`.

Achter een `List<? extends Number>` kan dus een lijst vol `Double`s zitten, of vandaar dat je er geen `Integer` of `Number` aan kan toevoegen. Je weet wel zeker dat de objecten die er in zitten in een `Number` variabele gestoken kunnen worden.

Je gebruikt dit als je uit de lijst Numbers wilt kunnen halen: `Number nummerke = list.get(0)` (als list van het type `List<? extends Number>` is).

## Lowerbound

Een lower bound kan je aangeven met: `List<? super Number>`. Dit is een lijst van **één** van de supertypes van Number.

Je gebruikt dit als je in de lijst elementen wilt kunnen bij steken: `list.add(new Integer(...))`. Ja kan echter geen types hoger dan `Number` toevoegen, dus niet: `list.add(new Object())`.

Als je er een een object uit wil halen kan je alleen zeker zijn dat het van het type `Object` zal zijn.

## Generics

Je kan deze upper- en lowerbounds ook gebruiken voor generische classe: `public class GenericSubClass<S extends Number,T>`.
