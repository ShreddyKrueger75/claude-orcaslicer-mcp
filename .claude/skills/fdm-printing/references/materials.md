# Material Profiles — Centauri Carbon

Starting points, not gospel. Always calibrate per spool (see SKILL.md Calibration). Brands vary; a "230°C PETG" from one maker wants 245°C from another. These ranges assume a 0.4 mm nozzle.

Quick selection:
- **PLA / PLA+** — prototypes, cosmetic parts, indoor/low-stress. Easiest, stiffest-feeling, but creeps under load and dies in a hot car (~50°C glass transition).
- **PETG** — functional parts needing toughness, mild heat, some chemical/water resistance. The everyday workhorse for things that have to survive.
- **PA-CF / PET-CF** — stiff, dimensionally stable, heat-resistant engineering parts (brackets, jigs, mounts). This is what the hardened nozzle and enclosure are *for*. Demands dry filament and patience.
- **TPU** — flexible parts (gaskets, bumpers, grips, vibration damping). Slow but the dual-gear direct drive handles it well.

---

## PLA / PLA+

| Setting | Value | Why |
|---|---|---|
| Nozzle | 200–220°C (PLA+ 210–225°C) | Calibrate with a temp tower; PLA+ runs slightly hotter |
| Bed | 55–60°C | Plenty for PEI |
| Plate | Textured PEI | Easy release; smooth PEI works too |
| Cooling | 100% | PLA wants maximum cooling for detail/overhangs |
| Aux fan | High on small/overhang parts | Helps bridging and steep overhangs |
| Speed | Most forgiving — push it | Volumetric flow ~20–24 mm³/s is the real cap |
| Drying | Usually none; 45°C/4h if old/brittle | Mildly hygroscopic |

**Centauri Carbon gotcha:** the enclosure works *against* PLA. Trapped chamber heat softens filament before the nozzle (heat creep → clogs) and makes overhangs droop. **Crack the door / leave the top open for PLA.** If you see mid-print clogs or mushy detail on tall PLA prints, chamber heat soak is the first suspect.

## PETG

| Setting | Value | Why |
|---|---|---|
| Nozzle | 230–250°C | Calibrate; too cool = poor layer bonding |
| Bed | 70–80°C | |
| Plate | Textured PEI | PETG bonds *too well* to smooth PEI — it can tear the coating |
| Release | Glue stick as a *release* agent on smooth PEI | Counterintuitive: glue prevents PETG from fusing to the plate |
| Cooling | 30–50% | The key tradeoff: more cooling = better overhangs but weaker, warp-prone layers; less = stronger but stringier/droopier |
| Speed | Slower than PLA; flow ~12–16 mm³/s | |
| Z-hop | On (~0.2–0.4 mm) | Reduces stringing and blobs |
| Drying | 65°C / 6–8h | Hygroscopic — wet PETG bubbles, hisses, and strings badly |

**Stringing is PETG's signature problem.** Attack it with: dry filament first, then lower temp (temp tower), then retraction + z-hop. Don't crank retraction before drying — you'll chase a moisture problem with the wrong tool.

## PA-CF / PET-CF (carbon-fiber filled)

| Setting | Value | Why |
|---|---|---|
| Nozzle | 260–290°C (within 320°C ceiling) | PET-CF lower end, PA-CF higher |
| Bed | 80–100°C | High adhesion needed; these warp aggressively |
| Chamber | Closed and warm — this is what the enclosure is for | Reduces warping and layer delamination |
| Plate | Textured PEI or engineering plate + glue | |
| Cooling | Low (0–30%) | CF/nylon wants slow cooling for layer bonding; too much fan = delamination |
| Speed | Slow, low flow (~8–12 mm³/s) | |
| Adhesion aids | Brim (5–8 mm) + draft shield | Fights corner lift |
| Drying | **Mandatory.** 70–80°C / 8–12h, ideally print from a dry box | Nylon is extremely hygroscopic — wet PA-CF pops, foams, and prints weak and ugly |

**Drying is not optional for nylon-based CF.** "My PA-CF prints are brittle/rough/popping" is almost always moisture, not settings. The hardened nozzle handles the abrasive CF, but it *will* wear over hundreds of hours — expect to replace it eventually. CF blends are stiff but **not** automatically stronger in the layer (Z) direction; orientation still rules.

## TPU (flexible)

| Setting | Value | Why |
|---|---|---|
| Nozzle | 220–235°C | |
| Bed | 35–50°C | |
| Plate | Textured PEI | |
| Cooling | Moderate (30–60%) | |
| Speed | **Slow: 15–40 mm/s** | The hard rule — TPU buckles and jams if pushed |
| Retraction | Minimal / short, no z-hop | Stretchy filament + long retraction = jams and gaps |
| Acceleration | Lower it | Avoid sharp speed/direction changes |
| Drying | 55°C / 4–6h | Hygroscopic |

**Softer = harder to print.** 95A TPU is forgiving; below ~90A it gets fussy and can slip in the extruder. The dual-gear direct drive is why this printer can run TPU at all — but it still demands patience and low speed. Disable or heavily limit retraction first if you're getting jams.
