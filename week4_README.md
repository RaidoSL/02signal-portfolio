# Nädal 4 — Sisu ja Turundus

**Konsultant:** [Sinu nimi]
**Roll:** 02Signal Advisory nooremkonsultant
**Klient:** UrbanStyle.ltd
**Artefakti tüüp:** Sisu-workflow + brand voice + 5 sisudraft'i + tagasiside-õppetund

---

## Probleem

UrbanStyle'i turundaja Anna kulutab nädalas **~18 h** sotsiaalmeedia postituste loomisele (5 postitust × 3+ h igaüks: idee → tekst → pildi kohandamine → postitamine). See on **35% ta tööajast** — ta ei jõua tegeleda kampaaniaplaneerimisega ega andmeanalüüsiga.

Lahendus: **AI loob sisubrief'i ja esimese mustandi, inimene toimetab ja avaldab.** Aeg/postitus: 60 min → 20 min.

---

## Workflow

```
[Form Trigger]  →  [Set: laadi brand voice]  →  [AI node (Groq)]  →  [Telegram]
```

| Node | Roll |
|------|------|
| 1. Form Trigger | Anna täidab vormi: toode, kanal, sõnum, sihtsegment |
| 2. Set | Lisab brand voice'i juhise + näited |
| 3. AI node | Groq/Llama loob 3 mustand-varianti |
| 4. Telegram | Saadab mustandid Anna Telegrami |

Anna saab mustandid Telegrami, valib parima, toimetab ja avaldab. Tagasiside ("hea" / "halb" / "miks") läheb tagasi süsteemi.

---

## Brand voice — UrbanStyle.ltd

Fail: `brand-voice.json`

```json
{
  "brand_name": "UrbanStyle.ltd",
  "voice_pillars": {
    "tone": ["soe", "otsekohene", "mitte üleliia ametlik"],
    "values": [
      "Põhjamaade minimalism",
      "Kvaliteet enne kvantiteeti",
      "Aus suhtlus — ei sunni ostma"
    ],
    "perspective": "räägime kui sõber, kes oskab riietuda"
  },
  "do_use": [
    "Sina-vorm (mitte Teie)",
    "Konkreetsed materjalid (meriinovill, lina, taastoodetud puuvill)",
    "Eestikeelsed sõnad enne ingliskeelseid",
    "Lühikesed laused (max 20 sõna)",
    "Üks selge CTA postituse kohta"
  ],
  "dont_use": [
    "Ekskluusiivne, unikaalne, premium (ületarbitud)",
    "Pakkumine kestab vaid... (surve)",
    "Trendikas (üldine ja sisutu)",
    "Hüüumärgid kahekordselt!!",
    "Suurtähtedega KIRJUTAMINE (karjumine)",
    "Emoji-ridade kombinatsioonid"
  ],
  "sentence_examples": {
    "good": [
      "Meriinopusa, mis ei näri ega kortsu — kontorist õhtusöögini.",
      "Uus kollektsioon kohal. Tule vaatama Tallinna poodi laupäeval."
    ],
    "bad": [
      "ŠOKEERIVALT EKSKLUSIIVNE PAKKUMINE!!! Kasuta ära nüüd!",
      "Trendikas premium kvaliteet — sina väärid parimat ✨🛍️💎"
    ]
  }
}
```

---

## Andmed (mida AI kasutab)

| Allikas | Mida annab |
|---------|------------|
| `n4_products_sample.csv` | Toote info (materjal, hind, kollektsioon) |
| `n4_customer_segments.csv` | Sihtsegmendi kanalieelistused (Instagram, FB, e-mail) |
| `n4_promotions_sample.csv` | Lubatud sõnumid + piirangud kampaaniate kohta |
| `n4_inventory_sample.csv` | Laoseis — ei reklaami, kui pole varu |

**Reegel:** AI saab promptis kaasa **konkreetse toote rea CSV-st** (mitte üldine kirjeldus) + brand voice + sihtsegmendi. See tagab, et väited on faktidega kaetud (materjal, hind, saadavus).

---

## 5 AI-loodud sisuühikut

