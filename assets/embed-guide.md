# Embed Guide – by Musti

Es gibt noch kein Website-Projekt. Diese Snippets bauen die Assets direkt ein. Pfade gehen davon aus, dass der Ordner `assets/` neben der `index.html` liegt. Für die Website immer die Dateien aus `assets/web/` nehmen, die PNGs in den anderen Ordnern sind die Originale für Druck und Bearbeitung.

Kein Video im Paket: Das Hero ist ein Standbild mit Haarstaub-Overlay.

## 1. Design-Tokens und Schriften

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Barlow:wght@400;500&family=Barlow+Condensed:wght@700;800&display=swap" rel="stylesheet">
```

```css
:root {
  --musti-gruen: #7DD864;
  --lichtkante:  #B4F29A;
  --lack-schwarz:#101012;
  --spiegel-grau:#2B302F;
  --milchglas:   #D7DAD6;
  --ziegel-weiss:#F5F5F2;

  --font-head: "Barlow Condensed", system-ui, sans-serif;
  --font-body: "Barlow", system-ui, sans-serif;
  --radius-pixel: 4px; /* Ecke der Pixel-Quadrate */
}
body {
  margin: 0;
  background: var(--lack-schwarz);
  color: var(--ziegel-weiss);
  font-family: var(--font-body);
}
h1, h2, h3, .preis {
  font-family: var(--font-head);
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: 0.02em;
}
```

## 2. Navigation mit Logo und Favicon

Das Logo kommt später aus VectorCraft. Bis dahin steht hier ein Platzhalterpfad.

```html
<link rel="icon" href="assets/logo/by-musti-favicon.svg" type="image/svg+xml">

<nav class="nav">
  <a href="/" class="nav__logo">
    <img src="assets/logo/by-musti.svg" alt="by Musti Friseur" height="44">
  </a>
  <a href="#leistungen">Leistungen</a>
  <a href="#preise">Preise</a>
  <a href="#kontakt" class="btn">Anfahrt</a>
</nav>
```

```css
.nav {
  position: sticky; top: 0; z-index: 10;
  display: flex; align-items: center; gap: 1.5rem;
  padding: 0.75rem 1.5rem;
  background: rgb(16 16 18 / 0.72);
  backdrop-filter: blur(14px);
  border-bottom: 2px solid var(--musti-gruen); /* die Lichtkante */
}
.nav a { color: var(--ziegel-weiss); text-decoration: none; }
.nav__logo { margin-right: auto; }
.btn {
  display: inline-block;
  text-decoration: none;
  background: var(--musti-gruen);
  color: var(--lack-schwarz);
  padding: 0.6rem 1.1rem;
  border-radius: var(--radius-pixel);
  font-weight: 500;
}
.btn:hover { background: var(--lichtkante); }
@media (max-width: 720px) {
  .nav { gap: 1rem; padding: 0.6rem 1rem; }
  .nav > a:not(.nav__logo):not(.btn) { display: none; }
}
```

## 3. Hero: Banner, Haarstaub-Overlay, Pixel-Quadrate

Das Banner hat rechts eine dunkle Freifläche, dort steht die Headline. Der Haarstaub liegt mit `mix-blend-mode: screen` darüber, dadurch verschwindet sein schwarzer Hintergrund.

```html
<header class="hero">
  <img class="hero__bild" src="assets/web/banner-hero-salon.webp"
       alt="Leerer Friseurstuhl unter grüner Lichtkante" fetchpriority="high">
  <img class="hero__staub" src="assets/web/extras-overlays-haarstaub.webp" alt="" aria-hidden="true">
  <div class="hero__pixel" aria-hidden="true"></div>
  <div class="hero__text">
    <h1>Herrenfriseur in Worms</h1>
    <p>Ohne Termin, Dienstag bis Samstag von 9 bis 19 Uhr.</p>
    <a href="#kontakt" class="btn">So findest du uns</a>
  </div>
