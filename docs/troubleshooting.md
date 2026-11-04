# Problems I ran into

## ATM10 lag when three of us explored

ATM10 was running well until three of us started generating new chunks. My first thought was to give it more RAM.

I checked system memory, Java's process memory, and then the heap itself. While idle, the heap was using about 3.55 GiB out of 10 GiB, with roughly 4 GiB of system memory available. That didn't give me a reason to increase the heap, so I kept the 8–10 GiB settings and looked into chunk generation instead.

Pregeneration was the next idea, but I don't have a completed run or before-and-after measurements documented yet. The [commands and heap screenshot](atm10-deployment.md#4-investigate-the-exploration-slowdown) are in the ATM10 notes.

## jcmd couldn't find the Java process

I tried checking a PID from an earlier command and got an attach error. The server had restarted, so Java had a new process ID.

```bash
pgrep -af java
jcmd -l
```

Finding the current process fixed the starting point for the check. If it still won't attach, process ownership and permissions are worth checking too.

## Ubuntu said repository metadata wasn't valid yet

During M720q setup, the system clock was about four hours out. The machine was using Chrony, not `systemd-timesyncd`.

```bash
sudo chronyc -N ntpdata
```

This was a clock-sync problem to investigate before blaming the package repository. I still need to add the final clock check to these notes.

## The disk was bigger than the root filesystem

The M710q had about 119.2 GiB of NVMe capacity, but only about 58 GiB assigned to root. The HP had a similar situation: a 223.6 GiB disk with a 100 GiB root LV.

```bash
lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINTS
df -h /
sudo vgs
sudo lvs
```

I checked the free space in the volume group before expanding the logical volume. The M710q expansion completed successfully. That was a useful reminder to check LVM allocation instead of assuming Ubuntu used the whole disk.

## Bedrock files were there, but the service wasn't

After copying Bedrock to the M710q, systemd didn't know about it yet. The installation files had transferred, but the service definition hadn't been created on the new host.

Once the service was configured, I checked that it started, listened on UDP 19132, and was enabled for startup. Copying the application folder wasn't the whole migration.

## Trying to update the Paper Images plugin

I tried getting the Images plugin working with Paper 26.3. The build ran into old Spigot dependencies, then dependencies on internal Minecraft and CraftBukkit classes.

I looked through the Maven modules, POM files, and imports, but this was becoming more work than it was worth for that plugin. I stopped the custom update attempt and decided to wait for a compatible release.
