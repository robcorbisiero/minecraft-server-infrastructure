# Setting up ATM10 on the M720q

I wanted my friends and me to keep playing our existing ATM10 world without relying on the single-player instance on my PC. I moved it to the M720q running Ubuntu, Java 21, and the matching ATM10 v8.2 NeoForge server pack.

## 1. Match the server pack to the existing world

I used `ServerFiles-8.2.zip` to match the version we were already playing. I wanted to get the move working before changing modpack versions.

I archived the save from Prism Launcher and transferred it to Linux:

```powershell
scp ".\ATM10-World-Backup.zip" rob@ATM10_HOST:/home/rob/
```

Replace `ATM10_HOST` with the server address. I kept the full save so our builds, inventories, quests, and dimensions could move together. I also kept the original single-player copy.

## 2. Runtime and memory allocation

I used these heap settings:

```text
-Xms8G -Xmx10G
```

That gives Java an 8 GiB starting heap and a 10 GiB maximum, leaving room for Linux and the rest of the JVM. I kept the NeoForge argument files from the server pack in the launcher.

I put the server in `/opt/minecraft/atm10/`. I started with a tmux session named `atm10`, then worked on `atm10.service` and RCON shutdown.

```bash
# Attach to the tmux console, if it is running there.
tmux attach -t atm10

# Check the systemd service.
systemctl status atm10 --no-pager -l
sudo journalctl -u atm10 -n 50 --no-pager
```

I still need to add the installed service and shutdown helper here. If systemd is running the server, don't launch another copy manually.

## 3. Diagnose startup exits before changing memory

At one point the server kept exiting during startup. I checked the service and logs to see whether Java was crashing or systemd was stopping it. I had also pasted a startup script underneath the generated script, so the file needed sorting out.

Checking the actual launcher mattered more than repeatedly restarting the service.

## 4. Investigate the exploration slowdown

ATM10 ran well until three of us started generating chunks at once. I checked the CPU, available memory, and Java process:

```bash
lscpu | grep "Model name"
free -h
ps -eo pid,rss,%cpu,args --sort=-rss | head -8
pgrep -af java
jcmd -l
```

Then I checked the Minecraft JVM:

```bash
# Replace JAVA_PID with the current Minecraft process ID.
jcmd JAVA_PID GC.heap_info
top -H -p JAVA_PID
```

I initially tried an old PID after the server had restarted. Finding the current Java process fixed that. `jcmd` also needs the JDK tools and permission to attach.

| What I saw | What it told me |
| --- | --- |
| About 14 GiB usable system RAM | Installed memory and OS-visible memory differ |
| About 4 GiB available | Low completely-free memory alone did not establish exhaustion |
| About 10.3 GiB process RSS | Includes memory outside the Java heap |
| About 3.55 GiB heap used, 10 GiB maximum | Idle heap use did not justify a larger allocation |
| Slowdowns during concurrent exploration | Chunk generation warranted profiling under load |

I kept the 10 GiB maximum. Chunk pregeneration was the next thing to try, but I haven't documented a completed run or measured improvement yet.

## 5. Backup-service investigation

These were the backup checks:

```bash
sudo systemctl status atm10-backup.service --no-pager -l
sudo journalctl -u atm10-backup.service -n 50 --no-pager
ls -ld /opt/minecraft/backups
ls -ld /opt/minecraft/atm10/world
systemctl is-active atm10
```

I still need to confirm the final schedule, successful runs, retention, and off-host copies. I've left RCON credentials out of the repo.

## What I would check next

The heap sample was taken while nobody was online. I want measurements while we're actually exploring before deciding whether to change memory settings or spend time on pregeneration.
