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
items/614.png           Diamond Bladed Knife, the only row on item 614
items/399-112-97.png    ArmaLite M-15A4, Yellow 112.97%
items/399-141-07.png    ArmaLite M-15A4, Yellow 141.07%
```

A banner bakes in the row's tier colour and tier line, and those are per **row**,
not per item. IRONCLAD holds two ArmaLites on item 399 at different qualities;
one shared file would have put a grey `STANDARD · 64.72%` banner on the $325m
Yellow rifle.

**Once suffixed, always suffixed.** Sharing is judged over every row the
inventory holds, retired ones included — not over today's board. A stem is a
property of the item ID's history, because the file it names is already
published at a tag and already stored in a Notion Image URL, so letting a
retirement un-share an ID would rename a live row's asset out from under it.

`items/241.png` used to be this section's bare-stem example, and it is now the
counter-example. Item 241 (Bushmaster Carbon 15) carried one row, so its banner
was bare; that unit sold on 2026-09-03 and the file was cut at v12. A second
Bushmaster arrived on 2026-09-08 at a different quality and takes
`items/241-166-26.png` — the dead bare stem is **not** revived. It is an Orange
167.39% banner and the new row is Orange 166.26%: the tier colour happens to
match, which makes reviving it more tempting than usual and no less wrong,
because the banner also bakes in the tier line and that carries the quality
figure itself.

## What each tag carries

Displayable stock only. Assets for gear that has sold are **not** carried
forward — `146` (Yasukuni Sword), `655` (Riot Helmet) and `656` (Riot Body)
exist at `v1`/`v1.0.1` and stop there. A tag is the board as it stood, not a
superset of the one before it.

**v15 (2026-09-08)** — adds `241-166-26.png`, a second Bushmaster Carbon 15
(Orange 166.26%, 23% Powerful) on item 241. A PURE ADDITION with a twist: the
ID's earlier row sold at v12 and its bare `241.png` was cut then, so nothing
migrates and nothing renames — but the new file is suffixed from its first
second on the board anyway, because the Sold row is still in the inventory and
241 is now pinned in `EVER_SHARED_IDS`. Same shape as `395-186-85.png` at v13.
Eleven banners for eleven displayable rows; the ten from v14 byte-identical (git
reported no change on any of them). `241.png` stays dead at @v11.

*Tags v4 through v14 are not written up in this section — the file records v1,
v2, v3 and v15 only. The authority on what a tag carries is `git ls-tree <tag>`,
not this list.*

**v3 (2026-08-25)** — adds `399-6-24.png`, a third live ArmaLite M-15A4
(Standard 6.24%) on item 399. The other seven banners are byte-identical to v2:
a banner carries no price, so a buyout change cannot move one. Eight banners for
eight displayable rows.

**v2 (2026-08-25)** — the first authored-banner tag. Seven banners.

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
