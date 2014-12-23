---
layout: default
---

# Method Beschrijving

De blok commentaar boven een method die beschrijft hoe deze zal werken. De informele beschrijving komt eerst, gevolgd door een formele. De formele wordt beschreven in eerste orde logica en elke lijn begint met een verticale streep.

@post postcondities van de methode. Gebruik new. om aan te geven dat er gesproken wordt over de state **na** de beschrevn methode. Gebruik this. (mag ook weggelaten worden) om te spreken over de state **voor** de methode.

@return de postconditie van een niet-basic inspector.

@effect beschrijving van het effect van een mutator in functie van andere mutators. Ook constructors kunnen hun effect beschrijven aan de hand van andere constructoren met *this(. . .)* of *super(. . .)*. Het resultaat van **niet-basic** inspectors kan beschreven worden aan de hand van andere inspectors.

In het algemeen mag de specificatie van publieke methodes enkel andere publieke methodes gebruiken. Private methods mogen wel andere private mothodes gebruiken in hun specificatie. Je kan echter private methodes annoteren met @Model, en dan mag je ze **wel** gebruiken in de specificatie van publieke methodes.

<!--more-->

## Voorbeeld van enkele constructors uit de clock oefening uit de les:

{% highlight java %}
/**
* Initialize this new digital clock with given hours and
* given minutes.
*
* @param  hours
*         The hours for this new digital clock.
* @param  minutes
*         The minutes for this new digital clock.
* @post   The lowest possible value for the hours of this new
* 		   digital clock is equal to 0.
*       | new.getMinHours() == 0
* @post   The highest possible value for the hours of this new
* 		   digital clock is equal to 23.
*       | new.getMaxHours() == 23
* @post   If the given hours are in the range 0..23, the hours of
*         this new digital clock are equal to the given hours.
*         If the given hours exceed 23, the hours for this new
*         digital clock are equal to the given hours modulo 24.
*         If the given hours are negative, the hours for  this new
*         digital clock are equal to 0.
*       | if ( (hours >= 0) && (hours <= 23) )
*       |   then new.getHours() == hours
*       | else if (hours > 23)
*       |   then new.getHours() == (hours % 24)
*       | else if (hours < 0)
*       |   then new.getHours() == 0
* @effect The given minutes are set as the minutes of this
*         new digital clock.
*       | setMinutes(minutes)
* @note   We cannot use the mutator setHours(int) in the specification,
*         because the range for the hours of this new digital clock is
*         0..0 upon entry to this constructor. We could introduce a more
*         general method setHours(hours,minHours,maxHours) to avoid
*         a duplication of the specification of setting the hours. At this
*         stage, we prefer not to do so to keep things simple at this
*         stage  of the course.
*/
public DigitalClock(int hours, int minutes) {
    // At this point, we can invoke the mutator setHours, because the range
    // is known at this point.
    setHours(hours);
    setMinutes(minutes);
}

/**
* Initialize this new digital clock with hours, minutes and seconds
* set to their lowest possible values.
*
* @effect This new digital clock is initialized with 0 as its
*         hours and with the lowest possible value for the minutes
*         as its minutes.
*       | this(0,DigitalClock.getMinMinutes())
*/
public DigitalClock() {
    // Java does not allow us to invoke instance methods in the
    // argument list. This seems reasonable, because at that time,
    // the object is not guaranteed to have a proper state.
    // We can therefore not write this.getMinMinutes() at this
    // point.
    this(0, DigitalClock.getMinMinutes());
}
{% endhighlight %}

## Voorbeeldjes van methodes:

{% highlight java %}
/**
* Set the minutes of this digital clock to the given minutes.
*
* @param  minutes
*		   The new minutes for this digital clock.
* @post   If the given minutes are in the range of the minutes for all
* 		   digital clocks, the minutes of this digital clock are equal
* 		   to the given minutes.
*       | if ( (minutes >= getMinMinutes()) && (minutes <= getMaxMinutes()) )
*       |   then new.getMinutes() == minutes
* @post   If the given minutes exceed the highest possible value for the
* 		   minutes of all digital clocks, or the given minutes are below
*         the lowest possible value for the minutes of all digital clocks,
*         the minutes of this digital clock remain unchanged.
*       | if ( (minutes > getMaxMinutes() || (minutes < getMinMinutes()) )
*       |   then new.getMinutes() = getMinutes()
* @note   The second postcondition is not really needed. Indeed, the
*         inertia axiom for specifications states that everything that is
*         not mentioned in the specification of a method, is left untouched.
*/
public void setMinutes(int minutes) {
    if ((minutes >= getMinMinutes()) && (minutes <= getMaxMinutes()))
    this.minutes = minutes;
}

/**
* Advance the time displayed by this digital clock with 1 minute.
*
* @effect If the minutes of this digital clock have not reached their highest
*		   possible value, the minutes currently displayed by this digital
*         clock incremented by 1 are set as the new minutes displayed
*         by this digital clock.
*       | if (getMinutes() < getMaxMinutes())
*       |   then setMinutes(getMinutes()+1)
* @effect If the minutes of this digital clock have reached their highest
*		   possible value, the hours of this digital clock are advanced by
*		   1 and the minutes of this digital clock are reset to their
*         lowest possible value.
*       | if (getMinutes() == getMaxMinutes())
*       |   then (advanceHours() && resetMinutes())
*/
public void advanceMinutes() {
    if (getMinutes() < getMaxMinutes())
    setMinutes(getMinutes() + 1);
    else {
        resetMinutes();
        advanceHours();
    }
}
{% endhighlight %}
