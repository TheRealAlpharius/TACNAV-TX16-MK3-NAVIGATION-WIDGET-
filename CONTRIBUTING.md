# Contributing to TACNAV

Contributions, bug reports, and field-test feedback are welcome.

## Reporting issues

When filing a bug, please include:

- Your radio model and EdgeTX firmware version.
- Which page/feature was involved.
- Your telemetry sensor names (from Model → Telemetry) if the issue is data-related.
- What you expected vs. what happened, and any on-screen error text.

## Development notes

- The entire widget is a single file: `WIDGETS/TACNAV/main.lua`. It is kept single-file on purpose to avoid EdgeTX module-loading and caching pitfalls.
- Configuration lives in the `CFG` block at the top of the file (sensor names, gains, switches, ranges).
- The UI is built with the EdgeTX LVGL Lua API. UI objects are created once per page and updated via getter functions; page changes rebuild the page.
- A few EdgeTX-Lua-specific constraints to keep in mind:
  - Use `string.sub(s, ...)` etc., not method syntax like `s:sub(...)` — the string metatable is not available.
  - `io.read(file, N)` reads N bytes, not lines; read in chunks and split manually.
  - Avoid float modulo (`%`) and float values passed to integer format specifiers; floor first.
  - Interactive LVGL controls (buttons, keypads) only work when the widget has App-mode focus (long-press ENTER).
- After editing, reload requires a full restart of the radio or simulator (Lua is cached hard).

## Testing

Please test changes in the EdgeTX Companion simulator before submitting. Note that steering output affects real aircraft control channels; validate any steering-related change carefully and never at the expense of manual authority.

## Safety

TACNAV is a navigation aid, not an autopilot. Any change that touches steering, channel output, or activation logic must preserve full pilot authority and fail safe (loss of assist, not loss of control).
