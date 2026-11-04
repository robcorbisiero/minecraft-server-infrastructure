# Bedrock Dedicated Server

| Item | Recorded setup |
| --- | --- |
| Machine | Lenovo ThinkCentre M710q |
| CPU | Intel Core i5-7500T, 4 cores / 4 threads |
| RAM | 16 GB DDR4 |
| Storage | 120 GB NVMe; terminal assessment reports 119.2 GiB |
| OS | Ubuntu Server 24.04.4 LTS |
| Hostname | `mc-bedrock` |
| Application port | UDP 19132 |
| Installation | `/opt/minecraft/bedrock/` |
| Service | `minecraft-bedrock` |

Bedrock originally ran on the M720q. Its worlds, behavior packs, resource packs, and configuration were transferred to the M710q before the M720q was repurposed for ATM10. Later messages confirmed the Bedrock service active and enabled. The M710q root LVM volume was expanded after migration.

The migration notes identify BOMD, Dynamic Lighting, and Better Backpacks directories. Earlier pre-migration notes list BDS 1.26.51.1, BOMD 1.5.1, and Dynamic Lighting 4.0; current versions have not been inspected.

Original terminal output records a recovery archive passing a tar listing check, followed by a successful service restart and UDP 19132 listener. This establishes archive readability, not a full restore test or a confirmed recurring backup schedule.

See [architecture](../../docs/architecture.md) and [operations](../../docs/operations.md).
