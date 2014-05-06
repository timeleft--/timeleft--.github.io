---
layout: bootstrap
---

{% for post in site.posts %}
<div class="blog-post">
    <h2 class="blog-post-title">{{ post.title }}</h2>
    <p class="blog-post-meta">{{ post.date | date: "%B %d, %Y" }}</p>
    {{post.content}}
</div>
{% endfor %}