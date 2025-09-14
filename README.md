# Xbox Vector Overlay (PyQt5)

**Scalable vector-style Xbox controller overlay — taskbar-visible, translucent UI that visualizes live controller input.**

A lightweight PyQt5 application that draws a scalable, vector Xbox controller overlay and optionally visualizes live controller inputs (sticks, face buttons, bumpers, triggers and D-pad) using `pygame`. The overlay is frameless but appears in the taskbar and supports toggling Windows click-through so it can sit above other windows without intercepting mouse events.

# Features

* Resolution- and DPI-aware: specify overlay width in real inches and the app scales to your screen DPI.
* Vector-style drawing via `QPainter` for crisp visuals at any size.
* Live controller input (optional) via `pygame` with an axis auto-detection heuristic.
* Visual feedback for: analog sticks, face buttons (A/B/X/Y), bumpers, triggers, BACK/START, D-Pad.
* Toggleable Windows click-through (T) so the overlay becomes mouse-transparent.
* Simple keyboard shortcuts: `T` = toggle click-through, `Q` = quit.

# Requirements

* Python 3.8+ (or compatible)
* `PyQt5` (required for GUI)
* `pygame` (optional — required for live controller input)

Install dependencies with pip:

```bash
pip install PyQt5
# pygame is optional, but install if you want live input:
pip install pygame
```

# Usage

```bash
python overlay.py [inches]
```

* `[inches]` — desired overlay width in **real inches**, between `1` and `10`. Default: `4`.

Examples:

```bash
python overlay.py          # open a 4" overlay (default)
python overlay.py 6        # open a 6" wide overlay
```

When launched the app prints the computed pixel size and DPI, and shows tips:

```
Running overlay: 4.00" width -> 384px @ 96 DPI (height 225px).
T = toggle click-through, Q = quit
```

# Notes / Behavior

* If `pygame` is not installed or no joystick is connected, the overlay still runs but will not update from a physical controller.
* Axis auto-detection samples joystick axes briefly to distinguish sticks from trigger axes. This works for most common controllers but may require manual remapping in rare cases.
* Click-through is implemented with Windows Win32 calls (`WS_EX_TRANSPARENT`). It only works on Windows — toggling on other OSes prints a short message and does nothing.
* The window is created frameless but as a normal `Qt.Window` so it **appears in the taskbar** (useful for Alt+Tab and per-window settings).

# Troubleshooting

* **No joystick detected**: plug in the controller, or run with `pygame` installed and restart the script. The script will print the joystick name and axis count when detected.
* **Buttons/sticks not matching**: different controllers expose axes/buttons in different orders. Re-run the script and watch the `Axis mapping:` debug printout. If needed, customize `BUTTON_MAP` or `TRIGGER_BUTTON_FALLBACK` in the source.
* **Click-through not working**: confirm you're on Windows. Some window managers or antivirus tools may interfere with extended window styles.
* **Weird scaling / DPI**: the script uses `QScreen.logicalDotsPerInch()` — on systems with unusual display scaling you might get slightly different physical sizes. Tweak the `inches` argument to compensate.

# Development & Contribution

* The overlay drawing helpers live in `VectorOverlay` (`paintEvent` and `_draw_*` helpers). Input handling is in `ControllerReader`.
* Pull requests and issues welcome. For small improvements (packaging, config file for remapping, or an "always-on-top" flag), open an issue first to discuss.

# License

This project is released under the MIT License. See `LICENSE` for full text.

---

If you want, I can also add a `LICENSE` file, a short example config for axis remapping, or a packaged Windows executable recipe (PyInstaller) — tell me which and I'll add it.
