---
layout: single
title: "Categoria"
permalink: /shop/categoria/abbigliamento/
classes: wide
author_profile: false
shop_category: abbigliamento
shop_category_name: Abbigliamento
---

<style>
.shop-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:1.5em;margin-top:1.5em}
.prod-card{background:white;border:1.5px solid #eee;border-radius:12px;padding:1.5em;text-decoration:none;color:#1a1a2e;transition:all .2s;display:block}
.prod-card:hover{border-color:#6c63ff;box-shadow:0 4px 16px rgba(108,99,255,.12);transform:translateY(-2px)}
.prod-card h3{font-size:1em;font-weight:700;margin:0 0 .4em}
.prod-card .price{font-size:1.1em;font-weight:700;color:#6c63ff}
</style>

<p style="margin-bottom:.5em"><a href="{{ '/shop/' | relative_url }}" style="color:#6c63ff;font-size:13px;text-decoration:none">← Torna allo Shop</a></p>
<p style="color:#666;font-size:14px">Prodotti nella categoria <strong>{{ page.shop_category_name }}</strong></p>

<div class="shop-grid">
{% assign cat_products = site.products | where: "category", page.shop_category %}
{% for product in cat_products %}
  <a class="prod-card" href="{{ product.url | relative_url }}">
    {% if product.image %}<img src="{{ product.image }}" alt="{{ product.title }}" style="width:100%;height:140px;object-fit:cover;border-radius:8px;margin-bottom:.8em">{% endif %}
    <h3>{{ product.title }}</h3>
    <div class="price">€ {{ product.price }}</div>
    {% if product.stock %}<div style="font-size:12px;color:#27ae60;margin-top:.3em">✅ {{ product.stock }} pz</div>{% endif %}
  </a>
{% else %}
  <p style="color:#bbb">Nessun prodotto in questa categoria.</p>
{% endfor %}
</div>
