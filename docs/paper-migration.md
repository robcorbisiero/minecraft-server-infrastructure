# Paper: Windows to Ubuntu migration

## Outcome

An existing Windows Paper server was transferred to an HP ProDesk 600 G2 SFF running Ubuntu. After DNS configuration and testing, the owner confirmed that everything was working. The setup notes describe approximately 23 GB of server data.

## Recorded layout

- Runtime directory: `/opt/minecraft/paper/`.
- Transfer staging directory: `/home/rob/paper-transfer/`.
- Runtime files included `paper.jar`, world folders, plugin data, configuration, libraries, and versions.
- The transfer also contained Windows startup artifacts and an older Paper JAR.

## Preservation requirements

Preserve all world folders, including Multiverse worlds, plugin data, datapacks, and configuration. A successful migration includes player validation of worlds and inventories, rather than relying only on a process starting.

## Follow-up identified during setup

The previous chat recommended inspecting disk usage, removing Windows-only startup artifacts after verification, and reviewing old JARs, logs, and crash reports. It also identified a roughly 23 GB duplicate staging copy. Completion of this cleanup was not recorded in the recovered messages.

The notes recorded view distance and simulation distance both at 14. Lower values were suggested as an optional performance adjustment; adoption was not confirmed.

Keep a verified independent backup before deleting staging data or changing the runtime. No deletion or tuning commands have been executed as part of this documentation task.
