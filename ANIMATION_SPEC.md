# Side Project - Animation Specification

Documento di recap per sviluppo e manutenzione delle animazioni del sito.

## Stack Animazioni

- GSAP `3` per animazioni timeline e reveal.
- ScrollTrigger per animazioni legate allo scroll.
- SplitType per reveal dei testi parola per parola.
- Lenis per smooth scroll globale.
- CSS transitions/keyframes per micro-interazioni, hero intro, menu e hover.

Tutte le animazioni devono rispettare `prefers-reduced-motion: reduce`: in quel caso evitare motion complessa, scrub e blur.

## Smooth Scroll

Lo scroll globale usa Lenis:

- `duration: 1.18`
- easing esponenziale custom
- `wheelMultiplier: 0.86`
- `touchMultiplier: 1.15`

Lenis deve aggiornare ScrollTrigger ad ogni evento scroll. Se Lenis non è disponibile, resta attivo il fallback nativo.

## Cursor Custom

Il cursore è composto da:

- dot centrale;
- asse verticale full viewport;
- asse orizzontale full viewport.

Stati principali:

- Default: dot arancione piccolo, assi grigio/nero molto leggeri.
- Menu hover: il dot viene nascosto, il blob arancione del menu appare sopra il testo.
- Work card hover: dot diventa pill “VIEW CASE”.
- Why/method link hover: dot grande arancione, `mix-blend-mode: multiply`.
- Logo hover: dot leggermente più grande, `mix-blend-mode: multiply`.
- Footer: dot nero. Sui link footer `mix-blend-mode: saturation`.

## Menu

Il menu ha due stati:

- Espanso: pill bianca full width, logo a sinistra, voci menu centrate, Contacts a destra.
- Compatto: pill piccola con logo e bottone hamburger arancione.

Comportamento:

- Scroll verso il basso: dopo un breve delay il menu si comprime.
- Scroll verso l’alto o vicino al top: il menu si espande.
- La transizione principale usa `800ms cubic-bezier(0.66, 0, 0.16, 1)`.
- Durante il morph, le voci menu devono restare centrate e Contacts allineato a destra.
- Nel footer il menu viene nascosto (`body.footer-revealed`).

## Hero Intro

La hero ha headline su tre righe:

- `HOW [pill] CRAZY`
- `[CAN YOUR] [pill]`
- `[pill] SPOT BE *`

Animazioni iniziali:

- I testi entrano con maschera orizzontale da sinistra.
- I pill arancioni nascono con espansione orizzontale.
- La foto sale dal basso con fade e scale leggermente ridotta.

Timing:

- Testi: `720ms cubic-bezier(0.76, 0, 0.24, 1)`.
- Pill 1: `900ms cubic-bezier(0.8, 0, 0.12, 1)`, delay `640ms`.
- Pill 2: `860ms cubic-bezier(0.8, 0, 0.12, 1)`, delay `820ms`.
- Pill 3: `760ms cubic-bezier(0.8, 0, 0.12, 1)`, delay `960ms`.
- Immagine: `900ms cubic-bezier(0.16, 1, 0.3, 1)`, delay `520ms`.

Note importanti:

- I pill sono fluidi in larghezza e devono respirare al resize.
- La maschera testo non deve tagliare lettere estreme come `W` e `Y`.
- L’asterisco è molto grande, sottile (`font-weight: 100`) e avvicinato alla `E`.

## Hero Scroll

Durante lo scroll della hero:

- La foto si ingrandisce progressivamente.
- La foto passa sopra il testo.
- Il testo hero va in fade out.
- I pill si restringono progressivamente per riavvicinare le parole.
- Lo scroll indicator va in fade out.

Implementazione:

- La sezione hero è alta `190vh`.
- `.hero-stage` è sticky a `100vh`.
- La foto interpola tra dimensioni iniziali e quasi full viewport con margini laterali di circa `80px`.
- Progress smoothing custom: `smoothProgress += (targetProgress - smoothProgress) * 0.11`.

## Text Reveal About / Why

I testi lunghi usano SplitType su parole:

- Stato iniziale: `opacity: 0.16`, `y: 18`.
- Stato finale: `opacity: 1`, `y: 0`.
- `stagger: 0.035`.
- ScrollTrigger scrub: `0.9`.

Obiettivo visivo:

- Reveal progressivo parola per parola.
- Niente crop laterali.
- Niente blocchi improvvisi o reveal “a lama”.
- Font weight del body about: `400`.

## Selected Works

