# Design Notes — Vial Label Wrapper

## Problem statement

Label a 2 mL glass HPLC vial (approx. 12 mm OD, 32 mm tall) with a self-adhesive barcode label, verify placement, and hand off to the next station. The label must wrap the curved glass surface without wrinkles or bubbles, and the adhesive must bond reliably to smooth borosilicate glass.

## Why the wrapper is the hardest station

1. **Small diameter.** A 12 mm vial means the label's inner radius is tight. Standard flat tamp applicators will crease the label on the curve.
2. **Glass surface energy.** Borosilicate is low-surface-energy; adhesive must be pressure-sensitive and rated for glass (most pharmaceutical label stocks are).
3. **Orientation.** The label must land in a consistent angular position so the barcode is scannable from a fixed reader angle.
4. **Verification.** A misapplied label (wrinkled, skewed, missing) must be caught before the vial leaves the station.

## Proposed solution: wrap belt + roller

Instead of trying to press a label flat onto a curve, use a moving belt that rolls the label around the vial as the vial spins:

```
        label stock
           |
     [peel plate] -----> label edge presented
           |
     [wrap belt]  <--- belt moves, presses label onto vial
           |
     [vial on spindle / held by arm gripper]
           |
     [barcode scanner]
```

- The arm (or a small rotary stage) spins the vial at controlled RPM.
- The wrap belt applies even pressure across the label width.
- A spring-loaded or pneumatic roller tamps the trailing edge.
- Scanner reads the barcode; software confirms match to assigned ID.

Alternative for low volume: a compliant silicone tamp pad on the arm's end effector, pressed with force control. Simpler but slower and less consistent on curves.

## Blank barcode labeler flow

1. Upstream thermal transfer printer prints blank-template labels: barcode only, no variable text. Print speed is not a bottleneck because the printer runs ahead of the wrapper.
2. Label roll feeds into the wrapper's peel plate.
3. Arm picks vial from input rack (SBS format), presents to wrapper.
4. Wrap belt applies label. Vial spins ~1–2 revolutions.
5. Scanner reads barcode. If match to database assignment -> pass. If no read or mismatch -> reject bin.
6. Arm places vial in output rack or hands to next station.
7. Software logs: barcode, lot, expiry, timestamp, station ID.

This decouples printing from application. The wrapper never needs to know what the label says — it only needs to apply and verify.

## Gripper design

- Vacuum cup (small, ~4–6 mm) on a compliant mount. 2 mL vials are light (~5 g empty) so vacuum force is minimal.
- Alternative: two soft rubber fingers (silicone, 30–40 Shore A) in a parallel gripper. Fingers must not contact the label area.
- Vial must be held by the body, not the cap, for label application.

## Open questions

- [ ] Label stock: what width and adhesive for borosilicate?
- [ ] Wrap belt material and tension control
- [ ] Scanner type: 1D (Code 128) vs 2D (DataMatrix) — DataMatrix is more compact for small vials
- [ ] ISO numbering scheme (see iso_numbering.md)
- [ ] Handoff mechanism between tandem arms
- [ ] Reject bin logic and rework path