### Sisu 1 — Instagram post (meriinopusa, karjäärinaised)

**Toode:** Meriinopusa "Helsinki" — 89 €, 100% meriinovill
**Kanal:** Instagram feed
**Segment:** Karjäärinaised

> Õhtu enne tähtsat koosolekut. Hommikul vaja kontorisse, õhtul pere juurde.
>
> Üks pusa, mis sobib mõlemasse — Helsinki meriinopusa. Ei näri, ei kortsu, peseme villapesule.
>
> 89 €, saadaval Tallinna poes ja e-poes.
>
> *Vaata kogu kollektsiooni → link bios*

**Anna tagasiside:** ✅ Hea — vajab ainult ühe sõna muudatust ("Vaata" → "Tutvu") + emoji eemaldamine bio-st. **Toimetamise aeg: 3 min.**

---

### Sisu 2 — Facebook post (Click & Collect, kõik segmendid)

**Sõnum:** Click & Collect teenus on saadaval
**Kanal:** Facebook

> Tellisid e-poest, aga ei taha kullerit oodata?
>
> Tule poodi järele. 24h jooksul ootab pakk sind Tallinnas Rotermanni või Tartu Ülikooli tn poes.
>
> Tasuta. Ilma järjekorrata.
>
> Tellimine: urbanstyle.ee

**Anna tagasiside:** ✅ Hea — avaldame, kasutame ka homses Story-s. **Toimetamise aeg: 0 min.**

---

### Sisu 3 — Instagram Story (uus kollektsioon, linnaminimalistid)

**Toode:** "Nordic Layers" sügis-talv 2026 kollektsioon
**Kanal:** Instagram Story (3 slaidi)

**Slaid 1 (pealkiri):**
> Nordic Layers on kohal.

**Slaid 2 (selgitus):**
> 12 eset. Kõik kombineerib teistega. Sa ei pea valima mantli ja pusa vahel — vali mõlemad.

**Slaid 4 (CTA):**
> *Esmaesitlus 20. septembril Tallinna poes. Broneeri oma aeg →*

**Anna tagasiside:** ⚠️ Korralik, aga "Slaid 4" numeratsioon on vale (peaks olema 3). AI loendas slaide valesti. Slaidi 2 sõnum on parem kui mina ise oleks kirjutanud. **Toimetamise aeg: 2 min.**

---

### Sisu 4 — E-mail subject line (tagasipöördujad 40+)

**Eesmärk:** Avamise määr (open rate) >25%
**Segment:** Tagasipöördujad 40+ (lojaalsed, hindavad otsekohesust)

**AI variandid:**
1. *Liisa, sinu suurus on lattu jõudnud — Helsinki meriinopusa.*
2. *Kollektsioonist, mida sa eelmisel aastal vaatasid: 3 uut värvi.*
3. *Sügis tuli varem. Su kapis on koht uuele villamantlile.*

**Anna tagasiside:** ✅ Variant 1 on geniaalne — kasutab nime ja "sinu suurus on lattu jõudnud" on **personaalne ilma manipuleerimata**. Variant 2 hea, variant 3 liiga "luuleline" UrbanStyle'i jaoks. **Anna valib variant 1.**

---

### Sisu 5 — Blogipost-sissejuhatus (kõik segmendid)

**Teema:** "Miks me valisime meriinovilla sügiskollektsiooni põhimaterjaliks"
**Kanal:** Blog + e-mail

> Sa võib-olla mäletad veel aega, kui villane pluus tähendas sügelust ja "pane T-särk alla".
>
> See pole enam nii — vähemalt mitte meriinoga.
>
> Selle aasta "Nordic Layers" kollektsiooni 8 toodet on tehtud meriinovillast: see ei näri nahka, hingab paremini kui sünteetika ja seda ei pea iga päev pesema.
>
> Selles postituses räägime, mis vahe on meriinol ja tavalisel villal, miks me valisime selle ja kuidas seda hoida nii, et see kestab 5+ aastat.

