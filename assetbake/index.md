> The manual lives at https://mendboxstudio.com/docs/assetbake

# AssetBake 0.2.3: user manual

*Manual for AssetBake 0.2.3 (Blender extension by Mendbox Studio). Written from the add-on's own code: every label, default, range and message below is what the add-on really shows.*

Buy: [Superhive](SUPERHIVE_URL) | [Gumroad](GUMROAD_URL) | Free Lite version: [LITE_URL](LITE_URL) | Website: <https://mendboxstudio.com> | Trailer: TRAILER_URL

**Contents:** [1 What it does](#1-what-it-does) | [2 Install](#2-requirements-installation-update-and-uninstall) | [3 Quick start](#3-quick-start) | [4 Reference](#4-reference-every-panel-button-and-setting) | [5 The Result box](#5-understanding-the-result-box) | [6 Worked examples](#6-worked-examples) | [7 Game engines](#7-using-the-asset-in-game-engines) | [8 Troubleshooting](#8-troubleshooting) | [9 Limits, glossary, FAQ, support](#9-known-limitations-glossary-faq-and-support) (limitations, glossary, FAQ, support and changelog)

---

## 1. What it does

AI services such as Meshy, Tripo, Hunyuan3D and Rodin make 3D models that look finished but are not ready for a game. A typical file has hundreds of thousands, sometimes millions, of triangles. Its textures are often messy. A game engine (Unity, Unreal, Godot) or a web viewer would run slowly with it.

**AssetBake turns such a model into a game-ready asset with one button.** You select the model in Blender and press **Make Game Asset**. AssetBake then:

1. Cleans the mesh (merges doubled points, removes broken faces).
2. Reduces it to the **triangle budget** you chose (for example 10,000).
3. Unwraps it: it creates new **UVs** (the map that says which part of a texture goes where).
4. **Bakes** the look of the original onto the new mesh: it paints a new set of textures (normal map, base colour, a packed roughness/metallic/AO texture, and emission or alpha when the model has them).
5. Builds a Blender material that Blender's own glTF exporter understands.
6. Exports a **GLB** file (one file with mesh and textures inside) with one more button.

The work runs in a second, hidden copy of Blender. **Your Blender stays usable and your original model is never changed**: it is only hidden after the job, and one click (**Select Source**) brings it back.

> **[PLACEHOLDER: final marketing GIF `anim/webp/oneclick.webp` goes here: one click from a heavy AI model to a game asset]**

![The AssetBake panel after a finished job (interim capture, shows the former product name)](../assets/assetbake/panel_done_result.png)

*Interim picture: it was taken with an earlier build that was still called "AI Asset Maker". The real panel says "AssetBake". A new capture will replace it.*

**Two modes**

| Mode | What you get | Use it when |
|---|---|---|
| **Game (triangles)** | A mesh made only of triangles, as close as possible to your budget. Works on every model and keeps the shape best. | You want to put the model in a game, a web page or an AR app. This is the default. |
| **Quad-dominant** | A mesh made mostly of **quads** (four-sided faces) on the thick parts and triangles on the thin parts, in one closed mesh. If a model does not allow it, a Game asset is made instead and the Result box tells you why. | You want to keep modelling, sculpting or rigging the asset afterwards, where quads are easier to work with. Needs a budget of 5,000 faces or more. Not on Windows yet. |

> **[PLACEHOLDER: final marketing GIF `anim/webp/wire.webp` goes here: the triangle budget, the shape stays and the mess goes]**

**Who it is for**

- Indie and hobby game developers who generate props and characters with AI and need them in Unity, Unreal Engine or Godot.
- People who build web 3D (three.js, model-viewer) or AR scenes and need small, fast files.
- Blender users who tried Decimate and a manual bake once and want the whole chain in one click.
- Beginners: you do not need to know what UV unwrapping or baking is. Section 9 has a glossary.

**What it is not**

- It is not a re-modelling or sculpting tool. It reproduces the shape you give it within the budget.
- It does not keep a rig, weights or animation (section 6, example 4).
- It makes **one GLB file per asset**. No FBX and no engine presets.

---

## 2. Requirements, installation, update and uninstall

**Requirements**

- Blender **4.2 LTS or newer** (4.2 LTS, 4.5 LTS and 5.x). AssetBake is a Blender *extension*.
- **Tested on macOS only.** The automatic test suite passes on Blender 4.2.23, 4.5.14 and 5.2.2 on a Mac with Apple Silicon. Windows and Linux have not been tested yet. The code for them is written, but nobody has run it.
- **Quad-dominant mode** is built for **macOS (Apple Silicon)** and **Linux (x86-64)**. There is **no Windows build yet**: on Windows the Quad-dominant button is greyed out and **Game mode** does the work. (On a Mac with an Intel processor there is no build either.)
- Blender's own **"glTF 2.0 format" add-on** must be on (it is on by default). Export GLB needs it.
- About 2 to 8 GB of free memory for normal AI models (more for very big ones, see section 8).
- No internet connection and no other software. Everything is inside the zip.
- The extension is a **.zip** file. **Do not unzip it.**
- Permission the extension asks for: write temporary job files, read the result, export GLB files. Nothing else.

**Install from disk**

1. In Blender open **Edit > Preferences** (Mac: `Cmd+,`; Windows and Linux: use the menu).
2. Click **Get Extensions** in the list on the left.
3. Click the small arrow menu (a down arrow) in the top-right corner and choose **Install from Disk...**
4. Pick the `assetbake-0.2.3.zip` file and confirm. (Never unpack the zip yourself and copy the folder.)
5. The extension turns on. If its checkbox is not ticked, tick it. Close Preferences.

> **[PLACEHOLDER: screenshot of Edit > Preferences > Get Extensions > arrow menu > Install from Disk]**

*If your browser added a note to the file name, for example `assetbake-0.2.3 (1).zip`, that is fine.*

**Where is the panel?**

1. Move the mouse pointer over the **3D Viewport** (the big window with the 3D scene).
2. Press `N` to open the sidebar (the same key on Mac, Windows and Linux).
3. Click the tab named **AssetBake** (written vertically at the right edge).
4. You see two panels: **AssetBake 0.2.3** (settings and the Make Game Asset button) and **Result · AssetBake v0.2.3** below it. The version number in the title tells you which version is installed.

The panel works in **Object Mode**. In any other mode (Edit Mode, Sculpt Mode) the settings are greyed out and the panel says "Go to Object Mode to make an asset".

**Update to a new version**

1. Edit > Preferences > Get Extensions > **Installed** (this is the list of what you have).
2. Find **AssetBake**, click the small **arrow** next to it and choose **Uninstall**.
3. Use the arrow menu at the top right > **Install from Disk...** and pick the new zip.
4. Restart Blender if the version in the panel title does not change.

**Uninstall**

Edit > Preferences > Get Extensions > Installed > the arrow next to AssetBake > **Uninstall**.
The older **Add-ons** tab has no remove button for extensions, so use Get Extensions. An older install called **"AI Asset Maker"** (the former name of this product) is removed the same way. You can have the free **AssetBake Lite** and the full AssetBake installed at the same time: they have different names and different tabs.

**Lite and Full**

AssetBake Lite is free. It does Game mode with small limits and always shows a short note about the full version.

| | **AssetBake Lite** (free) | **AssetBake** (full, $18 launch price, $24 regular) |
|---|---|---|
| Modes | Game (triangles) only | Game and **Quad-dominant** |
| Triangle budget | 1,000, 2,000 or 5,000 (5,000 is the maximum) | 2,000, 5,000, 10,000, 20,000, 50,000 or **Custom** (200 to 1,000,000) |
| Texture size | 1k only | 1k, 2k, 4k |
| Maps | Normal, Base Colour, AO | Normal, Base Colour, Roughness, Metallic, Emission, Alpha, AO |
| Scope | One asset for the whole selection | One asset for the selection **or one asset per part** |
| Normal map for export | OpenGL | OpenGL or **DirectX** (Unreal) |
| Remove hidden surface, Select Source, Cancel, background job, GLB export, "Also save textures" | Yes | Yes |
| Where | See the Lite page | Superhive and Gumroad |

---

## 3. Quick start

You need a model. Any mesh object works: an AI model imported as `.glb`, `.fbx`, `.obj`, or something you made in Blender. In Blender import a GLB with **File > Import > glTF 2.0 (.glb/.gltf)**.

### 3.1 Game mode in 5 steps

1. **Select the model.** In the 3D Viewport click the model. If the model has several parts, select all of them: hover over the viewport and press `A` (select all). The panel shows "Selected: N objects, X triangles".
2. **Open the AssetBake tab** (`N`, then the tab **AssetBake**). Leave **Mode: Game (triangles)**. Pick a **Triangle budget**, for example **10,000**. Leave the other settings as they are (Textures **2k**, all maps ticked, Scope **One asset for the selection**, **Remove hidden surface** ticked).
3. **Click Make Game Asset.** A progress bar and a **Cancel** button appear. You can keep working in Blender. When the bar finishes, the new asset appears next to your original and the original is hidden.
4. **Look at the result.** Press `Z` (a pie menu opens) and choose **Material Preview**. Orbit around the model. Read the **Result** panel: it says how many triangles you started with and ended with, how close the shape is, and what the textures are. (Section 5 explains every line.)
5. **Export.** With the asset selected, scroll to the **Export** box in the Result panel. Leave **OpenGL** (use **DirectX** only for Unreal Engine). Click **Export GLB**, choose a folder and a name. You now have a `.glb` file for your engine (section 7).

> **[PLACEHOLDER: final marketing GIF `anim/webp/budget.webp` goes here: pick a budget, from mobile to hero asset]**

### 3.2 Quad-dominant mode in 5 steps

Do this on a **Mac with Apple Silicon or on Linux** (x86-64). On Windows the mode is not available yet.

1. **Select the model** as above. A model that is a solid object (a character, an animal, a crate) works best. Thin sheets (a leaf, a flag, sheet metal) cannot become quads.
2. In the AssetBake tab click **Quad-dominant** under **Mode**. The budget now counts **faces** (a quad is one face, a triangle is one face). Choose **10,000** or more. Below 5,000 the button is greyed out. At 5,000 you get a warning that the quad share drops.
3. **Read the line under the budget.** A moment after you select the model (and after every change of budget), the panel says "Estimating the quad share..." and then for example "Expect about 85-100 % quads". If it says "Mostly thin parts: will likely fall back to Game", the model is too thin; you will get a triangle asset.
4. **Click Make Game Asset.** (The button has the same name in both modes.) The progress text shows "Reading the wall thickness", "Building quads", "Checking the quad mesh".
5. **Check the Result box.** It shows faces, "Quads x %, triangles y % of the faces", "Thin parts (kept as triangles)" and "In a game engine: N triangles". Press `Z` > **Wireframe** to see the quads, then **Material Preview**. If Quad-dominant was not possible, the Result box says so and gives the reason (section 5). Export with **Export GLB** as in 3.1.

> **[PLACEHOLDER: final marketing GIF `anim/webp/maps.webp` goes here: the baked texture set]**

**Undo.** `Cmd+Z` (Mac) or `Ctrl+Z` (Windows, Linux) removes the whole result (asset and the hiding of the source) in one step.

---
## 4. Reference: every panel, button and setting

The sidebar tab **AssetBake** has two panels. All settings are saved in your .blend file (per scene). Defaults are the values you see after install.

![Panel with the Mode box (interim capture, shows the former product name)](../assets/assetbake/panel_quad_mode_v020.png)

*Interim picture from an earlier build ("AI Asset Maker"). The boxes and controls are the same; the title now says AssetBake.*

### 4.1 Panel "AssetBake 0.2.3" (settings), in the order you see it

#### Box "Mode"

| Control | What it does | When to use it | Default / options |
|---|---|---|---|
| **Game (triangles)** | All triangles, reduced to the budget. Works on every model and keeps the shape best. | Almost always. Game engines turn everything into triangles anyway. | **On** (default) |
| **Quad-dominant** | Quads where the model is thick enough, triangles on thin parts, in one closed mesh. When a model does not allow it, a Game asset is made instead and the Result box says why. | You want to edit, sculpt, rig or subdivide the asset later. | Off. Greyed out when there is no build for your system (Windows). |
| Hint under the buttons | In Quad-dominant: "Quads on thick parts, triangles on thin parts." When the mode is not available: "Quad-dominant is not available on Windows (x86-64) yet: the quad remesher has no build for this system. Game mode works." (the system name follows your computer). On a Blender with too old a Python: "Quad-dominant needs Python 3.11 or newer inside Blender. Game mode works." | | |

*Tip:* if a .blend that was saved with Quad-dominant on is opened on a system without the build, the next job makes a Game asset and says so in the Result box ("... A Game asset was made.").

#### Box "Triangle budget" (Game) or "Face budget (quads + triangles)" (Quad-dominant)

| Control | What it does | When to use it | Default / range |
|---|---|---|---|
| **Budget** (dropdown) | The size of the finished asset. In Game mode: **triangles**. In Quad-dominant: **faces** (quads and triangles together). | See the table below. | **10,000**. Presets: 2,000 / 5,000 / 10,000 / 20,000 / 50,000 / **Custom** |
| **Triangles / Faces** (number field, only when Custom) | Your own number. | When a preset does not fit your engine's limits. | 10,000. Minimum **200**, maximum **1,000,000** (the slider stops at 200,000, but you can type up to 1,000,000). |
| Red line "Quad-dominant needs at least 5,000 faces." | Quad-dominant is blocked below 5,000 faces; the Make button is greyed out. | Raise the budget or use Game mode. | |
| Yellow line "Below 10,000 faces the quad share drops and a fallback to Game is more likely." | The mode was tuned at 10,000 faces. | Use 10,000 or more when you want quads. | |

How the finished asset compares with the budget: in Game mode the count is within **5 %** of the budget (the tool aims for 1 %). If it cannot get there, the Result box says why (section 5). A **quad** is two triangles in a game engine, so a Quad-dominant asset has up to **twice as many triangles as faces**. The Result box shows both numbers ("In a game engine: N triangles").

Budget guide (rough, for one prop or character):

| Use | Triangles | Texture |
|---|---|---|
| Mobile game, background prop, web/AR | 2,000 to 5,000 | 1k |
| PC/console prop, indie game character | 10,000 to 20,000 | 2k |
| Hero asset, close-up, cinematic | 50,000 | 4k |

#### Box "Textures"

| Control | What it does | When to use it | Default / options |
|---|---|---|---|
| **1k / 2k / 4k** | Width and height of the baked textures: 1024, 2048 or 4096 pixels. | 1k for small or distant objects, 2k for most, 4k for close-ups. See "Choosing a texture size" in section 6. | **2k** |
| **Normal** (ticked label) | Always baked; it cannot be switched off. The normal map carries the small bumps of the original. | | Always on |
| **Base Colour** | The colour of the surface. | Leave on. | On |
| **Roughness** | How shiny or matte the surface is. Packed into the **green** channel of the ORM texture. | Leave on for realistic materials. | On |
| **Metallic** | Which parts are metal. Packed into the **blue** channel of the ORM texture. | Leave on. | On |
| **Emission** | Parts that glow. Baked **only when the source glows**. | Leave on. Nothing is baked if the model does not glow. | On |
| **Alpha** | Transparency. Baked **only when the source has real transparency**; stored in the alpha channel of the base colour texture, used as a cut-out (see section 6, example 5). | Leave on. | On |
| **AO** | Ambient occlusion: soft shadow in cracks and corners. Packed into the **red** channel of the ORM texture. **The slowest map.** | Untick to save time if your engine adds its own occlusion. | On |

What gets made, exactly:

- `<name>_normal` (normal map, OpenGL, tangent space).
- `<name>_basecolor` (colour; with transparency in its alpha channel when the model has some).
- `<name>_orm` (one texture: **R** = AO, **G** = roughness, **B** = metallic). A channel whose map you switched off is filled with one constant value taken from the source (AO: white).
- `<name>_emission` (only when the model glows).
- All are 8-bit **PNG** and are packed inside your .blend file.
- When a map is not baked (box unticked, or the source has nothing to bake), the material uses one plain value instead. Example: Base Colour unticked gives the model's average colour.
- **AO detail:** above 1k the AO map is baked at half the size and enlarged (it is a soft map). AO is the slowest map; at 2k it is the largest part of the bake time.

#### Box "Scope"

| Control | What it does | When to use it | Default / options |
|---|---|---|---|
| **Scope** (dropdown) | **One asset for the selection**: everything you selected becomes one asset. **One asset per part**: every separate part becomes its own asset with its own textures and the full budget. A **part** is what stays connected after doubled points are merged, **not** a Blender object. | One asset: one character, one prop. Per part: a scene of several chairs, a kit of separate props that came in one file. | **One asset for the selection** |
| **Remove hidden surface** | Surface that is shut in between parts and can never be seen is left out. Its triangles and texture space go to the visible surface. The mesh is open there. | Leave on for solid-looking props. Untick if you will cut the model open, show its inside, or the result shows holes. | **On** |

Notes on Scope:

- With **one asset per part**, tiny loose pieces (eyes, buttons, debris) do not become assets: each joins the nearest large part. A part counts as large when its size is at least **30 %** of the largest part.
- More than **24** large parts, or more than **2,000** loose pieces: the split is refused, one asset is made for the whole selection, and a warning says so.
- Assets are named `<name>_asset_01`, `<name>_asset_02` ... largest first. Each gets its origin at the bottom centre of the part.
- The hidden-surface step looks at the surface with test rays from many directions. It needs at least 0.5 % of the area to be hidden before it removes anything. It is **switched off when the source has transparency** (glass hides nothing). In **Quad-dominant** mode it is not used.

#### Below the boxes: messages, progress, Make Game Asset

| Item | What it shows | Meaning |
|---|---|---|
| "Select one or more mesh objects" | Nothing usable is selected. | Select a mesh. Curves, empties, cameras and lights are ignored. Assets made by AssetBake are never used as a source. |
| "This is an asset made by this add-on, it is not used as a source. To make another version, select the original objects:" and the **Select Source** button | You selected an asset that AssetBake made. | Click **Select Source** to bring back and select the originals (see 4.3). |
| "Selected: N object(s), X triangles" | Your selection. | The triangle count is the count of the original (a quad counts as two triangles). |
| "Large input: this takes several minutes and several GB of memory." | More than **5,000,000** triangles. | It works, but slowly. |
| "Too large: the limit is 8,000,000 triangles. Decimate the source first." | More than **8,000,000** triangles. The Make button is greyed out. | Reduce the source in Blender first (Modifiers > Decimate, then apply). |
| Quad-dominant outlook lines | See 4.4. | |
| **Make Game Asset** (big button) | Starts the job. Same name in both modes. Greyed out when nothing valid is selected, over 8,000,000 triangles, or Quad-dominant with fewer than 5,000 faces. | |
| Progress bar and **Cancel** | While a job runs: a bar with a text such as "Building the low mesh". **Cancel** stops the job at once. | Nothing in your scene changes. |
| "Done - return to Object Mode to get the result" | The job finished while an object was in Edit, Sculpt or Paint Mode. | Switch every object back to Object Mode (`Tab` leaves Edit Mode). The result then appears. |
| "Go to Object Mode to make an asset" | You are not in Object Mode. | `Tab` out of Edit Mode. |

The progress text goes through these stages: Reading the selection, Cleaning the source (N triangles), Splitting into parts (only with one asset per part), Building the low mesh, Reducing the source, Looking for hidden surface, Decimating to the budget, (Quad-dominant: Reading the wall thickness, Building quads, Building quads, second attempt, Checking the quad mesh), Measuring the shape, Unwrapping UVs, Baking (with the map names, and "second pass for N missed texels"), Packing the textures, Saving the result. With several assets the text starts with "Asset 2 of 5:".

### 4.2 Panel "Result · AssetBake v0.2.3"

| What you see | Meaning |
|---|---|
| "No result yet" | No job has run in this scene. |
| "Working in the background..." / "Finished, waiting for Object Mode" | A job runs / is finished and waits for Object Mode. |
| A red message | The job failed or was refused. Section 8. If a log file exists you also see "Log file for support: ..." and the button **Copy log path**. |
| A blue message "Cancelled. Nothing was changed." | You pressed Cancel. |
| One box per asset with its name and lines | A finished asset. Section 5 explains every line and message. |
| "The source is hidden, not changed. Total time: N s." | Your original objects are hidden (viewport and render), never modified. |
| "For another version (other budget or textures), get the source back:" and **Select Source** | Same button as in the main panel. |

The result is saved with your .blend file.

**Box "Export"** (only in Object Mode)

| Control | What it does | When to use it | Default / options |
|---|---|---|---|
| **OpenGL / DirectX** (Normal map) | The convention of the normal map in the exported file. **OpenGL** = green channel up (Blender, Unity, Godot, glTF, three.js). **DirectX** = green channel down: the exported normal map gets its green channel flipped (Unreal Engine). | Choose **DirectX** only if your engine wants it. If you do not know, keep OpenGL. | **OpenGL** |
| **Also save textures** | Besides the GLB, writes the textures as PNG files next to it: `<file>_basecolor.png`, `_orm.png`, `_normal.png`, `_emission.png`. | When you want the textures for editing, or for an engine that does not read GLB textures. The GLB always contains the textures too. | Off |
| **Export GLB** | Saves the selected asset as a GLB file. | | Greyed out when no asset is selected and no asset of the last run exists. |

How Export GLB works:

- It exports the **selected AssetBake asset(s)**. If none is selected, it uses the assets of the last run that are still in the file.
- With **several assets** selected, every asset gets its own file: `<typed name>_<asset name>.glb`. With one asset, the file has exactly the name you typed.
- The file holds the triangles that the textures were baked on. A quad-dominant asset is split exactly as in the bake (every quad along the same diagonal), so the normal map fits.
- The object **scale is applied**, so the file carries no node scale. Tangents are written. The Y-up axis of glTF is used.
- Occlusion, roughness and metallic travel as one texture. Transparency is exported as a cut-out (glTF alphaMode MASK). An emission strength above 1 is exported with the glTF emissive-strength extension.
- The export works on a temporary copy. Your scene is left as it was.
- If you **edited a quad-dominant asset** after it was made (for example triangulated a face), the report line says "... was edited after it was made ..." (section 8). The file is still written.

### 4.3 The Select Source button

**Select Source** (in the main panel when you selected an asset, and in the Result box) shows and selects the original objects an asset was made from, so you can make another version with other settings. The asset stays where it is. It is one undo step.

- Every asset remembers its source objects, also after you save and reopen the .blend (then by object name: a source renamed after reopening is reported as not found).
- It also restores the render visibility of the originals as it was before the job.
- Messages it can give are in section 8.

When you make a **new version** from the same source, the earlier assets made from that source (still at their place) are hidden in the viewport, and the Result box says "Earlier versions of this asset were hidden: ...". They are hidden only in the viewport: they still show in a render. Click their eye icon in the Outliner to show them again.

**Asset names.** The name is taken from your objects: the first name that says something, walking from the selected object up through its parents. Names that importers add (`Sketchfab_model`, `RootNode`, `Object_10`, `Mesh_0`, `tripo_node_<id>`, hashes) are skipped. A file name kept as a node name gives its stem (`Wild_Pig_0130023416_texture.fbx` becomes `Wild_Pig_asset`). Then the main material name, then the mesh name, and at last `Asset`. At most 40 characters. The asset is placed in the same collection as your source.

### 4.4 Quad-dominant outlook lines (before you click)

When Quad-dominant is on, the panel tells you what to expect, before the wait.

| Line | Meaning | What to do |
|---|---|---|
| "Estimating the quad share..." | A quick look at the wall thickness of your selection is running (it takes a fraction of a second). | Wait. |
| "Expect about 80-100 % quads" (numbers vary) | The expected share of quads among the faces. | Fine. The real result is usually inside this range. |
| "Expect about 60-85 % quads, or a fallback to Game: many thin parts" | Many thin parts. The mode might fall back to a triangle asset. | Raise the budget or accept a fallback. |
| "Mostly thin parts: will likely fall back to Game" | Most of the surface is thinner than one quad. | Use Game mode, or expect a Game asset. |
| "The source is under the budget: nothing to reduce, it is kept as it is." | Your model has fewer triangles than the budget. | Nothing to do: it is kept (with new UVs and textures). |
| "Over 2,000,000 triangles: Quad-dominant is not available, a Game asset will be made. To get quads, decimate the source in Blender first (Modifiers > Decimate, below 2,000,000 triangles), apply it and run Quad-dominant." | The source is too big for the mode. | Decimate first, or accept a Game asset. |
| "One asset per part: every part is checked on its own and gets the full budget, so no quad share is estimated." | In per-part scope no estimate is made. | |

The estimate only looks at thin parts. It cannot foresee a fallback for the other reasons of section 5.

---

## 5. Understanding the Result box

After a job, the Result panel shows one box per asset. Example of a Game asset:

```
Wild_Pig_asset
145,859 -> 9,999 triangles
Shape: within 0.03 % of the size (largest gap 0.07 %)
Texture 2048 px: Normal, Base Colour, Roughness, Metallic, Ambient Occlusion
36 UV islands, 6,297 vertices in a game engine
Time: 10 s
(i) Missed texels: 21 of 2,484,879 (filled from their neighbours).
```

(The numbers are from our test run on a boar model; yours will differ.)

![Result box (interim capture)](../assets/assetbake/second_version_v020.png)

### 5.1 The lines

| Line | What it means | What is good |
|---|---|---|
| **A -> B triangles** (Game) | A = triangles of your source, B = triangles of the asset. | B close to your budget (within 5 %). |
| **A triangles -> B faces** (Quad-dominant) | B = faces (quads + triangles). | B close to your face budget. |
| **Quads x %, triangles y % of the faces** (Quad-dominant) | The share of four-sided faces and three-sided faces. | The higher the quad share the better. 80 % or more is a clean result; thin or complex models give less (45 % to 75 % is common). |
| **Thin parts (kept as triangles): z % of the surface** (Quad-dominant) | The part of the surface thinner than about one quad edge. It is built from triangles. | Low. Above 50 % the mode gives up and makes a Game asset. |
| **In a game engine: N triangles** (Quad-dominant) | Your engine splits every quad into two triangles. This is the real triangle count. | |
| **Shape: within X % of the size (largest gap Y %)** | **Deviation.** How far the asset's surface is from the original, measured two ways (original to asset, and asset to original). **X**: 99 out of 100 measuring points are closer than this. **Y**: the worst point. Both are percent of the **model's size** (the diagonal of its bounding box). | X under 0.5 % looks identical. 0.1 % of a 2 m character is 2 mm. The Result box warns above 0.5 %. |
| **Texture N px: Normal, ...** | Size and list of baked maps ("Ambient Occlusion" is the AO map). | The list matches your boxes, minus maps the source does not have. |
| **N UV islands, M vertices in a game engine** | **UV islands** are the pieces the surface was cut into to be laid flat on the texture. **Vertices in a game engine**: a game engine duplicates a vertex wherever the UV is cut, so this is the vertex count your engine really holds. | Few islands waste less texture space; very high counts are normal for complex models (hundreds). |
| **Time: N s** | Seconds for this asset. | |

### 5.2 The messages (blue = information, yellow = warning)

These appear in the Result box above the asset (for the whole job) or inside the asset's box.

**Messages about the job (source and scope)**

| Message | Meaning | What to do |
|---|---|---|
| "The source is rigged. The asset is built from the rest shape: the armature, the weights and the animation are not carried over." | Your model has an armature (a skeleton) or animation. | See section 6, example 4. |
| "The source has shape keys: the basis shape is used, the shape keys are dropped." | Shape keys (morph targets) are lost. | Apply the shape you need first. |
| "Modifiers of the source were applied for the asset (the source is not changed)." | Mirror, Subdivision and other visible modifiers are part of what you see, so they were applied to a copy. | Information. |
| "Large input: N triangles. This takes several minutes and several GB of memory." | Over 5,000,000 triangles. | Be patient or decimate first. |
| "N material(s) have no Principled BSDF or Emission shader on the surface (a shader inside a node group is not read) and were baked as plain grey: ..." | A material AssetBake cannot read. | Rebuild it with a Principled BSDF (not inside a node group) and run again. |
| "Faces without a readable material were baked from the mesh's vertex colours." | The mesh has vertex colours and an unreadable or missing material. | Information. |
| "Unlit material(s) (Emission shader): the emitted colour was baked as the base colour: ..." | A glTF file without lighting (an Emission shader). | Information. |
| "Texture file not found: ..." | The source material points to an image file that is missing. | Find the file (File > External Data > Find Missing Files) and run again. |
| "Transparency is baked as a cut-out: half-transparent surface such as glass becomes solid or a hole." | Alpha is on or off per texel, not soft. | Section 6, example 5. |
| "The source has transparent parts, so surface behind them was kept." | Hidden-surface removal was skipped because of the transparency. | Information. |
| "The selection has too many separate parts for one asset each: one asset was built for the whole selection." | More than 24 large parts. | Select fewer objects, or use one asset. |
| "The selection falls into more than 2,000 loose pieces: one asset was built for the whole selection." | Too many loose pieces. | Merge or delete loose pieces first. |
| "N small loose piece(s) were kept with the nearest large part." | Tiny parts joined a big one (one asset per part). | Information. |
| "Earlier versions of this asset were hidden: ..." | See 4.3. | |
| Warning "Texture with unsaved changes: ... Save or pack it, otherwise the bake uses the old picture." | A source image was painted in Blender and not saved. | Save or pack the image, then run again. |
| "Quad-dominant is not available on ... yet ... A Game asset was made." | The file asked for Quad-dominant, this system cannot. | Information. |

**Messages about one asset**

| Message | Meaning | What to do |
|---|---|---|
| "The source already has fewer triangles than the budget: the geometry is kept as it is." | Under budget. | Information. |
| "N % of the surface is enclosed by other surface (no test ray reaches it from outside) and was removed: the mesh is open there. Untick "Remove hidden surface" to keep it." | The hidden-surface step removed N % of the area. | Fine for most uses. Untick the option if you see holes you do not want. |
| "Parts of the source are tangled (faces that share one edge with several neighbours or touch at single points) and cannot be reduced the normal way. To keep the budget their points were merged by distance: these places can look rougher than the rest." | The mesh has non-manifold geometry that the reducer cannot collapse. | Information. Try a higher budget if the rough places bother you. |
| "The budget is N triangles, the asset has M. The mesh could not be reduced further: what is left are tangled or separate pieces that fall apart when they are merged more." | Game mode could not reach the budget from above (M is more than 5 % over). | Raise the budget or clean the source. |
| "The budget is N triangles, the asset has only M: that is all the visible surface kept after the hidden surface was removed. Untick "Remove hidden surface" to get closer to the budget." | Fewer triangles than the budget because hidden surface was removed. | Untick **Remove hidden surface**. |
| "The budget is N triangles, the asset has only M: the source has no more usable triangles after cleaning." | The cleaned source is simply smaller than the budget. | Lower the budget or ignore. |
| "The low mesh leaves the source shape by X % of the size. Try a higher budget." | Deviation is above 0.5 %. | Raise the budget. |
| "The bake found no source surface: the textures are empty." | Nothing was baked (very unusual). | See section 8, black patches. |
| "Missed texels: N of M (filled from their neighbours)." | **Texels** are the pixels of the texture. A few texels found no surface of the original under them; their colour was copied from neighbours. Warning only above 0.5 %. | Ignore small numbers (hundreds of a few million). If it is a warning, look for gaps in the model. |
| "N % of the texture was baked from the back of a surface. This happens on one-sided sheets and where parts almost touch." | Warning above 2 %. | Check thin sheets and touching parts in Material Preview. |
| "N very small triangles got no texture area." | Tiny triangles that cover no pixels. | Information. |

**Quad-dominant messages** are in 5.3.

### 5.3 Quad-dominant: the share lines and the fallback reasons

When the quad mesh was kept, you may see:

- "Thin parts (thinner than about one quad) and the seams next to them are triangles; everything else is quads. A higher budget gives a higher quad share."
- "Below 10,000 faces the quad share drops. The mode was tuned at 10,000."
- Warning "The budget is N faces, the asset has M: the remesher did not get closer in three passes." (more than 5 % away from the budget).
- "The textures were baked on a fixed split of every quad; Export GLB writes exactly those triangles."

**When Quad-dominant was not possible**, the Result box shows two lines:

> Warning: "Quad-dominant was not possible here, a Game asset (triangles) was made instead. Reason: ..."
> Info: "The quad mesh that was set aside: quads x %, triangles y % of the faces, thin parts z % of the surface." (or "Thin parts: z % of the surface.")

The asset itself is a normal, good Game asset. The mode first tries the quad mesh and **checks** it strictly. If the check fails, it tries **one more time** with a different setting. Only when both fail you get the fallback. You never get a half-repaired quad mesh. Every possible reason:

| Reason text | What it means | What to do |
|---|---|---|
| "the quad remesher has no build for this system" | There is no compiled quad remesher for your computer (for example Windows). | Use Game mode. |
| "the quad remesher could not be loaded on this system (...)" | The bundled remesher is there but would not start. Often a macOS security block. | Section 8: "macOS blocked a library". |
| "the quad remesher stopped with an error (Name: text)" | An unexpected error inside the quad step. | Run again; if it repeats, send the model to support. |
| "the source already has fewer triangles than the budget, so there is nothing to remesh" | The source has fewer triangles than the budget. | Nothing to reduce; use a lower budget or Game mode. |
| "the source has more than 2,000,000 triangles, the limit of this mode. To get quads, decimate the source in Blender first (Modifiers > Decimate, down to under 2,000,000 triangles), apply the modifier and run Quad-dominant again" | Too big. (In our tests a 3.9 million triangle model also took more than 3 minutes and flipped to Game; about 1 million is a good size.) | Decimate to under about 1,000,000 triangles. |
| "N % of the surface is thinner than one quad at this budget, so the result would be mostly triangles" | More than half of the surface is thin. | Use Game mode, or a much higher budget. |
| "the seam between the quads and the thin parts did not stay locked (X of Y seam vertices moved or lost)" | The join between quads and thin triangles broke. | Try a different budget (for example 10,000 instead of 12,000). |
| "the quad mesh fell into more pieces than the source (A instead of B)" | The remesher tore a part off. | Try another budget. |
| "the quad mesh has N open edges where the source is closed" | New holes. | Try another budget. |
| "the quad mesh has N edges shared by more than two faces" | Non-manifold edges. | Try another budget. |
| "X % of the source surface is not covered by the quad mesh (the limit is 0.5 %)" | The quad mesh misses part of the model. | Try another budget. |
| "X % of the quad mesh lies away from the source surface (the limit is 0.5 %)" | Stray faces floating off the model. | Try another budget. |
| "X % of the quad mesh is a collapsed double layer (the limit is 0.5 %)" | Two layers (front and back of a thin wall) fused. | Try a higher budget, or use Game mode. |
| "one spot of the quad mesh is X quad edges away from the source (the limit is 3)" | One bad spot. | Try another budget. |
| "the quad clean-up did not settle in N rounds" / "the seam repair did not settle in N rounds" | A repair loop did not finish. | Try another budget. |
| "the quad remesher returned an empty mesh" | Nothing came back. | Try another budget. |
| "N faces of the quad mesh lie on top of each other" | Doubled faces. | Try another budget. |
| "the quad mesh has faces that Blender does not accept" | Unusable faces. | Try another budget. |

Several reasons can be joined with ";". The reason quoted is that of the first try. About one thick model in eight still falls back for one of the small local defects above. A change of the budget by a few thousand usually changes the outcome. The result can also differ between Blender 4.x and 5.x, because Blender's own mesh tools differ.

---
## 6. Worked examples

The numbers below come from our own test runs on real AI models. Yours will differ a little. Times are from an Apple-silicon Mac with nothing else running.

### Example 1: A Meshy model to a 10,000 triangle asset

You have a character or animal from Meshy, exported as a `.glb`.

1. File > New > General. Delete the default cube (hover the viewport, `A`, then `X` > Delete).
2. File > Import > **glTF 2.0 (.glb/.gltf)** and pick your file. A Meshy file is one mesh with a base colour texture (or a full PBR set).
3. Hover the viewport and press `A` to select everything. Open the **AssetBake** tab. The panel shows, for example, "Selected: 1 object, 145,859 triangles".
4. Keep **Game**, budget **10,000**, **2k**, all maps. Click **Make Game Asset**.
5. In about 10 to 30 seconds the Result box says "145,859 -> 9,999 triangles", "Shape: within 0.03 % of the size".
6. `Z` > **Material Preview**. Click the eye icons in the Outliner to compare the asset and the hidden original. You should see the same colours and no creases or black patches.
7. **Export GLB**.

*Also try:* **Quad-dominant** at 10,000 faces on the same boar. In our test it gave 95 % quads (10,194 faces, 19,844 triangles in an engine).

### Example 2: A Tripo model with a transparent part

A Tripo terminal model of about 977,000 triangles has a badge that is partly transparent.

1. Import, select, open the tab. The panel shows "Selected: 1 object, 977,xxx triangles".
2. Keep all maps ticked (**Alpha** on). Budget **10,000**, **2k**.
3. Click Make Game Asset. The job takes roughly 30 to 60 seconds.
4. The Result box adds "Transparency is baked as a cut-out..." and "The source has transparent parts, so surface behind them was kept."
5. Check the badge in Material Preview: it should still be see-through where it was.

### Example 3: A scene of several objects, one asset per part

A Tripo file with eight chairs arrives as 18 separate Blender objects (AI files are cut into pieces at random). You want eight chair assets, not one big one.

1. Import the file and select all objects (`A`).
2. In the **Scope** box choose **One asset per part**.
3. Choose the budget **per part**: every part gets the *full* budget, so 5,000 gives each chair 5,000 triangles.
4. Click Make Game Asset. The progress text begins with "Asset 1 of 8:" and so on.
5. You get `<name>_asset_01` to `_08`, each with its own textures and its own origin at its bottom centre.
6. Export: select all the assets and click **Export GLB**. You get one file per asset: `<typed name>_<asset name>.glb`.

If the Result says "The selection has too many separate parts...", you selected more than 24 large parts. Select fewer.

### Example 4: A rigged character (what is dropped)

A Meshy character with a skeleton.

1. Select the **mesh** (and the armature if you like; only meshes are used).
2. Make Game Asset as usual.
3. The Result box warns: "The source is rigged. The asset is built from the rest shape: the armature, the weights and the animation are not carried over."
4. What you get: a static mesh in the **rest pose** (the pose without animation). What is dropped: the skeleton, vertex weights, animation, shape keys (the basis shape is used), constraints.
5. If you need a rig: keep your original rigged model for animation, use AssetBake's asset as a static prop or LOD, or rig the asset again (Quad-dominant gives cleaner quads for that, but nothing is promised about rigging).

### Example 5: A model with transparent parts

AssetBake bakes transparency as a **cut-out**: each texel is either solid or a hole.

- Good for: leaves, grilles, fences, a cut-out badge.
- Not good for: glass, water, smoke that are half-see-through. Half-transparent surface becomes solid or a hole.
- Alpha is baked **only when the source has real transparency** (an image whose alpha is 1 everywhere is ignored).
- In the exported GLB the material is alphaMode **MASK**. In Unity, Unreal and Godot, set the material to an alpha-cutout mode if the engine does not do it itself.
- Untick **Alpha** if you want an opaque asset.

### Example 6: Mobile versus hero budget (2,000 versus 50,000)

Use one model and make two versions (this is the **Select Source** workflow).

1. Make an asset at **2,000 triangles, 1k** textures (mobile). Check it.
2. Click **Select Source**. The original is back and selected.
3. Choose **50,000** and **4k**. Click Make Game Asset again.
4. The first asset is hidden automatically; the Result box says "Earlier versions of this asset were hidden". Show it again with its eye icon to compare.

What to expect: at 2,000 the silhouette is simpler but the normal map keeps the bumps. At 50,000 the shape is almost exact. A 4k bake of a 1-million-triangle source takes about 2 to 3 minutes of which AO is about three quarters. (Measured: normal map 20 s, AO 87 to 98 s, other maps 4 to 5 s each at 4k.)

### Choosing a texture size

| Size | Pixels | Use it for | Bake time (all maps, rough) | Memory (extra) |
|---|---|---|---|---|
| **1k** | 1024 x 1024 | Mobile, background props, web, anything seen small | about 10 s | small |
| **2k** | 2048 x 2048 | Most props and characters (default) | about 30 s | about +0.3 GB |
| **4k** | 4096 x 4096 | Hero assets and close-ups | 2 to 2.5 minutes | about +1.3 to 1.9 GB |

Rules of thumb:

- Bake time grows with the pixel count. 2k costs about 1.5 times the time of 1k for the bake step, 4k a lot more because of AO.
- A bigger texture does not fix a bad UV layout, and it makes the file bigger (PNG; a 4k map is some MB; the GLB holds all of them).
- A smaller budget with a bigger texture often looks better than a bigger budget with a small texture: the normal map carries the detail.
- 1k at 5,000 triangles is a good web/mobile pair. 2k at 10,000 to 20,000 is the usual PC pair. 4k at 50,000 is a hero.

### Time and memory (what to expect)

Measured on one Apple-silicon Mac, Game mode, 20,000 triangles, 1k texture:

| Source triangles | Total time | Peak memory |
|---|---|---|
| 1.7 million | about 45 s | about 5 GB |
| 2.7 million | about 1 minute | about 5.5 GB |
| 6.9 million | about 2.7 minutes | about 7.7 GB |
| 10.8 million | about 5 minutes | about 10.6 GB (the add-on refuses above 8 million) |

Time grows about 27 seconds per million source triangles. Real AI models are 0.05 to 1.7 million triangles. Quad-dominant on a 1 million triangle source took about 1 minute in our tests (mostly the remesher).

---

## 7. Using the asset in game engines

Export with **Export GLB**. The file contains: one mesh with tangents, one material, and the textures: base colour (with alpha when needed), one **ORM** texture, a normal map, and an emission map if the model glows. You can also tick **Also save textures** to get loose PNG files.

**Check before you leave Blender.** Import the GLB into a new Blender scene (File > New, File > Import > glTF 2.0), press `Z` > Material Preview. If it looks right there, the file is right.

**The ORM texture.** Occlusion, Roughness and Metallic are in one image: **R = ambient occlusion, G = roughness, B = metallic**. This is the glTF standard and the Unreal "ORM" layout.

> **[PLACEHOLDER: screenshot of the exported asset in Unity / Unreal / Godot: add after the engine tests]**

### 7.1 Unity (URP and HDRP)

Unity does not read GLB by itself. Use the **glTFast** package.

1. **Window > Package Manager**. Click **+** > **Add package by name...** and type `com.unity.cloud.gltfast` (older guides call it `com.atteneder.gltfast`). Click Add.
2. Drag the `.glb` file into the **Project** window. Unity imports it as a prefab.
3. Drag the prefab into your scene.
4. Keep **OpenGL** normal maps (Unity uses the same convention as glTF).
5. glTFast makes the material for your render pipeline (URP or HDRP) automatically. The ORM texture is used through its own shader graph (occlusion in R, roughness in G, metallic in B).

What to check:

- The material is not pink. Pink means the shader does not match your render pipeline: check that the pipeline package is installed and active (Project Settings > Graphics).
- The model is the right size (a GLB is in metres, Unity too).
- Transparent parts: if a cut-out looks solid, check that the material is in a cut-out mode.
- *If you want Unity's own Lit shader instead* (its "Mask map" has metallic in R, occlusion in G, smoothness in A), you must repack the channels yourself. AssetBake does not write that layout.

### 7.2 Unreal Engine 5

1. Before exporting in AssetBake, choose **DirectX** in the Export box (Unreal expects a green-down normal map). Click **Export GLB**. Tick **Also save textures** if you prefer to build the material yourself.
2. In Unreal: **Edit > Plugins**, search for the glTF importer (in UE 5 it is part of the **Interchange** framework: look for "Interchange" and "glTF" and enable them), then restart the editor. (Exact plugin names change between 5.x versions.)
3. Drag the `.glb` into the **Content Browser**. In the import dialog accept the defaults.
4. Open the imported material. If the ORM texture is not wired for you, wire it like this: add a Texture Sample with the `_orm` texture. In the texture editor set **sRGB off** and **Compression: Masks (no sRGB)**. Connect **R** to **Ambient Occlusion**, **G** to **Roughness**, **B** to **Metallic**.
5. The normal map texture should use **Compression: Normalmap** and **Flip Green Channel off** (you already flipped it in AssetBake). If you exported OpenGL by mistake, you can instead tick Flip Green Channel in the texture editor.

What to check: the surface looks lit and not inverted (bumps that look like dents mean the normal map convention is wrong), roughness looks plausible (shiny where it should be), the scale (GLB metres become centimetres).

### 7.3 Godot 4

1. Keep **OpenGL** (Godot uses green-up).
2. Drag the `.glb` into the **FileSystem** dock. Godot imports it as a scene.
3. Double-click the file to open **Advanced Import Settings** if you want to change anything, or just drag the file into your scene.
4. Godot reads the glTF material into a **StandardMaterial3D**. It sets the ORM texture for you: ambient occlusion from **red**, roughness from **green**, metallic from **blue**.

What to check: open the material and look at **Ambient Occlusion**, **Roughness** and **Metallic** (the texture channels should read Red, Green, Blue), turn on a light and a WorldEnvironment so you can see shading. If a cut-out looks solid, set the material's **Transparency** to **Alpha Scissor**.

### 7.4 three.js and web viewers

1. Keep **OpenGL**.
2. In three.js load the file with `GLTFLoader` (from `three/examples/jsm/loaders/GLTFLoader.js`). It reads the ORM texture, the normal map, the alpha mask and the emission for you.
3. **Add light.** A PBR material needs an environment map (`scene.environment`) or lights; without them the model looks dark. A quick way to test without code is a web viewer such as the glTF viewer at gltf-viewer.donmccurdy.com, Babylon.js Sandbox, or Google's `<model-viewer>` element: drop the GLB onto the page.
4. For small downloads: the GLB has textures as PNG. For the web you may want to compress them afterwards with a glTF optimiser. AssetBake does not do that.

What to check: the model faces the right way up (glTF is Y-up, already handled), colours look right (not washed out), the file size is acceptable, and the transparent parts are cut out.

---
## 8. Troubleshooting

### 8.1 Messages you can see

Where: **Panel** = main panel, **Result** = Result box, **Status bar** = the line at the bottom of Blender (also shown as a popup), **Pop-up** = the red or yellow report that appears at the bottom right.

| Message | Where | Meaning and fix |
|---|---|---|
| "Select one or more mesh objects" | Panel | Nothing usable is selected. Click your model. |
| "Select one or more mesh objects first" | Status bar | You started the job with no mesh selected (for example by a shortcut). Select a mesh. |
| "The selected meshes have no faces" | Status bar, Result | The mesh is empty (only points or edges). Use a mesh with faces. |
| "Nothing to work on: the selection has no mesh objects." | Result | The job found no mesh. Select mesh objects. |
| "Too large: the limit is 8,000,000 triangles. Decimate the source first." / "The selection has N triangles. The limit is 8,000,000: decimate the source first." | Panel, Result | Too big. In Blender add a **Decimate** modifier (Modifier properties tab > Add Modifier > Generate > Decimate), set a Ratio, **Apply** it, then run again. Or split the model. |
| "Large input: this takes several minutes and several GB of memory." | Panel, Result | Over 5 million triangles. It works but needs time and memory. Close other heavy programs. |
| "Quad-dominant needs at least 5,000 faces." / "Quad-dominant needs a budget of at least 5,000 faces." | Panel, Status bar | Raise the budget to 5,000 or more, or use Game mode. |
| "Quad-dominant is not available on Windows (x86-64) yet: the quad remesher has no build for this system. Game mode works." | Panel | No Windows build yet. Use Game mode. |
| "Quad-dominant needs Python 3.11 or newer inside Blender. Game mode works." | Panel | Your Blender is too old (below 4.2). Update Blender. |
| "Could not start the background job: ..." | Result, Status bar | The hidden Blender could not be started or its folder could not be written: disk full, no permission, or a security program blocked it. Free disk space, allow Blender to start another Blender process, and try again. |
| "The job failed: ..." | Result (with "Log file for support: ..." and **Copy log path**) | An unexpected error in the hidden Blender. The text after the colon says what. Section 8.3 shows where the log is. Send the log to support. |
| "The job failed: the worker stopped with exit code N." | Result | The hidden Blender crashed or was killed (often out of memory). See "Memory" in 8.2. |
| "The job failed: RuntimeError: the selection has no size" | Result | All points are in one place. Select a real mesh. |
| "The job failed: RuntimeError: could not join the selected objects into one mesh" | Result | The selected objects could not be merged. Try fewer objects, or join them yourself first (`Ctrl+J`). |
| "The job failed: could not bring the result into the scene (...)" | Result | The result is made but could not be added to your scene. Try again; send the log if it repeats. |
| "The job failed: the scene the job was started in was deleted" | Result | You deleted or replaced the scene while the job ran. Run again. |
| "Cancelled. Nothing was changed." | Result | You pressed Cancel (or opened another file, or switched the add-on off while a job ran). Nothing was changed. |
| "Done - return to Object Mode to get the result" | Panel | The result waits for Object Mode. Switch all objects to Object Mode. |
| "Go to Object Mode to make an asset" | Panel | Press `Tab` to leave Edit Mode. |
| "Texture with unsaved changes: ..." | Result, pop-up | A source image was changed but not saved. Save or pack it, run again. |
| "Texture file not found: ..." | Result | A source texture file is missing. Find it (File > External Data > Find Missing Files) and run again. |
| "Select an asset made with Make Game Asset first" | Status bar | **Select Source** needs an AssetBake asset. Click an asset, then the button. |
| "This asset does not remember its source (it was made with an older version). Unhide the original objects" | Status bar | Assets made before version 0.1.3 do not remember their source. Show the originals in the Outliner. |
| "The source is not in this scene any more (deleted, or renamed after the file was reopened): ..." | Status bar | The original is gone, or was renamed after you reopened the file. Restore it, or import the model again. |
| "The source is in a collection that is hidden or switched off. Show the collection in the Outliner, then click again: ..." | Status bar | Tick the collection's checkbox in the Outliner and click **Select Source** again. |
| "Selected N of M source object(s)" | Status bar | Success. When some sources could not be found, the same line continues with "; not found (...)", "; in a hidden collection, show it in the Outliner: ..." or "; no source remembered for: ..." (yellow warning). |
| "Select an asset made with Make Game Asset" | Status bar | **Export GLB** needs an asset. Select one. |
| "Blender's glTF 2.0 add-on is switched off: enable it in Preferences > Add-ons" | Status bar | Edit > Preferences > Add-ons, search "glTF", tick **Import-Export: glTF 2.0 format**. |
| "Export failed: ..." | Status bar | The file could not be written (folder not writable, name not allowed, disk full). Choose another folder and try again. |
| "<name> was edited after it was made: its triangles are no longer the ones the textures were baked on, so the normal map can show small errors where faces changed. The file is split by a fixed rule ... For an exact result make the asset again." | Status bar (warning) | You changed the faces of a quad-dominant asset. The file is still written. To be exact, make the asset again from the source. |
| "Exported N file(s) to <folder>" | Status bar | Success. |
| "Copied: <path>" | Status bar | The log path is on your clipboard (button **Copy log path**). |

### 8.2 Situations and symptoms

| Symptom | Cause | Fix |
|---|---|---|
| **Make Game Asset is greyed out** | Not in Object Mode; or no mesh selected; or more than 8,000,000 triangles; or Quad-dominant with a budget below 5,000; or a job is already running. | Press `Tab` to enter Object Mode, select a mesh, lower the triangle count, raise the budget, or wait for the job. |
| **The panel says an asset can not be used as a source** | You selected an asset AssetBake made. | Click **Select Source** to bring back the original, then run again. |
| **Nothing is selected / "Select one or more mesh objects"** | The model is a curve, an empty (the root of an imported file) or a group, not a mesh. | Open the object's tree in the Outliner and select the mesh itself, or press `A` in the viewport. |
| **The job is slow** | Big source, 4k textures, or AO. | See the time table in section 6. Leave the viewport alone: orbiting the heavy original in Material Preview makes Blender itself use the processor and the job takes about half as long again. Use 1k or 2k, untick AO, or decimate first. |
| **Blender is busy / slow while I orbit during a job** | The viewport draws the heavy original again and again. | Switch the viewport to Solid, or hide the original, or wait. The job itself runs in a separate process. |
| **Large inputs (over 2 million / over 8 million triangles)** | Over **2,000,000**: Quad-dominant is not available, a Game asset is made. Over **5,000,000**: warning. Over **8,000,000**: refused. | Decimate with Modifiers > Decimate and apply, then run. |
| **Out of memory / the job failed with an exit code** | Very big source, 4k textures, other programs. | Close other programs, use 2k, decimate the source below 5 million triangles, or run one job at a time. As a rough guide, a source of 8 million triangles needs about 8.5 GB of memory; on a 16 GB computer stay under about 4 million triangles. |
| **Black patches on the asset** | The model has gaps or thin sheets that touch; or the original material has parts the bake cannot read; or AO is dark in tight spots. | Look at the Result: "baked from the back of a surface" or "Missed texels". Try a higher budget, untick **AO** to check if the dark patches are occlusion, untick **Remove hidden surface**, or fix gaps in the source. |
| **Creases or hard facets on a smooth model** | Low budget. | Raise the budget. The normal map hides most facets, but silhouettes need triangles. |
| **Grey patches / the colour is wrong** | A material with no Principled BSDF or Emission shader (for example a shader inside a node group): "baked as plain grey". | Rebuild the material with a plain Principled BSDF. |
| **Textures are missing after I saved the .blend** | They are **packed** inside your .blend, so they should be there. If you used **File > External Data > Unpack**, they are written to a `textures` folder next to the .blend; if you moved the .blend without that folder you lose them. | Keep the `textures` folder with the .blend, or **Pack** again (File > External Data > Pack Resources). For engines, use **Also save textures** or the GLB, which always contains the textures. |
| **The exported GLB has no textures in my viewer** | The viewer does not read embedded textures. | Tick **Also save textures** and use the PNG files. |
| **Quad-dominant button is greyed out on macOS** | The remesher library is not loaded. | See "macOS blocked a library" below. |
| **Quad-dominant button is greyed out on Windows** | No Windows build yet. | Use Game mode. |
| **Result says Quad-dominant was not possible** | See section 5.3. | Use the reason; try another budget; or keep the Game asset, which is fine. |
| **Result says the source is under budget** | Your model has fewer triangles than the budget. | Nothing to reduce. The model is kept as it is with new UVs and textures. Lower the budget if you want it smaller. |
| **"Assets are not accepted as source"** | AssetBake never uses its own assets as a source. | Select Source. |
| **The result does not appear / I am in Edit Mode** | While an object is in Edit, Sculpt or Paint mode the result waits: "Done - return to Object Mode to get the result". | Return to Object Mode (`Tab`). |
| **I cannot see the new asset** | It sits exactly where the original was. The new one is visible and the earlier versions are hidden. | Check the Outliner eye icons. Press `Home` in the viewport to frame everything. |
| **Earlier versions disappeared** | By design: a new asset from the same source hides the earlier ones in the viewport. | Click their eye icon in the Outliner. They are still in renders. |
| **I pressed Cancel and nothing is left** | Cancel kills the job and removes its temporary files. Nothing in the scene was changed. | Run again. |
| **A job disappeared when I opened another file** | Opening a file or switching the add-on off cancels a running job (the hidden Blender must not run on). | Wait for the job to finish before opening another file. |
| **The asset is much smaller or bigger than my model** | The asset is placed with the origin of the source (or the top parent) and has the same size. With one asset per part, each part has its origin at its own bottom centre. | Check scale and location in the N panel's Item tab. |
| **The normal map looks inverted in my engine** | Wrong convention. | Re-export with **DirectX** (Unreal) or **OpenGL** (Unity, Godot, Blender, web). |
| **Roughness or metal looks wrong** | The engine reads the wrong channels of the ORM texture. | R = AO, G = roughness, B = metallic. Set the texture to linear (sRGB off). |

**macOS blocked a library.** If the Result says "the quad remesher could not be loaded on this system" or the mode is greyed out after install: macOS sometimes quarantines files that came from a browser download. Install the zip with **Install from Disk** (never unzip it yourself). If it still fails, write to support and quote the exact sentence from the Result box (it names the error). Game mode never needs the library.

**Windows: "Quad-dominant is not available".** Expected: Windows has no remesher build yet. Game mode is complete.

### 8.3 Where is the error log, and how do I copy its path?

When a job fails, the Result box shows "Log file for support: <path>" and a button **Copy log path**. Click it and paste the path into your e-mail, or open the file and attach it. The file is called `assetbake_last_error.log` and lives in your system's temporary folder (on a Mac a folder under `/var/folders/.../T/`, on Windows `%TEMP%`, on Linux `/tmp`). It holds the AssetBake and Blender version and the text the hidden Blender wrote. It contains no pictures of your model.

### 8.4 Reading the Result when the budget is not hit

If the count is not within 5 % of the budget, the Result says why (section 5.2): the mesh is tangled, or hidden surface was removed, or the source is smaller. In every case the asset is still usable.

---

## 9. Known limitations, glossary, FAQ and support

### 9.1 Known limitations

- **Tested on macOS only.** Blender 4.2 LTS, 4.5 LTS and 5.x pass the automatic tests on a Mac with Apple Silicon. Windows and Linux are untested. The Linux remesher build passed its own build checks but has never been loaded in Blender by us.
- **Quad-dominant:** no Windows build. About one thick model in eight still falls back to Game for a small local defect, and the result can change between neighbouring budgets and between Blender 4.x and 5.x. Sources over 2,000,000 triangles are not tried. Hidden surface is not removed in this mode. The triangles along the seam between quads and thin parts are closed but stretched. Nothing is promised about rigging or subdivision.
- **Rigs, weights, animation, shape keys, constraints are dropped.** The asset is a static mesh in the rest shape.
- **Transparency is a cut-out only.** No glass-like blending.
- **Only these material properties are carried over:** base colour, roughness, metallic, emission, alpha, and the shape's normals. Others (clearcoat, transmission, subsurface, anisotropy, displacement) are not.
- **A shader inside a node group is not read** (baked grey).
- **GLB export only.** No FBX, no engine presets, no LOD chain.
- **One material, one set of textures per asset.** Several materials of the source are merged into one texture set.
- **New UVs are always made.** The UVs of your source are not kept.
- **8-bit PNG textures.** No 16-bit or EXR output.
- **The AO map is baked at half size above 1k** and enlarged.
- **Limits:** 8,000,000 source triangles (warning from 5,000,000), 2,000,000 for Quad-dominant, 24 large parts and 2,000 loose pieces for one asset per part.
- **Very thin or open sheets** (leaves, flags, sheet metal) cannot become quads and are kept as triangles.
- **A baked asset is built for the exact shape.** If you edit it later, a quad-dominant asset's GLB can show small normal map errors.
- The tests used real AI models that were re-exported (from Sketchfab) as well as models made with several generators. Files straight from every generator's website may differ in small ways, such as names and material setups.

---

### 9.2 Glossary

| Term | Plain meaning |
|---|---|
| **Budget** | The number of triangles (Game) or faces (Quad-dominant) you allow for the finished asset. |
| **Triangle** | A face with three corners. Game engines draw everything as triangles. |
| **Quad** | A face with four corners. Easier to edit, sculpt and rig. A game engine splits it into two triangles. |
| **N-gon** | A face with five or more corners. AssetBake output has none. |
| **Face** | One flat piece of a mesh (a triangle or a quad). |
| **UV** | A flat map of the 3D surface. Each point of the model has a position on the texture. |
| **UV island** | One piece of the surface cut out and laid flat in the UV map. |
| **Atlas** | One texture holding all the islands of a model. |
| **Bake** | Painting the look of one object onto the textures of another, with light and shadow calculations. |
| **PBR** | Physically based rendering: surfaces described by base colour, roughness and metallic instead of painted-in light. |
| **Normal map** | A texture that fakes small bumps by telling the renderer which way the surface faces. |
| **OpenGL vs DirectX normal map** | The same map with the green channel flipped. OpenGL: green up (Blender, Unity, Godot, glTF). DirectX: green down (Unreal). |
| **AO (ambient occlusion)** | Soft darkness in cracks and corners. |
| **ORM** | One texture packing three maps: **O**cclusion in red, **R**oughness in green, **M**etallic in blue. |
| **Roughness / Metallic** | How rough or shiny a surface is / whether it is metal. |
| **Emission** | Light that a surface gives off (a glowing screen). |
| **Alpha** | Transparency. |
| **Cage** | A slightly puffed-up copy of the low mesh. The bake shoots rays from the cage onto the original. AssetBake sets its distance from the measured difference between the meshes. |
| **Deviation** | How far the new surface is from the original, as a percent of the model size. |
| **Texel** | One pixel of a texture. |
| **Source** | Your original selected objects. |
| **Part** | A piece of the model that stays connected after doubled points are merged. |
| **GLB** | A single file holding a 3D model and its textures (binary glTF). |
| **Object Mode** | The Blender mode in which you select whole objects (the default). `Tab` switches to Edit Mode and back. |
| **Rest shape** | The pose of a rigged model without animation. |

---

### 9.3 FAQ

**1. Does it change my original model?** No. The original is hidden (viewport and render), never modified. **Select Source** brings it back.

**2. Can I undo it?** Yes. `Cmd+Z` (Mac) or `Ctrl+Z` removes the asset and unhides the source in one step.

**3. Can I keep working in Blender while it runs?** Yes. It runs in a separate process. Switching to Edit Mode is fine: the result waits until you are back in Object Mode. Opening another file or switching the add-on off cancels the job.

**4. How long does it take?** Usually 10 seconds to 1 minute at 2k. Huge sources and 4k textures take minutes (see section 6).

**5. Which models work best?** Solid AI models (characters, creatures, props) with 50,000 to 2 million triangles. Open sheets, leaves and sheet metal need Game mode.

**6. Do I need to unwrap or clean the model first?** No. AssetBake merges doubled points, fixes faces, makes its own UVs and bakes.

**7. Does it keep my UVs?** No. It always makes new ones.

**8. Does it keep my textures?** It bakes them into a new set on the new mesh, so the look is kept and the layout is new.

**9. Game mode or Quad-dominant?** Game for games, web and AR. Quad-dominant if you will edit, sculpt or rig the asset. Quads give nothing in a game engine, which splits them again.

**10. Why did Quad-dominant give me a Game asset?** The model was too thin or the quad mesh failed a strict check. The Result box gives the reason. A try at a different budget often works.

**11. Can I use it on Windows?** Game mode, yes. Quad-dominant has no Windows build yet. Note that we tested on macOS only.

**12. Which Blender versions?** 4.2 LTS, 4.5 LTS and 5.x (tested on macOS).

**13. Does it work with FBX or OBJ models?** Yes, anything that is a mesh in Blender. Import it first. (FBX and OBJ sources are covered by our tests.)

**14. Does it keep my rig and animation?** No (section 6, example 4). It makes a static asset.

**15. What is the difference between OpenGL and DirectX?** Only the direction of the green channel of the normal map. Unreal wants DirectX; Unity, Godot, Blender and the web want OpenGL.

**16. Why is my asset a bit over or under the budget?** The reducer aims for 1 % and promises 5 %. Tangled sources can end outside that; the Result says why.

**17. Can I make several assets from one scene?** Yes: Scope **One asset per part**. Or select one object at a time.

**18. Can I use the result commercially?** AssetBake adds no licence restrictions of its own to the files you make. What you may do with them depends on your source model and on the terms of the AI service that made it (check those yourself). The extension itself is **GPL-3.0-or-later**, and the bundled remesher is our own binding of Instant Meshes (BSD) with its sources public at https://github.com/mendbox/im-remesh. Licences of the parts inside the binary are in the extension's `vendor/licenses` folder.

**19. Does it need internet or a graphics card?** No internet. The bake uses the processor (Cycles CPU), not the graphics card.

**20. Can I run it on a scene with 20 objects at once?** Yes. Select all and use one asset, or one asset per part. Over 24 large parts it makes one asset.

**21. Where are my textures?** Inside the .blend (packed) and inside the GLB. **Also save textures** writes PNG files next to the GLB.

**22. Is there a free version?** Yes: AssetBake Lite (Game mode, budgets up to 5,000 triangles, 1k textures, Base Colour, Normal and AO). See the Lite manual.

---

### 9.4 Support, compatibility and changelog

**Support:** support@mendboxstudio.com. Website: https://mendboxstudio.com. Please include: your Blender version and operating system, the AssetBake version (shown in the panel title), the text of the Result box (copy it), the settings (mode, budget, texture size, scope), the log file if the job failed (use **Copy log path**), and if possible the model or a small .blend that shows the problem. Never send passwords or personal data.

**Source and licence:** the extension is GPL-3.0-or-later. Corresponding source of the bundled remesher (GPL-3.0 section 6): https://github.com/mendbox/im-remesh (tag `v0.1.2`).

**Compatibility, as tested:** Blender 4.2.23, 4.5.14 and 5.2.2 on **macOS** (Apple Silicon): 33 test cases and 1,011 checks per version passed (last full run on the 0.2.2 build). **Windows and Linux were not tested.** Quad-dominant is built for macOS (Apple Silicon) and Linux (x86-64); there is no Windows build yet.

**Changelog**

| Version | Date | Change |
|---|---|---|
| 0.2.3 | 2026-11-17 | The panel title shows the version. Quad-dominant: faces that share the same three points are reduced to one, so the GLB matches the asset. |
| 0.2.2 | 2026-11-17 | Better use of the texture space of some UV layouts (a rule about where each face starts). |
| 0.2.1 | 2026-11-17 | Product renamed to **AssetBake** (formerly "AI Asset Maker"). The result no longer depends on the order of the source's points and faces. Review fixes. |
| 0.2.0 | 2026-11-16 | **Quad-dominant mode** (macOS Apple Silicon, Linux x86-64), with a second attempt, an estimate before the run, the fixed split for bake and GLB, and a compiled remesher inside the extension. |
| 0.1.5 | 2026-11-16 | Fixed a mix-up of roughness, metallic and alpha channels in the combined bake. Includes everything from 0.1.4. |
| 0.1.4 | never released | Its changes shipped in 0.1.5: names that tell what the model is, earlier versions hidden, bake about twice as fast at 2k. |
| 0.1.3 | 2026-11-16 | **Select Source** button; every asset remembers its source. |
| 0.1.2 | 2026-11-15 | The budget is kept on tangled meshes, with an honest report when it is not. |
| 0.1.1 | 2026-11-15 | Fixes from an independent review. |
| 0.1.0 | 2026-11-14 | First working build: Game mode in a background process, panel, GLB export. |

Versions before 0.2.3 were internal development builds.
