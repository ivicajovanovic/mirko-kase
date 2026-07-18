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
│   └── images/                # Lokalizovane slike sa Unsplash-a
│       ├── hero.jpg           # Slika kase/terminala na Hero sekciji
│       ├── manifest.jpg       # Serviser na poslu
│       ├── ledger-steel.jpg   # Detalj beskontaktnog plaćanja
│       ├── ledger-concrete.jpg # Kancelarijski materijal (rolne) - FIKSIRAN FAJL
│       ├── ledger-site.jpg    # Servisno vozilo na putu
│       ├── office.jpg         # Fotokopir papir i toneri (nova slika)
│       └── ... (ostale detaljne slike radova)
├── src/
│   ├── components/            # Pojedinačne sekcije sajta
│   │   ├── Hero.astro         # Početni ekran (Tekst premešten iznad naslova, naslov u 4 reda)
│   │   ├── Material.astro     # Usluge (Kase, Materijal, Teren - sa specifičnim lokacijama)
│   │   ├── Work.astro         # Servis (Usluge u praksi sa regionalnim primerima)
│   │   ├── Office.astro       # Kancelarija (Papir, toneri i pribor - NOVA SEKCIJA)
│   │   └── Commission.astro   # Forma za naručivanje, kontakt, Google mapa i footer
│   ├── layouts/
│   │   └── Layout.astro       # Glavni HTML šablon, globalni skriptovi, custom cursor i stiki navbar
│   ├── pages/
│   │   └── index.astro        # Glavna stranica koja sklapa sve komponente (Ažuriran title/meta)
│   └── styles/
│       └── global.css         # Svi globalni, brutalistički i responzivni stilovi
├── astro.config.mjs           # Astro konfiguracioni fajl
├── package.json               # Zavisnosti i skripte
├── tsconfig.json              # TypeScript konfiguracija
├── copy.md                    # Master copywriting plan na srpskom jeziku
├── handoff.md                 # Ova dokumentacija
└── download_images.sh         # Skripta za preuzimanje slika
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
2. **Logo i Navigacija ([Layout.astro](file:///home/ivica1979/projekti/mirko-kase/src/layouts/Layout.astro)):**
   * Integrisan je zvanični vektorski logotip `logo.svg` u zaglavlju (Navbar) i u podnožju (Footer), u potpunosti zamenjujući stari tekstualni natpis `VM MOBILE`.
   * Navbar je postavljen na `position: fixed;` sa polu-transparentnom tamnom pozadinom (`rgba(17,17,15,0.9)`) i `backdrop-filter: blur(8px)` za "glassmorphism" efekat pri skrolovanju.
   * Uklonjen je stari indikator mesta, a ubačen je **direktno klikabilan broj telefona `063 7 495 456` sa Lucide Phone ikonicom**.
   * Navigacija je proširena sa 3 na 4 kolone (`repeat(4, 1fr)`) kako bi ravnomerno rasporedila stavke menija.
3. **Položaj elemenata na Hero sekciji ([Hero.astro](file:///home/ivica1979/projekti/mirko-kase/src/components/Hero.astro)):**
   * Uvodni tekst je premešten **iznad** naslova i na desktopu i na mobilnom prikazu.
   * Glavni naslov je prelomljen u **četiri reda**: `Fiskalne` / `kase &` / `kancelarijski` / `materijal.`
   * Veličina fonta naslova je dinamički prilagođena (`clamp(54px, 8vw, 130px)`) da se spreči horizontalno prelivanje teksta.
   * Poziv na akciju (CTA link) je zadržan na originalnoj poziciji pri dnu sekcije.
   * Uklonjena je kartica sa specifikacijama (`.specimen` sitan tekst u donjem desnom uglu slike).
   * Uvedena je ista animacija postepenog otkrivanja slike na učitavanju stranice kao i na ostalim slikama. Da bi se izbeglo naglo "škljocanje" crne boje na poziciju klizača, implementirao sam glatku CSS tranziciju na `clip-path` i `left` poziciju klizača tokom učitavanja (`.hero.transitioning` klasa), tako da se i slika i sam razdelnik meko odklizavaju od 100% (potpuno crno) do podrazumevane vrednosti (44% na desktopu, 32% na mobilnom) u trajanju od 1.5s. Tranzicija se automatski gasi čim se učitavanje završi, obezbeđujući savršeno fluidan i trenutan odziv klizača pri ručnom prevlačenju.
4. **NFC/Folio Tracker:** Prati skrolovanje sekcija i ažurira levi sidebar u:
   * `00 / POČETAK` (Hero)
   * `01 / USLUGE` (Material)
   * `02 / SERVIS` (Work)
   * `03 / KANCELARIJA` (Office)
   * `04 / KONTAKT` (Commission)
5. **Dugme za povratak na vrh (Back to top):**
   * Stilizovano dugme `↑` je fiksirano u donjem desnom uglu. Pojavljuje se samo kada korisnik skroluje naniže (`scrollY > 400`). Ima crno-žuti hover efekat usklađen sa celim sajtom.
6. **Kontakt i Geografska Mapa ([Commission.astro](file:///home/ivica1979/projekti/mirko-kase/src/components/Commission.astro)):**
   * Ubačeni podaci servisera Mirka Vranića: telefon `063 7 495 456`, adresa `Srpskih vladara 210, Petrovac`.
   * **Google Maps Embed:** Ugrađen je interaktivni Google Maps iframe.
   * **Stilizacija mape:** Preko CSS-a je primenjen filter (`grayscale(1) invert(0.9)`) koji mapu pretvara u tamni monohromatski mod. Prilikom prelaza mišem (hover), filter se isključuje i mapa se prikazuje u punoj boji radi lakšeg čitanja.
 7. **Mobilna Responzivnost i UX Optimizacije (Mobilni Audit & UX Polish):**
    * **Prelazak i prelamanje reči:** Na svim naslovima na mobilnom su aktivirana pravila `word-break: break-word` i `overflow-wrap: break-word` kako bi se sprečilo ispadanje dugih reči iz okvira ekrana.
    * **Focus Trap i Overlay u Meniju:** Unutar otvorenog mobilnog menija dodat je focus trap koji zadržava fokus u krug (između dugmeta za zatvaranje i navigacionih linkova) radi potpune pristupačnosti. Klikom na zatamnjeni pozadinski overlay ili pritiskom na taster `Escape`, meni se automatski i bezbedno zatvara.
    * **iOS Zoom Prevention:** Postavljanjem fonta od **16px** na sva unosna polja forme (input, select, textarea) u mobilnom prikazu sprečeno je automatsko neželjeno zumiranje ekrana od strane iOS Safari pretraživača pri kucanju.
    * **Safe Area Insets (Notch podrška):** Za moderne uređaje sa zubom na ekranu, uneli smo CSS varijable `env(safe-area-inset-...)` na lepljivo zaglavlje, klizajući meni i "nazad na vrh" dugme.
    * **Haptički odziv (Touch feedback):** Uklonjen je fabrički plavi tap efekat na mobilnim pretraživačima i zamenjen modernim aktivnim haptičkim odzivom (`transform: scale(0.98)` sa brzim prelazom od 100ms).
 8. **Eliminacija Hero Flicker-a:**
    * Uvedena je sinhrona detekcija u `<head>` delu Layout-a koja dodaje klasu `js` na `<html>` element. CSS koristi ovo da započne učitavanje hero slike sa rezom od 100% (potpuno sakrivena), čime se eliminiše vizuelni skok/blesak slike pre nego što se učita klijentski JS i započne slow reveal animacija.

---

## 🚀 Dalji Rad i Održavanje

*   **Tekstovi:** Sve izmene tekstova se vrše direktno u `.astro` komponentama u `src/components/` ili u `Layout.astro` za zaglavlje i meni.
*   **Slike:** Sve slike se nalaze u `public/images/`. Možeš ih zameniti sopstvenim fotografijama pod uslovom da zadržiš iste nazive i formate fajlova.
