# Mortise

> A handcrafted walnut and maple AI desk companion. Speaker eyes. Retro gauge panel. Brass hardware. Local inference on NVIDIA Jetson Orin Nano Super. Built by a woodworker who wanted to see what happens when you take embodied AI seriously.

<br>

<p align="center">
  <img src="assets/mortise-concept.png" alt="Mortise concept render" width="600"/>
  <br>
  <sub><i>Concept render — close enough.</i></sub>
</p>

<br>

---

## What Is Mortise

Mortise is a retro humanoid robot that lives on your desk.

He plays music, watches YouTube, streams podcasts, holds conversations, recognizes your face, and notices when someone walks into the room. He expresses himself through light, sound, and movement — mostly without words.

His eyes are speakers. His ears are rotary encoders that control his personality and how much he talks. He has three brass porthole gauges where his heart would be — one shows what he's thinking, one shows what he's feeling, one shows what he's playing.

He is named after a woodworking joint. He is built from walnut and maple. He runs entirely local — no cloud, no subscription, no sending your conversations anywhere.

---

## Physical Architecture

Mortise is a three-piece system:

| Piece | Description |
|---|---|
| **Mortise** | The robot. Walnut and maple humanoid. Compute, audio electronics, display, controls, head movement. |
| **Base Platform** | Walnut and maple box. Houses both PSU bricks. Single IEC inlet — one cable to the wall. |
| **Sub Orb** | Lathe-turned walnut sphere housing a Dayton Audio RS125P-8 5" woofer. Sits beside or behind Mortise. |

---

## Hardware

### Compute
- **NVIDIA Jetson Orin Nano Super** — 67 TOPS, 8GB, JetPack 6.x (Ubuntu 22.04)
- Intel AX200 WiFi 6 + BT 5.0 (M.2)
- Waveshare 7" 1280×800 IPS capacitive touch LCD

### Audio
- **Eyes:** Tang Band W3-881SJF 3" full range drivers — stereo, high-passed at 150–200Hz
- **Sub:** Dayton Audio RS125P-8 5" paper/Kevlar woofer — external walnut orb enclosure
- **Amp + DAC:** Fosi Audio MC351 dehoused bare PCB — dual TPA3255, built-in USB DAC, built-in VU meter drive
- Zobel networks on all three drivers

### Body Controller
- **Adafruit Circuit Playground Express** — chest porthole mount, NeoPixels, built-in mic, sensors
- Drives NeoPixel emotional state ring and GC9A01 inference gauge via CircuitPython
- Communicates with Orin via USB serial

### Controls
- **Left ear:** Bourns PEC11R encoder — personality selector (4 positions, walnut knob)
- **Right ear:** Bourns PEC11R encoder — verbosity control (continuous, walnut knob)
- **Head:** MG996R servo, offset-mounted, linkage arm to lazy susan bearing — swivel only

### Front Panel

```
[ REBOOT ]  [ KEY SW ]  [ VU ]  [ CPX ]  [ INFERENCE ]  [ PWR ]
                    ──────── 7" LCD ────────
[ BT PAIR  ]                              [ VOLUME
[ MUTE     ]                                SLIDER ]
[ MIC KILL ]
[ SPARE    ]
        [ SUB VOL ] [ XOVER ] [ TREBLE ] [ BASS ]
```

Three brass porthole gauges dominate the upper panel:
- **VU porthole** — analog needle meter, moves with the music
- **CPX porthole** — NeoPixel ring, live emotional state
- **Inference porthole** — GC9A01 1.28" round LCD, climbs during thinking

---

## Software Stack

| Layer | Technology |
|---|---|
| OS | JetPack 6.x (Ubuntu 22.04) |
| Inference | Gemma 4 E4B via Ollama — local, no cloud |
| Vision | Gemma 4 multimodal — face recognition, presence detection |
| Camera | ELP 8MP 4K USB autofocus (9×62mm stick board) via OpenCV/V4L2 |
| Audio I/O | MC351 built-in USB DAC → amp → speakers |
| Body controller | Circuit Playground Express via USB serial (CircuitPython) |
| Remote access | SSH over Tailscale |
| Streaming | YouTube, podcasts, music via JetPack desktop |

---

## Personality System

The left ear encoder selects personality. The right ear encoder controls verbosity — clockwise for longer responses, counterclockwise for terse. Press to confirm.

| # | Name | Character |
|---|---|---|
| 1 | **Mortise** | Precise, dry, craftsman. Respects your work. Default. |
| 2 | **Jasper** | Old soul, measured, warm. More conversational. |
| 3 | **Glen** | Easy companion. Friendly, no agenda, genuinely glad you're there. |
| 4 | **Pawl** | Quiet observer. Says little. Means everything. |

Four named personalities as system prompt variants — hot-swappable at runtime with no restart.

---

## Emotional State System

Non-verbal primary — WALL-E inspired. Speech is rare, and when it comes, it's one or two words at most.

