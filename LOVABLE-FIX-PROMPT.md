# LOVABLE FIX PROMPT — Zemp Sanitär Build verbessern

> **Current Build:** https://zemp.lovable.app/
> **Original Master:** https://zemp-sanitar.onepage.me/homepage-version-1
> **Auftrag:** Bestehenden Build verbessern. NICHT von vorne bauen. Stack, Routes, Assets und Content bleiben. Folgende konkrete Fehler beheben:

---

## 0. SOFORT: Vergleichs-Workflow

Öffne in 2 Tabs nebeneinander:
- **Dein Build:** https://zemp.lovable.app/
- **Original (Wahrheit):** https://zemp-sanitar.onepage.me/homepage-version-1

Scroll parallel durch beide. Jeder Fehler unten ist 1:1 aus dem Vergleich abgeleitet. **Wo Original ≠ Build, ist Build falsch.**

Asset-Base (alle Bilder + Section-Reference-Screenshots, kein Upload nötig):
```
ASSETS:    https://cdn.jsdelivr.net/gh/rhvisuals/zemp-sanitaer-onepage-assets@main/images/
REFS:      https://cdn.jsdelivr.net/gh/rhvisuals/zemp-sanitaer-onepage-assets@main/reference-screenshots/
```

---

## FEHLER 1 — HEADER: Logo + CTA fehlen / falsch

**Problem (Build):**
- Logo nur als 1-zeilige Wortmarke „zemp sanitär".
- Rechts vom Telefon ist eine **leere Search-Bar** statt einer CTA-Pill.

**Original:**
- Logo ist 2-zeilig: „**zemp**" (lowercase, fett, sehr groß) + „**SANITÄR AG**" (uppercase, klein, darunter). Plus subtiles Z-Wellen-Logo-Mark.
- Rechts: Telefon-Link **+** „**Beratung anfragen**" als **weiße Pill** mit blauem Text.

**Fix:**

```tsx
// src/components/Header.tsx

<a href="#" className="flex items-baseline gap-1.5" aria-label="Zemp Sanitär AG">
  <span className="text-[26px] lg:text-[28px] font-extrabold tracking-tight text-white leading-none"
        style={{ fontFamily: "var(--font-head)" }}>
    zemp
  </span>
  <span className="text-[11px] lg:text-[12px] font-extrabold tracking-[0.18em] text-white/95 uppercase leading-none"
        style={{ fontFamily: "var(--font-head)" }}>
    Sanitär AG
  </span>
</a>

// Rechts: Telefon + CTA — KEINE Search-Bar
<div className="flex items-center gap-4">
  <a href="tel:0412603337" className="hidden sm:inline-flex items-center gap-2 text-[14px] font-semibold text-white/95 hover:text-white">
    <Phone className="h-4 w-4" />
    041 260 33 37
  </a>
  <a href="#kontakt"
     className="inline-flex h-10 items-center rounded-full bg-white px-5 text-[14px] font-extrabold text-[var(--color-blue)] hover:bg-white/95 transition-colors"
     style={{ fontFamily: "var(--font-head)" }}>
    Beratung anfragen
  </a>
</div>
```

Entferne die Search-Box komplett.

---

## FEHLER 2 — HERO: Komplett falsches Layout (KRITISCH)

**Problem (Build):**
- Hero hat **weißen Background**.
- Bild (Thomas) ist **klein, rechts als Card** mit rounded corners.
- Text **schwarz** auf weiß.
- **Trust-Strip fehlt komplett** (24H REAKTIONSZEIT · 15 FACHKRÄFTE · LUZERN & EMMEN · SUISSETEC MITGLIED).

