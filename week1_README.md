# Nädal 1 — Klient ja Turg

**Konsultant:** [Sinu nimi]
**Roll:** 02Signal Advisory nooremkonsultant
**Klient:** UrbanStyle.ltd
**Artefakti tüüp:** Kliendisegmentide analüüs + persona + konkurendianalüüs + SWOT

---

## Baas (70%) — Kliendisegmentide analüüs

### 1. Kliendisegmendid (AI-genereeritud + minu täiendused)

| # | Segment | Vanus | % käibest (hinnang) | Põhivajadus | Kanali eelistus |
|---|---------|-------|---------------------|-------------|-----------------|
| 1 | Linnaminimalistid | 25–35 | ~45% | Põhjamaade stiil, kvaliteet, vähem-on-rohkem | E-pood + Tallinn pood |
| 2 | Karjäärinaised | 30–45 | ~25% | Töörõivad, mis sobivad ka õhtuks | E-pood (õhtul) |
| 3 | Tagasipöördujad 40+ | 40–55 | ~15% | Lojaalsus, ajatu stiil, mugavus | Tallinn pood, telefon |
| 4 | Hinnatundlikud üliõpilased | 19–25 | ~10% | Madalam hind, allahindlused | Tartu pood, e-pood |
| 5 | Kingituse-ostjad | varieerub | ~5% | Lihtne valik, kingikomplektid | E-pood enne pühi |

**Minu täiendused AI-le võrreldes:**
- AI ei tuvastanud segmendi 5 olemasolu — see selgus, kui vaatasin müügi sesoonsust (detsembri piik)
- Liigutasin "karjäärinaised" ettepoole — AI pani nad 4. kohale, aga nende AOV (keskmine ostukorv) on kõrgem
- Lisasin "kanali eelistus" veeru — see on oluline, sest meie chatbot saab teenida ainult kanaleid, kus segment päriselt on

---

### 2. Kliendi persona — Karjäärinaised

> **Valisin selle segmendi**, sest see on prioriteet nr 1 chatboti pilootprojektiks (kõrgem AOV, õhtune aktiivsus = klienditeenindus pole tööl).

**Persona: Liisa, 36**

| Atribuut | Detail |
|----------|--------|
| Amet | Keskastme juht IT-ettevõttes (HR/Marketing/Finance) |
| Asukoht | Tallinn, Mustamäe / Põhja-Tallinn |
| Sissetulek | 2200–3500 € netto |
| Pereseis | Abielus, 1–2 last (5–12 a) |
| Vaba aeg | Vähe — 3–4 h nädalas ostlemiseks |
| Riietusvajadus | Hommikul töö → õhtul üritus, ilma vahepeal vahetamata |

**Tüüpiline ostuteekond:**
1. Näeb Instagramis stiililist (laupäeval hommikul)
2. Salvestab pildi, ei osta kohe
3. Tuleb e-poodi õhtul 21–23 (lapsed magama, paus)
4. Vaatab läbi 3–5 toodet, jätab korvi
5. Ostab järgmisel päeval, kui leiab vastuse: "Kas see sobib ka linnakontorisse?"

