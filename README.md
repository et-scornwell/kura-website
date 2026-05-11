# Kura Holdings website

The single-page marketing site for [kura.holdings](https://kura.holdings). Built with [Astro](https://astro.build/) as a static site — no backend, no database, just HTML/CSS that's generated at build time.

## What's where

```
kura website/
├── public/                    # static files served as-is
│   ├── favicon.png            # browser tab icon
│   ├── apple-touch-icon.png   # icon when saved to iOS home screen
│   └── logos/
│       ├── kura-full-dark.svg   # dark logo (used on light backgrounds)
│       └── kura-full-light.svg  # light logo (used on dark backgrounds, e.g. footer)
└── src/
    ├── layouts/
    │   └── Base.astro         # <head>, fonts, favicon, meta tags
    ├── pages/
    │   └── index.astro        # the one page — composes all sections
    ├── components/            # one file per section, easy to edit independently
    │   ├── TopBar.astro
    │   ├── Hero.astro
    │   ├── WhatWeDo.astro
    │   ├── Criteria.astro
    │   ├── Team.astro
    │   ├── PersonCard.astro
    │   ├── Approach.astro
    │   ├── Contact.astro
    │   └── Footer.astro
    └── styles/
        └── global.css         # brand colors (CSS variables), base typography
```

## Common commands

Run these from inside the `kura website` folder in your terminal:

| Command           | What it does                                              |
| :---------------- | :-------------------------------------------------------- |
| `npm install`     | Install dependencies (only needed once, or after a pull)  |
| `npm run dev`     | Start the local dev server at http://localhost:4321       |
| `npm run build`   | Generate the production site into `dist/`                 |
| `npm run preview` | Serve the built `dist/` locally to double-check it        |

While `npm run dev` is running, edits to any source file hot-reload in the browser within a second or two.

## Editing the site

- **Copy changes** — open the relevant component in `src/components/` and edit the text. For example, the headline lives in `src/components/Hero.astro`, and the bullet list lives in `src/components/Criteria.astro`.
- **Bio changes** — Steve and Tom's bios are in `src/components/Team.astro` as two JavaScript objects (`steve` and `tom`) at the top of the file.
- **Brand colors** — change the CSS variables at the top of `src/styles/global.css`. Updating `--ember`, `--ink`, etc. propagates everywhere.
- **Logos / favicon** — replace the files in `public/` with the same filenames.
- **Adding a real headshot** — drop the image into `public/team/steve.jpg`, then in `src/components/PersonCard.astro` replace the placeholder `<div class="person-photo">…</div>` with `<img src={headshot} alt={name} class="person-photo" />` and pass a `headshot` prop from `Team.astro`.

## Publishing the site

The site is set up to deploy automatically via GitHub Actions to GitHub Pages, with `kura.holdings` as the custom domain. DNS stays at GoDaddy.

The pieces that make this work — already in the repo, you don't need to touch them:

- `.github/workflows/deploy.yml` — the GitHub Actions workflow that builds the Astro site and publishes it to GitHub Pages on every push to `main`.
- `public/CNAME` — tells GitHub Pages which custom domain to serve the site on (`kura.holdings`).
- `astro.config.mjs` — sets the canonical site URL to `https://kura.holdings`.

### One-time setup

**1. Push to GitHub with GitHub Desktop**

1. Open GitHub Desktop → **File → Add Local Repository** → pick this `kura website` folder.
2. If GitHub Desktop says "Not a Git repository," click the **"create a repository"** link in the message — it'll initialize git in this folder for you.
3. On the left, write a commit message like *"Initial site"* and click **Commit to main**.
4. At the top, click **Publish repository**. Name it (e.g. `kura-website`), pick public or private (either works), and click **Publish**.

**2. Turn on GitHub Pages**

1. In your browser, go to your new repo on github.com → **Settings → Pages**.
2. Under **Build and deployment → Source**, choose **GitHub Actions**.

That's it for GitHub Pages config — the workflow file in the repo does the rest.

**3. Watch the first deploy**

1. Click the **Actions** tab in the repo. You should see a workflow run called "Deploy site to GitHub Pages" running.
2. It takes 1–2 minutes. When it finishes (green check), your site is live at `https://<your-github-username>.github.io/<repo-name>/` — and GitHub will also be serving it at `kura.holdings` as soon as DNS is pointed there.

**4. Point GoDaddy DNS at GitHub Pages**

Log into GoDaddy → **My Products → DNS** for `kura.holdings`. You're going to add the records below. Delete or replace any existing `A` or `CNAME` records on `@` and `www` that point to GoDaddy's parking page or to anything else.

Add **four A records** on the apex (`@`), all pointing to GitHub's Pages IPs:

| Type | Host | Value             | TTL     |
| :--- | :--- | :---------------- | :------ |
| A    | @    | `185.199.108.153` | default |
| A    | @    | `185.199.109.153` | default |
| A    | @    | `185.199.110.153` | default |
| A    | @    | `185.199.111.153` | default |

Add **one CNAME** for the `www` subdomain (so `www.kura.holdings` redirects to the apex):

| Type  | Host | Value                            | TTL     |
| :---- | :--- | :------------------------------- | :------ |
| CNAME | www  | `<your-github-username>.github.io.` | default |

Replace `<your-github-username>` with your actual GitHub username, lowercase. The trailing dot is okay if GoDaddy adds it; either form works.

Save the changes.

**5. Verify and turn on HTTPS**

DNS usually propagates in a few minutes; can take up to 24 hours.

1. Back in your repo → **Settings → Pages**, scroll to **Custom domain**. Enter `kura.holdings` and click **Save**.
2. GitHub will run a DNS check. Once it shows a green checkmark, tick **Enforce HTTPS** (you may have to wait a few minutes for the certificate to issue).
3. Open `https://kura.holdings` in your browser — the site should load over HTTPS.

### Updating the site after launch

The day-to-day workflow once you're set up:

1. Make edits in your editor (or have me make them).
2. Open GitHub Desktop. You'll see the changed files in the left panel.
3. Write a commit message, click **Commit to main**, then click **Push origin** at the top.
4. GitHub Actions automatically builds and deploys the update — the live site refreshes in 1–2 minutes. You can watch the run under the **Actions** tab.

No manual build, no FTP, no copy-paste. Just commit and push.

## Known TODOs

- [ ] Steve's real headshot (currently a placeholder with initials "SC")
- [ ] Tom's real headshot (currently a placeholder with initials "TV")
- [ ] Open Graph image for social sharing previews (1200×630 PNG in `public/og-image.png`, then reference it from `Base.astro`)
