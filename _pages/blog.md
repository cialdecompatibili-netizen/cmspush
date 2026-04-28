---
layout: blog-custom
title: Blog
permalink: /articoli/
author_profile: false
classes: wide
---

<style>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;600;700&family=Inter:wght@300;400;500;600&display=swap');

.blog-wrap {
  max-width: 860px;
  margin: 0 auto;
  padding: 3rem 1.5rem;
  background: #fff;
}

.blog-header {
  text-align: center;
  padding-bottom: 2.5rem;
  margin-bottom: 0.5rem;
  border-bottom: 1px solid #111;
}

.blog-header h1 {
  font-family: 'Playfair Display', Georgia, serif;
  font-size: 2.6rem;
  font-weight: 700;
  letter-spacing: 0.04em;
  color: #111;
  margin: 0 0 0.4rem;
}

.blog-header p {
  font-family: 'Inter', sans-serif;
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: #aaa;
  margin: 0;
}

.blog-list { list-style: none; padding: 0; margin: 0; }

.blog-item {
  display: flex;
  gap: 2.6rem;
  align-items: flex-start;
  padding: 3rem 0;
  border-bottom: 1px solid #e8e8e8;
  transition: opacity 0.25s;
}

.blog-item:last-child { border-bottom: none; }
.blog-item:hover { opacity: 0.82; }

.blog-img {
  flex-shrink: 0;
  width: 240px;
  height: 170px;
  overflow: hidden;
  background: #f5f5f5;
}

.blog-img img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: transform 0.55s ease;
}

.blog-item:hover .blog-img img { transform: scale(1.05); }

.blog-img-empty {
  width: 100%;
  height: 100%;
  background: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 22px;
  color: #ccc;
}

.blog-body {
  flex: 1;
  min-width: 0;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  padding-top: 0.15rem;
}

.blog-cats { display: flex; gap: 8px; flex-wrap: wrap; }

.blog-cat {
  font-family: 'Inter', sans-serif;
  font-size: 9px;
  font-weight: 600;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: #111;
  border: 1px solid #111;
  padding: 3px 11px;
  text-decoration: none;
  transition: background 0.2s, color 0.2s;
}

.blog-cat:hover { background: #111; color: #fff; }

.blog-title {
  font-family: 'Playfair Display', Georgia, serif;
  font-size: 1.38rem;
  font-weight: 700;
  line-height: 1.4;
  color: #111;
  text-decoration: none;
  display: block;
  letter-spacing: 0.01em;
}

.blog-title:hover { color: #444; }

.blog-excerpt {
  font-family: 'Inter', sans-serif;
  font-size: 13.5px;
  font-weight: 300;
  color: #888;
  line-height: 1.9;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.blog-meta {
  font-family: 'Inter', sans-serif;
  font-size: 11px;
  letter-spacing: 0.07em;
  color: #bbb;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.blog-meta-sep { color: #ddd; }

.blog-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-top: 0.3rem;
  padding-top: 0.9rem;
  border-top: 1px solid #efefef;
}

.blog-tags { display: flex; gap: 8px; flex-wrap: wrap; }

.blog-tag {
  font-family: 'Inter', sans-serif;
  font-size: 10px;
  letter-spacing: 0.07em;
  color: #ccc;
  text-decoration: none;
}

.blog-tag:hover { color: #111; }

.blog-readmore {
  font-family: 'Inter', sans-serif;
  font-size: 9.5px;
  font-weight: 600;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: #111;
  text-decoration: none;
  border-bottom: 1px solid #111;
  padding-bottom: 1px;
  white-space: nowrap;
  transition: opacity 0.2s;
}

.blog-readmore:hover { opacity: 0.4; }

@media (max-width: 640px) {
  .blog-item { flex-direction: column; gap: 1.2rem; }
  .blog-img { width: 100%; height: 210px; }
  .blog-title { font-size: 1.15rem; }
  .blog-footer { flex-direction: column; align-items: flex-start; }
  .blog-header h1 { font-size: 2rem; }
}
</style>

<div class="blog-wrap">

<div class="blog-header">
  <h1>Articoli</h1>
  <p>Idee, storie e riflessioni</p>
</div>

<ul class="blog-list">
  {% for post in site.posts %}
  <li class="blog-item">

    <div class="blog-img">
      {% if post.header.teaser %}
        <img src="{{ post.header.teaser }}" alt="{{ post.title }}">
      {% else %}
        <div class="blog-img-empty">✦</div>
      {% endif %}
    </div>

    <div class="blog-body">
      {% if post.categories.size > 0 %}
      <div class="blog-cats">
        {% for cat in post.categories limit:2 %}
        <a href="/categories/#{{ cat | slugify }}" class="blog-cat">{{ cat }}</a>
        {% endfor %}
      </div>
      {% endif %}

      <a href="{{ post.url }}" class="blog-title">{{ post.title }}</a>
      <div class="blog-excerpt">{{ post.excerpt | strip_html | truncate: 200 }}</div>

      <div class="blog-meta">
        <time>{{ post.date | date: "%d %b %Y" }}</time>
        {% if post.author %}<span class="blog-meta-sep">·</span><span>{{ post.author }}</span>{% endif %}
      </div>

      <div class="blog-footer">
        {% if post.tags.size > 0 %}
        <div class="blog-tags">
          {% for tag in post.tags limit:3 %}
          <a href="/tags/#{{ tag | slugify }}" class="blog-tag"># {{ tag }}</a>
          {% endfor %}
        </div>
        {% endif %}
        <a href="{{ post.url }}" class="blog-readmore">Leggi tutto &rarr;</a>
      </div>
    </div>

  </li>
  {% endfor %}
</ul>

</div>
