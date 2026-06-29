# One Piece — Grand Line Showcase

🔗 **Live site:** [one-piece-self-seven.vercel.app](https://one-piece-self-seven.vercel.app/)

An interactive, single-file **One Piece** landing experience. Cursor-driven reveals, full-bleed cinematic sections, canvas particle effects, and a looping soundtrack — all in one `index.html`, no frameworks and no build step.

What started as a single Gear 5 hover-reveal has grown into a full scrolling showcase: a hero awakening, a rivals gallery, two full-page crew sections, a cursor-shift gear stage, and background music.

---

## Demo

**▶ Live:** https://one-piece-self-seven.vercel.app/

Or open `index.html` in any modern browser. Move your cursor around each section — almost everything reacts to the pointer. On touch devices, drag a finger. Background music starts on your first interaction.

> Assets live in `images/`, `images/gears/`, and `audio/` — keep them alongside `index.html`.

---

## Sections

| # | Section | Interaction |
|---|---------|-------------|
| 1 | **Hero — Gear 5 Awakening** | Move the cursor over base Luffy to carve a glowing comet trail that reveals **Gear 5 (Nika)** Luffy underneath. |
| 2 | **Pirate Legends & Rivals** | A responsive card grid of rivals (Zoro, Sanji, Law, Shanks, Roger, …) with default → awakened artwork. |
| 3 | **The Straw Hat Crew** | Full-page crew artwork with a slow ken-burns zoom, subtle cursor parallax, and a canvas layer of rising embers + aura lightning. The whole crew stays bright and visible. |
| 4 | **Straw Hat Pirates Assembly** | Full-page canvas. Hover to sweep a spotlight that reveals the crew's awakened/thunder form, with a comet trail and head glows. |
| 5 | **Monkey D. Luffy Gears** | Full-page cinematic stage. **Slide the cursor left → right** to shift through Gear 1 → 5; each gear crossfades its background, recolors the scene, fires a power-up burst, and runs its own particle FX. |
| — | **Background music** | Looping soundtrack with an animated equalizer toggle (top-right). |

---

## Features

- **Cursor-driven everywhere** — comet-trail reveals, a left-to-right gear shifter, spotlight sweeps, and parallax.
- **Full-page cinematic sections** — sections 3–5 are full-viewport (`100vh`) with full-bleed imagery, no cards.
- **Canvas particle engines** — embers, aura lightning, steam, and per-gear effects via `requestAnimationFrame`.
- **Scroll-in reveals** — headers, text, and stages animate in as each section enters the viewport (with safe fallbacks so nothing stays hidden).
- **Looping background music** — auto-starts muted, becomes audible on first interaction, with a mute/play equalizer button.
- **Responsive** — layouts and type scale down for mobile; touch interactions supported.
- **Zero build** — one HTML file + static assets. No bundler, no dependencies (only Google Fonts over the network).
- **Reduced-motion aware** — honors `prefers-reduced-motion` for the gear and crew animations.

---

## Quick start

1. Keep this folder structure:
   ```
   index.html
   vercel.json
   images/            (character art: zoro.png, sanji_awakened.png, crew_default.png, …)
   images/gears/      (gear1.jpg … gear5.jpeg, crew.jpg)
   audio/one_piece.mp3
   ```
2. Open `index.html` in a browser (double-click), **or** serve the folder:
   ```bash
   npx serve .
   # or
   python -m http.server
   ```

A local server isn't required, but it most closely matches the deployed behavior (and avoids any `file://` quirks with audio).

---

## Project structure

```
One-Piece/
├─ index.html              # the entire site (markup, CSS, and JS inline)
├─ vercel.json             # Vercel config (clean URLs)
├─ README.md
├─ audio/
│  └─ one_piece.mp3        # looping background track
└─ images/
   ├─ zoro.png / zoro_awakened.png ...   # gallery rivals (default + awakened)
   ├─ crew_default.png / crew_awakened.png  # section 4 hover-reveal
   └─ gears/
      ├─ gear1.jpg … gear5.jpeg  # full-page gear backgrounds
      └─ crew.jpg                # section 3 full-bleed crew shot
```

---

## Deploy to Vercel

This is a static site, so deployment needs no build step.

**Option A — Vercel CLI (from this folder):**
```bash
npm install -g vercel    # one time
vercel login             # opens browser, one time
vercel --prod --yes      # uploads this folder and prints your live URL
```

**Option B — GitHub:** push the folder to your own GitHub repo, then **vercel.com → Add New → Project → Import** it (framework preset: *Other*, no build command).

Because the entry file is `index.html`, it's served at the root `/` automatically. `vercel.json` just enables clean URLs.

> **Note on audio:** browser autoplay policies block sound before any user interaction on every site, including the deployed one. The music therefore becomes audible on the visitor's first cursor move / scroll / click — this is expected and unavoidable.

---

## Tuning

Most knobs live at the top of the relevant `<script>` block in `index.html`.

| What | Where | Default | Effect |
|------|-------|---------|--------|
| Hero trail length | `const TRAIL_LENGTH` | `60` | Longer = longer comet tail |
| Hero reveal size | `let HEAD_RADIUS` | `~190` (auto-scaled) | Radius of the revealed circle |
| Hero darkness | `const OVERLAY` | `'rgba(0,0,0,0.42)'` | Higher alpha = darker scene |
| Music volume | `audio.volume` | `0.45` | Background track loudness (0–1) |
| Gear switch speed | the `420` ms lock in `switchGear` | `420` | How fast the cursor can shift gears |
| Crew parallax strength | `-dx * 26` / `-dy * 26` | `26` | Pixels of background drift on cursor move |

To swap the gear artwork, replace the files in `images/gears/` (note `gear4` is a `.jpeg`) and update the matching `background-image` URLs in the `.gear-bg` markup.

---

## How the hero reveal works

1. The **bottom** image (Gear 5) is drawn to the canvas, then darkened with the overlay.
2. Each frame the smoothed cursor position is pushed to the front of a `trail` array (capped at `TRAIL_LENGTH`).
3. On an offscreen canvas, each trail point is a black circle that shrinks/fades toward the tail.
4. The **top** image (base Luffy) is composited with `globalCompositeOperation = 'source-in'`, so it shows only inside the trail circles.
5. The offscreen result is drawn over the canvas — base Luffy appears only inside the comet, Gear 5 everywhere else.
6. A `lighter`-blended radial gradient draws the white-gold "awakening" glow at the cursor head.

---

## Browser support

Works in current Chrome, Edge, Firefox, and Safari. Requires Canvas 2D, `requestAnimationFrame`, CSS `mask`/`backdrop-filter`, and `IntersectionObserver` — all widely supported. Touch is handled via `touchmove`/`touchstart`.

---

## Credits & licensing

- **Code:** free to use and modify for your own projects.
- **Artwork & "One Piece" / characters:** One Piece is created by Eiichiro Oda; all character art, music, and trademarks belong to their respective rights holders. The images and audio here are fan/demo assets — replace them with material you have the right to use before publishing.
- The official One Piece logo typeface is proprietary and **not** bundled; the wordmark is a CSS approximation using the free [Luckiest Guy](https://fonts.google.com/specimen/Luckiest+Guy) font.

---

*An interactive canvas experiment across the Grand Line. Set sail. 🏴‍☠️*
