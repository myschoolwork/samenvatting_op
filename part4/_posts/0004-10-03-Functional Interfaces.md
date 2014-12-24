---
layout: default
---

# Functional Interfaces

Je kan een interface aanduiden als *functional* met @FunctionalInterface. De compiler checkt dan dat er één abstracte methode is. Deze annotatie is niet persee nodig, maar de compiler zal u vertellen als je er perongeluk te veel methodes in hebt steken. Java heeft al enkele van deze interfaces voorgedefinieerd:

* Predicate<T>, met methode `boolean test(T t)`
* Function<T,R>, met methode `R apply(T t)`
* Consumer<T>, met methode `void accept(T t)` (voert de functie uit op alles in de stream)
* Supplier<T>, met methode `T get()`
* BiFunction<T,U,R>
* en nog veel meer!
