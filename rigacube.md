---
layout: rigACubePost
title: "Rig A Cube"
category: rigACube
---
<div class="body">
  <ul class="post-list">
    {% for post in site.posts %}
        {% if post.isPost == true and post.category == "rigACube" %}
          <li>
            {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
            <span class="post-meta">{{ post.date | date: date_format }}</span>
            <h4>
              <a class="post-link" href="{{ post.url }}">RaC101 {{ post.title }}</a>
              <p>{{ post.description }}</p>
            </h4>
          </li>
        {% endif %}
    {% endfor %}
  </ul>
</div>
