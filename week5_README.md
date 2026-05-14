# Nädal 5 — Andmed ja Otsused

**Konsultant:** [Sinu nimi]
**Roll:** 02Signal Advisory nooremkonsultant
**Klient:** UrbanStyle.ltd
**Artefakti tüüp:** Google Sheets logi + andmepõhine raport + n8n workflow

---

## Probleem

UrbanStyle'i tootejuht **Marko Saar** koostab iganädalast raportit käsitsi: kogub müügiandmed e-poest ja kassasüsteemist, võrdleb eesmärkidega, paneb kokku Wordi, saadab Kristile (juhile). Aega kulub ~3 h/nädal × 50 nädalat = **150 h aastas**.

Lisaks: kuna iga osakond küsib **erinevat lõiget** (turundus tahab kampaania ROI-d, Kristi tahab nõukogu-ülevaadet, Liis tahab protsesside pudelikaela), teeb Marko sama tööd korduvalt erineva nurga alt.

**Otsus, mida vaja teha:** *Kas Narva (uue) poe kleitide kategoorias on probleem, mis nõuab kohest tegutsemist, või on see lihtsalt sesoonsuse müra?*

Kristi tahab vastust **kolmele küsimusele**:
1. Kui palju aega me automatiseerimisega säästsime?
2. Mis läks paremaks?
3. Kus on järgmine risk?

---

## Andmed

Kasutatud (anonüümseks tehtud näidisandmestik):

| Fail | Read | Mida sisaldab |
|------|------|----------------|
| `n5_sales_sample.csv` | 250 | Müügiandmestik kanali (e-pood / Tallinn / Tartu / Narva), kategooria, kuupäeva ja summa kaupa |
| `n5_kpi_targets.csv` | 12 | Eesmärgid kategooria × kanali kohta (nt "Narva pood kleidid: 1200 €/nädal") |
| `n5_report_log_headers.csv` | 1 | Google Sheets `Raportid` tabi päised |

**NB:** Ei kasuta päris kliendiandmeid. Numbrid on UrbanStyle näidisandmed `dataassets/` kaustast.

**Google Sheets struktuur (`UrbanStyle_N5_Logi`):**

Tab `Sissekanded` — iga workflow käivitamine logitakse:

| created_at | source | channel | category | amount_eur | note |
|------------|--------|---------|----------|------------|------|
| 2026-05-12 09:14 | n8n_form | narva | kleidid | 985 | nädal 19 kokku |
| 2026-05-12 09:14 | n8n_form | tallinn | kleidid | 2340 | nädal 19 kokku |
| ... | ... | ... | ... | ... | ... |

Tab `Raportid` — iga AI-loodud raport salvestatakse:

| created_at | report_type | period | key_metric | finding | recommendation | human_checked |
|------------|-------------|--------|------------|---------|----------------|----------------|
| 2026-05-12 17:30 | müük | N19 | Narva kleidid -18% vs eesmärk | Narva alustanud, lao järelnõudlus ebaselge | Kontrolli laoseisu enne Riia kampaaniat | ☐ |

---

## Workflow

**Nimi:** `urbanstyle_n5_data_report_v1`
**Server:** n8n.02signal.com
**Tase:** Ha (Sheets logi) + Ha+ (AI raport)

```
[Form Trigger]  →  [Sheets: kirjuta Sissekanded]  →  [Sheets: loe viimased 8 nädalat]
                                                              ↓
                                                     [AI node (Groq): leia muster]
                                                              ↓
                                                  [Sheets: kirjuta Raportid]
                                                              ↓
                                                          [Telegram: saada Kristile]
```

| Node | Roll | Konfiguratsioon |
|------|------|-----------------|
| **1. Form Trigger** | Marko sisestab nädala numbrid | Väljad: periood, kanal, kategooria, müük_eur, märkused |
| **2. Sheets — kirjuta Sissekanded** | Logitab toore rea | Append rida tab'i `Sissekanded` |
| **3. Sheets — loe viimased 8 nädalat** | Annab AI-le konteksti | Filter: viimased 8 nädalat, sama kanal+kategooria |
| **4. AI node (Groq/Llama)** | Leiab mustri ja sõnastab raporti | Prompt: vt allpool |
| **5. Sheets — kirjuta Raportid** | Salvestab valmis raporti | Append rida tab'i `Raportid` |
| **6. Telegram** | Saadab raporti Kristile + Markole | Lühike formaat (mobiilis loetav) |

