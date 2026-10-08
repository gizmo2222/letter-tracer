# Letter Tracer

A letter-tracing app for young children. One HTML file, no build step, no server code. Works with a finger on a phone or tablet, or with a mouse.

Live at **[metacrystal.com/letter-tracer.html](https://metacrystal.com/letter-tracer.html)**.

## How it plays

Pick a letter (tap the big letter at the top, or use ◀ ▶), then trace it. A star and confetti mean the letter was traced well.

**Lines mode** (default) teaches the strokes:

- The stroke to trace now is highlighted. A pulsing dot marks where it starts (numbered when the letter has more than one stroke) and an arrow shows which way to go.
- A stroke only counts when it's traced from its start in the right direction. Starting at the wrong end, or jumping into the middle, does nothing.
- When a stroke is done, the next one lights up. Finishing the last stroke finishes the letter.

**Blocks mode** shows a solid letter to colour in. The letter is done when about 80% of it is coloured.

**Staying on the line matters.** Ink on the letter is drawn in the theme's "on" colour, ink off it in the "miss" colour. If the letter is finished but less than 60% of the ink landed on it, there's no star: the app says "Stay on the line!" and gives a fresh letter. Scribbling over the whole screen never earns a star.

## Controls

| Control | What it does |
|---|---|
| **Aa / aA** | Switch between capital and small letters |
| 🎨 | Colour theme: Night, Paper, Chalk, Candy |
| ◀ ▶, or tap the letter | Change letter |
| **− / +** | Make the letter smaller or bigger (the drawing scales with it) |
| **Lines / Blocks** | Switch mode |
| **Clear** | Start the letter again |

The theme, case, mode, size, current letter and star count are saved in the browser and come back next visit. Rotating a tablet or resizing the window keeps what's been drawn.

## Tuning

The constants at the top of the script:

| Constant | Default | Meaning |
|---|---|---|
| `ACCURACY_MIN` | 0.60 | Share of the ink that must be on the letter for a star |
| `STROKE_DONE` | 0.85 | Lines mode: share of a stroke to trace, in order, to finish it |
| `COVER_THRESHOLD` | 0.80 | Blocks mode: share of the letter to colour in |
| `ORDER_SLACK` | 3 | Lines mode: how far ahead of the finger a point may still count (absorbs wobble) |
| `DETECT_RADIUS` | 25 | How close (CSS px) to the line counts as on it |

## Letter shapes

Each letter is an SVG path in a 0–100 square (`PATHS_UC`, `PATHS_LC`). Each `M` starts a new stroke, and the strokes are traced in the order and direction they're written, so **the path data is the stroke order**. To change how a letter is taught, reorder its sub-paths or reverse a stroke's points.

## Deploy

`.github/workflows/deploy.yml` uploads `index.html` to `metacrystal.com/letter-tracer.html` over SFTP on every push to `main`, or from **Run workflow** on the Actions tab. It uses the `SFTP_USER` and `SFTP_PASSWORD` repo secrets, and the server's SSH host keys are pinned in `.github/known_hosts` (refresh with `ssh-keyscan metacrystal.com` if the host ever changes them).
