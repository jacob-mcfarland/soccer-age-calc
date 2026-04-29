# MVYSA Age Group Calculator

Branded calculator for the Mount Vernon Youth Soccer Association, embedded into the Sports Connect site via iframe.

## Deployment via GitHub Pages

GitHub Pages is the simplest, free option. The hosted file is served as `text/html` from your `username.github.io` subdomain, which is iframe-embeddable from any HTTPS site.

### One-time setup

1. Create a public GitHub repo (e.g., `mvysa-age-calculator`).
2. Add `mvysa-age-calculator.html` to the repo root.
3. In the repo: **Settings → Pages**.
4. Under **Source**, choose **Deploy from a branch**, select `main` and `/ (root)`. Click **Save**.
5. Wait ~30 seconds. The URL will appear at the top of the Pages settings — typically:
   `https://YOUR-USERNAME.github.io/mvysa-age-calculator/mvysa-age-calculator.html`
6. Verify it loads by opening that URL in a browser.

### Important — what NOT to use

These look right but won't work:
- `github.com/...` — shows source code, doesn't render
- `raw.githubusercontent.com/...` — served as `text/plain`, browsers refuse to render in an iframe

Only the `username.github.io` URL (after enabling Pages) works for iframe embeds.

## Embedding on the Sports Connect site

1. Log into mvysa.org as administrator.
2. Navigate to the page where you want the calculator (likely the Player Registration page or a new "Age Groups" page).
3. Add a new Content Module.
4. In the rich-text editor, click the **Source** or **HTML** view button (looks like `<>`).
5. Paste the contents of `mvysa-sports-connect-embed.html`, replacing `YOUR-GITHUB-USERNAME` with your actual GitHub username.
6. Save the module and exit edit mode to verify.

## Updating each year

When the season rolls over (around July), open `mvysa-age-calculator.html` in your repo and change one line:

```html
<div class="mvysa-calc" data-season-end="2028" data-club="Northwest United">
```

Bump `data-season-end` from `2027` to `2028` for the 2027-28 season. Commit and push — GitHub Pages auto-redeploys within a minute, and the iframe on mvysa.org picks up the change instantly. No Sports Connect edit needed.

## Customization

Two attributes on the outer div control everything:

- `data-season-end="2027"` — the ending year of the season (2026-27 → `2027`)
- `data-club="Northwest United"` — the club name shown in the U15+ warning

To change brand colors, search for `#2A3C42` in the file and replace.

## How the math works

US Soccer / Washington Youth Soccer uses an **August 1 cutoff**:

- Born **August–December** → soccer year matches birth year
- Born **January–July** → soccer year is the previous calendar year
- Age group = `season_start_year − soccer_birth_year`

Example: Born March 2015, season 2026-27 → soccer year = 2014 → 2026 − 2014 = **U12**.

## Files

- `mvysa-age-calculator.html` — the standalone calculator (host this on GitHub Pages)
- `mvysa-sports-connect-embed.html` — iframe snippet to paste into Sports Connect
