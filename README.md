# Heaven Is Rare — Koaka Studios

The beat marketplace, built as a static site. A vending machine you can hear
before you buy. No build step, no server, no dependencies — GitHub Pages can
serve this folder exactly as it is.

```
index.html              the whole site (134 KB)
404.html                themed "slot empty" page
robots.txt
.nojekyll               tells Pages to serve the files as-is
assets/
  logo.jpg              brand image used across the page
  og.jpg                social preview card (link shares on IG, X, WhatsApp)
  favicon.png
  apple-touch-icon.png
audio/
  *.mp3                 ten 30-second tagged previews
```

Total: about 5 MB. GitHub Pages allows 1 GB, so there is a lot of room for
more beats.

---

## 1. Put it on GitHub

**From the browser** (no command line needed)

1. Go to <https://github.com/new>. Name the repo `koaka-studios`. Public.
   Don't add a README — this folder already has one.
2. On the empty repo page click **uploading an existing file**.
3. Drag in *everything* from this folder, including the `assets` and `audio`
   folders. Commit.
4. `.nojekyll` starts with a dot, so your file manager may hide it. On macOS
   press `Cmd + Shift + .` in Finder to show hidden files before dragging.
   If it still won't upload, see the note at the bottom.

**From the command line**

```bash
cd koaka-pages
git init -b main
git add -A
git commit -m "Heaven Is Rare — initial site"
git remote add origin https://github.com/YOUR-USERNAME/koaka-studios.git
git push -u origin main
```

## 2. Turn Pages on

Repo → **Settings** → **Pages** → under *Build and deployment* set
**Source: Deploy from a branch**, **Branch: `main`**, **Folder: `/ (root)`** →
**Save**.

Give it a minute or two. The site goes live at:

```
https://YOUR-USERNAME.github.io/koaka-studios/
```

That URL is real, public and shareable immediately. Everything works on it —
previews, catalogue, checkout links. You do not need a custom domain to start
selling.

---

## 3. The custom domain

**`koaka.studios` cannot be registered — `.studios` is not a real domain
ending.** The plural doesn't exist. `.studio` (singular) does, and it is
sold by most registrars. So the options are:

| Domain | Notes |
|---|---|
| `koaka.studio` | Closest to what you wanted. Reads as *koaka studio*. |
| `koakastudios.com` | Keeps the full brand name. `.com` is the safest for buyers. |
| `koaka.co.za` | Cheap, local, good if most buyers end up South African. |

Buy from Namecheap, Porkbun or Cloudflare. Expect roughly $20–30/year for
`.studio`, $10–15 for `.com`, and R100-ish for `.co.za`.

### Once you own it

**a.** Create a file named `CNAME` in the repo root (no extension) containing
just the domain, lowercase, nothing else:

```
koaka.studio
```

**b.** At your registrar's DNS settings, add these records. Four A records,
all with host `@`:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

And four AAAA records, also host `@`:

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

Then one CNAME record so `www` works too — host `www`, value
`YOUR-USERNAME.github.io` (no repo name, with the trailing dot if your
registrar wants one).

**c.** Back in **Settings → Pages**, enter the domain under *Custom domain*
and wait for the DNS check to pass. Then tick **Enforce HTTPS**. The
certificate can take up to an hour to issue — the padlock appearing is how
you know it's done.

> Don't add the `CNAME` file before you own the domain. Pages will start
> redirecting the working `github.io` URL to a domain that doesn't resolve,
> and the site will look broken.

**d.** Last step, easy to forget: open `index.html` and change the two
`og:image` / `twitter:image` tags from `assets/og.jpg` to the full
`https://koaka.studio/assets/og.jpg`. Some chat apps won't show the preview
image from a relative path.

---

## Adding a beat later

1. Drop the tagged 30-second preview into `audio/` as `<name>-preview.mp3`.
2. In `index.html`, find `const beats = [` and fill in the slot: `title`,
   `cat`, `bpm`, `key`, `tier`, `audio: 'audio/<name>-preview.mp3'`, and the
   `buy` checkout link.
3. Commit. Pages redeploys on its own, usually within a minute.

Slots without a checkout link stay hidden — `HIDE_UNLINKED = true` near the
top of the script controls that.

## Things worth knowing

- **Hard refresh after updating.** Browsers hold onto the old `index.html`.
  `Ctrl/Cmd + Shift + R`.
- **Checkout is Lemon Squeezy's**, not this site's. The page only sends the
  buyer there; payment, delivery and receipts happen on their side. A static
  page cannot take money or verify a buyer, and that's the right boundary —
  nothing sensitive lives in this repo.
- **The repo is public.** Anyone can read the source and download the preview
  MP3s. That's fine and unavoidable for any website — it's why the previews
  carry your voice tag and the full untagged files never go near this folder.
  Keep master WAVs out of here.

### If `.nojekyll` won't upload

Browsers sometimes refuse to drag dotfiles. On the repo page click
**Add file → Create new file**, type `.nojekyll` as the name, leave the body
empty, commit. Nothing here starts with an underscore, so the site will work
either way — the file just removes an unnecessary build step.