**Original:**
- Hero ist ein **Full-Bleed-Hero**: Bild von Thomas in der Werkstatt füllt die GANZE Section als Background.
- Über dem Bild liegt ein **dunkler Gradient** von links (dunkel) nach rechts (transparent), damit der Text links lesbar bleibt.
- H1 + Body + CTAs sind **weiß** und auf der **linken Hälfte** des Bildes positioniert.
- Unter dem Hero kommt direkt ein **blauer Trust-Strip-Balken** mit 4 Items: „24H REAKTIONSZEIT", „15 FACHKRÄFTE", „LUZERN & EMMEN", „SUISSETEC MITGLIED" mit kleinen Icons davor.

**Fix — Hero.tsx komplett ersetzen:**

```tsx
import { motion } from "motion/react";
import { ArrowRight, Phone, Star, Clock, Users, MapPin, ShieldCheck } from "lucide-react";

const HERO_IMG = "https://cdn.jsdelivr.net/gh/rhvisuals/zemp-sanitaer-onepage-assets@main/images/01-hero-bg.jpg";

export function Hero() {
  return (
    <section id="hero" className="relative isolate overflow-hidden bg-[var(--color-dark)] text-white">
      {/* Background-Image full-bleed */}
      <div className="absolute inset-0 -z-10">
        <img
          src={HERO_IMG}
          alt="Thomas Weber in der Werkstatt von Zemp Sanitär"
          className="h-full w-full object-cover object-center"
          fetchPriority="high" loading="eager"
        />
        {/* Dunkler Gradient von links — Text-Lesbarkeit */}
        <div className="absolute inset-0 bg-gradient-to-r from-[rgba(15,23,36,0.92)] via-[rgba(15,23,36,0.65)] to-[rgba(15,23,36,0.10)]" />
      </div>

      {/* Content — linksbündig */}
      <div className="mx-auto max-w-[1400px] px-8 pt-20 pb-24 lg:pt-28 lg:pb-32 max-[720px]:px-5 max-[720px]:pt-16 max-[720px]:pb-20">
        <motion.div
          initial={{ opacity: 0, y: 24 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.7, ease: [0.16, 1, 0.3, 1] }}
          className="max-w-[640px]"
        >
          {/* Star-Rating */}
          <div className="flex items-center gap-2 text-[14px]">
            <span className="flex gap-0.5">
              {[1,2,3,4,5].map(i => (
                <Star key={i} className="h-[18px] w-[18px] fill-[var(--color-star)] text-[var(--color-star)]" />
              ))}
            </span>
            <span className="font-bold text-white">4,5/5</span>
            <span className="text-white/75">aus 16 Google Bewertungen</span>
          </div>

          {/* H1 — WEISS, fett, groß */}
          <h1 className="mt-7 text-[42px] lg:text-[64px] font-extrabold leading-[1.04] tracking-[-0.025em] text-white max-w-[620px] max-[720px]:text-[34px]">
            Familienbetrieb für Sanitär und Badsanierung in Luzern. Seit 1963.
          </h1>

          {/* Body */}
          <p className="mt-7 text-[17px] lg:text-[19px] leading-[1.6] text-white/85 max-w-[560px]">
            Thomas Weber und sein 15-köpfiges Team sanieren Bäder in der Agglomeration Luzern zum Festpreis. Immer die gleichen Monteure. Planung mit 3D-Ansicht. Koordination aller Gewerke unter einem Dach.
          </p>

          {/* CTAs */}
          <div className="mt-10 flex flex-wrap items-center gap-4">
            <a href="#kontakt"
               className="inline-flex h-14 items-center gap-2 rounded-full bg-[var(--color-blue)] px-7 text-[16px] font-extrabold text-white shadow-[var(--shadow-pill)] hover:bg-[var(--color-blue-dark)] hover:-translate-y-0.5 transition-all"
               style={{ fontFamily: "var(--font-head)" }}>
              Kostenlose Beratung anfragen
              <ArrowRight className="h-4 w-4" />
            </a>
            <a href="tel:0412603337"
               className="inline-flex items-center gap-2 text-[15px] font-bold text-white hover:text-white/85 transition-colors">
              <Phone className="h-4 w-4" />
              041 260 33 37
            </a>
          </div>
        </motion.div>
      </div>

      {/* TRUST-STRIP — blauer Balken UNTER Hero */}
      <div className="bg-[var(--color-blue)] text-white">
        <div className="mx-auto max-w-[1400px] px-8 py-5 max-[720px]:px-5">
          <ul className="flex flex-wrap items-center justify-between gap-x-6 gap-y-3 text-[13px] font-extrabold uppercase tracking-[0.1em]"
              style={{ fontFamily: "var(--font-head)" }}>
            <li className="flex items-center gap-2.5">
              <Clock className="h-4 w-4 text-white/85" strokeWidth={2.2} />
              24H REAKTIONSZEIT
            </li>
            <li className="flex items-center gap-2.5">
              <Users className="h-4 w-4 text-white/85" strokeWidth={2.2} />
              15 FACHKRÄFTE
            </li>
            <li className="flex items-center gap-2.5">
              <MapPin className="h-4 w-4 text-white/85" strokeWidth={2.2} />
              LUZERN & EMMEN
            </li>
            <li className="flex items-center gap-2.5">
              <ShieldCheck className="h-4 w-4 text-white/85" strokeWidth={2.2} />
              SUISSETEC MITGLIED
            </li>
          </ul>
        </div>
      </div>
    </section>
  );
}
```

