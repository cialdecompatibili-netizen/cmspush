# [CLAUDE.md](http://CLAUDE.md) — Memoria di progetto cmspush## Cos'è questo progetto

cmspush è un CMS commerciale basato su Jekyll + GitHub Pages. Mirco lo vende ai clienti come prodotto finito. Il cliente riceve il proprio repo GitHub con Jekyll + il CMS admin già integrato. Mirco gestisce gli aggiornamenti centralmente dal repo sorgente.

---

## Repo e domini

CosaValoreRepo sorgente (sviluppo)cialdecompatibili-netizen/cmspushDominio commerciale[cmspush.com](http://cmspush.com) (da collegare al repo)Repo demo/test clientecialdecompatibili-netizen/cialdecompatibili-netizen.github.ioFile CMS admincms/admin/index.htmlFile versioneversion.jsonChangelog[CHANGELOG.md](http://CHANGELOG.md)

---
## Modello di vendita

- Repo `cmspush` resta **pubblica** (stesso modello di wallscript.9lessons.info)
- Vendita manuale: cliente paga €19 → Mirco invia zip preconfigurato via email
- La repo pubblica non è un problema: chi paga riceve un prodotto pronto, supporto e aggiornamenti
- **Motivo strategia manuale**: prima si valida se il prodotto vende, poi si automatizza. 1 progetto su 10 funziona, meglio non investire in automazioni prima di avere conferma del mercato.
- Quando/se si automatizza: Gumroad o Lemon Squeezy → zip automatico al pagamento

---

## Struttura progetto

```
cmspush/
```
├── admin/
│   └── index.html           ← CMS completo (~1000 righe, tutto in 1 file)
├── _config.yml              ← Jekyll + Minimal Mistakes (baseurl: "/cmspush" sul sito vetrina)
├── _data/navigation.yml     ← Menu sito
├── _data/categorie.json     ← Categorie CMS
├── _includes/
│   └── home-content.html    ← Landing page di vendita (hero, features, comparativa, download)
├── _layouts/home.html       ← Layout home
├── \_pages/ ← Pagine sito (about, blog, novita, 404...) ├── \_posts/ ← Articoli ├── [CHANGELOG.md](http://CHANGELOG.md) ← Storico versioni (visualizzato su /novita/) ├── version.json ← {"version":"x.x.x","changelog":"..."} — fonte aggiornamenti clienti ├── index.html ← Stub home (layout: home) ├── [CLAUDE.md](http://CLAUDE.md) ← questo file └── [README.md](http://README.md)
```

---

## Come funziona il sistema di aggiornamento

1. Mirco modifica `admin/index.html` nel repo sorgente
2. Mirco aggiorna `version.json` con nuova versione e note
3. Il cliente apre il suo CMS → tab 🔄 Aggiornamenti
4. Il CMS confronta version.json locale vs remoto (repo sorgente, pubblica)
5. Se c'è nuova versione → badge rosso ! nella sidebar + bottone "Aggiorna ora"
6. Il cliente clicca → il CMS sovrascrive `admin/index.html` nel repo cliente via GitHub API
7. La pagina si ricarica automaticamente dopo 3 secondi

Costanti JS nel CMS:

- `CMS_SOURCE_REPO = 'cialdecompatibili-netizen/cmspush'`
- `CMS_SOURCE_PATH = 'admin/index.html'`
- `CMS_VERSION_PATH = 'version.json'`

---

## Landing page di vendita

Il file `_includes/home-content.html` è la landing page del sito vetrina cmspush. Struttura:

- **Hero** — titolo, sottotitolo, 2 bottoni (Scarica v1.x €19 + Confronta versioni)
- **Cos'è cmspush** — paragrafo descrittivo
- **3 feature steps** — Scrivi e pubblica / Cambia tema / Aggiornamenti automatici
- **Tabella comparativa v1.x vs v2.x** — v1 disponibile €19, v2 in sviluppo
- **Sezione download** — CTA finale con link alla repo

```
Stile: bianco, minimale, solo testo. Grafica da definire in futuro. Il bottone download punta a: `https://github.com/cialdecompatibili-netizen/cmspush`Il prezzo attuale è: **€19 pagamento unico**.

---

## Tab ⚙️ Impostazioni CMS — campi disponibili

### 🎨 Tab Tema

- Seleziona skin Minimal Mistakes (9 skin disponibili)
- Modifica minimal_mistakes_skin in \_config.yml via GitHub API
- Live in \~60s dopo deploy
```

### 📋 Tab Changelog

```

- Legge [CHANGELOG.md](http://CHANGELOG.md) dal repo del CLIENTE (non dal sorgente)
- Render markdown minimale (titoli, liste, grassetto)
- Bottone ↻ per ricaricare

### 🔄 Tab Aggiornamento

- Legge version.json locale (../../version.json)
- Confronta con version.json nel repo sorgente (pubblico, senza token)
- Se versione diversa: mostra note + bottone Aggiorna
- Aggiorna sovrascrivendo cms/admin/index.html nel repo cliente
- Usa il token GitHub già salvato dal cliente

---

## Stack tecnico CMS

- Tutto in 1 file HTML (no dipendenze esterne, no npm, no build)
- GitHub API v3 per lettura/scrittura file
- localStorage per token GitHub
- Editor visuale con execCommand (no Quill, no librerie)
- Jekyll + Minimal Mistakes come tema sito cliente

---

## Regole chirurgiche per modifiche al codice

1. Prima leggi SOLO il range necessario: read_file con offset+length
2. Modifica con edit_block usando old_string minimo (3-5 righe di contesto)
3. MAI riscrivere l'intero file — è \~1000 righe
4. Per aggiungere funzioni JS: inseriscile PRIMA di window.onload
5. Per aggiungere tab: aggiungi voce nav E pagina HTML in 2 edit_block separati

---

## Workflow rilascio nuova versione

1. Modifica cms/admin/index.html
2. Aggiorna version.json: {"version":"X.X.X","notes":"Cosa c'è di nuovo"}
3. Aggiorna [CHANGELOG.md](http://CHANGELOG.md) con la nuova voce
4. Push su cialdecompatibili-netizen/cmspush
5. I clienti vedono l'aggiornamento disponibile automaticamente

---

## Cartella di lavoro

**Percorso locale:** `C:\Users\mirco\Desktop\cmspush`**Branch:** main **Remote:** origin → <https://github.com/cialdecompatibili-netizen/cmspush>

---

## Come pushare su GitHub

Claude può pushare direttamente usando Windows-MCP:PowerShell — NON serve che Mirco apra il terminale.

Comando push standard:
```powershell
cd "C:\Users\mirco\Desktop\cmspush"; git add .; git commit -m "descrizione modifica"; git push
```

Comando push release:
```powershell
cd "C:\Users\mirco\Desktop\cmspush"; git add .; git commit -m "Release v1.x.x - descrizione novità"; git push
```

**Regola:** dopo ogni modifica, Claude pusha autonomamente senza aspettare che Mirco lo chieda. Non servono credenziali aggiuntive — il token è già nel remote URL.

**Regola deploy:** dopo ogni push, Claude fa polling GitHub Actions API ogni 10s e scrive il risultato reale:
- `✅ Deploy completato! Live ora → [url]`
- `❌ Deploy fallito`

Script polling da usare dopo ogni push:
```powershell
$repo = "cialdecompatibili-netizen/cmspush"
$maxTentativi = 18; $intervallo = 10
Start-Sleep -Seconds 5
for ($i = 1; $i -le $maxTentativi; $i++) {
    $run = (Invoke-RestMethod "https://api.github.com/repos/$repo/actions/runs?per_page=1").workflow_runs[0]
    if ($run.status -eq "completed") {
        if ($run.conclusion -eq "success") { Write-Host "✅ Deploy completato! Live ora → https://cialdecompatibili-netizen.github.io/cmspush/" }
        else { Write-Host "❌ Deploy fallito ($($run.conclusion))" }
        break
    }
    Write-Host "⏳ Build in corso... ($($i * $intervallo)s)"; Start-Sleep -Seconds $intervallo
}
```

## Strumenti disponibili per Claude

| Strumento | Cosa fa |
|---|---|
| `Filesystem:read_multiple_files` | Legge file dal PC di Mirco |
| `Filesystem:list_directory` | Elenca cartelle dal PC di Mirco |
| `Desktop Commander:read_file` | Legge file con offset/length (file lunghi) |
| `Desktop Commander:edit_block` | Modifica chirurgica con old_string/new_string |
| `Windows-MCP:PowerShell` | Esegue comandi PowerShell/git sul PC di Mirco |

Claude usa Windows-MCP:PowerShell per git add/commit/push autonomamente.

\---## REGOLA: TOKEN SCARSI — MODIFICHE MIRATE

- Claude ha token limitati per sessione. Ogni operazione deve essere chirurgica.
- Prima di creare un file: verifica se esiste già o può essere COPIATO/SPOSTATO da altrove.
- MAI riscrivere file interi se serve solo modificare una parte — usare edit_block.
- MAI creare file che esistono già nel progetto sorgente — spostarli con PowerShell.
- Leggere solo i range necessari dei file lunghi (offset+length).
- In caso di dubbio: chiedere prima di fare.

## Struttura progetto attuale

```
cmspush/
├── admin/index.html            ← CMS completo (tab: Articoli, Pagine, Menu, Categorie, Token, Tema, Impostazioni, Aggiornamenti)
├── _config.yml                 ← Jekyll + Minimal Mistakes (skin: air)
├── _data/navigation.yml        ← Menu: Home, Blog(/articoli/), Categories, Tags, About, Novità(/novita/)
├── _data/categorie.json        ← Categorie CMS
├── _includes/home-content.html ← Contenuto home (slider + sezioni)
├── _layouts/home.html          ← Layout home con slider CSS
├── _layouts/blog-custom.html   ← Layout blog lista articoli
```

├── \_pages/novita.md ← Pagina Novità → legge [CHANGELOG.md](http://CHANGELOG.md) automaticamente ├── \_pages/ (about, blog, 404, archivi...) ├── \_posts/ ← Articoli ├── version.json ← {"version":"x.x.x","changelog":"..."} — fonte aggiornamenti clienti ├── [CHANGELOG.md](http://CHANGELOG.md) ← Storico versioni (visualizzato su /novita/) └── index.html ← Stub home (layout: home)

```

## Sistema aggiornamenti clienti

- I repo clienti puntano a questa repo come sorgente update
- version.json contiene versione + note changelog
- Tab "🔄 Aggiornamenti" nel CMS admin dei clienti:
  - All'avvio confronta version.json locale vs remoto (senza token, repo pubblica)
  - Se update disponibile: badge rosso ! nella sidebar + bottone "⬆️ Aggiorna ora"
  - Il bottone scarica admin/index.html dal sorgente e lo installa nel repo cliente via API
  - Aggiorna anche version.json locale e ricarica dopo 3 secondi
- Costanti JS: CMS_SOURCE_REPO, CMS_SOURCE_PATH, CMS_VERSION_PATH

## Workflow rilascio nuova versione

1. Modifica admin/index.html
2. Aggiorna version.json: {"version":"X.X.X","changelog":"Cosa c'è di nuovo"}
3. Aggiorna [CHANGELOG.md](http://CHANGELOG.md) (appare automaticamente su /novita/)
4. Push → i clienti vedono l'update al prossimo accesso al CMS

## Link progetto

- Repo: <https://github.com/cialdecompatibili-netizen/cmspush>
- Sito live: <https://cialdecompatibili-netizen.github.io/cmspush/>
- CMS admin (solo Mirco): <https://cialdecompatibili-netizen.github.io/cmspush/admin/>

## ⚠️ LEZIONE APPRESA: Override CSS su Minimal Mistakes

**Problema cronico:** `_includes/head/custom.html` viene iniettato SOLO sulle pagine che usano layout con `{% include head/custom.html %}`. Le pagine MM standard (categories, tags, archive, single) potrebbero non riceverlo o riceverlo dopo il CSS della gem.

**Regola fissa:** Gli override globali di MM (navbar, footer, colori) vanno in `assets/css/main.scss` — viene compilato e servito come `main.css` su **tutte** le pagine senza eccezioni. Il critical CSS inline in `head/custom.html` serve solo per eliminare il FOUC (flash prima del caricamento del CSS esterno), non come unico punto di override.

**Schema corretto:**
- `assets/css/main.scss` → override globali, valgono ovunque
- `_includes/head/custom.html` → solo stili critici per zero FOUC (duplica i più importanti da main.scss)

---



cmspush è un prodotto in vendita. Deve funzionare su **qualsiasi repo GitHub Pages** senza modifiche manuali, sia in root (`nomecliente.github.io`) che in sottocartella (`nomecliente.github.io/repo`).

### Regola assoluta
**MAI usare link interni con path assoluto hardcoded** come `href="/blog/"` o `href="/about/"`.

### Come scrivere i link in Jekyll (sempre)
| Situazione | Sintassi corretta |
|---|---|
| Link a pagina interna in `.md` o `.html` | `{{ '/blog/' | relative_url }}` |
| Link a post in Liquid | `{{ post.url | relative_url }}` |
| Link a categoria/tag | `{{ '/categories/#' | append: cat | relative_url }}` |
| Link a asset (CSS, immagini) | `{{ '/assets/img/foo.png' | relative_url }}` |
| Link esterno (GitHub, email, ecc.) | normale `href="https://..."` — ok |

### Il filtro `relative_url`
Aggiunge automaticamente il `baseurl` configurato in `_config.yml`. Se `baseurl` è vuoto (cliente root), non aggiunge nulla. Se è `/cmspush`, aggiunge `/cmspush`. Zero modifiche manuali da parte del cliente.

### File già verificati e corretti (audit completo)
- `_pages/blog.md` — post.url, categories, tags → tutti `relative_url` ✅
- `_pages/home.md` — link `/blog/` → `relative_url` ✅
- `_includes/home-content.html` — link `/blog/`, `/about/` → `relative_url` ✅
- `_data/navigation.yml` — gestito da Minimal Mistakes che aggiunge `baseurl` automaticamente ✅

### Quando aggiungi nuovi file
Prima del push, controlla sempre con:
```powershell
Select-String -Path "_pages\*.md","_layouts\*.html","_includes\*.html" -Pattern 'href="/'
```

Se escono risultati con path interni → vanno corretti con `relative_url`.

---

- Il **sito vetrina cmspush** (repo di sviluppo) ha `baseurl: "/cmspush"` nel `_config.yml` perché gira su `cialdecompatibili-netizen.github.io/cmspush`
- Il **template da vendere ai clienti** deve avere `baseurl: ""` perché i clienti usano `nomecliente.github.io` (root)
- Il campo **Base URL** è gestibile direttamente dalla tab ⚙️ Impostazioni del CMS admin (legge e scrive `_config.yml` via GitHub API)

---

## Ripresa sessione

Alla ripresa leggi in ordine:

1. Questo file ([CLAUDE.md](http://CLAUDE.md))
2. version.json (versione attuale)
3. [CHANGELOG.md](http://CHANGELOG.md) (ultime modifiche)
4. cms/admin/index.html righe finali (ultimi 80 righe) per vedere lo stato JS Poi chiedi a Mirco cosa fare — non assumere, non riscrivere.

```
```iscrivere.
```

```
```

---

## Ripresa sessione — AUTO-ISTRUZIONE OBBLIGATORIA

**REGOLA ASSOLUTA:** All'inizio di OGNI nuova sessione Claude si auto-istruisce PRIMA di fare qualsiasi cosa. Nessuna domanda a Mirco finché non hai letto tutto.

### Sequenza di boot (eseguila tutta, in ordine, senza chiedere):

1. Leggi `CLAUDE.md` (questo file)
2. Leggi `version.json` — versione attuale
3. Leggi `CHANGELOG.md` — ultime modifiche
4. Leggi ultime 80 righe di `admin/index.html` — stato JS
5. Leggi prime 30 righe di `_includes/home-content.html` — stato landing

Poi rispondi con uno **spizzato di 5 righe max** e chiedi a Mirco cosa vuole fare. Veloce, chirurgico.

---

## Fix applicati — log storico

Aggiorna questa tabella ad ogni fix. Non fare 2 volte la stessa cosa.

| Data | Fix | File | Commit |
|------|-----|------|--------|
| 2026-04-28 | Prima versione pubblica v1.0.0 | admin/index.html, version.json | Release v1.0.0 |
| 2026-04-29 | REPO dinamico detectRepo() da URL — zero hardcoded | admin/index.html, cms/admin/index.html | ef7b2cf |
| 2026-04-29 | Link topbar "Vai al sito" e "Admin" — ora JS dinamico | admin/index.html, cms/admin/index.html | ef7b2cf |
| 2026-04-29 | Tab Aggiornamenti: fallback fetch relativo + messaggio chiaro in locale | admin/index.html, cms/admin/index.html | b65b27d |
| 2026-04-29 | Rimossa doppia navbar Tailwind da home-content.html (restava solo masthead MM) | _includes/home-content.html | 898774a |
| 2026-04-29 | Critical CSS inline: footer + navbar in _includes/head/custom.html — zero FOUC | _includes/head/custom.html (nuovo), assets/css/main.scss | f9aad34 |
| 2026-04-29 | Fix navbar riga: border su .masthead (non .greedy-nav), override in main.scss globale | assets/css/main.scss, _includes/head/custom.html | 4f08179 |
| 2026-04-29 | Permalink blog /articoli/ → /blog/ + titolo H1 "Blog" | _pages/blog.md, _data/navigation.yml | cecbc81 |
| 2026-04-29 | CLAUDE.md aggiornato con Windows-MCP:PowerShell — Claude pusha autonomamente | CLAUDE.md | — |

---

## Regole operative veloci

- **Sincronizza sempre le 2 copie:** modifica `admin/index.html` → copia su `cms/admin/index.html` → push
- **Push rapido:** `cd "C:\Users\mirco\Desktop\cmspush"; git add .; git commit -m "msg"; git push`
- **Verifica push:** `git log --oneline -3`
- **MAI riscrivere file interi** — solo edit_block mirate con old_string+new_string
- **Nuove funzioni JS:** inseriscile PRIMA di `window.onload`
- **Nuova tab CMS:** 2 edit_block separati — voce nav + HTML pagina
- **Audit link:** `Select-String -Path "_pages\*.md","_layouts\*.html","_includes\*.html" -Pattern 'href="/'`

---

## Backlog (prossimi step possibili)

- [ ] Sezione pricing + one-click install sulla landing (verificare stato)
- [ ] Bozze articoli (published: false)
- [ ] Upload immagini drag & drop
- [ ] Dashboard mini contatore articoli/bozze
- [ ] Slug auto-generato dal titolo
- [ ] Pagina /demo/ CMS in sola lettura