**AI prompt (node 4):**

```
TAUST: Sa oled UrbanStyle.ltd andmeanalüütik. Sulle on antud
8 nädala müügiandmed ühe kanali ja kategooria kohta + KPI
eesmärgid samale periodile.

ÄRIEESMÄRK: Leia 1 oluline muster ja anna juhile (Kristi)
otsustamiseks piisav raport.

PÄRING: Tagasta raport TÄPSELT selles formaadis:

Periood: [täida]
Põhinumber: [täida — number + muutus %]
Oluline muster: [täida — 1 lause]
Võimalik põhjus: [täida — 1 lause, KASUTA SÕNA "võimalik"]
Soovitus: [täida — 1 konkreetne tegevus]
Mida kontrollida inimesel: [täida — 1 küsimus]

NÜANSID:
- ÄRA leiuta numbreid, mida andmetes pole.
- ÄRA väida põhjuslikkust, kui andmed näitavad ainult seost
  (kasuta sõnu "võimalik", "viitab", "tasub kontrollida").
- Kui andmeid on liiga vähe (alla 4 nädala), ütle seda.
- Maksimum 80 sõna kokku.

ESITLUS: Markdown, lühike, mobiilis loetav.
```

**Telegrami sõnumi formaat (node 6):**

```
📊 N5 UrbanStyle raport

Periood: {{ periood }}
Põhinumber: {{ põhinumber }}
Muster: {{ oluline_muster }}
Soovitus: {{ soovitus }}

⚠️ Inimese kontroll:
{{ mida_kontrollida }}

_Logitud Sheets'i: ✅_
```

---

## Muster (kõige olulisem leid)

**Esitatud küsimus:** *"Kas Narva poe kleitide kategoorias on probleem?"*

**AI vastus (peale 8 nädala andmete analüüsi):**

| Mõõdik | Väärtus |
|--------|---------|
| Periood | N12–N19 (2026) |
| Põhinumber | Narva kleidid: 985 €/nädal (eesmärk: 1200 €) — **−18%** vs eesmärk |
| Oluline muster | Narva kleidid −18%, **samal ajal Tallinn ja Tartu kleidid +5%** (sama kategooria, sama periood) |
| Võimalik põhjus | Narva pood avati alles 6 nädalat tagasi → lao järelnõudlust pole veel kalibreeritud; **võimalik**, et populaarsemaid suurusi (S, M) ei ole piisavalt varus |
| Soovitus | Kontrolli Narva poe laoseisu **enne** järgmist (Riia) kampaaniat — kui populaarsed suurused on otsas, kampaania ainult süvendab probleemi |
| Mida kontrollida inimesel | Kas Narva poe laoseisu andmed on värsked? (viimati uuendatud millal?) Kas Riia kampaania graafikus on aega laoseisu täiendada? |

**Miks see oluline:** Andmed üksi ei ole otsus. Number "−18%" võiks olla nii hooaja müra kui ka päris probleem. **Võrdluskontekst** (sama kategooria teistes kanalites samal perioodil) muutis ühe numbri **otsuseks kasvatatavaks signaaliks**.

---

## Soovitus UrbanStyle'i juhtkonnale

**Kristile (kolm vastust, mida ta tahab):**

1. **Kui palju aega säästsime?** Marko 3 h/nädal → 15 min/nädal = **säästu ~140 h aastas** (~2520 €). Workflow seadistus oli ühekordne ~6 h.
2. **Mis läks paremaks?** Raportid pole enam "millal Marko jõuab" küsimus — neid genereeritakse iga esmaspäev automaatselt. **Reageerime kiiremini** anomaaliatele (näide: Narva probleem tuvastatud N19, mitte 3 nädalat hiljem).
3. **Kus on järgmine risk?** Narva poe kleidid (-18%). Tegevus: laoseisu kontroll **enne** Riia kampaania algust (vt soovitust ülal).

**Markole (operatiivne):** Kontrolli Narva laoseisu täna. Kui populaarsed suurused on otsas, lükka Riia kampaania 1 nädala võrra edasi.

---

