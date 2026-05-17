---
name: squircle-ui
description: Use when building React UI con angoli arrotondati che devono avere il look Apple/iOS (app icon, button, card, avatar, hero image). Sostituisce `border-radius` e `rounded-*` di Tailwind con `@squircle-js/react` per ottenere curve G2-continue stile superellipse. Triggers — "fai squircle", "angoli stile iOS", "rounded che sembra Apple", "il radius CSS sembra rotto/spigoloso".
---

# Squircle UI

## Cos'è

`@squircle-js/react` rimpiazza `border-radius` (arco di cerchio, transizione G1, visivamente "rotta") con una **superellipse G2-continua** — la stessa curva delle app icon iOS. Il radius CSS standard salta di colpo dal lato dritto all'arco; lo squircle si raccorda dolcemente.

**Regola d'oro:** `cornerSmoothing` fisso a **0.6** (preset iOS / Apple). Si scala il `cornerRadius` in base alla grandezza reale dell'elemento.

## Quando usarla

- Bottoni, card, avatar, badge, icone app, hero image, immagini prodotto
- Ogni volta che vedi `rounded-xl` / `rounded-2xl` su un elemento sopra ~32px e vuoi look premium consumer/Apple

**Quando NON usarla:**
- Veri cerchi (avatar perfettamente tondo, dot indicator) → resta su `rounded-full`
- Elementi minuscoli < 16px (icone inline, ticker) → squircle indistinguibile dal radius CSS, non vale il render cost
- Testo inline / chip con `border-radius` < 6px → l'effetto è invisibile

## Install

```bash
npm install @squircle-js/react
```

## API: due componenti

| Componente | Quando usarlo | Come dimensiona |
|---|---|---|
| `Squircle` | Elementi **responsive** (card che si espandono, bottoni con padding variabile) | Usa `ResizeObserver` internamente |
| `StaticSquircle` | Elementi a **dimensioni fisse** (avatar, app icon, hero con `width`/`height` noti) | Richiede `width` e `height` espliciti |

**Props comuni:**
- `cornerRadius` — pixel del raggio (intero)
- `cornerSmoothing` — float 0–1. **Default canonico: `0.6`** (Apple/iOS). Solo eccezione: `0.8` per elementi ≥ 400px (hero, cover)
- `className` — passa Tailwind/CSS al wrapper
- `asChild` — renderizza il clip-path **direttamente sul child** senza wrapper div (essenziale per `<button>`, `<a>`, `<img>`, `motion.div`)

## Scala dei `cornerRadius`

| Elemento | Dimensione tipica | `cornerRadius` |
|---|---|---|
| Badge / pill piccolo | ~20px h | `6` |
| Icon button | 40×40 | `10` |
| Bottone standard | h ~40px | `12`–`14` |
| Card | larghezza variabile | `20` |
| Avatar | size variabile | `Math.round(size * 0.25)` |
| App icon iOS | 60×60 | `13` |
| Hero / cover | ≥ 400px | `28`–`32` (+ `cornerSmoothing: 0.8`) |

**Anti-blob:** se l'elemento è < 32px, usa `cornerRadius` ≤ `size/3`. Altrimenti il bordo si "gonfia" e perde i lati dritti.

## Pattern 1 — Bottone (`asChild`)

```jsx
import { Squircle } from "@squircle-js/react";

export function Button({ children, onClick }) {
  return (
    <Squircle
      cornerRadius={12}
      cornerSmoothing={0.6}
      className="bg-indigo-600 px-5 py-2.5 text-white font-semibold text-sm"
      asChild
    >
      <button type="button" onClick={onClick}>
        {children}
      </button>
    </Squircle>
  );
}
```

`asChild` clippa direttamente il `<button>` — niente wrapper div che rompe focus ring, eventi o layout flex.

## Pattern 2 — Card responsive (no `asChild`)

```jsx
import { Squircle } from "@squircle-js/react";

export function Card({ title, body }) {
  return (
    <Squircle
      cornerRadius={20}
      cornerSmoothing={0.6}
      className="bg-white shadow-md p-6 space-y-2"
    >
      <h3 className="font-semibold text-lg">{title}</h3>
      <p className="text-gray-600 text-sm">{body}</p>
    </Squircle>
  );
}
```

Senza `asChild`, `Squircle` crea il proprio div. OK quando il contenuto è figli liberi.

## Pattern 3 — Avatar / Image (StaticSquircle)