**Visual-Reference dieser Section:** `https://cdn.jsdelivr.net/gh/rhvisuals/zemp-sanitaer-onepage-assets@main/reference-screenshots/ref-01-hero.png`

---

## FEHLER 3 — VORHER/NACHHER: Kein echter Slider

**Problem (Build):**
- 2 getrennte Bilder nebeneinander mit weißer Trennlinie und kleinem Mini-Slider-Handle ganz unten.
- Bilder überlappen NICHT, kein Drag-Effekt.

**Original:**
- **Drag-Slider** über die volle Container-Breite. Bilder überlappen via `clipPath`. Drag mit Maus + Touch.
- Großer runder Handle in der Mitte mit Doppelpfeil-Icon (⇆), schwebt auf der weißen vertikalen Trennlinie.

**Fix:** Tausche die Implementation gegen einen echten Drag-Slider (clipPath-basiert):

```tsx
import { useState, useRef, useEffect, type MouseEvent as RM, type TouchEvent as RT } from "react";
import { ChevronsLeftRight } from "lucide-react";

const VORHER = "https://cdn.jsdelivr.net/gh/rhvisuals/zemp-sanitaer-onepage-assets@main/images/03-vorher.jpg";
const NACHHER = "https://cdn.jsdelivr.net/gh/rhvisuals/zemp-sanitaer-onepage-assets@main/images/04-nachher.jpg";

export function VorherNachher() {
  const [pos, setPos] = useState(50);
  const containerRef = useRef<HTMLDivElement>(null);
  const dragging = useRef(false);

  const move = (clientX: number) => {
    if (!containerRef.current) return;
    const r = containerRef.current.getBoundingClientRect();
    setPos(Math.max(0, Math.min(100, ((clientX - r.left) / r.width) * 100)));
  };

  useEffect(() => {
    const up = () => { dragging.current = false; };
    window.addEventListener("mouseup", up);
    window.addEventListener("touchend", up);
    return () => {
      window.removeEventListener("mouseup", up);
      window.removeEventListener("touchend", up);
    };
  }, []);

  return (
    <section id="vorher-nachher" className="bg-white">
      <div className="mx-auto max-w-[1100px] px-8 py-24 lg:py-28 max-[720px]:px-5 max-[720px]:py-16">
        <header className="text-center mb-14">
          <p className="eyebrow mb-5">Vorher / Nachher</p>
          <h2 className="text-[32px] lg:text-[44px] font-extrabold leading-[1.1] tracking-[-0.02em] text-[var(--color-dark)] max-w-[760px] mx-auto">
            Badsanierung Luzern: So verwandeln wir Ihr Badezimmer
          </h2>
          <p className="mt-6 text-[16px] leading-[1.65] text-[var(--color-muted)] max-w-[620px] mx-auto">
            Jede Badsanierung beginnt mit einem Raum, der seine besten Jahre hinter sich hat. Was daraus wird, entscheiden Sie gemeinsam mit uns. Ziehen Sie den Slider, um den Unterschied zu sehen.
          </p>
        </header>

        <div
          ref={containerRef}
          onMouseMove={(e: RM<HTMLDivElement>) => dragging.current && move(e.clientX)}
          onMouseDown={(e: RM<HTMLDivElement>) => { dragging.current = true; move(e.clientX); }}
          onTouchMove={(e: RT<HTMLDivElement>) => e.touches[0] && move(e.touches[0].clientX)}
          onClick={(e: RM<HTMLDivElement>) => move(e.clientX)}
          className="relative aspect-[1280/853] max-[720px]:aspect-[4/5] overflow-hidden rounded-[24px] shadow-[var(--shadow-card)] cursor-ew-resize select-none touch-none"
        >
          <img src={NACHHER} alt="Nachher" className="absolute inset-0 w-full h-full object-cover" loading="lazy" />
          <img src={VORHER} alt="Vorher" className="absolute inset-0 w-full h-full object-cover"
               style={{ clipPath: `inset(0 ${100 - pos}% 0 0)` }} loading="lazy" />

          <span className="absolute top-5 left-5 rounded-full bg-[#0F1724]/85 px-4 py-1.5 text-[12px] font-extrabold uppercase tracking-[0.12em] text-white">VORHER</span>
          <span className="absolute top-5 right-5 rounded-full bg-[var(--color-blue)] px-4 py-1.5 text-[12px] font-extrabold uppercase tracking-[0.12em] text-white">NACHHER</span>

          <div className="absolute top-0 bottom-0 w-1 bg-white pointer-events-none"
               style={{ left: `${pos}%`, transform: "translateX(-50%)" }}>
            <span className="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 flex h-12 w-12 items-center justify-center rounded-full bg-white shadow-[var(--shadow-card-hover)]">
              <ChevronsLeftRight className="h-5 w-5 text-[var(--color-blue)]" strokeWidth={2.5} />
            </span>
          </div>
        </div>
      </div>
    </section>
  );
}
```

