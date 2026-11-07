# Camera Node

Independent visual verification layer.

## Concept

IR-illuminated ESP32-CAM pointed at the roosting area. Run frame differencing at dusk — no AI, just whether the pixel blob count changed since the last known-good frame. Seeing is believing when RSSI and physical checks disagree.

## Planned contents

- `src/` — ESP32-CAM firmware
- `calibration/` — reference frames per lighting condition
- `notes.md` — IR LED placement, power budget, false triggers from mice or shadows

## Open questions

- ESP32-CAM vs a cheap dedicated IR board for lower power draw
- Motion threshold tuning to ignore settling birds versus a genuinely empty roost
