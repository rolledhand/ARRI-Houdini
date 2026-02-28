# ARRI-Houdini

Houdini-ready OCIO config based on ARRI's studio config, using ARRI Reveal as the display transform.

## Status
Stable and ready to use.

## Quick Start
```bash
export OCIO=./arri-CG.ocio
```

Set `OCIO` to `arri-CG.ocio` before launching Houdini.
With this config active, `ACEScg` is the intended default working space for CG content.

## What This Changes
The original ARRI studio config is valid, but its defaults are camera-log oriented and its default file-rule behavior is not a good fit for a CG-first Houdini workflow.

`./arri-CG.ocio` keeps the same transform inventory and LUT payloads, but switches the defaults to a CG-friendly setup so texture loading and working-space behavior are predictable in Houdini.

| Role | Original config | CG config |
| --- | --- | --- |
| `default` | `ARRI LogC4` | `ACEScg` |
| `scene_linear` | `Linear ARRI Wide Gamut 4` | `ACEScg` |
| `reference` | not set | `ACEScg` |
| `compositing_log` | `ARRI LogC4` | `ACEScct` |
| `color_timing` | `ARRI LogC4` | `ACEScct` |
| `matte_paint` | `ARRI LogC4` | `ACEScct` |
| `texture_paint` | `Gamma 2.4 Rec.709 - Texture` | `sRGB - Texture` |

File rules in `./arri-CG.ocio`:

- `*srgb_tx*` -> `sRGB - Texture`
- `*srgb_texture*` -> `sRGB - Texture`
- `*ACEScg*` -> `ACEScg`
- `.jpg` -> `sRGB - Texture`
- `.jpeg` -> `sRGB - Texture`
- `.png` -> `sRGB - Texture`
- `.tif` -> `sRGB - Texture`
- `.tiff` -> `sRGB - Texture`
- `.exr` -> `ACEScg`
- fallback `Default` -> `ACEScg`
Tag rules take precedence over extension rules.

## Troubleshooting
### Wrong-Looking Textures
If a texture looks washed out, oversaturated, or double-transformed, check whether it is already display-referred, whether the filename includes `srgb_tx`, `srgb_texture`, or `ACEScg`, and whether the extension matches the intended automatic rule.

- `albedo.png` -> `sRGB - Texture` (extension rule)
- `albedo_srgb_tx.png` -> `sRGB - Texture` (tag rule, takes precedence)
- `lighting.exr` -> `ACEScg` (extension rule)
- `lighting_ACEScg.exr` -> `ACEScg` (tag rule)

### Validating With `ociocheck`
```bash
ociocheck --iconfig arri-CG.ocio
ociocheck --iconfig arri-studio-combined-config-v1.0.0_aces-v1.3_ocio-v2.1.ocio
```

## Contributing
- Treat `arri-CG.ocio` as the supported config.
- Run `ociocheck --iconfig arri-CG.ocio` before submitting changes.
- Document any new LUT dependencies in [THIRD_PARTY.md](./THIRD_PARTY.md).
- Update `README.md` if roles or file rules change.

## Validation
`ociocheck` passes for both configs. The original config still has the extra default-rule/default-role warning, and `arri-CG.ocio` removes that specific warning.

## Legal / Provenance
No repository-wide license has been chosen yet. Bundled ARRI-derived assets remain the property of their respective owners. See [THIRD_PARTY.md](./THIRD_PARTY.md) for details.

## Related Projects
[chrisbrejon/ARRI-REVEAL-OCIO-Config](https://github.com/chrisbrejon/ARRI-REVEAL-OCIO-Config) is a broader ARRI Reveal community reference, while this repo is specifically focused on a Houdini-friendly CG-default workflow.
