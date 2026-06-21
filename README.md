# FARDEVBOT-site

Source + auto-deploy for the public site at **[fardevbot.com](https://fardevbot.com)**
(Cloudflare Pages project `fardevbot-site`).

## Layout

```
public/            # the deployed static site (one HTML file + assets)
  index.html       # the whole site (inline CSS/JS, no external scripts/fonts/CDNs)
  snapshot.json    # FARQUANT paper track record — percentages/index only, never dollars
  _headers         # Cloudflare security headers (CSP, etc.)
  favicon.svg
  og.png           # 1200x630 social card
.github/workflows/deploy.yml   # publishes public/ to Cloudflare Pages on every push
```

## How it deploys

Any push that touches `public/**` runs the `deploy-site` GitHub Action, which uses the
official `wrangler` CLI to publish `public/` to Cloudflare Pages. The only repo secret is
`CLOUDFLARE_API_TOKEN` (Actions → Secrets); the Cloudflare account ID is non-secret and
lives in the workflow.

`snapshot.json` is refreshed daily: the trading box regenerates a sanitized snapshot and
pushes just that file here via the GitHub API, which triggers the deploy. The box never
exposes an inbound connection and never publishes a dollar figure — only percentages and a
start=100 normalized index.
