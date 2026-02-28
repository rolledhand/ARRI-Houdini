# ARRI-Houdini

## Project Overview
ARRI-Houdini is a Houdini-focused OpenColorIO repository built around a functional CG-oriented variant of the ARRI studio config.

Current status: public beta / release candidate.

The supported config in this repository is `arri-CG.ocio`. It is the Houdini-ready variant intended for CG, lighting, lookdev, and rendering workflows.

The original file, `arri-studio-combined-config-v1.0.0_aces-v1.3_ocio-v2.1.ocio`, is included as an upstream reference artifact only. It is kept here so the provenance and baseline behavior are visible, but it is not the recommended runtime config for Houdini.

## Why This Exists for Houdini
The upstream ARRI-oriented config is camera-centric by default. That makes it awkward as a drop-in working config for Houdini, where a predictable CG default is more useful.

`arri-CG.ocio` keeps the same transform inventory and bundled LUT payloads, but changes the default workflow assumptions so Houdini behaves more like a typical ACES-based CG pipeline:

- `ACEScg` is the default working space.
- `ACEScct` is used for grading-style UI roles.
- common 8-bit textures resolve to `sRGB - Texture`.
- `.exr` resolves to `ACEScg`.

## What Changed From the Original Config
The CG config is a small, targeted derivative of the original file. The semantic changes are limited to the role assignments and file rules near the top of the config.

| Setting | Original config | CG config |
| --- | --- | --- |
| Default role | `ARRI LogC4` | `ACEScg` |
| Scene linear role | `Linear ARRI Wide Gamut 4` | `ACEScg` |
| Reference role | not set | `ACEScg` |
| UI color picking | `Gamma 2.4 Rec.709 - Texture` | `sRGB - Texture` |
| UI grading roles | `ARRI LogC4` | `ACEScct` |
| Default file rule | `ACES2065-1` | `ACEScg` |

Added file rules in `arri-CG.ocio`:

- `*srgb_tx*` -> `sRGB - Texture`
- `*srgb_texture*` -> `sRGB - Texture`
- `*ACEScg*` -> `ACEScg`
- `.jpg` -> `sRGB - Texture`
- `.jpeg` -> `sRGB - Texture`
- `.png` -> `sRGB - Texture`
- `.tif` -> `sRGB - Texture`
- `.tiff` -> `sRGB - Texture`
- `.exr` -> `ACEScg`

See [docs/CONFIG_COMPARISON.md](/Users/j7s/Documents/ARRI-Houdini/docs/CONFIG_COMPARISON.md) for the workflow-level comparison.

## Quick Start
1. Set the `OCIO` environment variable to the absolute path of [arri-CG.ocio](/Users/j7s/Documents/ARRI-Houdini/arri-CG.ocio).
2. Restart Houdini so it loads the new config.
3. Use `ACEScg` as the default working space unless you have a shot-specific reason to override it.
4. Let Houdini pick file rules automatically for standard textures and EXRs, then manually override only when a file is mis-tagged.

Example:

```bash
export OCIO="/path/to/ARRI-Houdini/arri-CG.ocio"
```

Detailed Houdini setup notes are in [docs/HOUDINI_SETUP.md](/Users/j7s/Documents/ARRI-Houdini/docs/HOUDINI_SETUP.md).

## Repo Structure
- [arri-CG.ocio](/Users/j7s/Documents/ARRI-Houdini/arri-CG.ocio): supported Houdini-ready CG config.
- [arri-studio-combined-config-v1.0.0_aces-v1.3_ocio-v2.1.ocio](/Users/j7s/Documents/ARRI-Houdini/arri-studio-combined-config-v1.0.0_aces-v1.3_ocio-v2.1.ocio): upstream baseline reference.
- [`luts/`](/Users/j7s/Documents/ARRI-Houdini/luts): bundled LUT assets required by the configs.
- [docs/CONFIG_COMPARISON.md](/Users/j7s/Documents/ARRI-Houdini/docs/CONFIG_COMPARISON.md): exact workflow delta between the two configs.
- [docs/HOUDINI_SETUP.md](/Users/j7s/Documents/ARRI-Houdini/docs/HOUDINI_SETUP.md): Houdini environment setup and troubleshooting.
- [THIRD_PARTY.md](/Users/j7s/Documents/ARRI-Houdini/THIRD_PARTY.md): provenance and rights caveats for bundled third-party material.

## Validation
Both configs currently pass `ociocheck`.

Recommended validation commands:

```bash
ociocheck --iconfig arri-CG.ocio
ociocheck --iconfig arri-studio-combined-config-v1.0.0_aces-v1.3_ocio-v2.1.ocio
```

Observed validation state:

- `arri-CG.ocio` passes validation and removes the default-rule/default-role mismatch warning present in the original config.
- `arri-studio-combined-config-v1.0.0_aces-v1.3_ocio-v2.1.ocio` also passes validation, but `ociocheck` warns that its default file rule resolves to `ACES2065-1`, which does not match its default role `ARRI LogC4`.
- Both configs still emit the shared warning that some color spaces do not have categories set.

## Legal / Provenance Notice
This repository includes maintainer-authored documentation plus third-party or vendor-derived config assets used to keep the Houdini workflow functional and reproducible.

A repository-wide license has not been selected yet. Do not assume that a general open-source license applies to the repository at this time.

This repo does not claim to re-license the bundled third-party files. See [THIRD_PARTY.md](/Users/j7s/Documents/ARRI-Houdini/THIRD_PARTY.md) for the current provenance and rights posture.

## Contribution Notes
Contributors should treat `arri-CG.ocio` as the supported config.

Before submitting changes:

- run `ociocheck --iconfig arri-CG.ocio`
- document any new LUT dependency in `THIRD_PARTY.md`
- update [docs/CONFIG_COMPARISON.md](/Users/j7s/Documents/ARRI-Houdini/docs/CONFIG_COMPARISON.md) and [README.md](/Users/j7s/Documents/ARRI-Houdini/README.md) if roles or file rules change

Additional contribution guidance is in [CONTRIBUTING.md](/Users/j7s/Documents/ARRI-Houdini/CONTRIBUTING.md).
