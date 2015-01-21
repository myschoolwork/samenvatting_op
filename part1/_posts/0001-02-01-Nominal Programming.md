---
layout: default
---

# Nominal Programming

Gebruik classe invarianten (worden aangegeven in de heading van een classe) en pre-condities.

Classe invarianten leggen restricties op de state van het object op en worden aangeven met @invar. Het zijn voorwaarden waar het object **altijd** aan moet voldoen. Methodes die het object in een inconsistente state brengen, of die toch gebruikt mogen worden wanneer het object in een inconsistente state is, worden aangeduid met @Raw en deze moeten dus geen rekeningen houden met de classe invarianten. Worden zowel formeel (logica) als informeel beschreven bovenaan in de classe.

Precondities worden op de zelfde manier gebruikt als postcondities. Ze worden aangeven met @pre.

Precondities worden nagekeken door middel van een assert.

<!--more-->

## Design by contract

Gebruikers moeten verzekeren dat objecten doorgegeven aan een methode voldoen aan hun classe invarianten. Ook alle pre-condities moeten voldaan zijn.

De programmeur moet verzekeren dat objecten in een juiste state zijn na het uitvoeren van een methode, dat ze voldoen aan hun classe invarianten. Ook alle post-condities moeten voldaan zijn na een methode.

De programmeur kan bij het implementeren van een methode er van uit gaan dat de precondities en classe invarianten voldaan zijn.

## Voorbeeldje van invarianten uit de les:

{% highlight java %}
/**
* A class of tanks for storing oil, involving a capacity and a content.
*
* @invar  The capacity of each oil tank must be a valid capacity for an
*         oil tank.
*         | isValidCapacity(getCapacity())
* @invar  The contents of each oil tank must be a valid contents for an
*         oil tank in view of its capacity.
*         | isValidContents(getContents(),getCapacity())
*
* @version  2.1
* @author   Eric Steegmans
*/
public class OilTank {
    . . .
}
{% endhighlight %}


## Voorbeeldje van preconditie en assert:

{% highlight java %}
/**
* Fill this oil tank with the given amount of oil.
*
* @param  amount
*         The amount to be added to this oil tank.
* @pre    The given amount must be positive.
*       | amount > 0
* @effect The contents of this oil tank is set to its current contents
*         incremented with the given amount of oil.
*       | setContents(getContents() + amount)
* @note   Because we use the method setContents in the specification,
*         this method has another precondition, namely
*            isValidContents(getContents()+amount,getCapacity())
*         If the sum of the contents and the amount would overflow,
*         the resulting value is guaranteed to be negative, and
*         will therefore not be accepted.
* @note   Because this is not a 'raw' method, this oil tank must
* 		   satisfy its class invariants upon entry to this method.
* 		   Upon exit, it is easy to see that this oil tank then still
* 		   satisfies its class invariants.
*/
public void fill(int amount) {
    assert amount > 0;
    setContents(getContents() + amount);
}
{% endhighlight %}
