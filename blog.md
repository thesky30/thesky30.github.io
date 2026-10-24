---
layout: default
title: Blog
permalink: /blog/
---

<section class="page-section blog-index">
  <h1 class="section-title">Blog</h1>

  {% assign cats = site.data.categories %}
  {% if cats and cats.size > 0 %}
  <nav class="cat-chips" aria-label="Filter by category">
    <button type="button" class="cat-chip is-active" data-cat="__all">All <span class="chip-count">{{ site.posts.size }}</span></button>
    {% for c in cats %}
      {% assign cat_posts = site.posts | where: "category", c.name %}
      <button type="button" class="cat-chip" data-cat="{{ c.name }}">{{ c.name }} <span class="chip-count">{{ cat_posts.size }}</span></button>
    {% endfor %}
  </nav>

  {% for c in cats %}
    {% if c.subs and c.subs.size > 0 %}
    {% assign cat_posts = site.posts | where: "category", c.name %}
    <nav class="sub-chips" data-cat-group="{{ c.name }}" aria-label="{{ c.name }} 小分类" hidden>
      <button type="button" class="sub-chip is-active" data-sub="__all">全部 <span class="chip-count">{{ cat_posts.size }}</span></button>
      {% for s in c.subs %}
        {% assign sub_count = 0 %}
        {% for p in cat_posts %}{% if p.tags contains s %}{% assign sub_count = sub_count | plus: 1 %}{% endif %}{% endfor %}
        <button type="button" class="sub-chip" data-sub="{{ s }}">{{ s }} <span class="chip-count">{{ sub_count }}</span></button>
      {% endfor %}
    </nav>
    {% endif %}
  {% endfor %}
  {% endif %}

  {% if site.posts.size == 0 %}
  <p class="post-list-empty">暂无文章。</p>
  {% else %}
  <ul class="post-list" id="post-list">
    {% for post in site.posts %}
    <li data-post-cat="{{ post.category }}" data-post-tags="{{ post.tags | join: '|' }}">
      <a class="post-title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <p class="post-meta"><time datetime="{{ post.date | date: '%Y-%m-%d' }}">{{ post.date | date: '%Y-%m-%d' }}</time>{% if post.category %}<span class="category-label">{{ post.category }}</span>{% endif %}</p>
      {% if post.description %}<p class="post-excerpt">{{ post.description }}</p>{% endif %}
    </li>
    {% endfor %}
  </ul>
  <p class="post-list-empty" hidden><em>这个分类暂时还没有文章。</em></p>
  {% endif %}
</section>
