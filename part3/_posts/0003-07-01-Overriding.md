---
layout: default
---

# Overriding

Je kan een method van een superclasse herdefinieren door *overriding*. In de subclasse maak je dan een functie met de zelfde naam en parameter-types en annoteert ze met @Override. Je kan steeds wel de versie in de superclasse aanspreken met `super.f(...)`.

Voor abstracte functies ben je **verplicht** een override te doen. Als een functie @Final geannoteerd staat in de superclasse dan mag je deze niet overriden. Statische methods kunnen ook niet *overridden* worden. Voor alle andere functies heb je de keuze of je het wil doen of niet.

Het dynamische type van een variabele bepaald welke versie van een methode word opgeroepen. Als een subclasse een methode override zal deze nieuwe versie aangeroepen worden, zelfs al wordt een variabele van het type van de superclasse gebruikt om deze aan te roepen. Dit gebeurt dus at-runtime.

Statische functies kunnen niet overridden worden. Er wordt dus ook bij het compileren beslist welke versie van de methode word aangeroepen, en hiervoor wordt naar het statische type gekeken.

<!--more-->

## Object

Object heeft enkele voorgedefinieerde methodes, zoals `toString`, `equals`, `clone` en `hashCode`.

toString wordt aangeraden te overriden, maar is niet verplicht.

equals checkt standaard de referenties. Dus voor objecten die de zelfde gegevens hebben (bv twee personen met de zelfde naam), override je dit niet. Als je echter twee verschillende objecten hebt die, hoewel ze hun eigen stukje geheugen hebben, het zelfde zijn, dan override je deze best naar een functie die de inhoud van de objecten na kijkt.

clone geeft standaard een shallow clone. De variabelen worden dus gecopieerd, maar de objecten waar ze mogelijks naar verwijzen niet.
