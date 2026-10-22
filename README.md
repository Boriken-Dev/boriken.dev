# boriken.dev

The public site: the Boriken Dev landing page, and a page per app.

**Nothing about source code lives here.** The apps themselves are closed
source in their own private repositories; this repository holds only what is
meant to be read by anyone — what each app is, how to use it, and the privacy
policy the app stores require at a public URL.

## Layout

```
index.html              the landing page: who we are, and the apps
es/index.html           the landing page, Spanish
style.css               one stylesheet, hand-written, no build step
img/<app>/              icon and screenshots for that app
img/<app>/README.md     where those screenshots came from, and the rules for new ones
<app>/index.html        the app's page
<app>/privacy.html      privacy policy
<app>/es/index.html     the app's page, Spanish
<app>/es/privacidad.html  privacy policy, Spanish
<app>/CHANGELOG.md      pushed here by the app's own release workflow
```

English is the default everywhere; every page links to its other language in
the header. Dominada is the one exception to the privacy paths: its policies
were already in store listings, so they stay at `dominada/en/privacy.html` and
`dominada/privacidad.html`. Its English page used to be `dominada/en/`, which
now sends the browser to `dominada/`.

A second app is a new directory and one card on the landing page. That is the
whole extension mechanism, and it is deliberately boring.

No generator, no dependencies, no build. Open a file to edit it; open
`index.html` in a browser to see it. A marketing page that needs a toolchain
resurrected before a typo can be fixed does not get its typos fixed.

## Keeping an app's page in step with its own repository

Each app is developed in its own private repository, and its page here will
drift unless somebody moves the text. Two ways to handle that, in order of
how much you should want them:

1. **By hand, at release.** An app's page changes when the app ships something
   users can see, which is rare. A person copying the wording across at that
   moment will also notice when the description has quietly gone stale, and a
   script will not.
2. **Pushed from the app's repository.** If it does become a chore, the app's
   own release workflow can push here directly, using a deploy key scoped to
   this repository alone. Keep the ownership boundary strict: an app may write
   `<app>/` and `img/<app>/`, and nothing else.

   **There is no pull request, on purpose.** What may become public is decided
   once, by the allow-list in the app's publishing workflow; re-reading a diff
   that says `CHANGELOG.md -> dominada/CHANGELOG.md` every release carries no
   information and costs four manual steps. The review belongs on changes to
   that list, and the commit on `main` is the record of what went out. Today
   Dominada uses this for `dominada/CHANGELOG.md` and nothing else — which is
   why that file is not edited here: a fix would be overwritten by the next
   release.

Whichever is used, **the privacy URL must stay stable.** It goes into store
listings, and changing it later means editing every listing.

## Publishing

Served by GitHub Pages from `main`. The custom domain lives in `CNAME`.

## Before adding screenshots

Screenshots go in `img/<app>/`. They are public, so:

- **No real people's names**, and no faces, documents or anything identifying.
  Use demo data — a screenshot of a live game will show whoever was playing.
- **Capture from a disposable device seeded with demo data**, never from a
  phone somebody actually uses: a device that holds no real game cannot leak
  one. For Dominada that is an emulated Android container — the how, and the
  traps, are in `img/dominada/README.md`.
- Phone dimensions, not a resized desktop window: an app store rejects desktop
  aspect ratios, and the same images usually serve both.

## Contact

`contact@boriken.dev` — published on every page and in the store listings, so it has to receive mail.
