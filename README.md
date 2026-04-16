# Mortise 🤖

A handcrafted AI desk companion. Speaker eyes. Retro gauge panel. Brass hardware. Local inference on NVIDIA Jetson Orin Nano Super.

---

## What Is This

Mortise is a retro humanoid robot that lives on your desk. He plays music, YouTube, streams podcasts, holds conversations, recognizes your face, notices when someone walks into the room, and expresses himself through light, sound, and movement — mostly without words.

He is named after a woodworking joint. His eyes are speakers. His ears are rotary encoders that control his personality and how much he talks. He has a brass porthole gauges where his heart would be — one shows what he's thinking, one shows what he's feeling.

This repository contains everything needed to build and run Mortise — software stack, configuration, personality engine, emotional state system, and hardware documentation.

---

## Physical Architecture

Mortise is a two-piece system:

**Mortise** — the robot. Walnut and maple humanoid. Compute, audio electronics, display, controls, head movement.

**Base platform/legs** House psu's fed by one iec inlet that splits in the torso. Legs house the power functions and the feet are pencil drawers.



---

## Hardware Overview

**Compute:** NVIDIA Jetson Orin Nano Super (67 TOPS, 8GB, JetPack 6.x)

**Audio:**
- Tang Band W3-881SJF 3" full range drivers as speaker eyes (stereo, high-passed 150-200Hz)
- Dayton Audio RS125P-8 5" paper/Kevlar woofer mounted rear facing in torso
- Fosi Audio MC351 dehoused bare PCB (dual TPA3255, built-in DAC, built-in VU meter)
- Zobel networks on all three drivers

**Display:** Waveshare 7" 1080x600 IPS capacitive touch LCD

**Camera:** ELP 8MP 4K autofocus USB camera with built-in mic (9x62mm stick board, walnut cap mount)

**Body controller:** Adafruit Circuit Playground Express — chest porthole mount, NeoPixels, built-in mic, sensors

**Lighting:** Adafruit NeoPixel 332 LED/m silicone bead strip (neck ring + chest)

**Head movement:** MG996R servo, offset mounted, linkage arm to lazy susan bearing, swivel only

**Ear encoders:**
- Left ear: Bourns PEC11R — personality selector (Mortise / Jasper / Glen / Pawl)
- Right ear: Bourns PEC11R — verbosity control (continuous dial)
- Both have push-button confirmation
- Walnut knobs turned from stock

**Gauges (three brass porthole bezels, upper torso):**
- Left — Inference gauge: GC9A01 1.28" round SPI display, driven by CPX, climbs during thinking
- Center — Emotional state: CPX NeoPixel ring, color and pattern reflect current state
- Right — VU meter: MC351 onboard meter movement desoldered and jumpered to panel-mount meter in brass bezel

**Power:** Base platform houses 19V 8A PSU (Orin) + MC351 32V supply (amp). Single IEC inlet, one cable to wall.

**Materials:** Walnut and maple, handcrafted, Rubio Monocoat finish, brass hardware throughout

---

## Front Panel Layout

```
[ REBOOT ]  [ KEY SW ]  [ CPX ]  [ INFERENCE ]  [LCD PWR]  [ PWR ]
                    ──────── 7" LCD ────────
[ BT PAIR  ]                              [ VOLUME
[ MUTE     ]                                SLIDER ]
[ MIC KILL ]
[ SPARE    ]
        [ SUB VOL ] [ XOVER ] [ TREBLE ] [ BASS ]
```

Top row left to right:
- **Covered toggle** — reboot/reset, cosmetic, leftmost
- **Key switch** — WiFi/BT connectivity kill, brass, keys dangling
- **CPX porthole** — NeoPixel emotional state display, center
- **Inference porthole** — thinking gauge, analog vu meter
- **Power toggle** — SPST latching, rightmost

- **Left pushbuttons** — BT pair (blue), mute (amber), mic kill (red), spare (white)
- **Right slider** — master volume, vertical sliding potentiometer
- **Bottom knobs** — sub volume, crossover, treble, bass (relocated MC351 pots)
- **Camera cover** — brass sliding cover on walnut cap, spring-loaded default closed

---

## Thinking Behavior

When Mortise receives a question:

1. Head snaps to attention, locks on
2. Inference begins — gauge climbs, NeoPixels shift blue-purple, head starts slow oscillation
3. Oscillation tightens and speeds up as inference peaks — 2-3° rapid tremor, like he's shaking trying to erupt an idea
4. At response ready — everything stops. Dead still. Half second pause.
5. Head locks forward. Gauge drops. He speaks.

---

## Personality System

Left ear encoder selects personality. Right ear encoder controls verbosity — turn clockwise for longer responses, counterclockwise for terse. Press to confirm or reset.

