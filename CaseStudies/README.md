# Case Studies — Integration Guide

How to add a new product teardown / case study to the portfolio without
reinventing the process. Current live decks are listed at the bottom.

## How this works (GitHub Pages)

- Repo: `CocoCommits/CocoCommits.github.io`, published at
  https://cococommits.github.io/ — GitHub Pages serves the repo as static
  files, preserving the folder structure exactly.
- Clicking an HTML file **on github.com** shows its source code (the blob
  view). The **Pages URL** renders it as an actual page. Always link to the
  Pages URL, never to the github.com blob URL.
- The portfolio cards use relative hrefs (`CaseStudies/...`), which resolve
  correctly both when previewing `index.html` locally and when published.

## Rules for every new deck

1. **Self-contained folder**: create `CaseStudies/<Brand>_<Topic>/` holding
   the deck HTML plus all of its images. All image references must be
   relative to that folder. No absolute URLs and no external image hosts —
   this is what keeps decks working when external sites change or vanish.
2. **One canonical deck file per folder.** Give it an explicit filename
   (e.g. `Brand_Teardown.html`). Don't keep duplicate copies — compare file
   hashes before committing.
3. **Case-sensitive paths.** Pages runs on Linux: `IMG_4465.PNG` is not
   `img_4465.png`. Keep references matching the on-disk casing exactly.
4. **Avoid spaces in filenames.** If unavoidable, they must be URL-encoded
   in references (`%20`).
5. Keep images reasonably sized and lazy-loaded where the deck allows.

## Steps to add a new case study

1. Create `CaseStudies/<Brand>_<Topic>/` and drop in the deck HTML plus all
   assets (screenshots, css images, etc.).
2. Get the product logo — transparent PNG or SVG, at least ~100px tall —
   and commit it as `assets/logos/<brand>.svg` (or `.png`), lowercase. If
   the logo is a full-bleed square app icon, add `class="cs-ico"` to its
   `img` tag (renders it as a rounded tile).
3. In `index.html`, find `<section id="teardowns">` and copy an existing
   `<a class="cs-card">` block. Update:
   - `href` — relative path to the new deck
   - `aria-label` — `"<Brand> <deck type> (opens new tab)"`
   - `img src` — the logo path from step 2
   - `.cs-name` — brand name
   - `.cs-tip` — `<b>` deck type, then a 1–2 line summary (scope + standout
     angle). This is purely display text, edit freely.
4. No nav or JS changes needed — the "Product Teardowns" nav item and the
   scrollspy already point at the section. The grid is a centered
   `flex-wrap`, so it scales from 4 to 10+ cards automatically.
5. Test locally: open `index.html`, hover a card (tooltip + halo + lift),
   click through to the deck, confirm the deck's own images load.
6. Commit and push:

   ```
   git add CaseStudies/ index.html assets/logos/
   git commit -m "Add <Brand> case study"
   git push origin <branch>
   ```

7. After deploy, verify at
   `https://cococommits.github.io/CaseStudies/<Brand>_<Topic>/<deck>.html`.

## Logo sourcing notes

- The Wikimedia API works from the corporate network and is the first
  choice (`commons.wikimedia.org`, and `en.wikipedia.org` for brand logos
  Commons won't host for licensing reasons). Get the direct file URL:

  ```
  https://commons.wikimedia.org/w/api.php?action=query&titles=File:X.svg&prop=imageinfo&iiprop=url&format=json
  ```

  Space requests out — en.wikipedia rate-limits (HTTP 429).
- The corporate proxy blocks many brand sites (e.g. swiggy.com,
  dineout.co.in return 407). For those, download the logo manually in a
  browser and drop it into `assets/logos/`.
- Never hotlink an external logo URL in `index.html` — always commit the
  file to `assets/logos/` so the portfolio has zero external image
  dependencies.

## Current decks

| Brand   | Deck                        | Path |
|---------|-----------------------------|------|
| Rapido  | Feature Teardown            | `CaseStudies/Rapido_Product_Design/Rapido_Feature_Teardown.html` |
| Groww   | Search & AI Assistant       | `CaseStudies/Groww_Search_Improvements/GrowwAIAssistant_SlideDeck.html` |
| Netflix | Product Design · Elderly    | `CaseStudies/Netflix_Product_Design/Netflix_Improvemens_for_Elderly.html` |
| Dineout | Full-Funnel Teardown        | `CaseStudies/DineOut_ProductTeardown/Dineout_Product_Teardown.html` |
