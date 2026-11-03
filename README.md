# Jam.education — Company Website

> **AI made learning personal. We make it shared.**
> Small learning circles, guided by educators, powered by AI.

This is the landing page for **Jam.education**, the group company behind
[ExamMate.ai](https://www.exammate.ai/) and [Jam.guru](https://jam.guru/).
Jam.education is a global company building AI-powered education tools, with a special
focus on emerging markets, founded by IIT Madras alumni.

It's a plain static site with no framework and no build step.

---

## Running and deploying

Open `index.html` in a browser, or serve the folder locally:

```bash
python3 -m http.server 8000
```

To deploy, upload the whole folder to any static host (GitHub Pages, Netlify, Vercel,
Cloudflare Pages, S3). The metadata assumes the site lives at **`https://jam.education/`**.
If the domain is different, update it in `index.html`, `robots.txt` and `sitemap.xml`.

## Project structure

```
index.html            Page markup, SEO/social metadata, structured data
styles.css            All styles (colour tokens, layout, responsive rules)
assets/
  logo.png            Full wordmark logo (nav, footer)
  mark.png            Original logo mark (raster)
  mark.svg            Logo mark traced to vector (source for the hero and icons)
  piece-*.svg         Individual logo pieces used as decorations (banner, product cards)
  og-image.png        1200×630 social share card
  favicon-32.png      Browser tab icon
  apple-touch-icon.png  iOS home-screen icon (180px)
  icon-192.png, icon-512.png  Android / PWA icons
favicon.ico           Legacy favicon (16/32px)
site.webmanifest      Web app manifest (name, icons, theme colour)
robots.txt, sitemap.xml  Search-engine files
```

---

## Design notes

### 1. Starting from the brand

The whole design comes from the logo. It has four soft, organic shapes in four colours,
arranged around an empty centre, so they look like people in a circle. That's the
company idea: learning together in small circles. The page uses that picture as its
main visual.

### 2. Colour

The four brand colours were sampled pixel-exact from the logo file:

| Token      | Hex       | Used for |
|------------|-----------|----------|
| `--blue`   | `#409FFF` | Primary accent, ExamMate.ai, contact banner, theme colour |
| `--sky`    | `#88D9FD` | Secondary accent, gradients, focus rings |
| `--yellow` | `#FEC144` | Highlights ("AI tutor ready" spark, stat cards) |
| `--coral`  | `#FF7170` | Warm accent, Jam.guru |

Everything else is kept neutral (near-black ink `#111318`, slate grey `#4A5060`, white
and a very pale blue `#F6F9FD`) so the four brand colours stand out. For small text and
icons on light backgrounds, the yellow is darkened to `#E0A21E` so it's readable.

Each product gets its own colour: **ExamMate.ai is blue** and **Jam.guru is coral**.
The badge, ticks, button and corner shape on each card all use it. The headline's
gradient runs through all four colours, so the brand appears in the first thing people read.

### 3. Typography

The page uses one typeface: **[Outfit](https://fonts.google.com/specimen/Outfit)** from
Google Fonts. It's a rounded geometric sans-serif that's close to the logo's wordmark,
with the same round letters and friendly tone. Weights carry the hierarchy:

- **600** for headlines, with slightly tight letter spacing (`-0.02em`)
- **500** for buttons, labels and nav
- **300** for body and supporting text, which reads as calm and open

Headline sizes use `clamp()` so they shrink smoothly from desktop to phone without
breakpoint jumps.

### 4. The hero illustration is the real logo

The first version of the hero used hand-drawn shapes "inspired by" the logo. They
weren't close enough. So the four shapes were **traced directly from the brand PNG**:

1. Each pixel was assigned to its nearest brand colour, keeping the anti-aliased edge alpha.
2. Each colour mask was upscaled 8×, lightly smoothed and thresholded to recover a clean edge.
3. The outline was traced, resampled evenly, and converted into smooth cubic Bézier
   curves (Catmull-Rom → Bézier).

The result is `assets/mark.svg`, a vector version of the mark that stays sharp at any
size. The hero uses these paths inline. The favicon and app icons were also regenerated
from it, so the large icons are no longer upscaled from a 147px image.

The pieces have a gentle animation: each one drifts a few pixels **outward from the
centre** and comes back, like the circle opening and closing. It's turned off for
visitors who have asked their device for reduced motion.

Every other decorative shape on the page is cut from the same traced logo. Each piece
is saved on its own (`assets/piece-*.svg`, plus pale "tint" versions). The coral and
yellow pieces peek into the contact banner. The product cards carry a pale version of
their own colour's piece in the corner: the blue piece for ExamMate.ai and the coral
piece for Jam.guru. They're plain coloured SVG images rather than CSS masks, so they
render on any host, even from a local file.

Two small floating labels sit on top: "Study circle · 6 learners" and "AI tutor ready".
They show the product idea, people plus AI, without needing a screenshot.

### 5. Page structure

The page follows the order a first-time visitor's questions come in:

1. **Hero:** what is this? The tagline, one line of explanation, and two buttons
   (*Explore our products*, *About us*).
2. **Our approach:** why is it different? Three cards that match the tagline word for
   word: *small learning circles*, *guided by educators*, *powered by AI*.
3. **Products:** what can I use? One card each for ExamMate.ai (university students)
   and Jam.guru (independent learners), with a short pitch, three points and a link.
4. **About:** who's behind it? Global company, emerging-markets focus, IIT Madras
   alumni founders, summarised in four stat cards.
5. **Contact banner:** a blue banner with two real logo pieces (coral and yellow)
   peeking in from the edges, and an email button.
6. **Footer:** logo, product links, copyright.

Small coloured dots (shaped like the logo pieces) mark each section label, and the four
colours rotate through the sections.

### 6. Shape language

Rounded corners everywhere, with no hard edges: pill-shaped buttons, 22–32px card
corners, and icon tiles with slightly uneven corner radii so they echo the irregular
logo shapes. Cards use soft, low-contrast shadows and lift a little on hover.

### 7. Responsive and accessible

- Laid out for phones first: at ≤900px the two-column hero and about sections stack,
  and the card grids become single columns. At ≤640px the nav collapses to the logo
  and the *Get in touch* button. Tested at 1440px and 390px widths with no horizontal scrolling.
- Visible keyboard focus rings (sky blue) on all links and buttons.
- Decorative art is hidden from screen readers (`aria-hidden`), and real images have alt text.
- Animations are turned off for visitors who prefer reduced motion.
- Body text is ink or slate on white, well above WCAG AA contrast.

### 8. Sharing and SEO

- **Social share card** (`og-image.png`, 1200×630): logo, headline and tagline on white,
  with the brand shapes around the edge. This is what appears when the link is shared on
  WhatsApp, LinkedIn, Slack, Facebook or X.
- **Open Graph and Twitter/X card tags** with absolute URLs, so the preview shows on every platform.
- **Favicons** for browser tabs, iOS home screens and Android, plus a web app manifest
  and a `theme-color` that tints mobile browser bars blue.
- **Structured data** (schema.org `Organization`) that describes Jam.education and lists
  ExamMate.ai and Jam.guru as its products, so search engines understand the group.
- Canonical URL, `robots.txt` and `sitemap.xml`.

---

## Editing tips

- **Colours:** change the tokens at the top of `styles.css` (`:root`).
- **Copy:** all text is in `index.html`. Search for the section ids: `approach`,
  `products`, `about`, `contact`.
- **Contact email:** currently `hello@jam.education` (in the contact banner and the
  structured data). Update both places if it changes.
- **Share image:** replace `assets/og-image.png` with any 1200×630 PNG.
- **Higher-fidelity logo:** if the original vector logo is available, replace the four
  `<path>` elements in the hero `<svg>` and `assets/mark.svg` with the original paths.
