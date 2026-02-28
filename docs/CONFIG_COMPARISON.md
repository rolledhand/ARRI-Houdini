# Config Comparison

## Exact Semantic Delta
`arri-CG.ocio` keeps the transform set, displays, looks, and LUT references from the original ARRI studio config. The practical changes are deliberately small and limited to the role assignments and file rules.

Role changes:

- `default` changes from `ARRI LogC4` to `ACEScg`
- `scene_linear` changes from `Linear ARRI Wide Gamut 4` to `ACEScg`
- `reference` is added and set to `ACEScg`
- `color_picking` changes from `Gamma 2.4 Rec.709 - Texture` to `sRGB - Texture`
- `texture_paint` changes from `Gamma 2.4 Rec.709 - Texture` to `sRGB - Texture`
- `matte_paint` changes from `ARRI LogC4` to `ACEScct`
- `color_timing` changes from `ARRI LogC4` to `ACEScct`
- `compositing_log` changes from `ARRI LogC4` to `ACEScct`

File rule changes:

- the original config has only one file rule:
  - `Default` -> `ACES2065-1`
- the CG config adds explicit pattern rules:
  - `*srgb_tx*` -> `sRGB - Texture`
  - `*srgb_texture*` -> `sRGB - Texture`
  - `*ACEScg*` -> `ACEScg`
- the CG config adds extension rules:
  - `.jpg`, `.jpeg`, `.png`, `.tif`, `.tiff` -> `sRGB - Texture`
  - `.exr` -> `ACEScg`
- the CG config changes the fallback:
  - `Default` -> `ACEScg`

Nothing else in the config was intentionally re-authored for this pass. The ARRI display transforms, view transforms, named transforms, and LUT references remain intact.

## Why the Original Is Awkward in Houdini
The original config is valid, but its defaults are not ideal for a CG-first Houdini workflow.

- Camera-log defaults are poor defaults for CG work. Setting `ARRI LogC4` as the default role makes the config feel camera-input-oriented rather than render/lighting-oriented.
- Missing file rules cause ambiguous texture and import behavior. Without extension or pattern rules, standard textures do not resolve to a practical working assumption for Houdini.
- The default file rule warning is undesirable. `ociocheck` warns that the original config’s default file rule (`ACES2065-1`) does not match its default role (`ARRI LogC4`), which is a bad signal for a production-facing default.

## Why the CG Version Is Better for Houdini
`arri-CG.ocio` changes the defaults to match how Houdini is typically used in a CG pipeline.

- `ACEScg` becomes the default working space, which is a practical default for shading, lighting, lookdev, and rendering.
- `ACEScct` is used for grading-style UI roles, which is a more sensible fit for interactive adjustments than `ARRI LogC4`.
- Texture assignment becomes predictable because common 8-bit texture formats resolve to `sRGB - Texture`.
- EXRs default to `ACEScg`, which aligns with common CG render-output expectations.

## Validation Impact
Both configs pass `ociocheck`, but the warning profile differs.

- `arri-CG.ocio` passes without the original default-rule/default-role mismatch warning.
- `arri-studio-combined-config-v1.0.0_aces-v1.3_ocio-v2.1.ocio` passes, but still warns that the default file rule maps to `ACES2065-1` while the default role is `ARRI LogC4`.
- Both configs share the warning that some color spaces do not have categories set.

## Support Position
For this repository:

- use [arri-CG.ocio](/Users/j7s/Documents/ARRI-Houdini/arri-CG.ocio) as the supported runtime config
- treat [arri-studio-combined-config-v1.0.0_aces-v1.3_ocio-v2.1.ocio](/Users/j7s/Documents/ARRI-Houdini/arri-studio-combined-config-v1.0.0_aces-v1.3_ocio-v2.1.ocio) as the upstream baseline reference only
