---
layout: default
---

# Special Classes

## Static Member Classes

Een classe kan gedefinieerd worden als een statische classe in een andere classe. Deze *static member classes* hebben toegang tot de private variabelen van de omringende classe, als ook tot de private variabelen van andere *static members classes* in de omringende classe. Geneste classe kunnen variabelen met de zelfde naam hebben als hun ouder.

{% highlight java %}
class EnclosingClass {
    private int x;

    protected static class NestedClass {
        private int x, y;
    }
}
{% endhighlight %}

Deze geneste classen kunnen van buitenaf aangesproken worden met `EnclosingClass.NestedClass` (eventueel met nog extra packages er voor). In de *enclosing class* zelf moet ken je gewoon rechtstreeks `NestedClass.Tralala` gebruiken.

## Non-static Member Classes

Bijna het zelfde, maar kunnen geen statische variabelen of methods bevatten. (Wel final static variabelen.) Men noemt deze *inner classes*. Een enclosing class en inner class hangen steeds samen, vanaf dat new gedaan is kan je ze niet meer uit elkaar halen, in essentie is de inner class dus een beetje *final*.

{% highlight java %}
EnclosingClass enclosingObject = new EnclosingClass();
NestedClass nestedObject = enclosingObject.new NestedClass();
{% endhighlight %}

Vanuit een inner-class kan je de bovenliggende aanspreken met `EnclosingClassName.this` (de inner class zelf met `this`). Als je een method aanroept in de inner class, en de inner class heeft geen method met die naam, dan wordt er gekeken naar de enclosing classe. Als zowel de inner class als enclosing class een method met de zelfde naam hebben, en de inner class roept deze aan, dan zal de versie van de inner class voorrang krijgen.

{% highlight java %}
class
EnclosingClass {
    public void enclMethod();
    public static void staticMethod();

    public class NestedClass {
        public void nestedMethod() {
            enclMethod(); EnclosingClass.this.enclMethod();
            staticMethod (); EnclosingClass.staticMethod();
        }
    }
}
{% endhighlight %}

## Local Classes

Je kan ook een classe aanmaken in een functie van een andere classe. Deze implementeren vaak een interface (of abstracte classe) die buiten de functie ook gekend is.

{% highlight java %}
class EnclosingClass {
    public void g(int x, final int y) {

        class LocalClass implements SomeInterface {

            public void nestedMethod() {
                y = 100;
                System.out.println(x);
            }
        }

        SomeInterface some = new LocalClass();
        ...
    }
}
{% endhighlight %}

## Anonymous Classes

Als je een locale classe wilt maken, maar je wilt er maar één van, dan kan je dit doen zonder de classe een naam te geven.

{% highlight java %}
class EnclosingClass {
    public void g(int x, final int y) {

        SomeInterface some = new SomeInterface { //of bv new SomeClass(arg1, arg2, ...)
            ...
        }

        ...
    }
}
{% endhighlight %}
