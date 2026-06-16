---
title: "Categoria"
layout: single
permalink: /categoria/
author_profile: false
classes: wide
---

<style>
#page-title{display:none}
.cat-header{margin-bottom:1.5em}
.cat-header-name{font-size:1.8em;font-weight:800;color:#1a1a2e;margin:0 0 .3em}
.cat-header-desc{font-size:14px;color:#888;margin:0}
.cat-back{font-size:13px;color:#6c63ff;text-decoration:none;display:inline-block;margin-bottom:1.2em}
.cat-back:hover{text-decoration:underline}
.post-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:1.4em}
.post-card{background:#fff;border:1.5px solid #eee;border-radius:12px;padding:1.2em 1.4em;text-decoration:none;color:inherit;display:block;transition:box-shadow .15s,border-color .15s}
.post-card:hover{border-color:#6c63ff;box-shadow:0 4px 16px rgba(108,99,255,.10);text-decoration:none}
.post-card-date{font-size:11px;color:#bbb;margin-bottom:.4em}
.post-card-title{font-size:1em;font-weight:700;color:#1a1a2e;margin-bottom:.4em;line-height:1.4}
.post-card-excerpt{font-size:13px;color:#888;line-height:1.5}
.cat-empty{color:#bbb;font-size:14px;padding:2em 0;text-align:center}
</style>

<a class="cat-back" href="{{ '/categories/' | relative_url }}">← Tutte le categorie</a>
<div id="cat-container"><div class="cat-empty">⏳ Caricamento...</div></div>

<script>
const BASE_URL = '{{ site.baseurl }}';
const SITE_POSTS = [
  {% for post in site.posts %}
  {
    title: {{ post.title | jsonify }},
    url: "{{ post.url | relative_url }}",
    date: "{{ post.date | date: '%d %b %Y' }}",
    excerpt: {{ post.excerpt | strip_html | truncate: 100 | jsonify }},
    categories: {{ post.categories | jsonify }}
  }{% unless forloop.last %},{% endunless %}
  {% endfor %}
];

(function(){
  const params = new URLSearchParams(window.location.search);
  const cat = params.get('cat') || '';
  const container = document.getElementById('cat-container');

  if(!cat){ container.innerHTML = '<div class="cat-empty">Nessuna categoria specificata.</div>'; return; }

  // Cerca descrizione categoria da data
  const CAT_DATA = {{ site.data.categorie | jsonify }};
  const catInfo = (CAT_DATA||[]).find(c => c.nome === cat) || {};

  const posts = SITE_POSTS.filter(p => p.categories && p.categories.includes(cat));

  document.title = cat + ' — Categorie';
  document.querySelector('.cat-back') && (document.querySelector('.cat-back').textContent = '← Tutte le categorie');

  let html = `<div class="cat-header">
    <div class="cat-header-name">${cat}</div>
    ${catInfo.descrizione ? `<div class="cat-header-desc">${catInfo.descrizione.replace(/<[^>]*>/g,'')}</div>` : ''}
  </div>`;

  if(!posts.length){
    html += '<div class="cat-empty">Nessun articolo in questa categoria.</div>';
  } else {
    html += '<div class="post-grid">';
    posts.forEach(p => {
      html += `<a class="post-card" href="${p.url}">
        <div class="post-card-date">${p.date}</div>
        <div class="post-card-title">${p.title}</div>
        <div class="post-card-excerpt">${p.excerpt}</div>
      </a>`;
    });
    html += '</div>';
  }

  container.innerHTML = html;
})();
</script>
