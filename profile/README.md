# DongLoRa

**/ˈdɒŋ.ɡəl.ɔːr.ə/** — a portmanteau of **dongle** and **LoRa**.

Transparent LoRa radio over USB. Flash the firmware onto a supported board, plug it in, and send/receive LoRa packets from your computer. The firmware handles the radio; your code decides what to do with it.

```
Your code ──► client library ──┐
Your code ──► client library ──┤
              ...              ├──► mux daemon ──► USB serial ──► firmware ──► LoRa radio
Your code ──► client library ──┤
Your code ──► client library ──┘
```

## Repositories

| Repo | Description | Language |
|------|-------------|----------|
| [firmware](https://github.com/donglora/firmware) | Embedded firmware for LoRa boards | Rust |
| [client-rs](https://github.com/donglora/client-rs) | Rust client library (`donglora-client` on crates.io) | Rust |
| [client-py](https://github.com/donglora/client-py) | Python client library (`donglora-python` on PyPI) | Python |
| [mux-rs](https://github.com/donglora/mux-rs) | USB multiplexer daemon | Rust |
| [mux-py](https://github.com/donglora/mux-py) | USB multiplexer daemon | Python |
| [bridge](https://github.com/donglora/bridge) | P2P LoRa bridge over the internet | Rust |
| [examples](https://github.com/donglora/examples) | Demo scripts — receive, transmit, bridge | Python |
| [ai-bot](https://github.com/donglora/ai-bot) | MeshCore AI chatbot (default mesh name: "Orac") | Python |
