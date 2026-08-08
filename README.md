# Backgammon (PWA)

A complete, standard-rules backgammon game — pass-and-play on one device —
built as a installable web app (PWA). No build step, no dependencies, works
on any phone, tablet, or desktop browser.

## Files

- `index.html` — the whole game (board, rules engine, UI). This is the only
  file you need to *play* — you can open it directly in a browser.
- `manifest.json` — makes the game installable ("Add to Home Screen").
- `service-worker.js` — caches the app so it keeps working offline once installed.
- `icon-192.png`, `icon-512.png` — app icons.

## Playing it right now

Just open `index.html` in any mobile or desktop browser. Everything runs
client-side — tap a checker, then tap a highlighted point to move it.

## Installing it as an app on your phone

Browsers only allow "install" and offline caching for pages served over
**https** (or `localhost`) — not for a file opened straight from disk. So to
get the installable/offline experience:

1. Put all five files (`index.html`, `manifest.json`, `service-worker.js`,
   `icon-192.png`, `icon-512.png`) in the same folder on a static host —
   GitHub Pages, Netlify, Vercel, Cloudflare Pages, or any web server all work,
   and all have free tiers.
2. Visit the hosted URL on your phone.
3. **iPhone (Safari):** tap the Share icon → "Add to Home Screen."
   **Android (Chrome):** tap the ⋮ menu → "Install app" (or you'll see an
   automatic install banner).
4. Launch it from your home screen — it opens full-screen, remembers nothing
   needs a network connection after the first load.

### Quick local test
From inside the folder, run a tiny local server and open it on `localhost`
(this is enough for the install prompt and offline caching to work while testing):

```bash
python3 -m http.server 8080
# then visit http://localhost:8080 on the same machine
```

## Rules implemented

- Standard starting position, alternating turns, two dice, doubles = four moves.
- Legal-move checking: blocked points, hitting blots, forced bar re-entry
  before any other move, bearing off only once all fifteen checkers are home
  (including the "use the extra pips on the farthest checker" rule).
- Pip count for both sides, undo-last-move (within the current turn), a
  manual "Done" button to end your turn early, and a win screen when someone
  bears off all fifteen checkers.
- Not implemented: the doubling cube, match/money scoring, and an AI
  opponent — this is built for two people sharing one device.

## Layout

The board follows a classic split-panel design: two separate wooden panels
side by side (left = the outer board, points 7–18; right = the home board,
points 1–6 and 19–24), with the bar as the narrow strip between them where
hit checkers wait to re-enter. Player cards sit above and below the board
and light up green on whichever side's turn it is. It's built portrait-only:

- `manifest.json` sets `"orientation": "portrait"`, so once installed to a
  home screen (see below), Android will keep it locked to portrait automatically.
- The page also makes a best-effort call to the Screen Orientation API to
  lock portrait — this only works once installed/running full-screen on
  browsers that support it (mainly Android Chrome); it's a no-op elsewhere.
- If someone does rotate to landscape on a phone, a "please rotate back"
  overlay appears rather than trying to render the board sideways. iOS
  Safari has no orientation-lock API at all, so on iPhone the overlay is
  the main defense — there's no way to truly force portrait there short of
  wrapping the page in a native app shell.

## Customizing

Everything (styling, layout, and game logic) lives in `index.html` in plain
CSS and vanilla JavaScript — no framework or bundler — so it's easy to tweak
colors, add an AI opponent, or wire up online multiplayer later.
