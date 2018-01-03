---
layout: default
title: "JamesDunlop-Homepage"
info: info.md
---
<div class="body">
  {{content}}

  <ul class="post-list">
    {% for post in site.posts %}
        {% if post.isPost == true %}
          <li>
            {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
            <span class="post-meta">{{ post.date | date: date_format }}</span>
            <h2>
              <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
            </h2>
          </li>
        {% endif %}
    {% endfor %}
  </ul>
</div>
