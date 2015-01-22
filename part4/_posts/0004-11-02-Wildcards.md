---
layout: default
---

# Wildcards

Een wildcard wordt geschreven als `?`. Zo is List<?> supertype van alle instantiaties van List. `List<? > myList` kan je zien als een lijst waar eender wat in kan zitten, zo kan je dus doen `myLyst = new ArrayList<Integer>();` want myList kan bijvoorbeeld een lijst van Integers zijn. Daarintegen is het volgende **NIET** mogelijk: `myList.add(new Integer())`, want myList kan eender wat bevatten, de compiler kan onmogelijk weten of dat nu wel Integers gaan zijn of niet. `Object o = myList.get(0)` kan dan weer wel, want alles erft over van Object, dus eender wat er in zit kan opgeslagen worden in een variabele met type Object. `Integer i = myList.get(0)` kan niet want wie weet zit er iets anders dan Integers (of iets dat van Integer overerft) in.

## Upperbound

Je kan zo'n wildcard een upperbound geven: `List<? extends Number>` bijvoorbeeld. Dit is dus een lijst van **één** van de subtypes van Number. Hier kan je geen `Integer` aan toevoegen. Een `List<? extends Number>` is wel het supertype van `List<Integer>`.

Achter een `List<? extends Number>` kan dus een lijst vol `Double`s zitten, vandaar dat je er geen `Integer` of `Number` aan kan toevoegen. Je weet wel zeker dat de objecten die er in zitten in een `Number` variabele gestoken kunnen worden.

Je gebruikt dit als je uit de lijst Numbers wilt kunnen halen: `Number nummerke = list.get(0)` (als list van het type `List<? extends Number>` is).

## Lowerbound

Een lower bound kan je aangeven met: `List<? super Number>`. Dit is een lijst van **één** van de supertypes van Number (of Number zelf).

Je gebruikt dit als je in de lijst elementen wilt kunnen bij steken: `list.add(new Integer(...))`. Ja kan echter geen types hoger dan `Number` toevoegen, dus niet: `list.add(new Object())`.

Als je er een een object uit wil halen kan je alleen zeker zijn dat het van het type `Object` zal zijn.

## Generics

Je kan deze upper- en lowerbounds ook gebruiken voor generische classe: `public class GenericSubClass<S extends Number,T>`.
