# Nädal 2 — Operatsioonid ja Protsessid

**Konsultant:** [Sinu nimi]
**Roll:** 02Signal Advisory nooremkonsultant
**Klient:** UrbanStyle.ltd
**Artefakti tüüp:** Protsessikaardistus + automatiseerimise hindamine + ROI-analüüs

---

## Baas (70%) — Protsesside kaardistamine ja analüüs

### 1. Protsessikirjeldused (5 protsessi)

#### Protsess A — E-poe tellimuse käsitlus *(põhiprotsess)*

**Sisend:** Klient esitab e-poes tellimuse
**Tegevused (sammud):**
1. Tellimus laekub e-poodi (automaatne)
2. Klienditeenindaja kontrollib lao saadavust käsitsi (vaatab Excelit)
3. Saadab kinnituse-meili (käsitsi mall)
4. Loob saatelehe (käsitsi printer)
5. Pakkimine + üleandmine kullerile
6. Ootab 1–2 päeva, jälgib kuller-API-d käsitsi
7. Saadab "Su pakk on teel" meili (käsitsi)

**Väljund:** Klient saab paki
**Tüüp:** Käsitsi → poolautomaatne (3 sammu on käsitsi, mis võiksid olla automaatsed)

---

#### Protsess B — KKK-vastamine klientidele *(põhiprotsess)*

**Sisend:** Klient küsib (e-mail, Facebook, telefon, e-poe vorm)
**Tegevused:**
1. Klienditeenindaja loeb küsimust
2. Otsib vastust peast / vanadest meilidest / Excel-failist "KKK"
3. Kirjutab vastuse käsitsi
4. Saadab vastuse
5. Ootab järgmist sõnumit

**Väljund:** Klient saab vastuse (keskmiselt 4–8 h, õhtul 12+ h)
**Tüüp:** 100% käsitsi
**Peidetud sammud:** ~30% vastustest on **samad küsimused** (tarne, suurus, materjal, tagastamine)

---

#### Protsess C — IT-osakonna iganädalane raport *(tugiprotsess)*

**Sisend:** Toomas (IT-juht) kogub andmeid esmaspäeva hommikul
**Tegevused:**
1. Eksportib müügiandmed e-poest (5 min)
2. Eksportib laoseisu Exceli (10 min)
3. Vormistab kokkuvõtte Word-dokumenti (45 min)
4. Loob graafikud käsitsi (60 min)
5. Saadab Kristile (juhile) meiliga (5 min)

**Väljund:** Iganädalane raport
**Kogu aeg:** ~2–3 h iga esmaspäev
**Tüüp:** 100% käsitsi
**Sagedus:** 1× nädalas → 50× aastas → **~125 h aastas**

---

#### Protsess D — Sisuloome sotsiaalmeediasse *(põhiprotsess)*

**Sisend:** Turundaja saab uue toote pildi + spetsifikatsiooni
**Tegevused:**
1. Vaatab toodet, mõtleb sõnumit (15 min)
2. Kirjutab teksti (20–30 min)
3. Kohandab pildi (15 min)
4. Postitab Instagrami + Facebooki (käsitsi, 5 min)
5. Vastab kommentaaridele (juhuslik, terve päev)

**Väljund:** 1 postitus, 1 toote kohta
**Tüüp:** 100% käsitsi
**Pudelikael:** kui tooteid on 20 nädalas, kulub turundajal 18+ h

---

#### Protsess E — Laoseisu kontroll ja täiendamine *(tugiprotsess)*

**Sisend:** Igapäevane (hommikul kell 9)
**Tegevused:**
1. Avab Excel-tabeli (1 min)
2. Vaatab "alla läviväärtuse" tooted (5 min)
3. Helistab tarnijale või saadab e-maili (15 min/toode)
4. Märgib Excelisse tellitud kogused (10 min)

**Väljund:** Tarnetellimus
**Tüüp:** Käsitsi
**Vea risk:** kõrge — keegi unustab vaadata = lao tühjenemine

---

### 2. Automatiseerimise hindamistabel

Hindamiskriteeriumid: skaala 1–5 (5 = väga kõrge)

| # | Protsess | Sagedus | Ajakulu | Veamäär | Andmete kättesaadavus | **Skoor** | **Soovitus** |
|---|----------|---------|---------|---------|------------------------|-----------|--------------|
| A | E-poe tellimuse käsitlus | 5 | 4 | 3 | 5 | **17/20** | **Automatiseeri** (osaliselt — meilid + jälgimine) |
| B | KKK-vastamine | 5 | 4 | 4 | 4 | **17/20** | **Automatiseeri** (FAQ chatbot) — Top prioriteet |
| C | IT iganädalane raport | 3 | 5 | 2 | 5 | **15/20** | **Automatiseeri** (n8n + Sheets + Telegram) |
| D | Sisuloome | 5 | 5 | 3 | 3 | **16/20** | **Lihtsusta** (AI mustand → inimene parandab) |
| E | Laoseisu kontroll | 5 | 2 | 5 | 4 | **16/20** | **Automatiseeri** (lävi-teavitus) |

**Eraldi otsus:** Protsess A puhul ei tasu kogu protsessi automatiseerida — saatmise pakkimine on ikka käsitsi. Aga **meilide automatiseerimine on madala riskiga kiire võit**.

---

### 3. Lühike analüüs

