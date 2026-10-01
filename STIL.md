# Stilguide for foredragene

Dette er oppskriften på foredragene i repoet. Les den før du lager eller endrer et
foredrag, slik at det nye ser ut og oppfører seg som de andre.

**Best å kopiere fra:**
- `javascript/` (Vg1) har kjøringene, minnet, scenene og sløyfa.
- `forms/` (Vg2) har skjema som virker på sliden, oppbygging med auto-animate, og
  kjøringer med objekter og en databasetabell.
- `fetch/no/` er den opprinnelige, enklere foredragsstilen.
- `japanese/` er på engelsk, laget for en konferanse, og bruker d3.

---

## 1. Hva repoet er

- **En reveal.js 5-fork** som publiseres på **prez.truls.dev** (se `CNAME`).
- **Ett foredrag per mappe:** `forms/index.html`, `javascript/index.html`,
  `fetch/no/index.html` …
  - Stiene peker oppover: `../dist/…` og `../plugin/…`, eller `../../` fra
    `fetch/no/`.
  - Hvert foredrag er **selvstendig**. CSS og skript ligger i den samme
    `index.html`, og det finnes ingen felles fil. Kopier det du trenger fra
    et annet foredrag.
- **`index.html` på rota** er lista over foredrag, med det nyeste øverst. Nye
  foredrag får en `<li class="talk-item">` der, med tittel, en setning og lenker
  (`Norsk →` / `English →`).
- **`dist/` er ferdig bygget.** Ikke rør `dist/`, `plugin/`, `css/` eller
  `js/`. De hører til reveal.js.
- **Forhåndsvisning:** en statisk server holder.
  ```bash
  python3 -m http.server 8001 --directory .
  ```
  `npm start` (gulp, port 8001) virker også, se `.claude/launch.json`. Er
  porten opptatt, kjører det sannsynligvis allerede en server du kan bruke.

---

## 2. Grunntonen

Foredragene er **muntlige**. Lysarkene er det læreren peker på mens hen
snakker, ikke et dokument som skal leses.

- **Norsk bokmål**, med muntlige setninger. Bare `japanese/` og `fetch/` er på
  engelsk.
- **Én idé per slide.**
- **Titlene er spørsmål** som en elev kunne ha stilt, og svaret kommer i
  fragmenter. Eksempler:
  - «Hva skjer når du trykker på knappen?»
  - «Hvorfor skjer det ingenting?»
  - «Må det hete det samme overalt?»
  - «`penger = penger - 25`?!»
- **Begynn med noe de har opplevd:** «Hvor har du brukt et skjema i dag?», med
  en emoji-liste som kommer frem punkt for punkt. Avslutt med én setning som
  samler det: «Hver gang du skriver noe inn og trykker på en knapp».
- **Gjett først, vis etterpå.** «**Gjett:** hva står i adresselinja når du
  trykker?» står over demoen før noe skjer.
- **Bruk ett eksempel hele veien.** I `forms/` er det spillbiblioteket (Zelda,
  Minecraft, Tetris), og i `javascript/` skolekiosken (brus til 25 kr). Kode,
  demoer og tabeller bruker de samme dataene.
- **Vis den ekte feilmeldingen** elevene kommer til å møte, ord for ord.
  Eksempler: `ReferenceError: consol is not defined` og
  `Provided value cannot be bound to SQLite parameter 1.`
- **Vær ærlig om forenklinger.** I `forms/` står det: «I dag holder vi navnene
  like – det er enklest mens alt er nytt. Etter praksis lar vi lagene få hvert
  sitt språk.»
- **Emoji** brukes i lister og på poengslides (🤔 😈 🎉 🌳). Én eller to per
  slide, aldri som pynt i titler.
- **Avslutningen:**
  - En «Husk»-slide med 5–7 punkter som fragmenter.
  - Så regnbuetittelen `h2.pride`. For en klasse er teksten «Lykke til!», for
    et foredrag «Tusen takk!». Under står `Slides:` og en lenke til
    `prez.truls.dev/<mappe>`.