**Valupunktid (UrbanStyle'i jaoks olulised):**
- ❗ "Õhtul 22:00 ei vasta keegi mu küsimusele toote materjali kohta — ostan siis hoopis ASOSest."
- ❗ "Ma ei tea, kas suurus on standardne või jookseb suurelt/väikselt."
- ❗ "Vaja näha, kuidas asi seljas päriselt välja näeb, mitte ainult mannekeenil."

**Mida AI peaks tegema (chatboti pilooti suunav järeldus):**
- Vastama materjali / suuruse / kombineerimise küsimustele 24/7
- Soovitama outfit-paari teiste toodete põhjal
- Eskaleerima inimese juurde, kui küsimus on tundlik (tagastamine, defekt)

---

### 3. Lühike analüüs (3–5 lauset)

Kõige üllatavam oli see, et **kõrgeim ärilise väärtusega segment ei ole suurim segment**. Linnaminimalistid annavad ~45% käibest, aga nende AOV on 30% madalam kui karjäärinaiste oma — see muutis chatboti prioriteedi täielikult.

AI tabas hästi demograafia ja kanalite üldist mustrit — see osa oli kasutuskõlbulik 5 minutiga. AI ei näinud aga segmendi 5 (kingituse-ostjad) olemasolu — see tuli ainult sesoonsuse andmetest. **Õppetund:** AI on hea **hüpoteeside genereerimisel**, aga lõppotsuse jaoks vajan oma müügiandmeid.

Mida ma muudaksin: lisaksin järgmisel iteratsioonil segmentidele konkreetsed sõnumid (mitte ainult kirjelduse) — see paneb segmendid kohe turunduskasutusse.

---

## Edasijõudnutele (30%) — Konkurentide AI-analüüs + SWOT

### 1. Konkurentide AI-kasutus (Eesti / Põhjamaade rõivasektor)

| Konkurent / tüüp | Mida nad AI-ga teevad | Vahe UrbanStyle'iga |
|------------------|------------------------|----------------------|
| Reserved.com | Personaalsed soovitused põhjenev ostuajaloole + chatbot 24/7 | UrbanStyle pole alustanud |
| Marimekko | AI-koostatud lookbook'id sesoonidele | UrbanStyle teeb käsitsi |
| H&M | Virtuaalne mannekeen ("Kuidas mul see välja näeb?") | Eesti turul keegi pole |
| Eesti väikebrändid (Tanel Veenre, Iris Janvier) | Põhiliselt manuaalne, AI kasutus madal | UrbanStyle saab eristuda kiiremini |
| Asos / Zalando | Suurusehindajad, "Like this? Try this" algoritmid | UrbanStyle ei suuda mahuga konkureerida → nišš |

**3 olulisemat järeldust:**
1. **Eesti turul on AI-võimekuse aken lahti** — keegi keskmise suurusega rõivabränd ei tee veel chatbot'e ega personaalseid soovitusi. UrbanStyle saab esimene olla.
2. **Suurte konkurentidega ei tasu kopeerida** — Zalando virtuaalne mannekeen vajab miljonite eurode pildibaasi. Meie eelis = otse-suhtlus, mitte mahu mäng.
3. **Konkurendid kasutavad AI-d "front-of-house" (klient näeb)** — me võiksime alustada "back-of-house" võitudest (sisuautomatiseerimine, korrastatud KKK), mis on madalama riskiga.

---

### 2. AI-laiendatud SWOT — UrbanStyle.ltd

| | **Praegune sisu** | **AI-laiendus** |
|---|--------------------|------------------|
| **S (Tugevused)** | Tugev bränditoon, lojaalne kliendibaas, 2 füüsilist poodi | AI saab bränditooni **võimendada** — iga AI-loodud sisu järgib brand voice dokumenti |
| **W (Nõrkused)** | Väike turundusmeeskond (1 in.), õhtuti pole klienditeenindust | AI saab nõrkusi **kompenseerida** — chatbot katab 24/7, AI loob mustand-sisu turundajale |
| **O (Võimalused)** | Sügiskollektsioon, Põhjamaade trend Eestis | AI saab võimalusi **realiseerida** kiiremini — sisu mitmes kanalis paralleelselt |
| **T (Ohud)** | Konkurendid (Reserved, ASOS), tarneahel | AI saab ohte **leevendada** — varajane signaal andmetest (mille kohta tuleb tagasiside vms) |

---

### 3. Top 3 võimalust (mida katsetada esimesena)

1. **FAQ chatbot karjäärinaiste segmendile (õhtune kasutus)** — väärtus kohene, risk madal, andmed olemas (faq_30.json)
2. **AI-genereeritud sisubriefid sotsiaalmeediasse** — säästab turundaja 6h/nädalas, järgib brand voice'i
3. **Sisemine "raporti-bot"** Toomase tarbeks — IT-juht säästab 3h/päev (täpsemalt nädal 3 teema)

---

## Soovitus UrbanStyle'i juhtkonnale (1–2 lauset)

> Alustage chatboti pilooti **karjäärinaiste** segmendiga ja **FAQ-küsimuste** põhjal — see annab kõige kiirema mõõdetava võidu (õhtune konversioon) madalaima riskiga, ilma et peaks looma uut sisu.

---

*Artefakt 2/5 — Nädal 1 portfoolio*
