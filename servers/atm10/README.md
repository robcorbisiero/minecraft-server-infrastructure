# All the Mods 10 (ATM10)

| Item | Setup |
| --- | --- |
| Machine | Lenovo ThinkCentre M720q |
| CPU | Intel Core i5-8500T, 6 cores / 6 threads |
| RAM | 16 GB DDR4 |
| Storage | 128 GB WD PC SN520 NVMe; comparison tables sometimes round this to 120 GB |
| OS | Ubuntu Server 26.04.1 LTS |
| Hostname | `mc-atm10` |
| Modpack | ATM10 v8.2, NeoForge |
| Java | Java 21 |
| Heap | `-Xms8G -Xmx10G` |
| Application port | TCP 25565 |
| Installation | `/opt/minecraft/atm10/` |
| Service | `atm10.service` |

I moved Bedrock off the M720q, then used it for ATM10 and brought over our Prism Launcher save.

I used tmux for console access and worked on systemd, RCON shutdown, and `atm10-backup.service`. I still need to confirm the final backup schedule and successful runs.

It ran well until three of us started generating new chunks. An idle heap check showed about 3.55 GiB used out of 10 GiB, so I kept the allocation and looked into pregeneration.

See [architecture](../../docs/architecture.md) and [operations](../../docs/operations.md).

Read the [deployment and JVM diagnostics walkthrough](../../docs/atm10-deployment.md), including the original heap screenshot.
