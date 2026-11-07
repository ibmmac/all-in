# All In

Modular coop automation for verifying every bird is roosted before the door closes at dusk.

## Architecture

Hub-and-spoke ESP32 network. Each concern is its own node publishing state over the mesh; the hub subscribes and drives door logic and alerts.

- **presence-node/** — RSSI-attenuation occupancy detection between two ESP32 endpoints. Binary all-in / not-all-in signal, no tags on birds.
- **camera-node/** — Independent verification layer. IR-illuminated ESP32-CAM running frame differencing at the roosting area for ground truth.
- **hub/** — Central coordinator. Subscribes to all publishers, runs door actuation, sends alerts when layers disagree or birds are still out.
- **docs/** — Calibration notes, RSSI threshold data, antenna placement, and experiment logs.

## Design principles

- Coarse modules first. Split finer only when a real boundary appears.
- Independent verification layers, not fused sensors. RSSI answers "did the RF signature change?"; camera answers "can I see them?"; agreement across layers raises alert confidence.
- Environment-specific thresholds. Plan a week of logging before trusting any cutoff.
- No location-specific details in public docs.

## Status

Early scaffold. Firmware and calibration data coming as nodes get built.
