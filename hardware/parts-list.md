# Mortise — Parts List

---

## Compute
- NVIDIA Jetson Orin Nano Super — 67 TOPS, 8GB RAM, JetPack 6.x

---

## Audio
- **Tang Band W3-881SJF** 3" full range drivers × 2 — speaker eyes, stereo, high-passed 150-200Hz
- **Dayton Audio RS125P-8** 5" paper/Kevlar woofer — sub orb, rear facing
- **Fosi Audio BT30D** — dehoused bare PCB, stereo amp
- **HiLetGo PCM5102** — I2S DAC board (Jetson I2S → PCM5102 → BT30D)
- Zobel networks on all three drivers *(not yet ordered)*

---

## Display
- **Waveshare 7" ~1080x600** IPS capacitive touch LCD *(confirm exact model/resolution)*

---

## Camera
- **Arducam 8MP IMX219** autofocus, 160° wide angle — CSI/FPC connection to Jetson CSI port

---

## Body Controller
- **Adafruit Circuit Playground Express** — USB serial to Orin, CircuitPython, built-in LEDs, built-in mic

---

## Inference Meter
- **Analog VU meter** (panel-mount, Amazon) — driven by CPX, shows thinking intensity (GAUGE:0-100 from Orin)

---

## Emotion Display
- **CPX built-in LEDs** behind diffused porthole bezel

---

## Head Movement
- **MG996R servo** — swivel only, offset mounted, linkage arm to lazy susan bearing, GPIO 32 PWM

---

## Encoders
- **Bourns PEC11R** × 2 — with push-button, walnut knobs turned from stock
  - Left ear: personality selector (GPIO 33/35/36)
  - Right ear: verbosity control (GPIO 37/38/40)

---

## Front Panel Controls
- Main power toggle (SPST latching, brass)
- LCD on/off power toggle
- WiFi/BT kill — keyed switch, brass, keys dangling
- Mute — latching pushbutton (amber)
- BT pair — momentary pushbutton (blue)
- Reboot — domed red momentary, emergency style (aesthetic but functional)
- Mic kill — latching pushbutton (red)
- Master volume — vertical sliding potentiometer
- BT30D amp controls (relocated under LCD):
  - Sub gain pot
  - Crossover pot
  - Treble pot
  - Bass pot
  - *(assess originals on BT30D arrival — reuse or replace with panel-mount)*

---

## Power
- Switched IEC inlet (single cable to wall)
- Wagos — split to two outputs
  - Output 1: Jetson OEM wall wart (Orin power)
  - Output 2: 24A PSU
    - 19V output → BT30D amp
    - 24V → 5V buck converter → accessories

---

## Lighting
- Neck ring LED — **on hold**

---

## Materials
- Walnut and maple — handcrafted, Rubio Monocoat finish
- Brass hardware throughout
- Sub orb: lathe-turned walnut sphere
