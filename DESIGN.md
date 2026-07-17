# DESIGN.md — Gino's Farewell Tribute Webapp

**Design System v1.0** | Glassmorphism × Celebration × Premium

---

## 1. Design Language

### Style
**Refined Glassmorphism** — semi-transparent backgrounds, backdrop blur, soft 1px borders with gradient. Elevated cards feel like frosted glass floating over a deep gradient backdrop.

**Inspiration:** Premium invitations, luxury brand cards, celebration micro-sites.

### Core Principles
- **Depth over Flatness** — layers, shadows, blur create spatial hierarchy
- **Celebration, Not Clutter** — confetti is the only "loud" element
- **Warmth + Professionalism** — gold accents, not childish colors
- **Motion with Purpose** — every animation tells part of the story

---

## 2. Color Palette

| Token | Hex | Usage |
|-------|-----|-------|
| `--bg-deep` | `#0a0a1a` | Page background (deepest layer) |
| `--bg-gradient` | `#0a0a1a → #1a1040 → #0d1b3e` | Radial gradient backdrop |
| `--glass-bg` | `rgba(255,255,255,0.05)` | Glassmorphic panels |
| `--glass-border` | `rgba(255,255,255,0.12)` | Panel borders |
| `--gold-primary` | `#D4AF37` | Wax seal, headings, accents |
| `--gold-glow` | `rgba(212,175,55,0.3)` | Glow effects |
| `--card-front-bg` | `#111827` | Flip-card front (dark, bold) |
| `--card-back-1` | `#1e3a5f` | Card back color 1 |
| `--card-back-2` | `#2d1b69` | Card back color 2 |
| `--card-back-3` | `#5c1a1a` | Card back color 3 |
| `--card-back-4` | `#1a3d2e` | Card back color 4 |
| `--text-primary` | `#f1f5f9` | Main text |
| `--text-muted` | `#94a3b8` | Secondary text |
| `--text-gold` | `#fbbf24` | Highlighted text |

### Card Color Variants (Back faces)
Each card gets one of 4 celebratory glassmorphic colors:
- **Sapphire** `linear-gradient(135deg, #1e3a5f, #3b5998)` — Blue
- **Amethyst** `linear-gradient(135deg, #2d1b69, #6d28d9)` — Purple
- **Ruby** `linear-gradient(135deg, #5c1a1a, #dc2626)` — Red
- **Emerald** `linear-gradient(135deg, #1a3d2e, #059669)` — Green

---

## 3. Typography

| Role | Font | Weight | Size |
|------|------|--------|------|
| Logo / Wax Seal | Montserrat | 700 | 12px |
| Main Greeting ("Dear Gino,") | Playfair Display | 700 | clamp(2rem, 5vw, 3.5rem) |
| Card Shout Text | Playfair Display | 700 | clamp(1rem, 2.5vw, 1.5rem) |
| Body Paragraph | Montserrat | 400 | clamp(0.85rem, 1.5vw, 1rem) |
| Signature | Montserrat | 600 italic | 0.8rem |
| Restart Button | Montserrat | 600 | 0.85rem |

**Google Fonts Import:**
```css
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;600;700;900&family=Playfair+Display:wght@400;600;700&display=swap');
```

---

## 4. Layout Architecture

### Viewport Lock
```css
html, body { height: 100vh; width: 100vw; overflow: hidden; }
```

### Grid System
```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 1rem;
  padding: 1rem;
  max-width: 1200px;
  margin: 0 auto;
}
```
- **Mobile (< 640px):** 1-2 columns
- **Tablet (640-1024px):** 2-3 columns
- **Desktop (> 1024px):** 3-4 columns

### Layers (z-index stack)
1. Background gradient (z: 0)
2. Envelope overlay (z: 100) — fades out
3. Dashboard content (z: 10)
4. Confetti particles (z: 200)
5. Restart button (z: 50)

---

## 5. Animation Specs

### Envelope Float
```css
@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-15px); }
}
```
Duration: 3s, ease-in-out, infinite

### Envelope Open → Dashboard Reveal
- Envelope scale down + fade out: 0.6s ease-in
- Dashboard fade in + slight scale up: 0.8s ease-out, 0.3s delay

### Card Flip (3D)
```css
.card.flipped .card-inner {
  transform: rotateY(180deg);
}
.card-inner {
  transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
  transform-style: preserve-3d;
}
```
- Front: `backface-visibility: hidden`
- Back: `backface-visibility: hidden; transform: rotateY(180deg)`

### Confetti Burst
- 20-30 emoji particles per flip
- Random spread: ±150px from card center
- CSS animation: `@keyframes confetti-fall` (translate up + out, then fall)
- Duration: 1.5s, ease-out
- Emojis: ✨ 🎊 🎇 💖 🎉 ⭐ 🌟

### Hover Pulse
```css
.card:not(.flipped):hover {
  transform: scale(1.03);
  box-shadow: 0 0 20px var(--gold-glow);
}
```

---

## 6. Sound Design (Optional)

- **Envelope open:** `open.mp3` — subtle paper/crinkle sound
- **Card flip:** `flip.mp3` — light card flip whoosh
- **Volume:** 30% default, no autoplay (browser policy)
- **Fallback:** Silent — all sounds wrapped in try/catch

---

## 7. Accessibility

- All cards are `<button>` elements with `aria-expanded="true|false"`
- Tab to focus, Enter/Space to flip
- Visible focus ring: `2px solid var(--gold-primary)`
- ARIA labels: `aria-label="Flip card: We are PROUD of YOU!"`
- `prefers-reduced-motion` respected: animations disabled
- Confetti respects `prefers-reduced-motion`

---

## 8. Responsive Breakpoints

| Breakpoint | Columns | Card Size | Font Scale |
|------------|---------|-----------|------------|
| < 480px | 1 | full width | 0.85× |
| 480-639px | 2 | ~180px | 0.9× |
| 640-1023px | 3 | ~200px | 1× |
| ≥ 1024px | 3-4 | ~220px | 1× |

---

## 9. File Structure

```
gino-farewell/
├── docs/
│   └── index.html          ← GitHub Pages serves from here
├── src/
│   ├── logo.png            ← Cosme-De.Net logo asset
│   └── sounds/
│       ├── open.mp3        ← Envelope open SFX
│       └── flip.mp3        ← Card flip SFX
├── PRD.md
└── DESIGN.md
```
