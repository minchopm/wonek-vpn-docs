# Using a Dedicated `/32` Static IPv4 Tunnel for SSH, Cloud Security Groups & WAF Allowlists

Canonical Portal Guide: [https://vpn.arte-soft.com/articles/best-dedicated-static-ipv4-vpn-for-ssh-and-cloud-allowlists-2026/](https://vpn.arte-soft.com/articles/best-dedicated-static-ipv4-vpn-for-ssh-and-cloud-allowlists-2026/)

## Why Shared VPN Gateways Break Infrastructure Access

Shared consumer VPN exits rotate across dozens of IPv4 addresses and multiplex hundreds of subscribers behind the same NAT address—triggering CAPTCHAs, breaking strict `/32` SSH bastion rules, and widening firewall exposure to every other customer on that exit node.

With Wonek VPN's **Dedicated Static IPv4** plan (`200 GB` / 30-day cycle), a single reserved `/32` IPv4 address is assigned exclusively to your tunnel profile across Apple Vision Pro, iPhone, iPad, Mac, and Android.

## Example AWS Security Group Rule (`aws ec2 authorize-security-group-ingress`)

```bash
# Replace 198.51.100.42/32 with your assigned Wonek VPN Dedicated Static IPv4
aws ec2 authorize-security-group-ingress \
  --group-id sg-0123456789abcdef0 \
  --ip-permissions IpProtocol=tcp,FromPort=22,ToPort=22,IpRanges='[{CidrIp=198.51.100.42/32,Description="Wonek Dedicated IPv4 SSH"}]'
```

## Example Cloudflare WAF Skip / Allow Expression

```text
(ip.src eq 198.51.100.42 and http.host eq "staging-admin.example.com")
```

## Verification Before Locking Down Production SSH

1. Connect Wonek VPN with your Dedicated Static IPv4 profile selected.
2. Confirm egress IPv4 stability from a terminal: `curl -4 https://icanhazip.com`.
3. Keep an out-of-band console path (AWS Systems Manager Session Manager or serial console) enabled alongside your `/32` ingress rule.
