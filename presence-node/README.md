# Presence Node

RSSI-based binary occupancy detection for a packed coop.

## Concept

A full coop of birds is a bag of water in the RF path. Compare RSSI between two ESP32 nodes with everyone in versus everyone out, and the attenuation separation is large enough for a simple threshold — no CSI analysis required for a binary check.

## Planned contents

- `src/` — ESP32 firmware (Arduino or ESP-IDF)
- `calibration/` — logged RSSI samples per bird count for threshold fitting
- `notes.md` — antenna placement, link geometry, wind false-positive handling

## Open questions

- Plain RSSI vs ESP32 CSI promiscuous mode for better sensitivity
- Subcarrier phase averaging for processing gain (from orchard Wi-Fi sensing work)
- Adaptive baseline to reject gust-induced false positives