**Reference:** `https://cdn.jsdelivr.net/gh/rhvisuals/zemp-sanitaer-onepage-assets@main/reference-screenshots/ref-05-vorher-nachher.png`

---

## FEHLER 4 — ÜBER UNS: H2 unsichtbar (schwarz auf dunkel)

**Problem (Build):**
- Section-BG ist dunkel (#1A2335) — gut.
- ABER die Headline „Familiengeführter Sanitärbetrieb seit 1963" ist **schwarz** und damit auf dem dunklen BG **fast unsichtbar**.

**Original:**
- H2 ist **weiß** auf dunklem BG, voll lesbar.

**Fix:** In `UeberUns.tsx` der H2:

```tsx
<h2 className="mt-4 text-[30px] lg:text-[44px] font-extrabold leading-[1.1] tracking-[-0.02em] text-white max-w-[560px]">
  Familiengeführter Sanitärbetrieb seit 1963
</h2>
```

(`text-white` statt schwarz/dark.)

Plus: Body-Text in zwei separate `<p>` mit `space-y-5` (nicht ein zusammengeklebter Block).

---

## FEHLER 5 — LEISTUNGEN: Bild-Card-Layout falsch

**Problem (Build):**
- Cards haben Bild OBEN und Text-Block UNTEN getrennt (klassische Card).
- Pills („BADSANIERUNG", „KÜCHE", „REPARATUR", „GAS") sitzen oben auf dem Bild.

**Original:**
- Die GROSSE Card (Badsanierung) hat das Bild als **Card-Background** (über die ganze Card-Fläche), Text als **Overlay** unten mit dunklem Gradient.
- Die kleinen Cards (Küche/Service/Spezialisierung/Umbau) haben Bild oben + Text unten — das ist OK.
- ABER die Pills im Original heißen für die 3 kleineren Cards: „SERVICE" / „SPEZIALISIERUNG" / „UMBAU" (zusätzlich zu KÜCHE).

**Fix:** Hero-Card (Badsanierung) als Bild-Overlay-Card umbauen:

```tsx
<article className="relative overflow-hidden rounded-[24px] aspect-[2/1] lg:row-span-2">
  <img src="https://cdn.jsdelivr.net/gh/rhvisuals/zemp-sanitaer-onepage-assets@main/images/07-badsanierung-hero.jpg"
       alt="Modernes saniertes Badezimmer"
       className="absolute inset-0 w-full h-full object-cover" loading="lazy" />
  <span className="absolute top-5 left-5 rounded-full bg-[var(--color-blue)] px-3 py-1.5 text-[11px] font-extrabold uppercase tracking-[0.12em] text-white">
    Badsanierung
  </span>
  {/* Gradient-Overlay + Text unten */}
  <div className="absolute inset-x-0 bottom-0 bg-gradient-to-t from-black/85 via-black/40 to-transparent p-8 lg:p-10">
    <h3 className="text-[22px] lg:text-[28px] font-extrabold leading-[1.2] text-white max-w-[520px]">
      Badsanierung in Luzern mit 3D-Planung und Festpreis
    </h3>
    <p className="mt-4 text-[14px] lg:text-[15px] leading-[1.6] text-white/85 max-w-[540px]">
      Von der bodenebenen Dusche über beheizte Handtuchhalter bis zum Geberit AquaClean Dusch-WC: Wir planen Ihr neues Bad in 3D, koordinieren alle Gewerke und übergeben Ihnen ein fertiges Badezimmer zum Festpreis.
    </p>
    <a href="#kontakt" className="mt-5 inline-flex items-center gap-1.5 text-[13px] font-extrabold text-white hover:underline">
      Mehr dazu →
    </a>
  </div>
</article>
```

(Die 3 kleinen Cards Bild-oben/Text-unten bleiben wie sie sind.)

---

## FEHLER 6 — TESTIMONIALS: Google-G + Sterne fehlen

**Problem (Build):**
- Cards haben nur blaues Quote-Icon, Name + Trennlinie. **Kein Google-G-Logo, keine 5 Sterne.**

**Original:**
- Unter dem Testimonial-Text steht: **Google-G-Logo (multi-color)** + 5 gold Sterne + Name.

**Fix:** Google-G als Inline-SVG hinzufügen, plus 5-Sterne-Reihe:

```tsx
const GoogleG = () => (
  <svg className="h-6 w-6 shrink-0" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
    <path fill="#4285F4" d="M21.6 12.227c0-.709-.064-1.39-.182-2.045H12v3.868h5.382a4.604 4.604 0 0 1-1.996 3.018v2.51h3.232c1.891-1.742 2.982-4.31 2.982-7.351z"/>
    <path fill="#34A853" d="M12 22c2.7 0 4.964-.895 6.618-2.422l-3.232-2.51c-.895.6-2.04.955-3.386.955-2.605 0-4.81-1.76-5.595-4.123H3.064v2.59A9.997 9.997 0 0 0 12 22z"/>
    <path fill="#FBBC05" d="M6.405 13.9a6.006 6.006 0 0 1 0-3.8V7.51H3.064a10.014 10.014 0 0 0 0 8.98l3.341-2.59z"/>
    <path fill="#EA4335" d="M12 5.977c1.468 0 2.786.504 3.823 1.495l2.868-2.868C16.96 2.99 14.695 2 12 2A9.997 9.997 0 0 0 3.064 7.51l3.341 2.59C7.19 7.736 9.395 5.977 12 5.977z"/>
  </svg>
);

// Card-Footer (nach Trennlinie):
<div className="mt-6 flex items-center gap-3 pt-5 border-t border-[var(--color-line)]">
  <GoogleG />
  <div>
    <div className="flex gap-0.5 mb-1">
      {[1,2,3,4,5].map(i => (
        <Star key={i} className="h-3.5 w-3.5 fill-[var(--color-star)] text-[var(--color-star)]" />
      ))}
    </div>
    <span className="text-[14px] font-extrabold text-[var(--color-dark)]" style={{ fontFamily: "var(--font-head)" }}>
      {testimonial.author}
    </span>
  </div>
</div>
```

---

## FEHLER 7 — EINZUGSGEBIET: Karte fehlt komplett

**Problem (Build):**
- Rechter Bereich ist ein **blaues Rechteck** mit einem kleinen weißen Tooltip „Küchensanierung" drin.
- **Das Karten-Bild ist nicht eingebunden!**

**Original:**
- Tatsächliche Schweiz-Karte (Vierwaldstättersee-Region) mit Pins für Hauptsitz und Filiale.
- Tooltip-Overlay „Küchensanierung" + Bullets ist auf der Karte positioniert.
- Unten kleines Legende: „● Projekt   ○ Standort".

**Fix:** Karten-Bild einbinden:

```tsx
<figure className="relative aspect-[1440/858] overflow-hidden rounded-[24px] bg-[var(--color-light)]">
  <img
    src="https://cdn.jsdelivr.net/gh/rhvisuals/zemp-sanitaer-onepage-assets@main/images/16-map.png"
    alt="Karte Einzugsgebiet Zemp Sanitär: Region Luzern + Vierwaldstättersee"
    className="h-full w-full object-cover"
    loading="lazy"
  />
  {/* Tooltip-Overlay */}
  <div className="absolute top-[32%] left-[42%] rounded-[10px] bg-white p-3 shadow-[var(--shadow-card)] max-w-[180px]">
    <p className="text-[12px] font-extrabold text-[var(--color-dark)]" style={{ fontFamily: "var(--font-head)" }}>
      Küchensanierung
    </p>
    <ul className="mt-2 space-y-1 text-[11px] text-[var(--color-dark)]">
      <li className="flex items-center gap-1.5"><span className="text-[var(--color-blue)]">✓</span> Neue Leitungen</li>
      <li className="flex items-center gap-1.5"><span className="text-[var(--color-blue)]">✓</span> Armaturen</li>
      <li className="flex items-center gap-1.5"><span className="text-[var(--color-blue)]">✓</span> Anschlüsse</li>
    </ul>
  </div>
  {/* Legende unten rechts */}
  <div className="absolute bottom-4 right-4 flex items-center gap-4 rounded-full bg-white/95 px-4 py-2 shadow-[var(--shadow-card)] text-[12px] font-semibold">
    <span className="flex items-center gap-1.5"><span className="h-2 w-2 rounded-full bg-[var(--color-blue)]" /> Projekt</span>
    <span className="flex items-center gap-1.5"><span className="h-2 w-2 rounded-full border-2 border-[var(--color-blue)] bg-white" /> Standort</span>
  </div>
</figure>
```

---

## FEHLER 8 — KONTAKT: H2 unsichtbar (schwarz auf blau)

**Problem (Build):**
- BG ist royal-blau. ✅
- ABER die H2 „Kostenlose Beratung für Ihre Badsanierung in Luzern" ist **dunkel/schwarz** → fast unsichtbar.
- Body „In drei kurzen Schritten…" ist **weiß** ✅ — passt.

**Original:**
- H2 ist **weiß** auf blauem BG, voll lesbar.

**Fix:** In `Kontakt.tsx`:

```tsx
<h2 className="text-[36px] lg:text-[52px] font-extrabold leading-[1.1] tracking-[-0.02em] text-white max-w-[820px] mx-auto">
  Kostenlose Beratung für Ihre Badsanierung in Luzern
</h2>
```

(`text-white` statt schwarz.)

---

## FEHLER 9 — KONTAKT-FORM: Weiter-Button hellblau / „disabled"-Optik

**Problem (Build):**
- „Weiter"-Button ist hellblau/blass → wirkt deaktiviert.
- „Zurück"-Button hat hellgraue Optik → wirkt auch deaktiviert.

**Original:**
- „Zurück" ist hellgrau aber klar lesbar (#F5F7FA, dark text).
- „Weiter" ist **kräftig blau** (#1858E6, weißer Text) sobald eine Choice gewählt ist.

**Fix:**

```tsx
// Zurück
<button className="flex-1 h-12 rounded-[12px] bg-[var(--color-light)] text-[var(--color-dark)] font-extrabold hover:bg-[#E8EAEE] transition-colors">
  Zurück
</button>

// Weiter — aktiv wenn Choice gewählt
<button
  disabled={!canStep1}
  className="flex-1 h-12 rounded-[12px] bg-[var(--color-blue)] text-white font-extrabold hover:bg-[var(--color-blue-dark)] disabled:opacity-40 disabled:cursor-not-allowed transition-colors flex items-center justify-center gap-2"
>
  Weiter <ArrowRight className="h-4 w-4" />
</button>
```

Default: Weiter ist nur dann opacity-40, wenn `canStep1 === false`. Sobald eine Option gewählt → voll-gesättigt blau.

---

## FEHLER 10 — LEISTUNGEN: Pill-Labels für die 3 kleinen Cards falsch

**Problem (Build):**
- 3 kleine Cards rechts: „KÜCHE" / „REPARATUR" / „GAS".

**Original:**
- 3 kleine Cards (unter der Hero-Card, in eigener Row): „SERVICE" / „SPEZIALISIERUNG" / „UMBAU".
- KÜCHE ist eine **separate** kleine Card oben.

Korrigiere die Pills auf die echten OnePage-Labels — siehe Original-Screenshot Sec 6 (Y=4800).

---

## FEHLER 11 — BEDENKEN-CARDS: Bold-Markup im Body fehlt

**Problem (Build):**
- Body-Text ist überall normal-weight.

**Original:**
- Einzelne Worte sind **bold** (z. B. „**verbindliche Festpreisofferte**", „**drei bis fünf Wochen**", „**einen Ansprechpartner**").

**Fix:** Body als JSX mit `<strong>`:

```tsx
{
  q: "Wird es am Ende teurer als besprochen?",
  a: <>Sie erhalten eine <strong className="font-extrabold text-[var(--color-dark)]">verbindliche Festpreisofferte</strong>. Was dort steht, zahlen Sie. Kein Franken mehr, auch wenn während der Bauphase etwas Unvorhergesehenes auftaucht.</>,
  payoff: "Festpreisgarantie auf jedes Projekt"
},
```

(Analog für Card 2 + 3.)

---

## FEHLER 12 — AQUACLEAN: Card seitlich beschnitten / Container-Padding

**Problem (Build):**
- AquaClean-Card ist **innerhalb** des Containers mit beidseitigem Padding → wirkt schmaler.

**Original:**
- AquaClean-Card-Komposition geht über die volle Container-Breite, **Border-Radius nur links und rechts unten** (außen), nahtlos.

**Fix:** Container-Padding für diese Section auf `px-0` setzen (für die Card), Card selbst hat `rounded-[24px]` global aber dann full-width relativ zum Viewport:

```tsx
<section id="aquaclean" className="bg-white py-24 lg:py-28 max-[720px]:py-16">
  <div className="mx-auto max-w-[1400px] px-8 max-[720px]:px-5">
    <div className="grid lg:grid-cols-2 overflow-hidden rounded-[24px]">
      {/* Linker blauer Block */}
      <div className="bg-[var(--color-blue)] text-white p-10 lg:p-12 flex flex-col justify-center">
        ...
      </div>
      {/* Rechtes Produkt-Bild */}
      <div className="aspect-[688/513] max-[720px]:aspect-square">
        <img src="..." className="h-full w-full object-cover object-left" />
      </div>
    </div>
  </div>
</section>
```

---

## FEHLER 13 — LOGO im FOOTER ist OK, aber Spacing

**Problem (Build):**
- Footer-Spalten sind teilweise zu eng (Adresse einzeilig statt 2-zeilig).

**Original:**
- Footer-Adressen sind **2-zeilig**: „Rothenring 9" / „6015 Luzern" — getrennt durch `<br/>`.

**Fix:** Sicherstellen, dass Adressen mit `<br/>` in 2 Zeilen:

```tsx
<address className="not-italic text-[14px] leading-[1.6] text-white/72">
  Rothenring 9<br/>
  6015 Luzern
</address>
```

---

## ZUSAMMENFASSUNG — Wenn du nichts anderes liest

| # | Fehler | Fix |
|---|---|---|
| 1 | Header: Logo 1-zeilig, Search-Box statt CTA | Logo 2-zeilig „zemp" + „SANITÄR AG", „Beratung anfragen"-Pill rechts |
| 2 | **Hero: weißer BG mit kleinem Bild rechts** | **Full-Bleed Hero mit Bild als Background + dunklem Gradient + weißem Text + Trust-Strip-Balken unten** |
| 3 | **Vorher/Nachher: keine echte Slider-Mechanik** | **clipPath-basierter Drag-Slider mit großem Mittel-Handle** |
| 4 | **Über-uns: H2 schwarz auf dunklem BG (unsichtbar)** | **H2 auf `text-white`** |
| 5 | Leistungen Hero-Card: Bild oben + Text unten getrennt | Bild als Card-BG + Text-Overlay mit dunklem Gradient |
| 6 | **Testimonials: Google-G + Sterne fehlen** | **Inline Google-G-SVG + 5-Sterne-Row über Namen** |
| 7 | **Einzugsgebiet: Karten-Bild fehlt komplett** | **`16-map.png` als BG + CSS-Tooltip-Overlay** |
| 8 | **Kontakt: H2 schwarz auf blauem BG (unsichtbar)** | **H2 auf `text-white`** |
| 9 | Kontakt-Form Weiter-Button blass | Voll blauer Button, disabled nur bei keiner Choice |
| 10 | Leistungen Pills nutzen falsche Labels | „SERVICE" / „SPEZIALISIERUNG" / „UMBAU" statt „REPARATUR" / „GAS" |
| 11 | Bedenken-Body ohne Bold-Markup | Schlüsselbegriffe mit `<strong>` highlighten |
| 12 | AquaClean Card seitlich beschnitten | Card-Layout mit `overflow-hidden rounded-[24px]` und `grid-cols-2` direkt unter Container |
| 13 | Footer-Adressen einzeilig | Adressen mit `<br/>` in 2 Zeilen |

---

## QA-CHECK NACH DEM FIX

Öffne https://zemp-sanitar.onepage.me/homepage-version-1 in einem Tab und https://zemp.lovable.app/ (gefixt) in einem anderen. Scroll parallel durch. Diese 3 Sections müssen **ohne Zweifel identisch** aussehen:

1. **Hero** — Full-Bleed, Bild Background, weißer Text, Trust-Strip unten.
2. **Über uns** — Headline weiß lesbar auf dunklem BG.
3. **Kontakt** — Headline weiß lesbar auf blauem BG.

Wenn diese 3 stimmen, ist der wichtigste Layout-Bruch behoben. Den Rest dann pro Section abhaken nach der Tabelle oben.

**Bau die Fixes jetzt.**