## Inimese kontroll (mida peab enne päris otsust üle kontrollima)

⚠️ **AI raporti kontrollnimekiri (kohustuslik enne juhtkonnale saatmist):**

1. ☐ **Andmeallikas:** Kas Sheets'i kirjutatud read pärinevad õigest perioodist? Kas keegi käsitsi midagi muutis vahepeal?
2. ☐ **Periood:** Kas raporti periood vastab tegelikkusele (N19 = 5.–11. mai 2026)?
3. ☐ **Põhjuslikkus:** Kas AI kasutas sõna "võimalik"/"viitab" (õigesti) või väitis põhjusena midagi, mida andmetes pole näha? Kui AI ütleb "põhjus on X" — eemalda see ja küsi uuesti.
4. ☐ **Numbrid:** Kas AI raportis numbrid kattuvad Sheets'i tegelike andmetega? (Kontrolli juhuslikult 1–2 rida.)
5. ☐ **Anomaalia vs müra:** Kas leitud "muster" püsib pikemalt kui 2 nädalat? Üks nädala kõikumine ei ole muster.
6. ☐ **Tegevus on realistlik:** Kas "soovitus" on midagi, mida saab tegelikult **sel nädalal** ette võtta?

**Kuldreegel:** AI raport on **tooresmaterjal**, mitte lõpptulemus. Inimene paneb kontrollkasti — siis (ja ainult siis) läheb raport edasi.

---

## Mida muutus N5-s võrreldes N4-ga

| Aspekt | N4 (sisu) | N5 (andmed) |
|--------|-----------|-------------|
| Workflow eesmärk | Loo midagi (sisudraft) | Mäleta, võrdle, kokku võta |
| Vool | Form → AI → Telegram | Form → **Sheets logi** → AI → **Sheets raport** → Telegram |
| Mälu | Ei ole | Google Sheets = "lihtne andmebaas" |
| Inimese roll | Toimetab teksti | Kontrollib numbreid + põhjuslikkust |
| Mis tekib aja jooksul | Postitused | **Ajalugu → mustrid → otsused** |

**Põhipööre:** Esimest korda muutub workflow **"hetkes kasulikust"** millekski, mis **kasvab koos andmetega**. Kui Marko logitab 6 kuud, on tal käes valmis dataset, mida saab teisteks otsusteks kasutada (mitte ainult raportite jaoks).

---

## Risk: kui Google Sheets enam ei piisa

Google Sheets on hea **algetapis**, aga sellel on piirid:

| Sümptom | Mida tähendab | Lahendus |
|---------|----------------|-----------|
| Sheet aeglustub (>10 000 rida) | Sheets pole andmebaas | Liigu Postgres / Supabase |
| Mitu workflow'd kirjutavad korraga → andmed segamini | Sheets ei toeta tehinguid (transactions) | Lisa "lock" muster või liigu andmebaasi |
| Tundlikud andmed (kliendi nimi, e-mail) Sheets'is | Andmekaitse risk | **Ära logi tundlikku infot Sheets'i** — kasuta anonüümseid ID-sid |

**Praegu (UrbanStyle, N5 etapp):** Sheets on **õige** valik. Liigu edasi siis, kui üks neist sümptomitest ilmub.

---

## Faili struktuur (week5-data-decisions/)

```
week5-data-decisions/
├── README.md                     ← see fail
├── workflow-export.json          ← n8n workflow export
├── sample-sheets-screenshot.png  ← Sheets'i tabelite kuvatõmmis
├── ai-prompt.md                  ← raporti-genereerimise prompt
└── decision-log.md               ← Narva kleitide otsuse-jälg
```

---

## Sild N6-sse

N5 lõpetas üksiku workflow'ga: andmed → mälu → raport.
N6 ühendab **kõik nädalad kokku** üheks demokõlblikuks tervikuks:

- N3 FAQ chatbot + N4 sisu-workflow + N5 andmeraport = üks **integreeritud süsteem**
- Esitlus juhtkonnale + tagasiside-ring

> *"Andmed üksi ei tee otsust. Aga andmed + muster + inimese kontroll = otsus, mille taga seisad."*
> — Marko Saar, UrbanStyle tootejuht, peale esimest automatiseeritud raportit

---

*Artefakt 5/5 — Nädal 5 portfoolio*
