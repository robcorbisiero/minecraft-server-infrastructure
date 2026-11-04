# Commands I use

A quick reference from setting up and troubleshooting the servers. Replace the example hostname and Java PID before using them.

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

I use these to check the machine, memory, Java version, and storage layout. `df` shows the filesystem size; `lsblk` helps explain where the rest of the disk went.

## Service state and logs

```bash
systemctl is-active minecraft-bedrock
systemctl is-enabled minecraft-bedrock
systemctl status atm10 --no-pager -l
sudo journalctl -u atm10 -n 50 --no-pager
systemctl status minecraft-paper --no-pager -l
sudo journalctl -u minecraft-paper -n 50 --no-pager
```

`active` tells me the service is running; `enabled` tells me it is configured to start at boot. The Paper name is from my migration plan, so check the installed unit if you adapt these.

## Listener and DNS checks

```bash
sudo ss -lunp | grep 19132
sudo ss -ltnp | grep -E '25565|25555'
dig +short paper.example.com A
dig +short SRV _minecraft._tcp.paper.example.com
```

I used priority 0, weight 0, port 25555, and a 30-minute TTL for Paper's SRV record. I check the local listener and router forwarding separately from DNS.

## Java investigation

```bash
pgrep -af java
ps -eo pid,rss,%cpu,args --sort=-rss | head -8
jcmd -l
jcmd JAVA_PID GC.heap_info
top -H -p JAVA_PID
```

Replace `JAVA_PID` with the current Minecraft PID. I check heap usage separately from RSS because the Java process uses memory outside the heap too. Check output for secrets before sharing it.

## Backup troubleshooting

```bash
sudo systemctl status atm10-backup.service --no-pager -l
sudo journalctl -u atm10-backup.service -n 50 --no-pager
ls -ld /opt/minecraft/backups
ls -ld /opt/minecraft/atm10/world
```

The archive command is in my [Bedrock migration notes](bedrock-migration.md). I keep raw logs and service files private until I've removed credentials and player details.