- **Presentatørnotater** i `<aside class="notes">` på nesten hver slide. De
  sier hva du gjør og hva du sier:
  - «Trykk på knappen …», «La dem svare før du viser lista».
  - Hva som ligger i stablene under.
  - Advarsler, som «NB: dette er løsningen på oppgave 10 – vis den etter at de
    har prøvd selv».

### Oppbygging

- **Hovedløpet går horisontalt.** Undertemaer og valgfrie sidespor ligger i
  vertikale stabler (`<section><section>…</section></section>`).
- **Hver akt starter med en `<h2>`-slide**, for eksempel «Hva skjer når du
  trykker på knappen?» eller «Hvordan viser vi mange spill samtidig?».
  Overganger kan også være en ren tekstslide:
  `<section>Men vi vil ikke laste sida på nytt</section>`.
- **Merk aktene i HTML-en med en kommentar:**
  ```html
  <!-- ======================================================
       Del 2 – skjema i JavaScript
       ====================================================== -->
  ```

---

## 3. Teknisk mal

```html
<!DOCTYPE html>
<html lang="nb">
	<head>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<title>Spørsmålet som er tittelen</title>
		<link rel="stylesheet" href="../dist/reset.css" />
		<link rel="stylesheet" href="../dist/reveal.css" />
		<link rel="stylesheet" href="../dist/theme/black.css" />
		<link rel="stylesheet" href="../plugin/highlight/monokai.css" />
		<style>/* se under */</style>
	</head>
	<body>
		<div class="reveal"><div class="slides"> … </div></div>
		<script src="../dist/reveal.js"></script>
		<script src="../plugin/notes/notes.js"></script>
		<script src="../plugin/highlight/highlight.js"></script>
		<script>
			// 1. data og hjelpefunksjoner for demoer
			// 2. lag usynlige .steg-fragmenter (FØR initialize)
			Reveal.initialize({ hash: true, width: 1100, height: 720, plugins: [RevealHighlight, RevealNotes] });
			// 3. Reveal.on(...) for demoer som følger klikkeren
		</script>
	</body>
</html>
```

Temaet er alltid `black` med kodefargene `monokai`. Flaten er 1100 × 720.

**Faste CSS-variabler og -rettinger.** Ta dem med i hvert nytt foredrag:

```css
:root {
	--gul: oklch(0.9 0.22 97.52);   /* aksent: aktiv linje, merk, knapper */
	--gronn: oklch(0.8 0.22 130);   /* ferdig, lest, trygg */
	--rosa: #ef5350;                /* feil, krasj, farlig */
	--blaa: #4fc3f7;                /* bokser, faner, id-er */
	--flate: #1d1d1d;
	--kant: #444;
	--editor: #272822;              /* samme som monokai */
}

/* Temaet gjør overskrifter til store bokstaver – men ikke kode */
.reveal h1 code, .reveal h2 code, .reveal h3 code { text-transform: none; white-space: nowrap; }

.lite { font-size: 0.6em; opacity: 0.7; }
.reveal .fragment.lite.visible { opacity: 0.7; }  /* ellers blir den 1 når den vises */
```

Knappen som går igjen (gul, blir grønn ved hover), fra `fetch/`:

```css
.btn { font: inherit; border: none; border-radius: 4px; padding: 0.25em 0.8em; font-weight: 700; color: #000; background: var(--gul); }
.btn:hover, .btn:focus-visible { cursor: pointer; background: var(--gronn); }
```

---

## 4. Byggeklosser

Alle finnes ferdige i `javascript/` eller `forms/`. Kopier derfra i stedet for å
skrive dem på nytt.

### 4.1 Fragmenter

- `class="fragment"` kommer frem. `class="fragment lite"` kommer frem dempet
  og liten, og passer til en bisetning under poenget.
- **`fragment merk`** dukker ikke opp, men blir gul der den står. Bruk den til
  å peke på ord i en linje som allerede vises:
  ```css
  .reveal .fragment.merk { opacity: 1; visibility: inherit; transition: color 0.3s ease; }
  .reveal .fragment.merk.visible { color: var(--gul); }
  ```