| Position | Name | Character |
|---|---|---|
| 1 | **Mortise** | Precise, dry, craftsman. Respects your work. Default. |
| 2 | **Jasper** | Old soul, measured, warm. More conversational. |
| 3 | **Glen** | Easy companion. Friendly, no agenda, genuinely glad you're there. |
| 4 | **Pawl** | Quiet observer. Says little. Means everything. |
| 5 | **Joker** | Sarcastic, affectionate, relentlessly teasing. Notices everything and will use it against you. Politely. Greets you by your flaws. Makes fun of you like a friend who's known you too long. Speech is less rare — and usually at your expense. |

---

## Emotional State System

Non-verbal primary — WALL-E inspired. Speech reserved for direct responses and rare peak moments (one or two words maximum).

| State | Head | CPX | Audio | Sub | Speech |
|---|---|---|---|---|---|
| Idle | Slow drift | Slow amber breathe | Quiet mechanical whir | Silent | Never |
| Listening | Locks on source | Steady cyan | Music fades 15% | Silent | Never |
| Thinking | Fast tight tremor | Blue→purple sweep | Rhythmic clicks | Low rumble | Never |
| Happy | Quick side wiggle | Warm yellow burst | Ascending melody | Warm thump | Rare — "yeah." |
| Curious | Slow tilt | Slow cool white | Single rising tone | Silent | Never |
| Startled | Fast snap back | White flash → red | Sharp yelp | Hard thump | Never |
| Uncomfortable | Slow turn away | Deep red pulse | Low dissonant hum | Quiet rumble | Rare — "no." |
| Sad | Aimless drift | Deep blue, dim | Long descending tone | Silent | Never |
| Excited | Rapid wide sweep | Rapid color cycle | Escalating tones | Rhythmic pulses | Rare — "oh!" |
| Focused | Locked forward | Steady cool white | Quiet steady hum | Silent | Functional only |
| Sleeping | Drifted, still | Single slow pulse | Slow mechanical breath | Silent | Never |
| Greeting | Turns toward person | White sweep | Warm two-tone chime | Warm tone | Rare — name only |

---

## Software Stack

- **OS:** JetPack 6.x (Ubuntu 22.04)
- **Inference:** Gemma 4 E4B via Ollama (Jetson AI Lab container)
- **Vision:** Gemma 4 multimodal — face recognition, presence detection, gesture recognition via ELP camera
- **Audio input:** CPX built-in mic + ELP camera built-in mic
- **Audio output:** MC351 built-in DAC (USB from Orin) → amp → speakers
- **Body controller:** Circuit Playground Express via USB serial (CircuitPython)
- **NeoPixels:** Driven by CPX
- **Inference gauge:** Orin sends integer 0-100 to CPX via serial → CPX animates GC9A01 needle
- **Head servo:** Orin GPIO PWM → MG996R
- **Encoders:** Orin GPIO → Bourns PEC11R (personality + verbosity)
- **Remote access:** SSH over Tailscale
- **Streaming:** YouTube, podcasts, music via JetPack desktop

---

## Repo Structure

```
mortise/
├── README.md
├── CLAUDE.md                         # Session context — paste at start of every session
├── .gitignore
├── hardware/
│   ├── parts-list.md
│   ├── wiring.md
│   ├── enclosure.md                  # Torso, head, base platform, sub orb dimensions
│   ├── panel-layout.md               # Front panel gauge and switch positions
│   └── assembly.md
├── firmware/
│   └── cpx/
│       └── code.py                   # CircuitPython — NeoPixels, GC9A01 gauge, serial handler
├── software/
│   ├── main.py                       # Event loop, state machine, module coordination
│   ├── config/
│   │   └── settings.py               # GPIO pins, serial port, Ollama endpoint, defaults
│   ├── personality/
│   │   ├── engine.py                 # Load/swap personality, verbosity scaling, context injection
│   │   ├── mortise.py
│   │   ├── jasper.py
│   │   ├── glen.py
│   │   ├── pawl.py
│   │   └── joker.py                  # Sarcastic, teasing — uses vision context for personalized greetings
│   ├── emotional_states/
│   │   ├── states.py                 # State enum + transition rules
│   │   ├── head.py                   # MG996R PWM, swivel, thinking tremor routine
│   │   ├── lights.py                 # NeoPixel states → CPX serial
│   │   └── audio.py                  # Non-verbal sound generation
│   ├── inference/
│   │   ├── gemma.py                  # Ollama wrapper, streaming, context management
│   │   └── gauge.py                  # Inference progress integer → CPX serial → GC9A01
│   ├── vision/
│   │   ├── camera.py                 # OpenCV/V4L2 capture, background thread
│   │   ├── presence.py               # Face detection, recognition, person enter/leave events
│   │   └── traits.py                 # Observable trait detection via Gemma multimodal (glasses, hat, etc.)
│   ├── media/
│   │   ├── player.py                 # Music and podcast playback via mpv
│   │   └── youtube.py                # YouTube search and stream via yt-dlp
│   ├── encoders/
│   │   └── encoders.py               # PEC11R interrupt handler — personality + verbosity
│   └── io/
│       ├── switches.py               # Toggles, pushbuttons, volume slider
│       └── cpx.py                    # Serial protocol layer — all Orin↔CPX communication
├── scripts/
│   ├── setup.sh                      # JetPack post-flash: apt deps, pip, Tailscale, systemd service
│   ├── install-ollama.sh             # Jetson AI Lab container, pull Gemma 4 E4B
│   └── start.sh                      # Launch software/main.py
└── docs/
    ├── build-log.md
    └── future-builds.md
```

