# Hardware inventory and workload migration

| Workload after migration | Physical machine | CPU | RAM | Storage |
| --- | --- | --- | --- | --- |
| Bedrock | Lenovo ThinkCentre M710q | i5-7500T, 4 cores / 4 threads | 16 GB DDR4 | 120 GB NVMe |
| ATM10 | Lenovo ThinkCentre M720q | i5-8500T, 6 cores / 6 threads | 16 GB DDR4 | 128 GB WD PC SN520 NVMe |
| Paper | HP ProDesk 600 G2 SFF | i5-6500 | 16 GB DDR4 | 240 GB SSD |

## Migration chronology

1. The M720q originally hosted Bedrock.
2. The M710q was initially considered for ATM10. Hardware assessment identified its i5-7500T, 16 GB DDR4, NVMe storage, and Ubuntu Server 24.04.4 LTS.
3. Bedrock moved to the M710q so the stronger six-core M720q could host ATM10. Setup messages confirmed the migrated Bedrock service running and enabled.
4. The M720q was repurposed as `mc-atm10`, with Java 21, ATM10 v8.2, and the existing single-player save.
5. The HP ProDesk became `mc-paper`, hosting the Paper server migrated from Windows.

An older Bedrock recap describes the M720q at that earlier stage. It should not be used as the post-migration Bedrock inventory.

## Evidence and capacity notation

The owner explicitly supplied the HP specs as i5-6500, 16 GB DDR4, and 240 GB SSD. Original Paper terminal output corroborates the HP ProDesk 600 G2 SFF model, Ubuntu 26.04.1 LTS, 14 GiB usable memory, and a 223.6 GiB device.

The M710q specifications appear in the earlier assessment of its terminal output. M720q CPU and memory are corroborated by later ATM10 diagnostics; the WD SN520 model and 128 GB capacity come from its earlier hardware recap. Some comparison tables call that drive 120 GB. Capacities are retained as recorded rather than silently treating these descriptions as identical measurements.

This inventory documents setup history recovered on November 3, 2026. It is not a live inventory scan.