States drive: head servo, NeoPixels via CPX serial, non-verbal audio, and sub behavior simultaneously.

| State | Head | CPX Ring | Audio | Sub | Speech |
|---|---|---|---|---|---|
| Idle | Slow drift 10–15° | Slow amber breathe | Quiet mechanical whir | Silent | Never |
| Listening | Locks on source | Steady cyan | Music fades 15% | Silent | Never |
| Thinking | Fast tight oscillation 2–3° | Blue→purple sweep | Rhythmic clicks | Low rumble | Never |
| Happy | Quick side wiggle | Warm yellow burst | Ascending 3-note melody | Warm thump | Rare — *"yeah."* |
| Curious | Slow tilt | Slow cool white | Single rising tone | Silent | Never |
| Startled | Fast snap back | White flash → red | Sharp yelp | Hard thump | Never |
| Uncomfortable | Slow turn away | Deep red pulse | Low dissonant hum | Quiet rumble | Rare — *"no."* |
| Sad | Aimless drift | Deep blue, dim | Long descending tone | Silent | Never |
| Excited | Rapid wide sweep | Rapid color cycle | Escalating tones | Rhythmic pulses | Rare — *"oh!"* |
| Focused | Locked forward | Steady cool white | Quiet steady hum | Silent | Functional only |
| Sleeping | Drifted, still | Single slow pulse | Slow mechanical breath | Silent | Never |
| Greeting | Turns toward person | White sweep | Warm two-tone chime | Single warm tone | Rare — name only |

---

## Thinking Behavior

When Mortise receives a question:

1. Head snaps to attention, locks on
2. Inference begins — gauge climbs, NeoPixels shift blue-purple, head starts slow oscillation
3. Oscillation tightens and speeds up as inference peaks — a 2–3° rapid tremor, like he's shaking trying to erupt an idea
4. At response ready — everything stops. Dead still. Half second pause.
5. Head locks forward. Gauge drops. He speaks.

---

## Repository Structure

```
mortise/
├── README.md
├── assets/
├── hardware/
│   ├── parts-list.md
│   ├── wiring.md
│   ├── enclosure.md
│   ├── panel-layout.md
│   └── assembly.md
├── firmware/
│   └── cpx/
│       └── code.py               # CircuitPython — NeoPixels, GC9A01, serial handler
├── software/
│   ├── main.py
│   ├── config/
│   │   └── settings.py           # GPIO pins, defaults, tuning
│   ├── personality/
│   │   ├── engine.py             # Personality + verbosity management
│   │   ├── mortise.py
│   │   ├── jasper.py
│   │   ├── glen.py
│   │   └── pawl.py
│   ├── emotional_states/
│   │   ├── states.py             # State definitions and transitions
│   │   ├── head.py               # MG996R swivel + thinking tremor
│   │   ├── lights.py             # NeoPixel states via CPX serial
│   │   └── audio.py              # Non-verbal audio generation
│   ├── inference/
│   │   ├── gemma.py              # Gemma 4 E4B via Ollama
│   │   └── gauge.py              # Inference progress → CPX → GC9A01
│   ├── vision/
│   │   └── camera.py             # ELP USB camera via OpenCV/V4L2
│   ├── encoders/
│   │   └── encoders.py           # PEC11R — personality + verbosity
│   └── io/
│       └── switches.py           # Toggle, pushbutton, slider handling
├── scripts/
│   ├── setup.sh
│   ├── install-ollama.sh
│   └── start.sh
└── docs/
    ├── build-log.md
    └── future-builds.md
```

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

## GPIO Reference

| Pin | Function |
|---|---|
| 32 | MG996R swivel servo PWM |
| 33 | Left encoder A (personality) |
| 35 | Left encoder B (personality) |
| 36 | Left encoder SW (push) |
| 37 | Right encoder A (verbosity) |
| 38 | Right encoder B (verbosity) |
| 40 | Right encoder SW (push) |

---

## Build Notes

- PSU bricks live in the base platform — thermally isolated from the torso
- MC351 dehoused PCB retains its original heatsink; 40mm fan is directed at it — not optional
- NeoPixels are driven entirely by the CPX — do not use Orin GPIO NeoPixel libraries
- Rubio Monocoat finish handles heat better than lacquer
- Sub orb is external; Mortise monitors Orin temperature and speaks up when running hot

---

## Future Builds

**V2 — WALL-E** *(Gift, Christmas 2026)*
WALL-E form factor. Two round LCD eyes. Simpler than Mortise, tuned for a child. Same software stack.

**V3 — Cloud Connected**
Pi 5 + Claude API. No local inference. BOM drops to $300–400. Always on the latest model, tool use native, long context.

**Animal Forms**
The Mortise DNA — speaker eyes, porthole gauges, NeoPixels — transfers to any form. Cat, fox, orb, dog.

**The Gray One**
Gray-stained ash. Cold war electronics aesthetic. Different robot, different name, different market.

---

*Walnut and maple. Brass and wood. Built in the shop.*
