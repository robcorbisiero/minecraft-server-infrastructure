# My Minecraft servers

I run Bedrock, All the Mods 10, and Paper on three Ubuntu machines. This repo is where I keep the setup, migration steps, commands, and problems I ran into along the way.

The idea was simple: use the PCs I already had, give each server its own machine, and keep our existing worlds and progress.

## The setup

<img src="photos/physical-server-setup.jpg" alt="My wireless router, two Lenovo Minecraft servers, and HP Paper server" width="760">

From left to right: the white wireless router, Bedrock on the M710q, ATM10 on the M720q, and Paper on the HP desktop.

| Server | Machine | CPU | RAM | Storage | OS |
| --- | --- | --- | --- | --- | --- |
| [Bedrock](servers/bedrock/README.md) | Lenovo ThinkCentre M710q | i5-7500T, 4C/4T | 16 GB DDR4 | 120 GB NVMe | Ubuntu Server 24.04.4 LTS |
| [ATM10](servers/atm10/README.md) | Lenovo ThinkCentre M720q | i5-8500T, 6C/6T | 16 GB DDR4 | 128 GB WD SN520 NVMe | Ubuntu Server 26.04.1 LTS |
| [Paper](servers/paper/README.md) | HP ProDesk 600 G2 SFF | i5-6500 | 16 GB DDR4 | 240 GB SSD | Ubuntu Server 26.04.1 LTS |

![Server layout](diagrams/server-topology.svg)

## How I ended up with three servers

Bedrock started on the M720q. When I wanted to host ATM10, I moved Bedrock to the M710q and used the newer six-core machine for the heavier modpack. I brought my single-player ATM10 world over from Prism Launcher so my friends and I could keep playing on the same save.

Paper was already running on Windows. I moved its roughly 23 GB of worlds, plugins, datapacks, and configuration to the HP running Ubuntu. After the move, I tested the server and confirmed everything was working.

## Setup notes

| Page | What's in it |
| --- | --- |
| [Moving Bedrock](docs/bedrock-migration.md) | Transferring the world and add-ons, starting the new host, and expanding LVM |
| [Setting up ATM10](docs/atm10-deployment.md) | Moving the save, Java settings, console access, and memory troubleshooting |
| [Moving Paper from Windows](docs/paper-migration.md) | Preparing Ubuntu, transferring the server, and checking Java and DNS |
| [Problems I ran into](docs/troubleshooting.md) | Startup issues, stale Java PIDs, clock problems, storage, and plugin compatibility |
| [Commands](docs/command-reference.md) | The checks I use for hardware, services, ports, DNS, and Java |
| [Screenshots](screenshots/README.md) | Java version, ATM10 heap usage, and Paper's DNS record |

## A couple of useful checks

### Paper's Java version

![Java version on mc-paper](screenshots/paper-java-version.jpg)

I installed OpenJDK 25.0.4.1 on the Paper host. The [migration notes](docs/paper-migration.md) cover the rest of the move.

### ATM10 memory usage

![ATM10 heap statistics](screenshots/atm10-heap-info.jpg)

ATM10 was using about 10.3 GiB of process memory, so I initially wondered if it needed more RAM. Checking the heap showed about **3.55 GiB used out of 10 GiB** while nobody was playing. I kept the allocation at 8–10 GiB and focused on the slowdown when we generated new chunks. More on that [here](docs/atm10-deployment.md#4-investigate-the-exploration-slowdown).

## Still to finish

- Check the final backup schedules and make sure copies are stored on another machine.
- Do a proper restore test. I checked that a Bedrock archive could be read, but that isn't the same as restoring and playing the world.
- Add the current service files and more monitoring details.
- Confirm the Paper staging-folder cleanup.

These notes describe the setup at the time I worked on it. Versions and settings may change. I've left out passwords, private addresses, player data, and world files; example hostnames and PIDs need replacing before reusing commands.

[Hardware details](docs/hardware-inventory.md) · [Networking](docs/architecture.md) · [Maintenance](docs/operations.md) · [About these notes](docs/evidence-method.md)

My other infrastructure project: [Enterprise Infrastructure Homelab](https://github.com/robcorbisiero/enterprise-infrastructure-homelab).