</header>
```

```css
.hero { position: relative; min-height: 88vh; overflow: hidden; display: grid; }
.hero__bild, .hero__staub {
  position: absolute; inset: 0; width: 100%; height: 100%; object-fit: cover;
}
.hero__bild { object-position: left center; }
.hero__staub { mix-blend-mode: screen; opacity: 0.45; pointer-events: none; }
.hero__pixel { /* zwei Reihen Pixel-Quadrate oben rechts, wie im Schaufenster */
  position: absolute; top: 0; right: 0; width: 45%; height: 48px;
  background: var(--pixel-svg) repeat;
  opacity: 0.85;
}
.hero__text {
  position: relative; align-self: center; justify-self: end;
  width: min(36rem, 90%); margin-right: 6vw;
}
.hero__text h1 { font-size: clamp(2.6rem, 6vw, 5rem); line-height: 0.95; margin: 0 0 1rem; }
@media (max-width: 720px) {
  .hero__text { justify-self: center; align-self: end; margin: 0 0 3rem; }
  .hero__bild { opacity: 0.55; }
}
```

## 4. Pixel-Fries (das Quadrat-Muster aus dem Schaufenster)

Als CSS statt als Bild, damit es scharf bleibt und nichts kostet. Passt über das Hero, über Sektionsgrenzen oder unter Überschriften.

```html
<div class="pixel-fries" aria-hidden="true"></div>
```

Eine Kachel ist 28 × 28 px, darin ein Quadrat von 20 × 20 px mit 4 px Eckenradius. Die Variable gehört zu den Tokens in `:root`:

```css
:root {
  --pixel-svg: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='28' height='28'%3E%3Crect width='20' height='20' rx='4' fill='%237DD864'/%3E%3C/svg%3E");
}
.pixel-fries {
  height: 48px;              /* = 2 Reihen; 20px = 1 Reihe */
  background: var(--pixel-svg) repeat;
}
.pixel-fries--weiss {        /* weiße Variante wie auf dem Milchglas */
  height: 48px;
  background: var(--pixel-svg) repeat;
  filter: brightness(0) invert(1);
  opacity: 0.6;
}
```

## 5. Leistungskarten

```html
<section id="leistungen" class="leistungen">
  <h2>Leistungen</h2>
  <div class="karten">
    <article class="karte">
      <img src="assets/web/heroes-fade.webp" alt="Haarmaschine am Übergang im Nacken" loading="lazy">
      <h3>Maschinenschnitt</h3><p class="preis">14 €</p>
    </article>
    <article class="karte">
      <img src="assets/web/heroes-bart.webp" alt="Trimmer zieht die Bartkontur" loading="lazy">
      <h3>Bart formen</h3><p class="preis">10 €</p>
    </article>
    <article class="karte">
      <img src="assets/web/heroes-komplett.webp" alt="Rasur mit heißem Handtuch und Schaum" loading="lazy">
      <h3>Komplett mit Bartrasur</h3><p class="preis">28 €</p>
    </article>
    <article class="karte">
      <img src="assets/web/heroes-augenbrauen.webp" alt="Augenbrauen mit Fadentechnik" loading="lazy">
      <h3>Augenbrauen mit Faden</h3><p class="preis">7 €</p>
    </article>
  </div>
</section>
```

Die Preise stehen so auf der Preistafel im Schaufenster. Vor dem Livegang bitte prüfen.

```css
.leistungen { padding: 5rem 1.5rem; }
.karten {
  display: grid; gap: 1rem;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
}
.karte {
  background: var(--spiegel-grau);
  border-radius: var(--radius-pixel);
  overflow: hidden;
  border-top: 2px solid transparent;
  transition: border-color .2s;
}
.karte:hover { border-top-color: var(--musti-gruen); }
.karte img { width: 100%; aspect-ratio: 1; object-fit: cover; display: block; }
.karte h3 { margin: 1rem 1rem 0.25rem; font-size: 1.4rem; }
.karte .preis { margin: 0 1rem 1rem; color: var(--musti-gruen); font-size: 1.6rem; }
```

## 6. Helle Sektion mit Milchglas-Textur

Für Preise, Öffnungszeiten oder Kontakt. Die Textur hat ihr Pixel-Raster oben rechts, Text deshalb links setzen.

```html
<section id="preise" class="milchglas">
  <div class="milchglas__inhalt">
    <h2>Preise</h2>
    <!-- Preisliste -->
  </div>
</section>
```

```css
.milchglas {
  position: relative;
  background: var(--milchglas) url("assets/web/extras-texturen-milchglas-pixel.webp") center / cover no-repeat;
  color: var(--lack-schwarz);
  padding: 5rem 1.5rem;
}
.milchglas::before { /* hält die Textur leise genug für Text */
  content: ""; position: absolute; inset: 0;
  background: rgb(245 245 242 / 0.35);
}
.milchglas__inhalt { position: relative; max-width: 40rem; }
```

## 7. Werkzeug-Flatlay als Trenner

Das Flatlay hat links das Werkzeug und rechts Freifläche. Gut für einen „Über uns“-Block mit Text rechts.

```html
<section id="kontakt" class="werkzeug">
  <div class="werkzeug__text">
    <h2>Hier findest du uns</h2>
    <p>Hafergasse 2A, 67547 Worms<br>Telefon 06241 3969654<br>Di–Sa 9–19 Uhr, montags geschlossen</p>
  </div>
</section>
```

```css
.werkzeug {
  min-height: 60vh; display: grid; align-items: center;
  background: var(--lack-schwarz) url("assets/web/detail-werkzeug-flatlay.webp") left center / cover no-repeat;
  padding: 4rem 1.5rem;
}
.werkzeug__text { justify-self: end; width: min(28rem, 100%); margin-right: 6vw; }
@media (max-width: 720px) {
  .werkzeug { background-position: 20% center; }
  .werkzeug__text {
    justify-self: stretch; width: auto; margin: 0;
    background: rgb(16 16 18 / 0.75); padding: 1.25rem; border-radius: var(--radius-pixel);
  }
}
```
