---
layout: null
---
{% for post in site.posts limit:1 %}
    {{ post.excerpt }}
{% endfor %}