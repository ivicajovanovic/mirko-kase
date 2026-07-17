# VM MOBILE — Astro Handoff Dokumentacija

Ovaj dokument služi kao primopredajni (handoff) fajl za projekat **VM MOBILE**, koji je transformisan iz statičke prezentacije u modularni **Astro framework** sa srpskim jezikom, novim vizuelnim identitetom i fokusom na prodaju i servis fiskalnih kasa i potrošnog materijala.

---

## 🛠️ Tehnološki Stack

1. **Framework:** [Astro v5](https://astro.build/)
2. **Stilovi:** Čist Vanilla CSS, smešten u [global.css](file:///home/ivica1979/projekti/mirko-kase/src/styles/global.css).
3. **JS/TS:** TypeScript (za stabilnost tipova u klijentskim skriptama).
4. **Slike:** Sve slike su lokalizovane i nalaze se u folderu `public/images/`.

---

## 📂 Struktura Projekta

Astro projekat je organizovan na sledeći način:

```text
mirko-kase/
├── public/
│   └── images/                # Lokalizovane nove slike sa Unsplash-a
│       ├── hero.jpg           # Slika kase/terminala na Hero sekciji
│       ├── manifest.jpg       # Serviser na poslu
│       ├── ledger-steel.jpg   # Detalj beskontaktnog plaćanja
│       ├── ledger-concrete.jpg # Kancelarijski materijal (rolne)
│       ├── ledger-site.jpg    # Servisno vozilo na putu
│       └── ... (ostale detaljne slike radova)
├── src/
│   ├── components/            # Pojedinačne sekcije sajta
│   │   ├── Hero.astro         # Početni ekran sa sliderom
│   │   ├── Material.astro     # Usluge (Kase, Materijal, Teren)
│   │   ├── Work.astro         # Radovi (Prodaja/Servis, Rolne, Konsultacije)
│   │   └── Commission.astro   # Forma za naručivanje, kontakt i footer
│   ├── layouts/
│   │   └── Layout.astro       # Glavni HTML šablon, globalni skriptovi i prevodi
│   ├── pages/
│   │   └── index.astro        # Glavna stranica koja sklapa sekcije
│   └── styles/
│       └── global.css         # Svi globalni i responzivni stilovi
├── astro.config.mjs           # Astro konfiguracioni fajl
├── package.json               # Zavisnosti i skripte
├── tsconfig.json              # TypeScript konfiguracija
└── handoff.md                 # Ova dokumentacija
```

---

## ⚙️ Pokretanje i Komande

U korenu projekta možeš izvršavati sledeće komande:

*   **Pokretanje razvojnog okruženja (Dev server):**
    ```bash
    npm run dev
    ```
    Sajt se pokreće na adresi [http://localhost:4321](http://localhost:4321).
*   **Build za produkciju (Generisanje statičkog sajta):**
    ```bash
    npm run build
    ```
    Kompiluje projekat u statičke HTML/CSS/JS fajlove unutar `dist/` foldera.
*   **Lokalni pregled produkcionog build-a:**
    ```bash
    npm run preview
    ```

---

## 🎨 Specifičnosti Dizajna i Implementacije

1. **Stilovi i Layout:** Zadržan je prvobitni brutalistički stil, paleta boja (crna + signalno žuta), spacing, tipografija, animacije i struktura grida.
2. **Klijentske skripte ([Layout.astro](file:///home/ivica1979/projekti/mirko-kase/src/layouts/Layout.astro)):**
   * Sve skripte su obmotane u `load` listener da bi se izbeglo izvršavanje pre učitavanja stilova u Vite-u (izbegnuta praznina/nevidljivost slika).
   * **Custom Cursor:** Tačka od 10px prati miš i menja dimenzije na hover slika i linkova.
   * **Hero Slider Divider:** Klizač za promenu razmere slike na vrhu (podržava tastaturu).
   * **NFC/Folio Tracker:** Prati skrolovanje sekcija i ažurira levi sidebar u `00 / POČETAK`, `01 / USLUGE`, `02 / RADOVI`, `03 / KONTAKT`.
3. **Kontakt i društvene mreže:**
   * Ubačeni podaci servisera Mirka Vranića: telefon `063 7 495 456`, adresa `Srpskih vladara 210, Petrovac`.
   * Dodati su linkovi ka Facebook-u i Instagram-u u sekciji za direktan kontakt.

---

## 🚀 Dalji Razvoj

*   Za bilo kakvu promenu tekstova, direktno menjaš sadržaj u `.astro` fajlovima unutar `src/components/`.
*   Sve slike koje se nalaze u `public/images/` možeš zameniti sopstvenim fotografijama iz servisa ili sa terena, pod uslovom da zadržiš iste nazive fajlova (`hero.jpg`, `manifest.jpg` itd.).
