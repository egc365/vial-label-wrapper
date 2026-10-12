# Vial Label Wrapper

Open-source design for a robotic label wrapper station for **2 mL glass HPLC vials**.

Part of a tandem robotic arm assembly line for syringe/vial production. This repo covers the label wrapper — the most essential station — plus the concept of a blank barcode labeler where the machine assigns and tracks IDs.

## Why this exists

- Gantry-based automation is useful when the use case fits (fixed, repetitive, high-volume).
- For a multi-station line (filling, capping, labeling, QC), a series of **robotic arms** working in tandem is easier to design, debug, and extend. Each arm owns one single-variable task and pushes the part to the next station.
- The label wrapper is the hardest station: a 2 mL vial is only ~12 mm in diameter, labels must wrap a curved glass surface without wrinkles, and adhesive must bond to slick glass.

## The blank barcode labeler concept

Instead of printing variable data (lot, expiry, product name) onto each label at apply time, the machine applies **blank barcode labels** and assigns the ID itself:

1. A thermal transfer printer prints a unique barcode (Code 128 or DataMatrix) onto blank label stock.
2. The wrapper applies the label to the vial.
3. A barcode scanner reads the label back to confirm placement and orientation.
4. The machine's software maps that barcode to the vial's record: lot number, expiry, fill volume, operator, timestamp.

### Does a person need a physical label?

No — not if the barcode is the label. The barcode *is* the identifier. Human-readable text (lot, expiry) is optional and can be printed on the same label for audit purposes, but the machine never needs to read it. What matters is:

- The barcode is unique and scannable.
- The database record exists and is linked.
- ISO-compliant numbering is used where required (see `iso_numbering.md`).

This eliminates the variable-data printing problem at the wrapper station entirely. The printer becomes a separate, simpler station upstream.

## Hardware concept

### Label wrapper station

- **Vial handling:** vacuum gripper or soft rubber-fingered clamp on a 6-axis arm. 2 mL HPLC vials are standard labware; they sit in SBS-format racks.
- **Label application:** a wrap belt or roller station. The arm presents the vial to a spinning roller that presses the label onto the curved surface with even pressure. Alternative: a tamp applicator with a compliant pad.
- **Verification:** a fixed barcode scanner (or camera + vision) reads the label after application. Fail = reject bin.
- **Throughput target:** 15–40 vials/min (semi-auto) to start; scale with arm count.

### Tandem arm architecture

```
[Arm 1: fill] -> [Arm 2: cap] -> [Arm 3: label wrap] -> [Arm 4: QC scan/weigh] -> [output rack]
```

Each arm is a single-variable task. Handoffs happen at fixed transfer points (pallets, nests, or conveyor segments). This is easier than one big gantry because:

- Failure is isolated to one station.
- You can add or remove stations without re-engineering the whole line.
- Arms have reach and orientation flexibility that gantries lack.

## Cost reference (2026)

| Item | Range |
|------|-------|
| Semi-auto vial labeler (China, Alibaba) | $350–$1,000 |
| Desktop automatic vial labeler | $1,500–$3,000 |
| 6-axis arm (used/refurb, e.g. UR5e, Franka) | $10k–$30k |
| Thermal transfer printer (blank labels) | $200–$800 |
| Barcode scanner module | $50–$300 |

DIY gantry builds run $250–$1,300 but are better suited to press-fit assembly than curved-surface labeling.

## Repo structure

- `README.md` — this file
- `design_notes.md` — detailed design rationale, gripper and wrapper geometry
- `iso_numbering.md` — ISO-compliant numbering scheme for vial IDs
- `bom.md` — bill of materials
- `cad/` — placeholder for CAD files (Fusion 360 / FreeCAD)
- `firmware/` — placeholder for arm control and scanner integration code

## License

TBD — likely MIT or CERN-OHL-S for hardware.

## Status

Design phase. Hardware not yet built.
