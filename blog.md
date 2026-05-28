---
layout: default
title: Blog
permalink: /blog/
---

<section class="page-section">
  <h2 class="section-title">Blog</h2>

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
  <p><em>No posts yet. Add one under <code>_posts/YYYY-MM-DD-title.md</code>.</em></p>
  {% else %}
  <ul class="post-list" id="post-list">
    {% for post in site.posts %}
    <li data-post-cat="{{ post.category }}" data-post-tags="{{ post.tags | join: '|' }}">
      <a class="post-title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <p class="post-meta">{{ post.date | date: "%Y-%m-%d" }}{% if post.category %} · {{ post.category }}{% endif %}{% if post.tags and post.tags.size > 0 %} · {{ post.tags | join: ", " }}{% endif %}</p>
      {% if post.description %}<p class="post-excerpt">{{ post.description }}</p>{% endif %}
    </li>
    {% endfor %}
  </ul>
  <p class="post-list-empty" hidden><em>No posts match this filter.</em></p>
  {% endif %}
</section>

<script>
(function () {
  var catChips = document.querySelectorAll('.cat-chip');
  var subGroups = document.querySelectorAll('.sub-chips');
  var posts = document.querySelectorAll('#post-list > li');
  var emptyMsg = document.querySelector('.post-list-empty');
  if (!catChips.length || !posts.length) return;

  var state = { cat: '__all', sub: '__all' };

  function applyState() {
    // Show the matching sub-chips group (or none if All)
    subGroups.forEach(function (g) {
      g.hidden = (state.cat === '__all' || g.dataset.catGroup !== state.cat);
    });
    // Active state for cat chips
    catChips.forEach(function (c) {
      c.classList.toggle('is-active', c.dataset.cat === state.cat);
    });
    // Active state for sub chips (only within the visible group)
    document.querySelectorAll('.sub-chip').forEach(function (c) {
      c.classList.toggle('is-active', c.dataset.sub === state.sub);
    });
    // Filter posts
    var visible = 0;
    posts.forEach(function (li) {
      var pCat = li.dataset.postCat || '';
      var pTags = (li.dataset.postTags || '').split('|').filter(Boolean);
      var catOk = (state.cat === '__all') || pCat === state.cat;
      var subOk = (state.sub === '__all') || pTags.indexOf(state.sub) >= 0;
      var ok = catOk && subOk;
      li.hidden = !ok;
      if (ok) visible++;
    });
    if (emptyMsg) emptyMsg.hidden = visible > 0;
  }

  function parseHash() {
    var h = location.hash || '';
    var mc = h.match(/cat=([^&]+)/);
    var ms = h.match(/sub=([^&]+)/);
    state.cat = mc ? decodeURIComponent(mc[1]) : '__all';
    state.sub = ms ? decodeURIComponent(ms[1]) : '__all';
    // Sanity: if sub set but cat is All, drop sub
    if (state.cat === '__all') state.sub = '__all';
  }

  function writeHash() {
    var parts = [];
    if (state.cat !== '__all') parts.push('cat=' + encodeURIComponent(state.cat));
    if (state.sub !== '__all') parts.push('sub=' + encodeURIComponent(state.sub));
    history.replaceState(null, '', location.pathname + (parts.length ? '#' + parts.join('&') : ''));
  }

  parseHash();
  applyState();

  catChips.forEach(function (c) {
    c.addEventListener('click', function (e) {
      e.preventDefault();
      var cat = c.dataset.cat;
      // Re-click active non-All cat = deselect
      if (state.cat === cat && cat !== '__all') cat = '__all';
      state.cat = cat;
      state.sub = '__all'; // Always reset sub when changing cat
      writeHash();
      applyState();
    });
  });

  document.querySelectorAll('.sub-chip').forEach(function (c) {
    c.addEventListener('click', function (e) {
      e.preventDefault();
      var sub = c.dataset.sub;
      // Re-click active non-All sub = deselect to "all in this cat"
      if (state.sub === sub && sub !== '__all') sub = '__all';
      state.sub = sub;
      writeHash();
      applyState();
    });
  });

  window.addEventListener('hashchange', function () { parseHash(); applyState(); });
})();
</script>
