# Bedrock Dedicated Server

| Item | Setup |
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

I moved Bedrock from the M720q to the M710q, including the worlds, behavior packs, resource packs, and configuration. I checked that the service was active and enabled, then expanded the root LVM volume.

The transferred add-ons included BOMD, Dynamic Lighting, and Better Backpacks. My earlier version notes list BDS 1.26.51.1, BOMD 1.5.1, and Dynamic Lighting 4.0; these may have changed since.

I checked a recovery archive with tar, restarted Bedrock, and confirmed the UDP 19132 listener. A full restore test and the final backup schedule are still on my list.

See [architecture](../../docs/architecture.md) and [operations](../../docs/operations.md).

Read the [full migration command walkthrough](../../docs/bedrock-migration.md).

## Service check — November 4, 2026

![Bedrock service status and UDP listener](../../screenshots/bedrock-service-status.png)

I checked `mc-bedrock`: the service was active, enabled at startup, and listening on UDP 19132. The status showed about 525.7 MB of memory use and a tmux session named `minecraft` launching `/opt/minecraft/bedrock/bedrock_server`.

![Backup directory and timer check](../../screenshots/bedrock-backup-check.png)

The backup directory contained one 375 MB recovery archive dated October 1. The filtered timer list only showed `dpkg-db-backup.timer`, which backs up the package database. This check didn't find a Minecraft systemd timer; I still need to check cron and any other scheduling before calling the backups automatic.
