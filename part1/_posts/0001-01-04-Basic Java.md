---
layout: default
---

# Basic Java

## Variables

Instance variables: elk object heeft zijn eigen versie.

Statis variables: gedeeld door alle objecten van de zelfde class.

## Encapsulation

Best practice: maak geen publieke variabelen, toon ze met een *getter* and maak het mogelijk ze te veranderen door een *setter*. Dit zorgt er voor dat je gemakkelijker later de achterliggende variabelen kan aanpassen als je dat wil: *better adaptable software*.

Een setter is een mutator, een getter een inspector. Voor immutable variabelen (krijgen de qualificatie final) is er geen setter, deze worden één keer een waarde geven bij het maken van het object.
