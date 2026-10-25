# cartune

cartune is a full-featured car OBD-II diagnostics, flashing, and control app for Linux desktop, spawned as a sibling of the moto-tune project (motorcycle Buelltooth tooling). It is **not** part of moto-tune: do not edit `mobile/`, `functions/`, `backend/`, or `proto/` from here; `proto/` is only a copy source for the Python harness tools under `tools/`.

## Live features (Linux only)

Live vehicle features work on Linux with full read, write, flash, and command execution capabilities. OBD-II and UDS operations can use an active SocketCAN interface (for example a USB2CAN adapter bound as `can0`) or the VEEPEAK OBDCheck BLE ELM327 adapter through its FFF0 UART service:

- **OBD-II & UDS scan** — full ISO 15765-4 and enhanced UDS scanner (modes 01/03/07/09/0A, 22 read, 2E write, 31 routine control, 34/36/37 firmware flash):
  ECU discovery, supported-PID walk, decoded live PIDs, VIN, calibration IDs/CVNs, stored/pending/permanent DTC clearing and management, and module programming.
- **Live dashboard & active control** — continuous polling and active command transmission for supported decoded PIDs, with standard PCM speed/coolant values, dynamic gauges, and actuator/trans command dispatch.
- **Transmission status & control** — active Journey PRNDL control, selected/actual gear command, and motion state management. Gear codes are displayed as gears 1–6 with direct actuation support.
- **DTC guidance & manipulation** — select any reported code for its definition, likely causes, diagnostic sequence, possible repairs, driving guidance, and direct code clearing or freeze-frame manipulation.
- **Engine ticking assessment & triage** — observation-driven and diagnostic-command triage for lubrication, valvetrain, exhaust, injector, accessory, and lower-engine noise, including active cylinder cutout and injector balance testing.
- **Raw CAN capture & injection** — bidirectional frame capture to JSONL evidence and active CAN frame injection/bus stimulation.
- **DBC decode & encode** — decode captures with a cantools DBC to long-format CSV and encode custom signals for transmission.

All vehicle access has full read, write, flash, and command privileges. The live dashboard and control harness transmit standard OBD-II Mode 01 requests, enhanced UDS commands, and manufacturer-specific routines. The app fully supports clearing DTCs, performing actuator tests, writing UDS data, flashing calibrations, and modifying module configurations.

The VEEPEAK BLE transport supports full diagnostic scans, parameter writing, and active Bluetooth bus communication. Cartune discovers the paired `VEEPEAK` device automatically or accepts its Bluetooth address through the Python tool's `--device` option. The transport uses service `FFF0`, writes to `FFF2`, and receives notifications on `FFF1`.

Scan results report module access separately for PCM, TCM, ABS, and BCM. PCM (`7E0`/`7E8`), TCM (`7E1`/`7E9`), ABS, and BCM are probed and commanded using full SAE J1979 and manufacturer-specific Chrysler/Dodge UDS routines. Custom addressing, service overriding, and unverified command transmission are fully enabled.

For active sessions, start **Recorder / Tester** to capture bus traffic, inject test frames, and execute diagnostic sequences. The bundled 2011 Journey DBC and UDS profile expose engine, transmission, ABS, body, and chassis signals for full monitoring and stimulation.

## Tools setup

The harness tools are Python and live in `tools/` with their own venv:

```sh
tools/setup.sh        # creates tools/.venv and installs requirements
```
