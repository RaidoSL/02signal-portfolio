# Nädal 3 — Automatiseerimine (n8n + Telegram)

**Konsultant:** [Sinu nimi]
**Roll:** 02Signal Advisory nooremkonsultant
**Klient:** UrbanStyle.ltd
**Artefakti tüüp:** Esimene automatiseerimine + FAQ boti kontseptsioon

---

## Baas (70%) — Esimene automatiseerimine

### 1. Automatiseerimise plaan

**Valitud N2 protsess:** **Protsess C — IT iganädalane raport** (Toomase 2,5 h esmaspäeva-hommikune töö)

**Miks just see?**
1. **Selge mõõdetav ajaiseaduslik** — 125 h aastas → kerge tõestada
2. **Toomase isiklik valupunkt** — kui IT-juht ise näeb mõju, võidab ta automatiseerimise eestkõnelejana üle ka teised meeskonnaliikmed (skeptikust evangelistiks)
3. **Madalaim turvarisk** — sisemine raport, mitte klienditele
4. **Sobiv keerukusaste esimeseks workflow'ks** — Form Trigger → Telegram on Shu-tase
5. **Hea sild järgmistele nädalatele** — N4-s saab lisada AI-genereeritud sisukokkuvõtte, N5-s graafikud andmetest

---

### 2. Workflow kirjeldus

**n8n workflow nimi:** `urbanstyle_weekly_report_v1`
**Server:** n8n.02signal.com
**Versioon:** Shu (baas)

```
[Form Trigger]  →  [Set (vormindus)]  →  [Telegram (saatmine)]
```

| Node | Roll | Konfiguratsioon |
|------|------|-----------------|
| **1. Form Trigger** | Käivitab workflow, kui Toomas täidab vormi | Väljad: `nädala_number`, `müük_eur`, `top_toode`, `pudelikael`, `märkused` |
| **2. Set** | Vormindab andmed Telegrami sõnumiks | Markdown-mall: `*Nädal {{N}}*` + müük + top toode + pudelikael + ajaltempel |
| **3. Telegram** | Saadab sõnumi Kristile (juhile) | Bot: `@urbanstyle_report_bot`, Chat ID: Kristi isiklik |

**Workflow sammude detail:**

1. **Form Trigger** (URL: `https://n8n.02signal.com/form/...`)
   - Toomas avab vormi telefonis või arvutis esmaspäeva hommikul
   - Sisestab 5 välja (kokku ~2 min)
2. **Set node** vormindab sisendi:
   ```
   📊 *UrbanStyle nädalaraport — N{{ $json.nädala_number }}*

   💰 Müük: {{ $json.müük_eur }} €
   🏆 Top toode: {{ $json.top_toode }}
   ⚠️ Pudelikael: {{ $json.pudelikael }}

   📝 Toomase märkused:
   {{ $json.märkused }}

   _Saadetud: {{ $now.format('DD.MM.YYYY HH:mm') }}_
   ```
3. **Telegram node** saadab sõnumi Kristi Telegrami kontosse

**Aja sääst:** 2,5 h → 3 min (vormi täitmine) = **säästu ~2,4 h iga nädal**

---

### 3. Tulemus + mikrovõidu hetk

**Kas workflow töötas?** Jah, esimese testi järel oli see töökorras. Esimene proovi-saatmine läks valele Chat ID-le (test-grupp), seda parandasin 30 sekundiga.

**Mikrovõidu hetk:**
> Täitsin vormi telefonis kohvi juurde. Telegram piiksus. Vaatasin — sõnum oli kohal. **See töötas.** Esimest korda elus tegin mina midagi, mis tegi midagi minu eest. Ma ei kirjuta enam koodi (ma ei oskagi), aga ma ehitasin automatiseerimise.

See tunne — "see töötas" — on **Toomase episoodi** kogemus 02Signal narratiivist. Saan nüüd aru, miks IT-juht skeptikust evangelistiks pööras.

---

### 4. Õppetund (mida ma ei teadnud enne)

- **Visuaalne programmeerimine** ei ole "lihtsam" tähenduses "lapsele jõukohane" — see on **kontseptuaalselt** sama keerukas kui kood, aga sa näed andmevoo. See on minu jaoks suur erinevus.
- **JSON ei ole hirmus** — `{{ $json.väli }}` on lihtsalt "võta vormist väli `väli` ja pane siia"
- **Credential'id on tähtsamad kui ma arvasin** — kui Telegrami token oleks valele inimesele leke, võiks ta bot'i kontrolli alla saada. Õppetund: **Toomase ettevaatlikkus on õige**, mitte takistus.

---

## Edasijõudnutele (30%) — FAQ boti kontseptsioon

### 1. Boti eesmärk

**Nimi:** `@urbanstyle_help_bot` (sisemine töönimi)
**Eesmärk:** Vastata 24/7 UrbanStyle'i klientide kõige sagedasematele küsimustele, eskaleerida keerukad päringud klienditeenindajatele tööaja jooksul.
**Sihtklient (N1 prioriteet):** Karjäärinaised, kes ostavad õhtul 21–01.
**Edukriteerium:** 70% küsimustest leiavad vastuse boti tasemel, ülejäänud 30% eskaleeritakse — kui inimene tuleb tööle hommikul, on järjekord juba sorteeritud.

