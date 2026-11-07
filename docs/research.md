# Research Notes

Findings from the All In project research phase (November 2026).

## RSSI-based occupancy detection

### Ohio State University — poultry layer barn path loss study

- Measured approximately 22.5 dB of additional path loss across a single fully stocked cage versus empty cage.
- Confidence interval under 1 dB wide — a large, clean separation ideal for threshold-based detection.
- Geometry and antenna placement details available in the original paper; replicate the link budget approach for coop-scale calibration.

### Zagreb, 2023 (arXiv) — RSSI-only human counting

- 98% accuracy counting up to 9 people using only RSSI standard deviation and a random forest classifier.
- Handful of detectors, no CSI, no tags. Demonstrates that plain RSSI carries enough statistical signal for occupancy decisions.
- Relevant because All In only needs binary all-in / not-all-in, a far easier problem than exact counts.

### Mostofi group, UC Santa Barbara — counting through walls

- Showed people can be counted by analyzing the *timing* of signal dips rather than their depth.
- Sidesteps much of the multipath noise that plagues RSSI amplitude measurements.
- Potential fallback if coop multipath (metal roof, wire mesh) degrades simple threshold approaches.

### Sheep proximity work (general)

- RSSI cutoffs are environment-specific; no universal threshold exists across installations.
- Plan on roughly a week of logging before trusting any alert threshold.

## Wi-Fi sensing (CSI-based, for reference)

### Yamashina Institute, Japan (2006)

- Tracked free-range chickens and guineafowl on a Wi-Fi grid using channel state information.
- ~2.6 m accuracy at 95% confidence, position logged every second.
- Closest published work on avian Wi-Fi tracking, though it targets localization rather than occupancy.

### Tohoku Gakuin University — outdoor bird detection in orchards

- Detects birds via CSI phase shifts; can catch crows flying several meters off the direct antenna line.
- Found that phase averaging across subcarriers provides processing gain, making low-multipath outdoor links surprisingly sensitive.
- Caveat: wind creates false positives; an adaptive baseline is needed.
- All In's coop is a low-multipath metal box — the opposite RF environment — but the same ESP32 firmware tricks (promiscuous mode, subcarrier processing) apply.

### allegorix/esp-wifi-rf-sensing (open source)

- Promiscuous-mode RSSI waterfall and occupancy heatmaps on ESP8266 and ESP32 with a Flask dashboard.
- Closest raw material for the RF headcount logic if plain RSSI proves insufficient.

### RuView (open source, 100k+ stars)

- Larger Wi-Fi sensing platform processing CSI from ESP32 nodes for presence and occupancy.
- More CSI-heavy than plain RSSI; reference only.

### Espressif esp-csi (1,700+ stars)

- Official on-device presence detection demo using ESP32 CSI.
- Reference for firmware patterns if the project escalates beyond RSSI.

## Open source coop automation projects

### OpenCoopControl

- Most complete open source ESP32 coop controller: door, feeder, temp, light, water, local web dashboard, PlatformIO build.

### CoopCommandESP32 (28 stars)

- Adds LayLight for winter laying hours and Blynk remote control.

### CluckControl Connect

- Most polished for reliability: state-based sun scheduling that catches missed events after reboot, full Home Assistant MQTT discovery, task watchdog.

### UberChickenCoop

- Closest to the hub-and-spoke topology: ESP-NOW between a door node and a bridge node that rebroadcasts to MQTT.

### Gap

- Every coop project uses cameras, RFID, or physical sensors for bird detection.
- Every Wi-Fi sensing project targets humans.
- RSSI-attenuation for a packed coop is novel in the open source space — document thoroughly as it gets built.

## Design decisions so far

- **Hub-and-spoke** over single controller: a dead node loses one zone, not the flock.
- **Presence detection as its own node**: publishes binary occupied/not-occupied; door logic, alerts, and dashboards subscribe. Allows adding camera or IR as independent publishers later.
- **Camera + RSSI as the two verification layers**: RSSI answers "did the RF signature change?"; camera answers "can I see them?"; agreement raises confidence. RFID deferred — banding is labor-heavy for a binary check.
- **Wi-Fi first** for the spoke transport given close proximity; ESP-NOW or RS-485 as fallback if interference appears.
- **Public repo** early for feedback while architecture is cheap to change. No location-specific details in public docs.
- **Over-the-air firmware updates** planned via the same RF link used for sensing.
