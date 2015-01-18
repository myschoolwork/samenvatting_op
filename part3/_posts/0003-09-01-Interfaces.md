# Interfaces

Een interface introduceert één of meerdere functies. Deze functies zijn altijd public en abstract.

Sinds Java8 kunnen er ook default functie implementaties in interfaces gegeven worden. Functies die dit hebben worden aangeduid met `default`.

{% highlight java %}
public interface SomeInterface {
    public static final int constante;

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

Een classe kan zo veel interfaces implementeren als hij maar wil, op bijna de zelfde manier als het overerven van een classe:

{% highlight java %}
public class MyClass
    extends OtherClass
    implements AnInterface, AnotherInterface
{...}
{% endhighlight %}

Een interface kan andere interfaces implementeren. De SubInterface kan bestaande methods dan een `default` implementatie geven, ze worden dan ook geannoteerd met `@Override`. Ze subinterface kan ook `default` methods on-implementeren. (Zie 'meer' voor voorbeeld.)

Als een classe meerdere interfaces inplementeerd, die elks een functie met de zelfde signatuur hebben, dan worden deze functies samen één functie.

<!--more-->

## (On)Implementeren van default methods

{% highlight java %}
public interface SubInterface extends SomeInterface {
    @Override
    // Implementation of inherited abstract method.
    public default int abstractMethod(Object o) {
        return -1;
    }

    // Un-implementation of inherited default method.
    @Override public boolean defaultMethod(Object o);

    // Invocation of super-version.
    @Override public default void defaultMethod2() {
        SomeInterface.super.defaultMethod2();
    }
}
{% endhighlight %}

## Meerdere interfaces implementeren met de zelfde method

{% highlight java %}
public interface I1 {
    public abstract int abstractMethod(Object o);

    public default boolean defaultMethod(Object o)
    {
        return true ;
    }
}

public interface I2 {
    public default int abstractMethod(Object o)
    {
        return -1;
    }

    public abstract boolean defaultMethod(Object o);
}

public interface I12
    extends I1, I2
{
    // methods worden samengevoegd

    @Override
    public abstract int abstractMethod(Object o);

    @Override
    public default boolean defaultMethod(Object o)
    {
        return false;
    }
}
{% endhighlight %}
