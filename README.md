# Coba Site

Marketing site for [Coba](https://www.coba.games) (Card Online Battle Arena), a Studio Palouse project. The game itself lives in [WorkingJB/Coba](https://github.com/WorkingJB/Coba) and is served at https://app.coba.games.

Built with [Astro](https://astro.build) as a fully static site (no client JavaScript) and hosted on Vercel. Colors and fonts come from the game client (`apps/web/src/style.css` in the Coba repo). It follows the system light or dark setting.

## Development

```sh
pnpm install
pnpm dev       # http://localhost:4321
pnpm build     # static output in dist/
pnpm check     # type-check .astro and .ts files
```

## Layout

| Path | What |
| --- | --- |
| `src/pages/index.astro` | Home page: hero, how a match works, features, faction war, status |
| `src/pages/404.astro` | Not found page |
| `src/layouts/Base.astro` | Head/meta, header, footer |
| `src/styles/global.css` | Design tokens (light and dark) and base styles |
| `public/favicon.svg` | Site icon |
| `scripts/og.svg` | Source for `public/og.png`, the social share image |

To regenerate the share image after editing `scripts/og.svg`:

```sh
node -e "require('./node_modules/.pnpm/node_modules/sharp')('scripts/og.svg').png().toFile('public/og.png')"
```

## Copy

Keep the status section honest as the game changes (private alpha, playtest key, placeholder art and names). No em dashes in user-facing copy. Playtest requests go to `contact@palouse.io`.

## Deploying (Vercel)

Vercel detects Astro automatically; no `vercel.json` is needed.

1. In Vercel (team **Palouse**), **Add New → Project** and import `WorkingJB/Coba-Site`. Keep the detected Astro preset.
2. Pushing to `main` deploys production; other branches get preview URLs.
3. Under **Settings → Domains**:
   - add `www.coba.games` as the primary domain
   - add `coba.games` and set it to redirect (308) to `www.coba.games`

## DNS (Namecheap)

| Host | Type | Value | Notes |
| --- | --- | --- | --- |
| `@` (coba.games) | A | Vercel's apex IP (shown in the Vercel domain settings) | Redirects to www in Vercel |
| `www` | CNAME | the `*.vercel-dns-*.com` target Vercel shows | The marketing site |
| `app` | A / CNAME | the game host (Fly) | Leave pointed at the game, not Vercel |

Today all three hosts point at the old Fly deployment. Move `@` and `www` to Vercel only once `app.coba.games` is serving the game, so players following old links still land somewhere that works.