```jsx
import { StaticSquircle } from "@squircle-js/react";

export function Avatar({ src, alt, size = 48 }) {
  return (
    <StaticSquircle
      width={size}
      height={size}
      cornerRadius={Math.round(size * 0.25)}
      cornerSmoothing={0.6}
      className="overflow-hidden shrink-0"
      asChild
    >
      <img src={src} alt={alt} className="w-full h-full object-cover" />
    </StaticSquircle>
  );
}
```

Per immagini sempre `StaticSquircle` + `asChild` sull'`<img>`.

## Pattern 4 — Hero image (smoothing 0.8)

```jsx
import { StaticSquircle } from "@squircle-js/react";

export function HeroImage({ src, alt }) {
  return (
    <StaticSquircle
      width={600}
      height={400}
      cornerRadius={32}
      cornerSmoothing={0.8}
      asChild
    >
      <img src={src} alt={alt} className="w-full h-full object-cover" />
    </StaticSquircle>
  );
}
```

Unica eccezione al `0.6` di default.

## Pattern 5 — App icon stile iOS

```jsx
import { StaticSquircle } from "@squircle-js/react";

export function AppIcon({ src, name }) {
  return (
    <div className="flex flex-col items-center gap-1.5">
      <StaticSquircle
        width={60}
        height={60}
        cornerRadius={13}
        cornerSmoothing={0.6}
        className="overflow-hidden"
        asChild
      >
        <img src={src} alt={name} />
      </StaticSquircle>
      <span className="text-xs text-gray-700">{name}</span>
    </div>
  );
}
```

`60×60` con `cornerRadius={13}` è il match esatto della mask Apple.

## Pattern 6 — Framer Motion

```jsx
import { motion } from "framer-motion";
import { Squircle } from "@squircle-js/react";

export function AnimatedCard({ children }) {
  return (
    <Squircle
      cornerRadius={20}
      cornerSmoothing={0.6}
      className="bg-gradient-to-br from-violet-500 to-indigo-600 p-6"
      asChild
    >
      <motion.div
        initial={{ opacity: 0, y: 16 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.3 }}
      >
        {children}
      </motion.div>
    </Squircle>
  );
}
```

`asChild` su `motion.div` funziona perfettamente — il clip-path resta stabile durante le transform.

## Pattern 7 — Bordo (workaround manuale)

`@squircle-js/react` **non ha un componente bordo nativo**. Per un bordo stile iOS usa due Squircle annidati: quello esterno è il colore del bordo, quello interno è il fill con `cornerRadius` ridotto dello spessore del bordo.

```jsx
import { Squircle } from "@squircle-js/react";

export function BorderedCard({ children }) {
  const borderWidth = 1;
  const outerRadius = 20;

  return (
    <Squircle
      cornerRadius={outerRadius}
      cornerSmoothing={0.6}
      className="bg-gray-200"
    >
      <Squircle
        cornerRadius={outerRadius - borderWidth}
        cornerSmoothing={0.6}
        className="bg-white p-6"
        style={{ margin: borderWidth }}
      >
        {children}
      </Squircle>
    </Squircle>
  );
}
```

Costoso (due clip-path). Usalo solo quando il bordo è davvero parte dell'identità visiva.

## Eccezioni accettabili

- `rounded-full` (Tailwind) → resta CSS standard per cerchi puri. Lo squircle non aggiunge nulla a un tondo perfetto.
- `rounded-sm` / `rounded` su input di testo, chip inline, righe di tabella → CSS classico va bene, lo squircle non si nota a < 8px.

## Errori comuni

| Sintomo | Causa | Fix |
|---|---|---|
| Forma "gonfia", lati non più dritti | `cornerRadius` troppo grande rispetto a `width`/`height` | Tieni `cornerRadius ≤ size/3` |
| Focus ring tagliato sui bottoni | Wrapper div di `Squircle` clippa l'outline | Usa `asChild` |
| `<img>` non clippato | Manca `asChild` o `overflow-hidden` | Aggiungi `asChild` sull'`<img>` |
| Layout shift al primo paint | `Squircle` (con ResizeObserver) su elemento a dim. fisse | Usa `StaticSquircle` |
| Look "troppo morbido", non iOS | `cornerSmoothing` ≥ 0.9 | Torna a `0.6` |

## Checklist prima di committare

- [ ] `cornerSmoothing={0.6}` ovunque (tranne hero ≥ 400px → `0.8`)
- [ ] `StaticSquircle` per dimensioni fisse, `Squircle` per responsive
- [ ] `asChild` su `<button>`, `<a>`, `<img>`, `motion.div`
- [ ] `cornerRadius` scalato sulla dim reale (vedi tabella)
- [ ] `rounded-full` lasciato sui cerchi veri
- [ ] Nessun `cornerRadius` ≥ `size/3` su elementi piccoli
