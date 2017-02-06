---
layout: index
---


{% for post in site.posts %}
<div class="post-preview">
    <a href="{{ post.url | prepend: site.baseurl }}">
        <h1 class="post-title">
            {{ post.title }}
        </h1>
        {% if post.subtitle %}
        <h3 class="post-subtitle">
            {{ post.subtitle }}
        </h3>
        {% endif %}
        <div class="post-content-preview">
            {{ post.content | strip_html | truncate:200 }}
        </div>
    </a>
    <p class="post-meta">
        Posted by {% if post.author %}{{ post.author }}{% else %}sdygt{% endif %}    {{ post.date | date: "%Y-%-m-%-e" }}
    </p>
</div>
<hr>
{% endfor %}