---

## GPIO Pin Assignments

| Pin | Function |
|---|---|
| 32 | MG996R swivel servo PWM |
| 33 | Left encoder A (personality) |
| 35 | Left encoder B (personality) |
| 36 | Left encoder SW (push button) |
| 37 | Right encoder A (verbosity) |
| 38 | Right encoder B (verbosity) |
| 40 | Right encoder SW (push button) |

---

## Getting Started

```bash
# Flash JetPack 6.x, complete setup, then:
./scripts/setup.sh
./scripts/install-ollama.sh
ollama pull gemma4:4b
./scripts/start.sh
```

Remote access via Tailscale:
```bash
ssh mortise@<tailscale-ip>
```

---

## Heat Management

- 40mm fan forced airflow through torso — not optional
- PSU bricks in base platform, thermally isolated from torso
- MC351 dehoused PCB retains original heatsink, fan directed at it
- Rubio Monocoat finish — handles heat better than lacquer
- Mortise monitors Orin temperature and reports when running hot

---

## Future Builds

### V2 — WALL-E Inspired (Gift, Christmas 2026)
WALL-E form factor. Two round LCD eyes. Single quality driver in torso. Simpler than Mortise, focused on character and expression. Same software stack, tuned for a child.

### V3 — Cloud Connected
Pi 5 + Claude API. No local inference. BOM drops to $300-400. Always running the latest model, tool use native, long context. The refined daily driver once you know what you want.

### Animal Forms
Same architecture, different shell. Speaker eyes, porthole gauges, NeoPixels — the Mortise DNA transfers to any form.
- **Cat** — triangular ears, bandsaw and lathe friendly, independent personality
- **Fox** — upright like Mortise but animal, bridge between humanoid and animal forms
- **Orb** — pure lathe-turned walnut speaker, Bluetooth only, entry level, afternoon build
- **Dog** — compelling but ears require hand carving — someday

### The Gray One
Gray-stained ash, mismatched retro switches, oscilloscope LCD, key switch with dangling keys. Cold war electronics aesthetic. Different robot, different name, different market. Needs its own session.

---

## Claude Code Session Start

Paste this at the start of any Claude Code session:

```
You are working on Mortise — a handcrafted walnut and maple AI desk companion robot running Gemma 4 E4B on NVIDIA Jetson Orin Nano Super with JetPack 6.x. Three-piece system: Mortise (robot), base platform (PSUs), sub orb (external subwoofer).

Key architecture:
- Inference: Gemma 4 E4B via Ollama (local, no cloud)
- Body controller: Adafruit Circuit Playground Express via USB serial (CircuitPython)
- NeoPixels driven by CPX — do not use GPIO NeoPixel libraries
- Head swivel: MG996R servo on GPIO 32 via PWM, offset mounted, thinking tremor routine
- Left ear encoder (GPIO 33/35/36): personality selector — Mortise, Jasper, Glen, Pawl, Joker
- Right ear encoder (GPIO 37/38/40): verbosity control — continuous dial
- Camera: ELP 8MP USB stick board (9x62mm) in walnut cap, OpenCV/V4L2
- Audio: MC351 dehoused PCB, USB DAC built in, no separate DAC board
- Inference gauge: Orin sends integer 0-100 to CPX via serial → CPX animates GC9A01 1.28" round SPI display
- Three portholes: left=inference gauge, center=CPX NeoPixel emotional state, right=VU meter (MC351 meter movement relocated via jumpers)
- Serial protocol: all Orin↔CPX communication goes through software/io/cpx.py — STATE:<name> and GAUGE:<0-100>

Personality system: five named personalities (Mortise, Jasper, Glen, Pawl, Joker) as system prompt variants, hot-swappable at runtime via left encoder. Verbosity modifies response length parameters via right encoder. Joker receives vision context (traits.py output) injected into prompt for personalized greetings.

Emotional states: 12 states, non-verbal primary. States drive head servo (including thinking tremor), NeoPixels via CPX serial, non-verbal audio, sub behavior. Speech is rare.

Thinking behavior: inference begins → gauge climbs → head oscillates 2-3° with increasing speed → at response ready everything stops → half second pause → response delivered.

Do not be agreeable. Push back on approaches that add unnecessary complexity. This is a passion project — elegance matters.
```

---

*Walnut and maple. Brass and wood. Built in the shop.*
