# Quick fixes for the live pages (audit 2026-11-16)

Exact text changes. Nothing here has been applied.

## 1. BedSplit, Gumroad (and Superhive, check the same line)

**Printer presets: 16 -> 18.** The code has 18 presets. The Gumroad line ends at "Elegoo Neptune 4." Replace the whole "Printer presets" bullet with:

> Printer presets: Bambu Lab A1 mini (180x180x180), A1 / P1 / X1 (256x256x256), H2D (350x320x325); Prusa MINI+ (180x180x180), MK4 / MK3 (250x210x220), XL (360x360x360), CORE One (250x220x270); Creality Ender 3 / V3 SE (220x220x250), K1 (220x220x250), K1 Max (300x300x300), Ender 5 Plus (350x350x400); Anycubic Kobra 2 (220x220x250) and Kobra 3 (250x250x260); Elegoo Neptune 4 (225x225x265) and Neptune 4 Max (420x420x480); Sovol SV06 (220x220x250); Voron 2.4 300 (300x300x280) and Voron 0.2 (120x120x120). Or type your own bed size.

Add "18 presets" to the heading line if there is room.

**Video.** Add under the title or as the first line of the description: `Trailer: https://youtu.be/-EbloCRxfUs` (take it from the live Superhive page, it matches the Lite README).

**Free updates wording.** Gumroad says "Free updates for 0.x and 1.x." Use the same wording on Superhive (see section 5).

**Platforms.** The page says "works on Windows, macOS and Linux". Replace with: "Pure Python on Blender's own tools, no external binaries. Tested on macOS; Windows and Linux should work but are not tested yet."

**Manual link.** Add: "Full manual with screenshots: BASE_URL/bedsplit/".

## 2. Image Per Instance, Gumroad

- Add the video line: `Trailer: https://youtu.be/CLFAlI3Aa5c` (same as Superhive and the Lite README).
- Platforms: replace "Pure Python: no binaries, no network, works on macOS, Windows and Linux." with "Pure Python: no binaries, no network. Tested on macOS; Windows and Linux are not tested yet."
- Replace "Free updates. Questions: reply to your receipt email or write to support@dynamicflower.pt." with the unified line in section 5.
- Superhive Documentation tab is at 0.1.x: replace it with the full manual (`image-per-instance/superhive_docs.md`).
- Manifest `website` points to the GitHub org page: point it to BASE_URL/image-per-instance/ in the next release.
- Add: "Full manual with screenshots: BASE_URL/image-per-instance/".

## 3. QuadFix

- **Bridge to Quads description is wrong on Gumroad (and check Superhive About/Docs/FAQ).** Live text: "Set the number of Cuts, Smoothness, Twist and a flip option". The code has only **Auto Cuts, Cuts and Smoothness**. There is no Twist and no flip option. Replace with: "Set Auto Cuts (or the number of Cuts) and Smoothness in the Adjust Last Operation panel."
- **Bridge minimum loop size.** Live text: "Bridge to Quads needs loops of at least four edges". The code accepts closed loops of three edges but a three-edge loop almost never takes a quad strip, and QuadFix then says to use Fill Holes. Suggested: "A three-edge loop is better closed with Fill Holes." Also state the limit that is missing everywhere: the two sides may differ by at most 4:1 in edge count.
- **Tested-on wording (FAQ).** Superhive FAQ answer says "on macOS" while About says Windows, macOS and Linux. Use one sentence everywhere: "Tested on Blender 4.2.23, 4.5.14 and 5.2.2 on macOS. Pure Python, so it should run on Windows and Linux too, but those systems are not tested yet."
- **Free updates wording.** Unify (section 5).
- **Support channel wording.** Gumroad: "reply to your receipt email". Superhive: "message me here". Use on both: "Questions: support@dynamicflower.pt (Superhive buyers can also send a message here)."
- **Version History on Superhive** starts at 1.4.3. Add one entry "1.0 to 1.4.2" or the table from section 10 of the manual (`quadfix/superhive_docs.md`).
- **Superhive Documentation tab:** replace with the full manual (adds Bridge to Quads, option defaults and ranges).
- **Local `quadfix/README.md`** says 1.2 and lacks Bridge: replace with this manual (short form).
- Add: "Full manual with screenshots: BASE_URL/quadfix/".
- Local `quadfix/gumroad/description.txt` is the old 1.2 text (no Bridge). The live Gumroad page is already newer. Do not paste the local file.

## 4. Support line (all pages)

The Superhive page shows the Cloudflare-obfuscated email. Write the address in plain text in the description: support@dynamicflower.pt. Add: "Please include your Blender version, operating system and the report line from the panel."

## 5. Unified "free updates" line

> Free updates: QuadFix 1.x, Image Per Instance 0.x and 1.x, BedSplit 0.x and 1.x.

Use exactly this on Gumroad, Superhive, READMEs. Today: QuadFix Gumroad "Free updates", Superhive and README "Free updates for 1.x"; IPI "Free updates"; BedSplit "Free updates for 0.x and 1.x".

## 6. Code and manifest items (need a release, not a text edit)

- Add `website = "<manual URL>"` to the manifests of QuadFix and BedSplit; change IPI's `website`.
- BedSplit screenshots in `bedsplit/marketing/panelgif/knight_result.png` show "BedSplit v0.1.1" in the Result header. Re-shoot if used in the listing (the manual includes only the panel GIF and the knight images).
