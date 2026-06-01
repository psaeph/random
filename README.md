# Valentine's Page – Swap Cheatsheet

A quick reference for customizing the Valentine's page.

---

## Confetti

The confetti comes from the [canvas-confetti](https://www.kirilv.com/canvas-confetti/) library, loaded via CDN in the `<head>`:

```html
<script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.9.3/dist/confetti.browser.min.js"></script>
```

Tweak the behavior inside the `fullScreenConfetti()` function:

```javascript
confettiInstance({
  particleCount: 12,       // how many pieces at once
  spread: 90,              // how wide they fan out (degrees)
  startVelocity: 45,       // how fast they shoot out
  ticks: 180,              // how long they stay on screen
  origin: { x: 0.5, y: 0.5 }, // where it fires from (0–1, like a percentage of screen)
  colors: ['#ff0000', '#00ff00'], // optional: custom hex colors
  shapes: ['circle', 'square'],  // optional: circle or square pieces
  scalar: 1.5              // optional: size multiplier
});
```

---

## GIF / Image After Clicking Yes

Located in the `<img>` tag inside the `.result` section:

```html
<img src="https://media.giphy.com/media/26ufdipQqU2lhNA4g/giphy.gif" />
```

To swap it:
1. Go to [giphy.com](https://giphy.com)
2. Find a GIF → click **Share → Copy Link**
3. Paste the new URL as the `src`

Or use a local file:

```html
<img src="./my-image.gif" />
```

---

## The Animal Illustration

The animal is a hand-coded SVG — no external image. You can replace the entire `<svg class="art">` block with any of the following:

**Option 1 – Emoji (simplest):**
```html
<div style="font-size: 80px">🐻</div>
```

**Option 2 – Image file:**
```html
<img src="your-image.png" class="art" />
```

**Option 3 – Free SVG:**
Download from [undraw.co](https://undraw.co) or [svgrepo.com](https://svgrepo.com) and paste the SVG code in place of the existing block.

---

## Mobile Support (Touch)

The default code only moves the No button on desktop (mouse). To support mobile, add this below the existing `pointermove` listener:

```javascript
zone.addEventListener("touchmove", e => {
  e.preventDefault();
  const touch = e.touches[0];
  const b = noBtn.getBoundingClientRect();
  const d = Math.hypot(
    (b.left + b.width / 2) - touch.clientX,
    (b.top + b.height / 2) - touch.clientY
  );
  if (d < 200) moveNo(touch.clientX, touch.clientY);
}, { passive: false });

noBtn.addEventListener("touchstart", e => e.preventDefault(), { passive: false });
```

> Tip: The proximity distance is set to `200` instead of `140` since fingers are less precise than cursors.

---

## Quick Reference

| Element | Where it lives | How to change |
|---|---|---|
| Confetti behavior | `fullScreenConfetti()` function | Tweak `particleCount`, `spread`, `colors`, etc. |
| Confetti library | `<script src="...">` in `<head>` | Keep this line or confetti breaks |
| The GIF | `<img src="giphy url">` in `.result` | Swap the URL for any image or GIF link |
| The animal | `<svg class="art">` block | Replace with an `<img>`, emoji, or new SVG |
| The question text | `<h1>` tag | Edit the text directly |
| No button escape distance | `if (d < 140)` in `pointermove` | Raise the number for easier triggering |
| Yes button max size | `Math.min(2.2, ...)` in `growYes()` | Change `2.2` to make it grow bigger or smaller |
