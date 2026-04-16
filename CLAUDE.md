# Mortise — Claude Code Session Context

Paste this at the start of any Claude Code session working on Mortise.

---

You are working on Mortise — a handcrafted walnut and maple AI desk companion robot running Gemma 4 E4B on NVIDIA Jetson Orin Nano Super with JetPack 6.x. Three-piece system: Mortise (robot), base platform (PSUs), sub orb (external subwoofer).

## Key architecture

**Inference:** Gemma 4 E4B via Ollama (local, no cloud)

**Body controller:** Adafruit Circuit Playground Express via USB serial (CircuitPython)
- CPX built-in LEDs drive emotion porthole — no external NeoPixels
- Serial protocol: all Orin↔CPX communication via `software/io/cpx.py`
  - `STATE:<name>` — sets emotional state LED animation on CPX
  - `GAUGE:<0-100>` — drives analog VU meter needle (inference intensity)

**Head:** MG996R servo on GPIO 32 via PWM, offset mounted, thinking tremor routine

**Encoders:**
- Left ear (GPIO 33/35/36): personality selector — Mortise / Jasper / Glen / Pawl / Joker
- Right ear (GPIO 37/38/40): verbosity control — continuous dial, push to reset

**Camera:** Arducam 8MP IMX219 autofocus 160° wide angle, CSI/FPC to Jetson CSI port. Capture via GStreamer or libcamera — not USB/V4L2.

**Audio:** HiLetGo PCM5102 I2S DAC → Fosi Audio BT30D dehoused amp → Tang Band W3-881SJF speaker eyes + RS125P-8 subwoofer. No USB DAC.

**Inference meter:** Standalone analog VU meter (panel-mount), driven by CPX PWM/DAC output. CPX receives GAUGE:0-100 from Orin over serial.

**Power:** Switched IEC inlet → wagos → Jetson wall wart + 24A PSU (19V to amp, 24V→5V buck to accessories)

## Personality system

Five named personalities, hot-swappable at runtime via left encoder. No restart required.

| # | Name | Character |
|---|---|---|
| 1 | Mortise | Precise, dry, craftsman. Default. |
| 2 | Jasper | Old soul, warm, conversational. |
| 3 | Glen | Easy companion, friendly, no agenda. |
| 4 | Pawl | Quiet observer. Says little. Means everything. |
| 5 | Joker | Sarcastic, teasing, uses vision context for personalized greetings. |

Right encoder: verbosity — clockwise = longer responses, push = reset.

## Emotional states

12 states. Non-verbal primary. Speech rare (one or two words max at peak moments only).
States drive: head servo (including thinking tremor), CPX LEDs, non-verbal audio, sub behavior.

States: Idle, Listening, Thinking, Happy, Curious, Startled, Uncomfortable, Sad, Excited, Focused, Sleeping, Greeting

## Sleep / Wake sequences

Sleep and wake are choreographed multi-step sequences, not simple state transitions. Implemented via sequence runner in `software/emotional_states/states.py`.

**Sleep sequence:**
1. Protest (2s) — head ±15° shake, CPX orange-red strobe, grumbling whine
2. Escalation (2s) — head ±30° erratic, CPX chaotic red/orange/white, dissonant tones, sub thumps, speech rare "no."
3. Give-in (1s) — dead still, CPX dim amber, sub single thump, speech rare "fine."
4. Turn away (2-3s) — head slow 180° rotation, CPX fades to slow deep blue, descending tone fades out
5. Sleep state (held) — head at 180° locked, inference suspended, camera still running

**Wake sequence:**
1. Stir (1s) — slow drift, CPX deep blue quickens, breath fades up then cuts
2. Orient (1-2s) — head begins rotation back, CPX blue→amber, rising tone
3. Return (2s) — head locks forward, CPX warm white sweep, chime, sub warm tone
4. Active — presence trigger → Greeting; voice/button → Listening. Inference resumes.

## Thinking behavior

1. Head snaps to attention
2. Inference begins — gauge climbs, CPX LEDs blue-purple, head slow oscillation
3. Oscillation tightens and speeds — 2-3° rapid tremor as inference peaks
4. At response ready — everything stops, dead still, half-second pause
5. Head locks forward, gauge drops, speaks

## Rules

- Do not use GPIO NeoPixel libraries — CPX handles all LED output
- Do not use USB/V4L2 for camera — CSI/GStreamer only
- Do not be agreeable. Push back on approaches that add unnecessary complexity.
- This is a passion project. Elegance matters.
