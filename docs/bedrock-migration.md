# Moving Bedrock to the M710q

I moved Bedrock from the M720q to the M710q so I could use the stronger machine for ATM10. I wanted to keep the world and add-ons intact. After the move, I checked that the service was active, enabled at startup, and listening on UDP 19132.

## Why move a working server?

Both machines had 16 GB RAM, but the M720q had the newer six-core i5-8500T. I moved Bedrock first and checked it on the M710q before touching the old installation.

## 1. Inventory before changing roles

I started by checking the hardware, storage, and running server:

```bash
hostname
lscpu | grep "Model name"
free -h
lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINTS
df -h /
systemctl list-units --type=service --all | grep -Ei 'bedrock|minecraft|mcserver'
ps aux | grep -Ei 'bedrock_server|minecraft' | grep -v grep
```

The M720q still had its old Bedrock hostname during part of the ATM10 setup, which made the notes confusing later. Checking the hardware model and CPU helped keep the machines straight.

## 2. Preserve the complete installation

I transferred the worlds, behavior packs, resource packs, permissions, and configuration together. I needed the add-ons as well as the world.

On the M710q:

```bash
sudo tar -xzpf ~/bedrock-migration.tar.gz -C /opt/minecraft/
sudo chown -R rob:rob /opt/minecraft/bedrock
ls -lah /opt/minecraft/bedrock/worlds/
ls -lah /opt/minecraft/bedrock/behavior_packs/
ls -lah /opt/minecraft/bedrock/resource_packs/
```

My installation used `rob` for ownership here. Use the account your own service runs under if you adapt this.

The world and add-on directories were present after extraction. The next check was whether anyone had played since the archive was made. The final-sync command was:

```bash
# Stop the source server before the final sync. Replace SOURCE_HOST.
rsync -avh --progress \
  rob@SOURCE_HOST:/opt/minecraft/bedrock/ \
  /opt/minecraft/bedrock/
```

Keep the destination server stopped during the final sync too.

## 3. Start, validate, then cut over networking

I kept the old installation as a backup, stopped it, and tested Bedrock on the new machine before changing the UDP forwarding rule.

```bash
sudo systemctl start minecraft-bedrock
systemctl is-active minecraft-bedrock
systemctl is-enabled minecraft-bedrock
sudo ss -lunp | grep 19132
```

Once the M710q was working, I left Bedrock disabled on the M720q and moved on to ATM10.

## 4. Recover unused LVM capacity

The NVMe showed about 119.2 GiB, but root only had about 58 GiB. I checked the volume group before expanding it:

```bash
sudo vgs
sudo lvs
df -h /
```

The expansion command for my layout was:

```bash
# Check the VG/LV names and free space before using this.
sudo lvextend -l +100%FREE -r /dev/ubuntu-vg/ubuntu-lv
```

This used the remaining free space in `ubuntu-vg` and resized the filesystem. The expansion succeeded. Check your own layout first; this command takes all free extents in that VG.

## 5. Validate a recovery archive

I also created and checked a recovery archive:

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

The relevant output was:

```text
Backup archive verified
active
UNCONN ... 0.0.0.0:19132 ... bedrock_server
```

I shortened the listener line above. This recovery archive contains the worlds; the migration archive contained the full installation. For future checks I would use an exact archive filename instead of the wildcard, and stop the server or coordinate its saves before backing up. The archive was readable, but I still need to do a full restore test.

## What I took from this

Keeping the original installation until the new one worked made the move easier to recover from. The two things easiest to overlook were the add-on folders and the service definition on the destination. LVM was another useful check: the drive had space that root wasn't using yet.
