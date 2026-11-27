# ehon-site

This Japanese site for the app えいごで むかしばなし えほん is served
at <https://ehon.kengblog.com/>. Use its
[privacy policy](https://ehon.kengblog.com/privacy) and
[support page](https://ehon.kengblog.com/support) URLs for the App Store listing.

The Taiwan app 日本故事繪本 has its own Traditional Chinese pages. Use
<https://ehon.kengblog.com/zh-hant/privacy> and
<https://ehon.kengblog.com/zh-hant/support> for its App Store listing.
A 繁體中文 link at the top of the Japanese privacy and support pages also
leads to them. There is no Traditional Chinese landing page.

It is a plain static site: no framework, no build step, no trackers,
no web fonts.

```
public/
  index.html      landing page
  privacy.html    privacy policy
  support.html    support page
  zh-hant/{privacy,support}.html  日本故事繪本's pages, in Traditional Chinese
  img/{shelf,book-page,unlock-screen}.webp  app screenshots in the phone frames
  img/app-icon.webp  decorative header icon
  img/og-image.jpg   shared 1200×630 link preview image
  favicon-32.png     browser favicon
  apple-touch-icon.png  iOS Home Screen bookmark icon
  _headers        response headers for Cloudflare static assets
wrangler.jsonc    Cloudflare Worker config (static assets + custom domain)
```

## Editing the wording

When editing `public/index.html`, `public/support.html`,
`public/privacy.html`, or a page in `public/zh-hant/`, keep the page's
`<title>` and meta description in sync with its `og:title` and
`og:description`, respectively, in `<head>`. The share image shows the
Japanese app, so the `public/zh-hant/` pages have no `og:image`.

If a change affects text shown in a screenshot, replace the matching
image in `public/img/` and revise its `alt` text and dimensions in the
HTML as needed. For example, when the price changes, update the pricing
paragraph and unlock screenshot's `alt` text in `public/index.html`,
replace `public/img/unlock-screen.webp` and `public/img/og-image.jpg`,
and update `og:image:alt` on all three pages: `public/index.html`,
`public/support.html`, and `public/privacy.html`.

Keep the public policy in `public/privacy.html` and the in-app policy
consistent with the app's actual behavior. Do the same for
`public/zh-hant/privacy.html` and 日本故事繪本's app, and keep its button
labels and price in `public/zh-hant/support.html` matching that app. Use
the email address from `public/support.html` as the public contact
instead of the App Store listing, on the Traditional Chinese pages too.
The landing page footer links to both the privacy policy and support;
each of those pages links to the other in its footer. Each Traditional
Chinese page links to the other one in its footer.

Keep the App Store button in `public/index.html` disabled until the
listing is public. Then replace the entire `<button>` with the link
below, using the published listing URL, and update the availability
note below the button. Add a Google Play link only once the Android
app is listed.

```html
<a class="store" href="APP_STORE_URL"><b>App Store</b></a>
```

The header icon, browser favicon, iOS Home Screen icon, and share card
use the Japanese app's icon (`AppIcon-1024.png` in the app project); the
Traditional Chinese pages share the first three. When that app icon
changes, refresh all four assets.

## Preview locally

```sh
npx wrangler@4 dev
```

Then open <http://localhost:8787/>. A basic static file server pointed at
`public/` can preview the landing page, but the privacy and support pages
and the `zh-hant/` pages require Wrangler or another server that serves
`.html` files at extensionless URLs.

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

After deploying, open the landing page, both privacy policies, and both
support pages at their public URLs above.
