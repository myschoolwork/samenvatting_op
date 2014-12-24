---
layout: default
---

# Polymorphism

Een variabele met een superclasse als type, kan stiekem verwijzen naar een subclasse. In het voorbeeld van de `Ownable` kan je een variabele van het type `Ownable` laten verwijzen naar bijvoorbeeld een `Painting`. Het statische type van de variabele is dan `Ownable`, maar het dynamische is `Painting` (het dynamische kan ook veranderen als we bv later beslissen aan die `Ownable` variabele nu een `Dog` toe te kennen, vandaar dynamisch).

Je kan dus enkel een object van een specifieker type toekennen aan een variabele van een algemener type. Om het omgekeerde te doen moet je expliciet casten.

{% highlight java %}
//Ownable has .value
Ownable ding = new Painting(...);
int val = ding.value; //OK
string title = ding.title; //NOT OK, title is a value of Painting, not Ownable

Painting painting = ding; //NOT OK
Painting painting = (Painting)ding; //OK
{% endhighlight %}

Polymorphisme kan ook tussen primitieve types, bv een `int` toekennen aan een `long`. Maar eigenlijk wordt dan de int gewoon omgezet in een long. Het statische en dynamische type van primitieve variabelen zijn dus altijd het zelfde.

De compiler gebruikt het statische type om na te gaan of een bepaalde methode kan aangeroepen worden op het object.

<!--more-->

## f(SuperClass) vs f(SubClass)

Als een classe zowel de functie `f(SuperClass)` als `f(SubClass)` heeft, en deze wordt aageroepen, dan bepaald het statische type van de variabele die je er aan mee geeft welke van de twee gebruikt zal worden.

Dus als je de functies `f(Ownable par)` en `f(Dog par)` hebt, en je doet het volgende:

{% highlight java %}
Ownable var = new Dog();
f(var);
{% endhighlight %}

dan zal de functie `f(Ownable par)` gebruikt worden, zelfs al zit er eigenlijk een `Dog` achter.

## Reflection

Classe zijn eigenlijk ook objecten met als supertype Class. Zo kan je at runtime op eender welk object de methode `getClass()` oproepen om het Class object er van te krijgen. Dit wordt gebruikt in het 3de voorbeeldje hier onder.

## Voorbeeldjes

Enkele methods uit de classe Person.java die verschillende manieren van checken of een betpaalde variabele eigenlijk naar een ander soort subclasse verwijst:

{% highlight java %}
// The methods listed below illustrate different concepts offered in Java
// to retrieve information concerning the class to which an object belongs.
// None of the concepts is superior, meaning that each of the methods
// can be worked out just as well using the other concepts.
// In general, one must be very careful to use these concepts. As it will
// be discussed in later sessions, explicitly asking for the class to which
// an object belongs sometimes has a negative impact on the adaptability
// of software systems. However, the definitions of the methods listed
// below are fully acceptable.

/**
* Return the total amount of food needed to feed all dogs
* owned by this person during the given number of days.
*
* @param  days
*         The given number of days.
* @return The total amount of food needed to feed all dogs
*         owned by this person during the given number of days.
*       | let
*       |   myDogs = set(dog: Dog | hasAsOwning(dog))
*       | in
*       |   result == sum({dog in myDogs : dog.getFoodAmount()})*days
* @throws IllegalStateException
*         This person is already terminated.
*       | isTerminated()
* @throws IllegalArgumentException
*         The given number of days is negative.
*       | days < 0
*/
public BigInteger getTotalFoodAmount(int days)
throws IllegalArgumentException, IllegalStateException {
    if (isTerminated())
    throw new IllegalStateException("Person already terminated!");
    if (days < 0)
    throw new IllegalArgumentException("Negative number of days!");
    BigInteger totalDailyAmount = BigInteger.ZERO;
    for (Ownable owning : ownings)
    try {
        // At this point, we use a type cast to verify whether the given owning
        // is a dog. A ClassCastException is thrown, if the given owning turns
        // out not to be dog (or an instance of a subclass of Dog).
        Dog currentDog = (Dog) owning;
        BigInteger currentDailyFoodAmount = BigInteger
            .valueOf(currentDog.getDailyFoodAmount());
        totalDailyAmount = totalDailyAmount.add(currentDailyFoodAmount);
    } catch (ClassCastException exc) {
        assert (!(owning instanceof Dog));
    }
    return totalDailyAmount.multiply(BigInteger.valueOf(days));
}

/**
* Return the car with the largest motor volume owned by this person.
*
* @return The resulting car is owned by this person.
*       | hasAsOwning(result)
* @return No other car owned by this person has a higher motor volume
*         than the resulting car
*       | for each car in Car:
*       |   if (hasAsOwning(car))
*       |     then result.getMotorVolume() >= car.getMotorVolume()
* @throws IllegalStateException
*         This person is already terminated.
*       | isTerminated()
* @throws NoSuchElementException
*         This person does not own any car.
*       | for each car in Car: (! hasAsOwning(car)) )
*/
public Car getMostPowerfulCar() throws NoSuchElementException,
IllegalStateException {
    if (isTerminated())
    throw new IllegalStateException("Person already terminated!");
    Car result = null;
    for (Ownable owning : ownings)
    // At this point, we use the operator instanceof, which checks whether the
    // object at its left-hand side belongs to the class at its right-hand side.
    // The null reference does not belong to any class, and an object of a class
    // is also an instance of its superclass.
    if (owning instanceof Car) {
        Car currentCar = (Car) owning;
        if ((result == null)
        || (result.getMotorVolume() < currentCar
        .getMotorVolume()))
        result = currentCar;
    }
    if (result == null)
    throw new NoSuchElementException("Person without cars!");
    return result;
}

/**
* Return a painting owned by this person and painted by the given painter.
*
* @param  painter
*		   The painter to search for.
* @return The resulting painting is owned by this person and painted
*		   by the given painter.
*       |    hasAsOwning(result)
*       | && (result.getPainter() == painter)
* @throws IllegalStateException
* 		   This person is already terminated.
*       | isTerminated()
* @throws NoSuchElementException
*		   This person does not own a painting by the given painter.
*       | for each painting in Painting:
*       |   (painting.getPainter() != painter) ||
*       |   (! hasAsOwning(painting)) )
*/
public Painting getPaintingBy(Person painter)
throws NoSuchElementException, IllegalStateException {
    if (isTerminated())
        throw new IllegalStateException("Person already terminated!");
    for (Ownable owning : ownings) {
        // At this point, we use some facilities of reflection offered by Java.
        // In Java, classes themselves are objects of the predefined class Class.
        // The expression ClassName.class returns a reference to the object representing
        // that class; the method getClass() returns a reference to the class to
        // which the given object belongs.
        // In this version it is important to use the method isAssignableFrom. If we
        // had just written ==, an object of a subclass of the class Painting would not
        // be taken into account then.
        if (Painting.class.isAssignableFrom(owning.getClass())) {
            Painting currentPainting = (Painting) owning;
            if (currentPainting.getPainter() == painter)
            return currentPainting;
        }
    }
    throw new NoSuchElementException(
        "Person without paintings of given painter!");
}
{% endhighlight %}
