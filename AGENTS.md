# AGENTS.md

Guidance for AI coding agents working on this repository.

## Project overview

Static website for **Autotechnik Ing. Paul Troger** (car spare parts, Unterlamm-Bad Loipersdorf, Austria),
served at <https://www.autotechnik-troger.at>.

- Built with **Jekyll** via the `github-pages` gem and deployed by **GitHub Pages** (custom domain in `CNAME`).
- Theme: `jekyll-theme-architect`, customised by `_layouts/default.html` and `assets/css/style.scss`.
- Plugins: `jekyll-seo-tag` (`{% seo %}` in the layout) and `jekyll-sitemap`.
- Site language is **German (Austria, `de-AT`)**. All visible content, `alt` texts and `aria-label`s must be in German.

## Repository layout

| Path | Purpose |
| --- | --- |
| `_config.yml` | Site settings, plugins, `exclude` list, default layout (`default`) |
| `_layouts/default.html` | The only layout: header, main navigation (incl. "Ersatzteile" dropdown), footer, inline JS (dropdown + image lightbox) |
| `assets/css/style.scss` | Custom styles; imports the theme via `@import "{{ site.theme }}";` (needs the empty front matter at the top) |
| `assets/images/` | Images used on pages |
| `*.md` (root) | One file per page (e.g. `bremsen.md`, `kontakt.md`, `impressum.md`, `404.md`) |
| `_site/` | Generated build output — gitignored, never edit or commit |

## Local development

```bash
bundle install
bundle exec jekyll serve      # preview at http://localhost:4000
bundle exec jekyll build      # build into _site/ (use to verify changes compile)
```

There is no test suite; a successful `bundle exec jekyll build` without warnings is the verification step.
Keep the `Gemfile` on `github-pages` only, so local builds match GitHub Pages. Do not add plugins that GitHub Pages does not support.

## Content conventions

- Every page needs front matter with `title` and an explicit trailing-slash `permalink`:

  ```markdown
  ---
  title: Bremsen
  permalink: /bremsen/
  ---
  ```

  The layout is applied automatically (`defaults` in `_config.yml`); do not set `layout` per page.
- The page `title` is rendered as the `<h1>` by the layout — do not repeat it as a heading in the page body.
- Always build internal URLs with the `relative_url` filter, e.g. `{{ '/kontakt/' | relative_url }}` and
  `{{ '/assets/images/bremsen.jpg' | relative_url }}`.
- Product images use kramdown attributes: `![Bremsenteile]({{ '/assets/images/bremsen.jpg' | relative_url }}){: .product-image}`.
- Click-to-enlarge images use the existing markup (handled by the lightbox script in the layout):

  ```html
  <button class="click-to-enlarge" type="button" aria-label="Anzeige vergrößern" aria-expanded="false">
    <img src="{{ '/assets/images/anzeige.jpg' | relative_url }}" alt="…">
  </button>
  ```

- Use `**bold**` to highlight key information, consistent with existing pages.
- Phone numbers as `tel:` links (e.g. `[+43 650 8764376](tel:+436508764376)`).
- Pages that should not appear in the sitemap set `sitemap: false` (see `404.md`).

## Adding or renaming a page

1. Create/rename the root-level `.md` file with `title` and `permalink`.
2. Update the navigation in `_layouts/default.html` (top-level list or the "Ersatzteile" submenu) if the page should be linked.
3. Update any links to the old URL in other pages.
4. Run `bundle exec jekyll build` to verify.

## Styling and scripts

- Put CSS changes in `assets/css/style.scss`; follow the existing BEM-like class naming (`site-nav__toggle`, `image-lightbox__image`, state classes `is-open`).
- JavaScript lives inline at the bottom of `_layouts/default.html`; it is plain vanilla JS without a build step or dependencies. Keep it that way.
- Preserve accessibility: keep `aria-expanded`/`aria-controls`/`aria-label` attributes in sync, keyboard support (Escape, focus handling) and meaningful German `alt` texts.

## Housekeeping

- Never commit `_site/`, `.jekyll-cache/`, `vendor/`, `.bundle/` or `Gemfile.lock` (see `.gitignore`).
- Repository-only files (like `README.md` and this `AGENTS.md`) must be listed under `exclude` in `_config.yml` so they are not published.
- The project is proprietary (see `README.md`); do not add license files or third-party code without permission.
- Only commit when explicitly asked.
