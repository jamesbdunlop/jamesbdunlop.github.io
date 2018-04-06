---
layout: default
title: "C++ Posts"
---
<div class="body">
  <ul class="post-list">
    {% for post in site.posts %}
        {% if post.isPost == true and post.category == "c++" %}
          <li>
            {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
            <span class="post-meta">{{ post.date | date: date_format }}</span>
            <h2>
              <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
              {{ post.description }}
            </h2>
          </li>
        {% endif %}
    {% endfor %}
  </ul>
</div>
