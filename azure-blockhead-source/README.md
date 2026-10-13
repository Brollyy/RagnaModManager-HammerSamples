# Azure Blockhead sample

This sample provides original source art and a UE 4.27 project for following the [custom hammer authoring guide](https://github.com/Brollyy/RagnaModManager/blob/master/docs/custom-hammer-authoring-ue4-27.md).

## Source files

Run `python3 generate_source.py` to regenerate these editable files:

- `Source/SM_RMM_AzureBlockhead.obj` and its `.mtl`: low-poly hammer mesh with `M_Character`, `M_JaugeManche`, and `M_JaugeTete` slots in the same order used by stock meshes. The pivot is at the grip point.
- `Source/T_RMM_AzureBlockhead_BaseColor.png`: color atlas used by the cyan mesh section.
- `Source/T_RMM_AzureBlockhead_Icon.png`: separate changing-room icon.
- `Source/T_RMM_AzureBlockhead_Silhouette.png` and `Source/T_RMM_AzureBlockhead_Symbol.png`: optional selector art. The sample package leaves the game's `Silhouette` and `Symbol` fields at their stock defaults because custom marker materials have not been validated safely in-game.

## Build the sample package

1. Open `UEProject/RMMAzureBlockhead.uproject` in Unreal Engine 4.27 on Windows.
2. Run `UEProject/Scripts/create_hammer_materials.py` and then `UEProject/Scripts/reimport_azure_mesh.py` with UE's Python Script Plugin. These scripts import the source textures, create surface/accent materials, and assign them to their matching mesh slots. The body and handle/head materials define the game's `handleGauge` and `circularGauge` scalar parameters and blend visible surfaces toward a bright energized color as combo builds. The body scales and clamps the gauge values so early buildup is visible across a large surface. Emissive output adds a second response but is not the only indication.
3. Cook for `WindowsNoEditor` with the project's `PCD3D_SM5` target and shared material shader code enabled.
4. Run `build_rhammer.py` from `UEProject/Scripts/`, passing the cooked `Content` directory. The script checks shader metadata, packages cooked assets and sidecars, and includes the project's shader archive.

See the full guide for the Windows cook setup, manifest fields, RMM import/deploy steps, and game verification. The checked-in [`azure-blockhead.rhammer`](azure-blockhead.rhammer) includes the corrected cyan/gold/navy material assignments, mesh, icon, dependencies, and D3D shader archive.

## Verification status

The clean source-to-package build produced this archive SHA-256: `0faf890fcfdb2e5888f782606d4b308b4d3c6e34f4e9eae5df2da110d13b46d4`. UE 4.27 imported the mesh/material assets and the separate icon Texture2D, cooked 52 PCD3D_SM5 shader jobs with 0 errors, and `build_rhammer.py` validated all 26 cooked files and the shader archive. The manifest references resolve to 11 authored UE packages.

A prior Azure package build was verified in the changing room and during a fixture-song race with only Azure enabled. RagnaLoader recorded 33 hits out of 40 (28 normal, 5 perfect, 7 misses, one boost event), and the cyan body visibly shifted to bright yellow as combo built. The current package only changes the content path to `azure-blockhead` and fixes the source script so a clean project imports the icon; this exact repack has not yet been redeployed for a new runtime pass.

