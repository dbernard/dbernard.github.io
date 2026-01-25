---
layout: astrophotography-page
title: Astrophotography
permalink: /astrophotography/
---

{% for post in site.astrophotography reversed %}
  <article>
    <h2>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </h2>
    <p class="post-meta">
      <time datetime="{{ post.date | date_to_xmlschema }}">
        {{ post.date | date: "%b %-d, %Y" }}
      </time>
    </p>
    {{ post.excerpt }}
  </article>
{% endfor %}
