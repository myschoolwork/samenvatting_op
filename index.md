---
layout: default
title: Objectgericht Programmeren Samenvatting
---

# Object Gericht Programmeren

<ul>
    {% for post in site.posts reversed %}
    <li>
        {{ post.excerpt }}
        {% if post.excerpt < post.content %}
        <a href="{{ site.github.url }}{{ post.url }}">Meer: {{ post.title }}</a>
        {% endif %}
    </li>
    {% endfor %}
</ul>
