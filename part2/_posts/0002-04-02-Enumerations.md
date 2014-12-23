# Enumerations

Enumerations definieerd een vast aantal benoemde objecten. Elk object kan eigen variabelen en methodes hebben, en ze kunnen er ook delen.

{% highlight java %}
public enum EnumerationType {

    ELEMENT1 { /* Methods and variables for ELEMENT1 */ },
    ...
    ELEMENTn { /* Methods and variables for ELEMENTn */ };

    /* Methods and variables common to all elements */
}
{% endhighlight %}

<!--more-->

{% highlight java %}
public enum Gender {

    MALE {

        /**
        * Return the symbol representing the male gender.
        *
        * @return  The symbol with unicode U+2642.
        *        | result == '\u2642'
        */
        public char getSymbol() {
            return '\u2642';
        }

    },

    FEMALE {

        /**
        * Return the symbol representing the female gender.
        *
        * @return  The symbol with unicode U+2640.
        *        | result == '\u2640'
        */
        public char getSymbol() {
            return '\u2640';
        }
    };

    /**
    * Return the symbol representing this gender.
    *
    * @note    An enumeration may define abstract methods (i.e.
    *          methods without a body). Each element of the
    *          enumeration must then supply its own body.
    */
    public abstract char getSymbol();
}
{% endhighlight %}
