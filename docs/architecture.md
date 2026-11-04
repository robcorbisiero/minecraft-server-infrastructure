# Architecture and networking

The environment consists of three dedicated physical Linux machines, one per workload: Bedrock on the Lenovo M710q, ATM10 on the Lenovo M720q, and Paper on the HP ProDesk 600 G2 SFF. See the [hardware inventory and migration chronology](hardware-inventory.md).

## Paper connection path

The setup notes record a non-default TCP port of `25555`. A Minecraft SRV record mapped the Paper hostname to that port, and the owner subsequently confirmed that everything was working.

Sanitized equivalent:

```dns
_minecraft._tcp.paper.example.com. 1800 IN SRV 0 0 25555 paper.example.com.
```

The target hostname also needs an A record resolving to the router's public IPv4 address. The router's TCP forwarding rule directs the public port to the Paper host. Public and private IP addresses and the live connection hostname are intentionally omitted here.

## Recorded configuration

| Detail | Bedrock | ATM10 | Paper |
| --- | --- | --- | --- |
| OS release | Ubuntu Server 24.04.4 LTS | Ubuntu Server 26.04.1 LTS | Ubuntu Server 26.04.1 LTS |
| Historical server/modpack version | BDS 1.26.51.1 in earlier notes | ATM10 v8.2 / NeoForge | Paper 26.3 build 159 |
| Installed RAM | 16 GB DDR4 | 16 GB DDR4 | 16 GB DDR4 |
| Storage | 120 GB NVMe | 128 GB WD SN520 NVMe | 240 GB SSD |
| Application port | UDP 19132 | TCP 25565 | TCP 25555 |
| Service name | minecraft-bedrock | atm10.service | minecraft-paper.service in migration plan |

These values describe the recorded setup, not a current live inspection. Exact service files, router rules, and current software versions remain to be captured. Historical firewall checks describe LAN-only SSH and Bedrock UDP 19132 allowed; ATM10 TCP 25565 was LAN-only at that stage. Those checks should not be presented as current public forwarding rules.
