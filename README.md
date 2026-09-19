# raw

Shortener for GitHub raw links, with an animated chip-board landing page (dark blue theme).

## Deploy

1. Create a repository (for example `Loader`) and upload `index.html`, `404.html` and `links.json` to its root.
2. Open Settings → Pages, choose branch `main` and folder `/ (root)`, then save.
3. The site goes live at `https://USERNAME.github.io/REPO/`.
4. In both `index.html` and `404.html`, set `<meta name="base" content="/REPO/">` to your repository name.

`404.html` is an exact copy of `index.html`. GitHub Pages serves it for addresses such as `/REPO/app`, which have no file of their own. Whenever you change `index.html`, copy it to `404.html` again.

## Add links

Edit `links.json`:

```json
{ "slug": "app", "raw": "https://raw.githubusercontent.com/USERNAME/REPO/main/app.js" }
```

The short link becomes `https://USERNAME.github.io/REPO/app`.

Only URLs starting with `https://raw.githubusercontent.com/` are accepted.

## Machine states

The chip board on the landing page has four states. The LED above the chip shows the active one.

| State | Color | Chip, fan and other mechanisms |
|---|---|---|
| Standby | blue, green LED breathing | Fan turns slowly, traces idle, memory low, gears stopped |
| Processing | yellow | Chip glows and is scanned, data flows along the traces, fan runs fast, gears turn, memory and bus lanes pulse |
| Done | green | Chip turns green, memory full, bus lanes lit, then the raw content is shown on the page |
| Error | red, red LED blinking | Fan stops, traces and bus lanes turn red |

An error appears when `links.json` fails to load or the slug does not exist.

## Raw content without redirecting

Opening `https://USERNAME.github.io/REPO/slug` shows the raw file content on that page, with **Copy content** and **Original raw** buttons. The address does not change.

This only works in a browser, because JavaScript fetches the content. Programs that fetch content over HTTP (for example `HttpGet`) receive the page HTML instead. For those, use `worker.js` (next section).

## Natural raw with a Worker

`worker.js` creates links that behave like the original raw link: a plain-text response with no HTML page, so programs can load it and browsers show it as text.

1. Sign in at dash.cloudflare.com (a free account is enough).
2. Open Workers & Pages, choose Create, then Create Worker. Give it a name, for example `raw`, then Deploy.
3. Choose Edit code, delete the contents, paste `worker.js`, then Deploy.
4. The link becomes `https://raw.ACCOUNT.workers.dev/Guess-the-Slapper`.
5. Optional: set `RAW_HOST` in `index.html` to the Worker address so the Copy button copies the Worker link.

The Worker's link list is the `LINKS` variable at the top of `worker.js`. To add a link, add one line there and deploy again. `links.json` is only used by the site, so add the link there too if you want it in the page list. The free Cloudflare plan is limited to about 100,000 requests per day.

## Row animations

Each link row has its own mechanism that runs when the link is requested. The type is chosen from the file extension. Override it with the `"mech"` field.

| Mechanism | Default extensions | `mech` value |
|---|---|---|
| Gear | js, mjs, ts, jsx | `gear` |
| Belt | json, yaml, yml, toml, csv | `belt` |
| Piston | py, sh, rb, go | `piston` |
| Press | md, txt, html, css | `press` |
| Cam | anything else | `cam` |

## Local testing

Change `<meta name="base" content="/REPO/">` to `content="/"`, run `python3 -m http.server`, then open `http://localhost:8000/?r=app`.