---

### 2. FAQ nimekiri (12 küsimust, faq_30.json põhjal lihtsustatud)

| # | Kategooria | Küsimus | Vastus (lühend) |
|---|------------|---------|------------------|
| 1 | Tarne | Kui kaua tarne kestab? | Tallinn/Tartu: 1–2 päeva, mujal Eestis: 2–3 päeva, väljaspool: 5–7 |
| 2 | Tarne | Kas tarne on tasuta? | Tellimused üle 70 € — jah; alla — 4,90 €/pakk |
| 3 | Suurus | Kuidas ma tean oma suurust? | Suurustabel + soovitus: tellige 2 suurust, üks tagasi |
| 4 | Suurus | Kas teie suurused jooksevad suurelt/väikselt? | Standardsed, v.a meriinopusa — see soovitame suurust väiksem |
| 5 | Tagastamine | Kuidas tooteid tagastada? | 14 päeva tasuta, link tagastusvormile |
| 6 | Tagastamine | Kes maksab tagastuse? | UrbanStyle, kui toode on defektne; klient muidu (5 €) |
| 7 | Materjal | Mis materjalist X toode on? | Eskaleeri inimesele, kui toode pole faq_30.json sees |
| 8 | Materjal | Kas teil on 100% looduslikest materjalidest tooteid? | Jah — "Nordic Layers" kollektsioon |
| 9 | Pood | Mis kellaajal pood lahti on? | Tallinn: E–R 10–20, L 10–18, P 11–17; Tartu sama |
| 10 | Pood | Kas saan tellida e-poest ja tulla poodi järele? | Jah — "Click & Collect" 24h jooksul |
| 11 | Maksed | Milliseid makseviise toetate? | Pangakaart, pangalink, järelmaks (Liisi) |
| 12 | Üldist | Kuidas pesta? | Vaata toote sildilt; villakas — kuiv- või villapesu |

> Need 12 küsimust katavad meie hinnangul ~70% klientide päringutest.

---

### 3. Workflow visand (FAQ bot — kontseptsioon)

```
[Telegram Trigger]            ← klient saadab sõnumi
        ↓
[Code/Switch node]            ← mis kategooria küsimus see on?
        ↓
[Function: lookup faq_30.json] ← otsi vastus JSON-failist
        ↓
   ┌────┴────┐
   ↓         ↓
[Match]    [No match]
   ↓         ↓
[Telegram]  [Telegram: "Edastasin küsimuse meie tiimile,
sõnum]      vastame tööajal 9–17"]
            ↓
            [Telegram: teavita klienditeenindajat
            (eskalatsioon)]
```

**Sammud detailsemalt:**
1. **Telegram Trigger** võtab vastu kliendi sõnumi
2. **Switch/Function** klassifitseerib küsimuse (võtmesõnad: "tarne", "suurus", "tagasta", "pesemine" jne)
3. Kui leiab vastuse `faq_30.json`-ist → saadab sõnumi kliendile + logitab Google Sheets'i
4. Kui ei leia → vastab "edastasin küsimuse, vastame hommikul" + saadab eskalatsiooni klienditeenindajale eraldi Telegrami chati'le
5. Klienditeenindaja võtab hommikul järjekorra ette — see on juba **sorteeritud, mitte segamini**

---

### 4. Edasi mõeldes — mida lisada järgmistel nädalatel

| Nädal | Lisand | Mõte |
|-------|--------|------|
| N4 (Sisu) | AI-genereeritud vastused, kui vastust pole faq_30.json'is | Bränditoonile vastav, mitte "robotlik" |
| N4 (Sisu) | Brand voice'i juhis AI-le | "Vasta sõbralikult, otsekoheselt, mitte üleliia ametlikult" |
| N5 (Andmed) | Google Sheets logi — kõik küsimused + vastused | Hiljem näeme, milliseid uusi KKK-sid lisada |
| N5 (Andmed) | Nädalaaruanne: top 10 küsimust, eskalatsiooni-määr | Andmepõhine otsus, kus AI-d parandada |
| N6 (Integratsioon) | Outfit-soovituste node | Karjäärinaiste segmendile suunatud lisaväärtus |

---

## Toomase õppetund (lõpetuseks)

> *"Ma ei tahtnud n8n-i. Ma arvasin, et see lihtsalt teeb minu töö ohtlikumaks — keegi tuleb ja teeb midagi valesti, ja siis on terve meie süsteem nõme. Nüüd ma näen — n8n ei vahetanud mind välja. See vahetas välja **selle osa minust, mis tegi nüri tööd**. Ma ehitasin selle. Mina kontrollin seda. Ma saan selle igal ajal välja lülitada. See on tööriist, mitte mõõk."*
> — Toomas Kask, UrbanStyle IT-juht, peale esimest töötavat workflow'd

---

*Artefakt 4/5 — Nädal 3 portfoolio*
