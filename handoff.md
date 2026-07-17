# FERRUM — Astro Handoff Dokumentacija

Ovaj dokument služi kao primopredajni (handoff) fajl za projekat **FERRUM**, koji je uspešno transformisan iz jedne statičke HTML stranice u modularni **Astro framework** projekat.

---

## 🛠️ Tehnološki Stack

1. **Framework:** [Astro v5](https://astro.build/) (trenutni industrijski standard za brze i dizajn-orijentisane sajtove).
2. **Stilovi:** Čist Vanilla CSS, smešten u [global.css](file:///home/ivica1979/projekti/mirko-kase/src/styles/global.css).
3. **JS/TS:** TypeScript (za stabilnost tipova u klijentskim skriptama).
4. **Slike:** Sve slike su lokalizovane i nalaze se u folderu `public/images/`.

---

## 📂 Struktura Projekta

Astro projekat je organizovan na sledeći način:

```text
mirko-kase/
├── public/
│   └── images/                # Lokalizovane slike sa Unsplash-a
│       ├── hero.jpg
│       ├── manifest.jpg
│       ├── ledger-steel.jpg
│       └── ... (ostalih 7 slika)
├── src/
│   ├── components/            # Pojedinačne sekcije sajta
│   │   ├── Hero.astro         # Početni ekran sa sliderom
│   │   ├── Material.astro     # Detalji o materijalima i ledger tabela
│   │   ├── Work.astro         # Portfolio izabranih radova
│   │   └── Commission.astro   # Forma za naručivanje i footer
│   ├── layouts/
│   │   └── Layout.astro       # Glavni HTML šablon, globalni skriptovi
│   ├── pages/
│   │   └── index.astro        # Glavna stranica koja sklapa sekcije
│   └── styles/
│       └── global.css         # Svi globalni i responzivni stilovi
├── astro.config.mjs           # Astro konfiguracioni fajl
├── package.json               # Zavisnosti i skripte
├── tsconfig.json              # TypeScript konfiguracija
└── ferrum-final-v10 (2).html  # Originalni HTML (ostavljen kao backup)
```

---

## ⚙️ Pokretanje i Komande

U korenu projekta možeš izvršavati sledeće komande:

*   **Pokretanje razvojnog okruženja (Dev server):**
    ```bash
    npm run dev
    ```
    Sajt se pokreće na adresi [http://localhost:4321](http://localhost:4321) sa podrškom za *Hot Module Replacement* (sve promene se automatski osvežavaju bez reload-a).
*   **Build za produkciju:**
    ```bash
    npm run build
    ```
    Kompiluje projekat u ultra-brze, statičke HTML/CSS/JS fajlove unutar `dist/` foldera.
*   **Lokalni pregled produkcionog build-a:**
    ```bash
    npm run preview
    ```
    Pokreće server koji simulira produkciono okruženje na osnovu fajlova iz `dist/` foldera.

---

## 🎨 Ključne Karakteristike i Implementacija

1. **Učitavanje resursa (LCP):** Sve slike su sačuvane lokalno. To poboljšava brzinu učitavanja jer browser više ne mora da razrešava eksterne URL domene ka Unsplash-u.
2. **Rešavanje asinhronog učitavanja stilova (Race Condition):**
   * Svi JS skriptovi su bezbedno obmotani u `window.addEventListener('load')` unutar [Layout.astro](file:///home/ivica1979/projekti/mirko-kase/src/layouts/Layout.astro). Ovo sprečava da se animacije i `IntersectionObserver` pokrenu pre nego što Vite učita CSS u dev modu.
3. **Funkcionalnosti prenete 1:1:**
   * **Custom Cursor:** Tačka i kursor koji menja oblike na hover.
   * **Interactive Hero Slider:** Klizač koji deli sliku i tekst na vrhu stranice (drag & drop i tastatura).
   * **Scroll Reveal:** Postepeno otkrivanje slika prilikom skrolovanja (`clip-path` animacije).
   * **NFC/Folio Sidebar Tracker:** Automatsko praćenje i promena teksta u levom sidebaru na osnovu sekcije u fokusu.

---

## 🚀 Sledeći Koraci za Dalji Razvoj

Zahvaljujući prelasku na Astro, dalji razvoj je znatno lakši:
1. **Dynamic Portfolio:** Možeš kreirati kolekciju sadržaja (Content Collections) za projekte u `src/content/projects/` koristeći Markdown fajlove, čime bi se portfolio u `Work.astro` automatski generisao iz fajlova.
2. **Konekcija forme:** Forma u `Commission.astro` se može lako povezati sa API endpointom (npr. Astro API rute ili eksterni servis kao Formspree).
3. **SEO optimizacija:** SEO tagovi se mogu dinamizovati direktno kroz props parametre u `Layout.astro`.
