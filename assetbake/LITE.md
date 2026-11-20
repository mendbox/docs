# AssetBake Lite: user manual

*Manual for AssetBake Lite 1.0.1 (free Blender extension by Mendbox Studio). The full manual of AssetBake: https://mendboxstudio.com/docs/assetbake*

Free download: LITE_URL | Full version: SUPERHIVE_URL | GUMROAD_URL | Website: <https://mendboxstudio.com> | Support: support@mendboxstudio.com

---

## 1. What it does

AI services (Meshy, Tripo, Hunyuan3D, Rodin) make models with hundreds of thousands or millions of triangles. **AssetBake Lite** turns such a model into a small game asset with one button: it cleans the mesh, reduces it to a **triangle budget** (1,000, 2,000 or 5,000), makes new **UVs**, **bakes** the look of the original into 1k textures (normal map, base colour, ambient occlusion) and exports a **GLB** file. It runs in a hidden second Blender, so Blender stays usable. Your original is never changed, only hidden.

Lite is free and is the Game (triangles) mode with small limits. It is GPL-3.0-or-later.

| | **AssetBake Lite** (free) | **AssetBake** (full) |
|---|---|---|
| Mode | Game (triangles) | Game **and Quad-dominant** (quads on thick parts) |
| Triangle budget | 1,000 / 2,000 / **5,000 max** | 2,000 to 50,000 and **Custom** (200 to 1,000,000) |
| Texture size | **1k** | 1k, **2k, 4k** |
| Maps | Normal, Base Colour, AO | + **Roughness, Metallic, Emission, Alpha** |
| Scope | One asset for the selection | + **One asset per part** |
| Normal map export | OpenGL | + **DirectX** (Unreal) |
| Remove hidden surface, Select Source, Cancel, GLB export, Also save textures | Yes | Yes |

---

## 2. Requirements, install, update, uninstall

- Blender **4.2 LTS or newer** (4.2, 4.5, 5.x). Tested on **macOS and Linux** (Windows is covered by the same pure-Python code but has not been tested). Blender's own "glTF 2.0 format" add-on must be on (default).
- The extension is a **.zip**. **Do not unzip it.**

**Install:** Edit > Preferences (Mac `Cmd+,`) > **Get Extensions** > arrow menu at the top right > **Install from Disk...** > pick the zip > tick the extension if it is not ticked.

**Find the panel:** hover over the 3D Viewport, press `N`, click the tab **AssetBake Lite**. You see the panel and below it **Result**, and at the bottom a panel **Full version**. The panel works in **Object Mode**.

**Update / uninstall:** Edit > Preferences > Get Extensions > **Installed** > the arrow next to AssetBake Lite > **Uninstall**. (The Add-ons tab has no remove button for extensions.) Then install the new zip. Lite and the full AssetBake can be installed together.

---

## 3. Quick start (5 steps)

1. **Select your model** in the 3D Viewport (several parts: `A` selects all). The panel shows "Selected: N objects, X triangles".
2. Open the tab **AssetBake Lite**. Choose a **Triangle budget**: **1,000 triangles**, **2,000 triangles** or **5,000 triangles** (default 5,000). Textures are 1k. Leave Base Colour and AO ticked. Normal is always baked.
3. Click **Make Game Asset**. A progress bar and **Cancel** appear. Keep working if you like.
4. When done, the asset sits next to your original (which is hidden). Press `Z` > **Material Preview** and look at it. Read the Result box.
5. In the **Export** box click **Export GLB**, pick a folder and a name.

Undo: `Cmd+Z` (Mac) or `Ctrl+Z` removes the result in one step.

---

## 4. Reference

**Panel "AssetBake Lite 1.0.1"**

