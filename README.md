# RePlay

**RePlay reads your own streaming record back to you, and shows you where it lives.**

Every Stremio-style app keeps your library and your playback position on a server, so your
place follows you between devices. RePlay signs in to that server as you, reads what it holds,
and renders it: hours logged, what you played, what you opened and never played, when you
watch, what you gravitate to, and the whole chronicle in order.

It supports three backends:

| Backend | How it is read |
| --- | --- |
| Stremio | `api.strem.io`, the same API the official clients use |
| Nuvio | `api.nuvio.tv`, over its published PostgREST schema |
| Any Nuvio-compatible backend | give it a URL; it asks for `/.well-known/nuvio` |

## Why this repository is public

RePlay asks for your streaming account credentials. You should not have to take anybody's word
for what happens to them, so the whole thing is one file you can read in an afternoon.

What the code does, and what you can verify for yourself:

- **There is no server.** `index.html` is a static page. There is no backend to send anything to.
- **Your password goes to your own backend and nowhere else.** Search for `fetch(` and check
  every call site. They are your backend, Stremio's own Cinemeta catalogue (for artwork and
  genres), and Kit, only if you tick the newsletter box.
- **Your password is never stored.** The session key your backend returns is held in
  `sessionStorage` for the tab, or in `localStorage` if you tick "stay signed in". Signing out
  deletes it locally and invalidates it upstream.
- **No analytics, no cookies, no third-party scripts, no web fonts.** Artwork is linked from
  Stremio's own metadata CDN with `referrerpolicy="no-referrer"`.

If you find something that contradicts any of the above, please open an issue.

## Running it

There is no build step.

```sh
git clone <this repo> && cd replay
python3 -m http.server 8080
# then open http://127.0.0.1:8080
```

Opening `index.html` directly from disk mostly works too, though some browsers are stricter
about `file://` origins.

## Hosting

ElfHosted runs the public instance, and also hosts private Nuvio-compatible backends for
people who would rather their record sat somewhere they administer:
<https://elfhosted.com>

## The social card

`og.png` is generated from `og-card.html`, which is a standalone 1200x630 page using the same
palette. To regenerate after a copy or design change, screenshot it at that viewport:

```js
// playwright
await page.setViewportSize({ width: 1200, height: 630 });
await page.goto('file:///path/to/og-card.html');
await page.screenshot({ path: 'og.png' });
```

## Licence

[GNU AGPL v3](LICENSE). In short: use it, read it, change it, run it. If you run a modified
version where other people can reach it, publish your changes.
