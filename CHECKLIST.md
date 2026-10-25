# IntelliWater — validation and completion record

## Current reference-picture revision
- [x] Preserved previous assembly as `main.preReference.kcl`; original part files and electrical drafts unchanged.
- [x] Replaced stand layout with photo-derived wooden cabinet, two taps/basins/catch bins, right controller bay and phone-style demo prop.
- [x] Connected hollow main manifold and curved bends; real basin drains and shelf clearances.
- [x] Removed unused old-stand bodies from active assembly by using an isolated `reference/` catalog.
- [x] New part validation and assembly ON/OFF checks. Final ON: 46 local sketches fully constrained; OFF: 44; no reported constraint failures or execution warnings.
- [x] Front/top/right/isometric views and custom tank, tap/manifold and electronics close-ups reviewed.
- [x] Restored ON demonstration state; original `demo.kcl` contents preserved after OFF test.
- [ ] Transparent material: unavailable in previously tested rendering context; current tank is a labelled cutaway.
- [ ] Independent branch controls and measurements, live dashboard, autonomous animation and fluid simulation.
- [ ] Sensor wiring, real valve internals, seals, joining hardware, structural and leakage checks.
- [ ] Three-channel electronics redesign. Archived electronic proposal covers only the main flow sensor; extra branch inputs are not assigned or verified.
- [ ] Native EDA schematic/PCB, ERC/DRC, Gerbers/drills and six unspecified old short conflicts remain open. No electrical manufacturing export claimed.

- [x] Current reference revision exported as structurally validated `main.step` and delivered. Export includes cutaway tank and static ON water markers; not manufacturing approval.

## Historical single-tap revision record
The checks and earlier STEP export below apply to the archived design, not automatically to this reference revision.

## Completed
- [x] Backed up original entrypoint; preserved original project configuration.
- [x] Native multi-file separated assembly with elevated tank, inlet/outlet, inline sensor, level probe, tap and independent handle, open electronics enclosure, board, MCU and buzzer envelopes.
- [x] Part-by-part KCL execution; all reported sketches fully constrained. Final assembly review reports21 fully constrained local sketch instances; no under/over-constrained sketches or execution warnings. Imported parts were checked separately.
- [x] Actual tank outlet, inlet/lid holes, PCB mounting holes, enclosure cable ports and tap passages modeled with subtraction. Tap inlet and downward spout voids visually confirmed in oblique close-up.
- [x] Front/top/right/isometric review images plus targeted tank/tap and PCB images returned in chat. Native viewer remains editable; image files were not separately saved by these tools.
- [x] Leader labels and nominal size callouts; front view preferred for reading text. PCB top annotations identify MCU, buzzer, power, sensor, UART and SWD locations.
- [x] Cutaway review exposes blue water and sections of the outlet/sensor flow path while full part geometry remains available.
- [x] OFF visual check: lever across flow axis and no outlet stream; ON visual check: aligned handle and blue stream.
- [x] Low-flow sample:0.4 L/min,60s,0.4 L used, displayed64% level. High-flow sample:1.2 L/min,60s,1.2 L used, displayed44% level and thicker stream.
- [x] Alert sample:0.4 L/min,300s continuous/cumulative time,5-minute threshold -> red indicator and visual ALERT,2 L used, displayed24% level.
- [x] Same sample with10-minute threshold -> quiet status. This validates conditional CAD state evaluation only.
- [x] Full-wall tap-OFF STEP export structurally validated and delivered as `main.step`.
- [x] Saved mechanical/electrical draft BOMs, proposed connection schedule, schematic specification and software behavior notes.

## Open / blocked
- [ ] True material transparency: rendering service error; opacity-enabled branch unverified. Cutaway is NOT transparency.
- [ ] Autonomous animation, fluid dynamics, physical valve seals/cartridge, realistic drain/refill dynamics, calibrated sensor readings, firmware and audible buzzer behavior.
- [ ] Mechanical completion: enclosure lid, cable harnesses, attachment screws, pipe retention/sealing, structural/stability checks and leakage tests.
- [ ] Exact flow/level sensor selections, dimensions, electrical interfaces, minimum flow and sensor blind-zone checks.
- [ ] Native EDA schematic and generated netlist, complete PCB component placement/routing and all footprints.
- [ ] ERC, DRC, copper-zone refill, current/thermal/EMC validation. **NOT RUN. No zero-short claim.**
- [ ] Previous short-circuit repair verification: prior native design absent. Six unspecified conflicts remain UNIDENTIFIED; require full old DRC report.
- [ ] Gerber/drill files, verified PCB STEP, manufacturing release and public publishing link are not delivered.

The successful STEP file is a mechanical presentation exchange model. The green board and key electronics envelopes are a placement study, not a manufacturing PCB or complete circuit model. The water solids are visual markers, not manufactured parts.
