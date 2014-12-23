---
layout: default
---

# Class Diagrams

![marriage]({{ site.github.url }}/part2/images/uml_marriage.png)

Classen worden voorgesteld als rechthoeken met 3 onderdelen. Eerst de naam van de classe, dan de attributes (of properties) en als laatste de methods. Attributes krijgen een achterliggende variabele, met eventueel een getter en setter.

Associaties worden aangegeven met een lijntje tussen classes. Er kan een bijhorende attribute aangegeven worden bij het lijntje (-spouse op de afbeelding), als ook een naam voor de associatie (marriage). Verder kan je de multipliciteit aangeven (bv 0..1, 0.., 1..).

Belangrijk: Als je bi-directionele assiciaties hebt moeten mutators, destructors en constructors de classe invarianten respecteren in **beide** richtingen.

<!--more-->

## Bi-directional

Bi-directionele associaties moeten een classe-invariant hebben die deze af dwingt. Meestal zal er gebruik gemaakt worden van `canHaveAsSomething(T)`. De consistentie wordt gechecked in the inspector `hasProperSomething()`


## Voorbeeldje

De person.java classe gemaakt in de les. Er zijn enkele stukken uit weggehaald met wat minder interessante delen.

{% highlight java %}
import be.kuleuven.cs.som.annotate.*;

/**
* A class of persons involving a gender and a
* marital partner.
*
* @invar   The gender of each person must be a valid gender
*          for a person.
*          isValidGender(getGender())
* @invar   Each person must have a proper spouse.
*        | hasProperSpouse()
*
* @version  2.0
* @author   Eric Steegmans
*/
public class Person {

    /**
    * Initialize this new person with given gender and given partner.
    *
    * @param  gender
    *         The gender for this new person.
    * @param  partner
    *         The partner for this new person.
    * @post   The gender of this new person is the same as the given gender.
    *       | new.getGender() == gender
    * @post   This new person references the given partner as its spouse.
    *       | new.getSpouse() == partner
    * @post   If the given partner references a true person, that partner
    *         references this new person as its spouse.
    *       | if (partner != null)
    *       |   then (new partner).getSpouse() == this
    * @throws IllegalArgumentException
    *         The given gender is not a valid gender for a person.
    *       | ! isValidGender(gender)
    * @throws IllegalPartnerException
    *         This new person cannot have the given partner as its spouse
    *         in view of the given gender.
    *       | ! canHaveAsSpouse(partner,gender)
    * @throws IllegalPartnerException
    *         The given partner is effective and already married.
    *       | (partner != null) && partner.isMarried()
    * @note   In the specification of this constructor, we cannot use the
    *         mutator 'marry'. Indeed, upon entry to the constructor, the
    *         gender of this new person is still null, meaning that that
    *         new person does not satisfy its class invariants. Because
    *         the method 'marry' is not a raw method, we cannot invoke it
    *         against the new person.
    */
    public Person(Gender gender, Person partner)
    throws IllegalArgumentException, IllegalPartnerException {
        if (!isValidGender(gender))
        throw new IllegalArgumentException("Improper gender!");
        this.gender = gender;
        if (partner != null)
        marry(partner);
    }

    /**
    * Return the spouse of this person.
    *    A null reference is returned if this person is not married.
    */
    @Basic @Raw
    public Person getSpouse() {
        return this.spouse;
    }

    /**
    * Check whether this person can have the other person as its spouse
    * in view of the given gender.
    *
    * @param  other
    *         The other person to check.
    * @param  gender
    *         The gender to be assumed for this person.
    * @return True if the other person is not effective.
    *       | if (other == null)
    *       |   then result == true
    *         Otherwise, false if this person or the other person
    *         is terminated.
    *       | else if ( this.isTerminated() || other.isTerminated() )
    *       |   then result == false
    *         Otherwise, true if and only if the given gender is a valid
    *         gender for a person and differs from the gender of the
    *         other person.
    *       | else
    *       |   result ==
    *       |     isValidGender(gender) &&
    *       |     (gender != other.getGender())
    * @note   We must add the gender as an extra argument to this method,
    *         because we need the method in the constructor at a time
    *         the gender of the new person is not yet registered.
    */
    @Raw
    public boolean canHaveAsSpouse(@Raw Person other, Gender gender) {
        if (other == null)
        return true;
        else if ((other.isTerminated()) || (this.isTerminated()))
        return false;
        else
        return isValidGender(gender) && (gender != other.getGender());
    }

    /**
    * Check whether this person has a proper spouse.
    *
    * @return True if and only if this person can have its spouse
    *         as its spouse in view of its current gender, and if
    *         that spouse, if it is effective, in turn references
    *         this person as its spouse.
    *       | result ==
    *       |   canHaveAsSpouse(getSpouse(),getGender()) &&
    *       |   ( (getSpouse() == null) ||
    *       |     (getSpouse().getSpouse() == this) )
    */
    @Raw
    public boolean hasProperSpouse() {
        return canHaveAsSpouse(getSpouse(), getGender())
        && ((getSpouse() == null) || (getSpouse().getSpouse() == this));
    }

    /**
    * Check whether this person is married.
    *
    * @return True if this person references an effective spouse;
    *         false otherwise.
    *       | result == (getSpouse() != null)
    */
    @Raw
    public boolean isMarried() {
        return getSpouse() != null;
    }

    /**
    * Register a marriage between this person and the given partner.
    *
    * @param  partner
    *         The new spouse for this person.
    * @post   The given partner is registered as the spouse of this person.
    *       | new.getSpouse() == partner
    * @post   This person is registered as the spouse of the given partner.
    *       | (new partner).getSpouse() == this
    * @throws IllegalPartnerException
    *         The given partner is not effective, or this person cannot
    *         have the given partner as its spouse in view of its current
    *         gender.
    *       | (partner == null) ||
    *       | (! this.canHaveAsSpouse(partner,this.getGender()))
    * @throws IllegalStateException
    *         This person is already married, or the given partner is an
    *         effective, married person.
    *       | this.isMarried() ||
    *       | ( (partner != null) && partner.isMarried())
    */
    public void marry(Person partner) throws IllegalPartnerException,
    IllegalStateException {
        if ((partner == null) || (!canHaveAsSpouse(partner, this.getGender())))
        throw new IllegalPartnerException(this, partner);
        if (this.isMarried() || partner.isMarried())
        throw new IllegalStateException("Married persons cannot marry");
        setSpouse(partner);
        partner.setSpouse(this);
    }

    /**
    * Register a divorce between this person and its partner, if any.
    *
    * @post   This person is no longer married.
    *	     | ! new.isMarried()
    * @post   The former spouse of this person, if any, is no longer
    *		   married.
    *       | if (this.isMarried())
    *       |   then ! (new (this.getSpouse())).isMarried())
    */
    public void divorce() {
        try {
            getSpouse().setSpouse(null);
            this.setSpouse(null);
        } catch (NullPointerException exc) {
            assert (!this.isMarried());
        }
    }

    /**
    * Register the given person as the spouse of this person.
    *
    * @param  person
    *         The person to be registered as the spouse of this person.
    * @pre    This person must be able to have the given person as
    *         its spouse, in view of its current gender.
    *       | canHaveAsSpouse(person,getGender())
    * @post   The spouse of this person is the same as the given person.
    *       | new.getSpouse() == person
    */
    @Raw
    private void setSpouse(@Raw Person person) {
        assert canHaveAsSpouse(person, getGender());
        this.spouse = person;
    }

    /**
    * Variable referencing the spouse of this person.
    */
    private Person spouse = null;

}
{% endhighlight %}
