# Munin Diagnostics — nettsted

Statisk nettsted for Munin Diagnostics. To sider, ingen backend, ingen byggeprosess.
Bare HTML, bilder og én video. Hostes gratis på GitHub Pages.

---

## Filstruktur

```
munin-web/
├── index.html          ← forsiden (landingsside)
├── logg.html           ← fremgang/loggbok
├── CNAME               ← kobler munin-diagnostics.com til siden (ikke slett)
├── README.md           ← denne fila
├── images/
│   ├── hero.jpg        ← bakgrunn øverst på forsiden (ravn + bil)
│   ├── mood.jpg        ← stemningsbånd midt på forsiden (bilsilhuett)
│   ├── device.jpg      ← plakat/fallback for videoen
│   └── logg/           ← legg loggbilder her (se under)
└── video/
    └── device.mp4      ← x-ray-bilen i den tekniske seksjonen
```

Strukturen må holdes som den er — `index.html` forventer at bildene ligger i `images/`
og videoen i `video/`. Flytter du filer, må stiene i HTML-en endres tilsvarende.

---

## Oppdatere fremgangsloggen (logg.html)

All loggdata ligger i én liste øverst i `<script>`-blokken i `logg.html`.
Søk etter `const LOGG = [` og legg til nye oppføringer der. Nyeste øverst.

En oppføring ser slik ut:

```js
{
  dato:"Juli 2026",
  type:"rapport",                       // "rapport" | "milepael" | "bilde"
  tittel:"Kort overskrift",
  bil:"Freelander 2 · TD4",             // valgfritt — sett "" for å utelate
  tekst:"Beskrivelse i én til tre setninger.",
  bilde:"images/logg/min-fil.jpg",      // valgfritt — sett "" for ingen bilde
  lenke:{ tekst:"Les rapporten", url:"rapporter/min-rapport.pdf" }  // valgfritt — sett null for ingen lenke
},
```

- **type** styrer fargen på prikken og taggen: `milepael` blir gull, resten blå.
- **bilde**: legg bildefila i `images/logg/` og skriv stien som over.
- **lenke**: vil du lenke til en PDF-rapport, lag en mappe `rapporter/`, legg PDF-en der,
  og pek `url` dit. Sett `lenke:null` hvis ingen lenke.

Lagre fila, last opp på nytt (commit/push, eller dra-og-slipp), ferdig.

---

## Endre tekst på forsiden

All tekst står rett i `index.html` som vanlig HTML — ingen skjult datakilde.
Søk etter teksten du vil endre og rediger den direkte.

---

## Bytte ut bilder eller video

Bytt fila i `images/` eller `video/` med samme filnavn, så plukker siden den opp automatisk.
Vil du bruke et annet filnavn, må du oppdatere stien i `index.html`.

Komprimer alltid bilder før opplasting (mål: under ~150 KB hver) — besøkende kan komme
inn over mobilnett. Nåværende vekt: hero 91 KB, mood 38 KB, device-video 0,9 MB.

---

## Publisere på GitHub Pages

1. Opprett et nytt repo på github.com (f.eks. `munin-web`).
2. Last opp hele innholdet i denne mappa (dra-og-slipp via «uploading an existing file»,
   eller `git push`).
3. Repo → **Settings → Pages** → velg `main`-branch som kilde → Save.
4. Siden er live på `https://<brukernavn>.github.io/munin-web` etter et par minutter.

## Koble til munin-diagnostics.com

`CNAME`-fila i denne mappa forteller GitHub at domenet er `munin-diagnostics.com`.
For at det skal virke, må DNS hos one.com peke mot GitHub:

- **CNAME-record:** `www` → `<brukernavn>.github.io`
- **A-records (apex):** `@` → GitHub sine fire IP-er
  (GitHub viser de eksakte verdiene under Settings → Pages når du legger inn domenet)

**Viktig:** Disse DNS-endringene rører IKKE e-posten. MX- og TXT-recordsene for
rapport-e-posten (Resend/SES) skal stå urørt. Legg til de nye web-recordsene ved siden av
de eksisterende e-post-recordsene — ikke slett noe.

Når GitHub har verifisert domenet, huk av «Enforce HTTPS» under Settings → Pages.
Da får siden gratis SSL-sertifikat.
