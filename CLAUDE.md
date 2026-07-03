# CLAUDE.md

Personal Jekyll site for ufuk.dev. See README.md for the general overview;
this file covers what matters when making changes.

## Commands

```sh
bundle exec jekyll serve        # dev server (config changes need a restart)
bundle exec jekyll build        # build to _site/
bundle exec rake og_image       # regenerate assets/images/og-image.png
```

## Design language

The theme is "terminal-flavored editorial": proportional sans for all prose
and titles, monospace reserved strictly for terminal/code-flavored chrome
(header, nav, prompts, `def`/`end` markers, dates, meta lines, footer
links). Do not let mono creep into prose or headings. Single accent color
is Ruby red; all colors/fonts are CSS custom properties in
`assets/css/_sass/theme.scss` (`:root` = light, `[data-theme="dark"]` =
dark) — components must only use `var(--*)`, never hardcoded values.

Structural motifs, use them for new sections/pages:

- Section = `def <name>` … `end` block (`.def-block` / `.section-head` /
  `.section-body` / `.section-end` in `listing.scss`)
- List entries = cards (`.entry`), grouped under `# <year>` markers
  (`.list-marker`), long text collapsed in `<details class="entry-abstract">`
- Bracket-style mono links: `[video]`, `[slides]` (`.entry-links`)

## Gotchas

- `domain` (not `host`) holds the bare hostname for display. `host` is a
  reserved Jekyll key — the `jekyll serve` bind address — and setting it to
  a domain makes the dev server fail with `EADDRNOTAVAIL`. `site.url` is
  overridden to localhost by `jekyll serve`, which is why display uses
  `site.domain`.
- The `analytics` config key contains a verbatim HTML snippet emitted by
  `_layouts/default.html`; there is no analytics include. Blank key =
  analytics off.
- Talks/podcasts collections have `output: false` but their `content` still
  renders as HTML on the listing pages — don't add `markdownify`.
- Home-page talk/podcast links depend on entry ids being
  `{{ title | slugify }}` on both the home layout and the listing pages;
  keep them in sync.
- After changing `domain`, `title`, `tagline`, `description`, `avatar`, or
  `navigation` in `_config.yml`, rerun `rake og_image` and commit the PNG.
  The `tagline` is rendered as a pipeline split on `" → "` (site hero and
  OG image both accent the arrows and the final segment).
- `design-options/` holds historical design mockups; it is excluded from
  the Jekyll build. The shipped design is `option-2d-combined.html`.
- The dark theme keeps code blocks monokai-dark in *both* modes
  (intentional); inline code adapts via `--bg-raised`.
