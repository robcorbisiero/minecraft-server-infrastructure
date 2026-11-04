# Paper: Windows to Ubuntu migration

## Outcome

An existing Windows Paper server was transferred to an HP ProDesk 600 G2 SFF running Ubuntu. After DNS configuration and testing, the owner confirmed that everything was working. The setup notes describe approximately 23 GB of server data.

## Recorded layout

![Paper host Java runtime validation](../screenshots/paper-java-version.jpg)

The original screenshot confirms OpenJDK 25.0.4.1 on the Paper host. See the [evidence gallery](../screenshots/README.md) for the capture context.

- Runtime directory: `/opt/minecraft/paper/`.
- Transfer staging directory: `/home/rob/paper-transfer/`.
- Runtime files included `paper.jar`, world folders, plugin data, configuration, libraries, and versions.
- The transfer also contained Windows startup artifacts and an older Paper JAR.

## Command walkthrough

### Inventory the destination

The owner supplied the output of these checks before transferring the workload:

```bash
hostnamectl
free -h
lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINTS
java -version
```

The original output identified Ubuntu 26.04.1 LTS, HP ProDesk 600 G2 SFF, 14 GiB usable RAM, 4 GiB swap, a 223.6 GiB device, and an initially 100 GiB root logical volume. Java was initially absent; the later screenshot above records the installed runtime.

### Prepare the runtime location

These commands were proposed in the migration sequence:

```bash
sudo useradd --system --home-dir /opt/minecraft/paper \
  --shell /usr/sbin/nologin minecraft
sudo mkdir -p /opt/minecraft/paper
sudo chown -R minecraft:minecraft /opt/minecraft/paper
```

The account creation is a one-time operation; inspect the account first on an existing host. These commands show the intended service identity and directory ownership, rather than running Paper as root.

### Stop the Windows workload and transfer all data

The source folder was `C:\Users\Rob\Paper-Server`. The application was to be stopped cleanly with `stop` in the Minecraft console before copying. The transfer staged approximately 23 GB under `/home/rob/paper-transfer/`.

Preserve the whole installation, including Multiverse world folders, plugin configuration, datapacks, and player state. Wait for the transfer to finish before copying staging files into the runtime location. Keep the Windows copy for rollback until gameplay has been validated.

### Validate process, logs, listener, and DNS separately

The following combines the recorded service name and DNS checks into a sanitized verification sequence. It is a reference sequence, not a fresh test:

```bash
systemctl status minecraft-paper --no-pager -l
sudo journalctl -u minecraft-paper -n 50 --no-pager
sudo ss -ltnp | grep 25555
dig +short paper.example.com A
dig +short SRV _minecraft._tcp.paper.example.com
```

`paper.example.com` is a placeholder. The SRV lookup should match the intended target and port; the original editor screenshot is in the [gallery](../screenshots/README.md#paper-dns-service-record).

Each check answers a different question: service state, startup errors, local listening socket, address resolution, and service-port discovery. The owner subsequently confirmed that everything was working. That gameplay check is stronger than a process-only check.

## Preservation requirements

Preserve all world folders, including Multiverse worlds, plugin data, datapacks, and configuration. A successful migration includes player validation of worlds and inventories, rather than relying only on a process starting.

## Follow-up identified during setup

The previous chat recommended inspecting disk usage, removing Windows-only startup artifacts after verification, and reviewing old JARs, logs, and crash reports. It also identified a roughly 23 GB duplicate staging copy. Completion of this cleanup was not recorded in the recovered messages.

The notes recorded view distance and simulation distance both at 14. Lower values were suggested as an optional performance adjustment; adoption was not confirmed.

Keep a verified independent backup before deleting staging data or changing the runtime. No deletion or tuning commands have been executed as part of this documentation task.
