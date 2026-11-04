# Paper

Paper runs on an HP ProDesk 600 G2 SFF with Ubuntu, following a migration from Windows. The owner confirmed successful operation after the move.

| Item | Recorded value |
| --- | --- |
| CPU | Intel Core i5-6500 |
| RAM | 16 GB DDR4; original Linux output shows 14 GiB usable |
| Storage | 240 GB SSD; original device output shows 223.6 GiB |
| OS | Ubuntu Server 26.04.1 LTS |
| Hostname | `mc-paper` |
| Runtime directory | `/opt/minecraft/paper/` |
| Transfer staging directory | `/home/rob/paper-transfer/` |
| Data size during migration | Approximately 23 GB |
| Connection port | TCP 25555 |
| DNS | A record plus `_minecraft._tcp` SRV record |
| Workload data | Worlds, Multiverse world folders, plugins, datapacks, configuration |

Historical notes identify Paper 26.3 build 159. The migration plan names `minecraft-paper.service`; the deployed unit contents and final Java version have not been rechecked. Before migration, the root logical volume was 100 GiB and swap was 4 GiB. The owner explicitly supplied the CPU, installed RAM, and SSD capacity in the earlier hardware discussion.

See the [migration record](../../docs/paper-migration.md), [network notes](../../docs/architecture.md), and [maintenance status](../../docs/operations.md).
