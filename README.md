# VM MOBILE — Prodaja i servis fiskalnih kasa

Moderna, brutalistički stilizovana web prezentacija za firmu **VM MOBILE** (Petrovac na Mlavi), specijalizovanu za prodaju, servis i fiskalizaciju kasa, kao i distribuciju pratećeg potrošnog i kancelarijskog materijala za Braničevski okrug i šire.

Projekat je izgrađen korišćenjem **Astro** framework-a, čistog **Vanilla CSS-a** i **TypeScript-a** za brze, ultra-optimizovane performanse i besprekorno korisničko iskustvo na svim uređajima.

---

## 🛠️ Tehnološki Stack

1. **Framework:** [Astro v5](https://astro.build/) (Static Site Generation - SSG)
2. **Stilovi:** Čist Vanilla CSS sa modernim brutalističkim dizajnom (boje: crna + signalno žuta).
3. **JS/TS:** Klijentske skripte pisane u TypeScript-u za visoku stabilnost.
4. **Slike:** Optimizovane i lokalno servirane iz `public/images/`.

---

## ✨ Ključne Karakteristike i UX Optimizacije

*   **Potpuna mobilna responzivnost (Responsive Design):** Prilagođeno svim ekranima od malih telefona (320px) preko tableta do desktop ekrana (≥ 1024px) bez ikakvog horizontalnog prelivanja.
*   **Interaktivni Meni (Mobile Drawer):**
    *   Fini klizajući meni na mobilnom sa ugrađenim **Focus Trap-om** za bezbednu i pristupačnu keyboard navigaciju.
    *   Zatimljeni pozadinski overlay koji automatski zatvara meni na klik van njega.
    *   Brzo zatvaranje menija pritiskom na taster `Escape`.
*   **Optimizacija protiv treperenja (Flicker-free loading):** Sinhrona detekcija JavaScript-a u `<head>` delu koja eliminiše vizuelne skokove slike na Hero sekciji tokom učitavanja.
*   **Safe Area insets (Notch podrška):** Pun komfor za moderne uređaje sa zubom/izrezom na vrhu i dnu ekrana.
*   **Haptički odziv na dodir:** Mikro-animacije skale (`transform: scale(0.98)`) i smanjenja prozirnosti pri dodiru na mobilnim uređajima, uz očišćen `-webkit-tap-highlight-color`.
*   **Ugrađena mapa u tamnom režimu:** Google Maps iframe stilizovan crno-belim CSS filterima koji se isključuju (prikazuju punu boju) na prelaz mišem.
*   **Pristupačna forma:** Svi inputi imaju ispravne `type`, `autocomplete` i `inputmode` atribute sa veličinom fonta od **16px** kako bi se sprečio neželjeni iOS auto-zoom na mobilnim pretraživačima.

---

## 📂 Struktura Projekta

```text
mirko-kase/
├── public/
│   ├── images/                # Optimizovane slike (hero, usluge, servisi)
│   ├── logo.svg               # Zvanični vektorski logotip
│   └── favicon.svg            # Favicon u SVG formatu
├── src/
│   ├── components/            # Pojedinačne sekcije sajta
│   │   ├── Hero.astro         # Početni ekran sa klizačem
│   │   ├── Material.astro     # Usluge (kase, materijal, teren)
│   │   ├── Work.astro         # Servisni portfolio sa primerima
│   │   ├── Office.astro       # Potrošni kancelarijski papiri i materijali
│   │   └── Commission.astro   # Forma, kontakt podaci, mapa i footer
│   ├── layouts/
│   │   └── Layout.astro       # Glavni HTML raspored, interaktivne skripte i navigacija
│   ├── styles/
│   │   └── global.css         # Brutalistički i responzivni CSS stilovi
│   └── pages/
│       └── index.astro        # Glavna stranica koja sklapa sve sekcije
├── package.json               # Skripte i zavisnosti projekta
├── tsconfig.json              # TypeScript podešavanja
├── handoff.md                 # Interna razvojna dokumentacija
└── README.md                  # Ovaj fajl
```

---

## 🚀 Pokretanje i Razvoj

Instalirajte zavisnosti:
```bash
npm install
```

Pokrenite lokalni razvojni server (Default: [http://localhost:4321](http://localhost:4321)):
```bash
npm run dev
```

Kompilacija (generisanje produkcionih statičkih HTML/CSS/JS fajlova u `/dist` folderu):
```bash
npm run build
```

Lokalni pregled generisanog produkcionog sajta:
```bash
npm run preview
```
