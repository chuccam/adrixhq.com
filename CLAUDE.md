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
  - Ground `--bg #081630`, cards `--surface`, lines `--line`.
  - Accent mint `--accent #4ecb9b` for primary buttons, checkmarks, links (`--accent-ink`).
  - Status: `--ok`, `--warn`, `--bad`. Red means a real problem, never decoration.
- Type: Geist (text), Geist Mono (labels, code, terminal), Poppins 700 (logo only).
  Loaded from Google Fonts in each page's `<head>`; no other fonts.
- Dark (`color-scheme: dark`) on every page except the home page. The home page uses the
  light theme: `<body class="light">` redefines the tokens (off-white `--bg`, navy `--ink`,
  forest-green `--accent`, `--max` 1200px) in the `.light` block of `style.css`. Its header
  adds a "View app" button and its footer adds the wordmark. Do not add a light variant to
  other pages unless asked.

## 2. What the site is for

Right now it sells **one app: Adrix B2B Pricing** (free). The home page, `/apps/`,
About and Support speak about that app only. HS Duty Check and Net Terms are not linked
from anywhere until they are ready to install; their pages may stay at their URLs.

## 3. Page skeleton

Every page: same `<head>` block (charset, viewport, title, description, theme-color
`#081630`, favicon, canonical, og:*, fonts, `/style.css`), same header (logo + Apps ·
About · Support), same footer (© Adrix Studio · Apps · About · Support · Privacy · email).
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

## 5. Screens (`.mock`)

- Until real screenshots exist, screens are HTML mockups built from **the app's real
  copy**: headings, field labels, help text and button labels copied from the route
  files in `shopify-apps/apps/<app>/app/routes/`. Never invent a label the app lacks.
- Numbers inside screens must agree with each other and with the rules the app applies
  (a group rate beats volume breaks; the list badge summarises the editor's rule).
- Keep the note "Screens are simplified from the app for this page." under the features
  while any screen is a mockup. Replace mockups with real screenshots when they exist.
- No grey placeholder boxes. If a screen needs an image (a product thumbnail), draw a
  simple inline SVG.
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

- Content width `--max` 1120px inside `.wrap`; sections separated by the `section` border.
- Breakpoints already in `style.css`: 960px (hero stacks), 900px (feature rows stack),
  860px (grids go single column). New components get a rule at one of these.
- Check every changed page in a browser at desktop width before pushing, and scroll the
  whole page; the terminal blocks and tables are where overflow shows up first.

## 8. Before you push

- [ ] No hex colours outside `.mock`; no new fonts.
- [ ] Every claim traceable to a measurement or the app's code.
- [ ] Screens use the app's real labels; numbers consistent.
- [ ] Header and footer identical to the other pages; privacy link correct.
- [ ] Looked at it in a browser, top to bottom.
- [ ] After push: the live URL returns 200 and shows the change.
