# Side Project - Animation Handoff

Documento operativo per lo sviluppo e la manutenzione delle animazioni del sito Side Project.

## Obiettivo

Il sito deve avere un comportamento molto fluido, editoriale e materico: scroll morbido, reveal progressivi, hover con cursore custom e transizioni mai brusche. Le animazioni devono valorizzare il layout senza far percepire salti, crop o cambiamenti improvvisi di stato.

## Stack

- GSAP `3` per timeline, reveal e micro-interazioni.
- ScrollTrigger per animazioni legate allo scroll.
- SplitType per reveal parola per parola.
- Lenis per smooth scroll globale.
- CSS transitions/keyframes per intro hero, menu morph e hover semplici.

## Regole Globali

- Rispettare sempre `prefers-reduced-motion: reduce`.
- Preferire `transform` e `opacity`.
- Usare `filter` con moderazione e solo su elementi non troppo grandi.
- Evitare cambi layout improvvisi durante transizioni di scroll.
- Dopo cambi dinamici di immagini/font/layout, chiamare `ScrollTrigger.refresh()` quando necessario.
- Non rimuovere `history.scrollRestoration = 'manual'`, serve a evitare aperture pagina in posizioni scroll inconsistenti.

## Smooth Scroll

Lo scroll globale usa Lenis.

Configurazione:

- `duration: 1.18`
- `wheelMultiplier: 0.86`
- `touchMultiplier: 1.15`
- easing esponenziale custom

Lenis deve aggiornare ScrollTrigger ad ogni evento scroll. Se Lenis non è disponibile, deve rimanere attivo il fallback nativo.

## Cursore Custom

Il cursore è composto da:

- dot centrale;
- asse verticale full viewport;
- asse orizzontale full viewport.

Stati:

- Default: dot arancione piccolo, assi sottili e opachi.
- Menu hover: blob arancione sopra il testo, con `mix-blend-mode: multiply`.
- Work card hover: morph in pill “VIEW CASE”.
- Why/method/link hover: dot grande arancione, `mix-blend-mode: multiply`.
- Logo hover: dot leggermente più grande, `mix-blend-mode: multiply`.
- Footer: dot nero, sui link footer `mix-blend-mode: saturation`.

Note:

- Le linee del cursore devono restare dietro al dot.
- Il dot deve avere transizioni morbide di size/morph.
- Nei footer link non deve cambiare il colore di background della cella.

## Menu

Il menu ha due stati principali.

Stato espanso:

- pill bianca full width;
- logo a sinistra;
- voci menu centrate;
- `Contacts` allineato a destra.

Stato compatto:

- pill piccola centrata;
- logo a sinistra;
- bottone hamburger arancione a destra;
- ombra morbida.

Comportamento:

- Scroll verso il basso: dopo un leggero delay il menu si compatta.
- Scroll verso l’alto o vicino al top: il menu torna espanso.
- Il morph deve essere continuo, non a scatto.
- Durante la compressione le voci menu devono restare centrate e `Contacts` deve rimanere allineato a destra finché non scompare.
- Nel footer il menu principale deve sparire.

Timing consigliato:

- durata morph: circa `1800ms`;
- curva: morbida, con finale rallentato;
- evitare overshoot violenti.

## Hero Intro

Headline:

- `HOW [pill] CRAZY`
- `[CAN YOUR] [pill]`
- `[pill] SPOT BE *`

Animazione:

- Prima entrano i testi con maschera orizzontale.
- Poi entrano i pill che separano/spostano visivamente le parole.
- La foto entra dal basso con fade e scale leggermente ridotta.

Timing:

- Testi: `720ms cubic-bezier(0.76, 0, 0.24, 1)`.
- Pill 1: `900ms cubic-bezier(0.8, 0, 0.12, 1)`, delay `640ms`.
- Pill 2: `860ms cubic-bezier(0.8, 0, 0.12, 1)`, delay `820ms`.
- Pill 3: `760ms cubic-bezier(0.8, 0, 0.12, 1)`, delay `960ms`.
- Foto: `900ms cubic-bezier(0.16, 1, 0.3, 1)`, delay `520ms`.

Note visive:

- I pill devono essere fluidi in larghezza al resize.
- Le lettere estreme come `W` e `Y` non devono mai sembrare tagliate.
- L’asterisco deve essere grande, leggero e vicino alla `E`.

## Hero Scroll

Durante lo scroll:

- la foto si ingrandisce progressivamente;
- la foto passa sopra il testo;
- il testo va in fade out;
- tutti i pill si restringono e riavvicinano le parole;
- lo scroll indicator va in fade out.

Implementazione:

- hero height: circa `190vh`;
- `.hero-stage` sticky a `100vh`;
- immagine verso quasi full viewport con margini laterali di circa `80px`;
- smoothing progressivo custom: `smoothProgress += (targetProgress - smoothProgress) * 0.11`.

## About e Why Text Reveal

I testi lunghi usano SplitType su parole.

Stato iniziale:

- `opacity: 0.16`;
- `y: 18`;
- leggero blur solo se non crea crop.

Stato finale:

- `opacity: 1`;
- `y: 0`;
- testo pulito, senza tagli laterali.

Timing:

