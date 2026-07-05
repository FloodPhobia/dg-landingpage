# Delta Green Campaign Hub — GitHub Pages Edition

A private campaign site for a Delta Green tabletop game, styled as a classified dossier. Built with Jekyll, hosted free on GitHub Pages at **dg.smessina.net** (domain DNS managed at Squarespace), and password-protected with StatiCrypt — every page is AES-encrypted at deploy time and unreadable without the shared passphrase.

Session recaps are Markdown files. The publishing workflow after each game is: write a Markdown file, `git push`, done. A GitHub Action builds the site, encrypts it, and deploys it automatically.

---

## One-time setup

### 1. Create the repository

1. Create a new GitHub repository (e.g. `dg-campaign`). Public or private both work — the *published site* is what StatiCrypt protects, and your *source* recaps are only as private as the repo, so **private repo is recommended** so the unencrypted Markdown isn't browsable. (GitHub Pages from a private repo requires a paid GitHub plan — GitHub Free only publishes Pages from public repos. If you're on Free and want the repo private, the compromise is a public repo where recap Markdown is technically visible to someone who finds the repo; the deployed site itself stays encrypted either way.)
2. Push this folder's contents to the repo's `main` branch.

### 2. Set the encryption secrets

Generate a salt once (any 32-character hex string):

```bash
openssl rand -hex 16
```

In the repo: **Settings → Secrets and variables → Actions → New repository secret**. Add two secrets:

| Name | Value |
|---|---|
| `STATICRYPT_PASSWORD` | The shared passphrase your players will type (e.g. `NIGHT-AT-THE-OPERA`) |
| `STATICRYPT_SALT` | The hex string you generated |

The salt must stay constant between deploys — it's what lets the "keep this terminal authenticated" (remember me) option survive site updates, so players aren't re-prompted every time you post a recap. Don't regenerate it unless you *want* to force everyone to log in again (which, incidentally, is also how you'd do a soft "log everyone out": change the salt or the password).

### 3. Enable GitHub Pages

**Settings → Pages → Build and deployment → Source: GitHub Actions.**

That's it — the included workflow (`.github/workflows/deploy.yml`) handles the rest. Push to `main` and watch the **Actions** tab; the first deploy takes a couple of minutes.

### 4. Point the domain (Squarespace DNS)

1. In Squarespace: **Domains → smessina.net → DNS → DNS Settings → Add Record**:
   - **Type:** CNAME
   - **Host:** `dg`
   - **Data:** `YOUR-GITHUB-USERNAME.github.io` (note: your *username*, not the repo name)
2. Back in the GitHub repo: **Settings → Pages → Custom domain** → enter `dg.smessina.net` → Save. GitHub runs a DNS check; DNS propagation usually takes minutes but can take up to a day.
3. Once the check passes, tick **Enforce HTTPS**. GitHub provisions a free certificate automatically.

The `CNAME` file in this repo keeps the custom domain setting from being wiped on redeploys — it's already set to `dg.smessina.net`; edit it if you choose a different subdomain.

**Recommended:** verify your domain with GitHub to prevent subdomain takeover if you ever disable Pages: GitHub **profile → Settings → Pages → Add a domain**, then add the TXT record it gives you in Squarespace DNS.

### 5. Customize the campaign

Edit `_config.yml` — operation codename, your handler name, cell designation, schedule, and the VTT URL all live there and flow into the site automatically. Then edit `roster.md` with your actual agents.

---

## Per-session workflow (the part you'll actually do every week)

1. Copy the sample post in `_posts/` and rename it: `_posts/YYYY-MM-DD-short-title.md`. The date in the filename is the post date.
2. Edit the front matter:

```yaml
---
title: "Interviews and Bad Coffee"
session: 4
agents:
  - AGENT MADISON
  - AGENT MOROCCO
casualties: "None (a first)"
san: "3 points"
---
```

The post layout builds the AAR header table from these fields automatically; `casualties` renders pre-redacted. Omit any field (like `san`) to drop that row.

3. Write the recap below in plain Markdown. Two special tricks:
   - **Redact anything** with `<span class="dg-redacted" tabindex="0">hidden text</span>` — players hover or tap to declassify.
   - Put `<!--more-->` after the first paragraph or two to control what appears as the excerpt on the Briefing and Reports list pages.
4. Commit and push to `main`. The Action rebuilds, re-encrypts, and redeploys — live in ~2 minutes.

You can do all of this from the GitHub website (Add file → Create new file in `_posts/`) if you don't want to touch a terminal, or even from the GitHub mobile app right after the session.

## Previewing locally (optional)

```bash
bundle install
bundle exec jekyll serve
```

Then open http://localhost:4000. Local preview is unencrypted — encryption only happens in the deploy pipeline.

## How the protection works (and its limits)

StatiCrypt encrypts each HTML page with AES-256 derived from your passphrase. Visitors hit a themed "RESTRICTED ACCESS" prompt; the correct passphrase decrypts the page in their browser and can be remembered for 30 days. Without the passphrase, the served files are ciphertext — genuinely unreadable, not just hidden.

Honest caveats: the CSS file and image assets are *not* encrypted (nothing sensitive lives there, but the theme itself is visible); page URLs are visible in the repo/deploy logs; and a shared passphrase is only as secret as your least discreet player. This is excellent protection for a gaming group, not for state secrets — which, given the subject matter, feels thematically appropriate.

## Adding comments later

When you're ready: [Giscus](https://giscus.app) drops into `_layouts/post.html` with one script tag and stores comments in GitHub Discussions (players need GitHub accounts; repo must be public). Ask your friendly AI to wire it up when the time comes.

## Legal footnote

Delta Green is a trademark of the Delta Green Partnership. Private fan campaign use; the disclaimer in the site footer should stay.
