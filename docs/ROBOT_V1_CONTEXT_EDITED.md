# ROBOT V1 — PROJECT CONTEXT
*Updated: April 2026 — FINAL v3*

## What This Is
A handcrafted walnut and maple AI desk companion robot. Jason's personal unit. Daily companion, development platform, embodied AI proof of concept. V2 is a WALL-E form factor for nephew (Christmas 2026) — not yet specced.

## Name
**Mortise** — named after a woodworking joint. Precise, dry, craftsman. He has four selectable personalities and a physical thinking gauge.

## Design Philosophy
V1 packs the tech in and makes it work. Serious music speaker, daily AI companion, YouTube and podcast player, and a showcase of what happens when hardware experience meets AI. Not a product prototype — a passion project and learning platform. The software (personality engine, emotional states, embodied AI interaction).

## Form Factor
Retro humanoid robot. Wider head than torso. Walnut and maple, handcrafted, Rubio Monocoat finish. Speaker eyes. Retro mismatched switch and gauge panel. WiFi antenna pigtails in brass tube ear housings. Stationary desk unit. Single IEC inlet on base platform rear.

## Physical Architecture — Three Piece System
**Mortise (torso + head)** — compute, audio electronics, display, controls, servos
**Base platform** — walnut and maple box, houses both PSU bricks, vented for heat, single IEC inlet, Mortise feet bolt to base
**Sub orb** — TO BE ASSESSED AS PARTS ARRIVE- we need to evaluate if we can fit all the electronics + sub in the torso and keep it cool. If not, we pivot to external lathe-turned walnut sphere housing RS125P-8, sealed or vented, sits beside or behind Mortise, speaker wire connects to base/torso.

## Torso Contents
- Orin Nano Super dev kit
- MC351 dehoused bare PCB (amp + DAC + VU drive circuit)
- Buck converter 19V→5V 5A
- MG996R servo + lazy susan assembly (top)
- Circuit Playground Express (chest, heart porthole)
- Wiring runs

## Base Platform Contents
- 19V 8A PSU brick (Orin supply)
- MC351 32V PSU brick (amp supply)
- IEC inlet rear panel
- Rocker switch (mains kill)
- Internal AC split to both PSUs
- DC cables run up through Mortise legs to torso
- Speaker wire runs up through legs to eye speakers
- USB runs up through legs to Orin

## Head
- Walnut and maple lamination, wider than torso
- Two sealed speaker chambers, two 1/4" Baltic birch dividers, center wiring chase
- Chamber dimensions: 3x3x4" internal = 36 cubic inches = 0.59L each
- Tang Band W3-881SJF 3" full range, 8 ohm, crossed over HIGH at 150-200Hz
- 1" tweeter cutout routed each ear, plugged with walnut disc — deferred
- WiFi antenna pigtails routed through head into brass tube ear housings
- Walnut cap: ELP USB camera + mic board (9x62mm stick, horizontal mount, lens forward)
- Sliding brass camera cover on walnut cap — spring loaded, default closed
- Back panel removable
- Left ear: Bourns PEC11R encoder — personality selector, walnut knob
- Right ear: Bourns PEC11R encoder — verbosity control, walnut knob

## Neck
- Turned spindle, decorative
- Coiled service loop: L+/L− + R+/R− speaker wires + ELP USB cable
- MG996R servo offset from center, linkage arm to lazy susan
- Wires pass through center of cylinder unobstructed

## Torso Front Panel Layout
```
[ REBOOT ]  [ KEY SW ]  [ VU ]  [ CPX ]  [ INFERENCE ]  [ PWR ]
                    ──────── 7" LCD ────────
[ BT PAIR  ]                              [ VOLUME
[ MUTE     ]                                SLIDER ]
[ MIC KILL ]
[ SPARE    ]
        [ SUB VOL ] [ XOVER ] [ TREBLE ] [ BASS ]
```
Top row left to right:
- Covered toggle switch: reboot/reset, cosmetic, leftmost position
- Key switch: WiFi/BT connectivity kill, brass, keys dangling
- VU meter porthole: MC351 VU meter movement desoldered, jumpered to brass porthole bezel, analog needle
- CPX porthole: NeoPixel ring, emotional state, center position
- Inference meter: GC9A01 1.28" round LCD, driven by CPX, climbs during thinking, rightmost gauge
- Power toggle: brass SPST latching, rightmost position

Left of LCD: 4 color-coded raised pushbuttons stacked vertically (BT pair blue, mute amber, mic kill red, spare white)
Right of LCD: vertical sliding potentiometer, master volume
Below LCD: 4 brass knobs — sub volume, crossover, treble, bass (relocated MC351 pots via jumpers)

