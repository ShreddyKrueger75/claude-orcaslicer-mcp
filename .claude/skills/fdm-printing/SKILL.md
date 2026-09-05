---
name: fdm-printing
description: Expert FDM 3D printing help tuned for the Elegoo Centauri Carbon (CoreXY, enclosed, 320°C hardened nozzle) running OrcaSlicer with Elegoo Link. Covers material selection, slicer settings and calibration, troubleshooting failed prints, and designing parts for printing. Use this whenever a print fails or misbehaves (warping, stringing, poor adhesion, clogs, layer shifts, weak parts), when choosing or dialing in a filament (PLA/PLA+, PETG, PA-CF/PET-CF, TPU), when asked "what temperature/settings/speed for X," when setting up or calibrating an OrcaSlicer profile, or when modeling/orienting/adding supports to a part for printing. Trigger it even when the word "print" isn't used — "why is this corner lifting," "is this filament okay for a bracket," "what clearance for a press fit," and "my first layer won't stick" all belong here.
---

# FDM Printing — Elegoo Centauri Carbon + OrcaSlicer

You are dialing in real prints on a real machine. Give concrete numbers, name the tradeoff, and don't hedge. Vague advice ("try raising the temp a bit") wastes filament and hours. Tie every recommendation back to what this specific rig can and can't do.

## The rig (this constrains every answer)

The Elegoo Centauri Carbon is a CoreXY FDM printer. Its hardware sets the ceiling on what's achievable, so anchor advice to these facts:

- **Build volume:** 256 × 256 × 256 mm. Parts larger than this must be split and joined.
- **Hotend:** all-metal, brass-hardened steel nozzle, 320°C max, ~32 mm³/s volumetric flow ceiling. The hardened nozzle means abrasive filament (CF, GF) is safe out of the box — but flow rate, not motor speed, is the real limit on print speed.
- **Bed:** PEI-coated spring steel (dual-sided), 110°C max.
- **Extruder:** dual-gear direct drive — handles flexibles (TPU) and abrasives well. Short filament path means retraction distances are small (~0.4–1.0 mm), unlike a Bowdens.
- **Enclosure:** fully enclosed chamber with chamber-circulation and auxiliary part-cooling fans, plus an air filter. The enclosure is an asset for ABS/ASA/nylon and a liability for PLA (heat soak → clogs and droopy prints). Manage the door/top accordingly.
- **Speed:** 500 mm/s and 20,000 mm/s² are marketing ceilings. Real quality settles at ~150–250 mm/s perimeters, 250–400 mm/s infill, and 80–120 mm/s for clean cosmetic surfaces.
- **Calibration on the machine:** auto bed-mesh leveling and resonance/input-shaping are handled by the printer. Per-filament flow, pressure advance, and temperature are NOT — you tune those in the slicer (see Calibration below).

**Slicer:** Use full OrcaSlicer with the Elegoo Link plugin for network upload, not the cut-down "Elegoo Slicer" fork (Elegoo strips out advanced settings). The stock Centauri Carbon 0.4 mm profile is a sane starting base; tune from there.

## Use the MCP when it's connected

This repo ships an `orcaslicer` MCP server. When it's connected, read the machine instead of asking the user to describe it — the real profile beats a remembered default, and a snapshot beats "can you describe what it looks like?"

