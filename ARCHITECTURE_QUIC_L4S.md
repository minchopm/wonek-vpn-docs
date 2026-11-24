# Adaptive QUIC / HTTP/3 + L4S Tunnel Architecture Across Mobile Handovers

Canonical Portal Guide: [https://vpn.arte-soft.com/articles/quic-http3-l4s-vpn-vs-wireguard-ipsec-openvpn-2026/](https://vpn.arte-soft.com/articles/quic-http3-l4s-vpn-vs-wireguard-ipsec-openvpn-2026/)

## Why System Tunnels Stall on Real-World Mobile & Guest Networks

1. **UDP Blocking on Captive & Enterprise Networks:** Fixed-port UDP protocols (`UDP 51820` or `UDP 500/4500`) are routinely dropped by hotel, university, and airline middleboxes that permit only DNS (`UDP 53`) and HTTPS (`TCP/UDP 443`).
2. **Wi-Fi to Cellular Address Changes:** Leaving a hotel lobby or train station swaps the client's local source IP and NAT mapping, stalling tunnels that bind state strictly to the 4-tuple `(src_ip, src_port, dst_ip, dst_port)`.
3. **Bufferbloat Under Concurrent Video & Spatial Streams:** On Apple Vision Pro (`visionOS`), macOS video calls, and mobile tethering, deep router queues add 80–300 ms of standing latency unless packets participate in Explicit Congestion Notification / Low Latency, Low Loss, Scalable Throughput (**L4S**).

## How Wonek VPN's System Tunnel Responds

- **QUIC Connection Migration (`UDP 443`):** Tracks sessions by cryptographic Connection ID rather than source socket 4-tuple, keeping SSH sessions and media streams intact when iOS, macOS, visionOS, or Android switches interfaces.
- **L4S Congestion Signaling:** Marks eligible outer UDP packets for L4S-aware queues on Apple's modern network stack (`iOS`, `macOS`, `visionOS`) to minimize queuing delay during interactive workloads.
- **Deterministic Fallback Ladder (`UDP 443` → `WSS:443` → `TLS:80`):** Runs bounded reachability checks inside Apple's `NEPacketTunnelProvider` and Android's `VpnService` so restrictive guest networks establish a tunnel without manual port toggling.
- **Zero Embedded Ad or Analytics SDKs:** Both client binaries and web portals (`vpn.arte-soft.com`, `vpn.wonek.com`) ship without third-party advertising or behavioral analytics trackers; inspect exact operational data handling at [https://vpn.arte-soft.com/privacy/](https://vpn.arte-soft.com/privacy/).
