# Favicon — Marysia Winkels

Design **A**: ink serif “M” with the terracotta full-stop from the wordmark, on warm paper.
Colours are the exact site tokens (`--paper`, `--ink`, `--accent`).

## Files
| File | Use |
|---|---|
| `favicon.svg` | Modern browsers — scalable, sharpest. |
| `favicon.ico` | Legacy / fallback. Bundles 16, 32, 48px. |
| `favicon-16x16.png`, `favicon-32x32.png` | Explicit PNG sizes (optional). |
| `apple-touch-icon.png` | 180×180, full-bleed — iOS home screen (iOS rounds it). |
| `favicon-512.png` | Large source / PWA manifest icon. |

## Drop-in
Put the files at your site root and add to `<head>`:

```html
<link rel="icon" href="/favicon.ico" sizes="any">
<link rel="icon" href="/favicon.svg" type="image/svg+xml">
<link rel="apple-touch-icon" href="/apple-touch-icon.png">
```

That’s all most setups need (the `.svg` is preferred where supported, `.ico` is the fallback).
If you want to be fully explicit, you can also add:

```html
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
```

## Note on the typeface
The PNG/ICO frames and the SVG both render the “M” in a serif. If your build pipeline can
embed **Newsreader** specifically (to match the site exactly), regenerate from `favicon-512.png`’s
source or outline the glyph; at 16–32px the difference is not visible.
```
