# Mortise Build Log

---

## 2026-04-16 — Session 1: Architecture & Hardware Finalization

### Hardware changes from original README spec

**Audio**
- Amp changed from Fosi Audio MC351 to **Fosi Audio BT30D** (dehoused)
- BT30D has no built-in DAC — added **HiLetGo PCM5102** I2S DAC (Jetson I2S → PCM5102 → BT30D)
- BT30D has no built-in VU meter movement
- Zobel networks still planned, not yet ordered

**Amp controls**
- BT30D sub gain, crossover, treble, and bass pots will be relocated to front panel under the LCD
- Assess original pots on arrival — reuse with extended wires or replace with panel-mount versions

**Camera**
- Changed from ELP 8MP USB stick board to **Arducam 8MP IMX219 autofocus 160° wide angle**
- Connects via **CSI/FPC cable** to Jetson CSI port (not USB)
- Capture via GStreamer pipeline or libcamera

**Display**
- Waveshare 7" — confirmed approximately **1080x600** IPS capacitive touch LCD (verify exact model)

**Inference meter**
- GC9A01 round SPI displays removed from Mortise build
- Replaced with **standalone analog VU meter** (Amazon) driven by CPX
- CPX receives GAUGE:0-100 over serial from Orin, outputs PWM/DAC to drive needle

**Lighting**
- NeoPixel strips removed from build entirely
- Emotion display: **CPX built-in LEDs** behind diffused porthole only
- Neck ring LED on hold

**Power architecture**
- Switched IEC inlet → wagos → two outputs:
  - Output 1: Jetson wall wart (Orin)
  - Output 2: 24A PSU → 19V out (BT30D amp) + 24V→5V buck converter (accessories)

**Front panel (finalized)**
- Main power toggle
- LCD on/off power toggle
- WiFi/BT kill — keyed switch
- Mute — latching pushbutton
- BT pair — momentary pushbutton
- Reboot — domed red momentary (emergency style, aesthetic but functional)
- Mic kill — latching pushbutton
- Master volume — sliding potentiometer
- Emotion porthole — diffused, CPX LEDs behind
- BT30D controls (under LCD) — sub gain, crossover, treble, bass (relocated from amp PCB)

---

### Software decisions

**Personalities**
- Five named personalities: Mortise, Jasper, Glen, Pawl, Joker
- Joker receives vision context (traits.py) injected into prompt for personalized greetings
- Hot-swappable at runtime via left encoder — no restart required

**Sleep / Wake behavior**

Sleep command initiates a multi-step choreographed sequence before entering sleep state:

1. **Protest** (2s) — head ±15° shake ~4Hz, CPX orange-red strobe, grumbling whine rising in pitch
2. **Escalation** (2s) — head ±30° erratic sweeps, CPX chaotic red/orange/white, dissonant escalating tones, sub irregular thumps, speech rare "no."
3. **Give-in** (1s) — everything stops dead still, CPX dim amber, sub single resigned thump, speech rare "fine."
4. **Turn away** (2-3s) — head slow 180° rotation to face wall, CPX fades to slow deep blue pulse, long descending tone fading out
5. **Sleep state** (held) — head at 180° locked, CPX single slow deep blue pulse, slow mechanical breath looped, inference suspended, camera still running (presence detection active)

Wake command (triggered by presence detection, voice, button, or schedule):

1. **Stir** (1s) — head slow single drift, CPX deep blue quickens, breath fades up then cuts
2. **Orient** (1-2s) — head begins slow rotation back from 180°, CPX fades blue→amber, single rising tone
3. **Return** (2s) — head completes rotation forward, CPX warm white sweep, warm two-tone chime, sub warm tone
4. **Active** — presence trigger → Greeting state; voice/button trigger → Listening state. Inference resumes.

**Implementation plan**
- Full plan in `/home/jason/.claude/plans/modular-squishing-mountain.md`
- Phase 0: repo scaffolding + docs
- Phase 1: config + CPX serial protocol
- Phase 2: hardware interfaces (servo, encoders, switches)
- Phase 3: emotional state system
- Phase 4: sleep/wake sequences
- Phase 5: inference (Gemma 4 E4B via Ollama)
- Phase 6: vision (Arducam IMX219 CSI, presence detection, trait detection)
- Phase 7: personality engine
- Phase 8: media (mpv, yt-dlp)
- Phase 9: main event loop
- Phase 10: scripts (setup, ollama install, start)
