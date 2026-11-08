# shedeploys.dev

A single static page, hosted on
**Firebase Hosting**, deployed automatically from GitHub on every push to `main`.

- **Live:** https://shedeploys.dev

## Stack

- Static HTML/CSS — one page, no build step
- Firebase Hosting (global CDN + free SSL)
- GitHub Actions for CI/CD (deploy on push to `main`)
- Domain registered at Namecheap (apex → Firebase, `cv` subdomain → GitHub Pages)

## Project structure

```
shedeploys.dev/
├── public/
│   └── index.html            # the landing page (this is what gets deployed)
├── firebase.json             # hosting config (public dir, clean URLs, /cv redirect)
├── .firebaserc               # default project: shedeploys-dev
├── .gitignore
├── .github/
│   └── workflows/
│       └── deploy.yml         # GitHub Actions → Firebase Hosting
└── README.md
```

## Local development

Preview locally before deploying:

```bash
npm install -g firebase-tools   # once
firebase emulators:start --only hosting
# open the printed localhost URL
```

## Deploy manually

```bash
firebase login
firebase deploy --only hosting
```

## Automatic deploys (CI/CD)

Pushes to `main` trigger `.github/workflows/deploy.yml`, which deploys to the **live** channel.
It needs one repository secret: `FIREBASE_SERVICE_ACCOUNT_SHEDEPLOYS_DEV`.

Easiest way to generate the workflow **and** the secret automatically:

```bash
firebase init hosting:github
```

It asks for the repo (`agapasieka/shedeploys.dev`), creates a service account, stores the
secret in GitHub for you, and writes the workflow files. (The committed `deploy.yml` here does
the same thing — if you use `firebase init hosting:github`, let it overwrite or keep just one.)

---

# 📝 Blog draft — "How to host a simple blog page on Firebase Hosting (on a budget)"

> Steps captured here so they can be turned into a Medium article later. This mirrors exactly how
> this very site is built, so it doubles as a working reference.

## Intro
- The goal: a fast, good-looking personal page/blog on your own domain for ~the cost of a domain
  (Firebase Hosting's free Spark plan covers a small static site; the only real cost is the domain).
- Why Firebase Hosting: free SSL, global CDN, custom domains, dead-simple CLI, and clean GitHub
  Actions integration.

## What you need
1. A Google account (for Firebase).
2. A GitHub account + an empty repo.
3. Node.js installed locally.
4. (Optional) A custom domain — e.g. from Namecheap.

## Step 1 — Create a Firebase project
1. Go to the Firebase console → **Add project**.
2. Name it (e.g. `shedeploys-dev`), skip Google Analytics if you don't need it.

## Step 2 — Install the Firebase CLI
```bash
npm install -g firebase-tools
firebase login
```

## Step 3 — Initialise Hosting
```bash
firebase init hosting
```
- Select your existing project.
- Set the public directory to `public`.
- Configure as a single-page app? **No** (for a simple static site).
- Set up automatic builds with GitHub? **Yes** if you want CI/CD now (or run `hosting:github` later).

## Step 4 — Build your page
- Put your `index.html` (and any assets) in the `public/` folder.
- Keep it self-contained: inline CSS, web fonts from Google Fonts, no backend needed.

## Step 5 — Preview locally
```bash
firebase emulators:start --only hosting
```

## Step 6 — Deploy
```bash
firebase deploy --only hosting
```
Your site is now live at `your-project.web.app` and `your-project.firebaseapp.com`.

## Step 7 — Add your custom domain
1. Firebase console → **Hosting → Add custom domain**.
2. Enter your apex domain (e.g. `shedeploys.dev`) and verify ownership (TXT record).
3. Firebase gives you two **A records** — add them at your registrar (Namecheap → Advanced DNS,
   Host `@`). Repeat for `www` if you want it.
4. Firebase auto-provisions the SSL certificate (can take up to ~24h, usually faster).

## Step 8 — Automate deploys with GitHub Actions
```bash
firebase init hosting:github
```
- Point it at your GitHub repo.
- It creates a service account, stores it as a repo secret, and adds a workflow that deploys on
  every push to `main`. Commit, push, done — every future change deploys itself.

## Bonus — host your CV on a subdomain for free
- Keep a CV/resume on **GitHub Pages** at `cv.yourdomain.com`:
  - Add a `CNAME` record: Host `cv` → `yourusername.github.io`.
  - Set the custom domain in the repo's **Settings → Pages**.
- Optionally add a `/cv` redirect in `firebase.json` so `yourdomain.com/cv` points there.

## Wrap-up
- Total cost: just the domain (~£10–15/yr). Hosting, SSL and CI/CD are free at this scale.
- Next steps ideas: add more pages, pull posts from an RSS/Markdown source, add analytics.
