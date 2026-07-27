# Troubleshooting Matrix — Centauri Carbon

Work the path: **symptom → most likely cause → cheapest fix first.** Don't change five things at once; you won't learn what fixed it. When the failure mode is ambiguous (warping vs. delamination vs. under-extrusion look different), pull a `printer_snapshot` if the MCP is connected — otherwise ask for a photo.

## First layer won't stick / lifts

1. **Dirty plate** (most common). Skin oils kill PEI adhesion. Wash the spring steel with dish soap and warm water; finish with IPA. Stop touching the print area with bare fingers.
2. **Nozzle too high.** Re-run auto bed leveling; adjust Z-offset in small steps (0.02–0.05 mm) until the first layer is squished, not stringy.
3. **First layer too fast / too cool.** Slow first layer to ~20–30 mm/s; bump first-layer temp +5°C.
4. **Wrong bed temp for material** — see materials.md.
5. **Worn/scratched PEI** — flip to the other side or replace the sheet.

## Warping / corner lift (mid-to-large parts)

Order of attack: raise bed temp → add brim (and draft shield for CF/ABS) → **close the chamber** (heat keeps lower layers from contracting) → reduce part cooling, especially the first 3 layers (set "no cooling for first N layers" = 3) → lower the auxiliary part-cooling fan to ~40–50%. PLA on this enclosed printer is the exception — its warping is rare and usually means *too much* enclosure heat, not too little.

## Stringing / oozing / wispy hairs

PETG and wet filament are the usual culprits. Fix in order: **dry the filament** → lower nozzle temp (temp tower) → enable z-hop → tune retraction (short, direct-drive values). Drying first — chasing strings with retraction while the spool is wet never works.

## Under-extrusion / gaps in walls / weak layers

1. **Flow too high a speed for the hotend** — you've exceeded volumetric flow. Lower speed or run the max-volumetric-speed calibration. This is common when pushing the 500 mm/s marketing numbers.
2. **Partial clog / heat creep** — especially PLA in the hot enclosure. Cold-pull or swap nozzle; open the door for PLA.
3. **Flow rate uncalibrated** — run flow calibration.
4. **Wet/old filament** — dry it.

## Layer shifts (print suddenly offset)

CoreXY shifts usually mean: belt slipped/loose, a collision (curled part or blob the head smacked into), or accel set absurdly high. Check belt tension, reduce acceleration toward ~10,000 mm/s², and make sure nothing is fouling the gantry. The printer auto-tunes resonance, so ringing ≠ shifting — a shift is a hard positional jump.

## Clogs / heat creep

On this printer, **PLA + closed enclosure** is the classic heat-creep clog: heat travels up the filament, softens it above the melt zone, and jams. Open the door/top for PLA. Otherwise: dry filament, correct temp, check for abrasive-worn or debris-packed nozzle.

## Poor overhangs / drooping

More cooling (raise fan, raise aux fan), lower temp slightly, slow down on overhang regions. On PLA, make sure chamber heat isn't softening the print — open it up.

## Ghosting / ringing (echoes after sharp features)

The printer's input shaping handles most of this. If it persists: lower acceleration, lower outer-wall speed (~150 mm/s), confirm frame is solid and feet are stable. Verify with the Voron/Orca calibration cube.

## Elephant's foot (bulged first layers)

Lower bed temp a few degrees, reduce first-layer flow slightly, or enable elephant-foot compensation (~0.1–0.15 mm) in Orca.

## Blobs / zits on the surface

Tune pressure advance, enable "wipe on retract," and adjust seam position/placement. Often improves automatically once PA is calibrated.

## CF parts: brittle, rough, popping/foaming

Moisture, almost every time. Dry PA-CF hard (70–80°C, 8–12h) and print from a dry box. Settings come second.

## TPU: jams, gaps, tangled extruder

Slow down (15–40 mm/s), kill retraction, lower acceleration. If it's slipping, the filament is too soft for the speed — drop speed further. Keep the spool from back-lashing/tangling on the side holder.

## Shipping/hardware damage (not a print issue, but worth ruling out)

Bent frames and cracked glass are known Centauri Carbon shipping casualties. If geometry is *consistently* off in one axis or the gantry feels rough, inspect the frame squareness and rails before blaming slicer settings.
