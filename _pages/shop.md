---
layout: single
title: "Shop"
permalink: /shop/
classes: wide
author_profile: false
---

<style>
.shop-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:1.5em;margin-top:1.5em}
.shop-cat-card{background:white;border:1.5px solid #eee;border-radius:12px;padding:1.5em;text-decoration:none;color:#1a1a2e;transition:all .2s;display:block}
.shop-cat-card:hover{border-color:#6c63ff;box-shadow:0 4px 16px rgba(108,99,255,.12);transform:translateY(-2px)}
.shop-cat-card .icon{font-size:2em;margin-bottom:.5em}
.shop-cat-card h3{font-size:1em;font-weight:700;margin:0 0 .3em}
.shop-cat-card .count{font-size:12px;color:#888}
</style>

<p style="color:#666;font-size:14px">Sfoglia i prodotti per categoria</p>

<div class="shop-grid">
{% assign all_products = site.products %}
{% assign cats = site.data.shop-categorie %}
{% if cats %}
  {% for cat in cats %}
    {% assign cat_products = all_products | where: "category", cat.slug %}
    <a class="shop-cat-card" href="{{ '/shop/categoria/' | append: cat.slug | append: '/' | relative_url }}">
      <div class="icon">📦</div>
      <h3>{{ cat.nome }}</h3>
      <div class="count">{{ cat_products.size }} prodott{% if cat_products.size == 1 %}o{% else %}i{% endif %}</div>
    </a>
  {% endfor %}
{% else %}
  <p style="color:#bbb">Nessuna categoria ancora.</p>
{% endif %}
</div>

<hr style="margin:2.5em 0">

<h2 style="font-size:1.1em;font-weight:700;margin-bottom:1em">Tutti i prodotti</h2>

<div class="shop-grid">
{% for product in site.products %}
  <a class="shop-cat-card" href="{{ product.url | relative_url }}">
    {% if product.image %}<img src="{{ product.image }}" alt="{{ product.title }}" style="width:100%;height:140px;object-fit:cover;border-radius:8px;margin-bottom:.8em">{% endif %}
    <h3>{{ product.title }}</h3>
    <div style="font-size:1.1em;font-weight:700;color:#6c63ff;margin:.3em 0">€ {{ product.price }}</div>
    <div class="count">{{ product.category }}</div>
  </a>
{% else %}
  <p style="color:#bbb">Nessun prodotto ancora.</p>
{% endfor %}
</div>
