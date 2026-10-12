# Tandem Robotic Arms vs. Gantry

## When a gantry fits

- Fixed, repetitive motion in a bounded volume (e.g., press-fit assembly, pick-and-place from racks).
- High volume, single task, no orientation changes needed.
- Cost-sensitive: gantry kits start around $250–$1,300 DIY.

## When arms fit better

- Multi-station lines where each station does a different thing.
- Parts need reorientation between steps (fill upright, label on side, QC from another angle).
- You want to add or remove stations without rebuilding the frame.
- Failure isolation: one arm down doesn't stop the whole line if you have buffer.

## Proposed line

```
[Arm 1: syringe fill + dispense] -> transfer point
[Arm 2: cap / crimp] -> transfer point
[Arm 3: label wrap] -> transfer point   <-- this repo
[Arm 4: QC (scan + weigh)] -> output rack
```

Each arm: one task, one gripper, one transfer point. Handoffs at fixed nests or short conveyor segments.

## Migration path

1. Build the wrapper station standalone (DIY, ~$300–$900) with a manual vial feed.
2. Add a scanner and ID allocator; verify blank-barcode flow.
3. Integrate with one arm; test handoff.
4. Add stations incrementally.

This is cheaper and lower-risk than designing the full line at once.
