# VM MOBILE — Plan i Tekstovi Sajta (Copywriting)

Ovaj dokument sadrži zvanične tekstove (copy) za sajt **VM MOBILE**, organizovane po sekcijama i komponentama. Cilj ovih tekstova je da klijentima jasno i nedvosmisleno prenesu sve ključne informacije, obezbede poverenje kroz profesionalni ton, i garantuju gramatičku i stilsku ispravnost na srpskom jeziku.

---

## 1. Zaglavlje i navigacija (`src/layouts/Layout.astro`)

*   **Naziv brenda u logotipu:**
    `VM MOBILE / FISKALNE KASE`
    *(Jasno pozicioniranje firme od prvog kontakta).*
*   **Stavke u navigaciji (Desktop i Mobilni):**
    *   `01 Usluge`
    *   `02 Servis`
    *   `03 Kontakt`
*   **Indikator lokacije i godine:**
    `PETROVAC / 2026`
    *(Naglašava lokalno prisustvo i ažurnost).*
*   **Tekst bočne trake (Folio Tracker):**
    *   Prva sekcija: `00 / POČETAK`
    *   Druga sekcija (Usluge): `01 / USLUGE`
    *   Treća sekcija (Servis): `02 / SERVIS`
    *   Četvrta sekcija (Kancelarija): `03 / KANCELARIJA`
    *   Peta sekcija (Kontakt): `04 / KONTAKT`
    *   Tekst na dnu trake: `KASE / SERVIS / TEREN`

---

## 2. Početni ekran (`src/components/Hero.astro`)

*   **Glavni brutalistički naslov (trodelni animirani tekst):**
    `FISKALNE` / `KASE &` / `KANCELARIJSKI` / `MATERIJAL`
    *(Direktan i ritmičan naslov koji odmah komunicira šta radimo).*
*   **Prateći opis (Intro):**
    "Prodaja i servis fiskalnih uređaja i pratećeg potrošnog materijala."
*   **Glavni poziv na akciju (CTA):**
    `ZAKAŽITE SERVIS / NARUČITE ↘`
    *(Jasno usmerava klijenta na formu na dnu stranice).*
*   **Kartica sa specifikacijama (Brzi tehnički podaci o firmi):**
    *   `Servis:` `VM–001`
    *   `Pokrivenost:` `Petrovac + 9 opština`
    *   `Usluga:` `Prodaja / Servis`
    *   `Pristup:` `Terenski rad`
    *   `Lokacija:` `Petrovac`
    *   `Status:` `Aktivan`
*   **Bočni slogan:**
    `Terenska podrška / Konsultacije`

---

## 3. Sekcija: Usluge i Oprema (`src/components/Material.astro`)

*   **Glavni naslov sekcije:**
    `Brz odziv. Rešenje.`
    *(Naglašava pouzdanost i brzinu – dva najvažnija faktora za vlasnike maloprodajnih objekata).*
*   **Manifest (Opis vrednosti):**
    "Kase se programiraju, servisiraju i isporučuju spremne za rad.  
    Kancelarija dobija papir, termo rolne i kompletan prateći materijal.  
    Zastoj u poslovanju je nedopustiv."
    *(Kratke, samouverene rečenice bez suvišnih fraza).*
*   **Tabela usluga (Ledger):**
    1.  **Fiskalne kase (`Fk`)**  
        *Naziv:* `Fiskalne kase`  
        *Opis:* "Prodaja, fiskalizacija, unos artikala i redovan servis. Pouzdani uređaji usklađeni sa zakonom za stabilan i siguran rad vašeg maloprodajnog objekta."
    2.  **Kancelarijski materijal (`Km`)**  
        *Naziv:* `Kancelarijski materijal`  
        *Opis:* "Termo rolne, fotokopir papir, koverte, registratori i prateći sitan potrošni materijal za sve svakodnevne potrebe vaše kancelarije."
    3.  **Terenski rad (`Tr`)**  
        *Naziv:* `Terenski rad`  
        *Opis:* "Pokrivamo Petrovac na Mlavi, Požarevac, Kučevo, Despotovac, Veliku Planu, Svilajnac, Majdanpek, Golubac i Malo Crniće. Dolazak na vašu lokaciju, hitne intervencije i obuka osoblja na terenu."
*   **Sumarni parametri (Podnožje sekcije):**
    *   `01` Terenski servis
    *   `Fk` Prodaja i servis
    *   `Km` Potrošni materijal
    *   `∞` 9 opština / gradova

---

## 4. Sekcija: Servis (`src/components/Work.astro`)

Ova sekcija prikazuje realne primere naših usluga u praksi, pružajući klijentima uvid u to kako rešavamo njihove probleme.