**Mis oli üllatav?**
Kõige enam aega ei kulu mitte sellele, mida ma arvasin (e-poe tellimused), vaid **KKK-vastamisele** ja **sisuloomele**. Need näevad väikesed välja, aga kumuleerib 20+ h nädalas.

**Minu prioriteet nr 1 ja miks:**

> **Protsess B (KKK-chatbot)** — sest:
> 1. **Suurim mõju kliendile** (24/7 vastus karjäärinaiste segmendile, mida tuvastasime nädal 1)
> 2. **Madalaim risk** — kui bot ei tea vastust, eskaleerib inimese juurde
> 3. **Andmed on olemas** (faq_30.json — 30 küsimust on juba kaardistatud)
> 4. **Kõige kiirem võit** — pilot 2 nädalaga

---

## Edasijõudnutele (30%) — ROI-analüüs ja ristviitamine

### 1. ROI-arvutus — Top 3 automatiseerimise kandidaati

**Eeldused:** 1 töötaja tunni kulu UrbanStyle'is = ~18 €/h (palk + maksud + üldkulu). Automatiseerimise kuukulu = ~30 € (n8n hostimine + AI API + Google Workspace).

#### Kandidaat 1 — KKK chatbot (Protsess B)

| Mõõdik | Praegu | Pärast | Sääst |
|--------|--------|--------|-------|
| KKK küsimuste maht | ~50/päev | 50/päev | – |
| Käsitsi vastatud | 50 (100%) | 15 (30% eskaleeritud) | 35 küsimust |
| Aeg kokku | 50 × 6 min = 5 h/päev | 15 × 6 min = 1.5 h/päev | **3.5 h/päev** |
| Aastane sääst | – | – | 3.5 × 250 × 18 € = **15 750 €** |
| Aastakulu | – | – | 30 € × 12 = **360 €** |
| **ROI aastal 1** | – | – | **~43×** |

#### Kandidaat 2 — IT iganädalane raport (Protsess C)

| Mõõdik | Praegu | Pärast |
|--------|--------|--------|
| Aeg | 2.5 h × 50 nädalat = 125 h/aasta | 0.25 h × 50 nädalat = 12.5 h/aasta |
| Sääst | – | 112.5 h × 18 € = **2 025 €** |
| Lisakulu | – | ~50 € seadistus, 0 € jooksev (n8n + Sheets) |
| **ROI aastal 1** | – | **~40×** + Toomase rahulolu (skeptiku konversioon!) |

#### Kandidaat 3 — Sisuloome AI-mustanditena (Protsess D)

| Mõõdik | Praegu | Pärast |
|--------|--------|--------|
| Postituste arv nädalas | 5 | 5 (sama) |
| Aeg/postitus | 60 min | 20 min (AI mustand + inimene toimetab) |
| Aeg/nädalas | 5 h | 1.7 h |
| Aastane sääst | – | (5 – 1.7) × 50 × 18 € = **2 970 €** |
| Lisakulu | – | API-kulud ~10 €/kuu = 120 €/aasta |
| **ROI aastal 1** | – | **~25×** |

**Top 3 kogu sääst:** **20 745 €** aastas, kogukuluga **~840 €** → puhas sääst ~**19 900 €** + 600 h töötaja aega vabaks tähtsamaks tööks.

---

### 2. Ristviitamine N1 ↔ N2

**Küsimus:** Millise kliendisegmendi kogemust parandab automatiseerimine kõige rohkem?

| Segment (N1) | Mõju automatiseerimisest |
|--------------|---------------------------|
| Karjäärinaised (30–45) | **MAKSIMUM** — õhtuti (22–01) tahab vastust, klienditeenindus pole tööl → chatbot annab kohe vastuse → konversioon kasvab |
| Linnaminimalistid | Keskmine — nad eelistavad e-poodi, aga küsivad vähem |
| Tagasipöördujad 40+ | Madal — nad tahavad **inimest**, mitte bot'i (võib isegi tagurpidi mõjuda) |
| Üliõpilased | Keskmine — kasutavad chat'i meelsasti, aga AOV madal |
| Kingituse-ostjad | Kõrge enne pühi — kingikomplektide soovitused chatboti kaudu |

**Järeldus:** **Sama segment** (karjäärinaised), mille tuvastasime N1-s prioriteet nr 1, on ka N2 protsessianalüüsi võitja. See pole juhus — see kinnitab, et meie analüüs on järjekindel.

---

### 3. Soovitus UrbanStyle'i juhtkonnale (Kristi'le)

> **Alustage protsessist B (KKK chatbot)**, sest see kombineerib N1 prioriteetse segmendi (karjäärinaised, õhtuti aktiivsed) ja N2 kõrgeima ROI võimaluse (~43× aastal 1, ~16 000 € puhas sääst). Toomase iganädalane raport (Protsess C) lisame järgmisesse iteratsiooni — see annab IT-juhi "konversioonimomenti" ja tõestab automatiseerimise väärtust kogu organisatsioonile.

**Põhimõte:** *Elimineeri enne, kui automatiseerid; lihtsusta enne, kui automatiseerid; automatiseeri viimasena.* Protsess B puhul oleme juba elimineerinud (KKK on standardiseeritud — 30 küsimust) ja lihtsustanud (faq_30.json on olemas) — nüüd on aeg automatiseerida.

---

*Artefakt 3/5 — Nädal 2 portfoolio*
