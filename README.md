# effective-data-consulting

Website for [effectivedataconsulting.com](https://effectivedataconsulting.com).

Hugo static site, no theme, deployed to GitHub Pages by GitHub Actions on every
push to `main`.

## Quick start

```bash
brew install hugo
git clone git@github.com:pauldria/effective-data-consulting.git
cd effective-data-consulting
hugo server        # http://localhost:1313, live-reloads on save
```

Hugo is the only prerequisite, and only for local preview — you can publish from
any machine with a text editor and git.

## Adding a page

Add an article:

```bash
hugo new content/insights/my-article.md
```

Then edit it. The `/insights/` index, the homepage "Recent insights" block, and
the RSS feed all pick it up automatically — no layout or nav edits.

Add a top-level page: create `content/whatever.md`, then add an entry under
`[[menus.main]]` in `hugo.toml` if it should appear in the nav.

## Repo layout

```
content/          ← everything you write
  _index.md         homepage
  about.md
  approach.md
  contact.md
  startups/         audience track
  enterprise/       audience track
  insights/         articles; one file per article
data/
  services.yaml   ← single source of truth for the service catalogue
layouts/          ← templates; rarely touched after setup
  baseof.html       page shell
  home.html         homepage
  page.html         single pages
  section.html      section index pages
  services.html     custom layout for the two services pages
  404.html
  _partials/        head, header, footer, service-list
static/
  css/main.css    ← all styling, one file
```

`content/` and `data/` are yours. You should never need to open `layouts/` to
publish.

> **Note on `layouts/`:** Hugo v0.146.0 restructured this folder — `_default/`
> was flattened into `layouts/`, `partials/` became `_partials/`, and the
> homepage template is `home.html`, not `index.html`. Older Hugo tutorials will
> tell you otherwise.

## The services data file

Both `/startups/services/` and `/enterprise/services/` render from
`data/services.yaml`. Facts — name, category, duration, summary, deliverables —
are defined **once**. Each track supplies only its own framing:

```yaml
- id: governance
  name: "Data Governance & Quality"
  duration: "8–12 weeks"
  summary: "Ownership models, quality controls, metric definitions."
  framing:
    startup: "Lightweight by design..."
    enterprise: "Aligned to existing risk and audit obligations..."
  deliverables:
    - "Data quality scorecard"
```

This is deliberate. Two parallel audience tracks rot when one is updated and the
other is forgotten; keeping the facts in one place means a deliverable change
updates both tracks at once.

To add a service, append an entry. To reorder them, reorder the list.

## Deployment

`.github/workflows/hugo.yml` runs on push to `main` and on manual dispatch:
installs Hugo, runs `hugo --gc --minify`, uploads `./public` as the Pages
artifact, deploys.

**The Hugo version is pinned** in that workflow (`HUGO_VERSION`). Hugo ships
breaking template changes in minor releases, so an unpinned build can fail with
no commit to this repo. Bump it deliberately, and run `hugo server` locally on
the same version first.

Watch a deploy at [Actions](https://github.com/pauldria/effective-data-consulting/actions).

### Repo settings (already configured)

- Settings → Pages → Source = **GitHub Actions**
- Settings → Pages → Custom domain = `effectivedataconsulting.com`
- Enforce HTTPS enabled

There is deliberately **no `CNAME` file**: with Actions-based publishing GitHub
ignores it, and the domain is configured in repo settings only. There is no
`.nojekyll` either — artifact deploys serve files verbatim, so Jekyll never runs.

## Verifying a deploy

Every page carries a `deploy-marker` comment, set from `params.deployMarker` in
`hugo.toml`. Bump it when you want to confirm a specific build went live:

```bash
curl -s https://effectivedataconsulting.com | grep deploy-marker
```

## Conventions

- **Voice:** first-person plural ("we"). Firm, not personal brand.
- **No motion.** No scroll animation, no marquees. Stillness reads as established.
- **Claims are qualified.** Specificity earns credibility; adjectives do not.
- **Front matter:** `title` and `description` on every page. `weight` controls
  ordering. Services pages additionally need `layout: services` and `audience`.
