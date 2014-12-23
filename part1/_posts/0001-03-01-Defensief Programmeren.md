---
layout: default
---

# Defensief Programmeren

Exceptions! Gooi exceptions bij onbruikbare parameters. Alternatief voor *totaal programmeren* (afgehanheld in de methode zef) of *nominal programming* (pre-condities).

Als er exceptions gegooid worden moeten deze bij de declaratie van de methode aangegeven worden: `void method() throws Exception1, Exception2`. Errors (stack overflow, out of memory, . . .) moeten hier niet aangegeven worden, **alle** andere exceptions wel.

De reden waarom een bepaalde exception gesmeten word moet ook aangegeven worden in de heading van een classe: @throws of @exception. Liefst zowel formeel als informeel.

<!--more-->

## Checked vs Unchecked

Checked exceptions **moeten** ge-catch-ed worden. De compiler zal na kijken of er een try-catch rond methodes staat die checked exceptions smijten. Zeer lastig en niet zo vaak gebruikt.

Unchecked spreekt dan voor zich, hier zal de compiler niet lastig doen als er geen try-catch rond staat.

## Try-Catch-Finally

{% highlight java %}
try {
    ... //code die mogelijk een exception smijt
}
catch (MijnException1 | MijnException2 ex) {
    ... //code die deze twee soorten exceptions af handels
}
...
catch (MijnException100) {
    ... //code die MijnException100 af handeld
}
finally {
    ... //word sowieso opgeroepen, of er nu een catch nodig was of niet.
}
{% endhighlight %}


## Voorbeeld van eigen exception class:

Hier is eigenlijk niets speciaals aan, bijna het zelfde als een normale classe.

{% highlight java %}
import be.kuleuven.cs.som.annotate.*;

/**
* A class for signaling illegal denominators for rational numbers.
*
* @note     In this session, we use checked exceptions to illustrate
*           the problems they bring in Java. Starting from the next
*           session, we will only work with unchecked exceptions.
* @version  2.0
* @author   Eric Steegmans
*/
public class IllegalDenominatorException extends Exception {

    /**
    * Initialize this new illegal denominator exception with given value.
    *
    * @param  value
    *         The value for this new illegal denominator exception.
    * @post   The value of this new illegal denominator exception is equal
    *         to the given value.
    *       | new.getValue() == value
    */
    public IllegalDenominatorException(long value) {
        this.value = value;
    }

    /**
    * Return the value registered for this illegal denominator exception.
    */
    @Basic @Immutable
    public long getValue() {
        return this.value;
    }

    /**
    * Variable registering the value involved in this illegal denominator
    * exception.
    */
    private final long value;

    /**
    * The Java API strongly recommends to explicitly define a version
    * number for classes that implement the interface Serializable.
    * At this stage, that aspect is of no concern to us.
    */
    private static final long serialVersionUID = 2003001L;

}
{% endhighlight %}


## Voorbeeld van throws

{% highlight java %}
/**
*  Return a rational number obtained by multiplying this rational
*  number with the given integer number.
*
* @param  factor
*         The integer number to multiply with.
* @return The resulting rational number has the same value as a rational number
*         whose denominator is equal to the denominator of this rational
*         number in normalized form, and whose numerator is equal to the
*         numerator of this rational number in normalized form multiplied
*         with the given factor divided by the greatest common divisor of
*         the absolute value of that factor and the denominator of this
*         rational number in normalized form.
*        | let
*        |   reducedFactor = factor / ExtMath.gcd
*        |     (Math.abs(factor),this.normalize().getDenominator())
*        | in
*        |   result.hasSameValueAs(
*        |     new Rational(
*        |       this.normalize().getNumerator()*reducedFactor,
*        |       this.normalize().getDenominator()))
* @throws TimesOverflowException
*         The product of the numerator of this rational number in
*         normalized form with the given factor divided by the greatest
*         common divisor of the absolute value of that factor and the
*         denominator of this rational number in normalized form is
*         outside the range of the type long.
*       | let
*       |   reducedFactor = factor / ExtMath.gcd
*       |     (Math.abs(factor),this.normalize().getDenominator())
*       | in
*       |   ! ExtMath.areMulipliable
*       |     (this.normalize().getNumerator(),reducedFactor)
*/
public Rational times(long factor) throws TimesOverflowException {
    try {
        try {
            // In a first attempt, we simply try to multiply the numerator
            // of this rational number with the given factor. This yields
            // efficient computations for small rational numbers and
            // factors.
            long newNumerator = ExtMath.times(getNumerator(), factor);
            return new Rational(newNumerator, getDenominator());
        } catch (TimesOverflowException exc) {
            assert !ExtMath.areMultipliable(getNumerator(), factor);
            // If we can reduce this rational number by normalization,
            // we invoke the method on the normalized version of this
            // rational number.
            Rational thisNormalized = this.normalize();
            if (thisNormalized.getNumerator() != this.getNumerator())
            return normalize().times(factor);
            // If the given factor and the denominator of this rational
            // number (which must be a normalized rational number at this
            // point), have some factor in common, we invoke the method again
            // reversing the roles of the factor and the numerator of this
            // rational number.
            Rational toReduce = new Rational(factor, this.getDenominator())
            .normalize();
            if (toReduce.getNumerator() != factor)
            return toReduce.times(this.getNumerator());
            throw exc;
        }
    } catch (IllegalDenominatorException exc) {
        assert false;
        return null;
    }
}
{% endhighlight %}
