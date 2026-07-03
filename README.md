# ufuk.dev

Personal site of Ufuk Kayserilioglu — built with [Jekyll](https://jekyllrb.com)
and a custom terminal-flavored editorial theme. Live at
[ufuk.dev](https://ufuk.dev).

## Development

```sh
bundle install
bundle exec jekyll serve   # http://127.0.0.1:4000
```

Note: `_config.yml` changes require a server restart; Jekyll only reads the
config at boot.

## Content

| What | Where |
|---|---|
| Home page bio | `index.md` |
| Talks | `_talks/*.md` (`title`, `event`, `date`, `video`, `slides`, `site`) |
| Podcast appearances | `_podcasts/*.md` (`title`, `podcast`, `episode`, `date`, `link`, `video`) |
| Blog posts | `_posts/` (standard Jekyll posts) |
| Identity (name, role, tagline, avatar, domain, socials) | `_config.yml` |

Talks and podcasts render as year-grouped cards with collapsed
abstracts/show notes; each card gets a slug anchor (`/talks/#talk-title`)
that the home page deep-links to.

## Theme

The stylesheet is plain CSS — no preprocessor, no build step:
`assets/css/main.css` is served as-is. All design tokens (colors, fonts,
light/dark palettes) live in the tokens section at the top as CSS custom
properties; components only use `var(--*)`. Light/dark mode follows
`prefers-color-scheme`, can be toggled from the header, and persists via
`localStorage`.

Sections: tokens · base · chrome (header/nav/footer) · home (hero) ·
listing (def…end blocks, cards) · article (post prose) · syntax
(code highlighting).

## Open Graph image

The social card (`assets/images/og-image.png`) is generated from
`_og/template.html.erb`, populated with values from `_config.yml`:

```sh
bundle exec rake og_image
```

Rendering uses [Ferrum](https://github.com/rubycdp/ferrum) driving headless
Chrome (set `CHROME_BIN` to point at a specific binary). Rerun after changing
`domain`, `title`, `tagline`, `description`, `avatar`, or `navigation`, and
commit the regenerated PNG.

## Deployment

Pushes to `main` build and deploy to GitHub Pages via
`.github/workflows/jekyll.yml` (publishes `_site/` to the `gh-pages` branch;
`CNAME` points the ufuk.dev domain at it).

## License

MIT — see [LICENSE](LICENSE).