## Audio System
- Eye drivers: Tang Band W3-881SJF x2, stereo, high-passed at 150-200Hz
- Sub: Dayton Audio RS125P-8, external walnut orb enclosure, vented or sealed TBD
- Amp: Fosi Audio MC351 dehoused bare PCB, 32V supply, dual TPA3255, built-in DAC
- DAC: MC351 built-in, USB input from Orin
- Signal chain: Orin → USB → MC351 DAC → amp → Zobel → eye speakers + sub orb
- Headphone jack: 3.5mm NC switch, front panel or side
- RCA inputs: analog in → MC351 RCA input, side panel
- Zobel: 6.8Ω 10W resistor + 0.47µF film cap per driver (3 total — 2 eyes + sub)
- MC351 Bluetooth: standalone speaker mode when Orin is off/sleeping
- VU meter: MC351 onboard movement desoldered, jumpered to brass porthole panel meter
- Note: VU meter sensitivity at desk volumes may be low — evaluate on pre-out signal vs speaker output

## Inference Gauge + Thinking Behavior
- GC9A01 1.28" round SPI display driven by CPX via SPI
- Orin sends inference progress integer (0-100) to CPX via USB serial
- CPX animates needle on GC9A01
- During thinking state: gauge climbs, head oscillates 2-3° rapidly (building tension)
- Oscillation starts slow, tightens and speeds up as inference peaks
- At response ready: everything stops, dead still, half second pause, then speaks
- Sequence: attention → tremor builds → peak → stillness → response

## Emotional State System
Non-verbal primary — WALL-E inspired. Speech rare and meaningful. 12 states.

| State | Head | CPX Porthole | Audio | Sub | Speech |
|---|---|---|---|---|---|
| Idle | Slow drift 10-15° | Slow amber breathe | Quiet mechanical whir | Silent | Never |
| Listening | Locks on source | Steady cyan | Music fades 15% | Silent | Never |
| Thinking | Fast tight oscillation 2-3° | Blue→purple sweep | Quiet rhythmic clicks | Low rumble | Never |
| Happy | Quick side wiggle | Warm yellow burst | Ascending 3-note melody | Warm thump | Rare — "yeah." |
| Curious | Slow tilt one side | Slow cool white | Single rising tone | Silent | Never |
| Startled | Fast snap back | White flash → red | Sharp yelp | Hard thump | Never |
| Uncomfortable | Slow turn away | Deep red pulse | Low dissonant hum | Quiet rumble | Rare — "no." |
| Sad | Aimless drift | Deep blue, dim | Long descending tone | Silent | Never |
| Excited | Rapid wide sweep | Rapid color cycle | Escalating tones | Rhythmic pulses | Rare — "oh!" |
| Focused | Locked forward | Steady cool white | Quiet steady hum | Silent | Functional only |
| Sleeping | Drifted, still | Single slow pulse | Slow mechanical breath | Silent | Never |
| Greeting | Turns toward person | White sweep | Warm two-tone chime | Single warm tone | Rare — name only |

## Personality System
Left ear encoder selects personality. Right ear encoder controls verbosity (continuous dial). Press either knob to confirm or reset.

| Position | Name | Character |
|---|---|---|
| 1 | Mortise | Precise, dry, craftsman. Default. |
| 2 | Jasper | Old soul, measured, warm. |
| 3 | Glen | Easy companion, friendly, no agenda. |
| 4 | Pawl | Quiet observer, says little, means everything. |

Human note- We need to add a "joker" personality- ask about this. 

## Power Architecture
- Single IEC inlet on base platform rear
- Rocker switch → splits to:
  - MC351 32V supply → MC351 dehoused PCB
  - 19V 8A PSU → Orin barrel jack
  - 19V → 5V 5A buck → CPX, servos, NeoPixels, fan

## Compute
- NVIDIA Orin Nano Super Dev Kit (P3766)
- JetPack 6.x, Ubuntu 22.04
- Gemma 4 E4B via Ollama/llama.cpp (Jetson AI Lab container)
- DisplayPort → HDMI adapter → 7" Waveshare LCD
- M.2 Key-E: Intel AX200 WiFi 6 + BT 5.0
- WD 1TB NVMe in USB enclosure (port 3) — upgrade to M.2 direct when ready
- microSD 64GB boot
- SSH over Tailscale

## USB Port Allocation
| Port | Device |
|---|---|
| 1 | ELP USB camera + mic |
| 2 | Circuit Playground Express |
| 3 | WD 1TB NVMe enclosure |
| 4 | USB-A panel mount passthrough |

## Heat Management
- Orin: active cooling via 40mm fan, forced airflow through torso
- MC351 dehoused PCB: retains original heatsink, fan directed at it
- PSU bricks: in base platform, vented downward, thermally isolated from torso
- Sub: external orb, no heat concern
- Finish: Rubio Monocoat hardwax oil — handles heat better than lacquer
- Mortise monitors Orin temperature via software, reports when hot

## Parts List (~$850 estimated, real prices $1,000+)

### NVIDIA / ARROW ELECTRONICS
- Orin Nano Super Dev Kit — $249

