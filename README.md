# ironclad-assets

Card artwork for the IRONCLAD storefront (GoLDan Systems, Torn).

## What these are (v2 onward)

**Authored 600x300 banners.** Torn publishes item art at 100x50 only
(`large.png` is the biggest variant — `medium`/`small` are smaller,
`xlarge`/`webp`/`v2` paths 404, and yata.yt mirrors the same file byte for
byte). Rather than pretend we have 600x300 of item art, each banner is 600x300
of _card_, with Torn's own sprite placed inside it at an exact integer scale:

1. fetch `https://www.torn.com/images/items/<id>/large.png` (100x50, palette)
2. depalettise to RGBA
3. enlarge **3x with NEAREST** → 300x150 — no interpolation, so no colour
   appears that was not in the source
4. compose onto a branded 600x300 canvas: 4px tier-coloured border, item name,
   tier line, wordmark

Nearest-neighbour pixel art at 3x is sharp and honest. The test suite asserts
the enlarge introduces **no new colours**, which is what catches somebody
swapping in an interpolating filter.

Builder: `cards/render_banner.py` in the IRONCLAD ops repo.
Spec: `BANNER-SPEC.md` there.

## Filenames

`items/<stem>.png`, where the stem is the Torn item ID — plus the row's quality
when two storefront rows share one item ID:

```
items/241.png           Bushmaster Carbon 15
items/399-64-72.png     ArmaLite M-15A4, Standard 64.72%
items/399-112-97.png    ArmaLite M-15A4, Yellow 112.97%
```

A banner bakes in the row's tier colour and tier line, and those are per **row**,
not per item. IRONCLAD holds two ArmaLites on item 399 at different qualities;
one shared file would have put a grey `STANDARD · 64.72%` banner on the $325m
Yellow rifle.

## What v2 carries

Displayable stock only. Assets for gear that has sold are **not** carried
forward — `146` (Yasukuni Sword), `655` (Riot Helmet) and `656` (Riot Body)
exist at `v1`/`v1.0.1` and stop there. v2 is the current board, not a superset.

## History

**v1 / v1.0.1 — Real-ESRGAN upscales, superseded.** The same sprite run through
`realesrgan-x4plus-anime` twice and Lanczos-downsampled to 600x300. It produced
a bigger file, never a sharper one: an anime-model upscale of a 100x50
anti-aliased PNG has no detail to recover, and full-width rendered that softness
at full size. Discord's media proxy was cleared of blame first — it served the
files byte-identical and pixel-identical to origin. The asset was the problem.

Those tags are left exactly as they are for anything still pointing at them.

Known artefact in v1: `63.png` (Minigun) — the ammo box carries source text too
small to resolve, and the upscaler rendered it as plausible-looking but
**meaningless glyphs**. v2 does not have that problem, because v2 does not
invent pixels.

## Usage

Served over jsDelivr, pinned to a tag so a URL never changes under a live card:

```
https://cdn.jsdelivr.net/gh/rodstar123/ironclad-assets@v2/items/399-112-97.png
```

Never point a live card at a branch.

## Artwork ownership

Torn item artwork belongs to Torn (torn.com). These are derived copies used to
illustrate rental listings for Torn players. Not affiliated with or endorsed by
Torn.
