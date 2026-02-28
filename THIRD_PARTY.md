# Third-Party Material

## Included Third-Party Material
This repository currently includes the following third-party or vendor-derived material:

Reference config:

- [arri-studio-combined-config-v1.0.0_aces-v1.3_ocio-v2.1.ocio](/Users/j7s/Documents/ARRI-Houdini/arri-studio-combined-config-v1.0.0_aces-v1.3_ocio-v2.1.ocio)

Bundled LUT assets in [`luts/`](/Users/j7s/Documents/ARRI-Houdini/luts):

- `ARRI_LogC2Video_2020_davinci3d_33.cube`
- `ARRI_LogC2Video_709_davinci3d_33.cube`
- `ARRI_LogC2Video_P3D65_davinci3d_33.cube`
- `ARRI_LogC2Video_P3DCI_davinci3d_33.cube`
- `ARRI_LogC4-to-Gamma24_Rec2020-D65_v1-65.cube`
- `ARRI_LogC4-to-Gamma24_Rec709-D65_v1-65.cube`
- `ARRI_LogC4-to-Gamma26_P3-D65_v1-65.cube`
- `ARRI_LogC4-to-Gamma26_P3-DCI_v1-65.cube`
- `ARRI_LogC4-to-HLG-1K_P3-D65_DW100_v1-65.cube`
- `ARRI_LogC4-to-HLG_1K_Rec2100-D65_DW100_v1_65.cube`
- `ARRI_LogC4-to-HLG_1K_Rec2100-P3lim-D65_DW100_v1_65.cube`
- `ARRI_LogC4-to-St2084-1K_P3-D65_DW100_v1-65.cube`
- `ARRI_LogC4-to-St2084_1K_Rec2100-D65_DW100_v1_65.cube`
- `ARRI_LogC4-to-St2084_1K_Rec2100-P3lim-D65_DW100_v1_65.cube`
- `ARRI_P3D65HLG-1K-100_33.cube`
- `ARRI_P3D65PQ-1K-100_33.cube`
- `ARRI_Rec2100HLG-1K-100_33.cube`
- `ARRI_Rec2100PQ-1K-100_33.cube`
- `LogC3_EI800_to_Linear.cube`
- `LogC4_to_Linear.cube`

## Source and Purpose
These files are included because they are required to keep the Houdini-focused configuration functional and reproducible in its current form.

The supported CG config, [arri-CG.ocio](/Users/j7s/Documents/ARRI-Houdini/arri-CG.ocio), depends on the bundled LUT assets and is derived from the included reference config.

## Rights Status
Ownership of bundled vendor-derived assets remains with their respective owners.

Redistribution terms for these third-party files must be reviewed by the maintainer.

This repository does not claim to re-license any third-party config or LUT file included here.

## Maintainer-Owned Content
The Houdini-focused configuration changes in [arri-CG.ocio](/Users/j7s/Documents/ARRI-Houdini/arri-CG.ocio) and the repository documentation are the maintainer’s original contributions.

No repository-wide license has been selected yet, so no general license grant is being declared for maintainer-authored material in this file set at this time.

## If Redistribution Must Change Later
If the redistribution status of the bundled third-party material needs to change, this repository may be updated to remove those files and require users to source upstream assets separately.
