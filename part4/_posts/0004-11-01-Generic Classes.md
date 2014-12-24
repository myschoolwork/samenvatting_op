---
layout: default
---

# Generic Classes

Je kan een class definieren rond enkele nog ongekende types. Deze types kan je dan later invullen. De gekozen types kunnen enkel reference types zijn, geen primitieve types (omdat ze in de achtergrond gwn naar Object worden omgezet).

{% highlight java %}
public class GenericClass<S,T> {
    public S someMethod(T p) {
        S localVar; //OK
        S otherVar = new S(); //NOT OK, geen new S() mogelijk

        S[] localArray; //OK
        S[] otherArray = new S[]; //NOT OK.
    }
}
{% endhighlight %}

Bij het instantieren van deze classe geef je dan types mee: `new GenericClass<Integer,Person>(...)` (maar niet `new GenericClass(int, Person)` want int is een primitief type). Als je dan `someMethod` wilt aanroepen zal je een Person moeten meegeven. (Je kan ook nog steeds `new GenericClass(...)` doen, zonder types, dan wordt voor allemaal `Object` verondersteld, maar de compiler zal hier een warning voor geven. Dit is voor backwards compatibility.)

## Compilatie

Als zo'n generische classe gecompileerd wordt worden alle generische types omgezet naar `Object`, en er wordt dus slechts één classe gecompileerd (in tegenstelling tot bv C++). Dit geeft soms rare gevolgen, want casten naar één van de generische types wordt dus eigenlijk casten naar Object, totaal nutteloos dus!

## Polymorphism

Hoewel `Integer` een subtype is van `Number`, is `List<Integer>` dat niet van `List<Number>`, maar bij arrays wel: `Integer[]` is een subtype van `Number[]`.
