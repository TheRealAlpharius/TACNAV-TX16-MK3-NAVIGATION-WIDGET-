# TACNAV

**A tactical GPS navigation aid for EdgeTX color-screen radios, built for Betaflight / acro FPV pilots.**

TACNAV turns the GPS telemetry your drone already sends back into a clean, glanceable navigation display: a radar-style compass, bearing and distance to waypoints and home, MGRS grids, waypoint entry, and an optional yaw-only steering assist.

> **TACNAV is a navigation aid, not an autopilot.** It shows the pilot where to fly and can nudge yaw toward a selected waypoint while an activation switch is engaged. The pilot always retains full authority over tilt, throttle, and flight mode. If you want autonomous flight, use a proper flight-controller autopilot such as ArduPilot — TACNAV is deliberately scoped as an assist for manual FPV navigation.

---

## Status

**Beta.** Developed and tested primarily in the EdgeTX Companion simulator. Real-hardware flight testing is the user's responsibility. Feedback and field reports are welcome — please open an issue.

---

## Features

### Radar-style compass (NAV page)
- **The drone is the center.** Waypoint and home dots are plotted by both **direction and distance** — fly over a waypoint and its dot slides to the center.
- **Auto-fit range**: the ring edge is automatically sized to your farthest waypoint, so everything is always on screen with no zoom needed. A range label shows the current scale.
- **North-Up or Track-Up** orientation (toggle in Settings). Track-Up rotates the whole picture so your heading is always at the top — a dot at the top means "dead ahead."
- Large readouts: distance-to-waypoint, distance-to-home, bearings in degrees, heading, altitude, speed, and your current position as an 8-figure MGRS grid.

### Waypoints (WPTS page)
- WP1–WP3, HOME, and a projected QUICK waypoint, each with lat/lon, MGRS, and live distance/bearing.
- **SEL** makes any waypoint the active nav target.
- **EDIT** opens the grid keypad to retype that waypoint.

### Waypoint entry (ENTRY page)
- **GRID KEYPAD** — type an exact MGRS grid with a smart per-position keypad (digits, then grid letters, then digits), and SET it to any waypoint.
- **QUICK MISSION** — project a waypoint from home (or your current position) along a bearing and distance. Set values by knob or by typing them on the keypad, watch the projected grid update live, then COMMIT and GO.

### Guidance (GUIDE page) — optional, experimental
- **Yaw-only** steering nudge toward the active waypoint. It points the nose; the pilot flies tilt and throttle. The flight controller's angle mode handles stabilization.
- **AUTO-BIND** an activation switch by tapping the button and flipping your chosen switch. Works with a momentary switch (hold-to-run dead-man) or a latching 3-position switch (flip to engage).
- Shows target, bearing error, distance to go, yaw command, and GPS/heading/moving preconditions.

### After-action log (AAR page)
- In-RAM position ring buffer plus CSV logging to the SD card for post-flight review.

### Diagnostics (DIAG page)
- Two-column raw telemetry readout for verifying sensor names and values, including pitch/roll and both battery voltages.

### Settings (SET page)
- Units (metric / imperial), night mode (red-shifted palette), compass orientation (north-up / track-up), and manual save/reload.

### Throughout
- **Persistence** — waypoints, home, bound switch, and preferences are saved to the SD card and restored on startup. All values remain fully editable at any time.
- **MGRS in and out** — forward (lat/lon → grid) and reverse (grid → lat/lon) conversion, verified accurate to under a meter.
- **Batteries** — drone (RxBt) and transmitter voltage on the status strip, color-coded.
- **Arrival alert** — a haptic buzz when you cross within a set distance of the active waypoint.

---

## Hardware and firmware requirements

- A color-screen EdgeTX radio with a touchscreen. Developed and tested on the **RadioMaster TX16S MK3** (800×480 touchscreen, STM32H7).
- **EdgeTX 2.11 or newer** — the LVGL widget UI and App-mode input focus require 2.11+.
- A GPS-equipped aircraft sending GPS telemetry to the radio (Betaflight, iNav, ArduPilot, Crossfire/ELRS — anything that populates GPS telemetry in EdgeTX).

---

## Installation

1. Copy the `WIDGETS/TACNAV/` folder to the root of your radio's SD card, so the script lives at `/WIDGETS/TACNAV/main.lua`.
2. On the radio, add or edit a screen, choose a full-screen widget layout, and set the widget to **TACNAV**.
3. Optionally turn off the system top bar for that screen to reclaim the full height.
4. Make the widget full screen.

### Activating touch input (App mode)

EdgeTX widgets only receive touch and key input when they have focus:

- **Long-press ENTER** while the widget is full screen. The EdgeTX logo switches to "FIX", indicating the widget now has input focus (App mode).
- **Long-press RTN / EXIT** to release focus back to the system.

Page navigation via the **S1 knob** works whether or not the widget has focus. On-screen buttons and keypads require App-mode focus.

---

## Telemetry sensor names

Discover these on the radio under **Model → Telemetry → Discover**. TACNAV expects:

| Purpose | Sensor name |
| --- | --- |
| GPS position | `GPS` |
| Altitude | `GAlt` |
| Ground speed | `GSpd` |
| Satellites | `Sats` |
| Heading | `Hdg` (falls back to `Yaw`, then GPS course) |
| Drone battery | `RxBt` |

If your setup uses different names, edit the `CFG` block at the top of `main.lua`.

---

## Optional: yaw steering setup

The GUIDE page can output a yaw nudge toward the active waypoint. It is **yaw-only** — it points the nose; the pilot flies everything else. To enable it on real hardware, add a single mixer line:

- **CH4 (yaw): `+= GV7`**, weight ~100%.

TACNAV writes the yaw correction to GV7 while the bound activation switch is engaged. Bind the switch from the GUIDE page (AUTO-BIND, then flip your chosen switch). Put the aircraft in **angle mode** first — the flight controller handles stabilization; TACNAV only supplies the slow navigation nudge.

If the activation switch is momentary, it behaves as a dead-man (assist only while held). If it is a latching 3-position switch, it stays engaged until flipped away. Either way, the pilot retains full manual authority, and loss of the assist never means loss of control.

---

## Configuration

Most behavior is adjustable in the `CFG` block at the top of `main.lua`:

- Telemetry sensor names.
- Steering gain, deadband, and minimum speed.
- Activation switch and threshold.
- Quick-mission bearing/distance sources and range.
- Waypoint cycle switch, arrival distance, and logging options.

---

## Known limitations

- **Steering is experimental and yaw-only.** It is an assist, not an autopilot. Test carefully and always keep full manual authority.
- The transmitter has no magnetometer. All bearings are computed from GPS coordinates (which are north-referenced), so no compass is required; heading is meaningful while the aircraft is moving.
- Track-Up orientation needs a valid heading to rotate by, so it holds the last heading when the aircraft is stationary (using GPS-course heading).
- Some vector/line rendering is limited by the EdgeTX Lua LVGL API on current firmware; the compass therefore uses positioned indicators rather than drawn needles.
- Interactive on-screen controls require App-mode focus (long-press ENTER).

---

## License

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.

## Disclaimer

This software is provided as-is, without warranty of any kind. It advises and can influence the operation of radio-controlled aircraft. You are solely responsible for safe, legal operation, including compliance with local regulations and airspace rules. Always maintain manual control authority and never rely on this software as a sole means of aircraft control.
