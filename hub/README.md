# Hub

Central coordinator for the All In mesh.

## Role

- Subscribe to presence-node and camera-node state over the chosen transport (Wi-Fi AP, ESP-NOW, or wired)
- Run dusk-triggered all-in evaluation: require agreement across verification layers before declaring safe
- Drive door actuation and send alerts (app notification, MQTT, or local siren) when birds are still out or layers disagree
- Expose a local web dashboard for status and manual override

## Planned contents

- `src/` — hub firmware
- `web/` — local dashboard
- `config/` — node registry, alert rules, schedule
- `notes.md` — transport choice rationale, power architecture

## Open questions

- Wi-Fi AP versus ESP-NOW versus RS-485 for the spoke link
- Alert channel: phone push, MQTT to Home Assistant, or standalone