*   **Mali naslov:** `Izabrane usluge / 2024–26`
*   **Glavni naslov:** `Terenska podrška`
*   **Projektne kartice:**
    1.  **Kartica 01: Prodaja i Servis**  
        *Naslov:* `Prodaja & Servis`  
        *Tehnički detalji (Meta):*  
        `VM–SRV / AKTIVAN`  
        Servis kase na terenu  
        Prodaja, fiskalizacija i instalacija  
        Brz odziv / Mirko Vranić  
        Petrovac / Požarevac / 2026
    2.  **Kartica 02: Potrošni materijal**  
        *Naslov:* `Kancelarijski materijal`  
        *Tehnički detalji (Meta):*  
        `VM–MAT / ISPORUKA`  
        Termo rolne i papir  
        Potrošni kancelarijski materijal  
        Isporuka na vašu adresu  
        Kučevo / Majdanpek / 2026
    3.  **Kartica 03: Konsultacije**  
        *Naslov:* `Stručne Konsultacije`  
        *Tehnički detalji (Meta):*  
        `VM–CON / PODRŠKA`  
        Saveti i fiskalizacija  
        Obuka osoblja i zakonski propisi  
        Telefonski / na lokaciji  
        Svilajnac / Despotovac / 2026

---

## 5. Sekcija: Kancelarijski materijal (`src/components/Office.astro`)

*   **Glavni naslov sekcije:**
    `Papir, toneri & materijal.`
*   **Manifest (Opis vrednosti):**
    "Kompletan asortiman za svakodnevno poslovanje vašeg preduzeća. Posedujemo i dostavljamo sav potrošni materijal na vašu adresu. Kvalitetni toneri i termo rolne garantuju dugotrajan rad štampača i kasa."
*   **Tabela usluga (Ledger):**
    1.  **Fotokopir papir (`Pp`)**  
        *Naziv:* `Fotokopir papir`  
        *Opis:* "A4 i A3 papir vrhunske beline i gustine (80g) namenjen za štampače, fotokopir aparate i faks mašine. Pakovanja od 500 listova."
    2.  **Termo rolne (`Tr`)**  
        *Naziv:* `Termo rolne`  
        *Opis:* "Termo rolne vrhunskog kvaliteta za sve tipove fiskalnih kasa, POS terminala i bankomata u standardnim širinama od 57mm i 80mm."
    3.  **Toneri i kertridži (`Tn`)**  
        *Naziv:* `Toneri & kertridži`  
        *Opis:* "Originalni i visoko-kvalitetni kompatibilni toneri za laserske štampače i kertridži za sve brendove (HP, Canon, Samsung, Epson)."
    4.  **Kancelarijski pribor (`Kp`)**  
        *Naziv:* `Kancelarijski pribor`  
        *Opis:* "Registratori, fascikle, koverte svih dimenzija, hemijske olovke, heftalice, bušači papira i sav ostali sitni kancelarijski potrošni materijal."
*   **Sumarni parametri (Podnožje sekcije):**
    *   `03` Kancelarija
    *   `Pp` Fotokopir papir
    *   `Tr` Termo rolne
    *   `Tn` Toneri i pribor

---

## 6. Sekcija: Kontakt i Naručivanje (`src/components/Commission.astro`)

*   **Glavni naslov sekcije:**
    `Pokrenite posao bez zastoja.`
    *(Povezuje proces fiskalizacije i servisa sa uspehom poslovanja klijenta).*
*   **Koraci saradnje (Steps):**
    *   `01` Kontakt / lokacija / potrebe
    *   `02` Ponuda kase + materijala
    *   `03` Fiskalizacija + unos artikala
    *   `04` Terenska isporuka + obuka
*   **Direktne informacije za kontakt:**
    "Kontakt servisa  
    Mirko Vranić (serviser)  
    [vm.mobile.vranic@gmail.com](mailto:vm.mobile.vranic@gmail.com)  
    [063 7 495 456](tel:+381637495456)  
    Srpskih vladara 210, Petrovac  
    Terenska podrška: Petrovac, Požarevac, Kučevo, Despotovac, Velika Plana, Svilajnac, Majdanpek, Golubac, Malo Crniće"
*   **Linkovi društvenih mreža:**
    [Facebook](#fb) / [Instagram](#ig)
*   **Formular za upit (Labele i Placeholderi):**
    *   *Ime i prezime / Naziv firme:* (Obavezno polje)
    *   *Email adresa:* (Obavezno polje)
    *   *Lokacija (Mesto / Opština):* (Obavezno polje, npr. Petrovac, Požarevac, Kučevo, Despotovac, Velika Plana, Svilajnac, Majdanpek, Golubac, Malo Crniće)
    *   *Vrsta usluge:* (Izbor: Servis kase, Kupovina nove kase, Kancelarijski materijal, Stručne konsultacije)
    *   *Obim posla:* (Placeholder: "NPR. BROJ ARTIKALA, KOLIČINA ROLNI")
    *   *Rok za realizaciju:* (Placeholder: "NPR. HITNO, OVE NEDELJE, MESEC")
    *   *Opis zahteva:* (Placeholder: "DETALJI O KVARU KASE, MODELU ILI SPISAK MATERIJALA")
    *   *Dugme za slanje:* `Pošaljite zahtev servisu ↗`
*   **Statusne poruke forme:**
    *   U slučaju greške: `Popunite obeležena polja pre slanja.`
    *   U slučaju uspeha: `Zahtev zabeležen. Povežite ovu formu sa backend-om pre lansiranja.` / `Zabeleženo ↗`

---

## 7. Podnožje stranice (Footer)

*   **Logotip:** `VM MOBILE`
*   **Kratak info:** `Prodaja i servis fiskalnih kasa / Petrovac, Požarevac i okolne opštine`
*   **Pravne napomene:** `© 2026 / Privatnost / pravne napomene`
