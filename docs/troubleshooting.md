# Troubleshooting case studies

## ATM10 slows down when three players explore

**Symptom:** normal gameplay was reported as good; simultaneous generation of new chunks caused delays.

**Investigation:** checked the i5-8500T, system memory, process RSS, current PID, and JVM heap. Idle diagnostics showed about 3.55 GiB heap used against a 10 GiB maximum and about 4 GiB system memory available.

**Decision:** retain the historical 8–10 GiB heap settings and investigate generation work under load. Pregeneration was proposed; a measured improvement was not recorded.

**Lesson:** low free RAM and a large Java process do not by themselves justify increasing the heap. See [the commands and interpretation](atm10-deployment.md#4-investigate-the-exploration-slowdown).

## Java diagnostic command targets a process that no longer exists

**Symptom:** `jcmd` could not attach to the previously recorded PID.

**Investigation:** rediscovered Java with `pgrep -af java` and `jcmd -l`; the server had a different PID.

**Resolution path:** use the current Minecraft PID and check process ownership before interpreting an attach failure as an application fault.

**Lesson:** identifiers from a prior terminal session can become stale after a service restart.

## Ubuntu cannot accept repository metadata

**Symptom:** package metadata was rejected as not yet valid during M720q setup.

**Investigation:** the setup identified Chrony rather than `systemd-timesyncd`, with a clock offset of approximately four hours. The recorded diagnostic was:

```bash
sudo chronyc -N ntpdata
```

**Lesson:** inspect the actual time synchronization service and clock state before treating package metadata errors as a repository outage. The retrieved excerpt does not establish a final corrected offset.

## Storage capacity appears smaller than the installed drive

**Symptom:** the M710q NVMe had approximately 119.2 GiB device capacity but root initially used about 58 GiB. Paper's 223.6 GiB device initially had a 100 GiB root LV.

**Investigation:** compare `lsblk`, `df -h /`, `vgs`, and `lvs`.

**Resolution:** inspect free VG extents before extending the intended logical volume and filesystem. Later Bedrock messages confirmed expansion succeeded.

**Lesson:** disk size, partition size, logical-volume size, and filesystem size are separate layers.

## Service not found after copying Bedrock

**Symptom:** the files were on the M710q but systemd could not start the workload.

**Investigation:** the destination did not yet have the Bedrock service definition.

**Resolution path:** configure the destination service, start it, check process/listener state, and enable startup. Later migration notes confirm the service running and enabled.

**Lesson:** transferring application data does not transfer operating-system service registration.

## Paper Images plugin fails after a Minecraft update

**Symptom:** attempts to build an Images plugin version encountered missing legacy Spigot artifacts and dependencies on internal Minecraft/CraftBukkit classes.

**Investigation:** inspected Maven module selection, POM dependencies, and imports of `net.minecraft.*` and `org.bukkit.craftbukkit.*`.

**Outcome:** the owner stopped the custom update attempt and chose to wait for a compatible release. No successful custom plugin build is claimed.

**Lesson:** a version-number change does not establish binary or internal-API compatibility. A documented decision to stop an unsuitable repair is part of maintaining a stable workload.
