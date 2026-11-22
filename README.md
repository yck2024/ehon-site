# ehon-site

This Japanese how-to-use page for the app えいごで むかしばなし えほん
will be served at <https://ehon.kengblog.com/> once published, with the
app's privacy policy at `/privacy` and its support page at `/support`.

It is a plain static site: no framework, no build step, no trackers,
no web fonts.

```
public/
  index.html      landing page
  privacy.html    privacy policy, served at /privacy
  support.html    support page, served at /support
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

`public/privacy.html` is the public copy of the privacy page shipped
inside the app. When the app's page changes, carry the same meaning over
here, keeping support@kengblog.com as the public contact instead of the
App Store listing. The landing page footer links to both the privacy
policy and support; each of those pages links to the other in its footer.

The two store buttons at the top are for this Japanese–English app.
Each stays disabled and reads まもなく公開 until the app is listed on
that store. Replace only that store's entire `<button>` with the matching
link below, using the published listing URL.

```html
<a class="store" href="APP_STORE_URL"><b>App Store</b></a>
<a class="store" href="GOOGLE_PLAY_URL"><b>Google Play</b></a>
```

## Preview locally

```sh
npx wrangler@4 dev
```

Then open <http://localhost:8787/>. A basic static file server pointed at
`public/` can preview the landing page, but `/privacy` and `/support`
require Wrangler or another server that serves `.html` files at
extensionless URLs.

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
