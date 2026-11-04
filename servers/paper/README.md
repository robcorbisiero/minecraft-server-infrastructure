# Paper

Paper runs on an HP ProDesk 600 G2 SFF with Ubuntu, following a migration from Windows. The owner confirmed successful operation after the move.

| Item | Recorded value |
| --- | --- |
| Runtime directory | `/opt/minecraft/paper/` |
| Transfer staging directory | `/home/rob/paper-transfer/` |
| Data size during migration | Approximately 23 GB |
| Connection port | TCP 25555 |
| DNS | A record plus `_minecraft._tcp` SRV record |
| Workload data | Worlds, Multiverse world folders, plugins, datapacks, configuration |

Historical notes identify the server as Paper 26.3. This has not been checked against the current runtime. Exact Java version, startup command, systemd unit, RAM, and disk capacity remain unverified.

See the [migration record](../../docs/paper-migration.md), [network notes](../../docs/architecture.md), and [maintenance status](../../docs/operations.md).
