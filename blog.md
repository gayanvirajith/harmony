---
layout: default
title: Blog archive
---
<div class="page-content wc-container">
  <h3>Blog Archive</h3>
  <ul id="blog-posts" class="posts">
    {% for post in site.posts %}
      <li>
	      <span>{{ post.date | date_to_string }} &raquo;</span>
	      <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
      </li>
    {% endfor %}
  </ul>
</div>