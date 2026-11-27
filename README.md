# ehon-site

This Japanese site for the app えいごで むかしばなし えほん will be served
at <https://ehon.kengblog.com/> once published. Use its
[privacy policy](https://ehon.kengblog.com/privacy) and
[support page](https://ehon.kengblog.com/support) URLs for the App Store listing.

It is a plain static site: no framework, no build step, no trackers,
no web fonts.

```
public/
  index.html      landing page
  privacy.html    privacy policy
  support.html    support page
  img/*.webp      app screenshots shown in the phone frames, and the
                  app icon shown in the header (app-icon.webp)
  img/og-image.jpg  1200x630 picture shown when a page is shared
  favicon-32.png, apple-touch-icon.png   the app icon at browser sizes
  _headers        response headers for Cloudflare static assets
wrangler.jsonc    Cloudflare Worker config (static assets + custom domain)
```

## Editing the wording

For the landing page, edit `public/index.html`, including the page title
and meta description in `<head>`. If a change affects text shown in a
screenshot, replace the matching image in `public/img/` and revise its
`alt` text as needed. For example, a price change affects the pricing
paragraph, the `unlock-screen.webp` screenshot and its `alt` text, and
the share picture `og-image.jpg` with its `og:image:alt` text.

`public/privacy.html` is the public copy of the privacy page shipped
inside the app. When the app's page changes, carry the same meaning over
here, using the email address from `public/support.html` as the public
contact instead of the App Store listing. The landing page footer links
to both the privacy policy and support; each of those pages links to the
other in its footer.

The App Store button at the top stays disabled and reads 審査中 until
the app is listed there; the line under it says the app is for
iPhone・iPad and that the Android version is not released. Once the
listing is public, replace the entire `<button>` with the link below,
using the published listing URL, and update that line. Add a Google
Play link only once the Android app is listed.

```html
<a class="store" href="APP_STORE_URL"><b>App Store</b></a>
```

The header icon, favicons and `img/og-image.jpg` are made from the
app's own icon (`AppIcon-1024.png` in the app project); when the app
icon changes, export them again.

## Preview locally

```sh
npx wrangler@4 dev
```

Then open <http://localhost:8787/>. A basic static file server pointed at
`public/` can preview the landing page, but the privacy and support pages
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

After deploying, open the landing page, privacy policy, and support page
at their public URLs above.
