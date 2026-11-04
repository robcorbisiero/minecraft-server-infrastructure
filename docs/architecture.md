# Architecture and networking

The environment consists of three dedicated physical Linux machines, one per workload. Bedrock uses a Lenovo mini PC, ATM10 uses an i5-8500T mini PC, and Paper uses an HP ProDesk 600 G2 SFF running Ubuntu.

## Paper connection path

The setup notes record a non-default TCP port of `25555`. A Minecraft SRV record mapped the Paper hostname to that port, and the owner subsequently confirmed that everything was working.

Sanitized equivalent:

```dns
_minecraft._tcp.paper.example.com. 1800 IN SRV 0 0 25555 paper.example.com.
```

The target hostname also needs an A record resolving to the router's public IPv4 address. The router's TCP forwarding rule directs the public port to the Paper host. Public and private IP addresses and the live connection hostname are intentionally omitted here.

## Inventory still needed

| Detail | Bedrock | ATM10 | Paper |
| --- | --- | --- | --- |
| Exact OS release | Not recovered | Not recovered | Ubuntu; release not recovered |
| Installed server/modpack version | Not recovered | Not recovered | Historical notes call it Paper 26.3; current version unverified |
| RAM and disk capacity | Not recovered | Not recovered | Not recovered |
| Application port | Not recovered | Not recovered | TCP 25555 recorded |
| Service definition and startup command | Not recovered | Not recovered | Not recovered |

No example firewall rules or systemd units are presented as deployed configurations without the actual files.
