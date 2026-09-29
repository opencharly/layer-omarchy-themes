# omarchy-themes

Selects one of Omarchy's bundled themes and renders it into every app config it
drives, as a charly layer.

The `omarchy-themes` candy installs **no packages**: all 22 themes ship inside
the `omarchy` package at `/usr/share/omarchy/themes`, and the templates that
project a theme into per-app configuration ship beside them at
`/usr/share/omarchy/default/themed`. This candy's whole job is to **apply** one,
by running `omarchy-theme-set` in its headless mode so the selection is baked
into the image rather than left to first login.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `omarchy-themes` |
| Requires | `layer-omarchy-base` (the foundation layer) |
| Packages | none (themes ship in `omarchy`) |
| Theme | `OMARCHY_THEME`, default `tokyo-night` |
| Service / port | none |

## Choosing a theme

`OMARCHY_THEME` selects which of the 22 bundled themes is baked in. Any bundled
name works, lowercased and hyphenated (`omarchy-theme-set` normalises
"Tokyo Night" to `tokyo-night`); `omarchy-theme-list` enumerates them.

The theme is applied with `THEME_HEADLESS=1` — `omarchy-theme-set`'s own headless
mode — which skips the wallpaper/background step (needs a running compositor) but
still renders the terminal, editor, bar, btop and Hyprland configs.

## How to use it

Compose the layer by pinning the member candy's sub-path in a desktop box's
`candy:` list:

```yaml
my-omarchy-desktop:
  candy:
    base: omarchy
    candy:
      - '@github.com/opencharly/layer-omarchy-base/candy/omarchy-base:v2026.242.0701'
      - '@github.com/opencharly/layer-omarchy-themes/candy/omarchy-themes:v2026.242.0837'
```

## Layout

- `charly.yml` — repo shape: the `discover:` rule that finds the member candy.
- `candy/omarchy-themes/charly.yml` — the candy entity (the `require:` dep, the
  `var:`/`env_accept:` for `OMARCHY_THEME`, the `plan:` `run:`/`check:` steps).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Foundation: `/charly-distros:omarchy-base`.
- Shell: `/charly-distros:omarchy-shell`.
- [`opencharly/opencharly](https://github.com/opencharly/opencharly) — the umbrella.
