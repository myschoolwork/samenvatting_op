# Interfaces

Een interface introduceert één of meerdere functies. Deze functies zijn altijd public en abstract.

Sinds Java8 kunnen er ook default functie implementaties in interfaces gegeven worden. Functies die dit hebben worden aangeduid met `default`.

{% highlight java %}
public interface SomeInterface {
    public abstract int abstractMethod(Object o);

    //sinds Java 8
    public default boolean defaultMethod(Object o) {
        return this.abstractMethod(o) < 0;
    }

    //sinds Java 8
    public default void defaultMethod2() {
        System.out.println("Hello from default method 2!");
    }

    //sinds Java 8
    public static void staticMethod() {
        System.out.println("Hello from Interface!");
    }
}
{% endhighlight %}

Een classe kan zo veel interfaces implementeren als hij maar wil, op de zelfde manier als het overerven van een classe:

{% highlight java %}
public class MyClass
    extends OtherClass
    implements AnInterface, AnotherInterface
{...}
{% endhighlight %}

Een interface kan andere interfaces implementeren.

Als een classe meerdere interfaces inplementeerd, die elks een functie met de zelfde signatuur hebben, dan worden deze functies samen één functie.
