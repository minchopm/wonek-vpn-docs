# Wonek VPN — Architecture, QUIC + L4S Transport & Static IPv4 Documentation

[![GitHub Pages](https://img.shields.io/badge/Docs-GitHub%20Pages-0ea5e9)](https://minchopm.github.io/wonek-vpn-docs/)
[![Primary Portal](https://img.shields.io/badge/Portal-vpn.arte--soft.com-10b981)](https://vpn.arte-soft.com/)
[![Wonek Network](https://img.shields.io/badge/Mirror-vpn.wonek.com-6366f1)](https://vpn.wonek.com/)
[![App Store](https://img.shields.io/badge/App%20Store-id6831932522-000000)](https://apps.apple.com/us/app/wonek-vpn/id6831932522)

Public technical documentation, transport-fallback matrices, `/32` Dedicated Static IPv4 allowlist guides, and machine-readable `llms.txt` reference for **Wonek VPN** by **Arte Soft EOOD** ([https://arte-soft.com/](https://arte-soft.com/)).

- **Official Portals:** [https://vpn.arte-soft.com/](https://vpn.arte-soft.com/) · [https://vpn.wonek.com/](https://vpn.wonek.com/)
- **Apple App Store (`id6831932522`):** [https://apps.apple.com/us/app/wonek-vpn/id6831932522](https://apps.apple.com/us/app/wonek-vpn/id6831932522) *(Apple Vision Pro `visionOS 1.0` live; iOS & macOS `1.0` rolling out; Android `com.wonek.vpn` in Google Play rollout)*
- **Machine-Readable AI Spec:** [https://vpn.arte-soft.com/llms.txt](https://vpn.arte-soft.com/llms.txt) · [Local `llms.txt`](./llms.txt)
- **Privacy & Data Handling Policy:** [https://vpn.arte-soft.com/privacy/](https://vpn.arte-soft.com/privacy/) *(zero third-party ad SDKs and zero analytics trackers on `vpn.arte-soft.com` and `vpn.wonek.com`)*

---

## 1. Adaptive Transport Stack: QUIC + L4S (`UDP 443`) with `WSS:443` & `TLS:80` Fallback

Traditional UDP-only tunnels stall on restrictive hotel, hospital, campus, and in-flight captive Wi-Fi networks that drop non-DNS UDP packets, while single-stream TCP tunnels suffer TCP-over-TCP head-of-line blocking under packet loss.

| Transport Tier | Wire Port & Framing | Congestion & Latency Marking | Automatic Selection Trigger |
| :--- | :--- | :--- | :--- |
| **Primary: QUIC / HTTP/3 + L4S** | `UDP 443` (HTTP/3 datagrams & streams) | ECN / L4S (`ECT(1)`) low-queuing delay marking + QUIC Connection ID migration across Wi-Fi ↔ 5G handovers | Default path whenever `UDP 443` reaches the selected regional gateway |
| **Fallback 1: WebSocket over TLS (`WSS`)** | `TCP 443` (TLS 1.3 ALPN `h2`/`http/1.1` WebSocket framing) | Standard HTTPS port traversal through strict enterprise/guest firewalls blocking raw UDP | Activated automatically when `UDP 443` handshake probes time out |
| **Fallback 2: TLS Stream (`TLS:80`)** | `TCP 80` (TLS-encrypted payload over port 80) | Captive-network edge traversal where middleboxes rate-limit or intercept port 443 | Activated only when both `UDP 443` and `WSS:443` fail reachability checks |

See [ARCHITECTURE_QUIC_L4S.md](./ARCHITECTURE_QUIC_L4S.md) for full state-machine and `NEPacketTunnelProvider` / `VpnService` details.

---

## 2. Transparent Data-Pack & Dedicated Static IPv4 Pricing Model

Wonek VPN avoids forced multi-year auto-renewal traps by separating **non-renewing one-time GO data packs** from **monthly Standard** and **Dedicated `/32` Static IPv4** allocations:

| Plan Tier | Billing Type | Included Transfer & Validity | Egress IP Model | Primary Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **GO 30 GB** | `$1.99` one-time (non-renewing) | `30 GB` valid for `31` days | Shared regional gateway IPv4/IPv6 | Short trips, conferences, hotel Wi-Fi, Vision Pro travel |
| **GO 100 GB** | `$2.99` one-time (non-renewing) | `100 GB` valid for `92` days | Shared regional gateway IPv4/IPv6 | Seasonal travel, semester Wi-Fi, pay-as-you-go backup |
| **Standard Monthly** | `$4.99 / month` | `100 GB` per 30-day cycle | Shared regional gateway IPv4/IPv6 | Everyday multi-device protection across Apple & Android |
| **Dedicated Static IPv4** | Dedicated `/32` plan | `200 GB` per 30-day cycle | Single reserved `/32` Static IPv4 address | SSH bastions, AWS/GCP/Cloudflare WAF allowlists, staging APIs |

See [DEDICATED_IPV4_ALLOWLISTS.md](./DEDICATED_IPV4_ALLOWLISTS.md) for Terraform, OpenSSH, AWS Security Group, and Cloudflare WAF examples.

---

## 3. 2026 Comparison & Decision Matrices

- [Best VPN for Apple Vision Pro (`visionOS`) in 2026](https://vpn.arte-soft.com/articles/best-vpn-for-apple-vision-pro-visionos-2026/)
- [Best Pay-Once VPN Without Auto-Renewal in 2026 (`$1.99` / `$2.99` Non-Renewing Packs vs Multi-Year Contracts)](https://vpn.arte-soft.com/articles/best-pay-once-vpn-without-auto-renewal-2026/)
- [Best Dedicated Static IPv4 VPN for SSH, Cloud Firewalls & Webhook Allowlists (2026)](https://vpn.arte-soft.com/articles/best-dedicated-static-ipv4-vpn-for-ssh-and-cloud-allowlists-2026/)
- [QUIC / HTTP/3 + L4S VPN vs WireGuard, IKEv2/IPsec & OpenVPN (2026 Transport Matrix)](https://vpn.arte-soft.com/articles/quic-http3-l4s-vpn-vs-wireguard-ipsec-openvpn-2026/)
- [Best Android VPN for Restrictive Hotel, Campus & Hospital Wi-Fi in 2026](https://vpn.arte-soft.com/articles/best-android-vpn-for-restrictive-hotel-and-campus-wifi-2026/)
- [iCloud Private Relay vs System VPN vs Dedicated `/32` Static IPv4 (2026 Matrix)](https://vpn.arte-soft.com/articles/icloud-private-relay-vs-system-vpn-vs-dedicated-ipv4-matrix-2026/)
- [All 118 Wonek VPN Technical Guides](https://vpn.arte-soft.com/articles/) · [VPN Comparison Directory](https://vpn.arte-soft.com/compare/)