- `stagger: 0.035`;
- ScrollTrigger scrub: `0.9`.

Obiettivo:

- reveal progressivo parola per parola;
- niente blocchi improvvisi;
- niente maschere che tagliano il testo;
- body about in `font-weight: 400`.

## Selected Works

Reveal card:

- iniziale: `autoAlpha: 0`, `y: 86`, `scale: 0.965`, `filter: blur(12px)`;
- finale: `autoAlpha: 1`, `y: 0`, `scale: 1`, `filter: blur(0px)`;
- ScrollTrigger: `start: top 92%`, `end: top 62%`, `scrub: 0.8`;
- delay alternato leggero: `(index % 2) * 0.08`.

Hover card:

- cursore diventa pill “VIEW CASE”;
- testo cursor: `16px`;
- padding pill cursor: `top 8px`, `right 10px`, `bottom 8px`, `left 10px`;
- immagine zooma a `scale(1.095)`;
- pan dinamico mouse: circa `24px` X e `18px` Y;
- tilt dinamico fino a circa `7deg`.

Responsive:

- su desktop molto larghi la griglia mantiene il comportamento full width;
- a 2560px devono essere presenti 8 card.

## CTA Linguetta

La CTA sotto la griglia works deve sembrare una linguetta che scende dalla griglia.

Layout:

- `margin-top` negativo, base `-104px` su viewport 1512;
- altezza base `330px` su viewport 1512;
- padding top base `84px` su viewport 1512;
- border radius solo inferiore: `0 0 48px 48px`.

Reveal:

- parte da `autoAlpha: 0`;
- parte dall’alto con `y: -118` e `scaleY: 0.58`;
- scende fino a `y: 0` e `scaleY: 1`;
- trigger ritardato: `start: top 72%`, `end: top 42%`;
- scrub: `0.85`;
- il bottone entra dopo con `autoAlpha: 0`, `y: -24`, `scale: 0.96`.

Hover bottone:

- flip/rotazione 3D;
- default: “This might be your project”;
- hover: “Contact Us”;
- hover background nero, testo arancione.

## Why Us Links

Hover:

- il pallino a sinistra scompare;
- il testo si sposta a sinistra occupando lo spazio del pallino;
- il cursore si ingrandisce sopra il testo;
- `mix-blend-mode: multiply`;
- lo sfondo cella diventa `#F6F6F6`;
- la freccia pill si riempie in arancione e trasla leggermente.

## Logos

Reveal:

- titolo: parole da `autoAlpha: 0`, `yPercent: 45`, `blur(10px)`;
- card: `autoAlpha: 0`, `y: 72`, `scale: 0.96`, `blur(10px)`;
- scrub leggero;
- start sfalsato per colonna.

Hover:

- card con leggero cambio background/bordo;
- logo interno scala a `1.06`;
- cursore si ingrandisce leggermente;
- `mix-blend-mode: multiply`.

## Method

Titolo:

- parti del titolo da `autoAlpha: 0`, `yPercent: 45`, `blur(10px)`;
- dots arancioni con scale `0` -> `1`;
- easing `elastic.out(1, 0.7)`;
- titolo orizzontale che si muove in base allo scroll.

Contenuto:

- aside e immagine entrano con reveal morbido;
- immagine con hover zoom leggero `scale(1.045)`;
- headline “OUR METHOD YOUR SUCCESS” scrolla orizzontalmente durante lo scroll.

## Footer Reveal

Il footer è fixed dietro il contenuto e viene rivelato allo scroll.

Struttura:

- `.site-footer` fixed;
- `bottom: 0`;
- height `100vh`;
- `.page-content` sopra con `margin-bottom: var(--footer-reveal-height)`.

Comportamento:

- il contenuto della pagina scorre sopra;
- il footer resta dietro e viene rivelato;
- quando il footer è in reveal, il menu principale viene nascosto.

Hover link footer:

- cursore nero;
- `mix-blend-mode: saturation`;
- pallino sinistro scompare;
- freccia trasla leggermente;
- nessun cambio background della cella.

## Responsive 2XL / 2560

Breakpoint dedicato: `min-width: 2200px`.

Hero:

- nav max width circa `1920px`;
- testo hero circa `198px`;
- immagine hero circa `1818px x 985px`;
- scroll indicator posizionato a destra dell’immagine.

Works:

- griglia full width;
- 8 card visibili nel dataset;
- non limitare la griglia a `1440px`.

## Checklist QA

- Il menu non deve saltare durante la compressione.
- Le voci menu restano centrate finché visibili.
- `Contacts` resta allineato a destra finché visibile.
- La hero non deve tagliare lettere o asterisco.
- I pill hero devono respirare al resize.
- La foto hero deve scrollare sopra al testo in modo morbido.
- I reveal testo devono essere progressivi, non a blocchi.
- Le card works entrano una dopo l’altra allo scroll.
- Hover card: zoom, pan e tilt devono essere percepibili ma eleganti.
- CTA linguetta deve partire invisibile e scendere dall’alto.
- Footer reveal non deve bloccare lo scroll.
- Nel footer il menu principale deve sparire.
- Il cursore deve cambiare stato coerentemente per menu, works, loghi, footer.

