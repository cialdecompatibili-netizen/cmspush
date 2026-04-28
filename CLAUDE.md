# [CLAUDE.md](http://CLAUDE.md) — Memoria di progetto cmspush

## Cos'è questo progetto

cmspush è un CMS commerciale basato su Jekyll + GitHub Pages. Mirco lo vende ai clienti come prodotto finito. Il cliente riceve il proprio repo GitHub con Jekyll + il CMS admin già integrato. Mirco gestisce gli aggiornamenti centralmente dal repo sorgente.

---

## Repo e domini

CosaValoreRepo sorgente (sviluppo)cialdecompatibili-netizen/cmspushDominio commerciale[cmspush.com](http://cmspush.com) (da collegare al repo)Repo demo/test clientecialdecompatibili-netizen/cialdecompatibili-netizen.github.ioFile CMS admincms/admin/index.htmlFile versioneversion.jsonChangelog[CHANGELOG.md](http://CHANGELOG.md)

---

## Struttura progetto

```
cmspush/
├── cms/
│   └── admin/
│       └── index.html   ← CMS completo (~1000 righe, tutto in 1 file)
├── docs/
├── CHANGELOG.md         ← cronologia versioni (letto dalla tab Changelog)
├── version.json         ← {"version":"x.x.x","notes":"..."}
├── CLAUDE.md            ← questo file
└── README.md
```

---

## Come funziona il sistema di aggiornamento

1. Mirco modifica cms/admin/index.html nel repo sorgente
2. Mirco aggiorna version.json con nuova versione e note
3. Il cliente apre il suo CMS → tab 🔄 Aggiornamento
4. Il CMS confronta version.json locale vs remoto (repo sorgente, pubblico)
5. Se c'è nuova versione → mostra bottone "Aggiorna ora"
6. Il cliente clicca → il CMS sovrascrive cms/admin/index.html nel repo cliente via GitHub API
7. La pagina si ricarica automaticamente dopo 3 secondi

Costanti nel codice:

- CMS_SOURCE_REPO = 'cialdecompatibili-netizen/cmspush'
- CMS_SOURCE_PATH = 'cms/admin/index.html'
- CMS_VERSION_PATH = 'version.json'

---

## Le 3 funzioni speciali del CMS

### 🎨 Tab Tema

- Seleziona skin Minimal Mistakes (9 skin disponibili)
- Modifica minimal_mistakes_skin in \_config.yml via GitHub API
- Live in \~60s dopo deploy

### 📋 Tab Changelog

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

## Ripresa sessione

Alla ripresa leggi in ordine:

1. Questo file ([CLAUDE.md](http://CLAUDE.md))
2. version.json (versione attuale)
3. [CHANGELOG.md](http://CHANGELOG.md) (ultime modifiche)
4. cms/admin/index.html righe finali (ultimi 80 righe) per vedere lo stato JS Poi chiedi a Mirco cosa fare — non assumere, non riscrivere.
