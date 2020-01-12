---
layout: summaryPage
title: "OM2 Posts"
---
<div class="body">
  <ul class="post-list">
    {% for post in site.posts %}
        {% if post.isPost == true and post.category == "om2" %}
          <li>
            {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
            <span class="post-meta">{{ post.date | date: date_format }}</span>
            <h4>
              <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
              {{ post.description }}
            </h4>
          </li>
        {% endif %}
    {% endfor %}
  </ul>
</div>
