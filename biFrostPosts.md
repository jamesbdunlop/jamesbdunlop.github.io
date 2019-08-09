---
layout: default
title: "Maya-BiFrost"
---
<div class="body">
  <ul class="post-list">
    {% for post in site.posts %}
        {% if post.isPost == true and post.category == "bifrost" %}
          <li>
            {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
            <span class="post-meta">{{ post.date | date: date_format }}</span>
            <h4>
              <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
              {{ post.description }}
            </h4>
          </li>
        {% endif %}
    {% endfor %}
  </ul>
</div>
