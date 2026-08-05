# site

**mapomo.org**

The public landing page. Static, no tracking, no cookies, no third-party
analytics, which is stated on the page itself and is a commitment rather than an
oversight.

## Deployment

GitHub Pages, served from `main`. Live at
[mapomo-africa.github.io/site](https://mapomo-africa.github.io/site/).

There is no build step and no CI transform: what is in `main` is what is served.
That is deliberate. A landing page for a transparency project should be as
auditable as the rest of the project, and a reader who wants to check that the
page says what the repository says should not have to reason about a pipeline.

To point mapomo.org at it, add a `CNAME` file containing `mapomo.org` and set the
DNS records GitHub gives you under Settings, Pages.

## Structure

Single-page scroll narrative in `index.html`, with assets in `assets/`. Plain
HTML, CSS and JavaScript, no framework and no bundler, so the page stays legible
and auditable.

`logo.svg` is the source lockup as exported from Illustrator. The three files in
`assets/` are derived from it: the stacked lockup, the mark alone, and the
wordmark alone, recoloured for the dark background.

## Brand assets

`assets/logo-full.svg` stacked lockup · `assets/logo-mark.svg` mark ·
`assets/logo-wordmark.svg` wordmark. Gold is `#E8B13C`, ink is `#0A0F1E`.

## Content rules

Same neutrality rules as every other repository: no real party, candidate or
donor names anywhere, including in imagery and illustrative examples. Where the
page shows a detection example, it is labelled as illustrative.

## License

CC-BY-4.0 for content. Code within the page is Apache-2.0.
