# Soccer Age Group Calculator

A configurable age group calculator embedded on Sports Connect sites for Skagit Valley youth soccer clubs. One codebase serves multiple orgs, each with their own branding and referral logic, via a single config file.

## Live URLs

| Org | URL |
| --- | --- |
| MVYSA | https://jacob-mcfarland.github.io/soccer-age-calc/?org=mvysa |
| Burlington Youth Soccer | https://jacob-mcfarland.github.io/soccer-age-calc/?org=burlington |
| La Conner Youth Soccer | https://jacob-mcfarland.github.io/soccer-age-calc/?org=la-conner |

## Files

- `index.html` — the calculator. Reads `?org=` from the URL and renders the matching config.
- `configs.json` — per-org configuration (colors, names, age ranges, referral logic).

## Adding a new org

1. Open `configs.json` and add a new entry. Use lowercase, hyphen-separated keys (e.g. `north-cascade`).
2. Commit and push. GitHub Pages redeploys in ~30 seconds.
3. Send the org their iframe URL pointing to `?org=their-key`.

Example minimal config:

```json
"north-cascade": {
  "name": "North Cascade Youth Soccer",
  "primary": "#7c2d12",
  "seasonEnd": 2027,
  "recMaxAge": 14,
  "selectClub": "Northwest United"
}
```

## Config field reference

| Field | Required | Notes |
| --- | --- | --- |
| `name` | yes | Full display name shown in warning text. |
| `primary` | yes | Brand hex color. Powers the title, focus ring, and result card. |
| `seasonEnd` | yes | Ending year of the season (2026-27 → `2027`). |
| `recMaxAge` | no | Highest U-age covered by recreational soccer. Triggers above-rec warning when paired with `selectClub`. |
| `selectClub` | no | Select club to recommend for ages above `recMaxAge`. |
| `recReferrals` | no | Map of age → referral org for gaps in the org's rec offerings. See below. |
| `minAge` / `maxAge` | no | Year dropdown range. Defaults to 6 / 19. |
| `footerNote` | no | Small grey text at the bottom of the calculator. |

### `recReferrals` example

If an org doesn't offer specific rec ages, list which ages and where to send those parents:

```json
"recReferrals": {
  "13": "Mount Vernon Youth Soccer",
  "14": "Mount Vernon Youth Soccer"
}
```

When a parent's child lands on one of those ages, the calculator shows: "U13 isn't offered at [org]. Mount Vernon Youth Soccer offers this age group..."

## Warning logic

The calculator shows at most one warning, prioritized as:

1. **Above rec age** — `ageNumber > recMaxAge` and `selectClub` is set → "Recreational soccer ends at U14. Players age U15 and older can try out at [selectClub]..."
2. **Rec gap** — `ageNumber` is a key in `recReferrals` → "U[age] isn't offered at [org]. [referralOrg] offers this age group..."
3. **No warning** — kid is in the org's normal rec range.

## How the age math works

US Soccer / Washington Youth Soccer use an August 1 cutoff:

- Born **August through December** → soccer year = birth year
- Born **January through July** → soccer year = birth year − 1
- Age group = `(seasonEnd − 1) − soccerBirthYear`

Example: Born March 2015, season 2026-27 → soccer year 2014 → 2026 − 2014 = U12.

## Embedding on Sports Connect sites

Sports Connect's rich text editor sanitizes raw `<iframe>` tags but allows the `[gccontent]` shortcode. Use this format in a Content Module:

```
[gccontent src="https://jacob-mcfarland.github.io/soccer-age-calc/?org=mvysa" style="border: none; outline: none;" title="Age Group Calculator" height="700px" width="100%" allowfullscreen="" frameborder="0" scrolling="yes" id="age-calc"] [/gccontent]
```

Just swap the `?org=` value per site.

## Yearly maintenance

Once a year (around July), find-and-replace `"seasonEnd": 2027` with `"seasonEnd": 2028` (and so on) in `configs.json`. One commit, all orgs updated. No edits needed on any client site.

## Local development

To preview locally before pushing:

```sh
python3 -m http.server 8765
```

Then visit `http://localhost:8765/?org=mvysa` in a browser.

## Hosting

Hosted on GitHub Pages from the `main` branch root. A `.nojekyll` file disables Jekyll processing so files are served as-is. HTTPS is enforced automatically.
