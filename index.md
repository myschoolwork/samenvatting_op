---
layout: default
title: Objectgericht Programmeren Samenvatting
---

# Object Gericht Programmeren

<ul>
    {% for post in site.posts reversed %}
    <li>
        {{ post.excerpt }}
        <a href="{{ post.url }}">Meer: {{ post.title }}</a>
    </li>
    {% endfor %}
</ul>