| Control | What it does | Default / options |
|---|---|---|
| **Triangle budget** | Triangles of the finished asset (within 5 %). | 5,000. Options: 1,000, 2,000, 5,000 |
| **Textures: 1k** | 1024 x 1024 pixels. The only size. | 1k |
| **Normal** | Always baked (the small bumps of the original). | Always on |
| **Base Colour** | The colour of the surface. | On |
| **AO** | Ambient occlusion (soft shadow in cracks), packed into the red channel of the ORM texture. The slowest map. | On |
| **Options: Remove hidden surface** | Surface that is shut in between parts and can never be seen is left out; its triangles go to the visible surface. The mesh is open there. | On |
| **Make Game Asset** | Starts the job. | |
| **Cancel** | Stops it at once. Nothing changes. | |
| **Select Source** | Brings back and selects the original objects of an asset, to make another version. | |

**Result box:** lines "A -> B triangles", "Shape: within X % of the size (largest gap Y %)", "Texture 1024 px: Normal, Base Colour, Ambient Occlusion", "N UV islands, M vertices in a game engine", "Time: N s". The shape number is the **deviation**: how far the asset is from the original, in percent of the model's size (under 0.5 % looks identical). The first message in the box is a one-line note about the full version: that is an advert, not a problem.

**Export box:** **Also save textures** (off by default) writes PNG files next to the GLB. The normal map is OpenGL (Blender, Unity, Godot, web). Export GLB writes the selected asset: scale applied, tangents, ORM texture (R = AO, G = roughness constant, B = metallic constant).

Lite bakes the three maps only. Roughness and metallic are not baked: the material uses one plain value for each.

**Full version panel:** lists what the full version adds and has a button **Get the full version**.

---

## 5. Common messages

| Message | Meaning and fix |
|---|---|
| "Select one or more mesh objects" | Select a mesh. |
| "Go to Object Mode to make an asset" | Press `Tab` to leave Edit Mode. |
| "Done - return to Object Mode to get the result" | The job finished while an object was in Edit/Sculpt/Paint Mode. Go back to Object Mode. |
| "Large input: this takes several minutes and several GB of memory." | Over 5,000,000 triangles. |
| "Too large: the limit is 8,000,000 triangles. Decimate the source first." | Decimate it in Blender (Modifiers > Decimate, apply), then run. |
| "Cancelled. Nothing was changed." | You pressed Cancel. |
| "The job failed: ..." with "Log file for support" and **Copy log path** | Send the log (`assetbake_lite_last_error.log` in your temp folder) to support. |
| "The source is rigged. The asset is built from the rest shape..." | The rig, weights and animation are not carried over. |
| "Transparency ..." | Lite does not bake alpha. |
| "Missed texels: N of M (filled from their neighbours)." | A few texels found no surface under them. Ignore small numbers. |
| "The budget is N triangles, the asset has M..." | The count could not reach the budget; the reason follows. |
| "Earlier versions of this asset were hidden: ..." | A new asset from the same source hides the earlier ones in the viewport. Click their eye icon in the Outliner to show them. |

More messages and every fix: the full manual (section 8).

---

## 6. Limits of Lite and known limitations

Budgets up to 5,000 triangles. 1k textures. No Quad-dominant mode, no per-part assets, no DirectX normal map, no roughness, metallic, emission or alpha maps. A rigged model becomes a static mesh. Sources up to 8,000,000 triangles. Tested on macOS only.

---

## 7. Get more: the full AssetBake

If you need more than Lite gives, the full **AssetBake** ($18 launch price, $24 regular) adds:

- **Quad-dominant mode**: quads on the thick parts, triangles on thin parts (macOS Apple Silicon, Windows x86-64 and Linux x86-64; Intel Mac not supported yet).
- Budgets up to **50,000** triangles and **Custom** numbers.
- **2k and 4k** textures.
- **Roughness, Metallic, Emission and Alpha** maps, packed as ORM.
- **One asset per part** for scenes and kits.
- **DirectX** normal maps for Unreal Engine.
- The complete manual with game engine guides (Unity, Unreal, Godot, web).

Get it: SUPERHIVE_URL | GUMROAD_URL

---

## 8. Support and changelog

Support: support@mendboxstudio.com (include Blender version, operating system, the Result text and the log path). Licence: GPL-3.0-or-later.

| Version | Change |
|---|---|
| 1.0.1 | Current release of AssetBake Lite (built from AssetBake 0.2.3). |