**Anna tagasiside:** ✅ Hea sissejuhatus. AI sai aru, et blogipost peab olema **hariv, mitte müüv** — see eristub Anna ootuse järgi. Põhitekst kirjutab Anna ise (AI mustand ainult sissejuhatus + struktuur). **Toimetamise aeg: 0 min sissejuhatusele; põhitekst eraldi.**

---

## Tagasiside — mida inimene parandas ja miks

| Sisu | Probleem | Anna parandus | Õppetund AI-le |
|------|----------|---------------|------------------|
| 1 | Sõna "Vaata" — meie bränditoonis kasutame "Tutvu" | Käsitsi asendus | Lisa brand voice'i lemma-tabel: "vaata"→"tutvu", "osta"→"vali" |
| 1 | Emoji eemaldamine | Käsitsi eemaldus | Brand voice'i "dont_use" peaks olema tugevam: 0 emoji'd postitustes |
| 3 | Slaidi numeratsioon vale (slaid 4 oleks pidanud olema slaid 3) | Käsitsi | AI loendab valesti, kui vahepealsed slaidid hõlmavad teksti. Lahendus: lisa "räägi vähemalt 3 slaidi, nummerda 1, 2, 3" |
| 4 | Variant 3 "Sügis tuli varem" — liiga luuleline | Käsitsi valisin variant 1 | Brand voice'is täpsusta: "väldi metafooride üledoosi" |

**Üldine järeldus tagasisidest:**
> AI saab **80% korrektse** sisu peaaegu kohe; viimase 20% jaoks vajab inimest. **See pole AI ebaõnnestumine, see on hea jagamine** — inimene teeb selle, mida ta paremini teeb (toon, kontekst, otsus), AI selle, mida tema paremini (kiirus, struktuur).

---

## Tulemused

| Mõõdik | Enne | Pärast | Muutus |
|--------|------|--------|--------|
| Aeg / postitus | 60 min | 20 min (mustand + toimetamine) | **−67%** |
| Postitusi / nädal | 5 | 5 (sama) | – |
| Kogu aeg / nädal | 5 h | 1.7 h | **−3.3 h** |
| Anna ülejäänud aeg | – | Kampaaniate planeerimine, andmeanalüüs | **+3.3 h väärtuslikku tööd** |

---

## Õppetund (kokkuvõte)

**Mida AI tegi hästi:**
- Brand voice'i järgis 90% ulatuses esimesel katsel
- Konkreetsed faktid (hind, materjal, kollektsioon) õiged, kui CSV oli promptis
- Struktuuri loomine (Story-slaidide jaotus, blogiposti pealkirjastruktuur) säästis aega

**Mida inimene pidi parandama:**
- Bränditooni nüansid (sõnavalikud nagu "tutvu" vs "vaata")
- Numeerimine ja konsistentsus (väikesed vead)
- Kontekstuaalsed otsused (millised emoji'd, kas blogipost on hariv või müüv)

**Põhiline:** AI ei asenda Anna't. AI annab Annale tagasi **3,3 h nädalas** — mida ta saab kasutada **strateegilisemaks tööks**, mida AI ei oska teha.

---

## Faili struktuur (week4-sisu-turundus/)

```
week4-sisu-turundus/
├── README.md              ← see fail
├── brand-voice.json       ← brand voice'i juhis (ülal)
├── sisu-draftid.md        ← 5 sisuühikut (ülal)
├── workflow-export.json   ← n8n workflow export
└── tagasiside.md          ← Anna parandused + õppetunnid
```

---

## Sild N5-sse

N4 lahendas **"kuidas luua sisu"**. N5 vastab küsimusele: **"kui meil tekib kümneid drafte, kuidas me näeme, mis töötab, mis kordub ja mida järgmisel nädalal paremini teha?"**

N4: Form → AI → Telegram
N5: Form → AI → Telegram → **Google Sheets logi → nädalaaruanne**

Iga AI-mustand + Anna parandus salvestatakse Sheets'i. Nädala lõpus näeme: milliseid sõnu Anna kõige rohkem parandab → täiendame brand voice'i. See on **andmepõhine paranemise tsükkel**.

---

*Artefakt 5/5 — Nädal 4 portfoolio*