| Instead of asking… | Call |
|---|---|
| "what profile are you using?" | `gui_project_state` (what's open in the GUI), then `get_profile` / `list_profiles` |
| "can you send a photo?" | `printer_snapshot` |
| "what's it doing right now?" | `printer_status` — normalized state, temps, layer progress |
| "tell me when it fails" | `watch_print` — bounded wait for a state change, target state, or error |
| "what temps did it actually run?" | `analyze_gcode` — parses the real `M109`/`M190`, not the profile's intent |
| "which printer/firmware is this?" | `printer_attributes`, or `printer_setup` if nothing is configured yet |

Calibration gets shorter too: `update_profile` edits a user preset and `slice_model` cuts the test print, so a temp tower or flow test is a couple of calls instead of a GUI walkthrough. System presets are read-only — clone to a user preset first.

**Don't let the tools outrun the human.** `start_print` heats hardware and can start a multi-day job: confirm before firing, and never infer `plate_cleared` — that flag means a person actually looked at the plate. If the last job finished, the part is probably still sitting on it.

**CC1 limit:** the Centauri Carbon's firmware can't list or delete files or report free space over SDCP, so `printer_files` comes back empty and storage is managed on the touchscreen. Diagnose from `printer_status` and snapshots instead.

## How to route a request

Most questions fall into one of four buckets. Identify which, then go deep:

1. **Material selection** — "which filament for X," "is PETG okay here," "what's the strongest option." Match material to the part's job (strength, heat, flex, cosmetics, outdoor), then read `references/materials.md` for the full per-filament profile (temps, cooling, plate, drying, gotchas).
2. **Slicer settings & calibration** — "what settings," "my profile," "how do I calibrate." Use the Calibration section below and `references/materials.md` for per-material starting points.
3. **Troubleshooting** — a print failed or looks wrong. Work the symptom → cause → fix path. The most common cases are below; the full matrix is in `references/troubleshooting.md`.
4. **Design for printing** — modeling, orientation, supports, tolerances, fits. Use the Design section below.

When a question spans buckets (it often does — "my CF bracket is warping and weak"), handle material + troubleshooting + design together.

## Calibration (do this once per new filament)

Skipping calibration is the single most common cause of "the printer is broken" complaints that aren't actually the printer. Per-filament, run these in OrcaSlicer in this order — each depends on the previous:

1. **Temperature tower** — find the lowest temp that still gives clean layer bonding. Lower temp = less stringing and better detail; too low = weak, gappy layers.
2. **Flow rate** (pass 1, then pass 2) — corrects over/under-extrusion. Do this before pressure advance; PA assumes flow is correct.
3. **Pressure advance** — sharpens corners, kills bulging seams and under-extruded corners. Direct drive uses small PA values.
4. **Max volumetric speed** — find where the hotend can't melt fast enough (under-extrusion appears). This is the true speed ceiling, ~32 mm³/s max on this hotend; real filaments cap lower (PLA ~20–24, PETG ~12–16, PA-CF ~8–12).
5. **Retraction** — last, to clean up stringing the temp tower didn't. Keep distances short (direct drive).

The Orca calibration cube / Voron cube is the standard sanity check for ghosting and dimensional accuracy after calibrating.

## Design for FDM (functional parts)

FDM parts are anisotropic — strong in the X/Y plane, weak between layers (Z). Almost every design rule follows from that:

- **Orient for strength.** Layer lines are the failure plane. Lay a part so load pulls *across* layers, not *peels them apart*. A hook printed flat (load along layers) is far stronger than one printed upright.
- **Overhangs:** clean to ~45–50° from vertical. Beyond that, add supports or redesign. Chamfer (45°) the bottom edge of a part instead of filleting it to avoid supports.
- **Bridges:** unsupported spans up to ~20–30 mm print fine with good cooling; longer needs support or a redesign.
- **Holes shrink.** Printed holes come out undersized (the inner perimeter pulls in). Model vertical holes ~0.1–0.2 mm oversize, or use Orca's hole compensation. For press fits, design the clearance in.
- **Fits & clearances:** ~0.2 mm clearance for a snug sliding fit, ~0.3–0.4 mm for a free/clearance fit, ~0.1 mm or interference for press fits. Test on the actual filament — PETG and TPU behave differently than PLA.
- **Minimum feature size:** ~0.8 mm (≈2× nozzle width). Thinner walls won't print reliably.
- **Walls beat infill for strength.** For functional parts, add perimeters (3–5 walls) before cranking infill above ~30%.
- **Split big or awkward parts** and design alignment features (pins, pockets) plus glue/fastener joints. Use the full 256³ but respect warping risk on large flat CF/ABS parts.

## Style of response

Lead with the fix or the number. Give the reasoning in one line so the person learns the *why*, not just the *what*. When there's a tradeoff (cooling vs. layer adhesion on PETG, chamber heat vs. PLA clogs), state both sides and pick a default. If a question needs info you don't have, fetch what the MCP can give you (profile, status, snapshot) and ask only for what it can't — filament brand, what the part is for, how it failed in the hand. One question that actually changes the answer; don't interrogate.
