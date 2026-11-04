# Case study: moving Bedrock to the M710q

**Objective:** preserve an existing Bedrock world and add-ons while freeing the six-core M720q for ATM10.

**Result:** the migration notes confirm Bedrock running on the M710q, with the service active and enabled. A later terminal check confirms the UDP listener. The workload moved between physical Ubuntu machines; no world reset was required.

## Why move a working server?

The original M720q had an i5-8500T and 16 GB RAM. The available M710q had an i5-7500T and the same installed memory. Assigning the heavier modded Java workload to the six-core machine made better use of the existing hardware. Bedrock moved first, and ATM10 was installed only after the new Bedrock host had been checked.

## 1. Inventory before changing roles

These commands were used in the setup discussion to identify the workload, capacity, and service:

```bash
hostname
lscpu | grep "Model name"
free -h
lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINTS
df -h /
systemctl list-units --type=service --all | grep -Ei 'bedrock|minecraft|mcserver'
ps aux | grep -Ei 'bedrock_server|minecraft' | grep -v grep
```

The important distinction was the physical machine versus its current hostname. The old M720q still used a Bedrock-oriented hostname during early ATM10 work. Hardware model and CPU were more reliable identifiers than the shell prompt alone.

## 2. Preserve the complete installation

The transfer included worlds, behavior packs, resource packs, permissions, and configuration. Copying only a world directory could omit the add-ons it depends on.

The recorded restore commands on the destination were:

```bash
sudo tar -xzpf ~/bedrock-migration.tar.gz -C /opt/minecraft/
sudo chown -R rob:rob /opt/minecraft/bedrock
ls -lah /opt/minecraft/bedrock/worlds/
ls -lah /opt/minecraft/bedrock/behavior_packs/
ls -lah /opt/minecraft/bedrock/resource_packs/
```

These commands document this installation's ownership, not a universal service-user recommendation. Check the service account before adapting them.

The directory inspection confirmed the transferred world and add-on directories. A final synchronization was discussed to avoid losing progress made after the initial archive:

```bash
# Historical command with the source address replaced by a placeholder.
# Stop the source workload cleanly before the final synchronization.
rsync -avh --progress \
  rob@SOURCE_HOST:/opt/minecraft/bedrock/ \
  /opt/minecraft/bedrock/
```

`SOURCE_HOST` is a placeholder. This command writes to the destination. The destination must not be running against the files during the final sync.

## 3. Start, validate, then cut over networking

The migration sequence kept the old installation as a fallback, stopped the old server, started the new server, and tested locally before moving the UDP forwarding rule.

```bash
sudo systemctl start minecraft-bedrock
systemctl is-active minecraft-bedrock
systemctl is-enabled minecraft-bedrock
sudo ss -lunp | grep 19132
```

The old service was left disabled once the destination was working. The M720q could then be repurposed without two Bedrock instances competing for player traffic.

## 4. Recover unused LVM capacity

The M710q assessment showed a roughly 119.2 GiB NVMe device but only about 58 GiB assigned to root. The setup inspected free volume-group space before expanding the root logical volume:

```bash
sudo vgs
sudo lvs
df -h /
```

The expansion discussed in setup was:

```bash
# Historical command: only applicable to this verified VG/LV layout.
sudo lvextend -l +100%FREE -r /dev/ubuntu-vg/ubuntu-lv
```

This allocates all remaining free extents in that volume group and resizes the filesystem. It should not be copied onto an uninspected storage layout. Later messages confirmed expansion succeeded.

## 5. Validate a recovery archive

The following commands and result were present in the owner's terminal transcript:

```bash
sudo tar -czf \
  /opt/minecraft/backups/bedrock-recovery-$(date +%F_%H-%M-%S).tar.gz \
  -C /opt/minecraft/bedrock worlds

sudo tar -tzf /opt/minecraft/backups/bedrock-recovery-*.tar.gz >/dev/null \
  && echo "Backup archive verified"

sudo systemctl restart minecraft-bedrock
systemctl is-active minecraft-bedrock
sudo ss -lunp | grep 19132
```

Selected recorded output:

```text
Backup archive verified
active
UNCONN ... 0.0.0.0:19132 ... bedrock_server
```

The listener line is abbreviated; it is not a fresh command run. The archive command above captures worlds only, whereas the migration archive covered the complete installation. The wildcard check was recorded during setup; for repeatable validation, select one exact archive path. A successful tar listing proves readability, not a playable restore. A consistent live backup also needs the server's save coordination or a clean stop.

## Lessons

- Record physical hardware separately from a hostname that may survive repurposing.
- Preserve add-on dependencies alongside the world.
- Validate the destination before changing player traffic.
- Inspect LVM allocation rather than assuming the whole disk is usable by root.
- Distinguish archive integrity from restoration testing.
