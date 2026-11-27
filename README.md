# tv-launcher themes

Official theme catalog for [tv-launcher](https://github.com/develoverli/tv-launcher), the ad-free,
tracking-free Android TV home screen. Themes are applied from
[adb-tv-studio](https://github.com/develoverli/adb-tv-studio): open a TV, go to **Themes** and pick
one from the catalog.

A theme is **data only**: a `theme.json` and, optionally, one background image. No code, no
scripts, no fonts. adb-tv-studio validates every theme against
[`schema/theme.schema.json`](schema/theme.schema.json) before it reaches a TV, and tv-launcher
clamps every value again when it reads it.

## Layout of this repo

```
index.json                 catalog read by adb-tv-studio
schema/theme.schema.json   the theme contract (JSON Schema, draft 2020-12)
themes/<id>/theme.json     one folder per theme, named after its id
themes/<id>/<image>        optional background image (png, jpg or webp)
```

## What a theme can change

| Field | Values |
|---|---|
| `layout` | `classic`, `rows`, `grid`, `minimal`, `sidebar` |
| `colors` | `background`, `surface`, `text` as `#RRGGBB`; `accent` as `#RRGGBB` or `auto` (the focused app's banner colour) |
| `background` | `type`: `solid`, `gradient` (`colors`, `angle`), `image` (`image`, `blur`), `photos` (the photos pushed to the TV); `veil` darkens images |
| `material` | `solid`, `glass`, `clear` (trays and pills) |
| `shape` | `square`, `rounded`, `round` |
| `focus` | `scale`, `lift`, `glow`, `outline` |
| `font` | `outfit`, `sans`, `serif`, `mono`, `condensed` (only fonts the launcher ships) |
| `motion` | `none`, `smooth`, `springy` |
| `labels` | `focus`, `always`, `never` |
| `clock` | `visible`, `size` (`small`, `normal`, `large`) |
| `columns` | 3 to 6 |
| `glow` | coloured light behind the dock |

Minimal example:

```json
{
  "$schema": "https://raw.githubusercontent.com/develoverli/tv-launcher-themes/main/schema/theme.schema.json",
  "schemaVersion": 1,
  "id": "my-theme",
  "name": "My theme",
  "author": "you",
  "version": "1.0.0",
  "layout": "grid",
  "colors": { "background": "#101012", "text": "#F5F5F7", "accent": "#3DD6D0" },
  "focus": "glow"
}
```

## Adding a theme

1. Create `themes/<id>/theme.json`. The folder name must equal `id` (lowercase letters, digits,
   dashes; `default` is reserved).
2. Optional: add one background image, at most 10 MB for the whole theme, and reference it from
   `background.image`.
3. Add an entry to `index.json` with `id`, `name`, `author`, `version`, `description`, `layout`,
   `path` (`themes/<id>`) and `files` (every file in the folder, `theme.json` included).
4. Open a pull request. Bump `version` whenever you change a published theme, so adb-tv-studio
   offers the update.

To try a theme before publishing it, zip the folder and import it in adb-tv-studio
(**Themes → Import .zip**).

## Applying a theme by hand

adb-tv-studio does this for you; for reference:

```bash
adb shell mkdir -p /sdcard/Android/data/lol.develover.tvlauncher/files/themes/my-theme
adb push theme.json /sdcard/Android/data/lol.develover.tvlauncher/files/themes/my-theme/theme.json
adb shell am start -n lol.develover.tvlauncher/.MainActivity --es lol.develover.tvlauncher.extra.THEME my-theme
```

`--es lol.develover.tvlauncher.extra.THEME default` goes back to the launcher's built-in look.
