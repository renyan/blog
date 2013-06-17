---
layout: page
title: 仁言利博
tagline: 科技 生活
---
{% include JB/setup %}
    
<!-- ## Blog Posts -->

<!-- Here's a my blog list.

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## To-Do -->

{% for post in site.posts limit:2 %}
<div class="post">
  <div class="top">
    <time datetime="{{ post.date | xmlschema }}">{{ post.date | date: "%d %b" }}</time>
    <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
  </div>
  <div class="content">
    {{ post.content}}
  </div>
  <div class="bottom">
    <span>{{post.date | date: "%A %D"}}</span>
  </div>
</div>
{% endfor %}

<div class="post index archive">
  <div class="top">
    <h2>Archive</h2>
  </div>
  <div class="content">
    <ol id="archive">
      {% for post in site.posts offset:2 %}
      <li>
        <time datetime="{{ post.date | xmlschema }}">{{ post.date | date: "%Y-%m-%d" }}</time>
        <p><a href="{{ post.url }}">{{ post.title }}</a></p>
      </li>
      {% endfor %}
    </ol>
  </div>
  <div class="bottom"></div>
</div>