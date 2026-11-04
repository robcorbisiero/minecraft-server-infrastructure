# Administration command reference

Commands below were used or discussed in the setup history, with addresses and transient PIDs replaced by placeholders. They are documentation, not commands executed against the servers during this repository work. Review write operations and verify paths before reuse.

## Hardware and storage

```bash
hostnamectl
lscpu | grep "Model name"
free -h
lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINTS
df -h /
sudo vgs
sudo lvs
java -version
```

These distinguish machine model, physical capacity, filesystem allocation, available memory, and runtime version. `df` describes the mounted filesystem, not the capacity of the whole device.

## Service state and logs

```bash
systemctl is-active minecraft-bedrock
systemctl is-enabled minecraft-bedrock
systemctl status atm10 --no-pager -l
sudo journalctl -u atm10 -n 50 --no-pager
systemctl status minecraft-paper --no-pager -l
sudo journalctl -u minecraft-paper -n 50 --no-pager
```

The Paper service name comes from the migration plan; confirm the installed unit. `active` describes the process/service state, while `enabled` describes startup configuration. Neither alone proves a player can join.

## Listener and DNS checks

```bash
sudo ss -lunp | grep 19132
sudo ss -ltnp | grep -E '25565|25555'
dig +short paper.example.com A
dig +short SRV _minecraft._tcp.paper.example.com
```

The DNS hostname is sanitized. The recorded Paper SRV values were priority 0, weight 0, port 25555, and a 30-minute TTL. A correct DNS response does not prove NAT forwarding or application health.

## Java investigation

```bash
pgrep -af java
ps -eo pid,rss,%cpu,args --sort=-rss | head -8
jcmd -l
jcmd JAVA_PID GC.heap_info
top -H -p JAVA_PID
```

Replace `JAVA_PID` with the current process. Process arguments and logs can contain secrets; review output before sharing. RSS includes more than heap memory, and a single `%CPU` sample is not proof of a CPU bottleneck.

## Backup troubleshooting

```bash
sudo systemctl status atm10-backup.service --no-pager -l
sudo journalctl -u atm10-backup.service -n 50 --no-pager
ls -ld /opt/minecraft/backups
ls -ld /opt/minecraft/atm10/world
```

See the [Bedrock migration case study](bedrock-migration.md) for the recorded recovery archive command and its limits. Avoid publishing complete logs or service files before checking them for credentials and player details.
