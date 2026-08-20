# ps-paper

Landing page behind the QR code on the conference poster for:

> **Discovery of RNA phosphorothioate modifications in anaerobic and thermophilic archaea:**
> *a dynamic backbone modification coupled to environmental conditions*
> Maman *et al.* — Accepted, *Cell* (2026)

Live at: **https://alexandermaclaude.github.io/ps-paper/**

Plain static HTML. No framework, no build step, no external assets — it loads
instantly on a phone over bad conference wifi.

## Contents

| File | Purpose |
|---|---|
| `index.html` | The landing page |
| `poster.pdf` | The **printed** poster (V16, as sent to the printer), 5.9 MB |
| `poster-lowres.pdf` | 90 dpi rasterised copy, 1.8 MB (3.2× smaller) |
| `qr/poster_qr.{svg,eps,pdf,png}` | Print-ready QR artwork, ECC Q |
| `robots.txt` | Embargo guard — blocks crawlers site-wide |

---

## ⚠️ The printed QR code cannot be changed

Once the poster is printed this URL is permanent. Therefore:

- **Never rename this repo** — `ps-paper` is baked into the printed URL.
- **Never rename or delete the `AlexanderMaClaude` account.**
- **Never disable GitHub Pages**, and keep the repo **public** (Pages from a
  private repo needs a paid plan).

A custom domain is the only thing that would make the printed QR portable. It
must be added *before* printing.

---

## On publication — you do nothing

The page asks **Crossref** on every visit whether the paper exists yet. Once it
does, the "Paper" row becomes a real link and the visitor is forwarded to it
after a short pause (with a "stay on this page" escape, so the poster downloads
stay reachable).

### How the match works, and why it is built this way

It fires only when **all** of these hold:

1. an author with family name `maman`, **and**
2. `phosphorothioate` in the title **or** ≥0.6 title-token similarity, **and**
3. publication year ≥ 2026.

Condition 2 exists because **there is an unrelated Yaakov Maman who also
publishes in *Cell*** (chromosome architecture, Nussenzweig lab). Surname alone
would risk redirecting your poster traffic to his paper. The keyword is the
primary key rather than the full title, so the match still fires if the title is
reworded during production — which matters, because the title has already
changed once.

### Knobs in `index.html`

```js
const REDIRECT = true;   // false = show the link but never forward
const DOI      = "";     // set the real DOI to bypass Crossref entirely
```

### Also on publication

- Delete `robots.txt`.
- Delete the `<meta name="robots" content="noindex">` line in `index.html`.

Both exist only to keep the in-press paper out of search engines.

---

## Before you print

- [ ] Confirm the **final title and author list** on the page match the accepted
      manuscript. Both were wrong in the first draft of this page.
- [ ] Contact email is the one you want (currently Gmail — a Weizmann address
      may suit a *Cell* poster better).
- [ ] Site returns HTTP 200 over **cellular data**, not just lab wifi.
- [ ] The **printed** QR scans on both an iPhone and an Android.
- [ ] The URL is printed as readable text under the QR as a fallback.
- [ ] Do **not** crop the QR's white quiet zone — it is functional.
- [ ] PI / journal sign-off that a public page naming the accepted paper is fine
      under Cell Press embargo policy.

## Note on timing

You present ~2 weeks before publication, so **at the poster session itself the
paper will not be out**. Everyone scanning during your talk lands on this page —
which is why the poster PDFs and the explanatory "Paper" row are the primary
experience, not an afterthought.

## Regenerating the low-res poster

```bash
conda activate gh_claude
python -c "
import fitz
d=fitz.open('poster.pdf'); o=fitz.open()
for p in d:
    pix=p.get_pixmap(dpi=90)
    o.new_page(width=p.rect.width,height=p.rect.height).insert_image(p.rect, stream=pix.tobytes('jpeg',jpg_quality=70))
o.save('poster-lowres.pdf', garbage=4, deflate=True)"
```

---

## Poster provenance

`poster.pdf` is the final printed artwork
(`Poster_Tara_AlexanderMaman_V16_ToPrint.pdf`, A0 841×1189 mm, 2026-08-20).

The QR code embedded in that printed file was decoded and confirmed to resolve
to `https://alexandermaclaude.github.io/ps-paper/` — i.e. **the poster on the
wall points at this page**.

The earlier draft is archived outside the repo at
`../ps-paper-archive/poster_draft_v1_20260817.pdf` (and remains in git history).

Note: the printed subtitle reads "coupled to environmental **conditions**"; the
draft said "sulfur metabolism". The landing page follows the printed version.
