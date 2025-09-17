---
layout: home
title: Home
---

# Welcome to RanFang's Personal Blog

Hello and welcome to my personal corner of the internet! This is where I share my thoughts, experiences, and insights on various topics that interest me.

## Latest Posts

<div class="post-list">
  {% for post in site.posts limit:5 %}
    <article class="post-preview">
      <h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
      <p class="post-meta">{{ post.date | date: "%B %d, %Y" }}</p>
      <p>{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
      <a href="{{ post.url | relative_url }}" class="read-more">Read more →</a>
    </article>
  {% endfor %}
</div>

## About Me

I'm passionate about technology, learning, and sharing knowledge. Feel free to explore my blog posts and get in touch if you'd like to connect!

[View All Posts](/posts/) | [About Me](/about/)