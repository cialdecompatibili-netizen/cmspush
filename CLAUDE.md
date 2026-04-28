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

## Cartella di lavoro

**Percorso locale:** `C:\Users\mirco\Desktop\cmspush`**Branch:** main **Remote:** origin → <https://github.com/cialdecompatibili-netizen/cmspush>

---

## Come pushare su GitHub

Il repo è già configurato con token nel remote. Aprire PowerShell o terminale nella cartella cmspush ed eseguire:

```powershell
cd "C:\Users\mirco\Desktop\cmspush"
git add .
git commit -m "descrizione modifica"
git push
```

Oppure con messaggio specifico per rilascio versione:

```powershell
git add .
git commit -m "Release v1.x.x - descrizione novità"
git push
```

Non servono credenziali aggiuntive — il token è già nel remote URL.

---

## REGOLA: TOKEN SCARSI — MODIFICHE MIRATE

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
├── _pages/novita.md            ← Pagina Novità → legge CHANGELOG.md automaticamente
├── _pages/ (about, blog, 404, archivi...)
├── _posts/                     ← Articoli
├── version.json                ← {"version":"x.x.x","changelog":"..."} — fonte aggiornamenti clienti
├── CHANGELOG.md                ← Storico versioni (visualizzato su /novita/)
└── index.html                  ← Stub home (layout: home)
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

## Ripresa sessione

Alla ripresa leggi in ordine:

1. Questo file ([CLAUDE.md](http://CLAUDE.md))
2. version.json (versione attuale)
3. [CHANGELOG.md](http://CHANGELOG.md) (ultime modifiche)
4. cms/admin/index.html righe finali (ultimi 80 righe) per vedere lo stato JS Poi chiedi a Mirco cosa fare — non assumere, non riscrivere.