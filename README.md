# ARRI-Houdini

Houdini-ready OCIO config based on ARRI's studio config, using ARRI Reveal as the display transform.

## Status
Stable and ready to use. Fixes Houdini's messy OCIO issues so you don't have to. Works with external renderers like Arnold in Solaris as well. Loading this config in Nuke will transfer flawlessly.

## Quick Start
Create or edit `ocio.json` file in your Houdini `packages` folder and replace the path with your local path of the config:

```json
{
  "enable": true,
  "show": true,
  "load_package_once": true,
  "env": [
    { "OCIO": "${OCIO-/path/to/ARRI-Houdini/arri-CG.ocio}" }
  ]
}
```

Restart Houdini after adding or changing the `OCIO` setting or package file, since the config is read at startup.

With this config active, `ACEScg` is the intended default working space for CG content. Select whichever non-default display you prefer from correction toolbar & you've got the `ARRI AFL2 or Reveal` transform working.

The Houdini's `OCIO editor` is broken, this isn't an issue of this config, but tech-debt of SideFX which I can't fix. Any tweaks from there won't work, you have to manually edit the `arri-CG.ocio` config file if you desire any changes. Don't forget restarting Houdini if you start tweaking.

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

If you need to inspect image files directly, [OpenImageIO documentation](https://openimageio.readthedocs.io/en/stable/) is a good reference, and tools like `iinfo` and `oiiotool` are useful for checking metadata, channels, and file properties.

## Validation
`ociocheck` passes for both configs. The original config still has the extra default-rule/default-role warning, and `arri-CG.ocio` removes that specific warning.

## Official ARRI Resources
For official downloads and ARRI-provided color-management materials, start here:

- [ARRI Technical Downloads](https://www.arri.com/en/learn-help/learn-help-camera-system/technical-downloads)
- [ARRI Color Management Assets](https://arri.canto.de/v/ARRIColorManagement/landing?viewIndex=0)

## Related Projects
[chrisbrejon/ARRI-REVEAL-OCIO-Config](https://github.com/chrisbrejon/ARRI-REVEAL-OCIO-Config) is a broader ARRI Reveal community reference, while this repo is specifically focused on a Houdini-friendly CG-default workflow.
