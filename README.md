# ehon-site

This Japanese how-to-use page for the storybook apps will be served at
<https://ehon.kengblog.com/> once published.

It is a plain static site: no framework, no build step, no trackers,
no web fonts.

```
public/
  index.html      the whole page; all wording lives here
  img/*.webp      app screenshots shown in the phone frames
  favicon.svg
  _headers        response headers for Cloudflare static assets
wrangler.jsonc    Cloudflare Worker config (static assets + custom domain)
```

## Editing the wording

Every piece of page text is in `public/index.html`. Change the Japanese
text in place; each sentence sits on its own line, so a wording edit is a
one-line change. The page title and share text are in the `<head>` at the
top of the same file (`<title>`, `description`, `og:title`,
`og:description`).

The store buttons at the top are disabled and read まもなく公開 until the
apps are released. To make one live, replace its `<button ... disabled>`
with a link to the store page (`<a class="store" href="...">`).

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
