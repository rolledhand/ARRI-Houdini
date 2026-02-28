# Houdini Setup

## Point Houdini at the CG Config
Houdini should be pointed at the supported config, [arri-CG.ocio](/Users/j7s/Documents/ARRI-Houdini/arri-CG.ocio), via the `OCIO` environment variable.

Set `OCIO` to the absolute path of the config file in your shell, launcher, or Houdini package environment:

```bash
export OCIO="/path/to/ARRI-Houdini/arri-CG.ocio"
```

The exact mechanism depends on how you launch Houdini, but the key requirement is the same: `OCIO` must resolve to the full path of `arri-CG.ocio` before Houdini starts.

## Expected Behavior
With `arri-CG.ocio` active:

- EXR files default to `ACEScg`
- PNG, JPG, and TIFF-family textures default to `sRGB - Texture`
- filename tags can override the automatic rules:
  - `srgb_tx`
  - `srgb_texture`
  - `ACEScg`

Examples:

- `albedo_srgb_tx.png` resolves to `sRGB - Texture`
- `plates_srgb_texture.jpg` resolves to `sRGB - Texture`
- `volumeCache_ACEScg.exr` resolves to `ACEScg`

## Recommended Working Assumptions
- Treat `ACEScg` as the default working space for CG content.
- Leave automatic rules enabled for standard textures and render outputs.
- Override manually only when a source file is mis-tagged or comes from a non-standard pipeline.

## Troubleshooting
### Wrong-looking textures
If textures look washed out, over-saturated, or obviously double-transformed:

- confirm the file extension is one of the mapped formats
- check whether the filename accidentally contains a tag that forces a different rule
- verify the texture was not already pre-converted into another display-referred space

### Check the active config path
Before launching Houdini, confirm the `OCIO` variable points to the expected file:

```bash
echo "$OCIO"
```

The output should be the absolute path to `arri-CG.ocio`.

### Validate with `ociocheck`
If Houdini behaves unexpectedly, validate the config directly:

```bash
ociocheck --iconfig arri-CG.ocio
```

You can also compare against the upstream reference:

```bash
ociocheck --iconfig arri-studio-combined-config-v1.0.0_aces-v1.3_ocio-v2.1.ocio
```
