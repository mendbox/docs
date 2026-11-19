<!-- Paste-ready for the Gumroad product description / content page. Tables are converted to lists. Replace https://mendbox.github.io/docs with the GitHub Pages address, no trailing slash. -->

# QuadFix 1.4.5: user manual

*Manual for QuadFix 1.4.5 (Blender add-on). Trailer: <https://youtu.be/h_6LSHC6sQY>*

Buy: [Superhive](https://superhivemarket.com/products/quadfix) | [Gumroad](https://mendbox.gumroad.com/l/quadfix) | Free Lite version: [GitHub](https://github.com/mendbox/quadfix-lite)

**Contents:** [1 What it does](#1-what-it-does) | [2 Install](#2-requirements-and-installation) | [3 Quick start](#3-quick-start) | [4 Reference](#4-reference-every-panel-button-and-setting) | [5 Worked examples](#5-worked-examples) | [6 Troubleshooting](#6-troubleshooting) | [7 Limitations](#7-known-limitations) | [8 Glossary](#8-glossary) | [9 FAQ](#9-faq) | [10 Support and changelog](#10-support-compatibility-and-changelog)


## 1. What it does

QuadFix repairs meshes in Blender. It closes holes with a grid of four-sided faces (quads), turns triangles and curved n-gons into quads, cleans up the mess a Boolean cut or an import leaves behind, bridges two edge loops with a clean strip of quads, and joins overlapping objects with a tidy quad seam. Select the problem, press one button, read the result in the panel.

![QuadFix before and after: a sphere with a hole is closed with a quad grid](https://mendbox.github.io/docs/assets/quadfix/gif_holes.gif)

**Who it is for**

- Game artists who clean up imported or AI-generated meshes (Meshy, Tripo, Hunyuan3D) before UV unwrapping or baking.
- Hard-surface modelers who need clean topology after Boolean cuts.
- 3D-print and CAD users who repair STL/OBJ imports and scans.
- Anyone who finds that Blender's *Tris to Quads* and *Grid Fill* do not finish the job.

**What it is not.** QuadFix repairs the mesh you already have. It is not an automatic retopology tool (it does not rebuild the whole edge flow like a remesher).


## 2. Requirements and installation

**Requirements**

- Blender 4.2 or newer (4.2 LTS, 4.5 LTS and 5.x). It is a Blender *extension*.
- No internet, no extra software, no compiled files. It uses Blender's own `bmesh`.
- The add-on is a **.zip** file. **Do not unzip it.**

**Install from disk**

1. In Blender open **Edit > Preferences** (on Mac: **Blender > Settings** also works, or press `Cmd+,`).
2. Click **Get Extensions** in the left list.
3. Click the small arrow (v) menu in the top-right corner of the window and choose **Install from Disk...**
4. Pick the QuadFix `.zip` file and confirm. On older 4.2 builds a short "online access" notice may appear; QuadFix itself never uses the internet.
5. The extension is added and turned on. If you see it in the list with an unchecked box, tick the box to enable it.

**Where is the panel?**

1. Move the mouse over the 3D viewport (the big window with your model).
2. Press `N` to open the sidebar (a panel on the right edge). Press `N` again to close it.
3. Click the tab named **QuadFix** (the tabs are written vertically at the right edge of the sidebar).
4. The tab holds three panels: **QuadFix** (the mesh tools), **Union (Object Mode)** and a footer called **QuadFix v1.4.5** that shows the last result.

Most tools need **Edit Mode** (select a mesh, press `Tab`). Union to Quads needs **Object Mode**. The buttons that do not fit the current mode stay visible but greyed out, with a hint at the top of the panel ("Edit Mode tools: press Tab" or "Object Mode tool: press Tab"). The panel is hidden in other modes such as Sculpt.

![The QuadFix tab in Edit Mode](https://mendbox.github.io/docs/assets/quadfix/small_edit.png)

**Update to a new version**

1. Download the new zip.
2. Do the same **Install from Disk** steps. Blender replaces the old version with the new one.
3. If the panel still shows the old version number in the footer, restart Blender.

Your settings are stored per scene (in the .blend file), so open files keep their option values.

**Uninstall**

**Edit > Preferences > Get Extensions**, find QuadFix in the list, open the arrow next to it and choose **Uninstall** (or remove it with the dropdown menu). Your models are not changed. Meshes that you already fixed stay fixed.

**Lite or full?**

- **Panel tab name**: QuadFix Lite (free, GPL): QuadFix Lite; QuadFix (full): QuadFix
- **N-gons to Quads**: QuadFix Lite (free, GPL): yes; QuadFix (full): yes
- **Clean & Quad (Boolean Prep)**: QuadFix Lite (free, GPL): yes; QuadFix (full): yes
- **Select Non-Quads & Holes**: QuadFix Lite (free, GPL): yes; QuadFix (full): yes
- **Fill Holes with Quads (+ Grid Span, Match Triangle Meshes)**: QuadFix Lite (free, GPL): no; QuadFix (full): yes
- **Bridge to Quads**: QuadFix Lite (free, GPL): no; QuadFix (full): yes
- **Union to Quads**: QuadFix Lite (free, GPL): no; QuadFix (full): yes


Lite and full can be installed side by side because the tabs have different names. This manual covers the full version; Lite has the three tools marked "yes".


## 3. Quick start

Try it on a Blender sphere with a few faces deleted.

1. Add a sphere (**Add > Mesh > UV Sphere**). Press `Tab` to enter Edit Mode.
2. Switch to Face select mode (press `3`), select a few faces and press `X` > **Faces** to make a hole.
3. Press `N`, open the **QuadFix** tab. Press `A` to select everything (QuadFix only touches the selection by default).
4. Press **Select Non-Quads & Holes**. The footer says, for example, "12 non-quad faces, 9 boundary edges". That is what needs fixing.
5. Press `A` again to select everything, then press **Fill Holes with Quads**. The footer reads "Filled 1 loop(s). Now: ... quads / ... tris / ... n-gons". Undo with `Cmd+Z` (Mac) or `Ctrl+Z` (Windows/Linux) if you do not like it.

![Panel clicks: select the problems, fill the hole](https://mendbox.github.io/docs/assets/quadfix/gif_panel_holes.gif)


## 4. Reference: every panel, button and setting

The panels appear in this order. A **report line** is the text in the footer panel (and in Blender's status bar). Every tool except *Select Non-Quads & Holes* can be undone with one `Cmd/Ctrl+Z`.

### 4.1 Panel "QuadFix" (Edit Mode)

- **Only Selected (checkbox)**: What it does: On: tools work only on the selected geometry. Off: tools work on the whole mesh.; When to use it: Leave on to fix one area without touching the rest. Turn off to fix everything without selecting.; Default / range: On
- **Select Non-Quads & Holes (button)**: What it does: Clears the selection, then selects every triangle and n-gon and every open boundary edge. Writes "N non-quad faces, M boundary edges" in the footer.; When to use it: First step: shows what is left to fix. Run it again after fixing to see that the numbers dropped.


A **boundary edge** is an edge that has a face on only one side, the border of a hole.

### 4.2 Box "Holes"

- **Fill Holes with Quads (button)**: What it does: Closes every closed boundary loop (hole) that is **fully selected** with a quad grid that continues the surrounding faces. Handles odd-sized loops, irregular holes, notched holes and band-shaped gaps.; When to use it: Holes from deleted faces, scan holes, ripped game models.
- **Grid Span**: What it does: The span value passed to Blender's grid fill (how many columns the grid uses). 0 lets QuadFix choose.; When to use it: Leave at 0. Try 1, 2, 3 only if the automatic grid looks too coarse or too fine.; Default / range: 0, whole numbers from 0 up
- **Match Triangle Meshes (checkbox)**: What it does: If most of the faces around the hole are triangles (a typical scan), the patch is kept triangular too, so it does not look out of place.; When to use it: Keep on for scans and triangulated meshes. Turn off if you want quads in the patch whatever the surroundings are.; Default / range: On


Selection rules for Fill Holes (important):

- A hole is only filled when **all** of its edges are selected (with *Only Selected* on). A half-selected hole is skipped and counted in the report.
- The outer border of a flat open sheet (a plane, terrain, cloth) is skipped on purpose: it is not a hole. The report says "N outer border(s) skipped".
- Every other closed boundary loop is filled, including openings you may not think of as holes (the eye sockets of a Suzanne monkey).

### 4.3 Box "Bridge"

The box shows the hint "Select two edge loops or chains".

- **Bridge to Quads (button)**: What it does: Builds a strip of quads between exactly two selected edge loops (closed rings) or two open edge chains. Loops with different numbers of edges are matched with quad "reductions" (up to 4:1). The result is checked for folds, twists and self-intersections and is taken back completely if a check fails.; When to use it: Connecting two holes (like a handle or a pipe joint), closing a gap between two rims, joining the end of a tube to another tube.
- **Auto Cuts (in "Adjust Last Operation")**: What it does: Adds loops between the two sides so the quads come out about square.; When to use it: Leave on.; Default / range: On
- **Cuts (in "Adjust Last Operation")**: What it does: Number of extra loops between the two sides. Only active when Auto Cuts is off.; When to use it: When you want a fixed number of rows. If the ratio needs more rows, QuadFix raises it and says so.; Default / range: 0, range 0 to 64
- **Smoothness (in "Adjust Last Operation")**: What it does: 0 = straight rows between the two sides. 1 = the rows follow the curvature of the surface around them.; When to use it: 0 for tubes and straight connections. Raise it when the two sides are tilted against each other, or to continue a curved surface. If the curved version fails the checks, QuadFix falls back to straight and tells you.; Default / range: 0.0, range 0.0 to 1.0


**Where do the three options appear?** They are not in the QuadFix tab. After you press **Bridge to Quads**, a small panel called **Bridge to Quads** opens at the bottom-left of the 3D viewport. If you cannot see it, use **Edit > Adjust Last Operation** (shortcut `F9`; on a Mac laptop you may need `Fn+F9`). Change a value and the result updates; the settings are remembered for the next bridge.

Selection rules for Bridge:

- Use **Edge select mode** (press `2`). In Vertex select mode, QuadFix ignores the inner edges and uses only open edges.
- Select exactly **two** separate loops or chains, both closed or both open. Edges must be open (boundary or wire edges), not edges that already have faces on both sides.
- Both sides need at least 3 edges when closed. A three-edge loop usually cannot take a quad strip; close it with Fill Holes instead.
- If the total number of edges on the two sides is odd, one triangle is unavoidable. It is selected together with the strip.

### 4.4 Box "Quads"

- **N-gons to Quads (button)**: What it does: Converts selected triangles and n-gons to quads. Curved n-gons are triangulated and joined again. Large grid-shaped n-gons (a dissolved patch of a grid) are opened like a hole and refilled with a proper grid.; When to use it: After a Boolean, after dissolving edges, on triangulated imports.
- **Face Angle**: What it does: The largest angle allowed between the normals (facing directions) of two triangles that are joined into a quad.; When to use it: Raise it to join more triangles across curved areas (scans). Lower it to keep creases.; Default / range: 90 degrees, range 0 to 180
- **Shape Angle**: What it does: How far a new quad may differ from a perfect rectangle.; When to use it: Raise it to get more quads from messy triangles. Lower it for regular quads.; Default / range: 90 degrees, range 0 to 180
- **Keep Planar N-gons (checkbox)**: What it does: Leaves flat n-gons (clean CAD faces, caps) untouched. Only curved n-gons and triangles are converted.; When to use it: Keep on for CAD and hard-surface parts. Turn off for scans, where flat n-gons are noise.; Default / range: On
- **Aggressive Quads (checkbox)**: What it does: Joins any two triangles into a quad whatever the angles. Fewest triangles left, but less regular quads.; When to use it: When you prefer "no triangles" over "nice shapes".; Default / range: Off


If everything selected is already quads (or flat n-gons with *Keep Planar N-gons* on) the button says "Nothing to convert (all quads or flat n-gons)". Report line: "n-gons A->B, tris C->D, quads E->F".

### 4.5 Box "Boolean / Import Cleanup"

- **Clean & Quad (Boolean Prep) (button)**: What it does: One click: merge doubles, remove degenerate and loose geometry, limited dissolve (removes extra edges on flat areas, only around triangles and n-gons), converts leftover triangles and n-gons to quads, recalculates normals. Clean quads and pole caps (the triangle fan at a sphere pole) are left alone.; When to use it: After a Boolean cut or after importing STL, OBJ, FBX, CAD or scan data.
- **Merge Distance**: What it does: Vertices closer than this are merged (a "double" is two vertices at the same place). Also used for removing degenerate geometry. Units are Blender units.; When to use it: Raise slightly for imports with tiny gaps. Keep tiny for precise models.; Default / range: 0.0001, minimum 0
- **Dissolve Angle**: What it does: Edges between faces that are flatter than this angle are removed.; When to use it: Raise to simplify more. Lower to keep gentle curves.; Default / range: 5 degrees, range 0 to 90
- **Keep Sharp Edges (checkbox)**: What it does: Never dissolves edges marked sharp or as seams.; When to use it: Keep on if you marked edges for shading or UV seams.; Default / range: On


Clean & Quad also uses **Face Angle, Shape Angle, Aggressive Quads and Keep Planar N-gons** from the Quads box. Report line: "verts A->B, quads ..., tris ..., n-gons ...".

### 4.6 Panel "Union (Object Mode)"

- **Union to Quads (button)**: What it does: Merges the selected objects into the active one with an exact Boolean union, then rebuilds the seam (the line where the shapes meet) as a strip of quads instead of triangles and n-gons. Needs Object Mode, at least two selected meshes, and an active mesh.; When to use it: Joining a sphere and a cylinder, a handle to a body, any two overlapping closed meshes.
- **Rings (in "Adjust Last Operation")**: What it does: How many face rings around the seam are rebuilt: Auto, 1, 2 or 3. Auto tries 1 to 3 and keeps the cleanest.; When to use it: Leave on Auto.; Default / range: Auto
- **Max Shape Change %**: What it does: Safety limit. A rebuilt strip that would move the surface by more than this percentage of the object size is rejected and that seam stays a plain Boolean.; When to use it: Raise it if many seams were "left as plain boolean". Lower it for stricter shape.; Default / range: 2.0, range 0.1 to 20
- **Keep Originals Hidden (checkbox)**: What it does: Hides the other objects instead of deleting them.; When to use it: Keep on. To get them back use **Object > Show/Hide > Reveal Hidden** (`Alt+H`).; Default / range: On


How to use: click the object you want to **keep** last (it becomes the active, brightest-outlined object), `Shift`-click the others, press **Union to Quads**. The active object receives the new mesh (the mesh is renamed with the ending `_union`). Materials of all objects are kept. The options live in **Edit > Adjust Last Operation** (`F9`).

Report line: "Union: N seam loop(s) rebuilt, M left as plain boolean. Mesh: quads A, tris B, n-gons C".

### 4.7 Footer panel "QuadFix v1.4.5"

Shows the report line of the last tool ("Last result: none yet" at start) and the version. Click the header to fold it.


## 5. Worked examples

### Example 1: Close a hole in a sphere

1. Create a UV sphere, delete a few neighbouring faces in Edit Mode (Face select mode, `X` > Faces).
2. Press `N`, tab **QuadFix**. Press `A` (select all).
3. Press **Fill Holes with Quads**.
4. Look at the result with **Select Non-Quads & Holes**: the boundary edge count is 0.

![A sphere with a hole before and after](https://mendbox.github.io/docs/assets/quadfix/holes_sphere_before.jpg)
![The same sphere after Fill Holes with Quads](https://mendbox.github.io/docs/assets/quadfix/holes_sphere_after.jpg)

Tip: a tilted band cut out of the sphere is rebuilt the same way.

### Example 2: Clean up after a Boolean cut

1. Cut a shape out of a cube with the Boolean modifier and **Apply** the modifier (**Object > Apply > Visual Geometry to Mesh** or the modifier's dropdown > Apply).
2. Go to Edit Mode, press `A`.
3. Press **Clean & Quad (Boolean Prep)**.
4. Check the footer: triangles and n-gons should drop. Press **Select Non-Quads & Holes** to see what is left.

![Boolean result before](https://mendbox.github.io/docs/assets/quadfix/boolean_cleanup_before.jpg)
![Boolean result after Clean and Quad](https://mendbox.github.io/docs/assets/quadfix/boolean_cleanup_after.jpg)

### Example 3: Triangulated scan to quads

1. Import the scan. Enter Edit Mode, press `A`.
2. Turn **Keep Planar N-gons** off (scans have no meaningful flat n-gons).
3. Press **N-gons to Quads**. Red faces in the solid-shading overlay are triangles left over; some are unavoidable on very irregular scans.
4. If too many triangles remain, raise **Face Angle** and **Shape Angle** a little or turn on **Aggressive Quads**, undo, and press again.

![Triangulated scan before](https://mendbox.github.io/docs/assets/quadfix/scan_to_quads_before.jpg)
![After N-gons to Quads](https://mendbox.github.io/docs/assets/quadfix/scan_to_quads_after.jpg)

### Example 4: Bridge two loops with quads

1. Make two open rings facing each other (for example two cylinders with the end caps deleted, or a hole in a wall and the open end of a pipe).
2. Edge select mode (`2`). Select the first ring (`Alt+click` an edge selects a loop). `Shift+Alt+click` selects the second ring.
3. Press **Bridge to Quads**. The report says how many quads, rows and reductions were made.
4. Open **Edit > Adjust Last Operation** and try **Smoothness** 0.5 if the rings are tilted.

![Bridge to Quads in the panel](https://mendbox.github.io/docs/assets/quadfix/gif_panel_bridge.gif)

### Example 5: Union of two objects with a quad seam

1. Object Mode. Place a sphere and a cylinder so they overlap.
2. Click the cylinder, `Shift`-click the sphere last (the sphere is active and will be kept).
3. Press **Union to Quads** in the **Union (Object Mode)** panel.
4. The sphere object now holds the joined mesh; the cylinder is hidden (`Alt+H` brings it back).

![Union to Quads sphere and cylinder](https://mendbox.github.io/docs/assets/quadfix/union_sphere_cylinder_pair.jpg)


## 6. Troubleshooting

### 6.1 Panel and buttons

- **I do not see the QuadFix tab**: Cause: The sidebar is closed, the mouse is not over the 3D viewport, or you are in a mode other than Object/Edit Mode.; Fix: Hover over the viewport, press `N`, look for the vertical tab named QuadFix. Return to Object or Edit Mode.
- **The buttons are greyed out**: Cause: The tool does not fit the mode. Mesh tools need Edit Mode, Union needs Object Mode.; Fix: Press `Tab`. The panel says "Edit Mode tools: press Tab" or "Object Mode tool: press Tab".
- **Union is still greyed out in Object Mode**: Cause: Fewer than two selected meshes, or the active object is not a mesh.; Fix: Select two or more mesh objects and make one of them active.
- **Nothing visible happens**: Cause: The result may be small.; Fix: Look at the footer report line.


### 6.2 Messages you can see

- **"No boundary edges (holes) in selection"**: Where: Fill Holes; Meaning and fix: Nothing selected is on a hole. Select the hole edges, or turn off *Only Selected*.
- **"Nothing to fill: N outer border(s), M partly selected hole(s)"**: Where: Fill Holes; Meaning and fix: The only loops found were the outer rim of an open sheet (skipped on purpose) or holes that are only partly selected. Select the whole hole loop.
- **"No holes"**: Where: Fill Holes; Meaning and fix: No usable hole was found in the selection.
- **"Filled N loop(s) (X outer border(s) skipped, Y partly selected hole(s) skipped). Now: Q quads / T tris / G n-gons"**: Where: Fill Holes; Meaning and fix: Success. The notes tell what was skipped.
- **"Nothing to convert (all quads or flat n-gons)"**: Where: N-gons to Quads; Meaning and fix: The selection has no triangles or curved n-gons. Turn off *Keep Planar N-gons* if you want flat n-gons converted.
- **"N non-quad faces, M boundary edges"**: Where: Select Non-Quads & Holes; Meaning and fix: Information. The faces and edges are selected for you.
- **"Bridge: select two edge loops or open edge chains (Edge select mode)"**: Where: Bridge; Meaning and fix: Nothing usable is selected.
- **"Bridge: N selected edge(s) already have faces on both sides: select open (boundary) edges"**: Where: Bridge; Meaning and fix: You selected inner edges. Select only the open rims. They are highlighted.
- **"Bridge: the selection branches at N vertex(es): select two simple loops or chains"**: Where: Bridge; Meaning and fix: A selected loop splits into branches. Select simple rings.
- **"Bridge: only one loop or chain is selected: select two (a single hole is closed by Fill Holes)"**: Where: Bridge; Meaning and fix: Select a second loop, or use Fill Holes for a single hole.
- **"Bridge: N separate loops or chains are selected: select exactly two"**: Where: Bridge; Meaning and fix: Deselect the extras.
- **"Bridge: a selected loop touches itself: select two simple loops or chains"**: Where: Bridge; Meaning and fix: The loop crosses or touches itself (figure eight). Select simple loops.
- **"Bridge: one side is a closed loop and the other an open chain: select two loops or two chains"**: Where: Bridge; Meaning and fix: Both sides must be the same kind.
- **"Bridge: a loop needs at least three edges"**: Where: Bridge; Meaning and fix: A closed loop of one or two edges cannot be bridged.
- **"Bridge: the two sides have A and B edges (R:1): at most 4:1 can be bridged with quads"**: Where: Bridge; Meaning and fix: One side has more than four times the edges of the other. Add edges to the small side (subdivide it) or fill part of the large side.
- **"Bridge: the two sides face opposite ways (recalculate their normals first: Mesh > Normals > Recalculate Outside)"**: Where: Bridge; Meaning and fix: Flipped normals. Fix them and try again.
- **"Bridge: a selected loop or chain has no length (...Merge by Distance)"**: Where: Bridge; Meaning and fix: Vertices sit on top of each other. Use **Mesh > Clean Up > Merge by Distance**.
- **"Bridge: no quad layout for A against B edges in N row(s): add cuts"**: Where: Bridge; Meaning and fix: Turn off Auto Cuts and raise Cuts, or turn Auto Cuts back on.
- **"Bridge: the two sides are already connected by faces"**: Where: Bridge; Meaning and fix: The loops are already joined.
- **"Bridge: a loop of three edges can not take a quad strip ... a triangular rim is better closed with Fill Holes"**: Where: Bridge; Meaning and fix: Use Fill Holes.
- **"Bridge: the strip folds / twists / ..." (the check that failed)**: Where: Bridge; Meaning and fix: The result was taken back; the mesh is unchanged. Move or rotate the loops, change Smoothness, or add Cuts.
- **"... (switched to Edge select mode to show the edges)"**: Where: Bridge; Meaning and fix: QuadFix changed to Edge select mode so you can see the problem edges.
- **"Bridge: N quads between A and B edges, R row(s), X reduction(s)"**: Where: Bridge; Meaning and fix: Success.
- **"... A+B edges in total is odd: one triangle is unavoidable (selected with the strip)"**: Where: Bridge; Meaning and fix: Normal. One triangle remains.
- **"... Cuts raised to N: R:1 needs M reduction rows"**: Where: Bridge; Meaning and fix: You set Cuts too low for the edge ratio, so QuadFix raised it.
- **"... Smoothness was dropped: following the surface ..."**: Where: Bridge; Meaning and fix: The curved strip failed the checks, so the straight strip was used.
- **"Boolean union produced no faces. Are both meshes closed and do they overlap?"**: Where: Union; Meaning and fix: Meshes with holes or no overlap cannot be unioned. Close holes first (Fill Holes) and make sure the objects overlap.
- **"The objects do not intersect: nothing was changed"**: Where: Union; Meaning and fix: The objects only touch or are apart. Move them so they overlap.
- **"Union: N seam loop(s) rebuilt, M left as plain boolean. Mesh: ..."**: Where: Union; Meaning and fix: If M is more than 0, those seams were too tricky or would change the shape more than *Max Shape Change %*. They stay as an ordinary Boolean seam.


### 6.3 Results that look wrong

- **Fill Holes did nothing to one hole**: Cause: The hole is partly selected, or it is the outer border of a flat sheet.; Fix: Select the complete loop. Use `Select > Select Loops > Select Boundary Loop` if needed.
- **Fill Holes also filled something I did not want filled**: Cause: Every fully selected closed boundary is filled (eye sockets, mouth).; Fix: Select only the loops you want, or undo and deselect them.
- **Leftover triangles after N-gons to Quads**: Cause: Irregular scans have regions that cannot become quads.; Fix: Raise Face/Shape Angle, or try *Aggressive Quads*. A few triangles are normal.
- **Union leaves seams as plain Boolean**: Cause: The strip would move the surface more than allowed, or the seam runs through a hole.; Fix: Raise *Max Shape Change %*, close holes first, or use similar mesh density on both objects.
- **Union result zig-zags**: Cause: Objects have very different mesh density.; Fix: Make densities similar (subdivide or decimate one object).
- **The original objects vanished after Union**: Cause: They are hidden (not deleted).; Fix: `Alt+H` in Object Mode, or **Object > Show/Hide > Reveal Hidden**.
- **Slow on a big mesh**: Cause: Pure Python tools.; Fix: Select only the area to fix. QuadFix was tested up to 200,000 faces; very dense meshes take longer.
- **I want to undo**: Fix: `Cmd+Z` / `Ctrl+Z`. One step per tool.



## 7. Known limitations

- QuadFix repairs the mesh you have. It does not rebuild edge flow like an auto-retopology tool.
- Some triangles are unavoidable on very irregular scans.
- Fill Holes fills every fully selected closed boundary loop and skips the outer border of flat open sheets and half-selected holes.
- Bridge needs exactly two simple loops or chains of the same kind, with an edge ratio of at most 4:1. A three-edge loop is better closed with Fill Holes. An odd total of edges leaves one triangle.
- Union to Quads rebuilds seams only. The new strip gets no UV coordinates. Seams that run through holes in the input stay a plain Boolean. On dense meshes with very different density on both sides the strip is all quads but zig-zags.
- Union works on the mesh data of the objects. If you rely on modifiers, apply them first (they are not part of the union).
- The tools do not create or preserve UV seams for the newly created faces beyond what Blender's operators keep. Check UVs after heavy edits.


## 8. Glossary

- **Quad:** a face with four corners. Quads deform and subdivide well.
- **Triangle (tri):** a face with three corners.
- **N-gon:** a face with more than four corners.
- **Triangle soup:** a mesh made only of triangles in a messy layout (typical for scans and AI meshes).
- **Boundary edge / hole:** an edge that has a face on only one side. A closed loop of them is a hole.
- **Edge loop / chain:** a ring (loop) or an open line (chain) of connected edges.
- **Boolean:** cutting or joining shapes with the Boolean modifier. It leaves triangles and n-gons at the seam.
- **Seam (here):** the line where two objects intersect.
- **Normal:** the direction a face points. Flipped normals face inside.
- **Planar:** flat. A planar n-gon lies in one plane.
- **Pole:** a vertex where many edges meet, such as the top of a UV sphere.
- **Manifold / non-manifold:** a manifold mesh is a clean closed surface. Non-manifold meshes have holes, loose edges or edges shared by more than two faces.
- **Merge by distance:** merging vertices that sit almost in the same place.
- **Limited dissolve:** removing edges between nearly flat faces.
- **Retopology:** rebuilding a mesh with a new clean layout.
- **UV:** the 2D coordinates used to put a texture on a mesh.


## 9. FAQ

**Why does Blender's Grid Fill fail on my hole?** It needs a closed loop with an even number of edges. Fill Holes with Quads handles odd loops and irregular outlines.

**Why does Tris to Quads (`Alt+J`) not convert all triangles?** It only joins neighbouring pairs within its angle limits and ignores n-gons. N-gons to Quads triangulates curved n-gons first and lets you set the angles.

**Is it a retopology tool or a remesher?** No. It repairs what is there.

**Can I use it on AI meshes from Meshy, Tripo or Hunyuan3D?** Yes for holes, doubles, triangle soup and a report of what is left. It will not rebuild edge flow.

**Does it work on Blender 5.x? On Windows and Linux?** Yes. Works on Windows, macOS and Linux (Blender 4.2 LTS and newer). Tested on Blender 4.2.23, 4.5.14 and 5.2.2 on macOS and Windows 11 (x86-64). It is pure Python.

**Does it need the internet?** No.

**Is it undo-safe?** Yes, one `Cmd/Ctrl+Z` undoes one button press.

**Does it keep UVs, materials, vertex groups, shape keys?** The tools edit the mesh with Blender's bmesh functions. New faces get no new UV data from QuadFix itself, and Union to Quads gives no UVs to the new strip. Materials are kept by Union. Check anything beyond that on a copy of your file first.

**Can I use it on a whole mesh without selecting?** Yes: turn off *Only Selected*.

**Do I get updates?** Free updates for the 1.x line.

**What is the difference between Lite and full?** See the table in section 2.

**Can I get a refund?** Refunds follow the store policy of where you bought it (Superhive or Gumroad). Write to support first if something does not work.


## 10. Support, compatibility and changelog

**Support:** support@mendboxstudio.com. Include your Blender version, your operating system, the report line from the footer (copy the text), and, if possible, a small .blend file that shows the problem.

**Compatibility, as tested:** the automated test suite passes on Blender **4.2.23, 4.5.14 and 5.2.2 on macOS and Windows 11 (x86-64)**. It runs each tool under vertex, edge and face select modes, with partial selections, checks undo, and includes a 200,000-face performance case.

**License:** GPL-3.0-or-later.

**Changelog** (dates are the commit dates of the tags; versions before 1.1.8 were development builds)

- **1.4.5**: Date: 2026-11-09; Change: Bridge to Quads: refits side B against the nearest points of side A; Smoothness continues the walls
- **1.4.4**: Date: 2026-11-09; Change: Bridge to Quads: angle smoothing stays inside its row, scale-free
- **1.4.3**: Date: 2026-11-08; Change: Clean & Quad leaves clean quads and pole caps alone
- **1.4.2**: Date: 2026-11-08; Change: N-gons to Quads refills grid-shaped n-gons like a hole
- **1.4.1**: Date: 2026-11-08; Change: Fill Holes splits notched grid holes into rectangles
- **1.4.0**: Date: 2026-11-08; Change: Union: reduction row behind dense loops, keeps staircase teeth
- **1.3.5**: Date: 2026-11-08; Change: Union: back to the narrow strip
- **1.3.4**: Date: 2026-11-08; Change: Union: wide strips win in seam crevices
- **1.3.3**: Date: 2026-11-07; Change: Union keeps body creases sharp through the strip
- **1.3.2**: Date: 2026-11-07; Change: Union absorbs vertex-count differences with quad reductions
- **1.3.1**: Date: 2026-10-31; Change: Union smooths the strip boundary loops along the seam
- **1.3.0**: Date: 2026-10-07; Change: Union seam strip matched by arc length
- **1.2.6**: Date: 2026-10-05; Change: Footer panel sorts last
- **1.2.5, 1.2.4**: Date: 2026-10-05; Change: Version and last result in a bottom panel
- **1.2.3**: Date: 2026-10-05; Change: All tools visible in both modes, greyed out when the mode does not fit
- **1.2.2**: Date: 2026-10-05; Change: Union to Quads: rejects folded strips; width and sliver penalty in Auto rings
- **1.1.8**: Date: 2026-09-28; Change: Smoother (biharmonic) hole patches. First public release
- **1.1.7**: Date: 2026-09-28; Change: Automated test suite on Blender 4.2/4.5/5.2 and the fixes it found
- **1.1.6**: Date: 2026-09-28; Change: Clean & Quad converts dissolve-created faces in face select mode
- **1.1.5**: Date: 2026-09-28; Change: Removes wire edges left after a reverted grid growth
- **1.1.4**: Date: 2026-09-28; Change: Grid rebuild of round holes on curved surfaces
- **1.1.3**: Date: 2026-09-28; Change: Clean converts dissolve-created n-gons; Fill matches triangle neighbourhoods
- **1.1.2**: Date: 2026-09-28; Change: Grid growth for all closed loops; fold detection reverts bad fills
- **1.1.1**: Date: 2026-09-28; Change: New grid vertices relaxed onto the surface
- **1.1.0**: Date: 2026-09-28; Change: Grid reconstruction for band holes
- **1.0.0 to 1.0.9**: Date: 2026-09-27; Change: First builds: Fill Holes, N-gons to Quads, Clean & Quad, Select Non-Quads & Holes