### 4.2 Kode som går trinn for trinn

```html
<pre><code class="language-javascript" data-trim data-line-numbers="|1|3|4">
const skjema = document.querySelector('#skjema');
</code></pre>
```

- **HTML inne i en kodeblokk** skrives i
  `<script type="text/template">…</script>` inni `<code>`, så den ikke må
  escapes.
- **Blander du kodetrinn og vanlige fragmenter,** må kodeblokken ha
  `data-fragment-index="1"`. Da får trinnene 1, 2, 3 …, og de andre
  fragmentene må ha den samme indeksen eksplisitt. Ellers kommer de i feil
  rekkefølge.

### 4.3 Oppbygging med auto-animate («La oss lage et»)

Flere slides etter hverandre med `data-auto-animate`, der koden vokser én del om
gangen og resultatet vises ved siden av:

```html
<section data-auto-animate>
	<h3>La oss lage et</h3>
	<div class="to-kolonner bygg">
		<pre data-id="bygg-kode"><code class="language-html" data-trim data-line-numbers="2"><script type="text/template">
<form>
  <input>
</form>
		</script></code></pre>
		<div class="demo" data-id="bygg-demo"> <!-- ekte HTML her --> </div>
	</div>
	<p>Så et felt å skrive i</p>
	<p class="fragment lite">Men hva skal jeg skrive her? 🤷</p>
</section>
```

- **Stiplet ramme med etikett** (`form.vis-boks`, `.tabell-boks`) viser hvor
  et usynlig element er.
- **Siste slide kan gå videre** fra samme `data-id`. Eksempel: radene
  animeres bort, og den tomme `tbody`-en blir igjen.

`javascript/` bruker det samme til «scener» med absolutt plasserte vinduer, der
en motor flyttes fra nettleseren til Node.

### 4.4 Levende demoer på sliden

Ekte skjema, knapper og tabeller som virker mens du presenterer.

- **Stopp all innsending, ellers lastes hele presentasjonen på nytt.** Én
  linje, med *capture*:
  ```js
  document.addEventListener("submit", (e) => e.preventDefault(), true);
  ```
- **Reveal ignorerer tastetrykk når et felt har fokus,** så man kan skrive i
  feltene uten at sliden skifter.
- **Vis det som ellers ville vært usynlig:**
  - en liksom-adresselinje (`.adresse`) med `?tittel=Zelda`
  - JSON som oppdateres mens du skriver
  - `textContent` mot `innerHTML` side om side
- **Kode som lages i JavaScript må fargelegges på nytt:**
  ```js
  kode.removeAttribute("data-highlighted");
  Reveal.getPlugin("highlight")?.hljs?.highlightElement(kode);
  ```
- **Fyll inn verdier på forhånd** (`value="Zelda"`), og ha gjerne knapper som
  fyller inn vanskelige tekster. Da slipper læreren å skrive foran klassen.

### 4.5 Demoer som følger klikkeren

Grepet bak både kjøringene og DOM-stegene:

1. **Lag usynlige fragmenter i JavaScript før `Reveal.initialize`,** ett per
   steg, med `data-fragment-index` 1…n. Da stemmer antall klikk alltid med
   antall steg.
2. **Ved `slidechanged`, `fragmentshown` og `fragmenthidden`:** tell
   `.steg.visible` på sliden, og **bygg tilstanden på nytt fra steg 0** fram til
   det tallet. Da virker både bakover og direkte lenker (`#/16/0/3`).
3. **Hvert steg beskriver bare det som endres.**

`forms/` bruker det i «Tre verb»: én DOM-linje per klikk, med tabellen, treet og
boksen «Laget, men ikke festet» oppdatert i takt.

### 4.6 Kjøringer (liksom-VS Code)

Kjøringene er signaturen til stilen: kode som går **én linje per klikk**.
- Aktiv linje får ▶ på gul bakgrunn, ferdige linjer får ✓, og en linje som
  krasjer blir rød med ✗.
