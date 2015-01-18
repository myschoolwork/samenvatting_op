---
layout: default
---

# Generic Methods

Gebruikte generische parameter types zet je *voor* het return type (`<T> int f...`).

{% highlight java %}
public <T> void genericMethod(Collection<T> coll)
{ ... }

public static <T> void genericMethod(Collection<T> coll, Iterator<? super T>)
{ ... }
{% endhighlight %}

Bij het aanroepen mag je expliciet de types mee geven, maar dit is niet nodig, de compiler kan zelf verzinnen welke ze moeten zijn aan de hand van de parameters die je mee geeft. De compiler zal komen klagen als je verkeerde mee geeft, of als hij er geen juiste kan afleiden uit je parameters.

{% highlight java %}
Collection<Integer> theCollection = ...;
Iterator<Number> theIterator = ...;

TheClass.<Integer> genericMethod (theCollection, theIterator);
TheClass.genericMethod(theCollection, theIterator);

{% endhighlight %}
