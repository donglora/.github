# DongLoRa

**/ˈdɒŋ.ɡəl.ɔːr.ə/** — a portmanteau of **dongle** and **LoRa**.

LoRa radios are cheap, long-range, low-power, and everywhere. But using one from a computer is surprisingly hard. You either run someone else's firmware with someone else's protocol baked in, or you write your own embedded code from scratch.

DongLoRa takes a different approach. You flash firmware onto a LoRa board once, and from that point on, the board is a dumb USB radio. It doesn't know or care what protocol you're running. It receives bytes from the air and hands them to your computer. It takes bytes from your computer and puts them on the air. That's all it does.

Everything else — what those bytes mean, how to route them, what to display, how to respond — happens in your code, on your computer, in whatever language you want.

## The Layers

DongLoRa is a stack of independent pieces. Each layer builds on the one below it.

**Firmware** sits on the LoRa board. It owns the radio hardware and speaks a simple binary protocol over USB. It boots, waits for commands, and does exactly what it's told. It has no opinion about LoRa protocols, mesh networks, or anything else. It's a pipe.

**Client libraries** (Python and Rust) run on your computer. They handle USB device discovery, protocol framing, and encoding/decoding so you don't have to think about bytes. You call `send()` and `receive()`. That's the API.

**The multiplexer** is optional. Normally, one program opens the USB port and that's it — no one else can use the dongle. The mux daemon sits between your programs and the dongle, so multiple applications can share one radio at the same time. A mesh bot, a packet logger, and a bridge can all run simultaneously on a single dongle.

**Applications** are built on top. A P2P bridge that relays LoRa packets across the internet. An AI chatbot that lives on a MeshCore mesh network. These are real programs that people run, built entirely on the layers below.

```
your code ──► client library ──► USB ──► firmware ──► LoRa radio

your code ──► client library ──┐
your code ──► client library ──┼──► mux ──► USB ──► firmware ──► LoRa radio
your code ──► client library ──┘
```

## Getting Started

Flash the firmware onto a supported board, plug it into USB, and run a script:

```
cd firmware && just flash heltec_v4
cd examples && just rx
```

That's it. You're receiving LoRa packets. The client library finds the dongle automatically — no port configuration, no driver installation, no setup.

From here, transmitting is one function call. Changing the radio frequency, bandwidth, or spreading factor is one function call. You have full control of the radio from Python or Rust, with the feedback loop of a scripting environment instead of a flash-and-pray embedded workflow.

## What People Use It For

Because the firmware is protocol-agnostic, a DongLoRa dongle works with any LoRa network — Meshtastic, MeshCore, custom protocols, or raw packets.

Some concrete examples:

- **Run mesh nodes on real hardware.** Connect to a Meshtastic or MeshCore network from a laptop, desktop, or Raspberry Pi. No dedicated device needed — just a dongle and a script.
- **Build mesh bots.** The AI chatbot in this org runs on a MeshCore mesh, receiving messages over LoRa, responding via an LLM, and transmitting replies — all from a Python script on a server.
- **Bridge distant networks.** The bridge application connects to local LoRa traffic, encrypts it, and relays it to other bridges over the internet. Two bridges in different cities extend a mesh network's range by thousands of miles.
- **Analyze RF traffic.** Sniff, decode, and log every packet on a frequency. Useful for debugging mesh deployments, understanding channel utilization, or reverse-engineering protocols.
- **Prototype without embedded development.** Test a LoRa idea in Python in an afternoon. Change parameters, observe results, iterate. When the idea works, you can move it to embedded if you need to — or just keep running it from your computer.
- **Deploy headless gateways.** A Raspberry Pi with a dongle plugged in is a LoRa gateway. Add the mux and you can run multiple services on it.

## Repositories

| Repo | What it is |
|------|-----------|
| [firmware](https://github.com/donglora/firmware) | Embedded firmware for LoRa boards (Rust) |
| [client-rs](https://github.com/donglora/client-rs) | Rust client library — `donglora-client` on crates.io |
| [client-py](https://github.com/donglora/client-py) | Python client library — `donglora` on PyPI |
| [mux-rs](https://github.com/donglora/mux-rs) | USB multiplexer daemon (Rust) |
| [mux-py](https://github.com/donglora/mux-py) | USB multiplexer daemon — `donglora-mux` on PyPI |
| [bridge](https://github.com/donglora/bridge) | P2P LoRa bridge — relays packets across the internet via encrypted gossip (Rust) |
| [examples](https://github.com/donglora/examples) | Ready-to-run scripts — receive, transmit, ping-pong, bridge (Python) |
| [ai-bot](https://github.com/donglora/ai-bot) | MeshCore AI chatbot — default mesh name "Orac" (Python) |

## Supported Boards

The firmware currently runs on these boards, all using the SX1262 LoRa radio:

| Board | MCU | Display |
|-------|-----|---------|
| Heltec V3 | ESP32-S3 | SSD1306 OLED |
| Heltec V4 | ESP32-S3 | SSD1315 OLED |
| RAK WisBlock 4631 | nRF52840 | SSD1306 OLED (optional) |
| Wio Tracker L1 | nRF52840 | SH1106 OLED |

Adding a new board is one Rust file — a pin mapping and peripheral list. The core firmware doesn't change.

## License

[MIT](https://github.com/donglora/firmware/blob/main/LICENSE)
