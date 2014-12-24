---
layout: default
---

# Inheritance

Een subclasse erft over van een superclasse. Alle methods en variabelen worden overgenomen, met als uitzondering constructors. In java mag je slechts overerven van één superclasse. De superclasse waar je van overerft kan wel nog overerven van een andere classe. Als je geen classe op geeft waar je van overerft erf je stiekem toch over van Object.

Overerven doe je met het keyword `extends` bv `public class Painting extends Ownable { ... }`.

Abstracte classe: kan je geen objecten van maken, dient enkel om van over te erven. Een abstracte classe kan methods, variabelen en zelfs constructors hebben (maar die laatste kan niet gebruikt worden om er een object van te maken, en enkel aangeroepen worden als deel van een andere constructor.) Je maakt een classe abstract door in zijn heading `abstract` te zetten.

Een subclasse heeft geen toegang tot private variabelen van de superclasse, de subclasse kan echter wel de getters en setters gedefineerd in de superclasse gebruiken. Een object van de subclasse bevat deze private variabelen echter wel, en maakt er dus geheugen voor, ze compiler laat alleen niet toe dat je ze rechtstreeks aanspreekt.

Een constructor van een subclasse is een volledig nieuwe method. Je moet er dus een volledig nieuwe specificatie voor uitwerken, en niet persé rekening houden met die van de superclasse. Je kan wel de constructor van de superclasse aanroepen in de subclasse door `super(...)` te gebruiken. (Een andere constructor in de classe zelf kan je aanroepen met `this(...)`.)

<!--more-->

## UML

In uml geeft een lijn met een **holle** pijl overerving aan.

![inheritance]({{ site.github.url }}/part3/images/uml_inheritance.png)
