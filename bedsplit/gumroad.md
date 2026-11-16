<!-- Paste-ready for the Gumroad product description / content page. Tables are converted to lists. Replace https://mendbox.github.io/docs with the GitHub Pages address, no trailing slash. -->

# BedSplit 0.1.3: user manual

*Manual for BedSplit 0.1.3 (Blender add-on). Trailer: <https://youtu.be/-EbloCRxfUs>*

Buy: [Superhive](https://superhivemarket.com/products/bedsplit) | [Gumroad](https://quadfix.gumroad.com/l/bedsplit) | Free Lite version: [GitHub](https://github.com/mendbox/bedsplit-lite)

**Contents:** [1 What it does](#1-what-it-does) | [2 Install](#2-requirements-and-installation) | [3 Quick start](#3-quick-start) | [4 Reference](#4-reference-every-panel-button-and-setting) | [5 Worked examples](#5-worked-examples) | [6 Troubleshooting](#6-troubleshooting) | [7 Limitations](#7-known-limitations) | [8 Glossary](#8-glossary) | [9 FAQ](#9-faq) | [10 Support and changelog](#10-support-compatibility-and-changelog)


## 1. What it does

BedSplit cuts a 3D model into parts that fit the print bed (the flat plate) of your 3D printer. It repairs the model first, adds round dowel sockets (holes for little pegs) in every joint so the parts line up, lays each part flat, and exports one STL file per part plus the dowel. You choose your printer, press **Split to Bed**, and print.

![BedSplit: a model cut into printable parts](https://mendbox.github.io/docs/assets/bedsplit/gif_split.gif)

**Who it is for**

- Cosplay and prop makers: helmets, armor and swords bigger than the build plate.
- People who print statues, busts, figurines and terrain in several parts.
- Owners of small printers (180 mm and 220 mm beds) who want large prints.
- Anyone who prints AI-generated models (Meshy, Tripo, Hunyuan3D) at real size. Those meshes are often broken, and BedSplit repairs them first.

It is made for FDM printers (the common kind that melts plastic filament).


## 2. Requirements and installation

**Requirements**

- Blender 4.2 or newer (4.2 LTS, 4.5 LTS and 5.x). It is a Blender *extension*.
- No internet and no extra software.
- The add-on is a **.zip** file. **Do not unzip it.**
- The manifest asks for one permission: to write files, used only by **Export STL**.

**Install from disk**

1. In Blender open **Edit > Preferences** (Mac: `Cmd+,`).
2. Click **Get Extensions** in the left list.
3. Click the small arrow (v) menu in the top-right corner and choose **Install from Disk...**
4. Pick the BedSplit `.zip` file.
5. The extension turns on. If its checkbox is not ticked, tick it.

**Where is the panel?**

1. Hover the mouse over the 3D viewport. Be in **Object Mode** (the panel only shows in Object Mode).
2. Press `N` to open the sidebar.
3. Click the tab named **BedSplit** (written vertically at the right edge).
4. There are two panels: **BedSplit** (settings and the Split button) and **Result  ·  BedSplit v0.1.3** below it.

If no mesh is selected the panel shows "Select a mesh object".

**Update to a new version**

1. Download the new zip.
2. **Install from Disk** again. Blender replaces the old version.
3. Restart Blender if the version in the Result panel header does not change.

**Uninstall**

**Edit > Preferences > Get Extensions**, find BedSplit, open the arrow next to it and choose **Uninstall**. Parts you already created stay in your file as normal objects (in the collection "BedSplit - <model name>").

**Lite or full?**

- **Panel tab name**: BedSplit Lite (free, GPL): BedSplit Lite; BedSplit (full): BedSplit
- **18 printer presets and Custom size**: BedSplit Lite (free, GPL): yes; BedSplit (full): yes
- **Resize for print**: BedSplit Lite (free, GPL): yes; BedSplit (full): yes
- **Split to Bed (automatic flat cuts, closed parts)**: BedSplit Lite (free, GPL): yes; BedSplit (full): yes
- **Parts laid flat, per-part STL export, undo**: BedSplit Lite (free, GPL): yes; BedSplit (full): yes
- **Dowels and sockets (diameter, depth, clearance, dowels per joint)**: BedSplit Lite (free, GPL): no; BedSplit (full): yes
- **Mesh repair (holes, flipped normals, overlapping shells, AI meshes)**: BedSplit Lite (free, GPL): no (closed meshes only); BedSplit (full): yes
- **Find Cuts and movable cut planes**: BedSplit Lite (free, GPL): no (automatic cuts only); BedSplit (full): yes



## 3. Quick start

1. Import or open your model. Select it in **Object Mode**. A `.glb` import with an empty parent is fine: BedSplit finds the mesh under it.
2. Press `N`, open the **BedSplit** tab. In **Printer** choose your printer (for example Bambu Lab A1 / P1 / X1).
3. Look at **Model size**. If "Now" shows the wrong size, tick **Resize for print** and type the real size of the longest side in millimetres.
4. Press **Split to Bed**. The parts appear next to your model, flat, each in its own colour. The **Result** panel lists every part with a check mark.
5. Press **Export STL** and choose a folder. Print all part files, and the dowel file as many times as its name says.

![Panel clicks: split a model](https://mendbox.github.io/docs/assets/bedsplit/gif_panel_split.gif)


## 4. Reference: every panel, button and setting

### 4.1 Panel "BedSplit"

#### Box "Printer"

- **Printer (dropdown)**: What it does: The printable volume of your printer (width x depth x height, in mm). 18 presets, listed below, plus **Custom size**.; When to use it: Always. Changing it clears the cut list.; Default / range: Creality Ender 3 / V3 SE
- **Bed (three numbers)**: What it does: Width, depth and print height in mm. Only shown when Printer is "Custom size".; When to use it: If your printer is not in the list: look up the "build volume" in the printer's specifications.; Default / range: 220 x 220 x 250, minimum 40 mm
- **"Bed: W x D x H mm"**: What it does: Shows the volume in use.


Printer presets (width x depth x height, mm):

- **Bambu Lab A1 mini**: 180 x 180 x 180
- **Bambu Lab A1 / P1 / X1**: 256 x 256 x 256
- **Bambu Lab H2D**: 350 x 320 x 325
- **Prusa MINI+**: 180 x 180 x 180
- **Prusa MK4 / MK3**: 250 x 210 x 220
- **Prusa XL**: 360 x 360 x 360
- **Prusa CORE One**: 250 x 220 x 270
- **Creality Ender 3 / V3 SE**: 220 x 220 x 250
- **Creality K1**: 220 x 220 x 250
- **Creality K1 Max**: 300 x 300 x 300
- **Creality Ender 5 Plus**: 350 x 350 x 400
- **Anycubic Kobra 2**: 220 x 220 x 250
- **Anycubic Kobra 3**: 250 x 250 x 260
- **Elegoo Neptune 4**: 225 x 225 x 265
- **Elegoo Neptune 4 Max**: 420 x 420 x 480
- **Sovol SV06**: 220 x 220 x 250
- **Voron 2.4 300**: 300 x 300 x 280
- **Voron 0.2**: 120 x 120 x 120


The values are printable volumes taken from each printer's specifications and may differ from yours if you changed the printer. Use Custom size to set your own.

#### Box "Model size"

- **"Now: N mm longest side"**: What it does: The current longest side of the selected model, in mm, as BedSplit reads it (scene units are respected).; When to use it: Check this first. If it is wrong, the model is in the wrong scale.
- **Resize for print (checkbox)**: What it does: Scales the parts so the longest side of the whole model equals the size below. Off: the model is printed at its current size.; When to use it: Statues and figurines imported at an arbitrary size. STL files made in mm and imported as metres.; Default / range: Off
- **Longest side, mm**: What it does: The printed size of the longest side of the whole model. Greyed out when Resize is off.; When to use it: Type 600 for a 60 cm statue.; Default / range: 400, minimum 10 (slider up to 3000)


How size is read: BedSplit measures the model in world space (position, rotation and scale of the object, and its modifiers, are included). **Cuts are always along the world X, Y and Z axes**, so a rotated model is cut along the world axes, not along its own axes. With Resize off, one Blender unit counts as 1 metre unless you changed the scene's unit scale.

#### Box "Cuts"

- **Find Cuts (button)**: What it does: Places the cut planes automatically for your printer and shows them in the viewport and as numbers in the panel. Reports "BedSplit: N cut(s)" or "the model fits the bed, no cut needed".; When to use it: When you want to see or move the cuts before splitting.
- **Show cuts in viewport (checkbox)**: What it does: Shows or hides the coloured planes. Moving a cut turns it back on. After a split the planes are hidden.; Default / range: On
- **Cut rows: **X / Y / Z** + number + x**: What it does: Each row is one cut plane. The letter is the axis; the number is the distance in mm from the start of the model along that axis. Change the number to move the cut. The x button removes the cut.; When to use it: Move a cut away from a face or a hand, or remove one.; Default / range: 0 to 2000 mm
- **Add cut: X Y Z (buttons)**: What it does: Adds a cut across that axis, in the middle of the largest remaining piece.; When to use it: Add a cut where the automatic ones are not enough.
- **Hint "Automatic"**: What it does: No cut list yet. Split will find the cuts itself.
- **Hint "No cuts: model fits the bed"**: What it does: The model already fits.


If you have a cut list, **Split to Bed** uses exactly those cuts. Removing every cut returns to "Automatic". Changing the printer, bed size or the Resize settings clears the cut list, because the cuts no longer match.

#### Box "Dowels"

- **Dowel diameter, mm**: What it does: Thickness of the round peg.; When to use it: Thicker for big parts.; Default / range: 6.0, range 2 to 20
- **Depth in each part, mm**: What it does: How deep the socket goes into each part. The dowel is twice this long.; Default / range: 6.0, range 2 to 30
- **Clearance, mm**: What it does: Gap between dowel and socket. 0.2 suits most printers. Raise it if dowels do not go in, lower it if they are loose.; When to use it: Test with one small joint first.; Default / range: 0.20, range 0 to 1
- **Dowels per joint**: What it does: The largest number of dowels placed on one cut face.; When to use it: More dowels for big faces.; Default / range: 2, range 1 to 3


A socket is placed only where the wall around it is thick enough: about 1.6 mm of material around each socket plus the clearance. With the defaults a cut section needs to be roughly 10 mm wide at that spot. Joints where it does not fit get no dowel and the Result panel tells you ("glue these").

#### The big button

- **Split to Bed**: Repairs the model, cuts it into parts that fit the printer, adds dowel sockets, and creates the parts as new objects. Your original model is **not changed**. Works in Object Mode with a mesh selected.


What you get:

- A collection "BedSplit - <model name>" next to your model. If you split the same model again, the previous result for it is replaced.
- Parts named "<model> part 01", "part 02"... laid out in a grid beside the original, each turned so its largest cut face lies flat on the ground. Viewport colours show each part (Solid mode).
- One extra object "<model> dowel xN": the dowel, to print N times.
- The new parts are selected. The original stays active so the panel keeps working on it.

### 4.2 Panel "Result  ·  BedSplit v0.1.3"

- **"No result yet"**: You have not split anything in this file.
- **"N parts, M dowels" + check or warning icon**: Totals. A check mark means all checks passed.
- **"<model>, T s"**: Which model and how long it took in seconds.
- **Messages (lines with an icon)**: What was repaired and what needs attention (see section 6).
- **"Part K: W x D x H mm" with a check or a cross**: Size of each part. Check = it fits the bed **and** is a closed solid. Cross = too big for the bed or not closed.
- **Export STL (button)**: Choose a folder. One STL per part in millimetres, plus one dowel file whose name says how many to print (for example `statue_dowel_x45.stl`). Reports "N STL file(s) saved to <folder>". Greyed out until you split.


STL files contain geometry only: no colours, textures or UVs.


## 5. Worked examples

### Example 1: A helmet for a 180 mm bed

1. Import the helmet. Select it in Object Mode.
2. Printer: **Bambu Lab A1 mini**.
3. Check "Now". If the helmet is 300 mm long, leave Resize off.
4. Press **Split to Bed**. Check that every part has a check mark.
5. **Export STL**, load the files in your slicer (PrusaSlicer, Bambu Studio, Cura). Print the dowels too.

![Printer presets change the number of parts](https://mendbox.github.io/docs/assets/bedsplit/gif_printers.gif)

### Example 2: A statue at 600 mm on a 220 mm printer

1. Import the statue (STL, OBJ or GLB).
2. Tick **Resize for print**, type **600** into **Longest side, mm**.
3. Printer: **Creality Ender 3 / V3 SE**.
4. Press **Find Cuts** to see the cut planes. Check that no cut goes through the face or a hand.
5. Press **Split to Bed**.

![Resize for print and cuts](https://mendbox.github.io/docs/assets/bedsplit/gif_bed.gif)

### Example 3: A printer that is not in the list

1. Find the printer's "build volume" in its specifications (width x depth x height in mm).
2. Printer: **Custom size**. Type the three numbers into **Bed**.
3. Split as usual. If your slicer needs some margin (skirt, brim), reduce the numbers by 5 to 10 mm.

### Example 4: Moving a cut off a face

1. Press **Find Cuts**. The planes appear.
2. Look from the front. If a plane crosses the face, change its number by a few millimetres (for example 198.9 to 190).
3. Press **Add cut** or the x button if needed.
4. Press **Split to Bed**. The cuts you edited are used exactly.

![Moving cut planes](https://mendbox.github.io/docs/assets/bedsplit/gif_planes.gif)

### Example 5: Test the dowel fit before a long print

1. Split your model as usual.
2. Print the smallest part of one joint and one dowel.
3. If the dowel does not go in, raise **Clearance** by 0.1 and split again. If it is loose, lower it by 0.05.
4. Then print all the parts with the final value.

![Dowels and sockets](https://mendbox.github.io/docs/assets/bedsplit/gif_dowels.gif)

### Example 6: A broken AI model

1. Import the Meshy, Tripo or Hunyuan3D model. Select it.
2. Press **Split to Bed**. The Result panel says "Repaired: holes closed, normals fixed", "overlapping shells joined" or "surface rebuilt".
3. If it says "Heavy repair: surface rebuilt, fine detail smoothed (X mm)", look at the parts. The shape moved by X mm in some places.

![Repair before the cut](https://mendbox.github.io/docs/assets/bedsplit/gif_repair.gif)


## 6. Troubleshooting

### 6.1 Messages you can see

- **"Select a mesh object" (in the panel)**: Where: Panel; Meaning and fix: Select the model. Curves, empties or Edit Mode are not accepted. For a `.glb` import with several meshes, select one mesh.
- **Buttons are greyed out**: Where: Panel; Meaning and fix: You are not in Object Mode, or no single mesh is selected.
- **"The object has no faces"**: Where: Split, Find Cuts, Add cut; Meaning and fix: The mesh is empty.
- **"The object has no size"**: Where: Split, Find Cuts, Add cut; Meaning and fix: All points are in one place.
- **"The model is N mm long and needs about M parts. Turn on Resize for print and type the real size"**: Where: Find Cuts, Split; Meaning and fix: The model is far too big for the bed: the scale is wrong (typical: a model made in mm imported as metres). Tick Resize for print and type the real length.
- **"BedSplit: the model fits the bed, nothing to split. Add a cut to split it anyway"**: Where: Split; Meaning and fix: No cut is needed. Press Add cut X, Y or Z to split it anyway.
- **"BedSplit: the model fits the bed, no cut needed"**: Where: Find Cuts; Meaning and fix: Same, information.
- **"BedSplit: N cut(s)"**: Where: Find Cuts; Meaning and fix: Success.
- **"BedSplit: split failed (...). Move a cut plane and try again"**: Where: Split; Meaning and fix: The cut could not be completed. Move a cut by a few millimetres (Find Cuts, change a number) and press Split again.
- **"Repaired: holes closed, normals fixed"**: Where: Result; Meaning and fix: Light repair succeeded. Information.
- **"Repaired: overlapping shells joined"**: Where: Result; Meaning and fix: Overlapping parts were merged. Information.
- **"Repaired: surface rebuilt (within X mm)"**: Where: Result; Meaning and fix: The mesh was rebuilt; the shape moved by less than 0.5 mm. Information.
- **"Heavy repair: surface rebuilt, fine detail smoothed (X mm)"**: Where: Result; Meaning and fix: The mesh was too broken. It was rebuilt and fine detail is smoothed by X mm (more than 0.5 mm). Check the parts. If this is not acceptable, repair the model yourself first.
- **"N part(s) not watertight: move a cut plane and split again"**: Where: Result; Meaning and fix: A part is not a closed solid, so a slicer may complain. Move a cut a few millimetres (Find Cuts), split again.
- **"N part(s) larger than the bed: add a cut plane"**: Where: Result; Meaning and fix: A part does not fit. Add a cut (Add cut X, Y or Z) or move cuts, and split again.
- **"N joint(s) without dowel (wall too thin): glue these"**: Where: Result; Meaning and fix: The cut face is too thin for a socket (under about 10 mm with default settings). Glue these joints flat, or move the cut to a thicker place.
- **"A dowel socket came out wrong: check the joints"**: Where: Result; Meaning and fix: A socket overlaps something or has the wrong gap. Look at the joints, try a different cut position.
- **"BedSplit: N parts, M dowels"**: Where: Status bar; Meaning and fix: Summary of a split. Notes are added after it when something needs attention.
- **"BedSplit: choose a folder"**: Where: Export STL; Meaning and fix: Pick a folder in the file window.
- **"BedSplit: cannot write the files (...)"**: Where: Export STL; Meaning and fix: The folder is not writable. Choose another one.
- **"BedSplit: N STL file(s) saved to <folder>"**: Where: Export STL; Meaning and fix: Success.


### 6.2 Things that look wrong

- **The parts are tiny / huge**: Cause: Wrong scale.; Fix: Use Resize for print. Check "Now: N mm longest side".
- **Cuts go through the face**: Cause: The automatic cut chose that place.; Fix: Find Cuts, change the number, Split.
- **I cannot see the cut planes after splitting**: Cause: They are hidden after a split.; Fix: Tick Show cuts in viewport, or edit a cut number, or press Find Cuts.
- **Part is marked with a cross**: Cause: Too big for the bed or not closed.; Fix: See the messages above.
- **Dowels do not go in**: Cause: Printer tolerance.; Fix: Raise Clearance by 0.1 and split again.
- **Dowels are loose**: Cause: Clearance too big.; Fix: Lower Clearance.
- **Split is slow**: Cause: Dense mesh.; Fix: The split takes from under a second to about 30 seconds on a dense mesh. Heavy repair is the slowest part.
- **Only some files were exported**: Cause: Only objects of the last split result are exported.; Fix: Split again, then Export.
- **My parts moved when I moved the model**: Cause: Parts are separate objects.; Fix: After moving the original, press Find Cuts again; planes are valid only for the model's current position.
- **Cut list disappeared**: Cause: You changed the printer, bed or resize settings.; Fix: Press Find Cuts again.
- **Undo**: Fix: `Cmd+Z` / `Ctrl+Z` removes the split and keeps your edited cuts.



## 7. Known limitations

- Cuts are flat planes along the world X, Y and Z axes. No curved cuts, no cuts along natural seams. You can move planes but not rotate them.
- Round dowels only. No dovetails, magnets or snap-fit.
- Thin walls (under about 10 mm at the joint with default settings) get no dowel; glue them.
- Heavily broken meshes are rebuilt with a voxel remesh. This closes them but smooths fine detail.
- No hollowing, no supports, no 3MF export. Made for FDM printing.
- STL files carry no colours, textures or UVs. The parts you see in Blender get plain viewport colours.
- Parts are laid out flat with the largest cut face down. If no cut face fits the bed that way, the part is just centred on the ground, and may be marked with a cross.
- Not tested on Windows or Linux (see section 10).


## 8. Glossary

- **Print bed / build plate:** the flat plate of the printer where the print is made.
- **Build volume:** the largest size the printer can print (width x depth x height).
- **FDM:** the printing method that melts plastic filament (most home printers).
- **STL:** a simple 3D file format made of triangles, used by slicers. Units are usually millimetres.
- **Slicer:** the program that turns a model into printer instructions (PrusaSlicer, Bambu Studio, Cura).
- **Dowel:** a small round peg that fits into sockets of two parts to line them up.
- **Socket:** the hole for the dowel.
- **Clearance:** the small gap between dowel and socket so the peg slides in.
- **Watertight / closed / manifold:** a mesh with no holes, like a sealed bottle. Slicers need this.
- **Non-manifold:** a mesh with holes, loose edges or edges shared by more than two faces.
- **Normals:** the direction each face points (outward on a good mesh). Flipped normals point inward.
- **Voxel remesh:** rebuilding a surface from tiny cubes. It closes a mesh but smooths detail.
- **Cut plane:** the flat imaginary sheet where the model is cut.
- **Joint:** the cut face where two parts meet.
- **Resize for print:** scaling the model to a real-world size.


## 9. FAQ

**Will every part fit my printer?** That is the point of Split to Bed: it picks the cuts so each part fits the width, depth and height you chose, and lays the parts flat. The Result panel shows a check mark per part. If you move cuts by hand and a part gets too big, the panel marks it.

**Does it work with AI-generated or broken meshes?** Yes. Holes, flipped normals, overlapping shells and non-manifold geometry are repaired before the cut. Very broken meshes are rebuilt with a voxel remesh, which smooths fine detail; the panel warns and shows how far the surface moved.

**What connectors does it make?** Round dowels: a socket in both faces of each joint, and one dowel STL to print as many times as the file name says.

**Can I choose where the cuts go?** Yes: Find Cuts, change numbers, add or remove cuts, split again.

**Which printer sizes are included?** 18 presets (see section 4.1). Or use Custom size.

**My STL is in millimetres but Blender shows it huge.** Tick Resize for print and type the real longest side.

**Can I print the original model without splitting?** Yes. If it fits, BedSplit says so.

**Does it change my model?** No. The parts are new objects in a new collection.

**How do I glue the parts?** Push the dowels in (with a drop of glue if you like) and glue the cut faces with cyanoacrylate or epoxy. This is general 3D printing practice; BedSplit does not prescribe a glue.

**Does it work on Windows and Linux?** It is pure Python. Tested on macOS only.

**Do I get updates?** Free updates for the 0.x and 1.x line.

**Can I get a refund?** Refunds follow the store policy of where you bought it. Write to support first if something does not work.


## 10. Support, compatibility and changelog

**Support:** support@dynamicflower.pt. Include your Blender version, your operating system, the lines of the Result panel (copy the text), the printer you chose, and if possible the model or a small .blend that shows the problem.

**Compatibility, as tested:** the test suite passes on Blender **4.2.23, 4.5.14 and 5.2.2 on macOS**. Exported parts were also checked by slicing them in PrusaSlicer. Windows and Linux were **not** tested.

**License:** GPL-3.0-or-later.

**Changelog**

- **0.1.3**: Date: 2026-11-13; Change: First public release: 18 printer presets and Custom size, Resize for print, Find Cuts with movable cut planes, repair of broken meshes, dowels and sockets, parts laid flat, Result checks, Export STL


There were no public releases before 0.1.3.
