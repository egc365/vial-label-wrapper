# Firmware

Placeholder. Intended contents:

- `wrapper_controller/` — main loop: arm command, belt speed, scanner read, pass/fail logic
- `id_allocator/` — unique ID generation service (SQLite-backed)
- `scanner_bridge/` — serial/USB bridge to barcode scanner
- `handoff_protocol/` — message format for tandem arm communication

Language TBD: Python (rapid prototyping) or C++ (real-time). Start with Python + pyserial.

## Handoff protocol sketch

Arms communicate over a simple message bus (MQTT or serial). Each station publishes its state; the next station subscribes.

```
station.wrapper.status: { "vial_present": true, "last_barcode": "VIAL-20261012-000001", "result": "pass" }
station.qc.status: { "vial_present": false, "last_action": "weigh" }
```

This keeps stations decoupled — adding a new station is just another subscriber.
