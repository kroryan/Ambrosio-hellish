# Ambrosio Spinola: exclusive custom skin for Marta (Hellish Quart)

A fan-made skin that works only for Marta. It runs on Marta's native 68-joint skeleton (Mafra5/Marfa), with her original moves and rapier. The look follows Rubens' portrait of Ambrosio Spinola: black-and-gold field armour, millstone ruff, puffed red sleeves and trunk hose.

- **Armour is iron, not cloth.** The breast and back plates are one rigid piece on the chest (Spine1) and only articulate at the waist plate, where the tassets begin. On the front and back of the chest the clavicle and upper-arm weights are handed to the chest, so a twist of the spine or a raised arm never bends the plates.
- **Arms rebuilt along Marta's bones.** The source rig gave the top of the upper arm (pauldron and puffed red sleeve) to the clavicle and chest, so with the arms down they stayed in T-pose and stretched into flaps. Everything past the shoulder joint now belongs to the arm, with short blends at the shoulder and elbow; the hands keep their own weights.
- **Head, neck and ruff rebuilt.** The source rig puts its neck and head joints at the ruff and weights the head to the chest and shoulders. Those weights are rebuilt from the geometry: the head above the chin is rigid, the ruff is a rigid ring on the neck that rests on the gorget, and the neck skin blends smoothly from neck to head. The head turns inside the ruff without tearing the face or folding the ruff.
- **Same proportions as the source model.** Ambrosio's shoulders are higher than Marta's, so the whole model is scaled to her shoulder height (×0.968). The neck and torso are never stretched or squashed.
- **No ghost body when hit.** Marta's 17 wound overlay meshes (`DecalBloody`, `DecalSpine1`, ...) are shaped like her own body. The visibility controller keeps them hidden under the skin, together with her stock face.
- **Sharp textures within the game budget.** The 1M-triangle source is reduced to 80k triangles. UV seams are welded first, so the reduced mesh shows the texture without cracks. Each part stays under the 65k vertices the game's wound decal builder supports.

## Release

The zip contains only the four bundles (`skin_marta_ambrosiospinola`, `pic_marta_ambrosiospinola` and their `.manifest` files) and `README.txt`. There are no scripts. Install by copying the four files into `Hellish Quart_Data/StreamingAssets/`. `README.txt` also documents the optional manual first-person (FPP) adjustment.

## Layout

- `model/AmbrosioSpinola_Marta.blend`: the fitted result on Marta's skeleton, editable in Blender 5.3.
- `model/references/`: the four reference views.
- `model/selector_portrait.png`: the selector picture (Rubens' portrait, upscaled 3× with Lanczos).
- `model/AmbrosioSpinola_source.glb` is not in the repository because of its size (97 MB). Place it there to rebuild from scratch.
- `blender_work/`:
  - `step1_prepare.py` welds, decimates and cleans the source, transfers its rig weights and exports the textures;
  - `step3_rig_marta.py` scales the model, fits Marta's limbs and hands, makes the cuirass rigid, rebuilds the head, neck and ruff weights, carries the mesh into Marta's bind pose and splits off the rigid head, with QA renders;
  - `verify_skeleton.py` checks the 68 bones and the weights.
- `unity_project/`: Unity 2019.4.0f1 project (menu **AmbrosioSpinola → Construir mod**).
- `release/README.txt`: the English text shipped in the zip.

## Rebuild

Copy `Hellish Quart_Data/Managed/PlayMaker.dll` from your own installation to `unity_project/Assets/Plugins/`, then run:

```powershell
$blender="C:\Program Files\Blender Foundation\Blender 5.3\blender.exe"
$skel="unity_project\Assets\AmbrosioSpinolaMod\Editor\MartaSkeleton.json"
& $blender -b --factory-startup -P blender_work\step1_prepare.py -- model\AmbrosioSpinola_source.glb model\AmbrosioSpinola_source.glb build
& $blender -b --factory-startup -P blender_work\step3_rig_marta.py -- build $skel
& $blender -b --factory-startup -P blender_work\verify_skeleton.py -- build\marta\Skin.fbx $skel
.\build.ps1
```

`build.ps1` is a developer build script and is never part of the release. It runs the Unity batch build, copies the four bundles into `release/` and creates `AmbrosioSpinola_Marta.zip`.