### PARTS EXPRESS
- Tang Band W3-881SJF 3" full range x2 — $68 (clearance, order soon)
- Dayton Audio RS125P-8 5" paper/Kevlar woofer — $50
- Zobel network components — $4
- Acoustic polyfill — $6
- Gasket tape + silicone sealant — $5

### WAVESHARE
- Waveshare 7" 1280x800 IPS capacitive touch — $70

### FOSI AUDIO
- Fosi Audio MC351 32V version — ~$170 (includes 32V supply, dehouse on arrival)

### ADAFRUIT
- Circuit Playground Express (ID 3333) — $24.95
- RPi Camera — removed, using ELP USB
- NeoPixel 332 LED/m Silicone Bead Strip 0.5m (ID 4865) — $37.50
- GC9A01 1.28" round SPI display — $8
- Bourns PEC11R rotary encoders x2 — $10

### MOUSER / DIGI-KEY
- 3.5mm headphone jack w/ NC switch — $2
- Zobel resistors 6.8Ω 10W x3 — in Zobel estimate
- Zobel capacitors 0.47µF film x3 — in Zobel estimate
- Brass key switch with keys — $12
- Brass SPST latching power toggle — $5
- Color coded raised pushbuttons x4 — $8
- Vertical sliding potentiometer (volume) — $6
- Covered flip safety switch (reboot/reset) — $8
- Panel mount potentiometers x4 (values TBD on MC351 arrival) — $8
- Panel mount VU meter movement, brass bezel (match MC351 drive circuit) — $12
- IEC inlet panel mount — $5

### AMAZON
- Intel AX200 M.2 WiFi 6 + BT 5.0 — $15
- M.2 antenna pigtails + adhesive antennas — $5
- 40mm 5V fan — $5
- M2.5 brass standoff set — $4
- DP-to-HDMI adapter — $8
- ELP 8MP 4K autofocus USB camera with mic (9x62mm) — $32.99
- MG996R servo (swivel only) — $8
- 2" lazy susan bearing — $5
- 19V 8A laptop-style PSU — $18
- Buck converter 19V→5V 5A — $6
- USB-A panel mount — $4
- RCA jack panel mount pair — $6
- Brass tube x4 (antenna ears x2 + pen holders x2) — $8
- Brass sliding camera cover hardware — $6
- Walnut knobs x2 (encoders) — from stock
- Brass knobs x4 (amp pots) — $12
- 24 AWG silicone 4 colors — $8
- 20 AWG silicone 2 colors — $5
- Spiral cable wrap — $4
- Dowels — $5

### HARDWARE STORE
- PVC port tube 1.5" diameter (sub orb vent) — $3

### FROM STOCK
- WD 1TB NVMe in USB enclosure
- microSD 64GB
- Wood + finish (torso, head, base platform, sub orb)

**Estimated total: ~$850 (real prices likely $1,000+)**

## Open Items
- Torso dimensions not finalized in SketchUp
- Base platform dimensions TBD — sized to house two PSU bricks with ventilation
- Sub orb dimensions TBD — sized to RS125P-8 Vas and port tuning
- MC351 arrival: photograph PCB, identify VU meter drive points, measure drive current, source matching panel-mount meter movement
- MC351 arrival: identify and document all pot values before ordering panel-mount replacements
- Verify ELP camera ribbon/board fits walnut cap cleanly
- Brass sliding camera cover design TBD
- Servo offset linkage arm design TBD — finalize after servo arrives
- Tang Band W3-881SJF clearance — order soon
- Check M.2 2280 direct fit on P3766 before buying NVMe enclosure

## Build Sequence
1. Sub orb — lathe turn walnut sphere, install RS125P-8, port or seal, test
2. Base platform — walnut/maple box, PSU bricks, IEC inlet, rocker, DC cable runs, ventilation
3. Sub chamber removed from torso — torso now compute and electronics only
4. Head — speaker chambers, drivers, walnut cap with ELP camera, brass tube ears, encoder housings, tweeter cutouts plugged
5. Torso — Orin, MC351 dehoused PCB, buck converter, LCD, front panel (gauges, switches, knobs, pushbuttons), CPX porthole, NeoPixel chest, back panel
6. Neck assembly — turned spindle, lazy susan, offset servo, linkage arm, coiled service loop
7. Connect base → torso → head, route all cables
8. Software — JetPack flash, Gemma 4 E4B, personality engine, emotional states, inference gauge

## Software Goals
- Gemma 4 E4B — conversation, vision, audio unified
- YouTube, podcast, music streaming
- Face recognition, presence detection via ELP camera
- Personality engine — 4 personalities, hot-swap via left encoder
- Verbosity control — continuous dial via right encoder
- Emotional state engine — 12 states, non-verbal primary
- Inference gauge — integer 0-100 sent to CPX → GC9A01 needle animation
- Head tremor during thinking — MG996R oscillation routine
- NeoPixel emotional states via CPX
- Temperature monitoring — Mortise reports when hot
- SSH over Tailscale remote management
