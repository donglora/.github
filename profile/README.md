# DongLoRa

**/ˈdɒŋ.ɡəl.ɔːr.ə/** — a portmanteau of **dongle** and **LoRa**.

Turn any LoRa board into a USB radio peripheral. Flash the firmware, plug it in, and you have a full LoRa workbench — transmitting, receiving, and building applications in minutes, not days.

DongLoRa is an ecosystem: firmware that turns cheap LoRa boards into dumb USB radios, client libraries that make talking to them trivial, a multiplexer that lets you run multiple applications on one dongle simultaneously, and real applications built on top of all of it.

## Get Started in 3 Steps

```
1. Flash firmware        just flash heltec_v4
2. Plug in the board     (USB, that's it)
3. Receive packets       just rx
```

No drivers to install. No configuration files. No SDK to learn. The client library auto-detects your dongle and you're on the air.

## How It Works

The firmware is a dumb pipe — it has zero opinions about what you do with your radio. All the intelligence lives on the host. This means any language that can open a serial port can speak LoRa.

```
Your app ──► client library ──► USB ──► firmware ──► LoRa radio ──► the air

Your app ──► client library ──┐
Your app ──► client library ──┤
             ...              ├──► mux ──► USB ──► firmware ──► LoRa radio
Your app ──► client library ──┤
Your app ──► client library ──┘
```

The multiplexer lets you run a mesh bot, a packet sniffer, and a bridge all on the same radio at the same time.

## What Can You Build?

- **Mesh network tools** — run Meshtastic, MeshCore, or your own mesh protocol from a laptop, server, or Raspberry Pi
- **LoRa packet analysis** — sniff, decode, and log every packet on a frequency
- **Long-range bridges** — relay LoRa traffic across the internet, connecting distant networks
- **Automated monitoring** — watch repeater telemetry, track fleet status, alert on events
- **AI-powered mesh bots** — autonomous chatbots that live on mesh networks
- **RF site surveys** — sweep frequencies and map signal strength from your desk
- **Rapid prototyping** — test LoRa ideas in Python without touching embedded code
- **Multi-radio coordination** — run multiple dongles for frequency hopping, diversity reception, or parallel monitoring
- **Education and research** — teach LoRa concepts with immediate, tangible feedback
- **Field deployments** — plug a dongle into a Raspberry Pi and you have a headless LoRa gateway

## The Ecosystem

| Repo | What it does | Language |
|------|-------------|----------|
| [firmware](https://github.com/donglora/firmware) | Embedded firmware — turns LoRa boards into USB radio peripherals | Rust |
| [client-rs](https://github.com/donglora/client-rs) | Rust client library (`donglora-client` on crates.io) | Rust |
| [client-py](https://github.com/donglora/client-py) | Python client library (`donglora-python` on PyPI) | Python |
| [mux-rs](https://github.com/donglora/mux-rs) | USB multiplexer — share one dongle across multiple apps | Rust |
| [mux-py](https://github.com/donglora/mux-py) | USB multiplexer (Python implementation) | Python |
| [bridge](https://github.com/donglora/bridge) | P2P LoRa bridge — relay packets across the internet via encrypted gossip | Rust |
| [examples](https://github.com/donglora/examples) | Ready-to-run demo scripts — receive, transmit, ping-pong, bridge | Python |
| [ai-bot](https://github.com/donglora/ai-bot) | MeshCore AI chatbot (default mesh name: "Orac") | Python |

## Supported Boards

Any board with an SX1262 radio and USB. Currently:

| Board | MCU | Display |
|-------|-----|---------|
| Heltec V3 | ESP32-S3 | SSD1306 OLED |
| Heltec V4 | ESP32-S3 | SSD1315 OLED |
| RAK WisBlock 4631 | nRF52840 | SSD1306 OLED (optional) |
| Wio Tracker L1 | nRF52840 | SH1106 OLED |

Adding a new board is one Rust file. The firmware architecture is designed so the list keeps growing.

## License

[MIT](https://github.com/donglora/firmware/blob/main/LICENSE)
