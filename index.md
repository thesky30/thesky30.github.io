---
layout: homepage
---

<section id="about" class="page-section">
  <h2 class="section-title">About Me</h2>

   <p>我是香港大学的一名经济学硕士研究生，本科毕业于香港中文大学（深圳），主修金融学，辅修经济学。我曾在睿璞投资、易方达基金、国寿资产、嘉实基金、广发证券等头部买/卖方机构从事二级市场研究相关实习，累计实习时长超过两年。</p>

  <p>我的研究兴趣是 AI 全产业链，重点关注光通信、晶圆代工、存储等方向，重视技术原理及产业链上下游验证。行业研究、公司研究与调研札记见 Blog 或公众号「Steven 的小宇宙」。</p>

  <p>我也关注量化投资，主要方向为量化选股，涵盖因子研究、策略复现和回测分析，相关内容见 Blog。<p>
  
  <p>在研究工具与工程实践上，我运用codex/claude code提高主观与量化投研效率。相关实践见Blog链接。</p>


<section id="blog" class="page-section">
  <h2 class="section-title">Blog</h2>

  {% assign latest = site.posts | slice: 0, 3 %}
  {% if latest.size == 0 %}
  <p><em>No posts yet. Add one under <code>_posts/</code>.</em></p>
  {% else %}
  <ul class="post-list">
    {% for post in latest %}
    <li>
      <a class="post-title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <p class="post-meta">{{ post.date | date: "%Y-%m-%d" }}{% if post.category %} · {{ post.category }}{% endif %}</p>
      {% if post.description %}<p class="post-excerpt">{{ post.description }}</p>{% endif %}
    </li>
    {% endfor %}
  </ul>
  {% endif %}

  <a class="view-all" href="{{ '/blog/' | relative_url }}">View all posts →</a>
</section>

<section id="projects" class="page-section">
  <h2 class="section-title">Projects</h2>

  {% if site.data.projects and site.data.projects.size > 0 %}
  <ol class="projects">
    {% for project in site.data.projects %}
    <li>
      <p class="project-title">{{ project.title }}</p>
      {% if project.role %}<p class="project-role">{{ project.role }}</p>{% endif %}
      {% if project.desc %}<p class="project-desc">{{ project.desc }}</p>{% endif %}
      {% if project.links and project.links.size > 0 %}
      <p class="project-links">
        {% for link in project.links %}<a href="{{ link.href }}">{{ link.label }}</a>{% endfor %}
      </p>
      {% endif %}
    </li>
    {% endfor %}
  </ol>
  {% else %}
  <p><em>No projects yet. Edit <code>_data/projects.yml</code> to add some.</em></p>
  {% endif %}
</section>

<section id="works" class="page-section">
  <h2 class="section-title">Models & Databases</h2>

  {% if site.data.works and site.data.works.size > 0 %}
  <ol class="works-list">
    {% for w in site.data.works %}
    <li>
      <p class="work-head">
        {% assign primary = w.links | first %}
        {% if primary %}<a class="work-title" href="{{ primary.href }}">{{ w.title }}</a>{% else %}<span class="work-title">{{ w.title }}</span>{% endif %}
        {% if w.type %}<span class="work-type">{{ w.type }}</span>{% endif %}
      </p>
      {% if w.desc %}<p class="work-desc">{{ w.desc }}</p>{% endif %}
      {% if w.links and w.links.size > 0 %}
      <p class="work-links">
        {% for link in w.links %}<a href="{{ link.href }}">{{ link.label }}</a>{% endfor %}
      </p>
      {% endif %}
    </li>
    {% endfor %}
  </ol>
  {% else %}
  <p><em>No works yet. Edit <code>_data/works.yml</code> to add some.</em></p>
  {% endif %}
</section>
