# IntelliWater — prototype notes

## Current reference-picture revision
This section supersedes the single-tap dimensions and presentation settings in the historical notes below.
- `main.kcl` now follows the supplied photograph: wooden cabinet, elevated blue-lidded tank, main distribution sensor, two sensor/tap branches, metal kitchen tray, curved white bathroom bowl, two removable catch bins, right-side controller/LED/buzzer compartment and a phone-style demo display.
- Cabinet nominal size: 600 W x 360 D x 460 H mm, excluding 14 mm top, 620 x 370 mm plinth/top overhangs and tank. Highest inlet point is 705 mm. Dimensions are inferred design choices, not measured from the photograph.
- Previous assembly source is preserved as `main.preReference.kcl` (formatting compacted, geometry/settings retained). Original `parts/`, `demo.kcl`, project configuration, electrical CSVs and historical documents were preserved. New active part definitions are in `reference/` so the old design remains recoverable.
- Tank remains a labelled CUTAWAY, not transparent material. True transparency was previously rejected by this renderer. Set `cutawayReview=false` for the intact tank; water is then obscured by the wall.
- `demo.kcl` controls both handles together. ON shows two static streams; OFF rotates both handles and removes both streams. Water height retains the existing manually sampled total-use calculation. The displayed flow is TOTAL system flow, not flow per branch; no independent branch measurements or hydraulic split calculation is implemented.
- Main and both branch sensors are mechanical envelopes. The retained STM32 controller is intentional despite the reference's Arduino/ESP32 caption. Existing electronic drafts are SINGLE-CHANNEL historical proposals; two additional flow inputs, connectors and firmware channels still require design and validation.
- Phone screen is a CAD prop with annotation-based demo readings, not an app. Text is optimized for front viewing; labels may be occluded or foreshortened from other directions.
- New smooth bowl and hollow bends/manifold were validated individually. Current ON assembly executes with 46 fully constrained local sketch instances; OFF has 44. No under/over-constrained sketches or warnings were reported. Imported part sketches were checked separately.
- Front/top/right/isometric and targeted tank/plumbing/controller snapshots reviewed. Both ON and OFF states checked; final state restored to ON. Snapshot previews are not separate saved image files.
- Cabinet boards remain distinct named bodies; shaped components are separate part files. Existing dimension-driven helper sketches are code-editable, not point-and-click editable inside functions.
- Still a presentation prototype, not a fabrication release: no tested valve cartridge, seals, fastener schedule, structural verification, harness routing, watertightness, fluid simulation or connected hardware. Cabinet compartment is open for display, not a sealed electronics enclosure.
- `MECHANICAL_BOM.csv` describes the archived stand; `REFERENCE_BOM.csv` is the current presentation assembly inventory. Electrical BOM/net schedule remain draft and do not cover the extra branch channels.

## Historical single-tap revision notes

## Scope and preservation
- Original empty `main.kcl` is preserved verbatim at `backup/main.original.kcl`. Original `.gitignore`, `project.toml` and metadata were left unchanged.
- Native multi-file mechanical assembly; separate tank, lid, supports, plumbing, handle, sensors, enclosure, PCB, MCU, buzzer, basin and spacers.
- Approximate envelope: 440 x 440 x 465 mm. Tank OD180 mm, wall4 mm, internal height176 mm, gross cylindrical capacity about4.09 L; initial demo fill about3.02 L. Dimensions are proposed, not extracted from the photo.
- Four 18 mm square posts support the 210 x 210 x 15 mm shelf at z225. Structural loads, tipping resistance and fasteners have not been engineered or tested.

## Demonstration controls
Edit `demo.kcl`, then let the model regenerate:
- `tapOn`: true aligns the lever with the pipe and enables the water stream; false rotates the lever90 degrees and removes flow geometry.
- `highFlow`: false0.4 L/min, true1.2 L/min; the latter uses a thicker stream.
- `cumulativeOnSeconds`: manually sampled total ON-time at the selected constant flow. Changing only tapOn preserves the level. Keep this value unchanged while OFF. This is not persistent time integration.
- `continuousOnSeconds`: manually sampled duration of the current uninterrupted flow episode; reset to0 when starting a new episode.
- `thresholdMinutes`:5 or10, or another positive value. Alert is a visual status, not sound or tested firmware.
- Withdrawal is capped at a20 mm retained water depth. The stream stops when that modeled reserve is reached. At1.2 L/min this small reservoir cannot sustain a5-minute test; at0.4 L/min it cannot sustain10 minutes without refill. Use a larger supply or explicit refill test for hardware demonstrations.
- Invalid negative time, water-height and threshold inputs are not guarded; use sensible values. Changing flow mode recomputes the whole constant-rate sample, not a mixed-rate usage history.

## Presentation modes
- `main.kcl`: `cutawayReview=true` is a sectioned REVIEW view of the tank, outlet and flow-sensor housing. It does not redefine their intact part files.
- `cutawayReview=false` restores the complete tank and plumbing walls.
- `transparencyEnabled=false` is the validated setting. KCL exposes opacity, but this execution service failed with `SetOrderIndependentTransparency: Graphics have not been enabled for this geometry context`. The true-transparency branch remains an UNVERIFIED option; it may fail here. Do not describe the cutaway as transparent material.
- `showLabels=true` enables leader annotations, nominal dimensions and the demo-data readout. Labels are view-dependent; the front view is the preferred annotated presentation.
- No animation timeline, fluid simulation, interactive joint solver, sensor acquisition or live dashboard was produced. Handle motion is a parameterized rigid rotation. The tap is an envelope with real passages, not a designed valve cartridge or seal mechanism.
- Repeated dimension-driven profile helpers are code-editable; their internal sketches are not point-and-click editable.

## Mechanical and electronics limits
- Seals, pipe-retention/thread details, enclosure lid, cable harnesses, mounting screws and tap valve internals remain unfinished. The small tank and catch basin require supervision and spill management; no watertightness or drinking-water suitability is claimed.
- Flow/level sensor models are generic envelopes. Confirm vendor dimensions, connector assignments, pressure drop, low-flow range and ultrasonic blind zone before purchase.
- The visible PCB is a component-placement STUDY, not a copper layout. Only key component envelopes are modeled; the draft BOM contains additional passives/protection not represented individually.
- Electronics assumptions, schematic specification and known short-conflict dispositions are in `ELECTRONICS.md`. Connection schedule `NETS_DRAFT.csv` is not an EDA-generated netlist. `BOM_DRAFT.csv` is not a purchasing release.
- No ERC, DRC, zone refill, hardware construction, firmware testing, Gerbers or drill files. The six unspecified old PCB conflicts remain unidentified without the original design and full report.
- No publishing tool is available here; no interactive public link was created.

## Proposed software behavior — not implemented
Count calibrated flow pulses; integrate pulse-derived volume; measure and plausibility-check level; time uninterrupted nonzero flow independently of inferred tap state; use a no-pulse timeout based on the selected sensor's minimum pulse rate; reset the continuous timer after confirmed cessation. Expose threshold5/10 minutes over UART with bounds checking and nonvolatile storage. Drive buzzer and red/green indicator; report faults separately from zero flow. Actual tap position cannot be sensed from this hardware alone: dashboard hardware mode must say `inferred flow state` unless a tap switch/encoder is added. No radio is included; a web dashboard would need a UART gateway or additional communication hardware.

## Export
`main.step` was successfully exported in the full-wall, tap-OFF state, with the internal blue water marker included. It is an assembly presentation exchange model, not a fabrication approval; STEP does not carry the editable demo controls.
