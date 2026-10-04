# adrixhq.com — design rules

Static HTML + one stylesheet (`style.css`), served by GitHub Pages. No build step:
push to `main` and the site is live in about 30 seconds. Check the live URL after
every push (`curl -s -o /dev/null -w "%{http_code}" https://adrixhq.com/<path>/`).

Read this before changing any page. The rules exist because each one was broken once.

## 1. Brand

- Name: **AdrixHQ** on the site (logo wordmark), **Adrix Studio** as the legal name in
  footers and policies. App names start with "Adrix": *Adrix B2B Pricing*.
- Logo: the mark is `favicon.svg` (navy tile `#152b55`, white left stroke, mint right
  stroke). Header logo = that SVG inline + `<span>Adrix<b>HQ</b></span>` in Poppins 700.
  Do not redraw it per page; copy the header from an existing page.
- Colours: only the tokens at the top of `style.css`. **Never write a hex value in a
  page**, except inside `.mock` screens, which imitate Shopify's light admin on purpose.
  - Ground off-white `--bg #f7f7f4`, text navy `--ink`, cards `--surface`, lines `--line`.
  - Accent forest green `--accent #1d5c3c` for primary buttons, checkmarks; `--accent-ink`
    for labels and links; `--accent-soft` for icon tiles and the call-to-action band.
  - Status: `--ok`, `--warn`, `--bad`. Red means a real problem, never decoration.
- Type: Geist (text and labels), Geist Mono (code, terminal, small data keys), Poppins 700
  (logo only).
  Loaded from Google Fonts in each page's `<head>`; no other fonts.
- Light only (`color-scheme: light`), no dark theme. Thin grey borders, soft shadows,
  8px radius; no gradients, glows or decorative blobs.

## 2. What the site is for

Right now it sells **one app: Adrix B2B Pricing** (free). The home page, `/apps/`,
About and Support speak about that app only. HS Duty Check and Net Terms are not linked
from anywhere until they are ready to install; their pages may stay at their URLs.

## 3. Page skeleton

Every page: same `<head>` block (charset, viewport, title, description, theme-color
`#f7f7f4`, favicon, canonical, og:*, fonts, `/style.css`), same header (skip link, logo, Apps ·
About · Support, "View app" button), same footer (logo with © Adrix Studio · Apps · About ·
Support · Privacy · email). `<main id="main">` so the skip link lands.
Copy these from an existing page; never hand-write a variant.

- `<title>`: "Page — Adrix" or "Product — what it does for Shopify".
- Privacy link in footers points to the privacy policy of the app the site is selling
  (currently `https://b2b.adrixhq.com/privacy`).
- Contact is always `hello@adrixhq.com`.

## 4. Product page structure

A product page follows this order (see `b2b-pricing/index.html`):

1. **Hero** (`.hero-split`): eyebrow "Name · for Shopify · Price", one-line promise as
   `h1` with the second half in `.dim`, a lede of 2–3 sentences, primary + secondary
   button, and an app screen on the right.
2. **The problem**: three `.pain` cards, then the evidence (a `.term` block with the real
   error text and where/when it was measured).
3. **Features**: `.feature` rows, text and screen alternating (`.flip` on every second
   row). Each row: mono label, `h3`, one paragraph, three checkmark bullets, one screen.
   Use `.feature.wide` when the screen needs more room (tables, spreadsheets).
4. **Why us**: `.why` grid of six, each with an icon, a two-word title and one sentence.
5. **How it works**: `.steps`, four steps at most.
6. **Compared**: `.table-wrap` table against Shopify's built-in way of doing it.
7. **FAQ**: `.faq` with `<details>`, 5–8 questions merchants really ask.
8. **Callout** with the one next action.

## 5. Screens (`.shot`, `.mock`)

- App screens are **real screenshots** (`<figure class="shot">`, WebP in the page's `img/`),
  cropped to the part the row talks about, with an `alt` that says what it shows.
  - Capture with the app iframe about 800px wide (Chrome window ~1040px): the app switches
    to its compact layout and text stays legible at column width. Never include the
    Shopify sidebar or store name.
  - Shoot on the review/demo store (adrix-b2b-demo) and never save there; a state that
    needs saved data (warnings) is shot on the QA store and put back afterwards.
  - Export at most 1200px wide. When the app changes what a screen shows, reshoot it.
- Copy beside a screenshot must match it: the example names, rates and labels in the text
  are the ones in the image.
- `.mock` is only for things that are not the app (the CSV spreadsheet). Keep the note
  under the features saying the spreadsheet is an illustration while one is on the page.
- No grey placeholder boxes. If an illustration needs an image, draw a simple inline SVG.
- A screen must fit its column without horizontal scrolling at 1440px wide.

## 6. Copy

- English, second person ("your customers"), short sentences. No exclamation marks.
- **Only claim what is measured or in the code.** Limits come from the code
  (`MAX_GROUPS = 50`, `MAX_BREAKS = 20`, `MAX_IMPORT_ROWS = 500`); when a limit changes
  there, change it here. If something is not verified, leave it out.
- **Do not compete with Shopify.** No Shopify Plus prices, no "skip Plus", no attack on
  Shopify's plans. Say what works on Basic, Grow and Advanced; the merchant draws the
  conclusion. Reviewers work for Shopify.
- No "best", "first", "only", no invented statistics or testimonials (App Store rules;
  the site should match the listing).
- Say where a price shows up and where it does not (cart and checkout, not product pages).

## 7. Layout and responsiveness

- Content width `--max` 1200px inside `.wrap`. Each `main > section` opens with a divider as
  wide as the content; give a call-to-action section `class="plain"` to drop it.
- Breakpoints already in `style.css`: 960px (heroes and two-column blocks stack), 900px
  (feature rows stack), 860px (grids go single column), 760px (phone: header wraps,
  16px gutter). New components get a rule at one of these.
- Check every changed page in a browser at desktop width before pushing, and scroll the
  whole page; the terminal blocks and tables are where overflow shows up first.

## 8. Before you push

- [ ] No hex colours outside `.mock`; no new fonts.
- [ ] Every claim traceable to a measurement or the app's code.
- [ ] Screens use the app's real labels; numbers consistent.
- [ ] Header and footer identical to the other pages; privacy link correct.
- [ ] Looked at it in a browser, top to bottom.
- [ ] After push: the live URL returns 200 and shows the change.
