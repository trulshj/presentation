# Foredragene på prez.truls.dev

Repoet er en reveal.js 5-fork med ett foredrag per mappe (`forms/`, `javascript/`,
`fetch/`, `japanese/` …). Lista over foredrag står i `index.html` på rota.

**Les [STIL.md](STIL.md) før du lager eller endrer et foredrag.** Der står:
- tonen: spørsmål som titler, «Gjett:», ett eksempel hele veien
- den tekniske malen og CSS-rettingene som alltid skal med
- byggeklossene: kjøringer, levende demoer, oppbygging med auto-animate
- fallgruvene vi allerede har gått i

Kort:
- **Hvert foredrag er én selvstendig `index.html`.** Kopier byggeklosser fra
  `javascript/` eller `forms/`.
- **Ikke rør** `dist/`, `plugin/`, `css/` eller `js/`. De hører til reveal.js.
- **Norsk bokmål** i norske foredrag.
- **Forhåndsvis med** `python3 -m http.server 8001 --directory .`
- **Sjekk før du er ferdig:** at alle slidene viser seg uten feil i konsollen,
  at demoene virker når du klikker, og at kjøringene går både forover og
  bakover.
- **Ikke commit uten å bli bedt om det,** og ikke rør andre foredrag enn det du
  jobber med.
