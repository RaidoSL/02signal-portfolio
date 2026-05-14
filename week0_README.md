# Nädal 0 — AI alused ja promptimine

**Konsultant:** [Sinu nimi]
**Roll:** 02Signal Advisory nooremkonsultant
**Klient:** UrbanStyle.ltd
**Artefakti tüüp:** Prompt'i enne-pärast võrdlus + NotebookLM eksperiment

---

## Baas (70%) — Prompt'i "enne-pärast" võrdlus

### 1. ENNE — Lihtne, struktureerimata prompt

**Prompt:**

> Kirjuta mulle UrbanStyle'i kohta meil klientidele uue kollektsiooni kohta.

**AI vastus (kokkuvõte):**

> "Tere! Tutvustame uut kollektsiooni. Tule vaatama meie poodi! Pakume kvaliteetset rõivastust soodsate hindadega. Ootame teid! Lugupidamisega, UrbanStyle meeskond."

**Hinnang:** Vastus on üldsõnaline, ilma konkreetsuseta, sobib ükskõik millisele ettevõttele. Puudub bränditoon, sihtkliendi info, konkreetne kollektsioon ja üleskutse tegevuseks (CTA).

---

### 2. PÄRAST — TÄPNE raamistikuga prompt

**Prompt:**

```
TAUST: Olen UrbanStyle.ltd turundusspetsialist. Oleme Eesti
keskmise suurusega rõivastusettevõte, mille kauplused on
Tallinnas ja Tartus + e-pood. Sihtklient on 25–40-aastane
linnaelanik, kes hindab kvaliteeti ja Põhjamaade minimalistlikku
stiili. Bränditoon: soe, otsekohene, mitte üleliia ametlik.

ÄRIEESMÄRK: Saata sügis-talv 2026 kollektsiooni meil ~3000
püsikliendile eesmärgiga tuua kauplusesse/e-poodi külastusi
esimese 2 nädala jooksul +15%.

PÄRING: Kirjuta meil eesti keeles, mis tutvustab uut
kollektsiooni "Nordic Layers" ja kutsub esmaesitlusele
20. septembril Tallinna kaupluses.

NÜANSID:
- Pikkus 120–150 sõna (mobiilis loetav)
- Mainida 3 võtmetoodet: villamantel, meriinopusa, taastoodetud
  teksased
- Ära kasuta sõnu "ekskluusiivne", "pakkumine kestab vaid"
  (need ei sobi meie bränditooniga)
- CTA: broneeri esmaesitlusele aeg lingiga

ESITLUS: Pealkiri + 2–3 lõiku + 1 selge CTA nupp. Lisa lõppu
2 alternatiivset pealkirja, mille seast valida.
```

**AI vastus (kokkuvõte):**

> Saadud meil on 135 sõna pikk, kasutab soojat ja otsekohest tooni, mainib täpselt 3 võtmetoodet, sisaldab kutset esmaesitlusele konkreetse kuupäeva ja kellaajaga, lõpeb selge CTA-ga "Broneeri oma aeg →" ning pakub 2 alternatiivset pealkirja A/B-testimiseks. Vältis keelatud sõnu.

---

### 3. Analüüs (mis muutus ja miks)

Esimese prompti vastus oli kasutu mall — AI ei teadnud, kellele kirjutada ega mida müüa. **TÄPNE raamistiku** rakendamine muutis vastuse otse kasutatavaks:

1. **T (Taust)** andis AI-le kliendi DNA — sihtgrupp ja bränditoon. Ilma selleta oleks AI valinud üldise turundustooni.
2. **N (Nüansid)** — keelatud sõnade nimekiri ja sõnade arv — sundisid AI-d jääma bränditooni piiridesse. See on koht, kus enamus inimesi promptimisel komistab: nad ei ütle, mida **mitte teha**.
3. **E (Esitlus)** muutis vastuse vormingulisest mallist päriselt kasutatavaks — A/B-testimiseks valmis variandid kohe juures.

**Õppetund:** Hea prompt = pool tööd tehtud. Halb prompt = poole rohkem aega parandamiseks kui käsitsi kirjutamiseks.

---

## Edasijõudnutele (30%) — NotebookLM eksperiment (RAG)

### 1. Küsimus

> "Millised on UrbanStyle.ltd-i 3 kõige olulisemat kliendisegmenti ja milline neist tasuks AI chatboti pilootprojektis esimesena katsetada?"

### 2. Vastus ILMA RAG-ita (tavaline Claude chat)

**Kokkuvõte:** Claude pakkus üldised segmendid Eesti rõivaturu kohta — "noored linnaelanikud 18–25", "keskklassi pered", "tudengid". Soovitas alustada noortega, kuna nad on "digitaalselt aktiivsed".

**Hinnang:**
- Üldsõnaline, võiks kehtida ükskõik millise rõivapoe kohta
- AI ei tea UrbanStyle'i tegelikku sihtgruppi (25–40, kvaliteediotsija)
- Hallutsineeris segmente, mida UrbanStyle'i profiilis pole
- Soovitus alustada "noortest" oli vastuolus tegeliku ärimudeliga

### 3. Vastus RAG-iga (NotebookLM, allikateks UrbanStyle profiil + tegelaste profiilid + programmi RAG-id)

**Kokkuvõte:** NotebookLM tuvastas konkreetselt UrbanStyle'i 3 segmenti:
1. **Linnaminimalistid (25–35)** — peamine segment, ~45% käibest
2. **Karjäärinaised (30–45)** — kõrgem keskmine ostukorv
3. **Tagasipöördujad (40+)** — väike, aga lojaalne grupp

Soovitas alustada **karjäärinaiste segmendist**, sest neil on kõrgeim "after-hours" ostlemiskäitumine (õhtul peale tööd, kui klienditugi pole töös) — täpselt see auk, mida chatbot katab. Lisas viited UrbanStyle profiilifaili konkreetsetele lõikudele.

**Hinnang:**
- Konkreetne, viidatud, kontekstipõhine
- Soovitus on ärilise loogikaga põhjendatud
- Allikaviited lubavad mind teha kontrolli — kus see info pärines

### 4. Järeldus

RAG-iga vastus oli mitte ainult **täpsem**, vaid ka **kasutatav**: see oli seotud UrbanStyle'i tegelike andmetega ja viitas allikatele. Ilma RAG-ita vastus oleks viinud meeskonda valele suunale (vale segment), mida oleksime hiljem pidanud ümber tegema.

> **Põhiõppetund:** Tavaline AI chat = "arvab" üldiste teadmiste põhjal. NotebookLM/RAG = "otsib" sinu enda dokumentidest. Äriotsuste tegemiseks vajame teist.

---

## Mikrovõidu hetk

Esimene kord, kui sain Claude'ilt vastuse, mille saab otse meili saata — ilma ühegi sõna muutmiseta. TÄPNE raamistik tundus alguses "liiga töömahukas", aga reaalsuses säästis see 15 minutit toimetamist.

---

*Artefakt 1/5 — Nädal 0 portfoolio*
