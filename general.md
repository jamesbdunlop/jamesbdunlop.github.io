---
layout: default
title: "General Maya Stuff"
---
<div class="body">
  <ul class="post-list">
    {% for post in site.posts reversed %}
        {% if post.isPost == true and post.category == "generalMaya" %}
          <li>
            {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
            <span class="post-meta">{{ post.date | date: date_format }}</span>
            <h4>
              <a class="post-link" href="{{ post.url }}">generalMaya- {{ post.title }}</a>
              <p>{{ post.description }}</p>
            </h4>
          </li>
        {% endif %}
    {% endfor %}
  </ul>
</div>
