# All the Mods 10 (ATM10)

| Item | Recorded setup |
| --- | --- |
| Machine | Lenovo ThinkCentre M720q |
| CPU | Intel Core i5-8500T, 6 cores / 6 threads |
| RAM | 16 GB DDR4 |
| Storage | 128 GB WD PC SN520 NVMe; comparison tables sometimes round this to 120 GB |
| OS | Ubuntu Server 26.04.1 LTS |
| Hostname | `mc-atm10` |
| Modpack | ATM10 v8.2, NeoForge |
| Java | Java 21 recorded in setup |
| Heap | `-Xms8G -Xmx10G` |
| Application port | TCP 25565 |
| Installation | `/opt/minecraft/atm10/` |
| Service | `atm10.service` |

After Bedrock moved to the M710q, the M720q was repurposed for ATM10. An existing Prism Launcher single-player save was transferred to retain builds, inventory, quests, and modded dimensions.

Earlier messages cover tmux console access, conversion to systemd management, RCON shutdown, and `atm10-backup.service` troubleshooting. Final backup-job success and recurring schedule still need confirmation.

The owner reported good performance except when three players generated new chunks. Historical idle heap diagnostics showed approximately 3.55 GiB used against a 10 GiB maximum. Chunk pregeneration was recommended; completion was not established in the reviewed messages.

See [architecture](../../docs/architecture.md) and [operations](../../docs/operations.md).
