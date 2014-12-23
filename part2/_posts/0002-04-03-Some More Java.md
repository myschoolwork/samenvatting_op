---
layout: default
---

# Some More Java

## Garbage Collector

Draait op de achtergrond (eigen thread) en kuist alle onbereikbare objecten op. Kan expliciet aangeroepen worden met `System.gc()` maar zelfs dat verzekerd niet dat hij zal beginnen.

Een object kan `finalize()` op zichzelf aanroepen om te vragen op gekuist te worden. De GC zal echter niet altijd luisteren, en doet het alleen als het object niet meer bereikbaar is.

## Destructor

Een destructor method krijgt de naam `terminate`, er mag **maximum** een (1) destructor per classe zijn. Een destructor mag exceptions smijten.

Een classe kan ook een (1) inspector implementeren `boolean isTerminated()`. Als het object geterminate is moet het echter nog steeds voldoen aan de classe invarianten, en die invarianten of de methodes moeten hier rekening mee houden. Mutators moeten er ook rekening mee houden dat een meegegeven object terminated kan zijn.

<!--more-->

## Een voorbeeldje van de terminate functionaliteit

Deze classe is weer een versie van de Person classe met wat stuff uit gesmeten die hier niet van belang is. Het toont de terminate dingen en een functie die rekening houdt met deze terminate.

{% highlight java %}
public class Person {

    /**
    * Terminate this person, breaking the marriage in which that person
    * might be involved.
    *
    * @post    his person is terminated.
    *       | new.isTerminated()
    * @effect This person is divorced from its spouse, if any.
    *       | this.divorce()
    */
    public void terminate() {
        divorce();
        // We feel no need to introduce a setter for "isTerminated". It is
        // only used at this point, because it should not be possible to
        // bring an object back to live.
        this.isTerminated = true;
    }

    /**
    * Return a boolean indicating whether or not this person
    * is terminated.
    */
    @Basic @Raw
    public boolean isTerminated() {
        return this.isTerminated;
    }

    /**
    * Variable registering whether this person is terminated.
    */
    private boolean isTerminated = false;

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

    // ...
}
{% endhighlight %}
