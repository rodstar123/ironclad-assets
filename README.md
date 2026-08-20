# ironclad-assets

Card artwork for the IRONCLAD storefront (GoLDan Systems, Torn).

## What these are

Official Torn item artwork, **AI-upscaled 6x**. Torn publishes item art at
100x50 only (`large.png` is the biggest variant — `medium`/`small` are smaller,
`xlarge`/`webp`/`v2` paths 404, and yata.yt mirrors the same file byte for
byte). Stretched full-width across a Discord embed, 100px of source rendered as
a grey smear.

Each file is `items/<torn_item_id>.png` at **600x300**, built by:

1. fetch `https://www.torn.com/images/items/<id>/large.png` (100x50, palette)
2. depalettise to RGBA
3. Real-ESRGAN `realesrgan-x4plus-anime` x4 → 400x200
4. Real-ESRGAN `realesrgan-x4plus-anime` x4 → 1600x800
5. Lanczos **down** to 600x300

Detail is invented once at high resolution and then averaged away, rather than
stretched — that is why the downsample step exists.

`realesrgan-x4plus` (the photographic model) was tried first and rejected: on
flat game art it rings a white halo around the silhouette and smears invented
texture. A plain Lanczos control was also rendered — faithful, but uniformly
soft, which is the problem this repo exists to solve.

Pipeline: `tools/upscale_art.py` in the IRONCLAD ops repo.

## Honest caveats

- **Upscaled, not redrawn.** No detail here is authoritative — where the source
  was ambiguous, the model guessed.
- `63.png` (Minigun): the ammo box in the source carries text too small to
  resolve, and the upscaler rendered it as plausible-looking but **meaningless
  glyphs**. The silhouette is faithful; the lettering is not real text.

## Usage

Served over jsDelivr, pinned to a tag so a URL never changes under a live card:

```
https://cdn.jsdelivr.net/gh/rodstar123/ironclad-assets@v1/items/399.png
```

## Artwork ownership

Torn item artwork belongs to Torn (torn.com). These are derived copies used to
illustrate rental listings for Torn players. Not affiliated with or endorsed by
Torn.
