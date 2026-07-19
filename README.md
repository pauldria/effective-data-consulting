# effective-data-consulting

Website for [effectivedataconsulting.com](https://effectivedataconsulting.com).

Plain static HTML — no build step, no framework, no dependencies. Deployed to
GitHub Pages by GitHub Actions on every push to `main`.

## Layout

| Path                           | Purpose                                                   |
| ------------------------------ | --------------------------------------------------------- |
| `index.html`                   | The site. Self-contained: markup + inline CSS, no assets.  |
| `404.html`                     | Served by Pages for unmatched paths.                       |
| `.github/workflows/static.yml` | Uploads the repo root and deploys it to Pages.             |

There is deliberately **no `CNAME` file** and **no `.nojekyll` file** — see
[Deployment](#deployment) for why.

## Contributing

There is no toolchain to install.

```bash
git clone git@github.com:pauldria/effective-data-consulting.git
cd effective-data-consulting
python3 -m http.server 8000     # then open http://localhost:8000
```

Edit `index.html`, reload the browser. Open a PR against `main`; merging deploys.

## Deployment

`static.yml` is GitHub's official `pages/static.yml` starter workflow, unmodified
apart from pinning the branch to `main`. It runs on push to `main` and on manual
dispatch, uploads the entire repo root as the Pages artifact, and deploys it as-is.

Two consequences worth knowing:

- **No Jekyll runs.** Artifact-based deploys serve files verbatim, so a `.nojekyll`
  marker would do nothing.
- **A `CNAME` file does nothing.** Per GitHub's docs, when publishing from a custom
  Actions workflow "no `CNAME` file is created, and any existing `CNAME` file is
  ignored and is not required." The custom domain lives in repo settings only.

Watch a deploy at [Actions](https://github.com/pauldria/effective-data-consulting/actions).

### Required repo settings

One-time, through the GitHub UI — these cannot be committed:

1. **Settings → Pages → Build and deployment → Source** must be **GitHub Actions**.
   If it is left on "Deploy from a branch", the workflow fails at the deploy step.
2. **Settings → Pages → Custom domain** → `effectivedataconsulting.com`, then Save.
   This is the *only* place the domain is configured.
3. **Enforce HTTPS** — tick it once GitHub finishes provisioning the certificate.
   The docs allow up to 24 hours, though it is usually minutes.

Recommended: verify the domain under **Settings → Pages → Verified domains** before
pointing DNS at it, which blocks takeover of the domain by other GitHub accounts.

### DNS

Apex domain needs four `A` records:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

Optionally `AAAA` for IPv6:

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

Optionally a `CNAME` for `www` → `pauldria.github.io` (no repo name). GitHub then
redirects `www` → apex automatically.

Verify with:

```bash
dig effectivedataconsulting.com +noall +answer -t A
```

## Verifying a deploy

`index.html` carries a `deploy-marker` comment. Bump it when you want to confirm a
specific build reached production:

```bash
curl -s https://effectivedataconsulting.com | grep deploy-marker
```
