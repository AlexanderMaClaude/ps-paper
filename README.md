# ps-paper

Landing page behind the QR code on the conference poster for:

> **Discovery of natural RNA phosphorothioates and their writer machinery**
> Krayn *et al.*, *Cell*, in press (2026)

Live at: **https://alexandermaclaude.github.io/ps-paper/**

Plain static HTML. No framework, no build step, no external requests — it loads
instantly on a phone over bad conference wifi.

---

## ⚠️ The printed QR code cannot be changed

Once the poster is printed, this URL is permanent. Therefore:

- **Never rename this repo.** `ps-paper` is baked into the printed URL.
- **Never rename or delete the `AlexanderMaClaude` account.**
- **Never disable GitHub Pages** on this repo.
- Keep the repo **public** — Pages from a private repo requires a paid plan.

If you ever need to move this elsewhere, add a **custom domain** *before*
printing. A custom domain is the only thing that makes the printed QR portable.

---

## Before you print

- [ ] `poster.pdf` is committed to the repo root — otherwise the poster link 404s
      in front of everyone at the poster session.
- [ ] Contact email in `index.html` is the one you want (currently the Gmail —
      swap for your Weizmann address if you'd rather).
- [ ] Site returns HTTP 200 over **cellular data**, not just lab wifi.
- [ ] The **printed** QR scans with both an iPhone camera and an Android phone.
- [ ] The URL is also printed as readable text under the QR, as a fallback.
- [ ] PI / journal sign-off that a public page naming the paper as "Cell, in
      press" is fine under Cell Press embargo policy.

## On publication — you do nothing

The page asks **Crossref** on every visit whether the paper exists yet. The
moment it is published, visitors are redirected straight to it. No edit, no
deploy, no remembering. Two states, one URL:

| When | What a scan does |
|---|---|
| Now → publication | Shows this landing page (title, poster PDF, contact) |
| After publication | Redirects to `https://doi.org/<the paper>` |

The match is deliberately strict — it requires a ≥0.75 title-token match **and**
an author with family name `Krayn` — so it cannot send anyone to the wrong
paper. Any failure (offline, bad conference wifi, no match, 2.5 s timeout)
silently leaves the landing page up.

### Manual override

If Crossref is slow to index, or the final title drifted too far, paste the DOI
into `index.html` and it wins immediately:

```js
const DOI = "10.1016/j.cell.2026.XX.XXX";
```

### Also on publication

- Delete the `<meta name="robots" content="noindex">` line so the page can be
  indexed once there's no embargo left to respect.

## Notes

- **At the poster session itself the paper will not be out yet.** Everyone
  scanning during your talk lands on the landing page — so the poster PDF and
  contact line are the primary experience, not an afterthought. The redirect is
  for the long tail: people scanning from a photo weeks later.
- If the title changes materially before publication, update `TITLE` in
  `index.html` or just set `DOI` directly.
