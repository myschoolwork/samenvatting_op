---
layout: default
---

# Generic Methods

{% highlight java %}

public <T> void genericMethod(Collection<T> coll)
{ ... }

public static <T> void genericMethod(Collection<T> coll, Iterator<? super T>)
{ ... }


Collection<Integer> theCollection = ...;
Iterator<Number> theIterator = ...;

TheClass.<Integer> genericMethod (theCollection, theIterator);
TheClass.genericMethod(theCollection, theIterator);

{% endhighlight %}
