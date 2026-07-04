# Changelog

All notable changes to TACNAV are documented here.

## [1.0.0-beta] - 2026-07-04

First public beta.

### Navigation
- Radar-style compass: drone at center, waypoint and home plotted by both direction and distance; a target at zero distance sits at the center.
- Auto-fit range (ring edge = farthest waypoint) with an on-screen range label.
- North-Up and Track-Up compass orientation, toggleable in Settings and persisted.
- Distance-to-waypoint and distance-to-home with clear labels, bearings in degrees, heading/altitude/speed, and current-position MGRS grid.
- MGRS forward (lat/lon → grid) and reverse (grid → lat/lon) conversion, verified accurate to under one meter.

### Pages
- **NAV** — radar compass, distance/bearing readouts, current-position grid.
- **WPTS** — WP1–3, HOME, QUICK list with SEL (select active) and EDIT (keypad) per row; live distance/bearing.
- **AAR** — after-action position log (RAM ring buffer + CSV to SD).
- **GUIDE** — experimental yaw-only steering with switch AUTO-BIND; target/bearing-error/distance/yaw readouts and preconditions.
- **DIAG** — two-column telemetry diagnostics including pitch/roll and both battery voltages.
- **ENTRY** — GRID KEYPAD (type an MGRS grid) and QUICK MISSION (project a waypoint from home/position by bearing + distance, by knob or typed).
- **SETTINGS** — units, night mode, compass orientation, manual save/reload.

### Input
- Page navigation via S1 knob (works without focus).
- On-screen buttons and keypads active in App mode (long-press ENTER).
- Configurable steering activation switch: momentary (dead-man) or 3-position latch, bound via AUTO-BIND.

### Data
- Persistence of waypoints, home, bound switch, and preferences to the SD card, restored on startup and fully editable.
- Drone (RxBt) and transmitter battery voltage on the status strip, color-coded.
- Arrival alert (haptic buzz) within a configurable distance of the active waypoint.

### Notes
- Steering is yaw-only and requires one manual mixer line (CH4 += GV7). It is an assist, not an autopilot.
- Developed and tested primarily in the EdgeTX Companion simulator.
