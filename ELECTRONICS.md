# IntelliWater — electronics schematic specification

## Reference-picture revision compatibility notice
The current CAD cabinet shows THREE flow sensors (main, kitchen, bathroom), while this preserved draft specifies ONE flow input. Two extra interfaces, connectors, MCU pin assignments, protection/filter networks and software channels remain UNDESIGNED. Do not treat the new mechanical controller/headers as a verified schematic or footprint layout. The STM32 model is retained from the original requirement rather than the photo's Arduino/ESP32 caption. The phone is a demo prop; no radio or live app is implemented. All prior ERC/DRC and unknown-conflict limitations below still apply.

**Status: preliminary connection design, not an EDA schematic or routed PCB. ERC/DRC have NOT been run. Do not fabricate from this document or the mechanical PCB model.** No previous schematic, PCB, netlist or full error report was attached. References below describe a new proposed design, not a verified repair of the old board.

## Architecture

```text
J1 regulated 12 V DC
  -> F1 resettable fuse -> D1 series reverse-polarity diode -> VIN_PROTECTED
  -> U2 TPS54302 buck + L1 -> +5V
  -> U3 LD1117S33TR -> +3V3 -> U1 STM32F103C8T6

J4 FLOW (5 V, GND, open-collector pulse) -> pull-up to 3V3 -> PA0
J5 LEVEL (5 V, GND, TRIG, ECHO) -> level conditioning -> PA1/PA2
PB0 -> R6 -> Q1 base; Q1 emitter -> GND; collector -> BUZZER_RETURN
+5V -> BUZ1 positive; BUZ1 negative -> BUZZER_RETURN
D3 cathode -> +5V; D3 anode -> BUZZER_RETURN
PA9/PA10 -> J2 UART; PA13/PA14/NRST -> J3 SWD
```

## Verified device references and proposed implementation

U1 is the **STM32F103C8T6, LQFP48**. Selected pin mappings are in `NETS_DRAFT.csv`; unused GPIOs remain unconnected, never shorted to ground merely to silence a check. The package pinout was checked against ST DS5319 Rev 20, Figure 8. The CAD body is nominally 7 x 7 mm; leads are simplified geometry, not a footprint. citeturn1view0

Use C10/C11/C12 = 100 nF at the three VDD pins, C13 = 10 uF bulk, C14 = 100 nF at VBAT, C15 = 100 nF and C16 = 1 uF at VDDA. Tie VBAT to +3V3 without a battery. Feed VDDA through FB1. This follows ST's decoupling guidance; placement remains to be validated. citeturn6view0

U2 **TPS54302DDCR** follows TI's 5 V reference network: 10 uH L1, 10 uF + 100 nF input, two 22 uF outputs, 100 nF bootstrap; feedback 49.9 ohm / 100 kohm / 13.3 kohm with 75 pF feed-forward; enable divider 511 kohm / 105 kohm. Keep BUCK_FB separate from all UART nets. Pin assignments: 1 GND, 2 SW, 3 VIN, 4 FB, 5 EN, 6 BOOT. These values are a reference starting point, not proof of stability in our layout. citeturn3view1turn6view1

U3 **LD1117S33TR**: pin 1 GND, pin 2 and tab +3V3, pin 3 +5V. Provide 100 nF input and at least 10 uF output; select the final capacitor technology/ESR and verify thermal margin. Assume a 100 mA maximum 3V3 design load initially. citeturn3view2turn3view3

Q1 **PMBT3904,215**: pin 1 base, 2 emitter, 3 collector. Proposed R6 = 1 kohm and R7 = 100 kohm base-emitter pull-down drive a candidate active 5 V buzzer limited to 20 mA; verify the actual buzzer current before release. citeturn5view0

## Interface assumptions requiring confirmation

- J4: 1 +5V, 2 GND, 3 FLOW_RAW. Sensor MUST have an open-collector/open-drain output compatible with a 3V3 pull-up. R10 = 10 kohm pull-up, R11 = 1 kohm series, C20 = 1 nF input filter. A push-pull 5 V sensor needs a different interface.
- J5: 1 +5V, 2 GND, 3 LEVEL_TRIG, 4 LEVEL_ECHO_RAW. Candidate ultrasonic interface only. R12 = 1 kohm trigger series. Echo divider R13 = 12 kohm / R14 = 18 kohm produces nominal 3 V from 5 V. Confirm 3V3 trigger recognition, range, blind zone, condensation tolerance and pin order. The probe shown is only a generic mechanical envelope.
- J2 UART: 1 GND, 2 MCU_TX, 3 MCU_RX, 4 +3V3 reference only. External adapter TX goes to MCU_RX. No RS-232 voltage levels or power backfeed.
- J3 SWD: 1 +3V3 reference, 2 GND, 3 SWDIO, 4 SWCLK, 5 NRST. UART is not a substitute for SWD debugging.
- BOOT0: 10 kohm pull-down plus normally open boot jumper to +3V3. PB2/BOOT1: 10 kohm pull-down. NRST: 10 kohm pull-up, 100 nF to ground, reset switch to ground.
- D1 series diode, F1 fuse, D2 input TVS and external-signal ESD protection need final manufacturer selections and coordinated ratings. Their pad numbers are intentionally NOT guessed.
- Use internal clock for initial firmware; UART bootloader operation and timing accuracy still need validation. No firmware is supplied or tested.

## PCB layout specification — not applied or checked

Proposed 110 x 110 mm, 1.6 mm FR4, two copper layers. Four 3.4 mm mounting holes at +/-48 mm from board centre match the CAD spacers. Reserve 5 mm copper keepouts around mounting holes. Initial rules: 0.25 mm signal tracks, 0.25 mm clearance, 1.0 mm low-voltage power tracks, 0.6/0.3 mm via pad/drill; revise for actual current, fabrication process and stackup. Place input capacitor/U2/inductor tightly together; keep the switching node small and away from FB, UART and sensor inputs. Use uninterrupted GND return and controlled copper-zone connections. Connector models in CAD are envelopes, not approved footprints.

Release sequence: capture real EDA schematic -> annotate -> validate every symbol/pad map -> ERC -> import netlist -> place -> route -> refill zones -> DRC -> inspect unconnected nets and all shorts -> review Gerbers/drills against board -> approve fabrication. No errors may be hidden or waived without a documented reason.

## Reported conflicts: disposition

| Prior report | Required separation / investigation | Status |
|---|---|---|
| +3V3 / PA14 at J3.1, JP2.36 | J3.1 reference supply; J3.4 SWCLK; U1.37 PA14 | New mapping defined; old copper unavailable |
| GND / PB14 / PB12 at D1.1, JP2 | U1.27 PB14 and U1.25 PB12 remain NC | Old copper unavailable |
| D3_K / BUZZER_DRIVE | D3 cathode +5V; PB0 only drives R6/Q1 base | New separation defined; not DRC-verified |
| BUZZER / PB10 at BUZ1.1, JP2.20 | BUZ1 positive +5V; PB10 U1.21 unused | Old mapping cannot be inferred |
| BUZ1_GND / PA3 / PA1 | BUZ1 negative is switched BUZZER_RETURN, not GND; PA1 trigger; PA3 unused | Not verified on previous board |
| BUCK_FB / UART_RX at R1.2, JP1.4 | U2.4 feedback; U1.31 UART_RX | Distinct proposed nets |
| Six additional conflicts | Need original DRC report with object IDs and native PCB/schematic | UNIDENTIFIED / OPEN |

**Release gate: BLOCKED.** Zero shorts has not been established. Gerbers, drill files and manufacturing export are intentionally not fabricated or represented as complete.
