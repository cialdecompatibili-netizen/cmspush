---
layout: single
title: "Prodotto"
permalink: /shop/prodotto/
classes: wide
author_profile: false
---

<style>
.prod-detail{max-width:800px;margin:0 auto}
.prod-detail img{width:100%;max-height:380px;object-fit:cover;border-radius:10px;margin-bottom:1.5em}
.prod-badge{display:inline-block;font-size:12px;background:#f0f0f0;padding:3px 10px;border-radius:20px;text-decoration:none;color:#666;margin-right:.5em}
.prod-price{font-size:2em;font-weight:700;color:#1a1a2e;margin:.3em 0 .8em}
.prod-stock{font-size:13px;color:#27ae60;margin-bottom:1.2em}
.prod-body{border-top:1px solid #eee;padding-top:1.2em;font-size:15px;line-height:1.8;color:#444}
</style>

<div class="prod-detail" id="prod-container">
  <p style="color:#aaa">Caricamento prodotto...</p>
</div>

<script>
const OWNER = 'cialdecompatibili-netizen';
const REPO  = 'cmspush';
const BASE  = 'https://cialdecompatibili-netizen.github.io/cmspush';

function parseFrontmatter(text) {
  const m = text.match(/^---\n([\s\S]*?)\n---/);
  const body = text.replace(/^---\n[\s\S]*?\n---\n?/, '');
  if (!m) return { _body: body };
  const obj = { _body: body };
  m[1].split('\n').forEach(line => {
    const i = line.indexOf(':');
    if (i < 0) return;
    const k = line.slice(0,i).trim();
    let v = line.slice(i+1).trim().replace(/^["']|["']$/g,'');
    obj[k] = v;
  });
  return obj;
}

async function loadProduct() {
  const params = new URLSearchParams(window.location.search);
  const slug = params.get('slug');
  const container = document.getElementById('prod-container');

  if (!slug) {
    container.innerHTML = '<p style="color:#e74c3c">Prodotto non specificato.</p>';
    return;
  }

  try {
    const r = await fetch(`https://api.github.com/repos/${OWNER}/${REPO}/contents/_products/${slug}.md`);
    if (!r.ok) throw new Error('not found');
    const f = await r.json();
    const text = atob(f.content);
    const p = parseFrontmatter(text);

    document.title = (p.title || slug) + ' — Shop';

    container.innerHTML = `
      <p style="margin-bottom:.5em"><a href="${BASE}/shop/" style="color:#6c63ff;font-size:13px;text-decoration:none">← Torna allo Shop</a></p>
      ${p.image ? `<img src="${p.image}" alt="${p.title}">` : ''}
      <div style="margin-bottom:.5em">
        ${p.category ? `<a class="prod-badge" href="${BASE}/shop/categoria/${p.category}/">${p.category}</a>` : ''}
        ${p.sku ? `<span class="prod-badge">SKU: ${p.sku}</span>` : ''}
      </div>
      <h1>${p.title || slug}</h1>
      <div class="prod-price">€ ${p.price || '—'}</div>
      ${p.stock ? `<div class="prod-stock">✅ Disponibile — ${p.stock} pezzi</div>` : ''}
      <div class="prod-body">${p._body || ''}</div>
      <div style="margin-top:2em;padding-top:1.5em;border-top:1px solid #eee">
        <a href="${BASE}/shop/" style="font-size:13px;color:#6c63ff;text-decoration:none">← Torna allo Shop</a>
      </div>`;
  } catch(e) {
    container.innerHTML = '<p style="color:#e74c3c">Prodotto non trovato.</p>';
  }
}

loadProduct();
</script>
