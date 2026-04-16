# Mortise — Wiring

*To be documented during build.*

## GPIO Pin Assignments

| Pin | Function |
|-----|----------|
| 32  | MG996R swivel servo PWM |
| 33  | Left encoder A (personality) |
| 35  | Left encoder B (personality) |
| 36  | Left encoder SW (push button) |
| 37  | Right encoder A (verbosity) |
| 38  | Right encoder B (verbosity) |
| 40  | Right encoder SW (push button) |

## Audio Signal Path

Jetson I2S → PCM5102 I2S DAC → BT30D amp → Tang Band W3-881SJF (× 2, speaker eyes) + RS125P-8 (sub orb)

## CPX Serial

Jetson USB → CPX USB (CircuitPython serial)
- Orin → CPX: `STATE:<name>`, `GAUGE:<0-100>`
- CPX → Orin: mic level, encoder events, button presses

## Camera

Arducam IMX219 FPC → Jetson CSI port

## Power

IEC inlet (switched) → wagos
- → Jetson OEM wall wart → Orin
- → 24A PSU → 19V rail → BT30D
- → 24A PSU → 24V→5V buck → accessories
