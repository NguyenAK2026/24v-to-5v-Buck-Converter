# 24V to 5V Buck Converter

A small step-down (buck) module that turns a 24 V supply into a regulated 5 V rail. It was built for a sim racing setup: the power supply has spare 24 V ports, and this board steps that down to power things that can't take 24 V, such as computer fans and extra 5 V electronics.

A PDF print of the schematic and PCB is in `24V_5V_Buck_Wheel.pdf`.

## Specs

| Item | Value |
|---|---|
| Input | 24 V DC (the regulator accepts 4.2 V to 65 V) |
| Output | 5 V DC, set to about 5.02 V by the feedback divider |
| Max output current | 2 A (regulator limit) |
| Regulator | Texas Instruments LMR36520FADDAR, synchronous buck, 8-pin SO PowerPAD |
| Connections | 2 screw terminal blocks (J1 input, J2 output) |
| Enable | Always on (EN tied to VIN) |

## How it works

- **J1** is the 24 V input. Pin 1 is +24 V and pin 2 is GND.
- **F1** (Bel Fuse 0ZCF0110AF2A, resettable PTC fuse) protects the input.
- **U1** (LMR36520) is the buck regulator. **L1** (Bourns SRP7028AA-100M, 10 µH) is the power inductor.
- **R1 / R2** (100 k / 24.9 k) set the output voltage. With the LMR36520's 1.0 V feedback reference, Vout = 1.0 V x (1 + 100 k / 24.9 k) = about 5.02 V.
- **R3** (100 k) pulls the power-good (PG) pin up to the output rail. PG isn't brought out to a connector.
- **J2** is the 5 V output. Pin 1 is +5 V and pin 2 is GND.

## Bill of materials

| Ref | Value | Part / note |
|---|---|---|
| U1 | LMR36520FADDAR | TI 65 V, 2 A synchronous buck, adjustable output |
| L1 | 10 µH | Bourns SRP7028AA-100M |
| F1 | PTC fuse | Bel Fuse 0ZCF0110AF2A |
| C1, C2 | 4.7 µF | Input capacitors |
| C3 | 220 nF | Input high-frequency bypass |
| C4 | 1 µF | VCC bypass |
| C5 | 100 nF | Bootstrap (BOOT to SW) |
| C6, C7 | 22 µF | Output capacitors |
| R1 | 100 k | Feedback divider, top |
| R2 | 24.9 k | Feedback divider, bottom |
| R3 | 100 k | PG pull-up |
| J1, J2 | Terminal block | Input and output |

## Board

- Designed in Altium Designer, with a ground pour on the PCB.
- The silkscreen reads "Test : Buck Converter v1", so this is a first-revision prototype.
- The last design rule check (`Project Outputs for 24V_5V_Buck_Wheel/`) reported 0 violations: clearance, shorts, unrouted nets and width rules all passed.

## Files

| File | Purpose |
|---|---|
| `24V_5V_Buck.SchDoc` | Schematic |
| `24V_5V_Buck.PcbDoc` | PCB layout |
| `24V_5V_Buck_Wheel.PrjPcb` | Altium project |
| `24V_5V_Buck_Wheel.BomDoc` | Bill of materials document |
| `24V_5V_Buck_Wheel.OutJob` | Output job configuration |
| `24V_5V_Buck_Wheel.pdf` | PDF print of the schematic and PCB |
| `History/`, `Project Logs for 24V_5V_Buck_Wheel/`, `__Previews/` | Altium auto-generated files |

## Things to check before building

- The input capacitors (C1 to C3) see the full 24 V supply. Use parts rated 50 V or higher, because ceramic capacitance drops a lot under DC bias. The schematic doesn't list voltage ratings.
- Check the PTC fuse's hold current in its datasheet against your load. At 5 V and 2 A the input draw is roughly 0.45 A, plus losses.
- Nothing in the project files shows this board has been tested. Check the output with a meter before connecting your electronics.