- Et gult notat bak linja viser hva som skjer: `→ "40" blir 40`.
- Panelene viser terminalen eller konsollen, og minnet som bokser.
- Under står en `p.forklaring` på én setning.

**Bruk:** `<div data-kjoring="navn"></div>` og et `KJORINGER`-objekt i skriptet.

```js
hei: {
	fil: "01-hei.js",
	rader: 5,                  // høyden på terminalen
	minne: true,               // vis «Minnet til Node»
	steg: [
		{ kode: [...linjer], inndata: "", forklaring: "Fila er lagret. Terminalen venter." },
		{ inndata: "node 01-hei.js", forklaring: "Du skriver kommandoen og trykker <kbd>Enter</kbd>." },
		{ inndata: null, terminal: ["$ node 01-hei.js"], leser: true },
		{ aktiv: 0, lag: ["penger", "100"], mer: ["Hei, verden!"], notat: [0, "→ 5 🤫"] },
		{ aktiv: null, ferdig: 2, inndata: "" },
	],
}
```

**Feltene i `javascript/`:**

| Felt | Hva det gjør |
|---|---|
| `kode` | Linjene i editoren. |
| `aktiv` | Linja som kjører nå. |
| `ferdig` | Antall ferdige linjer, eller en liste med linjenumre. |
| `krasj` | Den aktive linja blir rød. |
| `hoppet` + `hoppetTekst` | Linjer som ble hoppet over, for eksempel i en `if`-grein. |
| `notat: [linje, tekst]` | Gult notat bak en linje. |
| `terminal` | Erstatter innholdet i terminalen. |
| `mer` | Legger linjer til i terminalen. `"$ "` foran gir en kommando, `"! "` en feil. |
| `inndata` | Teksten ved markøren. `null` betyr at et program kjører. |
| `lag` | Ny boks: `[navn, verdi, "const"?]`. |
| `sett` | Ny verdi i en boks. Den gamle vises overstreket. |
| `les` | Boksene som leses, eller `"navn:i"` for ett rom i en liste. |
| `tom`, `fjern` | Tømmer minnet, eller fjerner én boks. |
| `rist` | Boksen rister, for eksempel ved `const`-feil. |
| `disk` + `ulagret` | Viser det som er lagret på disken ved siden av editoren, med ● på fanen. |
| `tast` | En tast som spretter opp. |

**Utvidelsene i `forms/`:**

| Felt | Hva det gjør |
|---|---|
| `minne` | Tittelen på panelet, for eksempel «🧠 Minnet til serveren». |
| `slutt` | Teksten som vises når boksene tones ut (`avsluttet: true`). |
| `konsoll` | Nettleserkonsollen i stedet for terminalen. |
| `db: { navn, kolonner, rader }` | Et panel med en databasetabell. `rad` i et steg legger til en rad. |
| Objekter i boksene | `lag: ["req.body", { tittel: '"Zelda"' }]`. Med `les: ["req.body:tittel"]` lyser ett felt opp. Det viser at koden plukker ut verdier. |
| `aktiv` som liste | Flere linjer aktive samtidig, for eksempel `[3, 4]`. |

**Mønsteret for en god kjøring:**
- **Steg 0** viser koden og stiller et «Gjett:»-spørsmål.
- **Ett steg per linje,** og forklaringen sier *hvorfor*, ikke bare hva.
- **Siste steg** gir poenget i én setning, gjerne i **fet**.

### 4.7 En linje plukket fra hverandre

En stor kodelinje der delene blir gule én etter én, med en forklaring under
hver del:

```html
<code class="stor-kode"><span class="fragment merk" data-fragment-index="1">&lt;input</span> <span class="fragment merk" data-fragment-index="2">name="tittel"</span>&gt;</code>
<div class="biter">
	<div class="fragment" data-fragment-index="1"><code>&lt;input&gt;</code><span>et felt å skrive i</span></div>
	<div class="fragment" data-fragment-index="2"><code>name</code><span>nøkkelen i dataene</span></div>
</div>
```

