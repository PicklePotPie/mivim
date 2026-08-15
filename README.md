# mivim.net

A one-page site for Mivim. Static HTML, no build step, no dependencies.

```
index.html                  the whole site: markup, CSS, inline SVG icons
CNAME                       tells GitHub Pages the custom domain
.nojekyll                   skips GitHub's Jekyll pass
images/plate-wide.*         text-free landscape artwork (3:1)
images/plate-tall.*         text-free portrait artwork (2:3)
images/wordmark.*           keyed wordmark, transparent
images/wordmark-leaf.*      leaf V alone, for the shimmer layer
images/og.jpg               1200x630 link preview card
images/favicon-32.png       favicon, cropped from the leaf
images/apple-touch-icon.png 180x180 home-screen icon
```

The plate and the wordmark are separate layers. The plate carries no type, so
it can crop freely at any aspect ratio; the wordmark scales independently and
is never cut off. Portrait screens swap to the tall plate via `<picture>`.

## Preview locally

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Rotate the window past square to see the plate and wordmark swap.

## Turning on a social link

Each icon starts unlit. To light one up:

```html
<!-- before -->
<a class="orb" aria-disabled="true" aria-label="Instagram, not yet">

<!-- after -->
<a class="orb" href="https://instagram.com/mivim" aria-label="Instagram">
```

Add `href`, delete `aria-disabled`, shorten the label. Leave the `<svg>` alone.
To drop a platform entirely, delete its whole `<li>` block.

## Deploy

Push to `main`, then Settings, Pages, deploy from `main` at root. This is a
project site, not the `username.github.io` user site, so it does not collide
with thecalmbluesea.net.

## DNS at GoDaddy

Delete the parked `@` A record and any `www` CNAME first, and turn off Domain
Forwarding if it is on. Then:

| Type  | Name | Value              |
| ----- | ---- | ------------------ |
| A     | @    | 185.199.108.153    |
| A     | @    | 185.199.109.153    |
| A     | @    | 185.199.110.153    |
| A     | @    | 185.199.111.153    |
| CNAME | www  | USERNAME.github.io |

Back in Settings, Pages, enter `mivim.net`. GitHub redirects `www` to the apex
on its own. Wait for the certificate before ticking Enforce HTTPS, and do not
re-save the domain field while waiting.
