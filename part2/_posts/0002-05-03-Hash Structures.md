# Hash Structures

Als idee zijn dit een aantal emmertjes. Elke hashcode van een object komt in een emmertje, en als dan een object gezocht wordt berekenen we de hash er van, kijken we in het corresponderende emmertje en zoeken tussen de objecten in dat emmertje het juiste.

Er kunnen meerdere objecten in het zelfde emmertje komen als hun `hash modulo #emmertjes` gelijk zijn. Als een emmertje drijgt over te lopen dan worden er meerdere emmertjes gemaakt en de objecten opnieuw verdeelt.

![emmertjes]({{ site.github.url }}/part2/images/emmertjes.png)
