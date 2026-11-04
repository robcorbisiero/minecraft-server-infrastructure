# Screenshot evidence

These captures come from the original setup conversation. They document historical configuration, not a new live test. Terminal images were captured from the original attachment viewer with surrounding interface excluded. No terminal output was regenerated or altered.

## Paper: installed Java runtime

![Original Paper host Java version output](paper-java-version.jpg)

**Command:** `java -version` on `mc-paper`.

**Shows:** OpenJDK 25.0.4.1 and the Ubuntu runtime build. This replaces the earlier documentation gap about the installed Paper Java version. It proves the runtime was installed at capture time; it does not alone prove the server started successfully.

## ATM10: measured heap use

![Original ATM10 jcmd heap statistics](atm10-heap-info.jpg)

**Command:** `jcmd 45471 GC.heap_info` on `mc-atm10`. The PID is historical.

**Shows:** reserved 10,485,760 KiB, committed 8,781,824 KiB, and used 3,721,633 KiB. These correspond to about 10 GiB reserved, 8.38 GiB committed, and 3.55 GiB used. This was an idle sample according to the surrounding conversation.

**Interpretation:** the heap was not full during that sample. It does not prove what happened during multiplayer exploration or establish a CPU bottleneck without under-load profiling.

## Paper: DNS service record

<img src="paper-dns-srv.png" alt="Original DNS SRV editor showing Minecraft TCP service, priority zero, weight zero, port 25555 and 30-minute TTL" width="360">

**Shows:** the DNS editor configured for `_minecraft._tcp.paper`, priority 0, weight 0, destination port 25555, and TTL 30 minutes. The original image includes the public server hostname; credentials and public IP addresses are not present.

**Limit:** this is an editor screenshot. It does not by itself prove the record was saved or resolvable. The later player confirmation is separate evidence of the migration outcome.

## What is deliberately not claimed

No synthetic terminal screenshots, invented dashboards, current uptime claims, or restored-world screenshots are included. Bedrock's archive check is preserved as an attributed text excerpt in its migration case study; it is not represented by an unrelated screenshot.
