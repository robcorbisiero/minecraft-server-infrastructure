# Paper

Paper runs on an HP ProDesk 600 G2 SFF with Ubuntu, following a migration from Windows. I tested it after the move and everything was working.

| Item | Setup |
| --- | --- |
| CPU | Intel Core i5-6500 |
| RAM | 16 GB DDR4; original Linux output shows 14 GiB usable |
| Storage | 240 GB SSD; original device output shows 223.6 GiB |
| OS | Ubuntu Server 26.04.1 LTS |
| Hostname | `mc-paper` |
| Java | OpenJDK 25.0.4.1 |
| Runtime directory | `/opt/minecraft/paper/` |
| Transfer staging directory | `/home/rob/paper-transfer/` |
| Data size during migration | Approximately 23 GB |
| Connection port | TCP 25555 |
| DNS | A record plus `_minecraft._tcp` SRV record |
| Workload data | Worlds, Multiverse world folders, plugins, datapacks, configuration |

I was running Paper 26.3 build 159. The service name in my migration notes is `minecraft-paper.service`; I still need to add the installed unit. The [Java screenshot](../../screenshots/paper-java-version.jpg) shows the installed runtime. Before migration, the root logical volume was 100 GiB and swap was 4 GiB. 

See the [migration record](../../docs/paper-migration.md), [network notes](../../docs/architecture.md), and [maintenance status](../../docs/operations.md).
