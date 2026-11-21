# ehon-site

This Japanese how-to-use page for the storybook apps will be served at
<https://ehon.kengblog.com/> once published.

It is a plain static site: no framework, no build step, no trackers,
no web fonts.

```
public/
  index.html      landing page
  img/*.webp      app screenshots shown in the phone frames
  favicon.svg
  _headers        response headers for Cloudflare static assets
wrangler.jsonc    Cloudflare Worker config (static assets + custom domain)
```

## Editing the wording

Edit the Japanese copy in `public/index.html`, including the page title
and meta description in `<head>`. If a change affects text shown in a
screenshot, replace the matching image in `public/img/` and revise its
`alt` text as needed. For example, a price change affects the pricing
paragraph, the `unlock-screen.webp` screenshot, and its `alt` text.

The two store buttons at the top are for the Japanese–English edition.
Each stays disabled and reads まもなく公開 until that edition is listed on
its store. Replace only that store's entire `<button>` with the matching
link below, using the published listing URL.

```html
<a class="store" href="JA_EN_APP_STORE_URL"><b>App Store</b></a>
<a class="store" href="JA_EN_GOOGLE_PLAY_URL"><b>Google Play</b></a>
```

## Preview locally

```sh
npx wrangler@4 dev
```

Then open <http://localhost:8787/>. Any static file server pointed at
`public/` works too.

## Publish

Publishing uses a Cloudflare Worker with static assets. `wrangler.jsonc`
points the Worker at `./public` and attaches it to `ehon.kengblog.com` as a
custom domain, so Cloudflare creates the DNS record and certificate on the
first deploy. The `kengblog.com` zone must be in the same Cloudflare
account.

```sh
npx wrangler@4 login     # once, opens the browser
npx wrangler@4 deploy
```

After the deploy, check <https://ehon.kengblog.com/>.