Les nøstede uttrykk **innenfra og ut**: først `new FormData(skjema)`, så
`Object.fromEntries(…)`, til slutt `const data =`.

### 4.8 Fargede deler av koden

Kode skrevet for hånd (`.kodeblokk`), der deler lyses opp i blått, gult og grønt
(`.del.a/.b/.c`), med merkelapper under (`.merkelapp`). Brukes til
`method`/`headers`/`body` i `forms/`, og til start/sjekk/oppdater i en
`for`-løkke i `javascript/`.

### 4.9 Annet

| Byggekloss | Hvor | Hva |
|---|---|---|
| `kbd` | begge | Taster: `<kbd>Ctrl</kbd> + <kbd>U</kbd>`. |
| Sløyfe | `javascript/` | En syklus i en sirkel, for eksempel endre, lagre, kjøre. |
| VS Code-meny | `javascript/` | En meny der ett punkt blir valgt. |
| To kort | `javascript/` | Sammenligning side om side, for eksempel `let` og `const`. |
| Liksom-innlegg | `javascript/` | Et innlegg med like-knapp som er en levende demo. |

---

## 5. Fallgruver (alle er faktisk møtt)

- **Store bokstaver i titler arves inn i `<code>`.** Filnavn blir `01-HEI.JS`.
  Rettingen står i malen over.
- **`.fragment.visible` setter `opacity: 1`,** og det overstyrer
  `.lite { opacity: 0.7 }`. Rettingen står i malen over.
- **`white-space: pre` i en beholder med blokk-barn** viser linjeskiftene i
  HTML-en som tomme linjer. I `.kodeblokk`: start innholdet på samme linje som
  starttaggen, og bruk bare `<span>` inni.
- **Auto-marger i et grid krymper elementet til innholdet.** En `.demo` med
  `margin: auto` inne i `.to-kolonner` ble en smal stripe. Sett `margin: 0`.
- **Lange kodelinjer kuttes stille.** Tommelfingerregler med linjenumre, ved
  1100 i bredde:

  | Skriftstørrelse på `pre` | Omtrent antall tegn |
  |---|---|
  | `0.55em` | 70 |
  | `0.5em` | 76 |
  | `0.45em` | 85 |

  Bryt linja, eller sett `style="font-size: …"` på `pre`.
- **Temaet `black` gir `pre code` en `max-height: 400px`.** Lang kode får et
  usynlig rullefelt. Sett `style="max-height: none"` på `code`.
- **To kolonner med kode og demo blir trange.** Gi kodekolonnen mer plass
  (`grid-template-columns: 1.35fr 1fr`), eller legg koden over demoen.
- **Indeksen i adressen** (`#/h/v/f`): `f` regnes fra 0, etter at reveal har
  sortert fragmentene.
- **Skjermbilder fra et skjult WebKit-vindu** kjører ikke CSS-animasjoner.
  Elementer med inngangsanimasjon kan se ut til å mangle. Sjekk med JavaScript
  i en ekte nettleser før du tror det er en feil.

---

## 6. Slik jobber du (for agenter)

1. **Spør hvem foredraget er for:** Vg1, Vg2 eller en konferanse. Spør også
   hva de har gjort før. Kodeeksemplene skal bruke de samme navnene som
   prosjektet klassen jobber i (`rader`, `skjema`, `lagRad`, `/api/spill` …).
2. **Lag `<mappe>/index.html`** ut fra malen. Kopier byggeklossene fra
   `javascript/` eller `forms/`.
3. **Legg foredraget øverst i lista** i `index.html` på rota.
4. **Sjekk før du sier at du er ferdig:**
   - Gå gjennom alle slidene, og se at konsollen er fri for feil.
   - Klikk i alle de levende demoene.
   - Gå forover og bakover gjennom hver kjøring og hvert DOM-steg.
   - Ta skjermbilder i 1280 × 800, og se etter kode som kuttes og innhold som
     havner under kanten.
5. **Ikke rør andre foredrag,** og ikke commit uten å bli bedt om det.
   Brukeren har ofte ulagrede endringer i arbeidstreet.