Le card entrano allo scroll:

- Iniziale: `autoAlpha: 0`, `y: 86`, `scale: 0.965`, `filter: blur(12px)`.
- Finale: `autoAlpha: 1`, `y: 0`, `scale: 1`, `filter: blur(0px)`.
- ScrollTrigger: `start: top 92%`, `end: top 62%`, `scrub: 0.8`.
- Delay alternato leggero: `(index % 2) * 0.08`.

Hover card:

- Cursor diventa pill “VIEW CASE”.
- Immagine zooma a `scale(1.095)`.
- Pan dinamico in base al mouse: circa `24px` X e `18px` Y.
- Tilt dinamico: `rotateX/rotateY` fino a circa `7deg`.
- Al mouse leave, pan e tilt tornano a zero.

## CTA Linguetta

La CTA sotto la griglia lavori deve sembrare una linguetta che scende dalla griglia.

Layout:

- `margin-top` negativo proporzionale, base `-104px` su 1512.
- Altezza CTA base `330px` su 1512.
- Padding top base `84px` su 1512.
- Border radius solo inferiore: `0 0 48px 48px`.

Reveal:

- Parte invisibile: `autoAlpha: 0`.
- Parte risucchiata verso l’alto: `y: -118`, `scaleY: 0.58`.
- Scende/apre fino a `y: 0`, `scaleY: 1`.
- Trigger ritardato: `start: top 72%`, `end: top 42%`, `scrub: 0.85`.
- Il bottone entra dopo, da `autoAlpha: 0`, `y: -24`, `scale: 0.96`.

Hover bottone:

- Flip/rotazione 3D su asse X.
- Testo default: “This might be your project”.
- Testo hover: “Contact Us”.
- Hover background nero, testo arancione.

## Why Us Links

Hover dei link:

- Il pallino a sinistra scompare.
- Il testo si sposta a sinistra occupando lo spazio lasciato dal pallino.
- Il cursore si ingrandisce sopra il testo con `mix-blend-mode: multiply`.
- La freccia pill si riempie in arancione e trasla leggermente a destra.

## Logos

Ingresso sezione:

- Titolo: parole in `autoAlpha: 0`, `yPercent: 45`, `blur(10px)`.
- Cards: `autoAlpha: 0`, `y: 72`, `scale: 0.96`, `blur(10px)`.
- Cards animate con scrub leggero (`0.75`) e start leggermente sfalsato per colonna.

Hover loghi:

- Card cambia leggermente background/bordo.
- Logo interno scala a `1.06`.
- Cursore si ingrandisce leggermente e usa `mix-blend-mode: multiply`.

## Method

Titolo:

- Parti del titolo entrano con `autoAlpha: 0`, `yPercent: 45`, `blur(10px)`.
- Dots arancioni entrano con scale da `0` a `1`, easing `elastic.out(1, 0.7)`.
- Il titolo orizzontale si muove in base allo scroll.

Contenuto:

- Aside e immagine entrano in reveal morbido.
- Immagine ha hover zoom leggero (`scale(1.045)`).

## Footer Reveal

Il footer è fixed dietro il contenuto e viene rivelato allo scroll.

Struttura:

- `.site-footer` fixed, `bottom: 0`, height `100vh`.
- `.page-content` sopra con `margin-bottom: var(--footer-reveal-height)`.
- Quando il footer entra in reveal, il menu principale viene nascosto.

Hover link footer:

- Pallino sinistro scompare.
- Freccia trasla leggermente.
- Cursore nero con blend `saturation`.
- Non deve cambiare background della cella.

## Responsive 2XL / 2560

Breakpoint dedicato: `min-width: 2200px`.

Hero:

- Nav max width `1920px`.
- Testo hero circa `198px`.
- Immagine hero circa `1818px x 985px`.
- Scroll indicator più grande e posizionato a destra immagine.

Selected Works:

- Griglia full width, non max-width 1440.
- Totale card: 8.

## Note di Manutenzione

- Evitare animazioni che modificano proprietà layout-costose in loop, tranne dove già controllato.
- Preferire `transform`, `opacity`, `filter` solo con attenzione.
- Ogni nuova animazione scroll deve chiamare/rispettare `ScrollTrigger.refresh()` dopo load quando necessario.
- Non cambiare il footer reveal in `position: static`: la logica attuale richiede footer fixed dietro `.page-content`.
- Non rimuovere `history.scrollRestoration = 'manual'`: serve a evitare aperture pagina in posizioni scroll inconsistenti.
