# IVs For Me

Marketing website for **IVs For Me** — physician-led IV drip therapy, medical weight loss (GLP-1),
wellness shots, and peptide therapy across Queens & Long Island.

Static HTML site (no build step). Deployed to GitHub Pages from `main`.

## Structure

- **Main pages** — `index.html`, `iv-drip-menu.html`, `wellness-shots.html`, `peptides.html`,
  `weight-loss.html`, `semaglutide.html`, `tirzepatide.html`, `retatrutide.html`,
  `iv-membership.html`, `faq.html`
- **Blog** — `blog.html` (index) plus 13 physician-written articles
- **Shared** — `carla-sans.css` (brand font), `favicon.svg`, `sitemap.xml`, `robots.txt`, `404.html`

## Design system

- Colors: cream `#FBF8F2`, forest green `#123524`/`#2E7D4F`, gold `#E8D24A`, sky blue `#6FA5C9`,
  lime `#9BC96B`, ink `#1A1A1A`
- Type: **Bricolage Grotesque** (display) + **Carla Sans** / **DM Sans** (body & headings)

## SEO

Every page ships with a unique title and meta description, canonical URL, Open Graph / Twitter
cards, and JSON-LD structured data (`MedicalClinic`, `MedicalWebPage`, `FAQPage`, `Blog`,
`BreadcrumbList`). A sitemap and robots.txt are included.

## Deploy

Pushing to `main` triggers `.github/workflows/deploy-pages.yml`, which publishes the site to
GitHub Pages. Live at: https://aririkushimeikito.github.io/ivsforme/
