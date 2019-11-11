---
layout: default
title: "JamesDunlop-Homepage"
info: info.md
---
<div class="body">
  {% for post in site.about %}
    {{ post.content }}
  {% endfor %}

  <center><h2>Recent Posts</h2></center>
  <ul class="post-list">
    {% for post in site.posts %}
        {% if post.isPost == true%}
          <li>
            {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
            <span class="post-meta">{{ post.date | date: date_format }}</span>
            {% if post.category == "rigACube" %}
                <h2>
                  <a class="post-link" href="{{ post.url | relative_url }}">RaC101 {{ post.title | escape }}</a>
                </h2>
            {% else %}
                <h2>
                  <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
                </h2>
            {% endif %}
          </li>
        {% endif %}
    {% endfor %}
  </ul>
</div>
